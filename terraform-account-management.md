# Terraform Interview Notes

This document covers common Terraform interview topics using practical examples: validations, loops, conditions, workspaces, and day-to-day commands.

## 1. Terraform Variable Validation

Variable validation is used to reject invalid input before Terraform creates or changes infrastructure.

### Basic Validation

```hcl
variable "environment" {
  type = string

  validation {
    condition     = contains(["dev", "uat", "prod"], lower(var.environment))
    error_message = "environment must be dev, uat, or prod."
  }
}
```

### Regex Validation

Use regex when input must follow a pattern.

```hcl
variable "account_name" {
  type = string

  validation {
    condition     = can(regex("^[a-z0-9][a-z0-9-]{2,62}$", var.account_name))
    error_message = "account_name must be 3-63 characters and use lowercase letters, numbers, and hyphens."
  }
}
```

### Email Validation

```hcl
variable "owner_email" {
  type = string

  validation {
    condition     = can(regex("^[^@\\s]+@[^@\\s]+\\.[^@\\s]+$", var.owner_email))
    error_message = "owner_email must be a valid email address."
  }
}
```

### Length Validation

```hcl
variable "cost_center" {
  type = string

  validation {
    condition     = length(trimspace(var.cost_center)) >= 3
    error_message = "cost_center must not be empty."
  }
}
```

### Interview Point

Terraform validation should reject bad input. It should not silently modify important values like account names, emails, environments, or approval states.

## 2. Locals

`locals` define reusable values inside a Terraform module.

```hcl
locals {
  common_tags = {
    Environment = var.environment
    CostCenter  = var.cost_center
    Owner       = var.owner
    ManagedBy   = "terraform"
  }
}
```

Use the local value like this:

```hcl
tags = local.common_tags
```

### Interview Point

Use locals to avoid repeating expressions and to keep resource blocks clean.

## 3. Conditions

Terraform supports conditional expressions using this format:

```hcl
condition ? true_value : false_value
```

### Example

```hcl
locals {
  backup_enabled = var.environment == "prod" ? true : false
}
```

### Conditional Resource Creation

Use `count` when you want to create zero or one resource.

```hcl
resource "aws_s3_bucket_versioning" "this" {
  count  = var.environment == "prod" ? 1 : 0
  bucket = aws_s3_bucket.this.id

  versioning_configuration {
    status = "Enabled"
  }
}
```

### Conditional For Each

```hcl
resource "aws_ce_cost_allocation_tag" "required" {
  for_each = var.activate_cost_allocation_tags ? local.cost_tags : toset([])

  tag_key = each.value
  status  = "Active"
}
```

### Interview Point

Use conditions to make behavior environment-aware, such as stricter settings for `prod` and simpler settings for `dev`.

## 4. Loops

Terraform does not have traditional `for` loops like Python or Java. It uses expressions such as `for_each`, `count`, and `for` expressions.

## 5. Count

`count` creates multiple copies of a resource using an integer.

```hcl
resource "aws_iam_user" "developer" {
  count = 3
  name  = "developer-${count.index + 1}"
}
```

This creates:

```text
developer-1
developer-2
developer-3
```

### Count With Condition

```hcl
resource "aws_cloudwatch_log_group" "debug" {
  count = var.environment == "dev" ? 1 : 0
  name  = "/app/debug"
}
```

### Interview Point

Use `count` for simple numeric repetition. Avoid `count` when resources need stable names based on keys.

## 6. For Each

`for_each` creates resources from a map or set.

### Set Example

```hcl
variable "readonly_groups" {
  type = set(string)
}

resource "aws_ssoadmin_account_assignment" "readonly" {
  for_each = var.readonly_groups

  principal_type = "GROUP"
  principal_id   = each.value
}
```

### Map Example

```hcl
variable "permission_sets" {
  type = map(object({
    policy_arn = string
  }))
}

resource "aws_ssoadmin_permission_set" "this" {
  for_each = var.permission_sets

  name = each.key
}
```

### Interview Point

Use `for_each` when each resource has a meaningful key, like group name, bucket name, account name, or policy name.

## 7. For Expressions

For expressions transform one collection into another.

### List Transformation

```hcl
locals {
  uppercase_envs = [for env in var.environments : upper(env)]
}
```

### Map Transformation

```hcl
locals {
  tag_map = {
    for key, value in var.tags : key => trimspace(value)
  }
}
```

### Filtered For Expression

```hcl
locals {
  prod_accounts = [
    for account in var.accounts : account
    if account.environment == "prod"
  ]
}
```

### Interview Point

For expressions are useful for shaping ServiceNow or pipeline input into the structure expected by Terraform resources.

## 8. Dynamic Blocks

Dynamic blocks generate nested blocks inside a resource.

```hcl
resource "aws_security_group" "web" {
  name = "web-sg"

  dynamic "ingress" {
    for_each = var.ingress_rules

    content {
      from_port   = ingress.value.from_port
      to_port     = ingress.value.to_port
      protocol    = ingress.value.protocol
      cidr_blocks = ingress.value.cidr_blocks
    }
  }
}
```

### Interview Point

Use dynamic blocks when nested configuration is repeated. Do not use them if normal static blocks are clearer.

## 9. Workspaces

Terraform workspaces allow multiple state files for the same configuration.

Common workspace use:

```text
dev
uat
prod
```

### Workspace Commands

```bash
terraform workspace list
terraform workspace new dev
terraform workspace new uat
terraform workspace new prod
terraform workspace select dev
terraform workspace show
```

### Using Workspace In Code

```hcl
locals {
  environment = terraform.workspace
}
```

Example:

```hcl
tags = {
  Environment = terraform.workspace
}
```

### Interview Point

Workspaces separate state, not code. For large production platforms, separate backend paths or separate root modules are often cleaner than relying only on workspaces.

## 10. Common Terraform Commands

### Initialize

```bash
terraform init
```

Downloads providers and initializes backend configuration.

### Format

```bash
terraform fmt -recursive
```

Formats Terraform files.

### Validate

```bash
terraform validate
```

Checks syntax and provider schema validity.

### Plan

```bash
terraform plan -var-file="../examples/dev-account.tfvars"
```

Shows what Terraform will create, update, or destroy.

### Apply

```bash
terraform apply -var-file="../examples/dev-account.tfvars"
```

Applies the planned changes.

### Destroy

```bash
terraform destroy -var-file="../examples/dev-account.tfvars"
```

Destroys infrastructure. Be very careful with this command.

### Show State

```bash
terraform state list
terraform state show aws_organizations_account.this
```

### Output

```bash
terraform output
terraform output -json
```

### Refresh Only

```bash
terraform plan -refresh-only
terraform apply -refresh-only
```

Updates Terraform state from real infrastructure without changing resources.

## 11. State

Terraform state maps code to real infrastructure.

Example:

```text
aws_organizations_account.this -> AWS account ID 123456789012
```

### Interview Point

State should be stored remotely for team usage, usually in S3 with DynamoDB locking for AWS.

Example backend:

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "aws-account-blueprint/dev/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

## 12. Variables And Tfvars

Variables are declared in `variables.tf`.

```hcl
variable "environment" {
  type = string
}
```

Values are passed from `.tfvars`.

```hcl
environment = "dev"
```

Command:

```bash
terraform plan -var-file="../examples/dev-account.tfvars"
```

### Interview Point

Keep reusable logic in `.tf` files and environment-specific values in `.tfvars` files.

## 13. ServiceNow Input Validation Strategy

Recommended validation layers:

```text
1. ServiceNow catalog form validation
2. Pipeline payload validation
3. Terraform variable validation
```

### Example ServiceNow Fields

```text
account_name
account_email
environment
cost_center
business_unit
owner
admin_groups
developer_groups
readonly_groups
approval_state
```

### Terraform Approval Validation

```hcl
variable "approval_state" {
  type = string

  validation {
    condition     = lower(var.approval_state) == "approved"
    error_message = "Request must be approved before Terraform can run."
  }
}
```

### Interview Point

Do not depend only on Terraform for approval checks. ServiceNow and the pipeline should also enforce approval before Terraform runs.

## 14. Terraform Resource Naming

Terraform resource syntax:

```hcl
resource "resource_type" "local_name" {
}
```

Example:

```hcl
resource "aws_organizations_account" "this" {
  name  = var.account_name
  email = var.account_email
}
```

Here:

```text
aws_organizations_account = resource type
this = Terraform local resource name
```

Reference:

```hcl
aws_organizations_account.this.id
```

### Interview Point

`this` is commonly used when a module manages one main resource. More descriptive names like `new_account` are also valid.

## 15. Account Creation Interview Example

```hcl
locals {
  tags = {
    Environment = var.environment
    CostCenter  = var.cost_center
    Owner       = var.owner
    ManagedBy   = "terraform"
  }
}

resource "aws_organizations_account" "new_account" {
  name  = var.account_name
  email = var.account_email

  role_name                  = "OrganizationAccountAccessRole"
  iam_user_access_to_billing = "DENY"

  tags = local.tags

  lifecycle {
    prevent_destroy = true
  }
}
```

### Why Prevent Destroy?

```hcl
lifecycle {
  prevent_destroy = true
}
```

This prevents accidental deletion from Terraform. For AWS accounts, deletion/closure should be a controlled process.

## 16. Common Interview Questions

### What is the difference between `count` and `for_each`?

`count` uses numbers and indexes. `for_each` uses keys from a map or values from a set. `for_each` is better when each resource has a stable identity.

### What is a Terraform provider?

A provider is a plugin that allows Terraform to manage a platform, such as AWS, Azure, GCP, Kubernetes, or ServiceNow.

### What is Terraform state?

State is Terraform's record of the infrastructure it manages.

### What is a module?

A module is a reusable Terraform package. Every Terraform folder is a module. A root module can call child modules.

### What is `terraform plan`?

It previews changes before applying them.

### What is `terraform apply`?

It creates, updates, or deletes infrastructure based on the plan.

### What is `terraform import`?

It brings existing infrastructure under Terraform state management.

Example:

```bash
terraform import aws_s3_bucket.example my-existing-bucket
```

### What is a backend?

A backend controls where Terraform stores state. For AWS teams, a common backend is S3 with DynamoDB locking.

## 17. Best Practices

- Use remote state for team projects.
- Use `terraform fmt` before committing.
- Use `terraform validate` in CI/CD.
- Use `terraform plan` for approval review.
- Keep secrets out of `.tfvars`.
- Use IAM roles instead of long-term IAM user access keys.
- Use `for_each` for stable multi-resource creation.
- Use variable validation for ServiceNow or pipeline input.
- Use `prevent_destroy` for critical resources.
- Use separate approval gates for production.

## 18. Suggested Learning Order

```text
1. Providers
2. Variables
3. Locals
4. Resources
5. Outputs
6. Tfvars
7. Validation
8. Conditions
9. Count
10. For_each
11. For expressions
12. State
13. Backend
14. Workspaces
15. Modules
16. CI/CD pipeline integration
```


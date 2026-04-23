# 🧪 AWS IAM Assume Role Debugging Lab (Hands-On)

## 🎯 Objective

Understand and debug AWS IAM Assume Role behavior using:

* Trust policies
* User permissions
* AWS CLI
* Debug logs

---

# 🧱 Lab Setup

## 👤 IAM User

* Name: `manohar-user`
* Policy: `IAMReadOnlyAccess`

## 🔐 IAM Role

* Name: `app-s3-role`
* Permission: `AmazonS3ReadOnlyAccess`
* Trust Policy: Allows `manohar-user`

---

# ⚙️ Step 1 — Configure AWS CLI

```bash
aws configure --profile manohar
```

---

# 🔎 Step 2 — Verify Identity

```bash
aws sts get-caller-identity --profile manohar
```

### ✅ Expected Output

```
arn:aws:iam::<ACCOUNT-ID>:user/manohar-user
```

---

# 🔎 Step 2.1 — Confirm IAM User Details (NEW)

```bash
aws iam get-user --profile manohar
```

### ✅ Why this helps:

* Confirms exact IAM user
* Validates account ID
* Ensures correct credentials are used

---

# ❌ Step 3 — Test Without Role

```bash
aws s3 ls --profile manohar
```

### Expected:

```
AccessDenied
```

---

# 🔐 Step 4 — Assume Role

```bash
aws sts assume-role \
  --role-arn arn:aws:iam::<ACCOUNT-ID>:role/app-s3-role \
  --role-session-name test \
  --profile manohar
```

---

# 🔁 Step 5 — Export Temporary Credentials

```bash
export AWS_ACCESS_KEY_ID=XXX
export AWS_SECRET_ACCESS_KEY=XXX
export AWS_SESSION_TOKEN=XXX
```

---

# 🔎 Step 6 — Verify Role Identity

```bash
aws sts get-caller-identity
```

### ✅ Expected:

```
arn:aws:sts::<ACCOUNT-ID>:assumed-role/app-s3-role/...
```

---

# 🧪 Step 7 — Test S3 Access

```bash
aws s3 ls
```

---

# 🧠 Debugging Scenarios

---

## 🔍 Check 1 — Who am I?

```bash
aws sts get-caller-identity
```

👉 Detect:

* IAM User
* Assumed Role

---

## 🔍 Check 2 — Confirm IAM User (NEW)

```bash
aws iam get-user --profile manohar
```

---

## 🔍 Check 3 — Reset Environment Variables

```bash
unset AWS_ACCESS_KEY_ID
unset AWS_SECRET_ACCESS_KEY
unset AWS_SESSION_TOKEN
```

---

## 🔍 Check 4 — List User Policies

```bash
aws iam list-attached-user-policies --user-name manohar-user
```

---

## 🔍 Check 5 — Check Group Membership

```bash
aws iam list-groups-for-user --user-name manohar-user
```

---

## 🔍 Check 6 — Verify Credentials File

```bash
cat ~/.aws/credentials
```

---

## 🔍 Check 7 — Verify Profile Config

```bash
aws configure list --profile manohar
```

---

## 🔍 Check 8 — Debug Assume Role Call

```bash
aws sts assume-role \
  --role-arn arn:aws:iam::<ACCOUNT-ID>:role/app-s3-role \
  --role-session-name test \
  --profile manohar \
  --debug
```

---

# 🔥 Key Debug Insight

From debug logs:

```
Found credentials in shared credentials file: ~/.aws/credentials
```

👉 Means CLI is using local stored credentials

---

# ⚠️ Common Issues

| Issue                             | Cause                             | Fix                               |
| --------------------------------- | --------------------------------- | --------------------------------- |
| Assume role works unexpectedly    | Using old role credentials        | `unset env variables`             |
| AccessDenied                      | Missing `sts:AssumeRole`          | Add user policy                   |
| Still works after removing policy | Credentials have higher privilege | Check profile                     |
| Wrong identity                    | Using wrong profile               | Verify with `get-caller-identity` |

---

# 🧠 IAM Evaluation Logic

```
User Policy
+ Role Trust Policy
+ Role Permission Policy
--------------------------------
= FINAL ACCESS
```

---

# 🔥 Golden Rules

* Access key = Authentication
* IAM policy = Authorization
* Trust policy = WHO can assume
* Role policy = WHAT can be done

---

# 🎯 Final Summary

> Assume role requires BOTH:
>
> * User permission (`sts:AssumeRole`)
> * Role trust policy

---

# 🚀 Advanced Practice

* Remove trust policy → test
* Remove user permission → test
* Use wrong ARN → observe error
* Use `--debug` → analyze request

---

# 🧠 One-Line Memory

> "Access keys log you in, AssumeRole changes who you are"

---
eval $(aws sts assume-role \
  --role-arn arn:aws:iam::<ACCOUNT-ID>:role/app-s3-role \
  --role-session-name test \
  --profile manohar \
  --query 'Credentials.[AccessKeyId,SecretAccessKey,SessionToken]' \
  --output text | awk '{print "export AWS_ACCESS_KEY_ID="$1"\nexport AWS_SECRET_ACCESS_KEY="$2"\nexport AWS_SESSION_TOKEN="$3}')

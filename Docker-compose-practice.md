# 🚀 Docker Compose DevOps Lab

## 🎯 Objective
Deploy and manage a multi-container application on AWS EC2 using Docker Compose and gain hands-on DevOps experience.

---

## 🧱 Architecture

User → Nginx → Flask App → Redis → PostgreSQL

- Nginx: Reverse proxy
- Flask: Backend app
- Redis: Cache
- PostgreSQL: Database

---

## ⚙️ Setup Steps

1. Launch EC2 (t3.small)
2. Install Docker & Docker Compose
3. Create project structure
4. Write:
   - Dockerfile
   - docker-compose.yml
   - Flask app
5. Run:
```bash
docker compose up -d


🚨 Issues Faced & Fixes
❌ Buildx Error

Error: compose build requires buildx
Fix:

DOCKER_BUILDKIT=0 docker compose up -d --build
❌ No containers created

Cause: Wrong directory
Fix:

pwd
ls
cd docker-lab
❌ Volume mount error

Error: not a directory
Fix: Ensure file exists (default.conf)

❌ High image size

Before: ~1.7GB
Fix: Use slim + alpine images
After: ~570MB

❌ Inefficient builds

Fix: Multi-stage Docker builds

📊 Optimization Results
Component	Before	After
Flask App	1.1GB	129MB
Total Size	1.7GB	~570MB
🔐 Secrets Management
Methods:
.env file (local)
Environment variables
Docker secrets
AWS Secrets Manager (recommended)
🧠 Key Learnings
Docker images vs containers
Docker Compose orchestration
Container networking (service name)
Debugging with logs
Image optimization
Multi-stage builds
🔍 Debugging Commands
docker ps -a
docker logs <container>
docker inspect <container>
docker stats
🧪 Experiments Done
Stopped Redis → app failure
Stopped Nginx → no access
Changed ports → routing test
Observed logs
🎯 DevOps Takeaways
Always check logs first
Validate working directory
Optimize images for performance
Use secure secret management
Understand system flow
🗣️ Interview Summary

"I deployed a multi-container application using Docker Compose on AWS EC2 with Nginx, Flask, Redis, and PostgreSQL. I troubleshooted multiple issues and optimized Docker images by ~67% using slim and multi-stage builds."


---

# 📄 `docker-devops-interview-qa.md`

```markdown
# 🎯 Docker DevOps Interview Q&A

---

## 🐳 Basics

### Q1: What is Docker?
A platform to build, package, and run applications in containers.

---

### Q2: What is Docker Compose?
A tool to define and manage multi-container applications using YAML.

---

### Q3: Difference between Docker and Docker Compose?
- Docker: Runs single container
- Compose: Runs multiple containers

---

## 🔗 Networking

### Q4: How do containers communicate?
Using Docker network via service names.

Example:
```python
redis.Redis(host="redis")
Q5: What is bridge network?

Default Docker network that allows container communication.

🔍 Debugging
Q6: How do you debug container issues?
docker logs <container>
docker ps -a
docker inspect <container>
Q7: What happens if container exits?

Check logs and exit code.

⚡ Optimization
Q8: How did you reduce image size?
Used slim images
Used alpine images
Used multi-stage builds
Q9: What is multi-stage build?

Separates build and runtime to create smaller, secure images.

🌐 Architecture
Q10: Why Nginx?

Acts as reverse proxy and load balancer.

Q11: Why Redis?

Used for caching and fast data access.

Q12: Why PostgreSQL?

Persistent data storage.

🔐 Security
Q13: How do you manage secrets?
.env (local)
AWS Secrets Manager (production)
Q14: Why not store secrets in code?

Security risk and exposure.

☁️ Real-world
Q15: What challenges did you face?
Buildx issue
Volume mount issue
Wrong directory
Large image size
Q16: How did you solve them?

Step-by-step debugging using logs and validation.

🚀 Advanced
Q17: What is Dockerfile?

Script to build Docker image.

Q18: What is image vs container?
Image: blueprint
Container: running instance
Q19: What is volume?

Persistent storage for containers.

Q20: What is CI/CD?

Automated pipeline to build, test, and deploy applications.


---

# 🚀 How to use this

1. Save files:
```bash
nano docker-devops-lab-notes.md
nano docker-devops-interview-qa.md
Paste content
Push to GitHub → 🎯 portfolio ready
💡 Pro Tip

Put this in GitHub repo:

docker-devops-lab/
 ├── docker-compose.yml
 ├── app/
 ├── nginx/
 ├── docker-devops-lab-notes.md
 ├── docker-devops-interview-qa.md

👉 This becomes a strong DevOps project showcase

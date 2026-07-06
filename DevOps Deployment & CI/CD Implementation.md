# Multi-Auth API with CI/CD Pipeline

## Project Overview

This project is a production-style deployment of a Node.js Multi-Authentication API using AWS cloud services and a complete Jenkins CI/CD pipeline.

The application is deployed on an AWS EC2 instance, uses Amazon RDS PostgreSQL as the database, is managed using PM2, served through Nginx, and automatically deployed using Jenkins whenever code is pushed to GitHub.

---

# Architecture

```
                    GitHub Repository
                           │
                     Git Push (Webhook)
                           │
                           ▼
                Jenkins (Port 9090)
                           │
                  Automatic Pipeline
                           │
        ┌──────────────────┴──────────────────┐
        │                                     │
 Checkout Code                     Install Dependencies
        │                                     │
        ▼                                     ▼
 npm install                      Prisma Client Generation
        │                                     │
        └──────────────────┬──────────────────┘
                           ▼
                     PM2 Restart
                           │
                           ▼
                     Health Check
                           │
                    Success / Rollback
                           │
                           ▼
                  Nginx Reverse Proxy
                           │
                           ▼
                 Multi-Auth Application
                           │
                           ▼
                  Amazon RDS PostgreSQL
```

---

# Technologies Used

## Cloud

- AWS EC2 (Ubuntu 24.04)
- Amazon RDS PostgreSQL
- IAM
- Security Groups

## Backend

- Node.js
- Express.js
- Prisma ORM
- PostgreSQL

## DevOps

- Jenkins
- GitHub
- GitHub Webhooks
- PM2
- Nginx
- SSH Agent
- Linux

---

# Infrastructure

## EC2

- Ubuntu 24.04 LTS
- Jenkins
- PM2
- Node.js
- Git
- Nginx

Public IP

```
16.113.35.228
```

---

## Database

Amazon RDS PostgreSQL

- Database Engine: PostgreSQL
- Separate database for Multi-Auth
- Connected through Prisma ORM

---

# CI/CD Pipeline

The Jenkins pipeline performs the following steps automatically after every GitHub push.

## Stage 1

Checkout source code

```groovy
checkout scm
```

---

## Stage 2

Install dependencies

```
npm install
```

---

## Stage 3

Generate Prisma Client

```
npx prisma generate
```

---

## Stage 4

Restart PM2

```
pm2 restart multi-auth
```

---

## Stage 5

Health Check

```
curl http://localhost/auth
```

Deployment succeeds only if the health check passes.

---

# Rollback Strategy

If deployment fails,

the Jenkins pipeline automatically

- Restores previous Git commit
- Installs dependencies
- Generates Prisma Client
- Restarts PM2

This ensures the application always returns to the previous working version.

---

# Jenkins Configuration

Jenkins URL

```
http://16.113.35.228:9090
```

Pipeline Type

Pipeline Script from SCM

Source Control

GitHub

Automatic Trigger

GitHub Webhook

---

# GitHub Webhook

Whenever code is pushed to GitHub,

GitHub automatically sends a webhook request to Jenkins.

Jenkins then

- Pulls latest code
- Builds application
- Deploys application
- Performs Health Check

No manual deployment is required.

---

# PM2

Application is managed using PM2.

Useful Commands

Start

```
pm2 start server.js --name multi-auth
```

Restart

```
pm2 restart multi-auth
```

Status

```
pm2 list
```

Logs

```
pm2 logs multi-auth
```

---

# Nginx

Nginx is configured as a reverse proxy.

Responsibilities

- Reverse Proxy
- HTTP Request Routing
- Production Traffic Handling

---

# Security

## Jenkins

- Admin User
- Viewer User

Viewer account can only inspect pipelines.

---

## AWS IAM

Created reviewer IAM user with Read-Only permissions.

Permissions include

- EC2 Read
- RDS Read
- CloudWatch Read

---

# Project Structure

```
Multi-Auth
│
├── config/
├── controllers/
├── middlewares/
├── prisma/
├── repositories/
├── routes/
├── services/
├── utils/
├── validations/
├── server.js
├── package.json
├── Jenkinsfile
└── README.md
```

---

# Deployment Process

Developer

↓

Git Push

↓

GitHub

↓

Webhook

↓

Jenkins Pipeline

↓

Checkout Code

↓

Install Packages

↓

Generate Prisma Client

↓

Restart PM2

↓

Health Check

↓

Application Live

---

# Health Check

Endpoint

```
http://localhost/auth
```

Expected Response

```json
{
    "status": true,
    "message": "System Works",
    "data": null
}
```

---

# Screenshots
<img width="1917" height="801" alt="image" src="https://github.com/user-attachments/assets/7e2f13fa-6700-49b9-ab1f-c75fc0c12a6e" />

<img width="1890" height="796" alt="image" src="https://github.com/user-attachments/assets/167652e8-9817-4ba7-bf8d-fe72279cda69" />

<img width="1882" height="617" alt="image" src="https://github.com/user-attachments/assets/76b14b17-9442-4142-83a6-12ec9cf2dabe" />
<img width="1852" height="767" alt="image" src="https://github.com/user-attachments/assets/1874d179-e569-4eac-b85a-d15ee6b7b2c6" />

<img width="1337" height="821" alt="image" src="https://github.com/user-attachments/assets/83f90dcd-e6fc-4342-8c4b-9da0971da440" />

<img width="1116" height="296" alt="image" src="https://github.com/user-attachments/assets/0e6171bb-0b1d-466e-870e-dd3048c6f86e" />

<img width="1682" height="875" alt="image" src="https://github.com/user-attachments/assets/570d427d-0488-481e-b8f2-2b81434efb90" />

<img width="1805" height="627" alt="image" src="https://github.com/user-attachments/assets/2e58d5c8-88b7-4d85-bbb5-0f0fd6fb5f7d" />

<img width="1917" height="787" alt="image" src="https://github.com/user-attachments/assets/3e44e9f2-a403-4b0f-b379-f54e84fe63bc" />

<img width="977" height="677" alt="image" src="https://github.com/user-attachments/assets/3b2f0edc-cc91-465e-a54a-ac89d69341c0" />

<img width="1840" height="672" alt="image" src="https://github.com/user-attachments/assets/ecc66099-ffc9-4088-838a-735bbc306da8" />

---

# Future Improvements

- Dockerize application
- Kubernetes deployment
- Terraform Infrastructure
- SonarQube Integration
- Automated Testing
- Blue-Green Deployment
- Monitoring using Prometheus and Grafana

---

# Author

**Jayanthi Dorepalli**

AWS Cloud & DevOps Engineer

GitHub

https://github.com/jayanthidorepalli

LinkedIn

https://linkedin.com/in/jayanthi-dorepalli-475400226

---

# Project Status

✅ Successfully Completed

- Infrastructure Provisioned
- Database Connected
- CI/CD Configured
- GitHub Webhook Configured
- Automatic Deployment Enabled
- Rollback Implemented
- Health Check Configured
- Jenkins Viewer Created
- AWS IAM Reviewer Created

```

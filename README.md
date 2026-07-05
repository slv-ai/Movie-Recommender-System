# Movie Recommender System: Complete MLOps Pipeline

> **Production-Grade ML System with Automated Training, Evaluation, and Kubernetes Serving**

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Technologies](#technologies)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Detailed Setup](#detailed-setup)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [Monitoring](#monitoring)
- [Deployment](#deployment)
- [Next Steps](#next-steps)
---

## 📖 Overview

This is a **complete, production-ready MLOps system** that demonstrates:

- ✅ **Automated model training** with SageMaker
- ✅ **Orchestrated workflows** with Step Functions
- ✅ **CI/CD automation** with GitHub Actions
- ✅ **Model serving at scale** with Kubernetes (EKS)
- ✅ **Auto-scaling** based on traffic
- ✅ **Zero-downtime deployments** with rolling updates
- ✅ **Comprehensive monitoring** with CloudWatch


### What This System Does

1. **Trains** a collaborative filtering model (SVD) on MovieLens data weekly
2. **Evaluates** model quality automatically
3. **Packages** the model in a Docker container
4. **Deploys** to Kubernetes for serving
5. **Scales** pods automatically based on demand
6. **Updates** to new models without downtime
7. **Monitors** all metrics and alerts on issues

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    GitHub (Your Code)                       │
└────────────────────┬────────────────────────────────────────┘
                     │ Push to main
                     ↓
┌─────────────────────────────────────────────────────────────┐
│                   GitHub Actions                            │
│  • Prepare environment                                      │
│  • Build Docker image                                       │
│  • Trigger Step Functions                                   │
│  • Monitor execution                                        │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ↓
┌─────────────────────────────────────────────────────────────┐
│              AWS Step Functions Workflow                    │
│  1. Validate training data (Lambda)                        │
│  2. Train model (SageMaker Training Job)                   │
│  3. Evaluate metrics (Lambda)                              │
│  4. Decision: Deploy or Reject                             │
│  5. Send notifications (SNS)                               │
└────────────────────┬────────────────────────────────────────┘
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
    ┌────────┐   ┌────────┐   ┌─────────┐
    │   S3   │   │ SageMaker│   │CloudWatch
    │ Models │   │ Training │   │Metrics
    └────────┘   └────────┘   └─────────┘
        │
        ↓
    ┌──────────────────────┐
    │   Docker Image       │
    │  (Model + API)       │
    └──────┬───────────────┘
           │
           ↓
    ┌──────────────────────┐
    │   ECR Registry       │
    │  (Docker storage)    │
    └──────┬───────────────┘
           │
           ↓
    ┌──────────────────────────────────┐
    │      EKS Kubernetes Cluster      │
    │  ┌────────────────────────────┐  │
    │  │   Load Balancer (ALB)      │  │
    │  │   Public IP endpoint       │  │
    │  └────────────┬───────────────┘  │
    │               │                   │
    │  ┌────────────▼───────────────┐  │
    │  │  Kubernetes Service        │  │
    │  │  Load-balancing across:    │  │
    │  └────────────┬───────────────┘  │
    │         ┌─────┼─────┐             │
    │         │     │     │             │
    │  ┌──────▼──┐┌──────▼──┐┌──────▼──┐│
    │  │  Pod 1  ││  Pod 2  ││  Pod 3  ││
    │  │(Model)  ││(Model)  ││(Model)  ││
    │  └─────────┘└─────────┘└─────────┘│
    │                                    │
    │  ┌────────────────────────────┐  │
    │  │ HPA Auto-Scaler            │  │
    │  │ Min: 2, Max: 10 pods       │  │
    │  │ Target: 70% CPU            │  │
    │  └────────────────────────────┘  │
    └──────────────────────────────────┘
           │
           ↓
    ┌──────────────────────┐
    │   Clients            │
    │ GET /recommend       │
    │ ?user_id=1&n=10      │
    └──────────────────────┘
```

---

## 🛠️ Technologies

|
 Component 
|
 Technology 
|
 Why 
|
|
-----------
|
-----------
|
-----
|
|
**
Data
**
|
 S3, MovieLens 
|
 Store training data 
|
|
**
Training
**
|
 SageMaker, scikit-learn 
|
 Scalable model training 
|
|
**
Model
**
|
 SVD (Singular Value Decomposition) 
|
 Fast, scalable, proven 
|
|
**
Orchestration
**
|
 Step Functions 
|
 Coordinate workflows 
|
|
**
Validation
**
|
 Lambda, Python 
|
 Data quality checks 
|
|
**
Evaluation
**
|
 Lambda, Python 
|
 Model quality metrics 
|
|
**
CI/CD
**
|
 GitHub Actions 
|
 Automated on every push 
|
|
**
Notifications
**
|
 SNS 
|
 Email alerts 
|
|
**
Containerization
**
|
 Docker 
|
 Package model + code 
|
|
**
Registry
**
|
 ECR 
|
 Store Docker images 
|
|
**
Kubernetes
**
|
 EKS 
|
 Orchestrate containers 
|
|
**
Serving
**
|
 Flask, Gunicorn 
|
 REST API 
|
|
**
Auto-Scaling
**
|
 HPA 
|
 Scale based on demand 
|
|
**
Monitoring
**
|
 CloudWatch 
|
 Track metrics 
|
|
**
Infrastructure
**
|
 Terraform 
|
 Infrastructure as Code 
|
|
**
Version Control
**
|
 Git, GitHub 
|
 Track all changes 
|

---

- ✅ **Infrastructure as Code** with Terraform

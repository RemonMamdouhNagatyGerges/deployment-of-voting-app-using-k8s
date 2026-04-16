# Kubernetes Voting App

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Udemy](https://img.shields.io/badge/Udemy-A435F0?style=for-the-badge&logo=udemy&logoColor=white)](https://www.udemy.com/course/learn-kubernetes/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A sample voting application deployed on Kubernetes, built while learning from the Udemy "Absolute Beginners" Kubernetes course. This repository includes both raw Pod manifests and Deployment + Service manifests so you can compare beginner-level and more realistic Kubernetes deployment patterns. This project demonstrates key DevOps skills including container orchestration, microservices architecture, and infrastructure as code.

## Project overview

This project deploys a simple voting application using these components:

- `voting-app`: frontend voting UI
- `result-app`: results dashboard
- `worker`: background vote processor
- `redis`: cache / queue system
- `postgres`: persistent storage

## Architecture

![Architecture Diagram](architecture.png)

1. Users vote through the `voting-app`
2. Votes are processed by the `worker`
3. Redis handles temporary state / queueing
4. PostgreSQL stores persistent vote data
5. `result-app` displays aggregated results

## Repository structure

This repository is organized to showcase different Kubernetes deployment strategies, highlighting my understanding of Kubernetes resources and DevOps best practices.

```text
.
├── README.md                           # Project documentation and setup guide
├── architecture.png                    # Visual representation of the application architecture
├── k8s-pods/                           # Raw Pod manifests (beginner level)
│   ├── voting-app-pod.yaml             # Defines the voting app Pod with container spec
│   ├── result-app-pod.yaml             # Defines the result app Pod
│   ├── worker.yaml                     # Background worker Pod for vote processing
│   ├── redis-pod.yaml                  # Redis cache Pod with basic configuration
│   └── postgres-pod.yaml               # PostgreSQL database Pod with environment variables
└── deployments+services/               # Production-ready Deployment and Service manifests
    ├── voting-app-deployment.yaml      # Deployment for voting app with replicas and selectors
    ├── result-app-deployment.yaml      # Deployment for result app
    ├── worker-deployment.yaml          # Deployment for worker service
    ├── worker-deployment copy.yaml     # Backup copy of worker deployment
    ├── redis-deployment.yaml           # Deployment for Redis with scaling capabilities
    ├── postgres-deployment.yaml        # Deployment for PostgreSQL with environment config
    ├── voting-app-service.yaml         # NodePort service for external voting app access
    ├── result-service.yaml             # NodePort service for result app
    ├── redis-service.yaml              # Internal service for Redis
    └── postgres-service.yaml           # Internal service for PostgreSQL
```

### File explanations

#### k8s-pods/ (Raw Pod Manifests)

These files demonstrate basic Kubernetes Pod creation, showing understanding of container specifications, ports, and environment variables. This approach is suitable for learning but not recommended for production due to lack of self-healing and scaling.

- **voting-app-pod.yaml**: Defines a Pod running the voting app container from Docker Hub, exposing port 80. Demonstrates basic Pod structure and container image usage.
- **result-app-pod.yaml**: Similar to above, for the result dashboard. Highlights container port configuration.
- **worker.yaml**: Pod for the worker service that processes votes. Shows how to run background tasks in Kubernetes.
- **redis-pod.yaml**: Redis Pod with port 6379. Illustrates deploying stateful services like caches.
- **postgres-pod.yaml**: PostgreSQL Pod with environment variables for user and password. Demonstrates secure credential handling in manifests.

#### deployments+services/ (Production-Ready Manifests)

These files use Deployments for scalability and reliability, along with Services for networking. This showcases advanced Kubernetes concepts like replicas, rolling updates, and selectors.

- **voting-app-deployment.yaml**: Deployment with 1 replica, matching labels, and container spec. Demonstrates high availability and updates.
- **result-app-deployment.yaml**: Deployment for result app with proper labeling.
- **worker-deployment.yaml**: Deployment for the worker, ensuring background processing reliability.
- **worker-deployment copy.yaml**: A duplicate for backup or versioning purposes.
- **redis-deployment.yaml**: Deployment for Redis, allowing scaling and self-healing.
- **postgres-deployment.yaml**: Deployment with environment variables, showing configuration management.
- **voting-app-service.yaml**: NodePort service for external access, with target port mapping.
- **result-service.yaml**: Similar for results.
- **redis-service.yaml**: Internal service for Redis communication.
- **postgres-service.yaml**: Internal service for database connectivity.

These manifests highlight skills in:

- **Infrastructure as Code**: Writing declarative YAML for reproducible deployments.
- **Microservices**: Separating concerns into individual services.
- **Container Orchestration**: Managing containers at scale with Kubernetes.
- **Networking**: Configuring services for internal and external communication.
- **State Management**: Handling databases and caches in Kubernetes.
- **DevOps Practices**: Version control, documentation, and CI/CD readiness.

## How to deploy

Apply the deployment manifests:

```bash
kubectl apply -f deployments+services/
```

Verify resources:

```bash
kubectl get pods
kubectl get svc
```

## Access

- Voting app: `NodePort 32000`
- Results app: `NodePort 32001`

> Note: Ports may vary depending on Kubernetes environment and NodePort availability.

## What I learned

- Kubernetes core resources: `Pod`, `Deployment`, `Service`
- Differences between raw Pod manifests and Deployment manifests
- Exposing applications using `NodePort`
- Connecting microservices in Kubernetes
- Deploying a multi-component application using Redis and PostgreSQL

## Future improvements

- Add `PersistentVolumeClaim` for PostgreSQL persistence
- Use `ConfigMap` and `Secret` for configuration and credentials
- Add liveness and readiness probes
- Replace `NodePort` with an `Ingress` controller
- Add Helm or Kustomize support

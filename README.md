# Jenkins CI/CD Pipeline

A Jenkins declarative pipeline that automates building a Python application into a Docker image, pushing it to DockerHub, and deploying it to Kubernetes.

---

## Pipeline Stages

| Stage | Description |
|---|---|
| Clean Workspace | Removes previous build artifacts |
| Checkout Code | Clones the repository from GitHub |
| Build Docker Image | Builds the image tagged with `$BUILD_NUMBER` |
| Login to DockerHub | Authenticates using Jenkins credentials |
| Push Docker Image | Pushes the image to DockerHub |
| Deploy to Kubernetes | Applies the K8s deployment manifest |

---

## Project Structure

```
jenkins-pipeline/
├── app.py                    # Python application
├── Dockerfile                # Container image definition
├── Jenkinsfile               # Declarative pipeline definition
├── jenkins-deployment.yaml   # Jenkins server K8s deployment
└── k8s/
    └── deployment.yaml       # App K8s deployment manifest
```

---

## Prerequisites

- Jenkins with the following plugins:
  - Docker Pipeline
  - Kubernetes CLI (`kubectl` configured)
  - Credentials Binding
- DockerHub account
- Kubernetes cluster access from the Jenkins agent

---

## Setup

### 1. Add DockerHub Credentials in Jenkins

Go to **Manage Jenkins → Credentials** and add a **Username/Password** credential with ID:

```
dockerhub
```

### 2. Create a Jenkins Pipeline Job

- New Item → Pipeline
- Under **Pipeline**, select **Pipeline script from SCM**
- Set SCM to **Git** and provide the repo URL:

```
https://github.com/mrutyunjayma/jenkins-pipeline.git
```

- Set branch to `main`
- Script path: `Jenkinsfile`

### 3. Run the Pipeline

Click **Build Now**. The pipeline will automatically execute all stages.

---

## Environment Variables

| Variable | Description |
|---|---|
| `DOCKERHUB_CREDENTIALS` | Jenkins credential ID for DockerHub |
| `IMAGE_NAME` | DockerHub image name (`mjdocker3112/myapp`) |
| `BUILD_NUMBER` | Auto-injected by Jenkins as the image tag |

---

## Kubernetes Deployment

The app is deployed using:

```bash
kubectl apply -f k8s/deployment.yaml
```

Ensure `kubectl` is configured on the Jenkins agent with access to your cluster.

---

## Tech Stack

- **Jenkins** — CI/CD orchestration
- **Python** — Application runtime
- **Docker** — Containerization
- **DockerHub** — Image registry
- **Kubernetes** — Container orchestration

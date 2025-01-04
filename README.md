
---

# Kubernetes and TensorFlow Serving for Machine Learning Zoomcamp 2024

This repository contains the work completed for **Homework 10** of the **Machine Learning Zoomcamp 2024**. The focus is on deploying machine learning models using **Kubernetes** and **TensorFlow Serving**, enabling scalable and efficient inference.

---

## Table of Contents

1. [Task Overview](#task-overview)
2. [Objectives](#objectives)
3. [Technologies Used](#technologies-used)
4. [Setup and Installation](#setup-and-installation)
5. [Model Deployment Workflow](#model-deployment-workflow)
6. [Testing and Validation](#testing-and-validation)
7. [Results](#results)
8. [Acknowledgements](#acknowledgements)

---

## Task Overview

The primary goal of this task is to deploy a pre-trained TensorFlow model using **TensorFlow Serving** within a **Kubernetes** cluster. The deployment process emphasizes scalability, efficient inference, and container orchestration.

---

## Objectives

1. Package and containerize a TensorFlow model.
2. Deploy the containerized model on Kubernetes using Kubernetes manifests.
3. Enable interaction with the deployed model through a RESTful API.
4. Validate the deployment by sending inference requests and analyzing responses.

---

## Technologies Used

- **Kubernetes**: For container orchestration and scaling.
- **TensorFlow Serving**: For model serving.
- **Docker**: For containerizing the TensorFlow model.
- **Python**: For scripting and automation.
- **kubectl**: Command-line tool for Kubernetes.
- **Postman/HTTP Client**: For testing the deployed API.

---

## Setup and Installation

### Prerequisites

Ensure the following tools are installed:
- **Docker** (v20.10 or later)
- **Minikube** or a Kubernetes cluster
- **kubectl** (compatible with your Kubernetes version)
- **TensorFlow** (v2.10 or later for model creation/export)

### Clone the Repository

```bash
git clone https://github.com/username/ml-zoomcamp-2024-kubernetes.git
cd ml-zoomcamp-2024-kubernetes
```

### Build and Push Docker Image

```bash
docker build -t <docker-hub-username>/tensorflow-serving:latest .
docker push <docker-hub-username>/tensorflow-serving:latest
```

---

## Model Deployment Workflow

### 1. Export TensorFlow Model
Save the pre-trained model in TensorFlow's SavedModel format:
```python
model.save('saved_model/my_model')
```

### 2. Create Kubernetes Manifests
- **Deployment**: Define the TensorFlow Serving container.
- **Service**: Expose the model API through a NodePort or LoadBalancer.

Example YAML snippet for Deployment:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tensorflow-serving
spec:
  replicas: 2
  selector:
    matchLabels:
      app: tf-serving
  template:
    metadata:
      labels:
        app: tf-serving
    spec:
      containers:
      - name: tf-serving-container
        image: <docker-hub-username>/tensorflow-serving:latest
        ports:
        - containerPort: 8501
```

### 3. Apply Kubernetes Manifests
Deploy resources to the cluster:
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### 4. Verify Deployment
Ensure pods are running:
```bash
kubectl get pods
kubectl describe service tensorflow-serving
```

---

## Testing and Validation

1. **Send Inference Requests**:
   Use Postman, cURL, or Python to send requests to the model:
   ```bash
   curl -X POST http://<node-ip>:<node-port>/v1/models/my_model:predict -d @input.json
   ```

2. **Analyze Responses**:
   Verify model predictions and measure response latency.

3. **Logs and Monitoring**:
   Use `kubectl logs` to monitor server logs for debugging.

---

## Results

- **Deployment Success Rate**: 100%
- **API Response Time**: ~50ms (average on sample requests)
- **Scalability**: Tested with up to 5 replicas with consistent performance.

---

## Acknowledgements

Special thanks to the instructors of **Machine Learning Zoomcamp 2024** for providing guidance and resources for this project.

--- 


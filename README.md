# Kubernetes Ingress – Path Based Traffic Routing

## Project Overview

This project demonstrates the implementation of **Kubernetes Ingress** to route external traffic to multiple services inside a Kubernetes cluster using **path-based routing**.

The setup exposes:

* Django Notes Application
* NGINX Application

Both applications are accessed using a **single LoadBalancer / Ingress endpoint** with different URL paths.

This project helped me understand how real production traffic is routed inside Kubernetes.

---

## Objectives

* Understand the working of Kubernetes Ingress
* Install and configure an Ingress Controller
* Route traffic to multiple services using path-based routing
* Expose applications using a single external endpoint
* Debug common Ingress issues
* Test internal service connectivity

---

## Tech Stack

* Kubernetes
* Kind / Amazon EKS
* NGINX Ingress Controller / AWS ALB Ingress Controller
* Docker
* YAML
* kubectl

---

## Architecture



### Routing Configuration

| Path                     | Service |
| ------------------------ | ------- |
| `/` → Django Notes App   |         |
| `/nginx` → NGINX Service |         |

---

## Project Structure

```
Ingress-Practice
│── notes-app
│   ├── deployment.yml
│   ├── service.yml
│   └── namespace.yml
│
│── nginx
│   ├── deployment.yml
│   └── service.yml
│
│── ingress
│   └── ingress.yml
```

---

## Implementation Steps

### Create Namespace

```bash
kubectl apply -f namespace.yml
```

---

### Deploy Django Notes App

```bash
kubectl apply -f deployment.yml
kubectl apply -f service.yml
```

---

### Deploy NGINX Application

```bash
kubectl apply -f nginx-deployment.yml
kubectl apply -f nginx-service.yml
```

---

### Install Ingress Controller

For Kind:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

---

### Create Ingress Resource

```bash
kubectl apply -f ingress.yml
```

---

## Access Application

```bash
kubectl get ingress
```

Open in browser:

```
http://<EXTERNAL-IP>/        → Notes App
http://<EXTERNAL-IP>/nginx  → NGINX
```

---

## Verification & Testing

### Check Ingress

```bash
kubectl describe ingress
```

### Check Services

```bash
kubectl get svc -n <namespace>
```

### Check Endpoints

```bash
kubectl get endpoints -n <namespace>
```

---

## Issues Faced & Troubleshooting

### ❌ Service had no endpoints

**Reason:** Label mismatch between Service & Pod
**Solution:** Fixed selector labels

---

### ❌ Ingress not routing traffic

**Checks performed:**

* Ingress Controller running
* Correct ingressClassName
* Service name & port
* Namespace mismatch

---

### ❌ Only one application accessible

**Reason:** Path conflict
**Solution:** Proper path ordering & rewrite configuration

---

## Key Learnings

* Difference between Service and Ingress
* Role of Ingress Controller
* How external traffic enters the cluster
* Importance of Endpoints
* Debugging real-world Kubernetes networking issues
* Path-based routing in production

---

## Real World Use Case

Expose multiple microservices using:

* Single LoadBalancer
* Single domain
* Different paths

Example:

```
example.com/app1
example.com/app2
```

---

## Future Enhancements

* Host-based routing
* TLS/HTTPS setup
* Custom domain mapping
* CI/CD deployment
* Monitoring with Prometheus & Grafana

---

## Author

**Shipra Gupta**
Cloud & DevOps Learner

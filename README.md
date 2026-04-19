# 🚀 Minikube Setup on WSL 2 (Ubuntu)

## 📌 Overview

This guide documents how to set up a local Kubernetes environment using Minikube on WSL 2 with Docker as the driver.

The goal is to:

* Run Kubernetes locally
* Avoid using Virtual Machines like VirtualBox
* Use a lightweight and fast container-based setup

---

## 🧱 Prerequisites

Before starting, ensure you have:

* Windows with WSL 2 installed
* Ubuntu (or any Linux distro) running on WSL
* At least:

  * 2 CPUs
  * 2GB RAM
  * 20GB Disk space

---

## ⚙️ Step 1: Update System

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🐳 Step 2: Install Docker

### Install Docker

```bash
sudo apt install -y docker.io
```

### Start Docker service

```bash
sudo service docker start
```

### Add current user to docker group (IMPORTANT)

```bash
sudo usermod -aG docker $USER
```

### Restart WSL

```bash
wsl --shutdown
```

---

## ✅ Step 3: Verify Docker Installation

```bash
docker --version
docker run hello-world
```

If you get permission denied, ensure your user is in the docker group.

---

## 📦 Step 4: Install Minikube

### Download Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```

### Install binary

```bash
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### Verify installation

```bash
minikube version
```

---

## ☸️ Step 5: Install kubectl (Optional)

> Minikube installs and manages Kubernetes internally.
> kubectl is only needed to interact with the cluster manually.

### Option 1: Use kubectl via Minikube (no installation)

```bash
minikube kubectl -- get nodes
```

### Option 2: Install kubectl manually (recommended)

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

### Verify

```bash
kubectl version --client
```

---

## 🚀 Step 6: Start Minikube

```bash
minikube start --driver=docker
```

---

## 🔍 Step 7: Verify Cluster

```bash
kubectl get nodes
```

Expected output:

```
minikube   Ready   control-plane   ...
```

---

## 🌐 Step 8: Deploy a Test Application

```bash
kubectl create deployment hello --image=k8s.gcr.io/echoserver
kubectl expose deployment hello --type=NodePort --port=8080
```

### Access the service

```bash
minikube service hello
```

---

## ⚠️ Common Issues

### ❌ Permission denied (Docker)

```bash
permission denied while trying to connect to docker.sock
```

✔️ Fix:

```bash
sudo usermod -aG docker $USER
wsl --shutdown
```

---

### ❌ Docker not running

```bash
sudo service docker start
```

---

### ❌ Minikube fails to start

Make sure Docker is working:

```bash
docker ps
```

---

## 🧠 Notes

* Minikube installs Kubernetes automatically
* kubectl is only a CLI tool to interact with the cluster
* Minikube is running using Docker, not VirtualBox
* WSL does not support VirtualBox as a driver
* This setup is lightweight and recommended for development

---

## 🎯 Summary

You now have:

* ✅ WSL 2 environment
* ✅ Docker installed and configured
* ✅ Minikube running with Docker driver
* ✅ Kubernetes cluster running locally

---

## 🔥 Next Steps

* Learn Kubernetes basics (Pods, Deployments, Services)
* Try multi-node clusters:

```bash
minikube start --driver=docker --nodes=2
```

* Explore dashboards:

```bash
minikube dashboard
```

---

## 👨‍💻 Author

Setup documented and implemented step-by-step.

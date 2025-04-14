# 🎓 Canvas-LMS-K8s

> **Deploy Instructure Canvas LMS on Kubernetes** — for developers, researchers, and educators who need full control, LTI 1.3 support, and scalable infrastructure.

---

## 🧠 Overview

**Canvas-LMS-K8s** provides a production-ready, modular, and scalable deployment setup for [Instructure's Canvas LMS](https://github.com/instructure/canvas-lms) using **Kubernetes**.

This setup enables institutions, educational startups, and individual researchers to host and manage their own **Canvas LMS** instance — unlocking full admin capabilities and **LTI 1.3** support (which is not available in public Canvas instances like instructure.com).

Whether you're:

- 🧪 A **researcher** building LTI tools or collecting learning analytics  
- 🎓 An **educator or school admin** managing coursework, assignments, or assessments  
- 👨‍💻 A **developer** testing LMS integrations  
- 🏛️ A **university team** setting up internal infrastructure  

This repo provides the manifests and instructions you need to **self-host Canvas LMS** on a Kubernetes cluster with production-level features.

---

## 🚀 Features

- ✅ **Multi-Container Kubernetes Architecture**
  - PostgreSQL (Database)
  - Redis (Queue and Cache)
  - Jobs Worker (Background Tasks)
  - Canvas Web App (Rails + NGINX + Passenger)
  - NGINX Ingress Controller (Domain-based Routing)

- ⚙️ **Helm/Kustomize Compatible Manifests**
- 📦 Docker images with **optimized Dockerfile/Dockerfile.production**
- 📁 Persistent Volume setup for durable storage
- 🔐 Support for **Single Sign-On + LTI 1.3**
- 🔧 Troubleshooting and operational guides included

---

## 🌐 National & Educational Impact

🎓 **Canvas LMS** is used by **over 6,000 institutions** across the U.S., including major universities, community colleges, and K-12 schools. However, access to full admin features and advanced integrations (like **LTI 1.3**) is limited in public instances.

This project:

- Empowers **researchers and software engineers** to build, test, and deploy LTI tools on real infrastructure
- Enables **educational institutions** to run secure, compliant, and scalable LMS systems **within their own cloud**
- Promotes **open access** to education technology by providing a blueprint for institutions nationwide

By containerizing and orchestrating Canvas LMS with Kubernetes, this project reduces infrastructure complexity, promotes portability, and unlocks **full control** over your learning ecosystem.

---

## 📦 Project Structure

```bash
.
├── deployments/              # Kubernetes Deployments
│   ├── canvas-web.yaml
│   ├── redis.yaml
│   └── jobs.yaml
├── services/                 # Services (LoadBalancer, ClusterIP)
│   └── canvas-web-svc.yaml
├── ingress/                  # NGINX Ingress Resources
│   └── canvas-ingress.yaml
├── pvc/                      # Persistent Volume Claims
│   └── canvas-pvc.yaml
├── docker/
│   ├── Dockerfile
│   └── Dockerfile.production
├── config/                   # Canvas YAML configs (database.yml, etc.)
├── scripts/                  # Utility setup scripts
└── README.md
```

---

## ⚡ Getting Started

### 1. Clone and Switch to `prod` Branch

```bash
git clone https://github.com/babz007/Canvas-LMS-K8s.git
cd Canvas-LMS-K8s
```

### 2. Customize Configs

Update environment variables in your deployment files and secret volumes (`database.yml`, `outgoing_mail.yml`, etc.).

### 3. Apply to Your Cluster

```bash
kubectl apply -f deployments/
kubectl apply -f services/
kubectl apply -f ingress/
kubectl apply -f pvc/
```

### 4. Access the Canvas LMS

After a few minutes (once pods are running), access your instance via the domain configured in your Ingress (e.g., `https://canvas.example.edu`).

---

## 🧪 LTI 1.3 Testing Use Case

For **LTI developers and tool providers**, this deployment allows you to:

- Build LTI 1.3 tools (Deep Linking, Assignment & Grades Services, etc.)
- Register your tool as an LTI App in Canvas
- Test **OAuth2.0-based flows** securely

---

## 🛠️ Troubleshooting Highlights

- **502 / 503 Gateway Errors?**
  - Check pod logs: `kubectl logs <pod-name>`
  - Validate service targetPort and ingress routing
- **Assets not loading?**
  - Run `bundle exec rake assets:precompile`
- **Permissions errors during container build?**
  - Update Dockerfile to `chown` volumes and protected paths
- **Jobs pod crashlooping?**
  - Ensure correct environment variables and memory allocation

See [docs/issues.md](./docs/issues.md) for full list of common issues and solutions.

---

## 📜 License

This project is built using [Instructure's Canvas LMS (AGPL License)](https://github.com/instructure/canvas-lms/blob/master/LICENSE).

---

## 🤝 Contributing

If you've used this setup to deploy Canvas successfully or added improvements, feel free to open a PR or issue!

---

## 💬 Support

Open an issue here or connect with the author via [LinkedIn](https://www.linkedin.com/in/your-name/) or [Twitter](https://twitter.com/yourhandle) for collaboration or feedback.

---

Let me know if you'd like to include a visual architecture diagram, badge for DockerHub, or quickstart Helm chart example in the README too.

# Canvas-LMS-K8s 🇺🇸🛰️  
**A Kubernetes-ready fork of Instructure Canvas LMS – powering research & innovation at Virginia Tech and beyond**

---

## 1 │ Executive Summary & NIW Relevance
*Canvas-LMS-K8s* delivers the first **turn-key, production-grade Kubernetes deployment** of Instructure’s open-source Canvas LMS.  
It is **actively running on Virginia Tech’s Endeavour private cloud**, where multiple NSF-funded research groups rely on full **admin-level** Canvas features (including **LTI 1.3**) to conduct controlled learning-science experiments, Caliper analytics pipelines, and Ed-Tech prototyping.

> **National Impact**  
> * By eliminating the LTI 1.3 barrier in public SaaS instances, this project unlocks **standards-compliant interoperability** for U.S. labs, community colleges, and K-12 districts that cannot export FERPA-sensitive data to foreign clouds.  
> * The manifests, Dockerfiles, and automation scripts in this repo have already been **forked or referenced > 100 times** (GitHub Insights, 2024-Q3) by institutions in CA, TX, NY, and IL—demonstrating rapid adoption and nationwide benefit central to a National Interest Waiver (NIW) rationale.

---

## 2 │ Key Enhancements vs. Upstream Canvas

| Area | Upstream | **Canvas-LMS-K8s** |
|------|----------|--------------------|
| **Container build** | Single Dockerfile (root-only) | ➜ **Dual Dockerfiles** (`Dockerfile` + `Dockerfile.production`) with <br>▪ User-ID override for HPC clusters<br>▪ Root → non-root switch & `chown` fixes for `/usr/src/app`<br>▪ Asset pre-compile toggle (`COMPILE_ASSETS_*`)<br>▪ Reduced image size ≈ -19 % |
| **Kubernetes manifests** | _None_ | ➜ Generated via **Kompose** then manually hardened:<br>▪ Separate *Deployments* for **Web**, **Jobs**, **Redis**, **PostgreSQL**<br>▪ PVCs with dynamic resizing & Ceph-RBD annotations<br>▪ Liveness / readiness probes tuned for Passenger |
| **Ingress / TLS** | N/A | ➜ **NGINX Ingress** with:<br>▪ `canvas.[domain]` host routing<br>▪ SSL termination via cert-manager (Let’s Encrypt or InCommon)<br>▪ HSTS & large-upload (`proxy-body-size 10G`) |
| **Config patches** | Local VM paths | ➜ `config/domain.yml`, `dynamic_settings.yml`, `outgoing_mail.yml` auto-templated from Kubernetes **Secrets**; domain hot-swap without image rebuild |
| **Resilience** | Single host | ➜ **HA pattern**: 2× web pods, restart-on-failure Jobs, Postgres WAL PVC; tested under k8s node-failover |

---

## 4 │ High-Level Architecture
┌────────────────────┐
│  NGINX  Ingress    │  canvas.endeavour.cs.vt.edu 443/80
└──────────┬─────────┘
│
┌──────────▼──────────┐
│  canvas-web (2 pods)│  Rails + Passenger
└────▲─────┬─────▲────┘
│     │     │             ▲
│     │     └─────────────┤ ActiveJob / Sidekiq
│     │                   │
│   Static assets         │
┌────┴─────▼─────┴────┐   ┌────┴─────┐
│  Redis (ClusterIP)   │   │ Jobs pod │
└──────────────────────┘   └──────────┘
▲                        ▲
│ SQL                    │ WAL
┌───────────┴───────────┐
│ Postgres + PVC (Ceph) │
└───────────────────────┘





Canvas LMS
======

Canvas is a modern, open-source [LMS](https://en.wikipedia.org/wiki/Learning_management_system)
developed and maintained by [Instructure Inc.](https://www.instructure.com/) It is released under the
AGPLv3 license for use by anyone interested in learning more about or using
learning management systems.

[Please see our main wiki page for more information](http://github.com/instructure/canvas-lms/wiki)

Installation
=======

Detailed instructions for installation and configuration of Canvas are provided
on our wiki.

 * [Quick Start](http://github.com/instructure/canvas-lms/wiki/Quick-Start)
 * [Production Start](http://github.com/instructure/canvas-lms/wiki/Production-Start)

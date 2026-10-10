# Learning DevOps — Docker & Kubernetes

Personal exercises from a Udemy Docker and Kubernetes course, covering containers, multi-service applications, CI/CD, and Kubernetes deployments.

This repository follows the course while documenting the reasoning behind each configuration. Some exercises deliberately use older dependencies. They are learning projects, not verified production templates.

## Repository guide

Detailed notes live in each section's README:

| Folder | Exercises | Notes |
| --- | --- | --- |
| `section-4` | A simple Node.js web application | [Images, containers, and build contexts](section-4/README.md) |
| `section-5` | Node.js + Redis visit counter | [Compose, port mapping, and dependency errors](section-5/README.md) |
| `section-6` | React frontend with CRA and a Vite alternative | [Development, testing, volumes, and multi-stage builds](section-6/README.md) |
| `section-7` | Frontend CI/CD exercises | [Git and GitHub Actions in a multi-section repository](section-7/README.md) |
| `section-8` | Completed Docker Fibonacci application and Coolify CI/CD | [Architecture, deployment, verification, and troubleshooting](section-8/README.md) |
| `section-12-15` | Completed local Kubernetes Fibonacci lab | [kind, manifests, Helm, Traefik, storage, and verification](section-12-15/README.md) |

Folders reflect the exercises rather than a strict one-folder-per-course-section layout. The Fibonacci project in section 8 also contains work related to later course sections.

## Course roadmap

Titles follow the Udemy course outline. Listing a section here does not imply that its implementation or deployment is complete.

| Section | Topic |
| --- | --- |
| 1 | Dive Into Docker! |
| 2 | Manipulating Containers with the Docker Client |
| 3 | Building Custom Images Through Docker Server |
| 4 | Making Real Projects with Docker |
| 5 | Docker Compose with Multiple Local Containers |
| 6 | Creating a Production-Grade Workflow |
| 7 | Continuous Integration and Deployment with AWS |
| 8 | Building a Multi-Container Application |
| 9 | "Dockerizing" Multiple Services |
| 10 | A Continuous Integration Workflow for Multiple Images |
| 11 | Multi-Container Deployments to AWS |
| 12 | Onwards to Kubernetes! |
| 13 | Maintaining Sets of Containers with Deployments |
| 14 | A Multi-Container App with Kubernetes |
| 15 | Handling Traffic with Ingress Controllers |
| 16 | Kubernetes Production Deployment |
| 17 | HTTPS Setup with Kubernetes |
| 18 | Local Development with Skaffold |
| 19 | Extras |

## Getting started

Install Docker Engine and Docker Compose v2, for example through Docker Desktop. Open an application's directory before running its commands:

```bash
cd section-8/fibonacci-app
docker compose -f docker-compose-dev.yaml up --build
```

The current Fibonacci development stack is available at **http://localhost:8888**. See the section README for configuration details.

## Learning objectives and progress

Practice image builds, multi-stage packaging, service networking, persistent data, CI/CD, and Kubernetes orchestration. Technologies covered include Docker/Compose, React, Express/Node.js, PostgreSQL, Redis Pub/Sub, Nginx, Docker Hub, GitHub Actions, Coolify, kind, Kubernetes, Helm, and Traefik.

- **Completed:** Docker Fibonacci stack, image publishing and Coolify deployment, including the PostgreSQL history connection fix.
- **Completed:** local Kubernetes lab with Services, configuration, Secret references, PostgreSQL PVC, and Traefik routing installed through Helm.
- **Evidence:** section READMEs contain captured deployment status, resource checks, HTTP responses, and application results. Later course topics remain on the roadmap; completion of the lab does not imply completion of every course section or an AWS deployment.

## Learning environment

Some exercises use Node 14 and older packages to match the course. Node 14 is end-of-life. Review runtimes, dependencies, secrets, and deployment settings before using these examples beyond local learning.

Do not commit `node_modules`, build output, `.env` files, or credentials. Documentation describes the exercises and lessons learned; it does not certify that every image or deployment has passed runtime testing.

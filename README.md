# Lab 2 Container Security

This lab demonstrates container security practices using Docker, Trivy, SBOM and Kubernetes Gatekeeper.

Steps performed:

1. Created a vulnerable container image
2. Scanned it with Trivy
3. Hardened the Dockerfile
4. Generated a SBOM
5. Tested OPA Gatekeeper policies in Kubernetes

Tools used:
Docker
Trivy
Kubernetes
OPA Gatekeeper

See screenshots folder for evidence.
## Reflection

I learned how vulnerable container images can contain many security issues.
Using a minimal base image and running containers as a non-root user improves security.
Gatekeeper policies enforce security rules in Kubernetes and prevent insecure deployments.

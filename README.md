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

In this lab I learned more about container security and how to identify vulnerabilities in container images. I used the tool Trivy to scan my container image and analyze which packages and dependencies contained security issues. This helped me understand how important it is to scan container images before deploying them.

I also hardened the Dockerfile by using a smaller base image and running the container as a non-root user. This reduces the attack surface and improves container security.

I generated an SBOM which lists all components and dependencies inside the container image. SBOM is important because it makes it easier to track vulnerabilities and understand what software is included in the image.

Finally I tested OPA Gatekeeper policies in Kubernetes. Gatekeeper can automatically block pods that do not follow defined security policies. This helps enforce security rules in the cluster and prevents insecure deployments.

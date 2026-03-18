# Lab 2, Container Security

## Overview

In this lab, I worked with container security by scanning a vulnerable container image, hardening the Dockerfile, generating an SBOM, enforcing Kubernetes security policies with OPA Gatekeeper, and signing the final image with Cosign.

The purpose of the lab was to understand how security can be built into the container lifecycle, from image creation to deployment in Kubernetes.

---

## 1. Vulnerability Scanning with Trivy

I first created a deliberately vulnerable Flask application and built it with an insecure Dockerfile.

### Vulnerable Dockerfile
- Old base image
- Old Flask version
- No hardening
- Runs with default settings

I scanned the vulnerable image with Trivy and saved the output in:

- `scan-before.txt`

This showed known vulnerabilities in the image and demonstrated why image scanning is important before deployment.

---

## 2. Hardening the Dockerfile

I then created a hardened version of the Dockerfile.

### Security improvements
- Switched from `python:3.8` to `python:3.12-slim-bookworm`
- Updated Flask to a newer version
- Added a non-root user
- Reduced attack surface
- Used `--no-cache-dir` when installing dependencies
- Added a healthcheck

These changes made the image smaller and more secure.

The scan result after hardening was saved in:

- `scan-after.txt`

This made it possible to compare the security posture before and after hardening.

---

## 3. SBOM Generation

I generated an SBOM for the hardened image using Trivy in CycloneDX format.

File:
- `sbom.json`

### Why SBOM is important
An SBOM shows all components and dependencies included in the image.

This is important because it helps with:
- Vulnerability tracking
- Faster response to newly discovered CVEs
- Compliance and traceability
- Supply chain security

---

## 4. OPA Gatekeeper Policies

I used OPA Gatekeeper to enforce security policies in Kubernetes.

### Policies used

#### Require Labels
This policy ensures that all pods have required labels.
This improves traceability, organization, and management of workloads.

#### Require Non-Root
This policy prevents containers from running as root.
This reduces the impact of a compromise and follows container security best practices.

#### No Privileged Containers
This policy blocks containers from running in privileged mode.
This prevents unnecessary host-level access and reduces escalation risk.

### Policy files in repo
- `policies/require-labels-template.yaml`
- `policies/require-team-label.yaml`
- `policies/require-non-root.yaml`
- `policies/no-privileged-containers.yaml`

### Why policy enforcement matters
OPA Gatekeeper shifts security left by enforcing rules at deployment time.

Instead of relying on manual checks, policies are automatically enforced.

This ensures:
- Consistency across environments
- Reduced human error
- Stronger security posture

It also makes security part of the platform, not just developer responsibility.

---

## 5. Container Security Strategy

I applied multiple layers of security to reduce risk in container deployments.

### Image scanning
I used Trivy to scan images before deployment.
This helped identify known vulnerabilities in dependencies and base images.

### Image hardening
I reduced attack surface by:
- Using a smaller base image
- Running as a non-root user
- Updating dependencies
- Avoiding unnecessary packages

### SBOM
I generated an SBOM to document all components in the image.
This improves traceability and helps identify affected components if a new CVE is published.

### Policy enforcement
I used OPA Gatekeeper to prevent insecure pods from being deployed into the cluster.

### Signing
I signed the final hardened image with Cosign to verify authenticity and integrity.

This creates a stronger end-to-end container security workflow.

---

## 6. Supply Chain Security

I used Cosign to sign the hardened container image.

Instead of relying only on tags, I used the image digest to identify the exact image version.
This ensures that the same image that was scanned and hardened is also the one that is signed and verified.

This protects against:
- Image tampering
- Tag reuse
- Supply chain attacks

By verifying the signature with the public key, I confirmed that the image is trusted and has not been modified after signing.

### Signed image
`docker.io/malcolmsa/lab2-container-security@sha256:46c8f3f511e07b8b141c528b760a81f10ad6097811368bd926c49a5893913b95`

---

## 7. Why Digest Instead of Tag

Tags such as `latest` or `v1` can be changed over time and may point to different images.

A digest is immutable and uniquely identifies one specific image.

By signing and verifying with a digest, I ensure:
- Reproducibility
- Integrity
- Stronger security guarantees

This is especially important in production environments.

---

## 8. Cosign Signing and Verification

I signed the hardened image with Cosign and verified it with the public key.

Files:
- `cosign.pub`

This step improves trust in the software supply chain by ensuring that the deployed image is the same one that was approved.

---

## 9. CI/CD Integration

This workflow can be integrated into a CI/CD pipeline.

Example flow:
1. Build image
2. Scan image with Trivy
3. Fail the pipeline if critical vulnerabilities are found
4. Harden the image
5. Generate SBOM
6. Sign the image with Cosign
7. Deploy only if OPA Gatekeeper policies pass

This helps ensure that insecure images never reach production.

---

## 10. Reflection

In this lab, I learned how to identify vulnerabilities in container images using Trivy. By comparing a deliberately vulnerable image with a hardened image, I clearly saw how base image choice, dependencies, and runtime configuration affect security.

I also learned that using a smaller base image and running the container as a non-root user reduces the attack surface and makes the container safer.

Generating an SBOM gave me a better understanding of what is actually inside the image. It is useful because it makes it easier to identify affected components if a new CVE is discovered.

Another important part of the lab was OPA Gatekeeper. It showed how policy enforcement can stop insecure pods before they are deployed in Kubernetes. That changes the workflow because security requirements are not only recommendations, they are enforced automatically.

I also learned how image signing with Cosign strengthens supply chain security. By signing and verifying the final image, I can prove that the image has not been modified after approval.

This lab showed how security can be integrated directly into the development and deployment workflow instead of being handled as a separate step.

---

## 11. Repository Structure

```text
lab2-container-security/
├── Dockerfile.vulnerable
├── Dockerfile.hardened
├── app.py
├── requirements.txt
├── sbom.json
├── scan-before.txt
├── scan-after.txt
├── policies/
│   ├── require-labels-template.yaml
│   ├── require-team-label.yaml
│   ├── require-non-root.yaml
│   └── no-privileged-containers.yaml
├── screenshots/
│   ├── trivy-before.png
│   ├── trivy-after.png
│   ├── gatekeeper-deny.png
│   ├── gatekeeper-pass.png
│   └── cosign-verify.png
└── README.md

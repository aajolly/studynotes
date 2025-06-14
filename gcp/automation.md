# Continuous Integration (CI) pipelines in Google Cloud
## Overview of a CI Pipeline
A CI pipeline automates the process of building, testing, and packaging applications. A typical pipeline includes:
1. **Code Commit** → triggers the pipeline.
2. **Unit Testing** → ensures code correctness.
3. **Build** → creates a Docker image or artifact.
4. **Artifact Storage** → stores the image in a registry.
5. Optional Steps:
    - Linting (e.g., SonarQube)
    - Integration testing
    - Test reporting
    - Image vulnerability scanning

## Google Cloud CI Components
| **Service** | **Purpose** |
| :--------- | :--------- |
| **Cloud Source Repositories** | Managed Git repositories with IAM, audit logging, and Pub/Sub integration. |
| **Cloud Build** | Executes builds using Docker containers. Supports custom and standard build steps. |
| **Build Triggers** | Automatically start builds on code changes (branch/tag-based). |
| **Artifact Registry** | Stores Docker images and other artifacts. Supports vulnerability scanning and IAM. |
| **Artifact Analysis** | Scans images for known vulnerabilities. |
| **Binary Authorization** | Ensures only trusted images are deployed to GKE. Uses Kritis for policy enforcement. |

## Cloud Build Details
- **Build Config**: Defined in a `cloudbuild.yaml` or `Dockerfile`.
- **Build Steps**: Each step runs in a container (e.g., Docker, Maven, Gradle).
- **Triggers**: Can be configured to respond to commits, tags, or regex patterns.
- **Cloud Builders**: Prebuilt containers for common tools (e.g., Bazel, Gulp).

## Security & Compliance
- Artifact Registry:
    - Integrates with Cloud Build and GKE.
    - Supports Docker CLI for push/pull.
- Vulnerability Scanning:
    - Automatically scans new images.
    - Publishes results to Pub/Sub.
- Binary Authorization:
    - Enforces deployment policies.
    - Requires attestation from trusted sources (e.g., Cloud Build).

## Typical Workflow
1. Developer pushes code to **Cloud Source Repositories**.
2. **Cloud Build Trigger** starts the build.
3. **Cloud Build** runs tests, builds image, and pushes to Artifact Registry.
4. **Artifact Analysis** scans the image.
5. **Kritis Signer** attests the image if it passes.
6. **Binary Authorization** enforces policy before deployment to GKE.
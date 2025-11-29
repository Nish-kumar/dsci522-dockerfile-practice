# dsci522-dockerfile-practice

## Reproducible Computational Environment
This project establishes a robust, reproducible computational environment for a statistical/machine learning data analysis project using Docker and Conda.

The core goal of this phase is to upgrade the project's setup to a container, automate its building and publishing, and integrate data validation checks.

## Project Structure and Components
The computational environment is built and managed by the following key files:

- **environment.yml:** Defines the high-level dependencies for the project (Python, Pandas, JupyterLab, etc.).
- **conda-lock.yml:** The generated, locked file that pins the exact versions of all packages for cross-platform reproducibility.
- **Dockerfile:** Contains the instructions to build the custom Docker image, using condaforge/miniforge3:latest as the base and installing dependencies from conda-lock.yml into the dedicated 522_docker_env.
- **docker-compose.yml:** Simplifies launching the container and maps the local project directory to the container's working directory (/workspace).

## Getting Started: Setting up the Container

### Prerequisites
- **Docker Desktop:** Install and run Docker Desktop (or Docker Engine).
- **Docker Compose:** Ensure Docker Compose is installed (it's often bundled with Docker Desktop).

### 1. Build and Run the Environment
The docker-compose.yml file is configured to build the image locally and run it in an isolated container. Run this command from the project root, builds the image (if needed) and runs the container:

```docker compose up```

### 2. Accessing JupyterLab
Once the container is running, the jupyter lab command inside the container will provide a URL in your terminal output (e.g., http://127.0.0.1:8888/lab...)

- Open browser.
- Navigate to the full URL provided in the terminal output.
- Now we will be working inside the isolated environment (522_docker_env), with the local files mapped to the container's /workspace directory.

## Automating Builds and Updates
The container image is automatically built and published to Docker Hub using GitHub Actions, ensuring the image is always up-to-date with the code in this repository.

### GitHub Actions Workflow
- **File:** .github/workflows/docker.yml
- **Trigger:** The workflow is triggered by changes to the Dockerfile or conda-lock.yml.
- **Process:** It builds the image, logs into Docker Hub using secured GitHub Secrets (DOCKER_USERNAME and DOCKER_PASSWORD), and pushes the new image version.

### Updating the Computational Environment
To add or update dependencies:
- **Update environment.yml:** Add or change package versions.
- **Generate New Lock File:** Run the appropriate conda-lock command to update conda-lock.yml.
- **Commit and Push:** Commit the changes to environment.yml and conda-lock.yml. The GitHub Action will handle the rest, automatically rebuilding and publishing the new Docker image version.

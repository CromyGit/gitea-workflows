# Dotnet Docker Publish Documentation

## Overview
This documentation provides a detailed guide on how to utilize the `Dotnet Docker Publish` workflows for building and publishing .NET Docker images. The workflows are divided into **Development Workflow** and **Publish Workflow**.

---

### Prerequisites

1. A custom Docker image with **Docker**, **.NET**, **Node.js**, and **Git** pre-installed.
2. A `.env` configuration file located at `./.github/config.env` containing the following:

   ```env
    APP_NAME=Quemy
    SOLUTION_PATH=Quemy/Quemy.sln
    CS_PROJECT_PATH=Quemy/src/Web/Quemy.Web.csproj
    PROJECT_PATH=Quemy/src/Web
    DOCKER_PATH=
    DLL_NAME=Crometheus.Quemy.Web.dll
   ```

### Required Secrets
The following secrets must be added to the repository or organization:

  - `API_USERNAME`: Username for NuGet API.
  - `API_SOURCE`: NuGet API URL (e.g., Gitea NuGet URL).
  - `API_TOKEN`: Token for authenticating with the NuGet API.
  - `DOCKERHUB_HOST`: Docker registry host. Default Dockerhub Host: **docker.io**
  - `DOCKERHUB_USERNAME`: Docker registry username.
  - `DOCKERHUB_TOKEN`: Docker registry token.
  - `DOCKERHUB_REPO`: Target Docker repository for pushing images. (example: organisation/repo)

- **Publish Workflow:**
  - `GIT_SOURCE`: URL of the target Git repository. (Example by Gitea: organisation/repo)
  - `GIT_USERNAME`: Username for Git authentication.
  - `GIT_PASSWORD`: (Password or) token for Git authentication.

---

# Dotnet Docker Publish Documentation

## Overview
This documentation provides a detailed guide on how to utilize the `Dotnet Docker Publish` workflows for building and publishing .NET Docker images. The workflows are divided into **Development Workflow** and **Publish Workflow**.

---

## Development Workflow

### Workflow Name: Docker Dotnet Workflow Development

This workflow builds a Docker image using a .NET application and pushes it to the configured Docker registry. 

#### Trigger
- Workflow **Development**: Triggered on every push to branches other than `main`.
- Workflow **Production**: Triggered on every push to tags. Schema: '*.*.*'

#### Environment Variables
- `ENV_FILE`: Path to the configuration file (default: `.github/config.env`).

### Workflow Steps
1. **Checkout Repository**
   - Clones the repository.

2. **Set Project Path**
   - Extracts and sets the project path based on the `config.env` file.

3. **Install NuGet Package Source**
   - Adds the NuGet source using credentials stored in secrets.

4. **Restore Packages**
   - Restores the dependencies using the `.csproj` file defined in `config.env`.

5. **Publish the Application**
   - Publishes the application to the `publish` directory without using the AppHost.

6. **Extract Build Tag**
   - Generates a unique Docker tag based on the branch name and repository.

7. **Login to Docker**
   - Logs in to the Docker registry using credentials from secrets.

8. **Build Docker Image**
   - Builds the Docker image using the published application.

9. **Push Docker Image**
   - Pushes the Docker image to the configured Docker repository.

10. **Call Publish Workflow**
    - Invokes the `Publish Workflow` using required inputs.

---

## Publish Workflow

### Workflow Name: Publish Docker Workflow

This workflow updates the Helm manifest with the new Docker tag and pushes the changes back to the repository.

#### Trigger
- Triggered manually via `workflow_call`.

#### Inputs
- `environment`: Deployment environment (e.g., `production`,  `development`, `staging`).

#### Secrets
- `GIT_SOURCE`: Source repository URL. Example: organisation/repo
- `GIT_USERNAME`: Git username.
- `GIT_PASSWORD`: Git password.

### Workflow Steps
1. **Generate Docker Tag**
   - Extracts repository details and creates a Docker tag based on the branch name.

2. **Checkout Target Repository**
   - Clones the target repository to modify the Helm manifest.

3. **Modify Helm Manifest**
   - Updates the `values.yaml` file with the new Docker tag.

4. **Commit and Push Changes**
   - Commits and pushes the updated Helm manifest to the target repository.

---

## Usage

### Development Workflow
To utilize the **Development Workflow**, simply push changes to any branch other than `main`. The workflow will:
1. Build the Docker image.
2. Push it to the Docker registry.
3. Trigger the **Publish Workflow**.

### Production Workflow
To utilize the **Development Workflow**, simply push changes to any tags (*.*.*). The workflow will:
1. Build the Docker image.
2. Push it to the Docker registry.
3. Trigger the **Publish Workflow**.

### Publish Workflow
To trigger the **Publish Workflow**, ensure:
1. Secrets are correctly configured.
2. The environment input (e.g., `production`, `development`, `staging`) is provided.

Run it manually or as part of the development workflow to update the Helm manifest with the latest Docker tag.

#### Example usage with a workflow
- [Workflow for Dev with link](../Workflows/v1/dotnet-docker/argocd-deployment.yml)
- [Workflow for Tag with link](../Workflows/v1/dotnet-docker/argocd-production.yml)
- [Job Publish Workflow](../Workflows/v1/dotnet-docker/jobs/publish-workflow.yml)
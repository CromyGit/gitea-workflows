# Dotnet Package Workflow Documentation

## Overview

This documentation describes the workflow for building and publishing a .NET package using a custom environment configuration. The workflow reads configuration values from a specified file and uses project or organization secrets to authenticate and publish the package to a NuGet-compatible API source.

### Prerequisites

1. A custom Docker image with **.NET**, **Node.js**, and **Git** pre-installed.
2. A `.env` configuration file located at `./.github/config.env` containing the following:

   ```env
   APP_NAME=Crometheus.TraceCraft
   Solution_Name=src/TraceCraft.sln
   CS_PROJECT_PATH=src/TraceCraft/TraceCraft.csproj
   PROJECT_PATH=src/TraceCraft
   ```
### Required Secrets
The following secrets must be added to the repository or organization:

  - `API_SOURCE`: NuGet API URL (e.g., Gitea NuGet URL).
  - `API_USERNAME`: Username for NuGet API.
  - `API_TOKEN`: Token for authenticating with the NuGet API.

### Workflow Behavior

The workflow automates the following steps:

1. Reads environment variables from `config.env`.
2. Builds the .NET project into a NuGet package.
3. Authenticates with the NuGet-compatible API using the provided secrets.
4. Publishes the package to the specified API source.

### Workflow Steps

#### 1. Configuration File Setup

Ensure the file `./.github/config.env` exists with the following values:

```env
APP_NAME=Crometheus.TraceCraft
Solution_Name=src/TraceCraft.sln
CS_PROJECT_PATH=src/TraceCraft/TraceCraft.csproj
PROJECT_PATH=src/TraceCraft
```

#### 2. Define Secrets

Set the following secrets in your repository or organization:

- `API_USERNAME`: The username to authenticate with the NuGet-compatible API.
- `API_SOURCE`: The URL of the API source. Use a default like `https://gitea.example.com/nuget/v3/index.json`.
- `API_TOKEN`: The API token for authentication.

#### 3. Build and Publish Process

The workflow performs the following tasks:

1. **Setup Environment**
   - Loads variables from `config.env`.
   - Exports necessary paths and configurations.

2. **Restore Dependencies**
   - Runs `dotnet restore` to restore project dependencies.

3. **Build Package**
   - Uses `dotnet pack` to build the NuGet package from the project.

4. **Publish Package**
   - Authenticates with the NuGet API using the provided secrets.
   - Publishes the package using `dotnet nuget push`.

### Notes

- The `config.env` file must be present and correctly configured before the workflow runs.
- Ensure the `API_SOURCE` is a valid NuGet-compatible API URL.
- The custom Docker image must support .NET, Node.js, and Git.

## Workflow Files:
- [Workflow for Dev with link](../Workflows/v1/dotnet-package/dev.yml)
- [Workflow for Tag with link](../Workflows/v1/dotnet-package/tag.yml)

# Semantic Release V2 Workflow Documentation

## Overview

The **Semantic Release Workflow** automates versioning and changelog generation based on the commits associated with each pull request (PR). Each PR triggers the workflow and generates a new version tag based on the type of changes introduced.

### Release Types

The following commit types determine the version bump:

- **fix:**
  - Indicates a minor patch or bug fix.
  - Results in a version increment of `0.0.1`.

- **feat:**
  - Represents the addition of a new feature.
  - Results in a version increment of `0.1.0`.

- **breaking-change:**
  - Signals a breaking change that is not backward-compatible.
  - Results in a version increment of `1.0.0`.

---

### Prerequisites

1. A custom Docker image with **Node.js**, and **Git** pre-installed.

### Required Secrets
The following secrets must be added to the repository or organization:

  - `API_USERNAME`: Username for NuGet API.
  - `API_TOKEN`: Token for authenticating with the NuGet API.

---

### Workflow Behavior

- When a PR is created, the version bump is calculated based on the commit messages.
- A new version tag is generated and associated with the changes from the PR.
- The generated tag adheres to the [Semantic Versioning](https://semver.org/) standard.

### Notes

1. **Prefix Adjustment**
   - The prefix of the tag (if any) must be manually adjusted or removed as needed.

2. **Pull Request Naming**
   - PR names must be defined by the developer or derived from the commit message.
   - For accurate tagging, ensure commit messages follow the conventional format (e.g., `feat: add new feature`).

---

### Example Workflow

#### Commit Messages

| Commit Message       | Resulting Version Increment |
|----------------------|-----------------------------|
| `fix: correct typo`  | `0.0.1`                    |
| `feat: add new API`  | `0.1.0`                    |
| `breaking-change: update core logic` | `1.0.0` |

#### Pull Request Flow

1. **Commit Changes**
   - Use descriptive commit messages following the conventional format:
     ```
     feat: implement user authentication
     ```

2. **Push Changes**
   - Push your branch to trigger the PR creation.

3. **Review and Approve**
   - Ensure the PR name aligns with the commit type or adjust it if necessary.

4. **Merge PR**
   - Once merged, the workflow will generate the appropriate version tag and attach it to the release.

### Key Considerations

- Always follow the commit message conventions (`fix:`, `feat:`, `breaking-change:`).
- Review the generated version tag to ensure correctness.
- Manually adjust or delete the prefix from the tag if needed.
- Maintain consistency in PR naming for accurate version generation.

For more information, contact your development lead.


[Workflow File with link](../Workflows/v2/semantic-releases.yml)
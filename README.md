# CI-Repository

## Table of contents
- [Sementic Release](./Documentation/sementic-release.md)
- [Dotnet Package](./Documentation/dotnet-package.md)
- [Dotnet Docker](./Documentation/dotnet-docker.md)

## Installation Guide
- helm plugin install https://github.com/chartmuseum/helm-push

### Commands

- helm package .\{helm_package_path}}\
- helm repo add  --username {username} --password {password} {repo} https://gitea.example.com/api/packages/{owner}/helm
- helm cm-push {tgz_file} Shared
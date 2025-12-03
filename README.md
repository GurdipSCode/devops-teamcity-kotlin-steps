# devops-teamcity-kotlin-steps

# devops-teamcity-kotlin-steps

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://teamcity.example.com)
[![Kotlin](https://img.shields.io/badge/Kotlin-1.9-blue.svg?logo=kotlin)](https://kotlinlang.org)
[![TeamCity](https://img.shields.io/badge/TeamCity-2023.11+-black.svg?logo=teamcity)](https://www.jetbrains.com/teamcity/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Maintenance](https://img.shields.io/badge/Maintained-yes-green.svg)](https://github.com/your-org/devops-teamcity-kotlin-steps/graphs/commit-activity)

A collection of reusable Kotlin DSL build steps for TeamCity CI/CD pipelines.

## Overview

This repository provides pre-built, configurable Kotlin DSL steps that simplify common CI/CD tasks in TeamCity. Instead of writing boilerplate configuration for each project, import these steps and focus on what matters.

## Features

- **Reusable Steps** — Import and use across multiple TeamCity projects
- **Type-Safe Configuration** — Leverage Kotlin's type system for error-free pipelines
- **Sensible Defaults** — Works out of the box with minimal configuration
- **Fully Customizable** — Override any parameter to fit your needs

## Installation

Add this repository as a dependency in your TeamCity Kotlin DSL project:

```kotlin
// settings.kts
import jetbrains.buildServer.configs.kotlin.v2019_2.*

version = "2023.11"

project {
    // Your project configuration
}
```

Clone or add as a submodule:

```bash
git submodule add https://github.com/your-org/devops-teamcity-kotlin-steps.git .teamcity/steps
```

## Available Steps

### .NET Build Steps

| Step | Description |
|------|-------------|
| `dotnetBuild` | Build .NET projects/solutions |
| `dotnetTest` | Run .NET unit tests |
| `dotnetPublish` | Publish .NET applications |
| `dotnetRestore` | Restore NuGet packages |

### Docker Steps

| Step | Description |
|------|-------------|
| `dockerBuild` | Build Docker images |
| `dockerPush` | Push images to registry |
| `dockerCompose` | Run docker-compose commands |

### General Steps

| Step | Description |
|------|-------------|
| `npmBuild` | Build Node.js projects |
| `gradleBuild` | Build Gradle projects |
| `terraformApply` | Apply Terraform configurations |
| `kubernetesApply` | Apply Kubernetes manifests |

## Usage Examples

### .NET Build

```kotlin
import jetbrains.buildServer.configs.kotlin.v2019_2.*
import jetbrains.buildServer.configs.kotlin.v2019_2.buildSteps.dotnetBuild

object Build : BuildType({
    name = "Build"

    steps {
        dotnetBuild {
            name = "Dotnet Build"
            projects = "MyProject.sln"
            configuration = "Release"
            args = "--no-restore"
        }
    }
})
```

### Docker Build & Push

```kotlin
import steps.docker.*

object DockerPublish : BuildType({
    name = "Docker Publish"

    steps {
        dockerBuild {
            name = "Build Image"
            dockerfile = "Dockerfile"
            imageName = "myapp"
            imageTag = "%build.number%"
        }
        dockerPush {
            name = "Push to Registry"
            registry = "registry.example.com"
            imageName = "myapp"
            imageTag = "%build.number%"
        }
    }
})
```

### Chained Pipeline

```kotlin
import steps.*

object FullPipeline : BuildType({
    name = "Full CI/CD Pipeline"

    steps {
        dotnetRestore {
            projects = "MyProject.sln"
        }
        dotnetBuild {
            projects = "MyProject.sln"
            configuration = "Release"
        }
        dotnetTest {
            projects = "**/*Tests.csproj"
        }
        dotnetPublish {
            projects = "src/MyApp/MyApp.csproj"
            outputDir = "publish"
        }
    }
})
```

## Configuration

Each step supports common parameters:

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | String | Display name in TeamCity UI |
| `workingDir` | String | Working directory for the step |
| `conditions` | Block | Execution conditions |
| `executionMode` | Enum | `DEFAULT`, `RUN_ON_FAILURE`, `ALWAYS` |

## Requirements

- TeamCity 2021.1+
- Kotlin DSL enabled on your TeamCity server

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-step`)
3. Commit your changes (`git commit -am 'Add new step'`)
4. Push to the branch (`git push origin feature/new-step`)
5. Open a Pull Request

## License

MIT License — see [LICENSE](LICENSE) for details.

# Dotnet Template

A simple and clean dotnet starter template with CI/CD configurations pre-configured.

## Creating a new repo

To create a new repo using this template, create a ticket in Ivanti mentioning this repo. Once the ticket is approved, a new repo will be created using this template.

## Setting Up

**Deploy to JFrog** - The build workflow can be used to deploy to JFrog. Make sure that the repository variable `DEPLOY_TO_JFROG` is set to `true`

**Node Version** - Modify the `DOTNET_VERSION` variable in `.github/workflows/build.yml` to set the dotnet version

**JFrog Repo Name** - Modify the `JFROG_REPO_NAME` variable in `.github/workflows/build.yml` to set the name of the JFrog repo. If not provided, or if it is empty, the default value `<repo-name>-generic-dev-local` will be used

**JFrog Remote Repo Name** - Modify the `JFROG_REMOTE_REPO_NAME` variable in `.github/workflows/build.yml` to set the name of the JFrog remote repo from which the nuget packages should be resolved. 

**Build Name** - By default, the build name will be the one defined in the `package.json` file. Modify the `BUILD_NAME` variable in `.github/workflows/build.yml` to set a custom name

## Build Workflow

- The build workflow triggers automatically when a PR to main is raised or the branch is merged to main.
- The workflow will configure JFrog, runs lint on the project, and builds it
- If the changes are merged and `DEPLOY_TO_JFROG` repo is set to `true`
    - The package will be published to JFrog
    - Then, a release bundle will be created in JFrog and set to the DEV stage

## Promotion Workflow
- The promotion workflow is manually triggered
- The target jfrog stage and release bundle version must be provided
- If the release bundle name is not provided, it uses the current package name
- If the JFrog repo name is not provided, it uses `<repo-name>-generic-<target-env>-local`

## Features

- **Code Formating Validation** - Code Quality and Style Enforcement
- **Dependabot** - Automatic dependency updates
- **JFrog Integration** - Upload builds to JFrog along with evidence

## Project Structure

```
dotnet-template/
├── src/
│   ├── Program.cs                    # Main application entry point
│   ├── Project.csproj                # .NET project file (targeting .NET 8.0)
│   ├── appsettings.json              # Application configuration (production)
│   └── appsettings.Development.json  # Development environment configuration
├── doc/                              # Documentation files
├── script/                           # Build and utility scripts
├── .github/
│   ├── workflows/
│   │   ├── build.yml                 # CI/CD pipeline for building and deploying
│   │   └── jfrog-promotion.yml       # Workflow to promote between JFrog release lifecycles
│   └── dependabot.yml                # Dependabot configuration for dependency updates
├── .editorconfig                     # Code style and formatting rules
├── .gitignore                        # Git ignore patterns
└── README.md                         # This file
```

### Key Files

- **src/Program.cs**: Minimal API setup with ASP.NET Core Web Application
- **src/Project.csproj**: Project configuration targeting .NET 8.0 with nullable reference types enabled
- **src/appsettings.json**: Base application settings including logging configuration
- **src/appsettings.Development.json**: Development-specific overrides for local testing
- **.editorconfig**: Code style enforcement rules for consistent code formatting
- **.github/workflows/build.yml**: Automated build pipeline that:
  - Restores dependencies
  - Validates code formatting
  - Builds and publishes the application
  - Optionally uploads artifacts to JFrog Artifactory

## Configuration

### Code Style

The code style is configured with recommended rules for dotnet projects. Customize the rules in `.editorconfig` to match your coding standards

### Dependabot

Dependabot is configured to check for nuget dependency updates weekly. It will automatically create pull requests for outdated packages.

## CI/CD

This template includes:
- **Dependabot** - Automated dependency updates
- **JFrog Integration** - Ready for artifact publishing (workflow configured separately)
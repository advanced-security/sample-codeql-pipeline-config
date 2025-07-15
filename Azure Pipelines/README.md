# CodeQL Analysis Pipeline

This repository contains sample Azure Pipelines YAML samples that demonstrate different methods of configuring CodeQL static analysis in Azure DevOps pipelines. These files can be used as starting points for setting up code scanning with GitHub Advanced Security.

There is a also an Azure Pipelines [steps template](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/templates) that can be used to run CodeQL analysis in Azure Pipelines from your Pipelines with no or minimal changes.

## Samples

Those samples can be used as a starting point to create your own pipelines.

### Windows

#### [Azure-Pipelines-template-windows.yml](Azure-Pipelines-template-windows.yml)

This sample provides a basic configuration for running CodeQL analysis on a Windows build agent. It:
- Downloads the CodeQL CLI Bundle during the pipeline run
- Creates a CodeQL database for JavaScript code
- Performs the analysis using the `javascript-security-and-quality.qls` query suite
- Uploads the results to GitHub Code Scanning

This template is suitable for simpler codebases where the [autobuilder](https://docs.github.com/en/code-security/code-scanning/creating-an-advanced-setup-for-code-scanning/codeql-code-scanning-for-compiled-languages#about-autobuild-for-codeql) can compile the code correctly.

#### [Azure-Pipelines-template-windows-with-indirect-build-tracing.yml](Azure-Pipelines-template-windows-with-indirect-build-tracing.yml)

This template demonstrates how to run CodeQL analysis using "sandwich mode" (indirect build tracing) on Windows. It:
- Uses PowerShell Core for all script execution
- Downloads the CodeQL CLI Bundle
- Initializes the CodeQL database before the build
- Captures and applies tracing environment variables
- Builds a C# application using VSBuild (as an example)
- Finalizes the database after the build
- Analyzes and uploads the results to GitHub Code Scanning

This approach is ideal for complex build processes or when you need precise control over the build steps while maintaining CodeQL tracing.

### Linux

#### [Azure-Pipelines-template-linux.yml](Azure-Pipelines-template-linux.yml)

This template provides a configuration for running CodeQL analysis on a Linux build agent. It:
- Downloads the CodeQL CLI Bundle
- Creates a CodeQL database for JavaScript code (configurable via variables)
- Uses the autobuilder for compilation
- Performs the analysis with the language-specific security and quality query suite
- Uploads the results to GitHub Code Scanning

This template is suitable for Linux-based builds where the [autobuilder](https://docs.github.com/en/code-security/code-scanning/creating-an-advanced-setup-for-code-scanning/codeql-code-scanning-for-compiled-languages#about-autobuild-for-codeql) can correctly compile the code.

#### [Azure-Pipelines-template-linux-with-indirect-build-tracing.yml](Azure-Pipelines-template-linux-with-indirect-build-tracing.yml)

This template demonstrates how to run CodeQL analysis using "sandwich mode" (indirect build tracing) on Linux. It:
- Downloads the CodeQL CLI Bundle
- Initializes the CodeQL database with begin-tracing
- Correctly propagates the tracing environment variables to the pipeline
- Includes an example Maven build step for Java applications
- Finalizes, analyzes and uploads the results to GitHub Code Scanning

This approach is recommended for complex build processes on Linux or when you need to integrate with existing build steps while maintaining CodeQL tracing.

## Azure Pipelines Templates

The [codeql-steps-template.yml](codeql-steps-template.yml) is a reusable steps template that can be easily included in existing Azure Pipelines. This template:

- Encapsulates all the CodeQL setup, analysis, and upload steps in a single importable file
- Supports both Windows, Linux and MacOS environments
- Configurable via parameters for language, query suite, packs and build commands
- Handles CodeQL CLI download and setup (if agent doesn't have it already)
- Supports both direct database creation and indirect build tracing ("sandwich mode") this is all abstract via the use of build modes.
- Uploads results to GitHub Code Scanning
- Supports push and pull request scans.

The template works on Linux, MacOS and Windows.

To use this template in your existing pipeline, you need to reference (after storing it on a central repository or the code repository for simpler cases).

### Examples

#### Non compiled languages

For a JavaScript application that doesn't require any explicit build steps, this would allow to easily scan your project from your pipeline (showing only the steps):

```yaml
- checkout: self
- template: codeql-steps-template@templates
  parameters:
    language: javascript
    query: security-extended
    buildmode: none
    token: $(GITHUB_TOKEN)
    packs:
      - 'githubsecuritylab/codeql-javascript-queries'
      - 'githubsecuritylab/hotspots-javascript-queries'
```

This example besides using the `security-extended` query suite, it uses two query packs as well (from [CodeQL Community packs](https://github.com/GitHubSecurityLab/CodeQL-Community-Packs)).

#### Compiled Languages

For compiled languages you can use `autobuild` for the `buildMode` or for compiled languages that can be scanned without being built (eg: Java or .Net).

If your code can't be scanned in autobuild or build mode none, you can set the `buildMode` to `manual` and define the build steps in the `manualbuildSteps` parameter.

```yaml
  - checkout: self
  - template: templates/codeql-template.yml
    parameters:
      language: java
      query: security-extended
      token: $(GITHUB_TOKEN)
      buildmode: manual
      manualbuildsteps:
        - task: JavaToolInstaller@1
          inputs:
            versionSpec: '8'
            jdkArchitectureOption: 'x64'
            jdkSourceOption: 'PreInstalled'
          displayName: Setup Java 8
        
        - bash: |
            mvn -B package cobertura:cobertura --file pom.xml -DskipITs --batch-mode --quiet
          displayName: Build with Maven
```

You can read more about build modes in [CodeQL build modes](https://docs.github.com/en/code-security/code-scanning/creating-an-advanced-setup-for-code-scanning/codeql-code-scanning-for-compiled-languages#codeql-build-modes)

## Requirements

These samples/template have the following requirements:
- A GitHub repository with Advanced Security enabled
- A GitHub personal access token (PAT) stored as a secret variable in the pipeline
  - For classic PATs: needs the `security events` scope (in repo)
  - For fine-grained PATs: needs read and write access to "Code scanning alerts"
- You can also use a GitHub App for authentication as an alternative to a PAT

## Configuration

Each sample includes comments that indicate where customization is needed. Look for comment markers like:
- `#   replace this with your actual triggers`
- `#   replace this with your actual build command`

Additionally, the language can be configured by changing the language parameter in the respective CodeQL commands or by modifying the `language` variable in the samples.

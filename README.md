# Sample pipeline files for using CodeQL in popular CI/CD systems

> ℹ️ This is an _unofficial_ project created by Field Security Services, and is not officially supported by GitHub.

This repository shows how to integrate CodeQL into various CI/CD systems, using the CodeQL CLI Bundle for Automated Code Scanning, in example pipeline configuration files.

These are supplementary to the GitHub.com docs on [setting up CodeQL code scanning in your CI system](https://docs.github.com/en/enterprise-cloud@latest/code-security/code-scanning/using-codeql-code-scanning-with-your-existing-ci-system/about-codeql-code-scanning-in-your-ci-system).

The CI/CD systems covered here are Jenkins, Azure Pipelines, CircleCI, TravisCI, AWS CodeBuild and DroneCI.

GitHub Actions is natively supported by GitHub Advanced Security, so use the instructions in the [GitHub.com docs to set up CodeQL for your repository](https://docs.github.com/en/enterprise-cloud@latest/code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/customizing-code-scanning).

For each CI/CD system a template is provided for both Windows and Linux.

There are examples/guidance for:

1. automatic builds for compiled languages using the AutoBuilder (with no `--command` flag)
2. manual builds for compiled languages with a `--command` flag
3. analysis of interpreted languages (which don't need a build)
4. (for Azure and Jenkins) an advanced example using indirect build tracing ("sandwich mode") wrapped around manually specified build commands

> ℹ️ This is an _unofficial_ project created by Field Security Services, and is not officially supported by GitHub.

## Requirements

> ℹ️ You must be using GitHub Advanced Security to use these pipeline files. If you are not using GitHub Advanced Security, please see the [GitHub Advanced Security website](https://github.com/features/security) for more information.

1. A CI/CD pipeline using one of:
    * AWS CodeBuild
    * Azure Pipelines
    * CircleCI
    * DroneCI
    * Jenkins
    * TravisCI
2. The [CodeQL Bundle](https://github.com/github/codeql-action/releases) installed in the CI/CD pipeline
3. [GitHub PAT to push results back to GitHub Advanced Security](https://docs.github.com/en/code-security/code-scanning/using-codeql-code-scanning-with-your-existing-ci-system/configuring-codeql-cli-in-your-ci-system#uploading-results-to-github)

## Usage

1. [Download and install the CodeQL Bundle in your CI system](https://docs.github.com/en/enterprise-cloud@latest/code-security/code-scanning/using-codeql-code-scanning-with-your-existing-ci-system/installing-codeql-cli-in-your-ci-system), testing that it works
2. Copy the relevant pipeline file from this repository into your repository
3. [Update the pipeline file with your required settings](https://docs.github.com/en/enterprise-cloud@latest/code-security/code-scanning/using-codeql-code-scanning-with-your-existing-ci-system/configuring-codeql-cli-in-your-ci-system)
    * read the [creating CodeQL database documentation for help](https://codeql.github.com/docs/codeql-cli/manual/database-create/)
    * the [full CodeQL CLI documenation](https://docs.github.com/en/enterprise-cloud@latest/code-security/codeql-cli/using-the-codeql-cli/about-the-codeql-cli) may also be useful

## License

This project is licensed under the terms of the MIT open source license. Please refer to the [LICENSE](LICENSE) for the full terms.

## Maintainers

See [CODEOWNERS](CODEOWNERS) for the list of maintainers.

## Support

See the [SUPPORT](SUPPORT.md) file.

## Background

See the [CHANGELOG](CHANGELOG.md), [CONTRIBUTING](CONTRIBUTING.md), [SECURITY](SECURITY.md), [SUPPORT](SUPPORT.md), [CODE OF CONDUCT](CODE_OF_CONDUCT.md) and [PRIVACY](PRIVACY.md) files for more information.

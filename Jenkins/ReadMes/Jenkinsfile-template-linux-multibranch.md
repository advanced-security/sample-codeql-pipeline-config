# Jenkins Pipeline for Security Analysis

This Jenkins pipeline script is designed to perform security analysis on a Java project using CodeQL and submit a dependency snapshot. The script is written in Groovy and is designed to be used with Jenkins' Pipeline plugin.

## Functions

The script defines several helper functions:

- `isPRBuild()`: Checks if the current build is for a pull request.
- `getPRNumber()`: Extracts the pull request number from the branch name.
- `getPRRef()`: Returns the Git reference for the pull request or branch being built.

## Environment Variables

The script sets several environment variables:

- `GITHUB_CREDS`: The Jenkins Credentials ID for your GitHub PAT credential.
- `DEFAULT_BRANCH`: The default branch name of the repository.
- `GITHUB_PR_REF_TYPE`: The type of ref that will be checked out for a job initiated by a GitHub PR.
- `GITHUB_REPO`: The name of the GitHub repository to run the analysis on.
- `CODEQL_LANGUAGE`: The programming language of the project.
- `CODEQL_BUILD_COMMAND`: The command to build the project.
- `CODEQL_QUERY_SUITE`: The CodeQL query suite to use for the analysis.
- `DEPENDENCY_SUBMISSION_EXECUTABLES_URL`: The URL to download the Dependency Submission Action executable archive.
- `DEPENDENCY_SUBMISSION_EXECUTABLE`: The Dependency Submission Action executable.
- `PR_REF`: The Git reference for the pull request or branch being built.

## Stages

The pipeline consists of a single stage, "Run security analysis", which contains two parallel stages:

- "Run CodeQL analysis": This stage creates a CodeQL database for the project, analyzes the database, and uploads the results to GitHub.
- "Submit dependency snapshot": This stage downloads the Dependency Submission Action executable, makes it executable, and runs it to submit a dependency snapshot.

## When to Run

The stages are configured to run under certain conditions:

- The "Run CodeQL analysis" stage runs if the current branch is the default branch or if the change was not made by Dependabot.
- The "Submit dependency snapshot" stage runs if the current branch is the default branch or if there is a change ID.

## Steps

Each stage consists of several steps, which are shell commands to be executed in the Jenkins environment. These commands perform the actual work of the stage, such as running the CodeQL analysis or submitting the dependency snapshot.

### Mermaid Diagram 

```mermaid
graph TD
    A[Start]
    B{isPRBuild}
    C[getPRNumber]
    D[getPRRef]
    E[Environment Setup]
    F{Run security analysis}
    G{Run CodeQL analysis}
    H{Submit dependency snapshot}
    I[End]
    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    F --> H
    G --> I
    H --> I
  ```

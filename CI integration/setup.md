# CI Integration for SonarQube Analysis  

---

## Objective

We have already set up SonarQube locally, but this solution is not scalable or sustainable for long-term use. As part of the current quarter's roadmap, we are moving this local setup into CI pipelines. This integration allows us to:

- Generate live and automatic code analysis during PRs or merges  
- Eliminate manual local dependencies  
- Improve visibility and enforce quality gates  
- Provide centralized access and consistent results across the team

---

## Required Knowledge & Tools

To fully understand, run, or troubleshoot this setup, the following knowledge or tools are useful:

| Topic | Description | Learn More |
|-------|-------------|------------|
| **SonarQube** | Code quality tool that scans code for bugs, vulnerabilities, and code smells | [Official Docs](https://docs.sonarsource.com/sonarqube/latest/) |
| **GitHub Actions** | CI/CD automation system used to run code workflows | [GitHub Actions Docs](https://docs.github.com/en/actions) |
| **sonar-scanner** | CLI tool used to send code analysis data to SonarQube | [SonarScanner CLI](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/sonarscanner/) |
| **npm scripts** | You may need to run `npm run test:coverage` to generate coverage reports | Check your project's `package.json` |
| **GitHub Actions Workflows**               | A series of steps defined in a `.yml` file to automate processes like testing, linting, or SonarQube scanning.                     | [About Workflows](https://docs.github.com/en/actions/using-workflows/about-workflows)                                                       |
| **GitHub Actions Jobs**                    | Each workflow is made up of one or more jobs that run in parallel or sequentially depending on dependencies.                       | [Jobs in GitHub Actions](https://docs.github.com/en/actions/using-jobs/using-jobs-in-a-workflow)                                            |
| **GitHub Actions Steps**                   | The smallest unit of execution in a job, steps execute commands or actions (reusable logic).                                       | [Steps](https://docs.github.com/en/actions/using-jobs/using-jobs-in-a-workflow#about-steps)                                                 |
| **Branch Protection Rules**                | GitHub settings that restrict actions on certain branches (like `main`) and can enforce passing status checks (like SonarQube CI). | [Branch Rules Docs](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches) |
| **SonarScanner CLI**                       | A command-line tool that scans code and sends analysis to the SonarQube server. Required for both local and CI analysis.           | [SonarScanner CLI](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/sonarscanner/)                              |
| **Environment Variables / GitHub Secrets** | Secure way to store tokens and configuration like `SONAR_TOKEN` and `SONAR_HOST_URL` in your GitHub project.                       | [GitHub Secrets Docs](https://docs.github.com/en/actions/security-guides/encrypted-secrets)                                                 |
| **CI/CD Pipeline Concepts**                | Understanding the flow from code changes to deployment via automated tools like GitHub Actions.                                    | [CI/CD Concepts](https://www.redhat.com/en/topics/devops/what-is-ci-cd)                                                                     |

---
## Prerequisites

Before proceeding, please ensure the following:

- ✅ **Local Setup is Working**: SonarQube must already be running locally and analyzing code successfully.  
- ✅ **`sonar-project.properties` is Ready**: All required SonarQube properties (like `sonar.projectKey`, `sonar.host.url`, `sonar.token`, etc.) are defined in a `sonar-project.properties` file.  
- ✅ **(Optional) Test Coverage is Enabled**: If you have written unit tests, ensure coverage reports are being generated, and the correct file paths are added in the properties file using keys like `sonar.javascript.lcov.reportPaths`.

---

## Step-by-Step CI Setup

### 1. Configure SonarQube Environment Variables

- In your GitHub repository, go to **Settings > Secrets and Variables > Actions**.  
- Add the following environment secrets:
  - `SONAR_TOKEN`: Your SonarQube project token.
  - `SONAR_HOST_URL`: If using a self-hosted SonarQube instance.

---

### 2. Create GitHub Action for SonarQube CI

- In your GitHub repo, go to the **Actions** tab.
- Create a new workflow YAML file (e.g., `.github/workflows/sonar.yml`).
- Define your trigger (on `pull_request`, `push`, or specific branches).

#### Example Workflow:

```yaml

```

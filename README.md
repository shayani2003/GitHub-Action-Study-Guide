
# GitHub Actions Study Notes

## 1. Introduction to GitHub Actions

GitHub Actions is a CI/CD (Continuous Integration and Continuous Deployment) tool that automates workflows directly within a GitHub repository. It enables developers to automate software development processes such as testing, building, and deploying applications. GitHub Actions leverages YAML configuration files to define workflows and runs jobs in different environments.

### Key Features:
- Native integration with GitHub repositories.
- Supports automation of various tasks like CI/CD, issue management, and notifications.
- Provides GitHub-hosted and self-hosted runners for executing workflows.
- Offers marketplace actions for reusability.

---

## 2. Key Concepts

### 2.1 Workflows
A **workflow** is an automated process defined in a YAML file stored in `.github/workflows/`. It consists of multiple jobs that execute when triggered by an event.

Example:
```yaml
name: CI Workflow
on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'
      
      - name: Install dependencies
        run: npm install
      
      - name: Run tests
        run: npm test
```

### 2.2 Events
**Events** are triggers that initiate a workflow. Some common events include:
- `push`: Runs when code is pushed to the repository.
- `pull_request`: Runs when a pull request is opened or updated.
- `schedule`: Runs at a scheduled time using cron syntax.
- `workflow_dispatch`: Runs manually when triggered by a user.

Example of scheduling a workflow:
```yaml
on:
  schedule:
    - cron: '0 0 * * *'  # Runs daily at midnight
```

### 2.3 Jobs
A **job** is a set of steps that execute on a specified runner. Jobs can run independently or sequentially.

Example:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building project"
```

### 2.4 Steps
**Steps** define individual tasks in a job, such as running commands or using actions.

Example:
```yaml
steps:
  - name: Install dependencies
    run: npm install
```

### 2.5 Actions
**Actions** are reusable components that simplify workflows. GitHub provides a marketplace for actions.

Example:
```yaml
uses: actions/checkout@v4
```

### 2.6 Runners
A **runner** is the environment where a job executes. GitHub provides:
- **GitHub-hosted runners**: Pre-configured environments like `ubuntu-latest`, `windows-latest`.
- **Self-hosted runners**: Custom machines configured by users.

Example:
```yaml
runs-on: ubuntu-latest
```

### 2.7 Artifacts and Caching
Artifacts store workflow outputs, while caching speeds up builds.

Example of caching dependencies:
```yaml
uses: actions/cache@v3
with:
  path: ~/.npm
  key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
```

---

## 3. Importance of Code in GitHub Actions

Code is essential in GitHub Actions as it defines the workflows and automates repetitive tasks. The automation achieved through GitHub Actions ensures efficiency, reliability, and consistency in software development.

### Why is Code Used in GitHub Actions?
1. **Automation**: Reduces manual intervention by automating repetitive tasks such as testing, deployment, and code formatting.
2. **Consistency**: Ensures that workflows execute the same way across different environments.
3. **Scalability**: Supports large-scale projects by streamlining development processes.
4. **Error Reduction**: Minimizes human errors by executing predefined scripts.
5. **Efficiency**: Saves time and effort by running scripts automatically whenever an event occurs.

### What Happens If Code is Not Used?
If code is not used in GitHub Actions, several problems arise:
1. **Manual Workload**: Developers must manually run tests, build projects, and deploy applications.
2. **Inconsistency**: Different team members may perform tasks differently, leading to unpredictable results.
3. **Increased Errors**: Manual execution increases the chances of mistakes, such as incorrect configurations or missing steps.
4. **Lack of Continuous Integration/Deployment**: Without automated workflows, integrating and deploying changes becomes time-consuming and error-prone.
5. **Inefficient Collaboration**: Teams working on different parts of a project may struggle to maintain synchronization without automated checks and deployments.

By using code in GitHub Actions, teams can optimize software development and deployment processes, improving productivity and software quality.

---

## 4. Questions and Answers

### **4.1 Short Answer Questions**

#### **1. What is GitHub Actions?**
**Ans:** GitHub Actions is an automation tool for CI/CD that helps in building, testing, and deploying code within a GitHub repository.

#### **2. What are workflows in GitHub Actions?**
**Ans:** Workflows are YAML-defined automation processes that run in response to events like push, pull requests, and scheduled triggers.

#### **3. How do you define a workflow trigger?**
**Ans:** By using the `on` key in a YAML file:
```yaml
on: push
```

#### **4. What is a job in GitHub Actions?**
**Ans:** A job is a collection of steps that execute commands within a workflow.

#### **5. What are steps in GitHub Actions?**
**Ans:** Steps are individual tasks within a job that run shell commands or actions.

#### **6. What is a runner?**
**Ans:** A runner is the environment where workflows execute. It can be GitHub-hosted or self-hosted.

#### **7. How can you reuse actions?**
**Ans:** By using the `uses` keyword:
```yaml
uses: actions/checkout@v4
```

#### **8. How do you schedule workflows?**
**Ans:** Using the `schedule` event with cron syntax:
```yaml
on:
  schedule:
    - cron: '0 0 * * *'  # Runs daily at midnight
```

#### **9. How do you pass environment variables in a job?**
**Ans:** Using the `env` key:
```yaml
env:
  NODE_ENV: production
```

#### **10. How do you cache dependencies?**
**Ans:** Using the `actions/cache` action:
```yaml
uses: actions/cache@v3
with:
  path: ~/.npm
  key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
```

---

## 5. Best Practices
- Keep workflows modular and reusable.
- Use caching to speed up builds.
- Secure secrets using GitHub Secrets.
- Use `matrix` strategy for testing multiple environments.

---
### **Multiple Choice Questions**

**Question 1:** GitHub-hosted runners support which capabilities? (Choose two)

**Options:**

**(A)** support for a variety of Linux variations including CentOS, Fedora, and Debian

**(B)** requiring a payment mechanism (e.g., credit card) to use for private repositories

**(C)** automatic patching of both the runner and the underlying OS

**(D)** automatic file-system caching between workflow runs

**(E)** support for Linux, Windows, and macOS

**Correct Answers:**

**(C)** Automatic patching of both the runner and the underlying OS

**(E)** Support for Linux, Windows, and macOS

**Explanation:**

GitHub-hosted runners are automatically updated and patched by GitHub. They support major operating systems, including Linux, Windows, and macOS. However, they do not provide a variety of Linux distributions like CentOS, Fedora, and Debian.

---



**Question 2:** Which workflow event is used to manually trigger a workflow run?

**Options:**

**(A)** status

**(B)** create

**(C)** workflow_run

**(D)** workflow_dispatch

**Correct Answer:**

**(D)** workflow_dispatch

**Explanation:**

The workflow_dispatch event allows users to manually trigger workflows from the GitHub Actions UI, API, or CLI.

---



**Question 3:** In which of the following scenarios should you use self-hosted runners? (Choose two)

**Options:**

**(A)** when a workflow job needs to install software from the local network

**(B)** when you want to use macOS runners

**(C)** when GitHub Actions minutes must be used for the workflow runs

**(D)** when jobs must run for longer than 6 hours

**(E)** when the workflow jobs must be run on Windows 10

**Correct Answers:**

**(A)** when a workflow job needs to install software from the local network

**(D)** when jobs must run for longer than 6 hours

**Explanation:**

Self-hosted runners provide better control over the execution environment, allowing installation of software from a local network. They also allow jobs to run for more than the 6-hour limit imposed on GitHub-hosted runners.

---



**Question 4:** A workflow that had been working now stalls in a waiting state until failing. The workflow file process_yaml-jobs specifying runs-on: [gpu] 1. Which of the following steps would troubleshoot the issue? (Choose two)

**Options:**

**(A)** Increase the usage limits for the GitHub-hosted runners.

**(B)** Review the contents of the Runner *.log files in the _diag folder.

**(C)** Update the org settings to enable GPU-based GitHub-hosted runners.

**(D)** Check the "Set up job" step for the logs of the last successful run to determine the runner.

**(E)** Rotate the GITHUB_TOKEN secret for the appropriate runners.

**Correct Answers:**

**(B)** Review the contents of the Runner *.log files in the _diag folder.

**(D)** Check the "Set up job" step for the logs of the last successful run to determine the runner.

**Explanation:**

Checking logs helps diagnose runner-related issues, and the "Set up job" step provides information about the last successful run, which can indicate potential issues.

---



**Question 5:** Scheduled workflows run on the:

**Options:**

**(A)** latest commit from the branch named main.

**(B)** latest commit on the default or base branch.

**(C)** latest commit and branch on which the workflow was triggered.

**(D)** latest commit from the branch named schedule.

**(E)** specified commit and branch from the workflow YAML file.

**Correct Answer:**

**(B)** latest commit on the default or base branch.

Explanation:
Scheduled workflows run on the latest commit of the default or base branch unless otherwise specified.
---



**Question 6:** You are a DevOps engineer working on a custom action. You want to conditionally run a script at the start of the action, before the main entrypoint. Which code block should be used to define the metadata file for your custom action?

**Options:**

**(A)**
```yaml
  runs:
    using: 'node16'
    pre: 'start.js'
    pre-if: github.event_name == 'push'
    main: 'index.js'
  ```

**(B)**
```yaml
  runs:
    using: 'node16'
    start: 'start.js'
    start-if: github.event_name == 'push'
    main: 'index.js'
  ```

**Correct Answer:**

**(A)**
 ```yaml
  runs:
    using: 'node16'
    pre: 'start.js'
    pre-if: github.event_name == 'push'
    main: 'index.js'
  ```

**Explanation:**

The pre property defines a script to be run before the main entry point, and pre-if ensures it runs conditionally.

---



**Question 7:** How should you print a debug message in your workflow?

**Options:**

**(A)** `echo "Set variable MyVariable to true" >> $DEBUG_MESSAGE`

**(B)** `echo "::debug::Set variable myVariable to true"`

**(C)** `echo "debug_message=Set variable myVariable to true" >> $GITHUB_OUTPUT`

**(D)** `echo "::add-mask::Set variable myVariable to true"`

**Correct Answer:**

**(B)** `echo "::debug::Set variable myVariable to true"`

**Explanation:**

In GitHub Actions, debug messages are logged using echo "::debug::message".

---



**Question 8:** Which choices represent best practices for publishing actions so that they can be consumed reliably? (Choose two)

**Options:**

(A) repo name

(B) organization name

(C) tag

(D) default branch

(E) commit SHA

**Correct Answers:**

**(C)** tag

**(E)** commit SHA

**Explanation:**

Using tags and commit SHAs ensures that consumers get a stable version of the action. The default branch is not a reliable reference since it can change over time.





---

**Question 9:** What is the simplest action type to run a shell script?

**Options:**

**(A)** Docker container action

**(B)** Composite action

**(C)** Bash script action

**(D)** JavaScript action

**Correct Answer:**

**(C)** Bash script action

**Explanation:**

A Bash script action allows execution of shell commands directly without needing a Docker container or JavaScript.

---



**Question 10:** Your organization needs to simplify reusing and maintaining automation in your GitHub Enterprise Cloud across all repositories. (Choose three)

**Options:**

**(A)** self-hosted runners

**(B)** actions stored in an organizational partition in the GitHub Marketplace

**(C)** custom Docker actions stored in GitHub Container Registry

**(D)** encrypted secrets

**(E)** actions stored in private repositories in the organization

**(F)** workflow templates

**Correct Answers:**
**(E)** Actions stored in private repositories in the organization

**(F)** Workflow templates

**(C)** Custom Docker actions stored in GitHub Container Registry

**Explanation:**

Private repositories, workflow templates, and containerized custom actions help standardize automation across multiple repositories.

---



**Question 11:** As a developer, which of the following snippets will enable you to run the commands `npm ci` and `npm run build` as part of a workflow?

**Options:**

**(A)**
  `run:`
  `  npm ci`
  `  npm run build`

**(B)**
  `shell:`
  `  npm ci`
  `  npm run build`

**Correct Answer:**

**(A)**
  `run:`
  `  npm ci`
  `  npm run build`


**Explanation:**

The run keyword allows multiple commands to be executed sequentially using |.
---



**Question 12:** As a developer, you need to leverage Redis in your workflow. What is the best way to use Redis on a self-hosted runner without affecting future workflow runs?

**Options:**

**(A)** Specify `container:` and `services:` in your job definition to leverage a Redis service container.

**(B)** Add a run step to your workflow, which dynamically installs and configures Redis as part of your job.

**(C)** Set up Redis on a separate machine and reference that instance from your job.

**(D)** Install Redis on the hosted runner image and place it in a runner group. Specify `label:` in your job to target the runner group.

**Correct Answer:**

**(A)** Specify `container:` and `services:` in your job definition to leverage a Redis service container.

**Explanation:**

Using a service container ensures Redis runs in isolation and does not interfere with future workflow runs. Other options like installing Redis dynamically can lead to conflicts.
---



**Question 13:** As a DevOps engineer, you need to execute a deployment to different environments like development and testing based on the labels added to a pull request. The deployment should use the `releases` branch and trigger only when there is a change in the files under `apps` folder. Which code block should be used to define the deployment workflow trigger?

**Options:**

**(A)**
  ```yaml
  on:
    pull_request:
      types: [labeled]
      branches:
        - 'releases'
      paths:
        - 'apps/**'
  ```

**(B)**
  ```yaml
  on:
    pull_request:
      types: [labeled]
      branches:
        - 'releases'
  ```

**Correct Answer:**

**(A)**
 ```yaml
  on:
    pull_request:
      types: [labeled]
      branches:
        - 'releases'
      paths:
        - 'apps/**'
  ```

**Explanation:**

This configuration ensures the workflow triggers only when a label is added, the branch is releases, and there is a change under the apps/ folder.
---



**Question 14:** In which locations can actions be referenced by workflows? (Choose three)

**Options:**

**(A)** the same repository as the workflow

**(B)** an action extension file in the repository

**(C)** the `runs-on:` keyword of a workflow file

**(D)** the repository's Secrets settings page

**(E)** a published Docker container image on Docker Hub

**(F)** a public NPM registry

**(G)** a separate public repository

**Correct Answers:**

**(A)** The same repository as the workflow

**(E)** A published Docker container image on Docker Hub

**(G)** A separate public repository

**Explanation:**

Actions can be referenced from the same repository, a public repository, or a Docker image on Docker Hub.



---



**Question 15:** What are the two ways to pass data between jobs? (Choose two)

**Options:**

**(A)** Use the copy action with restore parameter to restore the data from the cache.

**(B)** Use artifact storage.

**(C)** Use job outputs.

**(D)** Use data storage.

**(E)** Use the copy action to save the data that should be passed in the artifacts folder.

**(F)** Use the copy action with cache parameter to cache the data.

**Correct Answers:**

**(B)** Use artifact storage.

**(C)** Use job outputs.

**Explanation:**

Artifacts store data across jobs, while job outputs allow passing structured data between jobs.



---



**Question 16:** In which scenarios could the GITHUB_TOKEN be used? (Choose two)

**Options:**

**(A)** to leverage a self-hosted runner

**(B)** to publish to GitHub Packages

**(C)** to create a repository secret

**(D)** to add a member to an organization

**(E)** to create issues in the repo

**(F)** to read from the file system on the runner

**Correct Answers:**

**(B)** To publish to GitHub Packages

**(E)** To create issues in the repo

**Explanation:**

GITHUB_TOKEN provides authentication to publish packages and interact with the repo, but it cannot be used for organization membership or file system access.



---



**Question 17:** You need to trigger a workflow using the GitHub API for activity that happens outside of GitHub.

**Options:**

**(A)** workflow_run

**(B)** repository_dispatch

**(C)** check_suite

**(D)** deployment

**Correct Answer:**

**(B)** repository_dispatch


**Explanation:**

repository_dispatch triggers workflows from external events outside GitHub.



---



**Question 18:** While writing a custom action, some behavior within the runner must be changed. Which commands can be used to influence the runner's output? (Choose two)

**Options:**

**(A)** `echo "::error file=main.py,line=10,col=15::There was an error"`

**(B)** `echo "::error=There was an error::"`

**(C)** `echo "::error::There was an error"`

**(D)** `echo "::error message=There was an error::"`

**Correct Answers:**

**(A)** `echo "::error file=main.py,line=10,col=15::There was an error"`

**(C)** `echo "::error::There was an error"`


**Explanation:**

These formats allow GitHub Actions to recognize and format errors properly.

---



**Question 19:** You need to make a script to retrieve workflow run logs via the API. Which is the correct API to download a workflow run's logs?

**Options:**

**(A)** `GET /repos/:owner/:repo/actions/runs/:run_id/logs`

**(B)** `GET /repos/:owner/:repo/actions/artifacts/logs`

**(C)** `POST /repos/:owner/:repo/actions/runs/:run_id/logs`

**(D)** `POST /repos/:owner/:repo/actions/runs/:run_id`

**Correct Answer:**

**(A)** `GET /repos/:owner/:repo/actions/runs/:run_id/logs`

**Explanation:**

This API endpoint retrieves workflow logs.

---



**Question 20:** What is the most suitable action type for a custom action written in TypeScript?

**Options:**

**(A)** Docker container action

**(B)** Composite run step

**(C)** Bash script action

**(D)** JavaScript action

**Correct Answer:**

**(D)** JavaScript action

**Explanation:**

GitHub Actions natively supports JavaScript/TypeScript actions without requiring a container.

---



**Question 21:** As a DevOps engineer, you need to define a deployment workflow that runs after the build workflow has completed. Without modifying the build workflow, which trigger should you define in the deployment workflow?

**Options:**

**(A)** workflow_dispatch

**(B)** workflow_run

**(C)** workflow_exec

**(D)** repository_dispatch

**Correct Answer:**

**(B)** workflow_run

**Explanation:**

workflow_run triggers a workflow after another workflow (like build) has completed.

---



**Question 22:** Custom environment variables can be defined at multiple levels within a workflow file including: (Choose three)

**Options:**

**(A)** runner level.

**(B)** step level.

**(C)** top level.

**(D)** job level.

**(E)** stage level.

**(F)** default level.

**Correct Answers:**

**(B)** step level.

**(C)** top level.

**(D)** job level.

**Explanation:**

Environment variables can be defined at multiple levels, including step, job, and workflow top level.

---



**Question 23:** As a developer, you are designing a workflow and need to communicate with the runner machine to set environment variables, add debug messages to the output logs, and other tasks. Which of the following options should be used?

**Options:**

**(A)** self-hosted runners

**(B)** composite run step

**(C)** environment variables

**(D)** enable debug logging

**(E)** workflow commands

**Correct Answer:**

**(E)** workflow commands

**Explanation:**

Workflow commands interact with the runner to set environment variables and debug messages.

---



**Question 24:** Which of the following statements are true regarding the use of GitHub Actions on a GitHub Enterprise Server? (Choose three)

**Options:**

**(A)** Actions must be defined in the .github repository.

**(B)** Actions created by GitHub are automatically available and cannot be disabled.

**(C)** Third-party actions can be used on GitHub Enterprise Server by configuring GitHub Connect.

**(D)** Use of GitHub Actions on GitHub Enterprise Server requires a persistent internet connection.

**(E)** Third-party actions can be manually synchronized for use on GitHub Enterprise Server.

**(F)** Most GitHub-authored actions are automatically bundled for use on GitHub Enterprise Server.

**Correct Answers:**

**(E)** Third-party actions can be manually synchronized for use on GitHub Enterprise Server.

**(C)** Third-party actions can be used on GitHub Enterprise Server by configuring GitHub Connect.

**(F)** Most GitHub-authored actions are automatically bundled for use on GitHub Enterprise Server.

**Explanation:**

GitHub Enterprise Server allows third-party actions through synchronization or GitHub Connect.



---



**Question 25:** As a developer, you are optimizing a GitHub workflow that uses and produces many different files. You want to improve performance and reduce workflow run times versus workflow artifacts. Which two statements are true? (Choose two)

**Options:**

**(A)** Use artifacts to access the GitHub Package Registry and download a package for a workflow.

**(B)** Use artifacts when referencing files produced by a job after a workflow has ended.

**(C)** Use caching when reusing files that change rarely between jobs or workflow runs.

**(D)** Use caching to store cache entries for up to 30 days between accesses.

**Correct Answers:**

**(B)** Use artifacts when referencing files produced by a job after a workflow has ended.

**(C)** Use caching when reusing files that change rarely between jobs or workflow runs.

**Explanation:**

Artifacts are useful for persistent storage, while caching improves performance for rarely changing files.

---



**Question 26:** Which default GitHub environment variable indicates the name of the person or app that initiated a workflow run?

**Options:**

**(A)** GITHUB_ACTOR

**(B)** GITHUB_USER

**(C)** ENV_ACTOR

**(D)** GITHUB_WORKFLOW_ACTOR

**Correct Answer:**

**(A)** GITHUB_ACTOR

**Explanation:**

GITHUB_ACTOR stores the name of the user or app that triggered the workflow.

---



**Question 27:** As a developer, one of your workflows will require Xcode version 11.2 on a hosted macOS Catalina (i.e., v10.15) runner. You've already created and configured a self-hosted runner to conform to those requirements and registered it with your organization. What else should you do to ensure that the workflow accesses the correct runner instance? (Choose three)

**Options:**

**(A)** Assign the custom labels to the self-hosted runner.

**(B)** In the workflow, specify:
  ```yaml
  runs-on: [ self-hosted, macos-10.15, xcode-11.2 ]
  ```
**(C)** Add your runner to the appropriate runner groups.

**(D)** Create custom runner labels for macos-10.15 and xcode-11.2.

**(E)** In the workflow, specify:
  ```yaml
  runs-on: [ ${{groups.macos-10.15}}, ${{groups.xcode-11.2}} ]
  ```

**(F)** Create runner groups named `macos-10.15` and `xcode-11.2`.

**Correct Answers:**

**(A)** Assign the custom labels to the self-hosted runner.

**(E)** In the workflow, specify:
  ```yaml
  runs-on: [ ${{groups.macos-10.15}}, ${{groups.xcode-11.2}} ]
  ```

**(C)** Add your runner to the appropriate runner groups.

**Explanation:**

-Assigning custom labels allows you to differentiate the self-hosted runner from others.
-Specifying runs-on correctly ensures the workflow picks the right runner.
-Adding the runner to the appropriate groups helps in managing access control and usage.

---



**Question 28:** You are a DevOps engineer working on deployment workflows. You need to execute the `deploy` job only if the current branch name is `feature-branch`. Which code snippet will help you to implement the conditional execution of the job?

**Options:**

 **(A)**
  ```yaml
  jobs:
    deploy:
      if: github.ref_name == 'feature-branch'
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
  ```

**(B)**
  ```yaml
  jobs:
    deploy:
      if: github.branch_name == 'feature-branch'
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
  ```

**Correct Answer:**

**(A)**
  ```yaml
  jobs:
    deploy:
      if: github.ref_name == 'refs/heads/feature-branch'
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
  ```

**Explanation:**

-The correct GitHub Actions syntax uses github.ref_name to check the branch name.
-github.branch_name is not a valid GitHub Actions context variable.

---



**Question 29:** Which of the following is a valid reusable workflow reference?

**Options:**

**(A)** `uses: another-repo/.github/workflows/workflow.yml@v1`

**(B)** `uses: octo-org/another-repo/workflow.yml@v1`

**(C)** `uses: another-repo/workflow.yml@v1`

**(D)** `uses: octo-org/another-repo/.github/workflows/workflow.yml`

**Correct Answer:**

**(B)** `uses: octo-org/another-repo/workflow.yml@v1`

**Explanation:**

-A reusable workflow must be stored under .github/workflows/ inside the referenced repository.
-The correct format is:
 `uses: owner/repository/.github/workflows/workflow.yml@version`
-Other options reference workflows incorrectly.

---



**Question 30:** What are two reasons to keep an action in its own repository instead of bundling it with other application code? (Choose two)

**Options:**

**(A)** It widens the scope of the code base for developers fixing issues and extending the action.

**(B)** It decouples the action's versioning from the versioning of other application code.

**(C)** It allows sharing workflow secrets with other users.

**(D)** It makes the `action.yml` file optional.

**(E)** It makes it easier for the GitHub community to discover the action.

**Correct Answers:**

**(B)** It decouples the action's versioning from the versioning of other application code.

**(E)** It makes it easier for the GitHub community to discover the action.

**Explanation:**

-Keeping an action separate allows independent version control.
-Storing actions in their own repositories improves discoverability and reusability.

---



**Question 31:** Which command can you include in your workflow file to set the output parameter for an action?

**Options:**

**(A)** `echo "::add-mask::ACTION_COLOR"`

**(B)** `echo "action_color=purple" >> $GITHUB_OUTPUT`

**(C)** `echo "::debug::action_color=purple"`

**(D)** `echo "action_color=purple" >> $GITHUB_ENV`

**Correct Answer:**

**(B)** `echo "action_color=purple" >> $GITHUB_OUTPUT`

**Explanation:**

-$GITHUB_OUTPUT is used to set outputs for subsequent steps in GitHub Actions.
-Other options modify environment variables or mask secrets but don’t set outputs.

---



**Question 32:** As a DevOps engineer, you have configured an IP allow list on a GitHub organization. Which effects does the IP allow list have on GitHub Actions? (Choose two)

**Options:**

**(A)** You must allow GitHub Actions's IP address ranges in order to use marketplace actions.

**(B)** You can use standard GitHub-hosted runners since their IP addresses are automatically allowed.

**(C)** You can use GitHub-hosted larger runners since they can be configured with static IP addresses.

**(D)** You can use self-hosted runners with known IP addresses.

**Correct Answers:**

**(A)** You must allow GitHub Actions's IP address ranges in order to use marketplace actions.

**(D)** You can use self-hosted runners with known IP addresses.

**Explanation:**

-GitHub-hosted runners use dynamic IPs, so if an allow list is enforced, you must allow GitHub’s published IP ranges.
-Self-hosted runners work since their IPs are known and controlled.

---



**Question 33:** You installed specific software on a Linux self-hosted runner. You have users with workflows that need to be able to use this identified custom software. Which steps should you perform to prepare the runner and your users to run these workflows? (Choose two)

**Options:**

**(A)** Inform users to identify the runner based on the group.

**(B)** Create the group `custom-software-on-linux` and move the runner into the group.

**(C)** Inform users to identify the runner with the labels `custom-software` and `linux`.

**(D)** Configure the webhook and network to enable GitHub to trigger workflows.

**(E)** Add the label `custom-software` to the runner.

**(F)** Add the label `linux` to the runner.

**Correct Answers:**

**(C)** Inform users to identify the runner with the labels `custom-software` and `linux`.

**(E)** Add the label `custom-software` to the runner.

**Explanation:**

-Adding labels allows users to select specific runners in workflows.
-Labels like custom-software and linux help organize runners.

---



**Question 34:** What is the proper syntax to reference the system-provided run number variable?

**Options:**

**(A)** `$GITHUB_RUN_NUMBER`

**(B)** `${{ GITHUB_RUN_NUMBER }}`

**(C)** `$github.run_number`

**(D)** `${{ env.GITHUB_RUN_NUMBER }}`

**(E)** `${{ var.GITHUB_RUN_NUMBER }}`

**Correct Answer:**

**(B)** `${{ GITHUB_RUN_NUMBER }}`

**Explanation:**

-GitHub Actions stores system variables in GITHUB_* context.
-GITHUB_RUN_NUMBER provides a unique run number for each workflow execution.

---



**Question 35:** What are the mandatory requirements for publishing GitHub Actions to the GitHub Marketplace? (Choose two)

**Options:**

**(A)** Each repository can contain a collection of actions as long as they are under the same Marketplace category.

**(B)** The action's name cannot match a user or organization on GitHub unless the user or organization owns the action.

**(C)** The action's metadata file must be in the root directory of the repository.

**(D)** The action can be either in a public or private repository.

**(E)** The name should match with one of the existing GitHub Marketplace categories.

**Correct Answers:**

**(C)** The action's metadata file must be in the root directory of the repository.

**(B)** The action's name cannot match a user or organization on GitHub unless the user or organization owns the action.

**Explanation:**

-The action.yml file must be in the root directory.
-GitHub prevents name conflicts by requiring matching ownership.

---



**Question 36:** Which default environment variable specifies the branch or tag that triggered a workflow?

**Options:**

**(A)** `ENV_BRANCH`

**(B)** `GITHUB_BRANCH`

**(C)** `GITHUB_TAG`

**(D)** `GITHUB_REF`

**Correct Answer:**

**(D)** `GITHUB_REF`

**Explanation:**

-GITHUB_REF stores the branch name or tag that triggered the workflow.
-Other variables like GITHUB_BRANCH don’t exist.

---



**Question 37:** As a developer, you need to make sure that only actions from trusted sources are available for use in your GitHub Enterprise Cloud organization. (Choose three)

**Options:**

**(A)** Actions can be published to an internal marketplace.

**(B)** Actions created by GitHub are automatically enabled and cannot be disabled.

**(C)** GitHub-verified actions can be collectively enabled for use in the enterprise.

**(D)** Specific actions can individually be enabled for the organization, including version information.

**(E)** Individual third-party actions enabled with a specific tag will prevent updated versions of the action from introducing vulnerabilities.

**(F)** Actions can be restricted to only those available in the enterprise.

**Correct Answers:**

**(A)** Actions can be published to an internal marketplace.

**(D)** Specific actions can individually be enabled for the organization, including version information.

**(F)** Actions can be restricted to only those available in the enterprise.

**Explanation:**

GitHub Enterprise allows strict control over action usage via internal marketplace and restrictions.

---



**Question 38:** Disabling a workflow allows you to stop a workflow from being triggered without having to delete the file from the repo. In which of the following scenarios is disabling a workflow most useful? (Choose two)

**Options:**

**(A)** A workflow needs to be changed from running on a schedule to a manual trigger.

**(B)** A workflow error produces too many, or wrong, requests, impacting external services negatively.

**(C)** A workflow sends requests to a service that is down.

**(D)** A workflow is configured to run on self-hosted runners.

**(E)** A runner needs to have diagnostic logging enabled.

**Correct Answers:**

**(B)** A workflow error produces too many, or wrong, requests, impacting external services negatively.

**(C)** A workflow sends requests to a service that is down.

**Explanation:**

-Disabling workflows prevents unnecessary runs that could cause harm or waste resources.

---



**Question 39:** Where should workflow files be stored to be triggered by events in a repository?

**Options:**

**(A)** .github/workflows/

**(B)** workflows/

**(C)** .github/actions/

**(D)** anywhere

**(E)** Nowhere; they must be attached to an action in the GitHub user interface.

**Correct Answer:**

**(A)** .github/workflows/

**Explanation:**

-All workflows must be placed inside .github/workflows/ to be triggered automatically.

---



**Question 40:** What are the most significant advantages of adding documentation while distributing custom actions? (Choose two)

**Options:**

**(A)** It provides an example of the action.

**(B)** It shares the description of the action to the users.

**(C)** It generates auto completion when using the action in a workflow.

**(D)** It creates a `readme.md` for the consuming workflow.

**Correct Answers:**

**(A)** It provides an example of the action.

**(B)** It shares the description of the action to the users.

**Explanation:**

-Documentation guides users and improves action adoption.

---



**Question 41:** Which files are required for a Docker container action in addition to the source code? (Choose two)

**Options:**

**(A)** Actionfile

**(B)** metadata.yml

**(C)** Dockerfile

**(D)** action.yml

**Correct Answers:**

**(C)** Dockerfile

**(D)** action.yml

**Explanation:**

-Dockerfile defines the container.
-action.yml provides metadata.

---



**Question 42:** Which of the following is the most common way to target a specific major release version?

**Options:**

**(A)**
  ```
  steps:
    - uses: actions/checkout
  ```

**(B)**
  ```
  steps:
    - uses: actions/checkout@v3
  ```

**(C)**
  ```
  steps:
    - uses: actions/checkout@011673995124
  ```

**(D)**
  ```
  steps:
    - uses: actions/checkout@ac939985615ec5ede59e132de21d2eb1cbd6121c
  ```

**Correct Answer:**

**(B)**
  ```
  steps:
    - uses: actions/checkout@v3
  ```

**Explanation:**

-@v3 targets a major version while allowing minor updates.

---



**Question 43:** In the following workflow file, line 5 interprets lines 3 and 4 as Python. Which of the following is a valid option to complete line 5?

```yaml
1 steps:
2   - run: |
3     import os
4     print(os.environ['PATH'])
5 
```

**Options:**

**(A)** `shell: bash`

**(B)** `with: python`

**(C)** `working-directory: .github/python`

**(D)** `shell: python`

**Correct Answer:**

**(B)** `with: python`

**Explanation:**

-shell: python ensures the script runs in a Python environment.

---



**Question 44:** What is a valid scenario regarding environment secrets?

**Options:**

**(A)** ensuring only a specific step can access an environment secret

**(B)** configuring environment secrets to automatically pull from Azure Key Vault

**(C)** configuring environment secrets for connecting to GitHub Enterprise Server

**(D)** ensuring a job cannot access environment secrets until approval is obtained from a required reviewer

**Correct Answer:**

**(D)** ensuring a job cannot access environment secrets until approval is obtained from a required reviewer

**Explanation:**

-GitHub supports required reviewers to prevent unauthorized secret access.

---



**Question 45:** By default, which workflows can use an action stored in an internal repository? (Choose two)

**Options:**

**(A)** public repositories owned by the same organization as the enterprise

**(B)** internal repositories owned by the same organization as the enterprise

**(C)** private repositories owned by an organization of the enterprise

**(D)** selected public repositories outside of the enterprise

**Correct Answers:**

**(B)** internal repositories owned by the same organization as the enterprise

**(C)** private repositories owned by an organization of the enterprise

**Explanation:**

-Internal and private repositories can share actions within the organization.

---



**Question 46:** You are a DevOps engineer in ABC Corp. You need to schedule your deployment workflow twice a week at 7:45 AM on Wednesday and Saturday. What is the appropriate YAML structure?

**Options:**

**(A)**
  ```yaml
  on:
    cron: '45 7 * * 3,6'
  ```

**(B)**
  ```yaml
  on:
    cron:
      - schedule: '45 7 * * 3,6'
  ```

**(C)**
  ```yaml
  schedule: '45 7 * * 3,6'
  ```

**(D)**
  ```yaml
  on:
    schedule:
      - cron: '45 7 * * 3,6'
  ```

**Correct Answer:**

**(D)**
  ```yaml
  on:
    schedule:
      - cron: '45 7 * * 3,6'
  ```

**Explanation:**

-The correct cron format specifies 7:45 AM on Wednesdays (3) and Saturdays (6).

---



**Question 47:** You are reaching your organization's storage limit for GitHub artifacts and packages. What should you do to prevent the storage limit from being reached? (Choose two)

**Options:**

**(A)** Disable branch protections in the repository.

**(B)** Use self-hosted runners for all workflow runs.

**(C)** Configure the repo to use Git Large File Storage.

**(D)** Delete artifacts from the repositories manually.

**(E)** Configure the artifact and log retention period.

**Correct Answers:**

**(D)** Delete artifacts from the repositories manually.

**(E)** Configure the artifact and log retention period.

**Explanation:**

-Deleting artifacts and setting retention policies helps manage storage limits.

---



**Question 48:** As a developer, you have configured an IP allow list on a GitHub organization. Which effects does the IP allow list have on GitHub Actions? (Choose two)

**Options:**

**(A)** You must allow GitHub Actions's IP address ranges in order to use marketplace actions.

**(B)** You can use standard GitHub-hosted runners since their IP addresses are automatically allowed.

**(C)** You can use GitHub-hosted larger runners since they can be configured with static IP addresses.

**(D)** You can use self-hosted runners with known IP addresses.

**Correct Answers:**

**(A)** You must allow GitHub Actions's IP address ranges in order to use marketplace actions.

**(D)** You can use self-hosted runners with known IP addresses.

**Explanation:**

-GitHub-hosted runners change IPs frequently, so whitelisting them is required.

---



**Question 49:** What is the proper syntax to reference the system-provided run number variable?

**Options:**

**(A)** `$GITHUB_RUN_NUMBER`

**(B)** `${{ GITHUB_RUN_NUMBER }}`

**(C)** `$github.run_number`

**(D)** `${{ env.GITHUB_RUN_NUMBER }}`

**(E)** `${{ var.GITHUB_RUN_NUMBER }}`

**Correct Answer:**

**(B)** `${{ GITHUB_RUN_NUMBER }}`

**Explanation:**

-GITHUB_RUN_NUMBER provides a unique identifier for each workflow run.

---



**Question 50:** What are the mandatory requirements for publishing GitHub Actions to the GitHub Marketplace? (Choose two)

**Options:**

**(A)** Each repository can contain a collection of actions as long as they are under the same Marketplace category.

**(B)** The action's name cannot match a user or organization on GitHub unless the user or organization owns the action.

**(C)** The action's metadata file must be in the root directory of the repository.

**(D)** The action can be either in a public or private repository.

**(E)** The name should match with one of the existing GitHub Marketplace categories.

**Correct Answers:**

**(B)** The action's name cannot match a user or organization on GitHub unless the user or organization owns the action.

**(C)** The action's metadata file must be in the root directory of the repository.

**Explanation:**

-Actions must have a unique name that does not match an existing user or organization unless owned by them.
-The metadata file (action.yml) must be placed in the root directory so that GitHub Marketplace can correctly identify and list the action.

---



**Question 51:** Which default environment variable specifies the branch or tag that triggered a workflow?

**Options:**

**(A)** `ENV_BRANCH`

**(B)** `GITHUB_BRANCH`

**(C)** `GITHUB_TAG`

**(D)** `GITHUB_REF`

**Correct Answer:**

**(D)** `GITHUB_REF`

**Explanation:**

-GITHUB_REF stores the branch or tag reference (refs/heads/main for branches, refs/tags/v1.0 for tags) that triggered the workflow.

---



**Question 52:** As a developer, you are optimizing a GitHub workflow that uses and produces many different files. You want to improve performance and reduce workflow run times versus workflow artifacts. Which two statements are true? (Choose two)

**Options:**

**(A)** Use artifacts to access the GitHub Package Registry and download a package for a workflow.

**(B)** Use artifacts when referencing files produced by a job after a workflow has ended.

**(C)** Use caching when reusing files that change rarely between jobs or workflow runs.

**(D)** Use caching to store cache entries for up to 30 days between accesses.

**Correct Answers:**

**(B)** Use artifacts when referencing files produced by a job after a workflow has ended.

**(C)** Use caching when reusing files that change rarely between jobs or workflow runs.

**Explanation:**

-Artifacts store job-generated files after the workflow ends, making them available for later reference.
-Caching speeds up workflows by storing frequently used files between runs (e.g., dependencies).

---



**Question 53:** Which default GitHub environment variable indicates the name of the person or app that initiated a workflow run?

**Options:**

**(A)** GITHUB_ACTOR

**(B)** GITHUB_USER

**(C)** ENV_ACTOR

**(D)** GITHUB_WORKFLOW_ACTOR

**Correct Answer:**

**(A)** GITHUB_ACTOR

**Explanation:**

-GITHUB_ACTOR stores the username or app name that initiated the workflow run.

---



**Question 54:** As a developer, one of your workflows will require Xcode version 11.2 on a hosted macOS Catalina (i.e., v10.15) runner. You've already created and configured a self-hosted runner to conform to those requirements and registered it with your organization. What else should you do to ensure that the workflow accesses the correct runner instance? (Choose three)

**Options:**

**(A)** Assign the custom labels to the self-hosted runner.

**(B)** In the workflow, specify:
  ```yaml
  runs-on: [ self-hosted, macos-10.15, xcode-11.2 ]
  ```

**(C)** Add your runner to the appropriate runner groups.

**(D)** Create custom runner labels for macos-10.15 and xcode-11.2.

**(E)** In the workflow, specify:
  ```yaml
  runs-on: [ ${{groups.macos-10.15}}, ${{groups.xcode-11.2}} ]
  ```

**(F)** Create runner groups named `macos-10.15` and `xcode-11.2`.

**Correct Answers:**

**(A)** Assign the custom labels to the self-hosted runner.

**(B)** In the workflow, specify:
  ```yaml
  runs-on: [ self-hosted, macos-10.15, xcode-11.2 ]
  ```

**(C)** Add your runner to the appropriate runner groups.

**Explanation:**

-Labeling the runner ensures proper identification and allows workflows to select it based on OS and software version.
-Using runs-on: [...] in the workflow file directs the job to the appropriate runner.
-Grouping runners makes management easier.

---



**Question 55:** You are a DevOps engineer working on deployment workflows. You need to execute the `deploy` job only if the current branch name is `feature-branch`. Which code snippet will help you to implement the conditional execution of the job?

**Options:**

**(A)**
  ```yaml
  jobs:
    deploy:
      if: github.ref_name == 'feature-branch'
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
  ```

**(A)**
  ```yaml
  jobs:
    deploy:
      if: github.branch_name == 'feature-branch'
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
  ```

**Correct Answer:**

**(A)**
  ```yaml
  jobs:
    deploy:
      if: github.ref_name == 'feature-branch'
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
  ```

**Explanation:**

-github.ref_name provides the current branch name, ensuring that the deploy job only runs when on feature-branch.


---



**Question 56:** Which of the following is a valid reusable workflow reference?

**Options:**

**(A)** `uses: another-repo/.github/workflows/workflow.yml@v1`

**(B)** `uses: octo-org/another-repo/workflow.yml@v1`

**(C)** `uses: another-repo/workflow.yml@v1`

**(D)** `uses: octo-org/another-repo/.github/workflows/workflow.yml`

**Correct Answer:**

**(B)** `uses: octo-org/another-repo/workflow.yml@v1`

**Explanation:**

-The correct format for referencing a reusable workflow is:
 `uses: owner/repository/.github/workflows/workflow.yml@version`

---



**Question 57:** What are two reasons to keep an action in its own repository instead of bundling it with other application code? (Choose two)

**Options:**
- It widens the scope of the code base for developers fixing issues and extending the action.
- It decouples the action's versioning from the versioning of other application code.
- It allows sharing workflow secrets with other users.
- It makes the `action.yml` file optional.
- It makes it easier for the GitHub community to discover the action.

**Correct Answers:**
- **It decouples the action's versioning from the versioning of other application code.**
- **It makes it easier for the GitHub community to discover the action.**

**Supporting Statement:** Keeping an action in its own repository allows for independent versioning and makes it more discoverable by the GitHub community.

---



**Question 58:** Which command can you include in your workflow file to set the output parameter for an action?

**Options:**
- `echo "::add-mask::ACTION_COLOR"`
- `echo "action_color=purple" >> $GITHUB_OUTPUT`
- `echo "::debug::action_color=purple"`
- `echo "action_color=purple" >> $GITHUB_ENV`

**Correct Answer:**
- `echo "action_color=purple" >> $GITHUB_OUTPUT`

**Supporting Statement:** This command writes the output parameter "action_color" with the value "purple" to the `$GITHUB_OUTPUT` file, making it available to subsequent steps or other actions.

---



**Question 59:** As a developer, you are configuring GitHub Actions to deploy VMs to production. A member of the VM Ops team must provide approval before deployment occurs. Which of the following steps should you take? (Choose two)

**Options:**
- Navigate to the repository settings and create a Production environment with the VM Ops team as a required reviewer.
- Navigate to the organization settings and create a Production environment with the VM Ops team as a required reviewer.
- Add the VMs to the environment.
- Specify the VM Ops team as the owner of the environment.
- Specify the environment named Production in the workflow jobs that deploy to the VMs.

**Correct Answers:**
- **Navigate to the organization settings and create a Production environment with the VM Ops team as a required reviewer.**
- **Specify the environment named Production in the workflow jobs that deploy to the VMs.**

**Supporting Statement:** 
* Creating a production environment with required reviewers in the organization settings allows for centralized management of deployment approvals.
* Specifying the environment in the workflow jobs ensures that the deployment process is subject to the approval requirements.

---



**Question 60:** As a developer, you need to use GitHub Actions to deploy a microservice that requires runtime access to a secure token used by a variety of other microservices managed by different teams in different repos. To minimize management overhead and ensure the token is secure, which mechanisms should you use and access the token? (Choose two)

**Options:**
- Store the token as an organizational-level encrypted secret in GitHub. During deployment, use GitHub Actions to store the secret in an environment variable that can be accessed at runtime.
- Store the token in a configuration file in a private repository. Use GitHub Actions to deploy the configuration file to the runtime environment.
- Store the token as a GitHub encrypted secret in the same repo as the code. Create a reusable custom GitHub Action to access the token by the microservice at runtime.
- Use a corporate non-GitHub secret store (e.g., Hash Corp Vault) to store the token. During deployment, use GitHub Actions to store the secret in an environment variable that can be accessed at runtime.
- Store the token as a GitHub encrypted secret in the same repo as the code. During deployment, use GitHub Actions to store the secret in an environment variable that can be accessed at runtime.

**Correct Answers:**
- **Store the token as an organizational-level encrypted secret in GitHub. During deployment, use GitHub Actions to store the secret in an environment variable that can be accessed at runtime.**
- **Use a corporate

---



**Question 61:** As a developer, you need to use GitHub Actions to deploy a microservice that requires runtime access to a secure token used by a variety of other microservices managed by different teams in different repos. To minimize management overhead and ensure the token is secure, which mechanisms should you use and access the token? (Choose two)

**Options:**
- Store the token as an organizational-level encrypted secret in GitHub. During deployment, use GitHub Actions to store the secret in an environment variable that can be accessed at runtime.
- Store the token in a configuration file in a private repository. Use GitHub Actions to deploy the configuration file to the runtime environment.
- Store the token as a GitHub encrypted secret in the same repo as the code. Create a reusable custom GitHub Action to access the token by the microservice at runtime.
- Use a corporate non-GitHub secret store (e.g., Hash Corp Vault) to store the token. During deployment, use GitHub Actions to store the secret in an environment variable that can be accessed at runtime.
- Store the token as a GitHub encrypted secret in the same repo as the code. During deployment, use GitHub Actions to store the secret in an environment variable that can be accessed at runtime.

**Correct Answers:**
- **Store the token as an organizational-level encrypted secret in GitHub. During deployment, use GitHub Actions to store the secret in an environment variable that can be accessed at runtime.**
- **Use a corporate non-GitHub secret store (e.g., Hash Corp Vault) to store the token. During deployment, use GitHub Actions to store the secret in an environment variable that can be accessed at runtime.**

**Supporting Statement:** 
* Storing the token at the organizational level provides central management and control.
* Using a dedicated secret store like Hashicorp Vault offers enhanced security and centralized management for sensitive information.

---



**Question 62:** You installed specific software on a Linux self-hosted runner. You have users with workflows that need to be able to use this identified custom software. Which steps should you perform to prepare the runner and your users to run these workflows? (Choose two)

**Options:**
- Inform users to identify the runner based on the group.
- Create the group `custom-software-on-linux` and move the runner into the group.
- Inform users to identify the runner with the labels `custom-software` and `linux`.
- Configure the webhook and network to enable GitHub to trigger workflows.
- Add the label `custom-software` to the runner.
- Add the label `linux` to the runner.

**Correct Answers:**
- **Create the group `custom-software-on-linux` and move the runner into the group.**
- **Inform users to identify the runner based on the group.**

**Supporting Statement:** Organizing runners into groups based on their capabilities (like installed software) allows users to easily identify and select the appropriate runner for their workflows.

---



**Question 63:** What is the proper syntax to reference the system-provided run number variable?

**Options:**
- `$GITHUB_RUN_NUMBER`
- `${{ GITHUB_RUN_NUMBER }}`
- `$github.run_number`
- `${{ env.GITHUB_RUN_NUMBER }}`
- `${{ var.GITHUB_RUN_NUMBER }}`

**Correct Answer:**
- `${{ GITHUB_RUN_NUMBER }}`

**Supporting Statement:** This is the correct syntax to access the value of the `GITHUB_RUN_NUMBER` environment variable within a workflow file.

---



**Question 64:** What are the mandatory requirements for publishing GitHub Actions to the GitHub Marketplace? (Choose two)

**Options:**
- Each repository can contain a collection of actions as long as they are under the same Marketplace category.
- The action's name cannot match a user or organization on GitHub unless the user or organization owns the action.
- The action's metadata file must be in the root directory of the repository.
- The action can be either in a public or private repository.
- The name should match with one of the existing GitHub Marketplace categories.

**Correct Answers:**
- **The action's metadata file must be in the root directory of the repository.**
- **The action's name cannot match a user or organization on GitHub unless the user or organization owns the action.**

**Supporting Statement:** These are two of the essential requirements for publishing Actions to the Marketplace.

---



**Question 65:** Which default environment variable specifies the branch or tag that triggered a workflow?

**Options:**
- `ENV_BRANCH`
- `GITHUB_BRANCH`
- `GITHUB_TAG`
- `GITHUB_REF`

**Correct Answer:**
- **GITHUB_REF**

**Supporting Statement:** The `GITHUB_REF` environment variable contains the full Git reference (e.g., `refs/heads/main`, `refs/tags/v1.0`) that triggered the workflow.

---



**Question 66:** As a developer, you need to make sure that only actions from trusted sources are available for use in your GitHub Enterprise Cloud organization. (Choose three)

**Options:**
- Actions can be published to an internal marketplace.
- Actions created by GitHub are automatically enabled and cannot be disabled.
- GitHub-verified actions can be collectively enabled for use in the enterprise.
- Specific actions can individually be enabled for the organization, including version information.
- Individual third-party actions enabled with a specific tag will prevent updated versions of the action from introducing vulnerabilities.
- Actions can be restricted to only those available in the enterprise.

**Correct Answers:**
- **Actions can be published to an internal marketplace.**
- **GitHub-verified actions can be collectively enabled for use in the enterprise.**
- **Specific actions can individually be enabled for the organization, including version information.**

**Supporting Statement:** These options provide ways to control which actions are available within your organization, allowing you to prioritize trusted sources and manage access.

---



**Question 67:** Disabling a workflow allows you to stop a workflow from being triggered without having to delete the file from the repo. In which of the following scenarios is disabling a workflow most useful? (Choose two)

**Options:**
- A workflow needs to be changed from running on a schedule to a manual trigger.
- A workflow error produces too many, or wrong, requests, impacting external services negatively.
- A workflow sends requests to a service that is down.
- A workflow is configured to run on self-hosted runners.
- A runner needs to have diagnostic logging enabled.

**Correct Answers:**
- **A workflow needs to be changed from running on a schedule to a manual trigger.**
- **A workflow error produces too many, or wrong, requests, impacting external services negatively.**

**Supporting Statement:** Disabling a workflow is useful when you need to temporarily stop its execution without making permanent changes to the workflow file. This is helpful for scenarios like switching to manual triggers or mitigating issues caused by errors.

---



**Question 68:** Where should workflow files be stored to be triggered by events in a repository?

**Options:**
- .github/workflows/
- workflows/
- .github/actions/
- anywhere
- Nowhere; they must be attached to an action in the GitHub user interface.

**Correct Answer:**
- **.github/workflows/**

**Supporting Statement:** Workflow files must be placed in the `.github/workflows/` directory within the repository to be recognized and triggered by GitHub Actions.

---



**Question 69:** What are the most significant advantages of adding documentation while distributing custom actions? (Choose two)

**Options:**
- It provides an example of the action.
- It shares the description of the action to the users.
- It generates auto completion when using the action in a workflow.
- It creates a `readme.md` for the consuming workflow.

**Correct Answers:**
- **It shares the description of the action to the users.**
- **It generates auto completion when using the action in a workflow.**

**Supporting Statement:** Documentation is crucial for users to understand how to use an action. It helps them understand its purpose, inputs, outputs, and usage examples, and can often enable IDEs to provide auto-completion suggestions when using the action.

---



**Question 70:** Which files are required for a Docker container action in addition to the source code? (Choose two)

**Options:**
- Actionfile
- metadata.yml
- Dockerfile
- action.yml

**Correct Answers:**
- **Dockerfile**
- **action.yml**

**Supporting Statement:**
- The `Dockerfile` is essential for building the Docker image that will run the action.
- The `action.yml` file provides metadata about the action, such as inputs, outputs, and other configuration details.

---



**Question 71:** Which of the following is the most common way to target a specific major release version?

**Options:**
- ```
  steps:
    - uses: actions/checkout
  ```
- ```
  steps:
    - uses: actions/checkout@v3
  ```
-

---



**Question 72:** In the following workflow file, line 5 interprets lines 3 and 4 as Python. Which of the following is a valid option to complete line 5?

```yaml
1 steps:
2   - run: |
3     import os
4     print(os.environ['PATH'])
5 
```

**Options:**
- `shell: bash`
- `with: python`
- `working-directory: .github/python`
- `shell: python`

**Correct Answer:**
- `shell: python`

**Supporting Statement:** Since lines 3 and 4 contain Python code, specifying `shell: python` on line 5 tells the runner to interpret those lines as Python commands.

---



**Question 73:** What is a valid scenario regarding environment secrets?

**Options:**
- ensuring only a specific step can access an environment secret
- configuring environment secrets to automatically pull from Azure Key Vault
- configuring environment secrets for connecting to GitHub Enterprise Server
- ensuring a job cannot access environment secrets until approval is obtained from a required reviewer

**Correct Answer:**
- **ensuring only a specific step can access an environment secret**

**Supporting Statement:** This scenario demonstrates a valid use case for controlling access to sensitive information within a workflow.

---



**Question 74:** By default, which workflows can use an action stored in an internal repository? (Choose two)

**Options:**
- public repositories owned by the same organization as the enterprise
- internal repositories owned by the same organization as the enterprise
- private repositories owned by an organization of the enterprise
- selected public repositories outside of the enterprise

**Correct Answers:**
- **internal repositories owned by the same organization as the enterprise**
- **private repositories owned by an organization of the enterprise**

**Supporting Statement:** By default, workflows within an enterprise can access actions stored in internal repositories (private or internal) owned by organizations belonging to the same enterprise.

---



**Question 75:** You are a DevOps engineer in ABC Corp. You need to schedule your deployment workflow twice a week at 7:45 AM on Wednesday and Saturday. What is the appropriate YAML structure?

**Options:**
- ```yaml
  on:
    cron: '45 7 * * 3,6'
  ```
- ```yaml
  on:
    cron:
      - schedule: '45 7 * * 3,6'
  ```
- ```yaml
  schedule: '45 7 * * 3,6'
  ```
- ```yaml
  on:
    schedule:
      - cron: '45 7 * * 3,6'
  ```

**Correct Answer:**
- ```yaml
  on:
    schedule:
      - cron: '45 7 * * 3,6'
  ```

**Supporting Statement:** This YAML structure correctly defines a cron schedule to trigger the workflow at 7:45 AM on Wednesdays and Saturdays.

---



**Question 76:** You are reaching your organization's storage limit for GitHub artifacts and packages. What should you do to prevent the storage limit from being reached? (Choose two)

**Options:**
- Disable branch protections in the repository.
- Use self-hosted runners for all workflow runs.
- Configure the repo to use Git Large File Storage.
- Delete artifacts from the repositories manually.
- Configure the artifact and log retention period.

**Correct Answers:**
- **Configure the repo to use Git Large File Storage.**
- **Configure the artifact and log retention period.**

**Supporting Statement:** 
  * Git Large File Storage (LFS) stores large files outside the main repository, reducing storage usage.
  * Configuring retention periods for artifacts and logs helps automatically delete old data, freeing up space.

---



**Question 77:** As a developer, you have configured an IP allow list on a GitHub organization. Which effects does the IP allow list have on GitHub Actions? (Choose two)

**Options:**
- You must allow GitHub Actions's IP address ranges in order to use marketplace actions.
- You can use standard GitHub-hosted runners since their IP addresses are automatically allowed.
- You can use GitHub-hosted larger runners since they can be configured with static IP addresses.
- You can use self-hosted runners with known IP addresses.

**Correct Answers:**
- **You must allow GitHub Actions's IP address ranges in order to use marketplace actions.**
- **You can use self-hosted runners with known IP addresses.**

**Supporting Statement:** 
* Marketplace actions run on GitHub's infrastructure, so their IP addresses need to be allowed for communication.
* With self-hosted runners, you control the IP addresses, allowing you to add them explicitly to the allow list for security. 


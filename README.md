
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



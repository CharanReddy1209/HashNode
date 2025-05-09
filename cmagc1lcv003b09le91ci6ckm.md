---
title: "Mastering GitHub Actions: Jobs, Artifacts, Variables, and Secrets"
seoTitle: "Master GitHub Actions: Jobs and Secrets Explained"
seoDescription: "Explore intermediate GitHub Actions: job orchestration, artifact management, variable scope, and secrets for efficient CI/CD pipeline automation"
datePublished: Fri May 09 2025 05:05:22 GMT+0000 (Coordinated Universal Time)
cuid: cmagc1lcv003b09le91ci6ckm
slug: mastering-github-actions-jobs-artifacts-variables-and-secrets
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1746766414612/fefcce67-cbdd-47f5-80ca-503d9f055b18.png
tags: automation, devops, ci-cd, github-actions-1, devops-articles

---

GitHub Actions is a powerful CI/CD tool that enables automation directly in your GitHub repositories. Whether you're running tests, building Docker images, or deploying applications, mastering GitHub Actions unlocks scalable and maintainable automation workflows.

In this post, we'll cover some of the most important intermediate concepts:

* Working with multiple jobs (parallel and sequential execution)
    
* Storing workflow data as artifacts
    
* Using variables at different levels (step, job, global)
    
* Working with repository-level secret and variables.
    

## Working with Multiple Jobs

Job is a collection of steps that run on the same GitHub Actions Runner. Jobs can run independently (in parallel) or be dependent on other jobs (sequential).

When a workflow contains multiple jobs, each job runs on a separate runner and executes in parallel by default. This can lead to workflow failures if the jobs depend on each other and require a specific execution order.

* ### Parallel Job Execution
    
    By default, jobs run in parallel. This is useful for optimizing pipeline time.
    

```yaml
name: Parallel Jobs Example
on: [push]
jobs:
  job1:
    runs-on: ubuntu-latest
    steps:
      - name: Print Job 1
        run: echo "Running Job 1"

  job2:
    runs-on: ubuntu-latest
    steps:
      - name: Print Job 2
        run: echo "Running Job 2"
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746719626462/aa7da3cb-be53-4c22-ac0b-ce9af2fd5e2f.png align="right")

Here, job1 and job2 run simultaneously, speeding up the workflow.

* ### Sequential Job Execution
    
    To make one job wait for another, use the ***needs*** keyword.
    

```yaml
name: Sequential Jobs Example
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build
        run: echo "Building the app"

  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Run Tests
        run: echo "Running tests"

  deploy:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - name: Deploy
        run: echo "Deploying to production"
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746719982215/4379b882-e634-4a1b-a0f2-d560401ce169.png align="center")

In this example, the **test** job only runs after **build**, and **deploy** only after **test**.

## Storing Workflow Data as Artifacts

**Artifacts** allow you to persist data from one job and use it in another (or download it manually).

Artifacts are files generated during a workflow run that can be uploaded, stored, and later downloaded either by other jobs within the same workflow or manually by developers.

Jobs in GitHub Actions run on separate runners, so they **do not share filesystem data**. Artifacts bridge this gap by enabling you to pass data (e.g., build outputs, test reports) from one job to another.

Artifacts are stored within GitHub itself, so there\`s no need to set up and maintain external storage systems.

* ### Storing Artifacts
    

```yaml
- name: Save build output
  uses: actions/upload-artifact@v4
  with:
    name: build-output
    path: ./dist/            # this is the path of the file or folder you want as artifact
```

* ### Downloading Artifacts in Another Job
    

```yaml
- name: Download build output
  uses: actions/download-artifact@v4
  with:
    name: build-output
    path: ./retrieved-build
```

Using artifact is a clean and efficient way to pass data between the dependent jobs.

## Working with Variables in GitHub Actions

In GitHub Actions, variables are used to store values in your workflows.

For adding the variable we need to use the keyword “***env:***”

There are several types of variables available:

### Types of Variables:

1. **step-level variable**
    
    step-level variable is a variable that is defined and accessible only within a specific **step** in the job.
    
    **Example of Step-Level Variable**
    
    ```yaml
    steps:
      - name: Step with local env
        env:
          LOCAL_VAR: "Hello"
        run: echo $LOCAL_VAR
    ```
    
2. **job-level variable**
    
    job-level variable is a variable that is defined and accessible within the specific job.
    
    **Example of Job-Level Variable**
    
    ```yaml
    jobs:
      example:
        runs-on: ubuntu-latest
        env:
          JOB_LEVEL_VAR: "Available in all steps"
        steps:
          - run: echo $JOB_LEVEL_VAR
    ```
    
3. **workflow-level variable**  
    workflow-level variable are the variables that are defined at workflow level and can be accessible through out the workflow.  
    **Example of Workflow-Level Variable**
    
    ```yaml
    name: Variable Example
    env:
      GLOBAL_VAR: "I am global"
    jobs:
      print:
        runs-on: ubuntu-latest
        steps:
          - run: echo $GLOBAL_VAR
    ```
    

## Working with Repository-Level Secrets and Variables

### **Repository-Level Secrets**

**Repository-level secrets** allow you to securely store sensitive data (like API keys, passwords, or tokens) and use them in your workflows without exposing them in code.

### Setting Up Repository Secrets

**GitHub Web UI**:

* Go to your **repository** → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**.
    
* Enter a **Name** (e.g., AWS\_ACCESS\_KEY\_ID) and **Value**.
    

### Using Secrets in Workflow

Secrets can be referenced in workflows using ${{ secrets.SECRET\_NAME }}.

“secrets” is the keyword for accessing the repository level secretes inside the workflow.

**Example Workflow**

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Use a secret
        run: echo "Using AWS Key: ${{ secrets.AWS_ACCESS_KEY_ID }}"
```

### **Repository-Level Variables**

**Repository-level variables** allow you to store **non-sensitive configuration values** (like URLs, IDs, or flags) that can be reused across workflows in a repository.

Unlike **secrets**, these are **not encrypted**, so they should **only store non-sensitive data**.

### Setting Up Repository-Level Variables

**GitHub Web UI**:

* Go to your **repository** → **Settings** → **Secrets and variables** → **Actions**.
    
* Click **Variables** → **New repository variable**.
    
* Enter a **Name** (e.g., API\_BASE\_URL) and **Value** (e.g., https://api.example.com).
    

### Using Variables in Workflows

Variables can be referenced in workflows using ${{ vars.VAR\_NAME }}.

“vars” is the keyword for accessing the Repository level secretes inside the workflow.

**Example Workflow**

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Use a repository variable
        run: echo "API URL: ${{ vars.API_BASE_URL }}"
```

## Conclusion

Understanding intermediate concepts in GitHub Actions—like job orchestration, artifact sharing, variable scopes, and secure secret and variable management gives you greater control and flexibility in automating your workflows. These features not only improve CI/CD reliability but also help structure your pipelines for team collaboration and long-term maintainability.

In my [previous post,](https://hashnode.com/post/cmac18o42000m09jy1qjf122f) I demonstrated how I implemented a CI/CD pipeline for a project using GitHub Actions. That project helped me understand how to chain jobs, manage secrets, and pass build artifacts between stages—all of which are concepts discussed in this post.
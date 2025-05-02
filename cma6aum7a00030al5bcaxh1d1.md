---
title: "Level Up Your GitHub Actions"
seoTitle: "Enhance Your GitHub Actions Skills"
seoDescription: "Learn advanced GitHub Actions techniques for cleaner workflows, multi-line commands, and optimizing triggers and skips"
datePublished: Fri May 02 2025 04:34:15 GMT+0000 (Coordinated Universal Time)
cuid: cma6aum7a00030al5bcaxh1d1
slug: level-up-your-github-actions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1746029391684/71ee6877-d25b-4a27-b247-e63e041f789f.png
tags: automation, devops, cicd, github-actions-1, devops-articles

---

In my [previous blog post](https://hashnode.com/post/cma218zo3000409legkvv5c4p), I covered the fundamentals of GitHub Actions — from workflows and jobs to events and runners. If you’re already familiar with the basics, it’s time to level up!

In this blog post, we will go one step further and explore the other topics in GitHub Actions like:

* Difference between “run” and “uses”,
    
* Executing multi line commands in workflow,
    
* Running shell scripts through GitHub Actions,
    
* Skipping the workflows,
    
* Different ways to trigger the workflows.
    

These topics will help you to write clean, effective, and more flexible GitHub Actions workflows.

### Difference between “run” and “uses”

* ### run:
    
    For executing the shell commands directly in the workflow.
    
    when you want to **execute shell commands** like installing dependencies, running tests, or custom scripts.
    

```yaml
- name: Install Dependences
  run: npm install
```

This will execute the “*npm install*“ command in the runner terminal.

* ### uses
    
    It is used to call a pre-defined action from the GitHub marketplace.
    

```yaml
- name: checkout code
  uses: actions/checkout@v4
```

This will download the repository code into the runner.

### Executing Multi - Line commands in Workflow

Sometimes you need to run multiple commands in your workflow for doing an automation task especially when your are installing a package or setting up environment variables. But for each command we can’t write a different step in the workflow.

So for executing a multi-line commands we need to use the **run** with **pipe (|).**

**syntax:** run: |

```yaml
jobs:
  build:
   runs-on: ubuntu-latest
   steps:
   - name: checkout code
     uses: actions/checkout@v4
   - name: build and push docker image
     run: |
       docker build -t demo-image:v1 .
       docker push demo-image:v1
```

This makes your scripts easier to read, debug , and modify.

### Running shell scripts through GitHub Actions

If you have a long script or logic that you want to run on the runner, then it’s better to move the script to .sh file and store the file in the same repository.

For example you want to install the trivy in the runner for scanning the file system of your project. Let the file name be **trivy.sh.**

```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
```

Workflow for running a script

```yaml
jobs:
  install_trivy:
   runs-on: ubuntu-latest
   steps:
   - name: checkout code
     uses: actions/checkout@v4

   - name: Change the permission
     run: sudo chmod +x ./trivy.sh   # path to your script file

   - name: Install trivy
     run: ./trivy.sh
```

By default the file does not have the execute permission so we need to grant it.

### Skipping the workflows

There are situations where running a workflow is not necessary — like a minor change in the documents or in the README.md file and other files where the source code is not changed.

If you add any of the following string along with the commit message while pushing the code.

* `[skip ci]`
    
* `[ci skip]`
    
* `[no ci]`
    
* `[skip actions]`
    
* `[actions skip]`
    

```bash
git commit -m "updated README file [skip ci]"
```

### Different ways to trigger the workflows

Workflow triggers are events that cause a workflow to run. These events can be:

* Events that occur in your workflow's repository
    
* Scheduled times
    
* Manual
    

Workflow triggers are defined with the `on` key.

For example, you can configure your workflow to run when a push is made to the default branch of your repository, when a release is created, or when an issue is opened.

GitHub Actions supports multiple trigger types, depending on your use case.

**on: push**

```yaml
on:
  push:
    branch: [main]
```

The workflow will run on a push event whenever changes are made to the main branch and pushed to GitHub.

**on: pull\_request**

```yaml
on:
  pull_request:
    branch: [main]
```

The workflow will run when a pull request is created or updated with the target branch set to `main`.

**on: workflow\_dispatch**

```yaml
on:
  workflow-dispatch:
```

The workflow can be manually triggered using the **workflow dispatch** event in the GitHub Actions interface.

This allows you to trigger workflows on demand via the GitHub UI.

**on: schedule**

```yaml
on:
  schedule:
    - cron : '0 0 * * *'  # Runs every day at midnight
```

The workflow will run automatically on a schedule, specifically every day at midnight, based on the defined cron expression `'0 0 * * *'`.

**Using multiple event triggers**

You can specify a single event or multiple events. For example, a workflow with the following `on` value will run when a push is made to any branch in the repository or when someone forks the repository:

```yaml
on: [push, fork]
```

If you specify multiple events, only one of those events needs to occur to trigger your workflow. If multiple triggering events for your workflow occur at the same time, multiple workflow runs will be triggered.

Apart from above triggers there are many other triggers you can use, you can get the information from official docs — [github actions docs](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows).

### Conclusion

GitHub Actions offers a wide range of capabilities that go far beyond just triggering tests or builds. By understanding the difference between `run` and `uses`, learning how to execute multi-line commands or shell scripts, and using workflow triggers and skip techniques effectively — you can write workflows that are **clean, flexible, and highly efficient**.

These advanced techniques are especially useful for real-world CI/CD automation, where controlling when and how workflows execute can save time, resources, and unnecessary complexity
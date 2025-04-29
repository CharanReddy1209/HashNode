---
title: "GitHub Actions For Beginners"
seoTitle: "GitHub Actions: A Beginner's Guide"
seoDescription: "Learn GitHub Actions: a powerful CI/CD tool integrated into GitHub for automating builds, tests, deployments, and more with ease and flexibility"
datePublished: Tue Apr 29 2025 04:54:24 GMT+0000 (Coordinated Universal Time)
cuid: cma218zo3000409legkvv5c4p
slug: github-actions-for-beginners
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1745902192351/e1deb44c-3fd2-4c6d-96f0-07a62735668b.png
tags: github, devops, ci-cd, devops-articles, githubaction

---

In this blog post we are going to know about what is GitHub Actions, what are the benefits and use cases for Actions, and also about the key concepts of GitHub Actions like workflows, actions, events, jobs. steps, runners.

### What is GitHub Actions?

GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline.

You can create workflows that build and test whenever you push a change to your repository, or that deploy merged pull requests to production.

### Benefits of GitHub Actions

1. Built into GitHub
    
    No need of external CI/CD tool setup (like Jenkins, CircleCI).
    
2. Easy to start
    
    Just add the YAML file in *.github/workflows.*
    
3. Free to public repositories
    
    Unlimited free minutes for public repos, and some free minutes for the private repos.
    
4. Huge Marketplace
    
    10,000+ prebuilt Actions you can reuse (Docker, AWS, Terraform, Slack bots, etc.).
    
5. Secure
    
    Fine-grained permissions and secrets management for safe operations.
    

### Use cases of GitHub Actions

1. CI/CD Pipeline
    
    Automatically **build**, **test**, and **deploy** your code when you push changes (or on any event).
    
2. Scheduled Jobs
    
    Run workflows on a schedule (like a cron job).
    
3. Docker Automation
    
    Build Docker images, push them to DockerHub/ECR/GCR, and deploy containers to Kubernetes or ECS.
    
4. Cloud Deployment
    
    Deploy applications to AWS, Azure, GCP, DigitalOcean, or any cloud provider.
    
5. Infrastructure Automation
    
    Use Terraform, Pulumi, or Ansible inside GitHub Actions to automate infrastructure changes.
    

### What is Workflow

A workflow is a configurable automation process that will run one or more jobs.

Workflows are defined in a YAML file inside *.github/workflows/* in your GitHub repository.

These workflows will run when triggered by an event in your repository. or they can be triggered manually, or at a defined schedule time. Some of the Events like push, pull\_request, schedule, or manually(workflow-dispatch)

Workflow can have multiple jobs(build, test, deploy etc).

```yaml
# This is an example for the workflow with a sigle job
name: demo workflow
on: push
jobs:
  demo:
    runs-on: ubuntu-latest
      steps:
        - name: welcome message
          run: echo "Welcome to GitHub Actions"
```

### What is Runners

A Runner is a server that runs your workflow when they are triggered. Each runner can run a single job at a time.

GitHub provides free runners like: Linux, Windows, and MacOS machines.

You can also use a self-hosted machine as a runners for more control.

```yaml
jobs:
  demo:
    runs-on: ubuntu-latest
```

**Meaning of *runs-on***

> This job will run on a free **Ubuntu** server provided by GitHub.

### what is Actions

Action is a single task you can reuse inside your workflow.

An Action is a custom application for GitHub Actions platform that perform a complex but frequently repeated task.

GitHub has thousands of ready-made actions in [GitHub Marketplace](https://github.com/marketplace?type=actions).

Example Actions are like Checkout, Login to DockerHub, Deploy to AWS ,etc.

There are Two types of Actions:

1. Pre built Actions: Example actions/checkout, actions/setup-node
    
2. Custom Actions: you can even write your own actions if needed.
    

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: actions/setup-node@v4
    with:
      node-version: '18'
```

actions/checkout — This is used get the git repo code into the runner machine.

actions/setup-node — This is used for installing the NodeJS into the runner machine.

### What is Jobs

A Job is a set of steps in a workflow that is executed on the same runner.

Jobs can run independently, sequentially, or in parallel.

Each job has:

* A Runner
    
* A list of steps (shell commands or actions)
    

```yaml
name: Example Workflow
on: push
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building the project"
  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - run: echo "Testing the project"
```

> In this workflow there are two jobs “build and test” , these two jobs are running on two different runners.

> Note:
> 
> **needs:** this is used to define the dependencies between two jobs to control the order of execution.

### what is Events

An Event is a specific activity in a repository that triggers a workflow to run.

Examples of events:

* push — when code is pushed to repo
    
* pull\_request — when a pull request is created or updated
    
* schedule — run on a timer(like a cron job)
    
* workflow\_dispatch — for manual triggering of the workflow
    
    and many more.
    

You can get more information about events from the [GitHub Actions Documentation](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows).

```yaml
# Triggers when code is pushed
on: push
```

```yaml
# Triggers on daily schedule
on:
  schedule:
    - cron: '0 0 * * *'   # Runs on everday at midnight
```

```yaml
# For manually triggering the workflow from GitHub web UI
on: workflow_dispatch
```

### Conclusion

GitHub Actions is a powerful, flexible, and easy-to-use CI/CD tool that is tightly integrated into GitHub. It empowers developers to automate their workflows without needing to rely on external tools. Whether you want to build and test your applications, deploy to production, manage infrastructure, or automate scheduled tasks — GitHub Actions has you covered.  
Its ease of use, massive marketplace, and deep GitHub integration make it a must-know tool for any modern DevOps or software engineer.
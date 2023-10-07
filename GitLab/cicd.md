# Introduction to GitLab CI/CD
## GitLab's CI/CD Workflow - Feature Breakdown
If we take a deeper look into the basic workflow, we can see the features available in GitLab at each stage of the DevOps lifecycle. On the image below, you can see some of the features available in GitLab according to each stage (Verify, Package, Release)
![gitlab_cicd_workflow](/images/gitlab/gitlab_cicd_workflow.png)

### Verify Stage
In this stage, you will automatically build and test your application with Continuous Integration and analyse your source code quality with a feature called `GitLab Code Quality`. It can determine the browser and server performance that impact code changes using browser performance and server load performance testing (both are pipeline features built into [GitLab Browser Performance Testing](https://docs.gitlab.com/ee/ci/testing/browser_performance_testing.html) and [Load Performance Testing](https://docs.gitlab.com/ee/ci/testing/load_performance_testing.html)).

### Package Stage
In this stage you can store Docker images with GitLab Container Registry and store packages with the [GitLab Package Registry](https://docs.gitlab.com/ee/user/packages/container_registry/index.html).

### Release Stage
In this stage, Continuous Deployment, automatically deploys your app to production. GitLab supports the deployment of static websites with [GitLab Pages](https://docs.gitlab.com/ee/user/project/pages/index.html).
GitLab ships features to only a portion of your pods and let a percentage of your user base to visit the temporary deployed feature with Canary Deployments.
Other features include:
- Deploy your features behind [Feature Flags](https://docs.gitlab.com/ee/operations/feature_flags.html).
- Add release notes to any Git tag with [GitLab Releases](https://docs.gitlab.com/ee/user/project/releases/index.html).
- View of the current health an status of each CI environment running on Kubernetes with [Deploy Boards](https://docs.gitlab.com/ee/user/project/deploy_boards.html).
- Deploy your application to a production environment in a Kubernetes Cluster with [Auto Deploy](https://docs.gitlab.com/ee/topics/autodevops/stages.html#auto-deploy).

## GitLab Runner
### Architecture
![gitlab_runner_architecture](/images/gitlab/gitlab_runner_architecture.png)

A GitLab Runner is a lightweight, highly-scalable agent that picks up a CI job through the coordinator API of GitLab CI/CD, runs the job, and sends the result back to the GitLab instances.
- It can be installed on any platform where you build Go binaries, including Linux, macOS, Windows, FreeBSD, Bare Metal, your workstation, and Docker.
- It can test any programming language including .NET, Java, Python, C, PHP and others.
- It is typically created by the administrator and made visible in the GitLab UI for certain tasks and jobs.

### Types of Runners
In the GitLab UI there are 3 types of runners, based on who you want to have access:
1. [Shared runners](https://docs.gitlab.com/ee/ci/runners/runners_scope.html#shared-runners) are available to all groups and projects in a GitLab instance.
2. [Specific/projects runners](https://docs.gitlab.com/ee/ci/runners/runners_scope.html#project-runners) are associated with specific projects. Typically, specific runners are used for one project at a time.
3. [Group runners](https://docs.gitlab.com/ee/ci/runners/runners_scope.html#group-runners) are available to all projects and subgroups in a group.

#### Shared Runners
Shared runners are available to every project in a GitLab instance. Use shared runners when you have multiple jobs with similar requirements. Rather than having multiple runners idling for many projects, you can have a few runners that handle multiple projects.
Shared runners process jobs by using a fair usage queue. This queue prevents projects from creating hundreds of jobs and using all available shared runner resources. The fair usage queue algorithm assigns jobs based on the projects that have the fewest number of jobs already running on shared runners. They are also typically managed by GitLab Admins.

#### Specific Runners
Specific runners are used when you want to use runners for specific projects. For example, when you have jobs with specific requirements, like a deploy job that requires credentials, or projects with a lot of CI activity that can benefit from being separate from other runners.
You can set up a specific runner to be used by multiple projects. Specific runners must be enabled for each project explicitly.

### Tagging Runners
An easy way you can make sure your shared runners will only run the jobs they are equipped to run is to tag each runner for the types of jobs it can handle.
- Tags allow us to select a runner from a pooling algorithm. A tagged runner will only be selected to run jobs tagged with a specific tag. Runners should be setup to run all the different types of jobs that it may encounter on the projects it’s shared over. This would be problematic for large amounts of projects, if it weren’t for tags. GitLab CI tags are not the same as Git tags. GitLab CI tags are associated with runners. Git tags are associated with commits.
- Untagged runners can be used on any pipeline that does not have tags attached to it, but these can be easily overwhelmed if you have several pipelines running off of them.

### Protected vs. Non-Protected Runners
GitLab allows you to protect runners so they don’t reveal sensitive information. When a runner is protected, the runner picks jobs created on protected branches or protected tags only, and ignores other jobs. GitLab recommends protecting the runner if it contains deploy keys or other sensitive capabilities.

## Common and Uncommon Executors Used at GitLab
GitLab Runner implements a number of executors that can be used to run your builds in different scenarios. The Executer provides the dependencies for your builds need to be installed in the environment required for a GitLab Runner.

### Shell
The Shell executor is a simple executor that you use to execute builds locally on that machine where GitLab Runner is installed. It supports all systems on which the Runner can be installed. That means that it's possible to use scrips generated for Bash, PowerShell Core, Windows PowerShell, and Windows Batch (deprecated).

### Docker
The Docker executor when used with GitLab CI, connects to Docker Engine and runs each build in a separate and isolated container using the predefined image that is setup in .gitlab-ci.yml and in accordance in config.toml. This is the most common executor used.

### Docker Machine
The Docker Machine is a special version of the Docker executor with support for auto-scaling. It works like the normal Docker executor but with build hosts created on demand by Docker Machine. This executor is typical in cloud deployments.

### Kubernetes
The Kubernetes executor allows you to use an existing Kubernetes cluster for your builds. The executor will call the Kubernetes cluster API and create a new Pod (with a build container and services containers) for each GitLab CI job.

### Less Common
#### Virtual Box
It allows you to use VirtualBox's virtualization to provide a clean build environment for every build. This executor supports all systems that can be run on VirtualBox. The only requirement is that the virtual machine exposes its SSH server and provide a bash-compatible shell.

#### Parallels
Typically used as a platform on top of virtual box.

#### SSH
This is a simple executor that allows you to execute builds on a remote machine by executing commands over SSH.

### Summary
While the two most common executors used today are Docker and Kubernetes, it's important to know that GitLab supports a wide range of executors. Take a look at [Selecting the executor](https://docs.gitlab.com/runner/executors/#selecting-the-executor) for more details.

# Anatomy of a Pipeline
## Pipelines
### DAG Pipeline
If efficiency is important to you and you want everything to run as quickly as possible, you can use Directed Acyclic Graphs (DAG). Use the needs keyword to define dependency relationships between your jobs. When GitLab knows the relationships between your jobs, it can run everything as fast as possible, and even skips into subsequent stages when possible.
![dag_pipelines](/images/gitlab/gitlab_dag_pipeline.png)

### Parent-Child Pipelines
A pipeline can trigger a set of concurrently running child pipelines, but within the same project:
- Child pipelines still execute each of their jobs according to a stage sequence, but would be free to continue forward through their stages without waiting for unrelated jobs in the parent pipeline to finish.
- The configuration is split up into smaller child pipeline configurations, which are easier to understand. This reduces the cognitive load to understand the overall configuration.
- Imports are done at the child pipeline level, reducing the likelihood of collisions.
- Each pipeline has only relevant steps, making it easier to understand what’s going on.

### Child Pipelines
Child pipelines work well with other GitLab CI/CD features:

- Use the only/except or rules variables to trigger pipelines only when certain files change. This is useful for monorepos, for example.
- Since the parent pipeline in `.gitlab-ci.yml` and the child pipeline run as normal pipelines, they can have their own behaviors and sequencing in relation to triggers.

### Multi-Project Pipelines
You can set up GitLab CI/CD across multiple projects, so that a pipeline in one project can trigger a pipeline in another project.

Multi-project pipelines are useful for larger products that require cross-project inter-dependencies, such as those adopting a microservices architecture.

When you configure GitLab CI/CD for your project, you can visualize the stages of your jobs on a pipeline graph.
![multi-project_pipeline](/images/gitlab/gitlab_multi-project_pipeline.jpeg)

## GitLab Pipeline Graph
The pipeline graph allows you to see the status of each job and stage as defined in your pipeline.

### Job
Jobs define what we want to accomplish in our pipeline executed by Runners and are executed in stages. Jobs in each stage are executed in parallel and if all jobs in a stage succeed, the pipeline moves on to the next stage. If one job in a stage fails, the next stage is not (usually) executed.

### Stages
- Stages define when and how to run jobs.
- Stages that run tests after stages that compile the code.

### Environments
Environments are where we deploy to, for example, staging or production. These are specified in the jobs within the ci.yml file.

### Typical Pipeline Defaults
By default, a pipeline will have three stages: build, test, and deploy. Having default stages does not mean the users have to create a job for each stages, for stages with no jobs will simply be ignored.

## Stages
Stages are used to separate jobs into logical sections. A stage is defined per-job and relies on stages, which is defined globally. Use stages to define which stage a job runs in, and jobs of the same stage are executed in parallel (subject to certain conditions).

Stages separate jobs into logical sections while Jobs perform the actual tasks.
### Typical Stages in a pipeline
#### Build
Source code and other dependencies are combined and built.
#### Test
Automated tests are run to validate the code and behavior.
#### Review
The code is put through review apps for peer review and final approvals.
#### Deploy
The final product is deployed to a designated environment.

## Jobs and Scripts
A pipeline configuration begins with jobs. Jobs are the most fundamental element of a .gitlab-ci.yml file. Jobs are:

- Defined with constraints stating under what conditions they should be executed.
- Top-level elements with an arbitrary name and must contain at least the script clause.
- Not limited in how many can be defined.

The `script` keyword is the only required keyword that a job needs. It’s a shell script that is executed by the runner.

## Use of Images on a Pipeline
- An image is used to specify a Docker image to use for the job.
- The image keyword can be used in a global scope or inside your job definition as a local image or in the local scope.
- The image tag will be right at the top of the .yml file and will be used globally for every job unless a job has its own specific image tag.

### Images are pulled from DockerHub by default
```bash
# Use of a public image:
image: ruby:2.3
```
### Images stored in the GitLab Container Registry
```bash
# Use of a custom image:
image: 'registry.gitlab.com/gitlab-org/ci-sample:latest'
```
## Other Important Parameters
### Environment
The environment keyword defines where the app is deployed and is defined by 3 parts: name, url, and when.
- Static environments have static names, like staging or production.
- Dynamic environments have dynamic names. Dynamic environments are a fundamental part of [Review apps](https://docs.gitlab.com/ee/ci/review_apps/index.html)
```yaml
environment:
  name: prod
  url: http://$CI_PROJECT_NAME.$KUBE_DOMAIN
when: manual
```
### Rules
The rules keyword can be used to include or exclude jobs in pipelines. It is a new and improved "only & except" keyword. It does many of the same things that only & except does, but we can get more granular with it.

Rules are evaluated in order until the first match. When matched, the job is either included or excluded from the pipeline, depending on the configuration. If included, the job also has certain attributes added to it.

Rules replaces only/except and can’t be used in conjunction with it. If you attempt to use both keywords in the same job, the job will error out and fail.
```yaml
pseudo-deploy:
  stage: deploy
  script:
  - command deploy_review
  rules:
	- if: '$CI_COMMIT_REF_NAME == "main"'
		when: never
	- when: always
	envrionment:
		name: review
		url: http://$CI_PROJECT_NAME-review.$KUBE_DOMAIN
```
### Cache and Artifacts
**cache**: For storing project dependencies.
Caches can increase the speed of a given job in subsequent pipelines. You can store downloaded dependencies so that they don’t have to be fetched from the internet again.

**artifacts**: Use for stage results that are passed between stages.
Artifacts are files that are generated by a job so they can be stored and uploaded. You can fetch and use artifacts in jobs in later stages of the same pipeline. You can’t create an artifact in a job in one stage, and use this artifact in a different job in the same stage.
```yaml
cache:
  paths:
    - binary/
    - .config
```

### Tags
In GitLab we use tags to select a specific runner from the list of all runners that are available for the project. When you register a runner, you can specify the runner’s tags, for example ruby, postgres, development. GitLab CI tags are not the same as Git tags. GitLab CI tags are associated with runners. Git tags are associated with marking a specific commit.

In the example, it shows that the job will only execute on runners that have the ‘ruby’ and ‘test’ tags.
```yaml
job-name:
  tags:
    - ruby
    - test
```
# CI/CD Parameters and Variables
## Common Job Keywords
### Keyword Reference
[Keyword Reference](https://docs.gitlab.com/ee/ci/yaml/) for the gitlab-ci.yml file
### Dependencies
Dependencies restrict which artifacts are passed to a specific job by providing a list of jobs to fetch artifacts from selectively. They are defined in the job and pass a list of all previous jobs the artifacts should be downloaded from. See below for an example YML with dependencies. 
```yaml
build:osx:
  stage: build
  script: make build:osx
  artifacts:
    paths:
      - binaries/

build:linux:
  stage: build
  script: make build:linux
  artifacts:
    paths:
      - binaries/

test:osx:
  stage: test
  script: make test:osx
  dependencies:
    - build:osx

test:linux:
  stage: test
  script: make test:linux
  dependencies:
    - build:linux

deploy:
  stage: deploy
  script: make deploy
```
### Pipeline Editor
[Pipeline Editor](https://about.gitlab.com/blog/2021/02/22/pipeline-editor-overview/)
[![Pipeline Editor](http://img.youtube.com/vi/MQpSyvMpsHA/0.jpg)](http://www.youtube.com/watch?v=MQpSyvMpsHAE)
### Needs

### Parallel
### Trigger

## Job Policy Patterns
### Why are jobs so important
### Use the pipeline graph
### Troubleshooting Job Failures
### Job Severity
### Organizing Your Pipeline
### Syntax for Grouping Jobs
### Jobs in Pending State
### The Basics of CI

## Registries, Deployments, and Security Scanning
### Package Types
### GitLab Container Registry
### CI/CD Variables
### GitLab Security Scanning
### Security Trends
### GitLab Security Scanning Tools
### Security with DevOps
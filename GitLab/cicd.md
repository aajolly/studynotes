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
The Docker executor when used with GitLab CI, connects to Docker Engine and runs each build in a separate and isolated container using the predefined image that is setup in .gitlab-ci.yml and in accordance in config.toml.

# GitLab Security Concepts
## Security in the DevSecOps (SDLC) Lifecycle
The DevSecOps lifecycle can be roughly split into two phases:
- **Development** - The "left side" of the lifecycle: The planning, defining, and writing of software
- **Operations** - The "right side" of the lifecycle: Deploying and actually running the software

The **Secure Stage** spans across the DevSecOps Lifecycle. GitLab offers tooling to to enforce a security posture at every stage of the software development lifecycle.
![gitlab_sdlc](/images/gitlab/gitlab_sdlc.png)

GitLab's security scanners enable developers to "shift security left," and start implementing security practices during the early development stages and throughout the DevSecOps Lifecycle.
| Security Scanners  | Security Support Features |
| ------------------ | ------------------------- |
| <ul><li>SAST</li><li>Secret Detection</li></ul><ul><li>DAST</li><li>Dependency Scanning</li></ul><ul><li>Container Scanning</li><li>Fuzz Testing</li></ul><ul><li>IaC Scanning</li></ul> | <ul><li>Security Reports</li><li>Vulnerability Management</li></ul><ul><li>Policies</li></ul> |

**SAST**: Static Application Security Testing looks for known problems in source code (e.g., unvalidated user input leads to command injection)

**Secret Detection**: Secret detection looks for hard-coded secrets in source code (e.g., passwords)

**DAST**: Dynamic Application Security Testing passively or actively calls a web app or an API to look for security vulnerabilities

**IaC Testing**: Infrastructure-as-Code testing scans IaC configuration files (e.g., Ansible, Terraform) for known security vulnerabilities

**Dependency Scanning**: Dependency scanning looks at project’s dependencies to see if there are known vulnerabilities with those versions (e.g., third-party YAML-parsing library)

**Container Scanning**: Container Scanning looks at your project's Docker images and scans for known vulnerabilities

**Fuzz Testing**: Fuzz testing sends random input to your functions, trying to cause unexpected problems (e.g., 200-character password or Klingon characters in Unicode)

**Security Reports**: Security reports show security vulnerabilities in different places and different ways

**Vulnerability Management**: Vulnerability Management lets you view problems in several places within GL and accept/dismiss/mark for action

**Security Policies**: Policies provide security teams a way to require scans of their choice to be run whenever a project pipeline runs according to the configuration specified

## Security Scanning Workflow
GitLab provides tools to detect and remediate problems during development as well as after deployment. The security scanners are offered as docker images and run as CI/CD jobs.
![gitlab_security_scanning_workflow](/images/gitlab/gitlab_security_scan_workflow.png)

1. **Tested on Commit**: Every piece of code is tested upon commit through GitLab CI and the results are documented in the pipeline report.

2. **On to CD**: The piece of code is merged to main and deployed using CD.

3. **DAST Scanning**: At the Review app stage, DAST scanning occurs. After DAST creates its report, GitLab evaluates it for discovered vulnerabilities between the source and target branches. Relevant findings are noted in the merge request.

4. **Remediation**: The developer can remediate vulnerabilities immediately while they are still working in that code, or the developer can create an issue with one click.

5. **Resolution**: The security dashboard displays a roll-up of the remaining vulnerabilities that the developer did not resolve on their own. The security team can choose to dismiss them or create a new issue to resolve.

Each of the GitLab application security tools is relevant to specific stages of the feature development workflow.
![gitlab_scanners](/images/gitlab/gitlab_scanners.png)

## Scanners Vary By Language
1. GitLab supports different computer languages for different scanner types. For example, SAST supports quite a few different languages.
2. GitLab automatically detects computer languages in your project through trigger files, specific files or extensions which alert the scanner to what language is being used. Once the language(s) are detected, GitLab runs only the version of the scanners that are relevant to that language. For example if you're using SAST, once the scanner detects the extension `*.py`, the python analyzers will activate. Additionally, the scanner will also look for specific files typically included when using a specific language such as the requirements.txt file in Python, which is used to store dependencies.
3. It's OK to have multiple languages in your pipeline; the scanners will be able to detect each language automatically.

## Common Details for all scanners
| Commonality | What you should know |
| ----------- | -------------------- |
| Packaged as Docker images | Your pipeline will automatically download the docker images for all of your scanners. You do not need to action on this functionality, but just be aware that, depending on how complicated the code is, downloading the docker image will impact how long the scan takes. |
| Require GitLab Runner with Docker or Kubernetes executor	| Typically runners are installed and configured by the GitLab Admin and will most likely not be something that you will need to worry about. |
| Updated monthly (at least) | These scanners are updated at least monthly, some more often than that. |
| Most findings include links to outside vulnerability databases | Most of the vulnerability reports include links to outside vulnerability databases (like the CVE) that provide you with additional information on the vulnerabilities found. This information can help you to understand how important the different vulnerabilities are and some databases will provide you with steps on how to fix those vulnerabilities. |
| Finding vulnerabilities does not stop the pipeline | By default, finding security vulnerabilities does not stop the pipeline. GitLab empowers users to identify which vulnerabilities you want to fix and which vulnerabilities that want to ignore. |
| Offline Environments | It’s possible to run most of the GitLab security scanners when not connected to the internet. |
| Enabled by Auto DevOps | Auto DevOps is an optional GitLab feature that detects your programming language and uses CI/CD templates to create and run default pipelines to build and test your application. Be aware that enabling Auto Devops will enable all security scanners which will slow down your pipeline. |

You can integrate most third-party security scanners into GitLab CI/CD pipelines.

- Any scanner that can be triggered within the command line can be added to your GitLab pipeline.
- Integrating a security scanner into GitLab consists of providing you with a CI job definition that you can add to your CI configuration files to scan your GitLab projects.
- For example, you can create a job in your CI/CD pipeline that runs the same command that you would have typed manually and that will trigger the scanner.
- This CI job should then output its results in a GitLab-specified format. These results are then automatically presented in various places in GitLab, such as the Pipeline view, merge request widget, and Security Dashboard.
- The scanning job is usually based on a Docker image that contains the scanner and all its dependencies in a self-contained environment.

## SAST
SAST (Static Application Security Testing) scans your source code for vulnerabilities without actually running the code.
- If you’re using GitLab CI/CD, you can use Static Application Security Testing (SAST) to check your source code for known vulnerabilities. The analyzers output JSON-formatted reports as job artifacts.
- SAST runs in the test stage, which is available by default. If you redefine the stages in the `.gitlab-ci.yml` file, the test stage is required.
- To run SAST jobs, by default, you need GitLab Runner with the docker or kubernetes executor. If you’re using the shared runners on GitLab.com, this is enabled by default.

### Steps for using SAST
#### 1. Enable and Configure SAST
There are two ways of enabling SAST

- **Manually**: Edit the `.gitlab-ci.yml` file. Ensure the test stage is defined. Include the SAST template
```yaml
stages:
    - test

include:
    - template: Security/SAST.gitlab-ci.yml
```

- **GitLab Project UI**: Navigate to the Security & Compliance > Configuration. Static Application Security Testing > Enable
Merge auto-generated MR to edit `.gitlab-ci.yml`

When enabling SAST, you are including the file `SAST.gitlab-ci.yml` and this file is going to define the jobs in your pipeline that perform SAST scanning.

> SAST jobs are set up to run only in a test stage. Before running your CICD pipeline, make sure that you've defined a test stage. 

Once you've enabled SAST, you may want to configure the scanner's behavior. 

| Sample Customizations | Why would I want to do that? |
| ---------- | -------- |
| Set severity thresholds | You want SAST to flag only critical or high severity issues based on your organization's security policy requirements. |
| Disable analyzers	| Your CICD pipeline is set up to potentially trigger more than one SAST analyzer but you only need a specific analyzer to run: For example, if your job is in Python, GitLab has two different Python analyzers for SAST, Bandit and Semgrep. If Bandit gives you all of the functionality you need, you can disable the Semgrep analyzer. |
| Pin analyzer versions	| If your security team has approved a specific analyzer version: For example, let's say that Semgrep Version 9 is approved, but Version 10 is not, you can configure the scanner to only use version 9. |

#### 2. Run Pipeline
#### 3. Review Findings
GitLab has three major vulnerability report types for different use cases and each report type will show you results of all eight scanners:
| Where? | What does it show? |
| ------ | ------------------ |
| Vulnerability report | Vulnerabilities on default branch |
| Pipeline details | Vulnerabilities on pipeline’s branch |
| Merge request | Vulnerabilities on pipeline’s branch but not on default branch, or vice versa (delta or diff) |

#### 4. Take Action
The three different reports that we covered provided us with vulnerabilities that were uncovered by the SAST scan. Now we want to take action on them or, rather, change the vulnerabilities status. 
There are four possible statuses for each vulnerability:
| Status | Definition |
| ------ | ---------- |
| Detected/Needs Triage	| The default state for a newly discovered vulnerability. Appears as “Needs triage” in the UI. |
| Confirmed | A user has seen this vulnerability and confirmed it to be accurate. |
| Dismissed | A user has seen this vulnerability and dismissed it because it is not accurate or otherwise not to be resolved. Dismissed vulnerabilities are ignored if detected in subsequent scans. |
| Resolved | The vulnerability has been fixed or is no longer present. Resolved vulnerabilities that are reintroduced and detected by subsequent scans have a new vulnerability record created. |

> When vulnerabilities are initially found and logged in the reports, their initial status is Needs Triage. 

To change the status of a vulnerability:

1. Navigate to Security and Compliance > Vulnerability Report
2. Select the vulnerabilities description
3. From the Status dropdown list, select a status, then select Change status. 
4. Optionally, at the bottom of the page, add a comment to the log entry

When dismissing a vulnerability, one of the following reasons must be chosen to clarify why it is being dismissed:
| Vulnerability Dismissal Reasons | Definition |
| --------- | -------- |
| Acceptable risk | The vulnerability is known, and has not been remediated or mitigated, but is considered to be an acceptable business risk. |
| False positive | An error in reporting in which a test result incorrectly indicates the presence of a vulnerability in a system when the vulnerability is not present. |
| Mitigating Control | A management, operational, or technical control (that is, safeguard or countermeasure) employed by an organization that provides equivalent or comparable protection for an information system. |
| Used in tests	| The finding is not a vulnerability because it is part of a test or is test data. |
| Not applicable | The vulnerability is known, and has not been remediated or mitigated, but is considered to be in a part of the application that will not be updated. |

GitLab has a workflow directly from the Vulnerability report to create a GitLab issue so that you can track progress around addressing uncovered vulnerabilities.

To Create a GitLab Issue:

1. Click the Details of the vulnerability with which you want to action. 
2. Click Create Issue. 
3. The issue will pre-populate with the information provided in the vulnerability report. Make any changes and click Create Issue.

> This functionality can also be integrated with [JIRA](https://docs.gitlab.com/ee/user/application_security/vulnerabilities/#create-a-jira-issue-for-a-vulnerability).

Once you've created an issue, you may want to resolve the vulnerability.

## Secret Detection
Secret Detection looks for secrets inside files in your projects repo. 

What do you mean by "secrets"?

- US Social Security numbers
- Private SSH keys
- Deploy keys
- etc.

While the Secret Detection scan is similar to Static Analysis, the scanner looks at any files in your directory, not just in source code. 

After you enable Secret Detection, scans run in a CI/CD job named secret_detection. The analyzers output JSON-formatted reports as job artifacts.

> Secret Detection runs in the test stage, which is available by default. If you redefine the stages in the .gitlab-ci.yml file, the test stage is required.

> To run Secret Detection jobs, by default, you need GitLab Runner with the docker or kubernetes executor. If you’re using the shared runners on GitLab.com, this is enabled by default.

> Secret Detection uses the open-source tool [GitLeaks](https://about.gitlab.com/handbook/security/gitleaks.html).

### Steps for using Secret Detection
#### 1. Enable and Configure Secret Detection
There are two ways of enabling Secret Detection (similar workflow to SAST):
- **Manually**: Edit the `.gitlab-ci.yml` file. Ensure the test stage is defined. Include the Secret Detection template
```yaml
stages:
    - test

include:
    - template: Security/Secret-Detection.gitlab-ci.yml
```
> When enabling Secret Detection, you are including the file Security/Secret-Detection.gitlab-ci.yml and this file is going to define the jobs in your pipeline that perform the Secret Detection scan.

> Note that Secret Detection jobs are set up to run only in a test stage. Before running your CICD pipeline, make sure that you've defined a test stage.

> After you enable Pipeline Secret Detection, scans run in a CI/CD job named `secret_detection`.

On you've enabled Secret Detection, you may want to configure the scanner's behavior.
| Sample Customizations | Why would I want to do that? |
| -------- | ------- |
| Enable historic mode | By default, Secret Detection scans only the current state of a Git repository.Any secrets contained in the repository’s history are not detected. To address this, Secret Detection can scan the Git repository’s full history. **Recommendation**:You should do a full history scan only once, after enabling Secret Detection. A full history can take a long time, especially for larger repositories with lengthy Git histories. After completing an initial full history scan, use only standard Secret Detection as part of your pipeline. |
| Define which commits to scan | You don't want to scan all historic commits, maybe the last two hundred. |
| Define which paths to exclude	| You might not need to scan certain paths, for example, a test directory that might be made up of fake data. |

> Unlike SAST, there are no options to configure Secret Detection via the GU, only through the .gitlab-ci.yml file. 

#### 2. Run Pipeline
#### 3. Review Findings
Secret Detection findings are in the same location as the SAST findings we reviewed earlier in the SAST section.

#### 4. Take Action
Same as SAST section.

## DAST
### Overview
- DAST (Dynamic Application Security Testing) scans a running web app for security vulnerabilities. Also known as Black Box Testing.

- Instead of looking at your source code, like SAST and Secret Detection does, Dynamic Analysis interacts with your code as it's running. DAST looks at web apps or REST API. For URLs, DAST starts at the home page, and then spiders your site. A tester using DAST examines an application when it is running and tries to hack it, just like an attacker would.

- SAST can be considered white box testing, meaning that it's able to look inside your application to look at the code. DAST is black box testing, meaning that the application is tested from the outside.

- DAST requires a deployed application to be available to scan. 

- To run DAST, just as with Static and Secret Detection, by default, you need GitLab Runner with the docker or kubernetes executor. If you’re using the shared runners on GitLab.com, this is enabled by default.

- There are two different ways that DAST approaches scanning: 
    - **Passive mode** is run by default - It sends normal requests to the web app and analyzes responses.
    - **Active mode** simulates malicious behavior coming at your site, trying to break it.

- GitLab provides the following DAST analyzers, one or more of which may be useful depending on the kind of application you’re testing. 

- For scanning websites, use one of:
    - The [DAST proxy-based analyzer](https://docs.gitlab.com/ee/user/application_security/dast/proxy-based.html) for scanning traditional applications serving simple HTML. The proxy-based analyzer can be run automatically or on-demand.
    - The [DAST browser-based analyzer](https://docs.gitlab.com/ee/user/application_security/dast/browser_based.html) for scanning applications that make heavy use of JavaScript. This includes single page web applications.

- For scanning APIs, use:
    - The [DAST API analyzer](https://docs.gitlab.com/ee/user/application_security/dast_api/index.html) for scanning web APIs. Web API technologies such as GraphQL, REST, and SOAP are supported.

> DAST uses the open-source tool [OWASP Zed Attack Proxy](https://owasp.org/).

### Steps for using DAST
#### 1. Enable and configure DAST
There are two ways of enabling DAST (similar to SAST and Secret Detection but note the differences below).
- **Manually**: Edit the `.gitlab-ci.yml` file. Include the DAST template
```yaml
stages:
    - dast

include:
    - template: DAST.gitlab-ci.yml
```
- **GitLab Project UI**
    1. Navigate to Security & Compliance > Configuration
    2. DAST > Enable
    3. Generate CI/CD configuration code fragment
    4. Add fragment to .gitlab-ci.yml

> Note that there is no need to include a test stage as with SAST and Secret Detection. DAST scans deployed code and runs in its own Stage.

> Also note that the template is not in Security/dir, so the template format is slightly different than SAST and Secret Detection. For details please take a look [here](https://docs.gitlab.com/ee/user/application_security/dast/).

Dynamic Analysis configuration is more complicated than most other scanners.
| Sample Customizations | Why would I want to do that? |
| ------- | ------- |
| Choose to have DAST scan your site "actively" | By default, DAST will perform a passive scan, but you want Dynamic Analysis to actively attack your website looking for vulnerabilities. |
| Scan a URL, API, or both | You need to define where the scan should point - a REST API or URL. |
| Target URL | If scanning a URL, you need to provide the URL(s) to be scanned. |
| Provide authentication credentials | If you want DAST to scan a web app that requires authentication, such as a password, you need to provide what that authentication is so that Dynamic Analysis can log into your site. |
| Scanner Profile | In order to create a new DAST scan, you need to have at least one completed scanner profile. The scanner profile configures how the scanner should work - Do you want Active or Passive mode? When should the scanner timeout? Do you want to show debug messages? |
| Site Profile | In order to create a new scan, you need to have at least one completed site profile. The site profile defines what site or REST API you want DAST to scan. You can also define any URLs that you want to exclude and if the site requires authentication, you can provide the authentication details. |

#### 2. Run Pipeline
#### 3. Review Findings
Dynamic Analysis findings are in the same locations as the SAST and Secret Detection findings we reviewed earlier.
#### 4. Take Action
The workflow for dismissing vulnerabilities and and creating issues to track progress is identical to that of Static Analysis and Secret Detection.

## Infrastructure as Code Scanning
### Overview
- IaC (Infrastructure as Code) Scanning finds known security vulnerabilities in IaC configuration files. 

- IaC scans IaC configuration files, similar to a SAST scan, meaning that it scans the configuration files without actually running them. IaC Scanning supports configuration files for Terraform, Ansible, Kubernetes, etc.

- IaC Scanning runs in the test stage, which is available by default. If you redefine the stages in the .gitlab-ci.yml file, the test stage is required.

- It is recommended to have a minimum of 4 GB RAM on runners executing IaC scanning to ensure consistent performance ensure consistent performance.

- To run IaC Scanning jobs, by default, you need GitLab Runner with the docker or kubernetes executor. If you’re using the shared runners on GitLab.com, this is enabled by default.

- IaC Scanning supports a variety of IaC configuration files. The IaC security scanners also feature automatic language detection which works even for mixed-language projects. If any supported configuration files are detected in project source code we automatically run the appropriate IaC analyzers. 

- IaC Scanning uses the open-source tool [KICS](https://kics.io/) to perform static analysis on IaC files.

### Steps for using IaC Scanning
#### 1. Enable and configure IaC Scanning
There are two ways of enabling IaC Scanning
- **Manually**: Edit the `.gitlab-ci.yml` file. Ensure the test stage is defined. Include the IaC template
```yaml
stages:
    - test

include:
    - template: Security/SAST-IaC.gitlab-ci.yml
```
- **GitLab Project UI**
    1. Navigate to Security & Compliance > Configuration
    2. Infrastructure as Code (IaC) Scanning > Configure with a merge request
    3. Merge auto-generated MR to edit .gitlab-ci.yml

> When enabling IaC Scanning, you are including the file `SAST-IaC.gitlab-ci.yml` and this file is going to define the jobs in your pipeline that perform SAST scanning.

> Note that IaC Scanning jobs are set up to run only in a test stage. Before running your CICD pipeline, make sure that you've defined a test stage. [Documentation](https://docs.gitlab.com/ee/user/application_security/iac_scanning/#enable-iac-scanning-via-an-automatic-merge-request)

Once you've enabled IaC Scanning, you may want to configure the scanner's behavior. 

Unlike the other security scanners, currently, IaC Scanning only has one configuration setting that you may want leverage depending on the security standards set by your organization.
| Sample Customizations | Why would I want to do that? |
| ------- | ------ |
| Use a scanner based on a FIPS (Federal Information Processing Standards) image | FIPS are standards and guidelines for federal computer systems that are developed by the National Institute of Standards and Technology (NIST) in accordance with the Federal Information Security Management Act (FISMA) and approved by the Secretary of Commerce. These standards and guidelines are developed when there are no acceptable industry standards or solutions for a particular government requirement. 

Although FIPS are developed for use by the federal government, many in the private sector voluntarily use these standards. |

```yaml
variables:
    SAST_IMAGE_SUFFIX: '-fips'

include:
    - template: Security/SAST-IaC.gitlab-ci.yml
```
The workflow for enabling IaC Scanning is identical to SAST and DAST, which we saw previously. For a refresher, watch the video below.

## Dependency Scanning
### Overview
Dependency Scanning finds known security vulnerabilities in project dependencies and suggests alternative project dependencies that do not contain those same vulnerabilities. 

What do you mean by "known"?

Dependency Scanning only finds problems that have been discovered and catalogued by security experts in the GitLab Advisory Database.

- The scanner looks in the GitLab Advisory Database to see if a package and/or version of your dependencies has any documented vulnerabilities.
- Dependency Scanning does not look at source code, unlike Static Analysis and Secret Detection. 
- Dependency Scanning does not detect vulnerabilities that you introduced yourself (that's what Static Analysis is for!).
- The Dependency Scanner scans recursively, meaning that it looks at dependencies' dependencies.

- When developing software, it's common to use third-party dependencies and open-source libraries. However, some of those dependencies might have security vulnerabilities that you are unaware of. For example, dependency scanning lets you know if your application uses an external (open source) library that is known to be vulnerable. You can then take action to protect your application.

- Dependency Scanning is often considered part of **Software Composition Analysis** (SCA). SCA can contain aspects of inspecting the items your code uses. These items typically include application and system dependencies that are almost always imported from external sources, rather than sourced from items you wrote yourself.

The Dependency Scanner uses package managers to discover project dependencies, which vary by language. 

Dependency Scanning uses the [Gemnasium analyzer](https://docs.gitlab.com/ee/user/application_security/dependency_scanning/analyzers.html), an open-source tool, to automatically detect the languages used in the repository. 

All analyzers matching the detected languages are run automatically.

### Steps for using Dependency Scanning
#### 1. Enable and configure Dependency Scanning
Similarly to the previous scanners, there are two ways of enabling Dependency Scanning.
- **Manually**: Edit the `.gitlab-ci.yml` file. Ensure the test stage is defined. Include the Dependency Scanning template.
```yaml
stages:
    - test

include:
    - template: Security/Dependency-Scanning.gitlab-ci.yml
```

- **GitLab Project UI**
    1. Navigate to Security & Compliance > Configuration
    2. Dependency Scanning > Configure via Merge Request
    3. Merge auto-generated MR to edit `.gitlab-ci.yml`

> When enabling Secret Detection, you are including the file `Security/Dependency-Scanning.gitlab-ci.yml` and this file is going to define the jobs in your pipeline that perform the Dependency Scan.

> Note that Dependency Scanning jobs are set up to run only in a test stage. Before running your CICD pipeline, make sure that you've defined a test stage. [Documentation](https://docs.gitlab.com/ee/user/application_security/dependency_scanning/)

Once you've enabled Dependency Scanning, you may want to configure the scanner's behavior.

| Sample Customizations | Why would I want to do that? |
| --------- | -------- |
| Exclude analyzers	| Dependency Scanning uses the Gemnasium analyzer by default but some languages have different analyzers. Similar to disabling analyzers in SAST and Secret Detection, if Gemnasium gives you the functionality that you need, you can exclude the other analyzers to optimize your scan. |
| Automatic Remediation	| When Dependency Scanning identifies a specific package version that has a known vulnerability, automatic remediation will automatically upgrade your dependency to a version without that vulnerability. However, you may not want the scanner to automatically make these updates and so you can toggle this feature on and off. Note that automatic remediation is situation-specific and may not always been an option. |
| Depth of directories to scan | Allows you to define how many levels deep that the analyzer should search for supported files to scan. You may want to configure this feature to speed up analysis. |

You can find Dependency Scanning reports in the same three locations where we viewed our SAST, DAST, and Secret Detection scans previously. 

## Container Scanning
### Overview
- Container Scanning finds known security vulnerabilities in packages in Docker images or Docker containers in the project's container registry.

What do you mean by "known"?

Much like Dependency Scanning, Container Scanning only finds problems that have been discovered and catalogued by the open-sourced tools (listed below) and security experts in the [Vulnerabilities Database](https://docs.gitlab.com/ee/user/application_security/container_scanning/#vulnerabilities-database). 

- The scanner looks in the [Vulnerabilities Database](https://docs.gitlab.com/ee/user/application_security/container_scanning/#vulnerabilities-database) to see if a package in your Docker images has any documented vulnerabilities. These packages include:
    - Default or Dockerfile-installed packages
    - OS packages - [View supported OS images by scanner](https://docs.gitlab.com/ee/user/application_security/container_scanning/#supported-distributions)
    - Language packages (optionally)

- Similar to Dependency Scanning:
    - Container Scanning does not look at source code, unlike Static Analysis and Secret Detection.
    - Container Scanning does not detect vulnerabilities that you introduced yourself (that's what Static Analysis is for!).

- Container Scanning is often considered part of **Software Composition Analysis** (SCA). SCA can contain aspects of inspecting the items your code uses. These items typically include application and system dependencies that are almost always imported from external sources, rather than sourced from items you wrote yourself.

- Container Scanning runs in the test stage, which is available by default. If you redefine the stages in the .gitlab-ci.yml file, the test stage is required.

- To run Container Scanning jobs, by default, you need GitLab Runner with the docker or kubernetes executor. If you’re using the shared runners on GitLab.com, this is enabled by default.

> The Container Scanner uses open-source tools to scan the project's container registry: `Trivy`, `Grype`

> The languages supported depend on the scanner used. [Documentation](https://docs.gitlab.com/ee/user/application_security/container_scanning/#change-scanners)

### Steps for using Container Scanning
There are two ways of enabling Container Scanning:
- **Manually**: Edit the `.gitlab-ci.yml` file. Ensure the test stage is defined. Include the Container Scanning template.
```yaml
stages:
    - test
include:
    - template: Security/Container-Scanning.gitlab-ci.yml
```
> When enabling Container Scanning, you are including the file `Security/Container-Scanning.gitlab-ci.yml` and this file is going to define the jobs in your pipeline that perform the Dependency Scan.

> Note that Container Scanning jobs are set up to run only in a test stage. Before running your CICD pipeline, make sure that you've defined a test stage.

Once you've enabled Container Scanning, you may want to configure the scanner's behavior. 

| Sample Customizations | Why would I want to do that? |
| -------- | -------- |
| Scan a remote image | You want to scan an image outside of your project's container registry, like an image on Docker Hub or somewhere else. |
| Provide remote container registry credentials	| If you need Container Scanning to log into a remote registry, you can provide the appropriate credentials. |
| Enable language package scanning | Configure how and whether the scan reports findings related to programming languages |

> Unlike SAST, there are no options to configure Container Scanning via the GUI, only through the `.gitlab-ci.yml` file. [Documentation](https://docs.gitlab.com/ee/user/application_security/container_scanning/#configuration)

![gitlab_container_scanning_example](/images/gitlab/gitlab_container_scanning_example.png)

1. **The docker image template**
In the build stage, the image property uses the latest docker image from the Docker hub. 
2. **CI/CD variables**
The variable IMAGE here is composed of three other variables, giving flexibility to the pipeline. Also, this IMAGE variable refers to the image that will be built and scanned by the container scan job.
3. **Script property**
From the script property, docker commands are run to login, build, and push the docker image to the Container Registry.
4. **Container Scan included**
With a simple include property, container scanning can be brought into the pipeline using a template.

You can find Container Scanning reports in the same three locations where we viewed our SAST, DAST, and Secret Detection scans previously.

## Fuzz Testing
### Overview
Fuzz testing finds problems that QA teams can easily miss by throwing random inputs at your app in an effort to cause unexpected behavior.

There are two ways of running Fuzz Testing:
| | Coverage-Guided Fuzzing | Web API Fuzzing |
| ------- | ----- | ---- |
| What is it? | Coverage-guided fuzzing sends random inputs to an instrumented version of your application in an effort to cause unexpected behavior. Such behavior indicates a bug that you should address. You can add coverage-guided fuzz testing to your pipelines. This helps you discover bugs and potential security issues that other QA processes may miss. | Web API fuzzing performs fuzz testing of API operation parameters. Fuzz testing sets operation parameters to unexpected values in an effort to cause unexpected behavior and errors in the API backend. This helps you discover bugs and potential security issues that other QA processes may miss. |
| Fuzzing of | Instrumented (non-production) version of application	| API operation parameters |
| Methodology | Sends random inputs to an instrumented version of your application | Sets operation parameters to unexpected values |
| Fuzz Test Yields | Cause unexpected behavior and errors in the API backend | Cause unexpected behavior, such as a crash |
| Requirements | Coverage-guided fuzz testing runs in the fuzz stage. If you redefine the stages in the .gitlab-ci.yml file, the fuzz stage is required. If your application is not written in Go, provide a Docker image using the matching fuzzing engine. | Web API fuzzing runs in the fuzz stage of the CI/CD pipeline. To ensure API fuzzing scans the latest code, your CI/CD pipeline should deploy changes to a test environment in one of the stages preceding the fuzz stage. |
| Supported Languages and Frameworks | Please note that the supported fuzzing engines and languages are more limited than the scanners that we discussed previously. | One of the following web API types: <ul><li>REST API</li><li>SOAP</li></ul><ul><li>GraphQL</li><li>Form bodies, JSON, or XML</li></ul> One of the following assets to provide APIs to test: <ul><li>OpenAPI v2 or v3 API definition</li><li>HTTP Archive (HAR) of API requests to test</li></ul><ul><li>Postman Collection v2.0 or v2.1</li></ul> |

![gitlab_fuzz_testing](/images/gitlab/gitlab_fuzz_testing.png)

### Steps for using Fuzz Testing
#### 1. Enable and configure a Fuzz Test for a single function
Coverage-guided fuzz testing can only be enabled manually via the .gitlab-ci.yml file though [you can manage corpus files used as seed inputs using the GUI](https://docs.gitlab.com/ee/user/application_security/coverage_fuzzing/#seed-corpus).
- **Manually**: Edit the `.gitlab-ci.yml` file. Ensure the fuzz stage is defined. Include the Coverage-Guided Fuzz Testing template
> Note that unlike most of the other security scanners, the template is not in the Security/ directory and so the template syntax differs slightly than what we have seen previously when enabling security scanners
```yaml
stages:
    - fuzz:
include:
    - template Coverage-Fuzzing.gitlab-ci.yml
my_fuzz_target:
    extends: .fuzz_base
    script:
    # Build your fuzz target binary in these steps, then run it with gitlab-cov-fuzz
    # See GitLab example repos for how to do this with any of the supported languages
    - ./gitlab-cov.-fuzz run --regression=$REGRESSION -- <your fuzz target>
```

> When enabling Coverage-Guided Fuzz Testing, you are including the file `Coverage-Fuzzing.gitlab-ci.yml` and this file is going to define the jobs in your pipeline that perform the scan. 

> Note that Fuzz Testing jobs are set up to run only in a test stage. Before running your CICD pipeline, make sure that you've defined a test stage.

A corpus is the set of meaningful test cases that are generated while the fuzzer is running. Each meaningful test case produces new coverage in the tested program. It’s advised to re-use the corpus and pass it to subsequent runs.

The [corpus](https://docs.gitlab.com/ee/user/application_security/coverage_fuzzing/#corpus-registry) registry is a library of corpuses. Corpuses in a project’s registry are available to all jobs in that project. A project-wide registry is a more efficient way to manage corpuses than the default option of one corpus per job.

Benefits of using a corpus:
1. Speeds up bug-finding: Gives your fuzz target a starting point. Without a corpus, the fuzz scanner has to guess inputs from scratch, which can take time depending on the inputs and complexity of the target format.
2. Detects Regressions: Allows you to identify specific bugs or issues for the fuzzer to look for, ensuring that code changes or updates do not reintroduce them.

## Security Policies
### Overview
Application security policies are a set of guidelines and procedures that ensure the confidentiality, integrity, and availability of data in software applications. By implementing these policies, organizations can protect sensitive information and mitigate the risk of security breaches.

GitLab supports the following security policies:

| | Scan Execution Policy | Scan Result Policy (Merge Request Policy) |
| ------ | ------- | ------ |
| What is it? | Security Teams use scan execution policies to require vulnerability scans to be run, either on a specified schedule or as part of a pipeline job. Scan execution policies enforce security scans for particular branches at a certain time. | Scan Result Policies allow Security Teams to enforce approval on a merge request when policy conditions are violated. For example, users can require approval on merge requests that introduce newly-detected, critical vulnerabilities into their application. Scan result policies ensure that security issues are checked before merging a merge request. |
| Why is it important? | Scan execution policies enforce a common set of security tool standards across projects. | Scan result policies prevent developers from accidentally or unintentionally committing out-of-compliance code to the codebase. | 
| Use Case Examples	| Run a DAST scan with Scan Profile A and Site Profile A when a pipeline run against the main branch. | If any scanner finds a newly detected critical vulnerability in an open merge request targeting the master branch, then require two approvals from any member of App security. |
| What Scanners does it apply to? | Static Analysis, Secret Detection, Container Scanning, Dependency Scanning, and Dynamic Analysis Scans | All scanners |

### Scan Execution Policies
- Group, subgroup, or project owners can use scan execution policies to require that security scans run on a specified schedule or with the project pipeline. The security scan runs with multiple project pipelines if you define the policy at a group or subgroup level. 

- If you create a policy at the group level, it applies to every child project or subgroup. You cannot edit a group-level policy from a child project or subgroup.

- Both Scan Execution and Scan Results policies are enabled and configured through the GitLab Project UI.
    1. Navigate to Security & Compliance > Policies
    2. Scan execution policy > Select Policy
    3. YAML mode allows you to enter a policy definition in `.yaml` format
    4. When you finish creating or editing your policy, save and apply it by selecting Configure with a merge request

```yaml
type: scan_result_policy
name: Security Approval Policy
description: Require approval for all critical vulnerabilities
enabled: true
rules:
    - type: scan_finding
      branches:
        - main
      scanners: []
      vulnerabilities_allowed: 0
      severity_levels:
        - critical
      vulnerability_states:
        - newly_detected
actions:
    - type: require_approval
      approvals_required: 1
      user_approvers_ids:
        - 168
      group_approvers_ids:
        - 249
```
Scan execution policies allow you to configure the below:
| Rule Type | What Does it Do? |
| --------- | ---------------- |
| `pipeline` rule type | This rule enforces the defined actions whenever the pipeline runs for a selected branch. |
| `schedule` rule type | This rule enforces the defined actions and schedules a scan on the provided date/time. You can define: <ul><li>type - The rule's type</li><li>branches - The branch the given policy applies to</li><li>cadence - Scheduled time</li><li>agents - The name of the GitLab agents where Operational Container Scanning runs</li></ul> |
| `agent` schema | Use this schema to define agents objects in the schedule rule type. |
| `scan` rule type | This action executes the selected scan with additional parameters when conditions for at least one rule in the defined policy are met. <ul><li>`scan` - Scan type (e.g. SAST, DAST, etc.)</li><li>`site_profile` - The DAST site profile to execute the DAST scan. This field should only be set if scan type is dast</li><li>`scanner_profile` - The DAST scanner profile to execute the DAST scan. This field should only be set if scan type is dast.</li><li>`variables` - A set of CI variables to apply and enforce for the selected scan.</li><li>`tags` - A list of runner tags for the policy.</li></ul>|
[Tutorial: Set up a scan execution policy](https://docs.gitlab.com/ee/tutorials/scan_execution_policy/)

### Scan Result Policy
You can use scan result policies to take action based on scan results. For example, one type of scan result policy is a security approval policy that allows approval to be required based on the findings of one or more security scan jobs. Scan result policies are evaluated after a CI scanning job is fully executed.

Both Scan Execution and Scan Results policies are enabled and configured through the GitLab Project UI.
[Tutorial: Set up a scan result policy](https://docs.gitlab.com/ee/tutorials/scan_result_policy/)

### License Approval Policies
License approval policies allow you to specify multiple types of criteria that define when approval is required before a merge request can be merged in.
> License scanning is completed by the dependency scanner.









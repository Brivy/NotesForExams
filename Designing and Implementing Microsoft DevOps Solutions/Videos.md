# Exam prep videos

Each of the following five videos will have some keypoints in them that could help for studying for the exam.

## Preparing for AZ-400 - Configure processes and communications (1 of 5)

The following timestamps have exam information on them:

- `3:30-4:44`: Plan and implement a structure for the flow of work and feedback cycles.
- `4:45-5:42`: Lead time and cycle time. Lead time is the time it takes to go from idea to production. Cycle time is the time it takes to go from code to production.
- `5:43-6:59`: Implement traceability policies decided by development. With work items, you can link them to other work items, commits, pull requests, and builds.
- `7:00-7:32`: Integrate release pipeline with work items.
- `7:33-9:24`: Integrate GitHub as repository with Azure Boards. How can you integrate GitHub and Azure Boards. You can also use commit messages to link work items. If you do this from GitHub, you can use the **AB#** syntax to link Azure DevOps work items. Other cool stuff that you can do with commit messages is to close work items. This can be done using some keywords like *close*, *closes*, *closed*, *fix*, *fixes*, *fixed*, *resolve*, *resolves*, and *resolved*. For example, if you write `git commit -m "Fixes #123: Describe the fix here"`, it will close the work item with ID 123.
- `9:25-10:29`: Create custom dashboards. You need to know some widgets and how to create them.
- `10:30-11:23`: Document a project using DevOps tools. This can be done with Markdown and Mermaid (to create diagrams). Check it out.
- `11:24-12:04`: Check out Cross Platform Release Notes Generator (creates release notes file based on a user-defined handlebars template) and Azure DevOps Release Notes Generator (creates a markdown file whenever a new release is created in Azure DevOps using Azure Blob Storage).
- `12:05-12:42`: You can also use the Git history to create release notes.
- `12:43-13:24`: Communicate with non-native components, by using **webhooks**. But also check out **service hooks**, where you can run tasks when something happens in Azure DevOps.

And the recap:

- Plan and implement a structure for the flow of work and feedback cycles.
- Identify appropriate metrics related to flow of work.
- Integrate release pipeline with work item tracking.
- Implement traceability policies decided by development.
- Integrate GitHub as repository with Azure Boards.
- Create custom dashboards in Azure DevOps.
- Document a project using DevOps tools.
- Configure release documentation, including release notes and API documentation.
- Automate creation of documentation from Git history.
- Configure notifications by using web hooks.

## Preparing for AZ-400 - Design and implement source control (2 of 5)

The following timestamps have exam information on them:

- `0:40-1:44`: Centralized and Decentralized code organization strategy. Centralized can lock files and set permission on file/folder level, but Git is decentralized and you can work offline.
- `1:45-3:15`: Should we have a **Monorepo** or **Multiple repos**? Monorepo is good for starting a project, but it can get messy. It's also easier to refactor code and you have a good discoverability meaning you have all your files in one repo. Multiple repos is good for large projects that have multiple teams working on them. It's also good for security and permissions, but it can grow very complex.
- `3:16-3:53`: You can authenticate to Azure code repositories using SSH keys or PATs (Personal Access Tokens).
- `3:54-5:13`: We don't want large files in our repo, so we can use **Git LFS (Large File Storage)**. It's a Git extension that replaces large files with text pointers inside Git, while storing the file contents on a remote server like GitHub.com or Azure DevOps. If you have installed it with `git lfs install`, you can track your files with `git lfs track "*.psd"`. You can also use the `git lfs migrate` command to migrate your files to Git LFS.
- `5:14-6:13`: How can we enforce some policies in our decentralized environment? By using Git Submodules. It's a way to nest one repository inside another as a subdirectory. This can be done with `git submodule add <url> <path>`. You can also use `git submodule update --init --recursive` to initialize and update your submodules. If your dependency has it's own lifecycle, you can use **Azure Artifacts** to store your dependencies.
- `6:14-7:03`: Remember that we have **Git hooks**. They are scripts that Git executes before or after events such as: commit, push, and receive. You can use them to enforce policies, but they are mostly on the client side.
- `7:04-8:00`: There are branching strategies, like **git flow** and **feature branching** that you need to know.
- `8:01-9:21`: Using PRs (Pull Requests) is a good way to review code and enforce policies. They consist of allowing requestors to approve their own PRs or for example require a minimum number of reviewers. You can also **include certain reviewers based on a path or file**.
- `9:22-10:04`: You can also link work items to PRs and make them required.
- `10:05-11:01`: There are multiple ways to merge your code. You can use **squash merge** to squash all commits into one. You can also use **rebase** to rebase your commits on top of the target branch. You can also use **cherry-pick** to pick a commit from another branch and merge it into your current branch. But you can also enforce **branch policies** to require a PR to be completed before merging. They consist of requiring a minimum number of reviewers, requiring a minimum number of successful builds, requiring linked work items, and requiring a minimum number of comments.
- `11:02-11:54`: GitHub can work together with Azure DevOps pipelines.
- `11:55-12:36`: While you can't give permissions at file level in Git, you can do it for Repositories and branches.
- `12:37-13:04`: You can also use **Git tags** to mark a commit with a version number. Specify for an annotation or a release.
- `13:05-14:14`: What happens if I delete a repository? It's not deleted immediately, but it's kept for 30 days. The url in devops to show soft-deleted git repositories is `https://dev.azure.com/<organization>/<project>/_apis/git/recycleBin/repositories?api-version=6.0-preview.1`. To recover a soft-deleted repository, you can use PATCH `https://dev.azure.com/<organization>/<project>/_apis/git/recycleBin/<repositoryId>?api-version=6.0-preview.1`. But you can also use the UI to restore it by using a specific search.
- `14:15-14:50`: Another tool that comes in handy is the **BFG Repo-Cleaner** to remove some stuff. It's a tool that can remove large files from your Git repo, by using the `bfg --strip-blobs-bigger-than 100M` or `bfg --delete-files mylargefile.dat` or `bfg --replace-text passwords.txt` commands.

And the recap:

- Design source code organization strategy.
- Design authentication strategies.
- Design approach for managing large binary files.
- Design approach for cross repository sharing.
- Implement workflow hooks.
- Define branch strategy.
- Design and implement a pull request (PR) workflow.
- Define PR guidelines to enforce work item correlation.
- Implement branch merging restrictions.
- Integrate GitHub and DevOps Pipelines.
- Configure permissions in the source control repository.
- Organize the repository with git-tags.
- Plan for content recovery in all repository states.
- Purge data from source control.

## Preparing for AZ-400 - Design and implement build and release pipelines (3 of 5)

The following timestamps have exam information on them:

- `0:51-2:27`: Take a look at Microsoft DevOps pipelines. You have pipelines, that have stages, that have multiple jobs, that have multiple steps. In GitHub, you don't have stages, but you have the rest. And just learn the YAML side of things.
- `2:28-3:52`: Check out tasks, the **Task Index**. If they don't have the stuff you need, you can go to the **Marketplace**. But at last, you can also create your own tasks (just learn the process, not the code). Be aware of agents and agent pools.
- `3:53-5:52`: Check out **Quality gates**. Mostly used on the post-deployment to tell if the deployment went fine, but it can also be a pre-deployment. Some gates are: *Invoking an Azure Function*, *Invoking a REST API*, *Invoking a Logic App*, *Invoking an Azure Monitor Alert*, *Invoking an Azure DevOps Pipeline* and *Invoking a Jenkins Job*.
- `5:53-7:11`: Testing strategies. You can use **smoke tests** to check if the application is working. You can use **Unit tests** to test the smallest unit of code. You can use **Integration tests** to test the integration between multiple components. You can use **Regression tests** to test if the new code breaks the old code. You can use **UI tests** to test the UI of the application. You can use **Usability tests** to test if the application is usable. Etc. Ec.
- `7:12-7:50`: Integrate those tests to Azure Pipelines.
- `7:51-8:16`: You'll be creating Artifacts. Have a little of experience of creating/building/pushing artifacts to Azure Artifacts and GitHub Packages.
- `8:17-9:09`: Designing and implementing package feeds (public feeds and private feeds).
- `9:10-10:54`: We need immutable version numbers. We can use **Semantic Versioning**. It's a versioning system that consists of MAJOR.MINOR.PATCH. MAJOR is for breaking changes, MINOR is for new features, and PATCH is for bug fixes. You can also use **Semantic Versioning with Pre-release**. It's the same as Semantic Versioning, but you can add a pre-release tag. You can also use **Date-based Versioning**. It's a versioning system that consists of YYYY.MM.DD. You can also use **Build Number**. It's a versioning system that consists of YYYYMMDD.RRRR. You can also use **Build Number with Pre-release**. It's the same as Build Number, but you can add a pre-release tag.
- `10:55-11:43`: Be aware of the various triggers that can start pipelines. Think of the internal ones, but you also have external ones like the REST API.
- `11:44-14:04`: Agents (Azure Devops) and Runners (GitHub). You can use **Microsoft-hosted agents** (Azure DevOps) or **GitHub-hosted runners** (GitHub). You can also use **Self-hosted agents** (Azure DevOps) or **Self-hosted runners** (GitHub). Know the process. And know that on a Microsoft-hosted agent, you have 10 GB, 10 free parallel jobs and jobs that can run up to 360 minutes if you have a public project. If you don't have one, you get one free parallel job and jobs that can run up to 60 minutes (after a request process). They also run as an administrator on Windows and as a passwordless sudo user on Linux. This means that you can install software on them. But everything will be removed after it's done.
- `14:05-15:44`: Know the various triggers in the pipelines: **Continuous Integration** (CI), **Continuous Delivery** (CD), **Scheduled** (CRON), **Manual** (Manual), **Pull Request** (PR), **Release** (Release), etc, and in GitHub you also have **Comment** triggers.
- `15:45-16:51`: Build pipelines.
- `16:52-18:04`: You can have multi-stage pipelines (with multiple stages) and parallel pipelines (with multiple jobs in a stage).
- `18:05-19:03`: Microsoft hosted agents can only reach public endpoints, but self-hosted agents can reach public and private endpoints.
- `19:04-21:23`: Manage agent pools. You can for example have an agent pool for each team or for each project. And know the difference between **interactive** (process with auto-logon enabled) and **service mode** (a service that starts automatically) agents.
- `21:24-23:46`: You can use **Templates** in YAML pipelines to increase reusability. You can also use **Task groups** to group tasks together (old pipeline stuff). You can also use **Variable groups**. They are used to store values that you want to control and make available across multiple pipelines.
- `23:47-25:11`: You need to use environments in Azure DevOps, if you need checks and approvals in YAML pipelines.
- `25:12-26:29`: Understand other deployment strategies, like **run once** (deploy once and never again), **run to completion** (deploy once and then update it), **rolling** (deploy to a subset of servers and then to the rest), **canary** (deploy to a subset of servers and then to the rest, but with a rollback if something goes wrong).
- `26:30-28:00`: Multiple jobs can depend on each other. You can use the `dependsOn` keyword to do this. It's `needs` in GitHub.
- `28:01-29:09`: Blue-green deployments, where you have two identical environments and you switch between them. Typically requires a load balancer or slot swapping.
- `29:10-29:58`: Design a hotfix path plan.
- `29:59-30:50`: What are the load balancing tools, know the tools (like Azure Traffic Manager and Azure Web Apps). Also know the Lifecycle hooks: **Pre-deployment** (before the deployment), **Post-deployment** (after the deployment), **Pre-swap** (before the swap), **Post-swap** (after the swap), **Route-traffic** (when the traffic is routed), and more.
- `30:51-32:26`: Implementing feature flags with Azure App Configuration. You can use feature flags to enable/disable features in your application. You can use Azure App Configuration to store the feature flags.
- `32:27-33:12`: Know the overall deployment process. You have the **Build** (build the application), **Test** (test the application), **Package** (package the application), **Release** (deploy the application), **Configure** (configure the application), **Monitor** (monitor the application), and **Scale** (scale the application).
- `33:13-34:43`: Know the infrastructure you're deploying too. Know the IaC tools like Azure Resource Manager (ARM) templates, Terraform, and Bicep. Know the syntax a little bit.
- `34:44-36:39`: Use **Azure Desired State Configuration (DSC)** to configure the infrastructure. It's a declarative way to configure the infrastructure. You can use it to configure the infrastructure and to configure the application. For example, What would it take to get it up and running?
- `36:40-37:12`: Know how to deploy a ARM template or Bicep in a pipeline.
- `37:13-37:58`: IaC should be treated as code, you can lint and validate it.
- `37:59-38:49`: Know the difference between Azure Automatization State Configuration, Azure Resource Manager or Bicep. They are: **Azure Automation State Configuration** (a service that can be used to configure the infrastructure), **Azure Resource Manager** (a service that can be used to deploy the infrastructure), and **Bicep** (a language that can be used to deploy the infrastructure).
- `38:50-39:30`: Monitor pipeline health in the Analytics tab.
- `39:31-40:45`: Know that you can use **Azure Artifacts** to store your package *if it deserves it's own lifecycle*. Otherwise, you can also use pipeline caching with a task called `Cache@2`.
- `40:46-42:25`: Know how many build agents you need.
- `42:26-43:16`: Design a retention strategy for artifacts, like how long do you want to keep them? Do we only want to keep the latest version? Do we want to keep all versions?

And the recap:

- Build automation overview.
- Integrate the build pipeline with external tools.
- Implement quality gates and Design a testing strategy.
- Integrate multiple tools and Recommended package management tools.
- Design and implement package feeds.
- Design and implement a dependency versioning strategy.
- Design a versioning strategy.
- Select a deployment automation solution, including GitHub Actions and Azure Pipelines.
- Design and implement build agent infrastructure.
- Develop and implement build trigger rules and Develop build pipelines.
- Design and implement a strategy for job execution order, including parallelism and multi-stage.
- Develop complex pipeline scenarios such as containerized agents and hybrid.
- Manage self-hosted build agents and Create reusable build subsystems.
- Design and implement checks and approvals by using YAML environments.
- Design and implement a release strategy.
- Design the release pipeline to ensure reliable order of dependency deployments.
- Plan for minimizing downtime during deployments.
- Design a hotfix path plan for responding to high priority code fixes.
- Implement a load balancing for deployment, including Azure Traffic Manager and Azure Web Apps.
- Implement feature flags by using Azure App Configuration Feature Manager.
- Develop application deployment process and Design and implement infrastructure as code.
- Assess a configuration management mechanism for application infrastructure.
- Implement a configuration management strategy for application infrastructure, including IaC.
- Define an IaC strategy, including source control and automation for testing and deployment.
- Select the appropriate desired state solution for a deployment environment.
- Monitor pipeline health and Optimize build.
- Analyze CI load to determine build agent configuration and capacity.
- Design and implement a retention strategy for pipeline artifacts and dependencies.

## Preparing for AZ-400 - Develop a security and compliance plan (4 of 5)

The following timestamps have exam information on them:

- `0:33-1:56`: Service connections store urls and credentials of external resources. You can determine which users/pipelines/projects can use them. Then you can use it by it's name in the YAML pipeline file.
- `1:57-2:37`: If you need external resources, you probably use PAT (Personal Access Tokes). You can limit the permissions on them very granularly.
- `2:38-4:48`: Secrets in pipelines can store secrets in **variable groups with secrets that no one can read back**. You can also limit the permissions on them very granularly. Secrets can also be stored in Azure Key Vault. You can use them in your variable group (used when used in multiple pipelines) or in the pipelines by using the `$(secretName)` syntax (used for one-offs).
- `4:49-5:20`: Grant permissions to the service connection, for example for the key vault, you typically use the Get and List permissions.
- `5:21-5:47`: Certificates/SSH keys can be stored in Secure files for your pipelines. You can also store them in key vault.
- `5:48-8:17`: Prevent leakage of sensitive information by: not providing secrets in fork build, manually triggering fork build and use Microsoft-hosted agents for fork builds.
- `8:18-9:31`: For source code security, you can use **git hooks** on the client, use **GitHub security features like dependabot/vulnerability scanning**, use pipeline scanning with **WhiteSource Bolt/Mend** or **Checkmarx** and use **SonarQube** for code quality.
- `9:32-10:37`: Dependency scanning can be done in version control, in the build pipeline, or in the source code.
- `10:38-11:20`: You can use **WhiteSource Bolt/Mend** to scan your dependencies in your build pipeline. And DependaBot to update your dependencies and it can creates PRs automatically.

And the recap:

- Implement and manage service connections.
- Implement and manage personal access tokens (PATs).
- Implement and manage secrets, keys and certificates.
- Design a secrets storage and retrieval strategy.
- Design and implement a strategy for managing sensitive files during deployment.
- Design pipelines to prevent leakage of sensitive information.
- Design a source code complicance strategy.
- Automate dependencies scanning for security and compliance.
- Automate analysis of licensing, vulnerabilities, and versioning of open-source components.

## Preparing for AZ-400 - Implement an instrumentation strategy (5 of 5)

The following timestamps have exam information on them:

- `0:32-1:52`: APM's are Application Performance Monitoring tools. They can be installed on multiple levels, like on the agent or without an agent (and without code). Microservices can also be monitored with Prometheus or something in Kubernetes. Azure monitor can then be used to monitor the hardware and the application.
- `1:53-3:24`: Where is this information going to go? You want to integrate it in your feedback loop, for example create alerts from them or create some dashboards.
- `3:25-4:08`: They are generating a lot of data, so know about insuring the safety of the information. For example using storage accounts and locking them down.
- `4:09-5:14`: Configure alerts and notifications for pipelines. You can use the `@mention` syntax to mention someone in a pipeline. But you can also create events based on the pipeline, for example, creating an alert for something through a logic app that puts it inside Microsoft Teams.
- `5:15-6:51`: With Application Insights, we can have distributed tracing. We can see the whole flow of the application. We can also use it to monitor the performance of the application. If a language isn't supported, we can use OpenTelemetry/OpenCensus to send data to Application Insights.
- `6:52-8:00`: For Application Insights, there are three instrumentation options to receive telemetry from your app: **Runtime** (we don't set it up, we only check a checkbox in Azure), **Build-time** (including the SDK in the application) and **Client-side** (including a javascript library).
- `8:01-8:51`: When you have the data, you can inspect it with the performace indicators. It's Azure Monitor which is generated your metrics and logs (text based events). This information is often published to a Log Analytics workspace.
- `8:52-9:59`: Which metrics do we want to use? Success vs Failure metrics (200 vs 400 requests), Timing metrics (how long did it take?), Throughput metrics (how many requests per second?), and Quality metrics (how many errors?).
- `10:00-11:05`: Integrating the user analytics. What pages do a user check? What features do they use? This can all be tracked with Application Insights.
- `11:06-12:45`: You can use basic **Kusto queries (KQL)** to query your data. Know the keywords for the exam. It can render charts and tables.

And the recap:

- Configure and integrate with monitoring tools.
- Create feedback loop from platform monitoring tools.
- Manage access control to the monitoring platform.
- Configure alerts for pipeline events.
- Inspect distributed tracing by using Application Insights.
- Inspect application performance indicators.
- Inspect infrastructure performance indicators.
- Identify and monitor metrics for business value.
- Integrate user analytics.
- Interrogate logs using basic Kusto (KQL) queries.

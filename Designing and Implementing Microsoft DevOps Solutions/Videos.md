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

TODO

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

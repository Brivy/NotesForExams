# Introduction to DevOps

DevOps is the union of people, process, and products to enable continuous delivery of value to our end users.

## What is DevOps?

The contraction of "Dev" and "Ops" refers to replacing siloed Development and Operations. It include agile planning, continuous integration, continuous delivery, and monitoring of applications.

![Cool Picture](Pictures/devops-cycle-98924900.png)

### Understand your cycle time

The cycle time is described as **OODA** (Observe, Orient, Decide, Act). Every cycle has 33% of bringing some improvements, 33% will do nothing and 33% will fail (this is called called *pivot* or *persevere*). Your cycle time determines how quickly you can gather feedback to determine what happens in the next loop. The feedback that you collect with each cycle should be factual, actionable data. We call it **validated learning**.

Advantages of using DevOps practices:

* You shorten your cycle time by working in smaller batches.
* Using more automation.
* Hardening your release pipeline.
* Improving your telemetry.
* Deploying more frequently.

How frequenter you can deploy, how more you learn from each cycle (*validated learning*).

![Cool Picture](Pictures/devops-road-optimize-learning-3f77fb52.png)

### Explore the DevOps journey

There are different areas existing from DevOps:

* **Continuous Integration** drives the ongoing merging and testing of code, leading to an early finding of defects. Other benefits include less time wasted fighting merge issues and rapid feedback for development teams.
* **Continuous Delivery** of software solutions to production and testing environments helps organizations quickly fix bugs and respond to ever-changing business requirements.
* **Version Control**, usually with a Git-based Repository, enables teams located anywhere in the world to communicate effectively during daily development activities. Also, integrate with software development tools for monitoring activities such as deployments.
* Use **Agile planning** and **lean project management techniques** to: - Plan and isolate work into sprints. - Manage team capacity and help teams quickly adapt to changing business needs. - A DevOps Definition of Done is working software collecting telemetry against the intended business goals.
* **Monitoring and Logging of running applications**. Including production environments for application health and customer usage. It helps organizations create a hypothesis and quickly validate or disprove strategies. Rich data is captured and stored in various logging formats.
* **Public, hybrid and private clouds** make infrastructure maintaining easier to manage.
* **Infrastructure as Code (IaC)**: Enables the automation and validation of the creation and teardown of environments to help deliver secure and stable application hosting platforms.
* Use **Microservices architecture** to isolate business use cases into small reusable services that communicate via interface contracts. This architecture enables scalability and efficiency.
* **Containers** are the next evolution in virtualization. They're much more lightweight than virtual machines, allow much faster hydration, and easily configure files.

### Explore shared goals and define timelines

These outcomes should include specific, measurable targets like:

* Reduce the time spent on fixing bugs by *60%*.
* Reduce the time spent on unplanned work by *70%*.
* Reduce the out-of-hours work required by staff to no more than 10% of total working time.
* Remove all direct patching of production systems.

It is essential to have an ongoing series of short-term goals. Overall, projects should have timelines that span anywhere from a few months to a year or two in any DevOps transformation project. Every few weeks, the improvements should be clear and measurable. Ideally, evident to the organization or its customers:

* It is easier to change plans or priorities when necessary.
* The reduced delay between doing work and getting feedback helps ensure that the learnings and feedback are incorporated quickly.
* It is easier to keep organizational support when positive outcomes are clear.

## Choose the right project

Are you interested in implementing DevOps practices but are not sure what it can do for you?

### Explore greenfield and brownfield projects

There are two kinds of projects:

* A **greenfield** project is one done on a green field, undeveloped land. In the software world, this can be seen as a fresh project that can easily integrate with DevOps.
* A **brownfield** project is done on the used ground for other purposes. In the software world, this can be seen as a older project, and while difficult to migrate, it is certainly possible to use DevOps.

### Decide when to use systems of record versus systems of engagement

When you want to transform a system, you can use a practice of managing two separate, coherent modes of IT delivery - one focused on stability and predictability and the other on agility:

* **Systems of record**: This are systems that provide the truth about data elements and are often-called systems of record. These systems have historically evolved slowly and carefully, like a crucial banking system.
* **Systems of engagement**: This are systems that are more exploratory. These often use experimentation to solve new problems. Systems of engagement are modified regularly. Usually, it is a priority to make quick changes over ensuring that the changes are correct.

While transforming a system of engagement is mostly easier then a system of record, both are possible.

### Identify groups to minimize initial resistance

In discussions around continuous delivery, we usually categorize users into three general buckets:

* **Canary** users voluntarily test bleeding edge features as soon as they're available.
* **Early adopters** who voluntarily preview releases, considered more refined than the code that exposes canary users.
* **Users** who consume the products after passing through canary and early adopters.

It's also important to roll out changes incrementally. When starting, it is essential to find an improvement goal that:

* It can be used to gain early wins.
* It is small enough to be achievable in a reasonable timeframe.
* Has benefits that are significant enough to be evident to the organization.

### Identify project metrics and key performance indicators (KPIs)

It is essential to establish (and agree upon) appropriate metrics and **Key Performance Indicators (KPIs)** to ensure these goals are measurable. While there is no specific list of metrics and KPIs that apply to all DevOps Projects, the following are commonly used:

Faster outcomes:

* **Deployment Frequency**. Increasing the frequency of deployments is often a critical driver in DevOps Projects.
* **Deployment Speed**. It is necessary to reduce the time that they take.
* **Deployment Size**. How many features, stories, and bug fixes are being deployed each time?
* **Lead Time**. How long does it take from the creation of a work item until it is completed?

Efficiency:

* **Server to Admin Ratio**. Are the projects reducing the number of administrators required for a given number of servers?
* **Staff Member to Customers Ratio**. Is it possible for fewer staff members to serve a given number of customers?
* **Application Usage**. How busy is the application?
* **Application Performance**. Is the application performance improving or dropping? (Based upon application metrics)?

Quality and security:

* **Deployment failure rates**. How often do deployments (or applications) fail?
* **Application failure rates**. How often do application failures occur, such as configuration failures, performance timeouts, and so on?
* **Mean time to recover**. How quickly can you recover from a failure?
* **Bug report rates**. You do not want customers finding bugs in your code. Is the amount they are seeing increasing or lowering?
* **Test pass rates**. How well is your automated testing working?
* **Defect escape rate**. What percentage of defects are being found in production?
* **Availability**. What percentage of time is the application truly available for customers?
* **Service level agreement achievement**. Are you meeting your service level agreements (SLAs)?
* **Mean time to detection**. If there is a failure, how long does it take for it to be detected?

Culture:

* **Employee morale**. Are employees happy with the transformation and where the organization is heading? Are they still willing to respond to further changes? This metric can be challenging to measure but is often done by periodic, anonymous employee surveys.
* **Retention rates**. Is the organization losing staff?

## Describe team structures

Starting to apply Agile practices in your company is not easy. It demands collaboration from your teams, stakeholder support, and training.

### Explore agile development practices

For traditional software development, you would use the **waterfall** method. This follows a sequential order where the team only moves to the next phase if the previous step is completed. Usually, the project takes a long time, and the outcome may no longer match the customer's needs.

On the other end, the **agile** method constantly emphasizes adaptive planning and early delivery with continual improvement. It is based on releases and iterations:

* One release might consist of several iterations.
* Each iteration is like a small independent project.
* After being estimated and prioritization:
  * Features, bug fixes, enhancements, and refactoring width are assigned to a release.
  * And then assigned again to a specific iteration within the release, generally on a priority basis.
* At the end of each iteration, there should be tested working code.
* In each iteration, the team must focus on the outcomes of the previous iteration and learn from them.

| Waterfall                                                                                  | Agile                                                                                      |
|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Divided into distinct phases.                                                              | Separates the project development lifecycle into sprints.                                  |
| It can be rigid.                                                                           | Known for flexibility.                                                                     |
| All project development phases, such as design, development, and test, are completed once. | It follows an iterative development approach so that each phase may appear more than once. |
| Define requirements at the start of the project with little change expected.               | Requirements are expected to change and evolve.                                            |
| Focus on completing the project.                                                           | Focus on meeting customers' demands.                                                       |

### Explore principles of agile development

The agile principals are:

* Our highest priority is to satisfy the customer through early and continuous delivery of valuable software.
* Welcome changing requirements, even late in development. Agile processes harness change for the customer's competitive advantage.
* Deliver working software frequently, from a couple of weeks to a couple of months, with a preference to the shorter timescale.
* Business people and developers must work together daily throughout the project.
* Build projects around motivated individuals. Give them the environment and support they need and trust them to get the job done.
* The most efficient and effective method of conveying information to and within a development team is face-to-face conversation.
* Working software is the primary measure of progress.
* Agile processes promote sustainable development. The sponsors, developers, and users should be able to maintain a constant pace indefinitely.
* Continuous attention to technical excellence and good design enhances agility.
* Simplicity - the art of maximizing the amount of work not done - is essential.
* The best architectures, requirements, and designs emerge from self-organizing teams.
* At regular intervals, the team reflects on how to become more effective, then tunes and adjusts its behavior accordingly.

### Team stuff for agile development

You have **horizontal** and **vertical** teams. Vertical teams have been shown to provide more good outcomes in Agile projects and you can scale it by adding more vertial teams. Each product must have an identified owner.

Horizontal:

![Cool Picture](Pictures/devops-ds-image-101-cd10ac81.png)

Vertical:

![Cool Picture](Pictures/devops-ds-image-103-b26487ac.png)

A good team requires both *cultural changes*, *cross-function team collaboration* and *tooling*.

## Choose the DevOps tools

Do you currently use tools to manage daily work and plan for the future? It is essential when implementing DevOps if you want to increase your maturity level.

### What is Azure DevOps?

This is a Software as a Service (SaaS) platform that provides the following:

* *Azure Boards*: agile planning, work item tracking, visualization, and reporting tool.
* *Azure Pipelines*: a language, platform, and cloud-agnostic CI/CD platform-supporting containers or Kubernetes.
* *Azure Repos*: provides cloud-hosted private git repos.
* *Azure Artifacts*: provides integrated package management with support for Maven, npm, Python, and NuGet package feeds from public or private sources.
* *Azure Test Plans*: provides an integrated planned and exploratory testing solution.

### What is GitHub?

This is also a Software as a Service (SaaS) platform and more code focused that Azure DevOps. It  provides the following:

* *Codespaces*: Provides a cloud-hosted development environment (based on Visual Studio Code) that can be operated from within a browser or external tools. Eases cross-platform development.
* *Repos*: Public and private repositories based upon industry-standard Git commands.
* *Actions*: Allows for the creation of automation workflows. These workflows can include environment variables and customized scripts.
* *Packages*: The majority of the world's open-source projects are already contained in GitHub repositories. GitHub makes it easy to integrate with this code and with other third-party offerings.
* *Security*: Provides detailed code scanning and review features, including automated code review assignment.

### Explore an authorization and access strategy

Microsoft products (like Azure DevOps), you can use your Microsoft, GitHub or Azure Active Directory account. But for services that don't directly support Microsoft accounts, you can use **Personal Access Tokens (PAT)**. This can be used for example in Git-based repositories, NuGet or Xcode.

Azure DevOps is pre-configured with **default security groups**. There you can configure different **access levels**, **access policies** and **Conditional Access policies**. Also conditional access policies can configure *Multi-factor authentication (MFA)*.

### Migrating existing stuff to Azure DevOps or GitHub

Before you start, check if your existing work and test tools can be imported to Azure DevOps.

### Design a license management strategy

It's important to consider the licensing strategy. Not everything is free and one of these examples is running parallel pipelines. You should consider:

* What phase are you in?
* How many people are using the feature?
* How long are you willing to wait if in the queue for pipelines? Is this urgent? Is this a validation only?
* Should all users access all features? Are they Stakeholders? Basic users? Do they already have a Visual Studio license?
* Do you have an advanced Package Management strategy? Maybe you need more space for Artifacts.

## Plan Agile with GitHub Projects and Azure Boards

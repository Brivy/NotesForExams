# Introduction to Azure fundamentals

This is the first chapter of the online Microsoft Azure Fundamentals course.

## What is cloud computing?

- You pay only for the cloud services you use, which lead to:
  - Lower operating costs.
  - Running your infrastructure more efficiently.
  - Scale as your business needs change.

- There are several benefits of using the cloud environment:
  - **High availability**: Depending on your service-level agreement (SLA), there is little to no downtime.
  - **Scalability**: Scaling applications can be done in two ways:
    - **Vertical**: Adding more RAM or CPU's to the vm.
    - **Horizontal**: Adding more vms to the mix.
    - **Elasticity**: Applications can use autoscaling so they will always have enough resources.
    - **Agility**: Cloud-based resources can be deployed and configured quickly when your application requirements change.
    - **Geo-distribution**: Applications and data can be deployed through regional datacenters around the world.
    - **Disaster recovery**: By taking advantage of the points above, you can deploy with confidence that data will be safe on a disaster.

- There are multiple service models that define levels of shared responsibility that a cloud provider and cloud tenant are responsible for:
  - **IaaS** (infrastructure as a service):
    - Closest to managing physical servers
    - The cloud provider manages the servers, storage, networking, firewalls, security and of course the hardware.
  - **PaaS** (platform as a service):
    - Managed hosting environment:
    - In addition to the IaaS, the cloud provider now also manages the operating system, development tools, database management and business analytics.
  - **SaaS** (software as a service):
    - Only needs to provide their data to the application that is managed by the cloud provider.
    - In addition to the IaaS and PaaS, the cloud provider now also manages the applications and data storage.

- Serverless computing enables developers to build applications faster by eliminating the need for them to manage the infrastructure.
  - They only use resources when a specific function or trigger occurs (like running on low RAM).
  - The term  "Serverless" is more a less derived from not focussing on the server but instead delivering business value.

- Deployment models for cloud computing:
  - **Public cloud**: Services are offered over the public internet and available to anyone who wants to purchase them (for example Azure).
  - **Private cloud**: Computing resources are used exclusively by users from one business or organization (for example an on-premise server).
  - **Hybrid cloud**: Combines a public cloud and a private cloud by allowing data and applications to be shared between them (for example Azure Stack).

## What is Azure?

- Azure uses virtualization to separate the tight coupling between a computer's hardware and its OS by using a hypervisor.
  - The hypervisor emulates all the functions of a real machine and its CPU in a VM.
  - Each server rack in an Azure warehouse uses the hypervisor to run emulation on all the machines.
  - You can interact with all of this by using the Azure API (also known as the Azure Portal)
  - The Azure portal doesn't have any downtime for maintenance.
  - The Azure Marketplace helps connect users with Microsoft partners, independent software vendors, and startups that are offering their solutions and services, which are optimized to run on Azure.

## Tour of Azure services

- These are the most commonly used categories within Azure:
  - **Compute**: Provides a range of options for hosting applications and services.
  - **Networking**: Networking functionality in Azure includes a range of options to connect the outside world to services and features in the global Azure datacenters.
  - **Storage**: Storage services have several common characteristics:
    - Durable and highly available with redundancy and replication.
    - Secure through automatic encryption and role-based access control.
    - Scalable with virtually unlimited storage.
    - Managed, handling maintenance and any critical problems for you.
    - Accessible from anywhere in the world over HTTP or HTTPS.
  - **Mobile**: Includes features like:
    - Offline data synchronization
    - Connectivity to on-premises data.
    - Broadcasting push notifications.
    - Autoscaling to match business needs.
  - **Databases**: Provides multiple database services to store a wide variety of data types and volumes. And with global connectivity, this data is available to users instantly.
  - **Web**: Includes first-class support to build and host web apps and HTTP-based web services.
  - **IoT**: Smart computers and all.
  - **Big Data**: We are talking about data like from weather reports and all.
  - **AI**: Machine learning is a data science technique that allows computers to use existing data to forecast future behaviors, outcomes, and trends.
  - **DevOps**: DevOps brings together people, processes, and technology by automating software delivery to provide continuous value to your users.

## Get started with Azure accounts
  
- An Azure free account includes:
  - Free access to popular Azure products for 12 months.
  - A credit to spend for the first 30 days.
  - Access to more than 25 products that are always free.

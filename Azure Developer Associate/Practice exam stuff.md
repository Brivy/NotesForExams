# Question list with explained anwsers

(Sorry Measureup and Microsoft for stealing this :))

In this chapter, I will go through all the practice exam questions and figure out what I still need to research/investigate.

## Case studies

1. Creating an automatic scaling App service inside an resource group.
    - First create a resource group with the specified name.
    - Secondly, create the App Service Plan with the **Standard Plan**. This scales (so not Basic plan) and isn't to cost heavy (Premium plan).
    - Third, create the App Service itself with the specified name.
    - References: [Create an ASP.NET Core app in App Service on Linux](https://docs.microsoft.com/en-us/azure/app-service/quickstart-dotnetcore?pivots=development-environment-vs&tabs=net60), [Azure App Service plan overview](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans?toc=/azure/app-service/containers/toc.json)
2. Complete the function.
    - The name of the function `[FunctionName("...")]` can be seen in the requirements.
    - The first parameter of the function should be `[ServiceBusTrigger("...")]`. This is because the first binding should be the *trigger*. The others are just bindings.
    - The second parameter of the function should be `[Blob("orders/{id}", FileAccess.Write)]`. This is because it's an *binding* and not a *trigger*. Also we want to *write* stuff instead of *reading*.
    - References: [Azure Functions triggers and bindings concepts](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=csharp), [Azure Blob storage output binding for Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-blob-output?tabs=csharp), [Azure Service Bus trigger for Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus-trigger?tabs=csharp)
3. Complete a multi-stage build docker file that is fully optimized.
    - In a *multi-stage build*, you should first build the application using the .NET Core SDK (`FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build`) and copy it to a new stage in the built application with a lighter runtime image (`FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS runtime`). We don't use the `FROM ../runtime:3.0 AS runtime` because we not ony have .NET Core applications.
    - Because we want to copy all the application code to the build stage before publishing the project, we use `COPY checkout/ ./checkout`. And because we want to copy the build application in the runtime stage, we use `COPY --from=build /app/checkout/out ./` to copy the artifact from the build stage.
    - By using the `RUN dotnet restore`, we download the application dependencies in the first layer to maximize the container image layer cache. We also use the `RUN dotnet publish -c Release -o out` to create a release output folder.
    - References: [Docker images for ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/docker/building-net-docker-images?view=aspnetcore-3.0), [Dockerfile reference](https://docs.docker.com/engine/reference/builder/), [Use multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/)
4. Creating an Azure Container Instance by Azure PowerShell
    - You should first create a resource group with the specified name (found in the specification) with `New-AzResourceGroup -Name BackendRg -Location WestUS`.
    - And then you can create a Azure Container Instance with the specified details with `New-AzContainerGroup -ResourceGroupName BackendRg -Name CheckoutService -Image company1/checkout-service -OsType Linux -DnsNameLabel checkout-service`. One important thing to note is that you shouldn't use `New-AzContainerService`, because it's a deprecated service to manage container orchestrators.
    - References: [Quickstart: Deploy a container instance in Azure using Azure PowerShell](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-powershell), [New-AzContainerService](https://docs.microsoft.com/en-us/powershell/module/az.compute/new-azcontainerservice?view=azps-0.10.0&viewFallbackFrom=azps-3.8.0)
5. Choosing bindings for a function
    - Because the function should be triggered from an HTTP request, the `HTTP trigger binding` is appropriate here.
    - In the function, we want to read from the Blob storage, so using an `Blob output binding` should suffice. Be careful to not use a `Blob input binding` because this should be used to write to the storage (and thus not read).
    - References: [Azure Functions triggers and bindings concepts](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=csharp), [Azure Functions HTTP trigger](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp), [Azure Blob storage bindings for Azure Functions overview](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-blob), [Azure Service Bus trigger for Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus-trigger?tabs=csharp)
6. Should we deploy and configure a NoSQL Cosmos DB instance and update the web apps to store session state to the database?
    - The answer should be *No*, because ASP.NET applications only support saving session state data to a SQL Server database, not a NoSQL one.
    - References: [Understanding the differences between NoSQL and relational databases](https://docs.microsoft.com/en-us/azure/cosmos-db/relational-nosql), [What is Azure Database for PostgreSQL?](https://docs.microsoft.com/en-us/azure/postgresql/overview), [About Azure Cache for Redis](https://docs.microsoft.com/en-us/azure/azure-cache-for-redis/cache-overview), [Configure data persistence for a Premium Azure Cache for Redis instance](https://docs.microsoft.com/en-us/azure/azure-cache-for-redis/cache-how-to-premium-persistence)
7. Should we configure the web apps to use the ASP.NET Memory Session State Provider?
    - The answer should be *No*, because the requirements state that it should be distributed over all web apps (and that's not the case here).
    - References: [Understanding the differences between NoSQL and relational databases](https://docs.microsoft.com/en-us/azure/cosmos-db/relational-nosql), [What is Azure Database for PostgreSQL?](https://docs.microsoft.com/en-us/azure/postgresql/overview), [About Azure Cache for Redis](https://docs.microsoft.com/en-us/azure/azure-cache-for-redis/cache-overview), [Configure data persistence for a Premium Azure Cache for Redis instance](https://docs.microsoft.com/en-us/azure/azure-cache-for-redis/cache-how-to-premium-persistence)
8. Should we deploy and configure a Premium tier Azure Cache for Redis and update the web applications?
    - The answer should be *Yes*, because it provides:
      - A shared session state across ASP.NET web applications.
      - Persists session state information as a snapshot
      - Provides multiple readers with concurrent access to the same session state data.
      - Minimize latency when retrieving session state information
    - Also we should use the *Premium tier*, because it's required for persisting session state data as a snapshot.
    - References: [Understanding the differences between NoSQL and relational databases](https://docs.microsoft.com/en-us/azure/cosmos-db/relational-nosql), [What is Azure Database for PostgreSQL?](https://docs.microsoft.com/en-us/azure/postgresql/overview), [About Azure Cache for Redis](https://docs.microsoft.com/en-us/azure/azure-cache-for-redis/cache-overview), [Configure data persistence for a Premium Azure Cache for Redis instance](https://docs.microsoft.com/en-us/azure/azure-cache-for-redis/cache-how-to-premium-persistence)
9. Should we use Azure Functions with the Premium plan configured with an IoT Hub trigger, Cosmos DB as the input binding, and Azure Storage Account as output binding for IoT devices that are streaming data to an IoT Hub?
    - The answer should be *Yes*, because it provides:
      - The *Premium plan* will provide pre-warmed instances.
      - The *IoT Hub trigger* will fire when a message arrives.
      - The *Cosmos DB input binding* gives access to the configuration.
      - The *Azure Storage output binding* gives the ability to save the data.
    - References: [Azure Functions triggers and bindings concepts](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=csharp), [Azure Functions Premium plan](https://docs.microsoft.com/en-us/azure/azure-functions/functions-premium-plan?tabs=portal)
10. Should we use Azure Functions with the Premium plan configured with an Cosmos DB trigger, IoT Hub as the input binding, and Azure Storage Account as output binding for IoT devices that are streaming data to an IoT Hub?
    - The answer should be *No*, because it can't do anything with the *Cosmos DB trigger* in this case.
    - References: [Azure Functions triggers and bindings concepts](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=csharp), [Azure Functions Premium plan](https://docs.microsoft.com/en-us/azure/azure-functions/functions-premium-plan?tabs=portal)
11. Should we use Azure Functions with the Consumption plan configured with an IoT Hub trigger, Cosmos DB as the input binding, and Azure Storage Account as output binding for IoT devices that are streaming data to an IoT Hub?
    - The answer should be *No*, because the *Consumption plan* does not guarantee that a runtime environment will always be available because the cold start could take a while.
    - References: [Azure Functions triggers and bindings concepts](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=csharp), [Azure Functions Premium plan](https://docs.microsoft.com/en-us/azure/azure-functions/functions-premium-plan?tabs=portal)

## Normal questions

1. Deploying a single ASP.NET Core application on an Azure Container Instance (ACI). Sidecar a container on the same host that doesn't increase cost and requires minimal change.
    - We should use *ACI Container Groups* (yes, it's the same service). They provide the capability to host a set of containers on the same host machine, share the same resources and can communicate with each other. You can also install monitoring agents inside the separate container instances.
    - The other options were:
      - *Azure Container Registry*. This is where the container images are hosted.
      - *Kubernetes Pods*. Will work, but because we already have ACI working, this would only increase the cost and complexity.
      - *Azure App Services*. Will work, but because we already have ACI working, this would only increase the cost and complexity.
    - Resources: [Container groups in Azure Container Instances](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-container-groups), [Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/), [Tutorial: Deploy a multi-container group using a Resource Manager template](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-multi-container-group), [Introduction to private Docker container registries in Azure](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-intro)
2. Creating a Dockerfile that will build and publish the container image to an Azure Container Registry (ACR).
    - First you should build the Dockerfile with `docker build`. This will create a new container image with the instructions in it.
    - Secondly, you should tag the image with `{{ACR_name_here}}.azurecr.io/{{App_name_here}}`. This is needed to push a container image to the ACR.
    - Third, login in the {{ACR_name_here}} with `az acr login` and push the image to {{ACR_name_here}}. If you don't login, it doesn't know where to push. Don't confuse this with `az acr pack`, because this command is to build container images using Cloud Native Buildpacks using ACR Tasks.
    - References: [Push your first image to your Azure container registry using the Docker CLI](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli?tabs=azure-cli), [az acr pack](https://docs.microsoft.com/en-us/cli/azure/acr/pack?view=azure-cli-latest)
3. Encrypt a VHD and create a virtual machine when deploying on Azure.
    - First you should encrypt the VHD using BitLocker without a TPM and upload it to an Azure storage account. Note that a TPM is chip on a motherboard that the virtual machines do not have (and thus not support TPM).
    - Secondly, you should run the `Set-AzVMOSDisk` PowerShell command. This will set the disk as an OS disk for the virtual machine.
    - Third, you should run the `New-AzVM` PowerShell command. This will create the new VM.
    - Not that if you are already running the VM and want to encrypt the disk, you should use the `Set-AzVMDiskEncryptionExtension` command.
    - References: [What is a TPM, and Why Does Windows Need One For Disk Encryption?](https://www.howtogeek.com/237232/what-is-a-tpm-and-why-does-windows-need-one-for-disk-encryption/), [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/en-us/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-6.6.0&viewFallbackFrom=azps-3.6.1), [https://docs.microsoft.com/en-us/powershell/module/az.compute/set-azvmosdisk?view=azps-6.6.0&viewFallbackFrom=azps-3.6.1](Set-AzVMOSDisk)
4. Create a ARM template from a resource group that you will use as your deployment source by PowerShell.
    - Use the `Export-AzResourceGroup` cmdlet. This will retrieve all the resources or (by specification) retrieve a single VM or something like that.
    - The other options were:
      - *Register-AzResourceProvider*. This command is used for registering a resource provider for your Azure account and has nothing to do with this question.
      - *Get-AzResourceGroup*. This command will retrieve information about the resource group, but it doesn't create an ARM template.
      - *Save-AzDeploymentTemplate*. This command will save an existing template to a file, but not create a template from an existing resource group.
      - References: [Download the template for a VM](https://docs.microsoft.com/en-us/previous-versions/azure/virtual-machines/windows/download-template), [Export-AzResourceGroup](https://docs.microsoft.com/en-us/powershell/module/az.resources/export-azresourcegroup?view=azps-6.6.0&viewFallbackFrom=azps-3.8.0), [Register-AzResourceProvider](https://docs.microsoft.com/en-us/powershell/module/az.resources/register-azresourceprovider?view=azps-6.6.0&viewFallbackFrom=azps-3.8.0), [Get-AzResourceGroup](https://docs.microsoft.com/en-us/powershell/module/az.resources/Get-AzResourceGroup?view=azps-6.6.0&viewFallbackFrom=azps-3.8.0)
5. Uploading an on-premise Windows VM to the cloud and encrypt it.
    - Azure Disk encryption is supported for generation 1 and generation 2 VMs. The only thing not supported on the generation 2 VMs is 32-bit systems.
    - You must convert the VHDX file to VHD before uploading it to Azure. This is because VHDX is not supported there.
    - You must convert the dynamic disk to a fixed-size disk before uploading. This is needed to support encryption, because that isn't supported on a dynamic disk.
    - References [Prepare a Windows VHD or VHDX to upload to Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/prepare-for-upload-vhd-image), [Azure Disk Encryption for Windows VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-overview), [Azure Disk Encryption for Windows virtual machines FAQ](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-faq)
6. Generate a container image.
    - Use the `az acr build` command. This will (after a successful build), push the image to your container registry.
    - The other options were:
      - *az acr create*. This command will create a container registry.
      - *az acr update*. This command is used to update parameters for an ACR, such as enabling an administrative account or updating tags.
      - *az acr import*. This command will import an image to an ACR from another ACR.
      - References [Quickstart: Build and run a container image using Azure Container Registry Tasks](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-quickstart-task-cli), [az acr](https://docs.microsoft.com/en-us/cli/azure/acr?view=azure-cli-latest)
7. Configure and access a Linux VM from your local machine.
    - The first command is `adminUser='az vm show -g group1 -n vm1 --query osProfile.adminUsername -o tsv'`. This will retrieve the admin username from vm1.
    - The second command is `publicIp='az vm show -g group1 -n vm1 -d --query publicIps -o tsv`. This will retrieve the public IP from vm1 (don't forget the `-d` that will list the publicIps).
    - The last command is `ssh $adminUser@$publicIp`. This will simply make a SSH connection with the stored variables.
    - The other options were:
      - *az vm nic show*. This command will show information about a specific network adapter.
      - *privateIps*. This will show you the private IP of the vm. We want the public one.
      - *mstsc /v:*. This will connect by Remote Desktop Protocol (RDP). We use a Linux vm.
    - References: [Tutorial: Create and Manage Linux VMs with the Azure CLI](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm), [How to query Azure CLI command output using a JMESPath query](https://docs.microsoft.com/en-us/cli/azure/query-azure-cli?view=azure-cli-latest), [Tutorial: Create and Manage Windows VMs with Azure PowerShell](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-manage-vm), [az vm nic](https://docs.microsoft.com/en-us/cli/azure/vm/nic?view=azure-cli-latest)
8. Publish the most recent Windows Core image into a ACR that is available in the Microsoft Container Registry.
    - The command should be `az acr import --name myreg1 --source mcr.microsoft.com/windows/servercore:latest -t servercore:latest`. This will import the image into the ACR and publish it to myreg1.
    - Other options were:
      - *az acr replication*. This command is used for geo-replicated regions of ACRs, not the ACR content.
      - *az acr update*. This command is used to update parameters for an ACR, such as enabling an administrative account or updating tags.
      - *az acr create*. This command will create a container registry.
      - *--registry*. This parameter will specify a source registry.
      - *--image*. This parameter will identify the image you are importing.
      - *-n*. This parameter will specify the destination ACR.
      - *-r*. This parameter will define a source ACR.
    - References: [Import container images to a container registry](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-import-images?tabs=azure-cli), [az acr](https://docs.microsoft.com/en-us/cli/azure/acr?view=azure-cli-latest), [Tutorial: Build and deploy container images in the cloud with Azure Container Registry Tasks](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-tutorial-quick-task)
9. Create a ARM template based on the current state of resource group myRG and its resources.
    - The correct command should be `az group export --name myRG`. This will export an ARM template of the group myRG.
    - Other options were:
      - *az group create*. This command will create a resource group.
      - *az group deployment*. This command will export the template used for a deployment.
      - *az group deployment create*. This command will start a deployment from a specific template file
      - *myACR*. This parameter is the name of the resource, while we want a template from the resource group.
    - References: [Use Azure portal to export a template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/export-template-portal), [Manage Azure Resource Manager resource groups by using Azure CLI](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-cli), [az group](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest), [az group deployment](https://docs.microsoft.com/en-us/cli/azure/group/deployment?view=azure-cli-latest)
10. Creating a generalized vm image.
    - First you should use `Stop-AzVm -ResourceGroupName $rgName -Name $vmName -Force`. This will deallocate the vm so we can create an image.
    - Secondly, you should use `Set-AzVM -ResourceGroupName $rgName -Name $vmName -Generalized`. This will generalize the vm status.
    - Third, you should use `$vm = Get-AzVm -Name $vmName -ResourceGroupName $rgName`. This will set the vm into the $vm variable.
    - Fourth, you should use `$image = New-AzImageConfig -Location $location -SourceVirtualMachineId $vm.Id`. This will create the image configuration and load it into the $image variable.
    - Fifth, you should use `New-AzImage -Image $image -ImageName $imageName -ResourceGroupName $rgName`. This will create the image itself using all the other variables.
    - References: [Create a managed image of a generalized VM in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource), [Create a VM from a managed image](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/create-vm-generalized-managed)
11. Creating a flexible ARM template with the same configuration but a different name.
    - You should create a parameter section in the ARM template and define the parameters. This provides the flexibility while still creating the same vm.
    - Other options were:
      - *Create a variable section*. We don't want to modify the template each time.
      - *Create separate templates*. We don't want to have separate templates.
      - *Create linked templates*. We don't want to rely on each developer to provide the logic to create a vm in their template.
    - References: [Understand the structure and syntax of ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/syntax), [Parameters in ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/parameters), [Variables in ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/variables)
12. Enable the generation of (queryable) logs by an Linux Azure App Service.
    - You should enable AppServiceAppLogs under Diagnostics settings and set the destination to send logs to Log Analytics. The AppServiceAppLogs are exceptions and/or logs written by the application. Sending the logs to Log Analytics allows teams to write Kusto queries.
    - Other options were:
      - *Enabling AllMetrics*. This will retrieve data by agents on the App Service and report the usage of host resources (like CPU, memory, disk I/O, etc.).
      - *Sending logs to storage accounts*. While this can be good to store logs, you can't query them.
    - References: [Enable diagnostics logging for apps in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/troubleshoot-diagnostic-logs)
13. Configure an app that receives messages from a queue to scale automatically.
    - First, you should set the metric source to *Service Bus* queue. This is done because we want to measure the service bus and not the application itself.
    - Secondly, you want to check for *Active Messages Processed/Instance (Avg)*, because we are only concerned about messages being processed. The other option (*Messages Processed/Instance (Avg)*) should be selected when you want to know the messages in the queue irrespective of whether they are being processed.
    - Third, you want the operator to be *Less than or equal to*. This is because we want to include the trigger value itself also.
    - Fourth, you want to select the *average* metric. We don't want to have the *least*, *max* or *sum* because of scaling reasons.
    - Fifth, you want to select *Decrease count by*, because we don't want to increment according to the text.
    - References: [Get started with Autoscale in Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/autoscale/autoscale-get-started?toc=%2Fazure%2Fapp-service%2Ftoc.json)
14. Creating an Azure CLI script to create web apps and deploy them to a deployment slot.
    - The first command should be `az group create --location centralus --name $resourcegroupname`. We always start by creating a group.
    - The second command should be `az appservice plan create --name $appPlan --resource-group $resourcegroupname --sku P3V2`. After the group, we can create an App Service Plan.
    - The third command should be `az webapp create --name $appName --resource-group $resourcegroupname --plan $appPlan`. With the App Service Plan in place, we can add an web app.
    - The fourth command should be `az webapp deployment slot create --name $appName --resource-group $resourcegroupname --slot $slotName`. We first need to create an deployment slot before we use it.
    - The fifth command should be `az webapp deployment source config --name $appName --resource-group $resourcegroupname --slot $slotName --repo-url $gitRepo --branch master --manual-integration`. After we have created the deployment slot, we can configure it. This now points to an GitHub repository.
    - References: [Create an App Service app and deploy code to a staging environment using Azure CLI](https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-deploy-staging-environment)
15. Configure another app to scale out by adding a rule to the queue.
    - We first want to only scale out when the value is *Greater than*. With the other values, it won't scale out when the queue size increases.
    - Furthermore, the aggregation type should be set to *Average*. This will aggregate the last five minutes of active messages every minute from a queue (also configurable). The other values are other thresholds, not suitable for this use case.
    - At last, we want to set the threshold value to *500*, because we don't want to scale to early/to late (this is also stated in the text).
    - References: [Get started with Autoscale in Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/autoscale/autoscale-get-started), [Monitoring Azure Service Bus data reference](https://docs.microsoft.com/en-us/azure/service-bus-messaging/monitor-service-bus-reference)
16. Create another Azure CLI script that should create and deploy a Linux application.
    - First we want to create the group: `az group create --location centralus --name group1`.
    - After the group creation, we can create the App Service plan: `az appservice plan create --name linux-service-plan --resource-group group1 --is-linux --sku S1`.
    - Now we can create the Web app: `az webapp create --name application1 --resource-group group1 --plan linux-service-plan`.
    - And finally, we can configure where to deploy when code has been pushed to a GitHub repo: `az webapp deployment source config --name application1 --resource-group group1 --repo-url $gitRepo --branch master --manual-integration`.
    - Things to note:
      - The *--is-linux* parameter will create an App Service Plan on Linux platform.
      - The *--manual-integration* parameter is used to perform a one time deploy, without enabling automatic sync between source control and App Service.
    - References: [Create an App Service app with deployment from GitHub using Azure CLI](https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-deploy-github)

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

1. Creating a Dockerfile that will build and publish the container image to an Azure Container Registry (ACR).
    - First you should build the Dockerfile with `docker build`. This will create a new container image with the instructions in it.
    - Secondly, you should tag the image with `{{ACR_name_here}}.azurecr.io/{{App_name_here}}`. This is needed to push a container image to the ACR.
    - Third, login in the {{ACR_name_here}} with `az acr login` and push the image to {{ACR_name_here}}. If you don't login, it doesn't know where to push. Don't confuse this with `az acr pack`, because this command is to build container images using Cloud Native Buildpacks using ACR Tasks.
    - References: [Push your first image to your Azure container registry using the Docker CLI](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli?tabs=azure-cli), [az acr pack](https://docs.microsoft.com/en-us/cli/azure/acr/pack?view=azure-cli-latest)
2. Encrypt a VHD and create a virtual machine when deploying on Azure.
    - First you should encrypt the VHD using BitLocker without a TPM and upload it to an Azure storage account. Note that a TPM is chip on a motherboard that the virtual machines do not have (and thus not support TPM).
    - Secondly, you should run the `Set-AzVMOSDisk` PowerShell command. This will set the disk as an OS disk for the virtual machine.
    - Third, you should run the `New-AzVM` PowerShell command. This will create the new VM.
    - Not that if you are already running the VM and want to encrypt the disk, you should use the `Set-AzVMDiskEncryptionExtension` command.
    - References: [What is a TPM, and Why Does Windows Need One For Disk Encryption?](https://www.howtogeek.com/237232/what-is-a-tpm-and-why-does-windows-need-one-for-disk-encryption/), [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/en-us/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-6.6.0&viewFallbackFrom=azps-3.6.1), [https://docs.microsoft.com/en-us/powershell/module/az.compute/set-azvmosdisk?view=azps-6.6.0&viewFallbackFrom=azps-3.6.1](Set-AzVMOSDisk)
3. Create a ARM template from a resource group that you will use as your deployment source by PowerShell.
    - Use the `Export-AzResourceGroup` cmdlet. This will retrieve all the resources or (by specification) retrieve a single VM or something like that.
    - The other options were:
      - *Register-AzResourceProvider*. This command is used for registering a resource provider for your Azure account and has nothing to do with this question.
      - *Get-AzResourceGroup*. This command will retrieve information about the resource group, but it doesn't create an ARM template.
      - *Save-AzDeploymentTemplate*. This command will save an existing template to a file, but not create a template from an existing resource group.
      - References: [Download the template for a VM](https://docs.microsoft.com/en-us/previous-versions/azure/virtual-machines/windows/download-template), [Export-AzResourceGroup](https://docs.microsoft.com/en-us/powershell/module/az.resources/export-azresourcegroup?view=azps-6.6.0&viewFallbackFrom=azps-3.8.0), [Register-AzResourceProvider](https://docs.microsoft.com/en-us/powershell/module/az.resources/register-azresourceprovider?view=azps-6.6.0&viewFallbackFrom=azps-3.8.0), [Get-AzResourceGroup](https://docs.microsoft.com/en-us/powershell/module/az.resources/Get-AzResourceGroup?view=azps-6.6.0&viewFallbackFrom=azps-3.8.0)
4. Uploading an on-premise Windows VM to the cloud and encrypt it.
    - Azure Disk encryption is supported for generation 1 and generation 2 VMs. The only thing not supported on the generation 2 VMs is 32-bit systems.
    - You must convert the VHDX file to VHD before uploading it to Azure. This is because VHDX is not supported there.
    - You must convert the dynamic disk to a fixed-size disk before uploading. This is needed to support encryption, because that isn't supported on a dynamic disk.
    - References [Prepare a Windows VHD or VHDX to upload to Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/prepare-for-upload-vhd-image), [Azure Disk Encryption for Windows VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-overview), [Azure Disk Encryption for Windows virtual machines FAQ](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-faq)
5. Generate a container image.
    - Use the `az acr build` command. This will (after a successful build), push the image to your container registry.
    - The other options were:
      - *az acr create*. This command will create a container registry.
      - *az acr update*. This command is used to update parameters for an ACR, such as enabling an administrative account or updating tags.
      - *az acr import*. This command will import an image to an ACR from another ACR.
      - References [Quickstart: Build and run a container image using Azure Container Registry Tasks](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-quickstart-task-cli), [az acr](https://docs.microsoft.com/en-us/cli/azure/acr?view=azure-cli-latest)
6. Configure and access a Linux VM from your local machine.
    - The first command is `adminUser='az vm show -g group1 -n vm1 --query osProfile.adminUsername -o tsv'`. This will retrieve the admin username from vm1.
    - The second command is `publicIp='az vm show -g group1 -n vm1 -d --query publicIps -o tsv`. This will retrieve the public IP from vm1 (don't forget the `-d` that will list the publicIps).
    - The last command is `ssh $adminUser@$publicIp`. This will simply make a SSH connection with the stored variables.
    - The other options were:
      - *az vm nic show*. This command will show information about a specific network adapter.
      - *privateIps*. This will show you the private IP of the vm. We want the public one.
      - *mstsc /v:*. This will connect by Remote Desktop Protocol (RDP). We use a Linux vm.
    - References: [Tutorial: Create and Manage Linux VMs with the Azure CLI](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm), [How to query Azure CLI command output using a JMESPath query](https://docs.microsoft.com/en-us/cli/azure/query-azure-cli?view=azure-cli-latest), [Tutorial: Create and Manage Windows VMs with Azure PowerShell](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-manage-vm), [az vm nic](https://docs.microsoft.com/en-us/cli/azure/vm/nic?view=azure-cli-latest)
7. Publish the most recent Windows Core image into a ACR that is available in the Microsoft Container Registry.
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
8. Create a ARM template based on the current state of resource group myRG and its resources.
    - The correct command should be `az group export --name myRG`. This will export an ARM template of the group myRG.
    - Other options were:
      - *az group create*. This command will create a resource group.
      - *az group deployment*. This command will export the template used for a deployment.
      - *az group deployment create*. This command will start a deployment from a specific template file
      - *myACR*. This parameter is the name of the resource, while we want a template from the resource group.
    - References: [Use Azure portal to export a template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/export-template-portal), [Manage Azure Resource Manager resource groups by using Azure CLI](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-cli), [az group](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest), [az group deployment](https://docs.microsoft.com/en-us/cli/azure/group/deployment?view=azure-cli-latest)
9. Creating a generalized vm image.
    - First you should use `Stop-AzVm -ResourceGroupName $rgName -Name $vmName -Force`. This will deallocate the vm so we can create an image.
    - Secondly, you should use `Set-AzVM -ResourceGroupName $rgName -Name $vmName -Generalized`. This will generalize the vm status.
    - Third, you should use `$vm = Get-AzVm -Name $vmName -ResourceGroupName $rgName`. This will set the vm into the $vm variable.
    - Fourth, you should use `$image = New-AzImageConfig -Location $location -SourceVirtualMachineId $vm.Id`. This will create the image configuration and load it into the $image variable.
    - Fifth, you should use `New-AzImage -Image $image -ImageName $imageName -ResourceGroupName $rgName`. This will create the image itself using all the other variables.
    - References: [Create a managed image of a generalized VM in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource), [Create a VM from a managed image](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/create-vm-generalized-managed)
10. Creating a flexible ARM template with the same configuration but a different name.
    - You should create a parameter section in the ARM template and define the parameters. This provides the flexibility while still creating the same vm.
    - Other options were:
      - *Create a variable section*. We don't want to modify the template each time.
      - *Create separate templates*. We don't want to have separate templates.
      - *Create linked templates*. We don't want to rely on each developer to provide the logic to create a vm in their template.
    - References: [Understand the structure and syntax of ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/syntax), [Parameters in ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/parameters), [Variables in ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/variables)
11. Enable the generation of (queryable) logs by an Linux Azure App Service.
    - You should enable AppServiceAppLogs under Diagnostics settings and set the destination to send logs to Log Analytics. The AppServiceAppLogs are exceptions and/or logs written by the application. Sending the logs to Log Analytics allows teams to write Kusto queries.
    - Other options were:
      - *Enabling AllMetrics*. This will retrieve data by agents on the App Service and report the usage of host resources (like CPU, memory, disk I/O, etc.).
      - *Sending logs to storage accounts*. While this can be good to store logs, you can't query them.
    - References: [Enable diagnostics logging for apps in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/troubleshoot-diagnostic-logs)
12. Configure an app that receives messages from a queue to scale automatically.
    - First, you should set the metric source to *Service Bus* queue. This is done because we want to measure the service bus and not the application itself.
    - Secondly, you want to check for *Active Messages Processed/Instance (Avg)*, because we are only concerned about messages being processed. The other option (*Messages Processed/Instance (Avg)*) should be selected when you want to know the messages in the queue irrespective of whether they are being processed.
    - Third, you want the operator to be *Less than or equal to*. This is because we want to include the trigger value itself also.
    - Fourth, you want to select the *average* metric. We don't want to have the *least*, *max* or *sum* because of scaling reasons.
    - Fifth, you want to select *Decrease count by*, because we don't want to increment according to the text.
    - References: [Get started with Autoscale in Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/autoscale/autoscale-get-started?toc=%2Fazure%2Fapp-service%2Ftoc.json)
13. Creating an Azure CLI script to create web apps and deploy them to a deployment slot.
    - The first command should be `az group create --location centralus --name $resourcegroupname`. We always start by creating a group.
    - The second command should be `az appservice plan create --name $appPlan --resource-group $resourcegroupname --sku P3V2`. After the group, we can create an App Service Plan.
    - The third command should be `az webapp create --name $appName --resource-group $resourcegroupname --plan $appPlan`. With the App Service Plan in place, we can add an web app.
    - The fourth command should be `az webapp deployment slot create --name $appName --resource-group $resourcegroupname --slot $slotName`. We first need to create an deployment slot before we use it.
    - The fifth command should be `az webapp deployment source config --name $appName --resource-group $resourcegroupname --slot $slotName --repo-url $gitRepo --branch master --manual-integration`. After we have created the deployment slot, we can configure it. This now points to an GitHub repository.
    - References: [Create an App Service app and deploy code to a staging environment using Azure CLI](https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-deploy-staging-environment)
14. Configure another app to scale out by adding a rule to the queue.
    - We first want to only scale out when the value is *Greater than*. With the other values, it won't scale out when the queue size increases.
    - Furthermore, the aggregation type should be set to *Average*. This will aggregate the last five minutes of active messages every minute from a queue (also configurable). The other values are other thresholds, not suitable for this use case.
    - At last, we want to set the threshold value to *500*, because we don't want to scale to early/to late (this is also stated in the text).
    - References: [Get started with Autoscale in Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/autoscale/autoscale-get-started), [Monitoring Azure Service Bus data reference](https://docs.microsoft.com/en-us/azure/service-bus-messaging/monitor-service-bus-reference)
15. Create another Azure CLI script that should create and deploy a Linux application.
    - First we want to create the group: `az group create --location centralus --name group1`.
    - After the group creation, we can create the App Service plan: `az appservice plan create --name linux-service-plan --resource-group group1 --is-linux --sku S1`.
    - Now we can create the Web app: `az webapp create --name application1 --resource-group group1 --plan linux-service-plan`.
    - And finally, we can configure where to deploy when code has been pushed to a GitHub repo: `az webapp deployment source config --name application1 --resource-group group1 --repo-url $gitRepo --branch master --manual-integration`.
    - Things to note:
      - The *--is-linux* parameter will create an App Service Plan on Linux platform.
      - The *--manual-integration* parameter is used to perform a one time deploy, without enabling automatic sync between source control and App Service.
    - References: [Create an App Service app with deployment from GitHub using Azure CLI](https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-deploy-github)
16. Create a Dockerfile for a C# project that processes the first build of the application and then deploys it over port 443.
    - The first part of the dockerfile will set up the image with the aspnetcore runtime environment while also setting the working directory to `/app` en expose port 443:  
    `FROM microsoft/dotnet:2.2-aspnetcore-runtime-stretch-slim AS base`  
    `WORKDIR /app`  
    `EXPOSE 433`
    - The second part will set up the image that we are going to use while building. The myContainerApp.csproj file needs to be built into myContainerApp.dll before it can be deployed:  
    `FROM microsoft/dotnet:2.2-sdk-stretch AS build`  
    `WORKDIR /myAppWorkingDir`  
    `RUN dotnet build "myContainerApp.csproj" -c Release -o /app`
    - Now we need to create a publish label for myContainerApp.dll. You can use that as a source to deploy the application:  
    `FROM build AS publish`  
    `RUN dotnet publish "myContainerApp.csproj" -c Release -o /app`
    - After publishing, take the runtime image definition (base) and copy the published version of myContainerApp.dll to the app folder:  
    `FROM base AS final`  
    `WORKDIR /app`  
    `COPY --from=publish /app .`
    - Finally define the entry point configuration where your docker image should route the requests to:  
    `ENTRYPOINT ["dotnet", "myContainerApp.dll"]`
    - References: [Quickstart: Docker in Visual Studio](https://docs.microsoft.com/en-us/visualstudio/containers/container-tools?view=vs-2019)
17. Run commands to create a Azure App Service Web App for Containers.
    - First you should create a group: `az group create`.
    - Then we need to create the App Service Plan: `az appservice plan create`
    - Finally create the application: `az webapp create`.
    - Other options were:
      - `az container create`. We don't need to create a Azure Container Instance.
      - `az group update`. We don't update the group.
    - References: [Create an ASP.NET Core app in a Docker container from Docker Hub using Azure CLI](https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-linux-docker-aspnetcore), [az group](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest), [az appservice plan](https://docs.microsoft.com/en-us/cli/azure/appservice/plan?view=azure-cli-latest), [az webapp](https://docs.microsoft.com/en-us/cli/azure/webapp?view=azure-cli-latest), [az container](https://docs.microsoft.com/en-us/cli/azure/container?view=azure-cli-latest)
18. Create a web application that is composed of two containers that are configured using Docker Compose.
    - First we need to specify over which resource group we are talking with `az webapp create --resource-group blogResourceGroup`.
    - The plan name is also defined in the text: `--plan blogServicePlan`.
    - Just like the plan name, the name of the app is also specified: `--name blog`.
    - Then we need to choose `--multicontainer-config-type compose`, because we are creating a web app that uses a Docker Compose file as the application configuration.
    - Finally we also need to choose `--multicontainer-config-file docker-compose.yml`, because it has been specified that we need to use that file.
    - Other options were:
      - `--multicontainer-config-type kube`. We are not using kubernetes so we don't need this parameter.
      - `--multicontainer-config-file kube.yml`. There is no specification of this file.
      - `--docker-custom-image-name php:apache-7`. We have all the specifications inside the Docker File so this is not needed.
    - References: [Tutorial: Create a multi-container (preview) app in Web App for Containers](https://docs.microsoft.com/en-us/azure/app-service/tutorial-multi-container-app), [az webapp](https://docs.microsoft.com/en-us/cli/azure/webapp?view=azure-cli-latest), [Understanding Kubernetes Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)
19. Create, build, tag and push a web application in a Docker container to a public repository.
    - The first part is all about matching the correct variables with the correct commands. This should be fairly easy just looking at the commands.
    - The most important command (`az webapp config container set --docker-custom-image-name $dockerHubContainerPath --name $appName --resource-group myResourceGroup`) is configuring the container image that will pushed to Docker Hub and that will be used by the Web App. The variable `$dockerHubContainerPath` stands for `company1/asp-app`. This is because of the default pattern `<organization/username>/<image>`.
    - Other options were:
      - `az webapp config set ...`. This command can be used to configure default web app configuration.
      - `az webapp config container set --docker-registry-server-url $dockerHubContainerPath ...`. This parameter will reference a custom Docker container registry, but we are just using the default one.
      - `$dockerHubContainerPath = company1.azurecr.io/asp-app`. This will set the path to an Azure Container Registry (ACR) named company1, but we want to push our image the the Docker Hub registry.
    - References: [Create an ASP.NET Core app in a Docker container from Docker Hub using Azure CLI](https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-linux-docker-aspnetcore), [az webapp config](https://docs.microsoft.com/en-us/cli/azure/webapp/config?view=azure-cli-latest)
20. Monitor the CPU usage of a web application on a Azure Service Plan standard tier.
    - First you need to use the *CPU Percentage metric*. This will check the CPU usage over time.
    - Then if you want to monitor the CPU usage App Service, you need to monitor the *App Service Plan*. This resource will contain all the data you need related to VM usage, like CPU.
    - Other options were:
      - *webapp1*. You need to montior the App Service Plan for this.
      - *group1*. You need to montior the App Service Plan for this.
      - *CPU Time metric*. Only use this for monitoring when you are running the App Service plan with quota restrictions like Free or Shared plans.
      - *Average Response Time metric*. This does not measure CPU usage.
    - References: [Monitor apps in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/web-sites-monitor)
21. Statements about Filesystem and Blob storage.
    - Filesystem and Blob storage options are **not** supported for both Windows and Linux apps. Only Filesystem storage is supported for Linux apps.
    - When using Blob storage, the storage account **must** be in the same region as the App Service.
    - Filesystem storage **is designed** for short-term logging and turns itself off after 12 hours.
    - References: [Enable diagnostics logging for apps in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/troubleshoot-diagnostic-logs), [Monitor apps in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/web-sites-monitor)
22. Make a client certificate (CA) available for your ASP.NET app so you can use Transport Layer Security (TLS).
    - For ASP.NET apps, it's only available through the *HttpRequest.ClientCertificate* property.
    - Other options were:
      - *Through the HTTPS request header*. This option is suitable for other application types like Node.JS, were the certificate is passed as a base64-encoded X-ARR-CLientCert header.
      - *Through the client cookie*. This is meant for local storage and not certificates.
      - *Through an URL query string*. This isn't a valid method to pass certificates.
    - References: [Configure TLS mutual authentication for Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-configure-tls-mutual-auth), [certreq](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certreq_1)
23. Fixing cross domain origin requests within an SPA and an API.
    - You should run the command `az webapp cors` with the `--allowed-origins=https://<website_here>.com` parameter on the API. This will add a Cross-Origin Resource Sharing (CORS) rule that will allow all requests from the SPA domain to work.
    - Other options were:
      - *Run the az storage cors command in spa1*. This won't work because you need to run the command on the destination domain.
      - *Disable the Application Request Routing (ARR) affinity setting in api1*. Only disable the ARR affinity for stateless applications that do not require the client to use the same App Service instance for the life of the session.
      - *Change the spa1 domain to https://spa.company1.com*. Because the host will still be different, this also won't work.
    - References: [Tutorial: Host a RESTful API with CORS in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-tutorial-rest-api), [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS), [az storage cors](https://docs.microsoft.com/en-us/cli/azure/storage/cors?view=azure-cli-latest), [Configure an App Service app in the Azure portal](https://docs.microsoft.com/en-us/azure/app-service/configure-common), [Choose Between Traditional Web Apps and Single Page Apps (SPAs)](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/choose-between-traditional-web-and-single-page-apps), [Using the Application Request Routing Module](https://docs.microsoft.com/en-us/iis/extensions/planning-for-arr/using-the-application-request-routing-module)
24. Update an application so the connection string will also be updated based on the current environment.
    - First you must set the connection string attribute to a setting name in your code. The framework provides the DbConfigurationType attribute to set the connection string. Azure will read from this when deployed.
    - Then you should use the configuration setting on the Azure function and local.settings.json in your local environment to set the connection string. Your local machine will read from this.
    - Other options were:
      - *Set the connection string in the host.json file in each environment*. The host.json file should only be used for configurations that define how your execution environment is configured.
      - *Use global variables that read the setting from a keyvault on function startup*. This will make the connection string globally available, what shouldn't be the case for this casus.
    - References: [DbConfigurationTypeAttribute Class](https://docs.microsoft.com/en-us/dotnet/api/system.data.entity.dbconfigurationtypeattribute?view=entity-framework-6.2.0), [How to specify EntityFramework ProviderName in an Azure Function](https://stackoverflow.com/questions/39916981/how-to-specify-entityframework-providername-in-an-azure-function)
25. Deploying a single ASP.NET Core application on an Azure Container Instance (ACI). Sidecar a container on the same host that doesn't increase cost and requires minimal change.
    - We should use *ACI Container Groups* (yes, it's the same service). They provide the capability to host a set of containers on the same host machine, share the same resources and can communicate with each other. You can also install monitoring agents inside the separate container instances.
    - The other options were:
      - *Azure Container Registry*. This is where the container images are hosted.
      - *Kubernetes Pods*. Will work, but because we already have ACI working, this would only increase the cost and complexity.
      - *Azure App Services*. Will work, but because we already have ACI working, this would only increase the cost and complexity.
    - References: [Container groups in Azure Container Instances](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-container-groups), [Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/), [Tutorial: Deploy a multi-container group using a Resource Manager template](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-multi-container-group), [Introduction to private Docker container registries in Azure](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-intro)
26. Fix a durable function that should aggregate the results based on a fan ou/fan in pattern.
    - You should use add the `await Task.WhenAll(parallelTasks);` to your code. This should wait on all the parallel functions for them to be completed.
    - Other options were:
      - *Add `await context.CreateTimer(deadline, CancellationToken.None);`*. This only pauses the application for three seconds and does not ensure that all the functions have been executed.
      - *Change the `CallActivityAsync` parameter to `OrdersProcessed`*. This change will only make you run a wrong function.
      - *Add `Thread.Sleep(50);`*. This will only make the execution delayed by 50 milliseconds and does not ensure that they are all executed.
    - References: [What are Durable Functions?](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp), [Timers in Durable Functions (Azure Functions)](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-timers?tabs=csharp)
27. Determine the correct NCRONTAB expression.
    - The correct expression for *every 3 hours* should be `"0 0 */3 * * *"`. For example, this can you like `TimeTrigger("0 0 */3 * * *)]TimerInfo myTimer` as a parameter of a function.
    - Other options were:
      - `"0 */3 * * * *`. This stands for every three minutes.
      - `"0 0 3 * * *`. This stands for every day at 3 AM.
      - `"0 0 0 * * 3`. This stands for midnight every Wednesday.
    - References: [Create a function in the Azure portal that runs on a schedule](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-scheduled-function), [Timer trigger for Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-timer?tabs=csharp)
28. Choose Azure function patterns for different scenario's
    - When we have a series of functions that need to be executed in a specific order, we should use the *function chaining application pattern*. This will be implemented as a set of orchestrator functions that orchestrate the execution order of other Durable functions.
    - When we have multiple functions that execute in parallel and wait for result aggregation, we should use the *fan out/fan in application pattern*. Tis is also implemented as orchestrator functions that can call other durable functions with output form called functions saved to local variables.
    - When we have event data provided in batches by multiple sources over time that should be combined into a single entity, we should use the *aggregation application pattern*. Each function acts like a small services that communicates via messages.
    - Other options were:
      - *Async HTTP APIs application pattern*. Only used when coordinating the state of long-running operations with external clients.
      - *Monitoring application pattern*. Only used to support a flexible, recurring process in a workflow, often with a timer trigger.
    - References: [What are Durable Functions?](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp), [Durable Functions types and features](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-types-features-overview), [Durable Orchestrations](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-orchestrations?tabs=csharp), [Entity functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-entities?tabs=csharp)
29. Modify the host.json file from a function to set runtime-specific configuration information.
    - You should use the *Azure Portal* to modify the host.json file with the function editor.
    - Other options were:
      - *Azure PowerShell cmdlets*. Doesn't have suffice commands to update the host.json file.
      - *Azure CLI commands*. Doesn't have suffice commands to update the host.json file.
      - *Text editor*. Just do it through the editor.
    - References: [Introduction to Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview), [Azure Functions developer guide](https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference?tabs=blob), [Azure Functions Java developer guide](https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference-java?tabs=bash%2Cconsumption)

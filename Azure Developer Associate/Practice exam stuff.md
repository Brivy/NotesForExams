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
12. Configure access for app service to the azure key vault by enabling system generated managed identity with *get* and *list* permissions?
    - The answer should be *Yes*, because this created identity gets access to the key vault to read and list secrets stored there.
    - References: [How to use managed identities for App Service and Azure Functions](https://docs.microsoft.com/en-us/azure/app-service/overview-managed-identity?tabs=dotnet), [Azure Key Vault security](https://docs.microsoft.com/en-us/azure/key-vault/general/security-features)
13. Configure access for app service to the azure key vault by enabling system generated managed identity with *write* and *delete* permissions?
    - The answer should be *No*, because the created identity gets access to the key vault to write and delete secrets, while this isn't the use case.
    - References: [How to use managed identities for App Service and Azure Functions](https://docs.microsoft.com/en-us/azure/app-service/overview-managed-identity?tabs=dotnet), [Azure Key Vault security](https://docs.microsoft.com/en-us/azure/key-vault/general/security-features)
14. Configure access for app service to the azure key vault by configuring an Azure Active Directory application that has get and list permissions, but store the access key and password in the service principal in the application.
    - The answer should be *No*, because you now give everyone access to the key vault that is using the application.
    - References: [How to use managed identities for App Service and Azure Functions](https://docs.microsoft.com/en-us/azure/app-service/overview-managed-identity?tabs=dotnet), [Azure Key Vault security](https://docs.microsoft.com/en-us/azure/key-vault/general/security-features)

## Normal questions

These questions aren't related to each other.

### Azure compute questions

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
30. Running a few Python scripts in s serverless application that reminds states.
    - You should go for *Durable functions*. These are functions that remind states, can be function chained and can be executed in Python.
    - Other options were:
      - *Azure Logic App*. Not suitable for Python scripts.
      - *WebJobs*. Not serverless.
      - *Power Automate*. Not suitable for Python scripts.
    - References: [What are Durable Functions?](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp), [Choose the right integration and automation services in Azure](https://docs.microsoft.com/en-us/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs?toc=/azure/azure-functions/durable/toc.json), [What is Azure Logic Apps?](https://docs.microsoft.com/en-gb/azure/logic-apps/logic-apps-overview), [Get started with Power Automate](https://docs.microsoft.com/en-us/power-automate/getting-started), [Run background tasks with WebJobs in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/webjobs-create)
31. Function programming questions.
    - If you code in a language not supported by Azure Functions app, should include a custom handler executable. This can be located anywhere, as long as you specify the *path to the executable in your host.json configuration file*.
    - The host.json file should *always* be located at the root of your Azure Functions app.
    - The function.json file is a metadata file that defines your Azure Function, so you *should place it inside a folder with the same name as your Azure Functions name*.
    - References: [Introduction to Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview), [Azure Functions custom handlers](https://docs.microsoft.com/en-us/azure/azure-functions/functions-custom-handlers)

### Azure Storage questions

1. Accessing the first item in a Azure Cosmos DB Account
    - First you should create a `DocumentClient` object that represents a connection to a Cosmos DB account. This can be done with `new DocumentCLient(uri, accessKey)`.
    - Then we want ot retrieve the databases inside the account. This can be done with `var tasks = dc.CreateDatabaseQuery().Where(x => x.Id == "Test").AsEnumerable().First();`.
    - Now that we know the database, we want to retrieve a container/collection from the database. This can be done with `var items = dc.CreateDocumentCollectionQuery(tasks.SelfLink).Where(d => d.Id == "TestItems").AsEnumerable().First();`.
    - To retrieve the item we are looking for from the container/collection, we must query it. This can be done with `var item = dc.CreateDocumentQuery(items.SelfLink).AsEnumerable().First();`.
    - References: [Azure Cosmos DB.NET V3 SDK (Microsoft.Azure.Cosmos) examples for the SQL API](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/sql-api-dotnet-v3sdk-samples), [DocumentQueryable.CreateDatabaseQuery Method](https://docs.microsoft.com/en-us/previous-versions/azure/dn850286(v%3dazure.100)), [DocumentQueryable.CreateDocumentCollectionQuery Method](https://docs.microsoft.com/en-us/previous-versions/azure/dn850284(v%3dazure.100)), [DocumentQueryable.CreateDocumentQuery Method](https://docs.microsoft.com/en-us/previous-versions/azure/dn850285(v=azure.100)), [Tutorial: Build a .NET console app to manage data in Azure Cosmos DB SQL API account](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/sql-api-get-started), [Azure Cosmos DB resource model](https://docs.microsoft.com/en-us/azure/cosmos-db/account-databases-containers-items)
2. Ensure that a client never receives a partial write of data when they retrieve data from Cosmos DB.
    - By using *Strong* consistency level, you ensure that clients never receive a partial write or uncommitted data during a read operation.
    - Other options were:
      - *Consistent prefix*. Ensures that reads never see out of order writes, but does not prevent reads from seeing partial writes.
      - *Eventual*. No read guarantee and clients can potentially receive a partial write or uncommitted data during a read operation.
      - *Session*. This does not prevent a client from receiving partial writes.
    - References: [Consistency levels in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels)
3. Accessing the first item in a container by using the Query Explorer in Cosmos DB.
    - You should use `SELECT TOP 1 i.name, i.description FROM i`. This does not have syntax errors in it. But I don't think this is the right answer, but sure.
    - Other options were:
      - `SELECT TOP 1 {name, description} FROM Items`. Has syntax errors.
      - `SELECT TOP 1 name, description FROM Items`. Has syntax errors.
      - `SELECT TOP 1 {name, description} FROM Tasks/Items`. Has syntax errors.
    - References: [Getting started with SQL queries](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/sql-query-getting-started), [FROM clause in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/sql-query-from)
4. Choosing a partition key on an Azure Cosmos DB.
    - A partition key allows you to group documents into logical partitions. All documents that have the same partition key share the same partition. When you choose a partition, you should choose one that spreads a workload evenly over time. By this logic the partition key should be set on *project*.
    - Other options were:
      - *Amount*. Value has a too large of a range.
      - *Number*. Value is unique, so it will create new partitions every time.
      - *Date*. Almost correct, but there are still far too many different dates.
    - References: [Partitioning and horizontal scaling in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/partitioning-overview)
5. Create a query that returns all receipts, while also being the only database/collection in the Cosmos DB account.
    - The first option can be `SELECT * FROM receipts r WHERE r.category = "Lodging"`. This is a standard query with collection aliases, which are required when column names are directly referenced in other SQL clauses, including SELECT, WHERE ORDER BY and GROUP BY.
    - The second option was `SELECT * FROM root r WHERE r.category = "Lodging"`. This queries the default collection (receipts is the only one in the database) with collection aliases, which are required when column names are directly referenced in other SQL clauses, including SELECT, WHERE ORDER BY and GROUP BY.
    - Other options were:
      - `SELECT * FROM root WHERE category = "Lodging"`. Collection aliases are required when column names are directly referenced in other SQL clauses, including SELECT, WHERE ORDER BY and GROUP BY.
      - `SELECT * FROM receipts WHERE category = "Lodging"`. Collection aliases are required when column names are directly referenced in other SQL clauses, including SELECT, WHERE ORDER BY and GROUP BY.
    - References: [Getting started with SQL queries](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/sql-query-getting-started)
6. Evenly distribute documents across multiple containers by using a partition key.
    - By *creating a partition key from concatenating multiple properties*, you create **synthetic keys**. Because the keys are not consistent, such a concatenated key will create randomness to enable proper distribution across various containers.
    - By *creating a hash value that is appended to the document property*, you will create randomness that properly distributes the documents.
    - Other options were:
      - *A property that is present in most of the documents*. With this, most of the documents will just end up in the same container.
      - *A property for which the value is the same 90 percent of the time*. With this, most of the documents will just end up in the same container.
    - References: [Partitioning and horizontal scaling in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/partitioning-overview), [Create a synthetic partition key](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/synthetic-partition-keys)
7. Recommend the most appropriate Cosmos DB API for an application that will gather large amounts of data and uses graph database algorithms for data anlysis.
    - By using *Gremlin API* (fully-managed data-based solution for graph data) as your data solution, you have native graph support that is widely used in scenarios in which you need to work with large amounts of data.
    - Other options were:
      - *Cassandra API*. Mostly used for Apache Casandra stuff and is not designed for graph data.
      - *MongoDB API*. Used for document, key-value, graph, and columnar data models.
      - *Core (SQL) API*. Does not support graph queries.
    - References: [Introduction to Gremlin API in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/graph/graph-introduction), [Understanding the differences between NoSQL and relational databases](https://docs.microsoft.com/en-us/azure/cosmos-db/relational-nosql), [Analyze the decision criteria](https://docs.microsoft.com/en-us/learn/modules/choose-api-for-cosmos-db/3-analyze-the-decision-criteria)
8. A lot of requirements for choosing a consistency level for Cosmos DB.
    - By following the requirements, we should implement the *staleness consistency level*. This includes the **consistent prefix**, which guarantees that data writes are returned in order. You can also configure the **staleness**, which determines the data version returned based on either the number of versions ore time delay.
    - Other options were:
      - *Consistent prefix*. While it has the consistent prefix, it does not let you configure staleness.
      - *Session*. While it has the consistent prefix, but only supports a single client session and does not let you configure staleness.
      - *Eventual*. Does not have the consistent prefix and staleness.
    - References: [Consistency levels in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels)
9. Azure Cosmos DB questions.
    - You can trigger user-defined functions from code. See also `var iterator = client.GetContainer("database", "container").GetItemQueryIterator<dynamic>("SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000");`.
    - While you *can't* write triggers, stored procedures and user-defined functions (UDFs) in a Cosmos DB database running Table API, you **can** do this by running the SQL API with JavaScript. Other API's wont' work.
    - Stored procedures must be written with Javascript (and not C#).
    - References: [Stored procedures, triggers, and user-defined functions](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/stored-procedures-triggers-udfs), [How to register and use stored procedures, triggers, and user-defined functions in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/how-to-use-stored-procedures-triggers-udfs), [JavaScript query API in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/javascript-query-api)
10. Database scaling with certain spikes on Azure Cosmos DB.
    - You should use *autoscale throughput* on the database itself. Because of the uncertain spikes, you shouldn't autoscale a specific container.
    - Other options were:
      - *Autoscale throughput on the container*. Because of the uncertain spikes, it's better to scale the database itself.
      - *Standard throughput on the container/database*. There is no steady traffic, so autoscale is better.
    - References: [Introduction to provisioned throughput in Azure Cosmos DB](https://docs.microsoft.com/en-us//azure/cosmos-db/set-throughput), [Create Azure Cosmos containers and databases with autoscale throughput](https://docs.microsoft.com/en-us//azure/cosmos-db/provision-throughput-autoscale), [How to choose between standard (manual) and autoscale provisioned throughput](https://docs.microsoft.com/en-us//azure/cosmos-db/how-to-choose-offer)
11. Guarantee the highest possible availability and lowest latency for data reads on Cosmos DB.
    - By using the *Eventual* consistency level, we can provide the lowest latency and higher availability, but this isn't necessary for a static database that won't receive new writes.
    - Other options were:
      - *Consistent prefix*. Is the most consistent, but has more latency and less availability.
      - *Session*. While it's very cool, it has more latency and less availability.
      - *Bounded staleness*. Can lag behind writes, but also have more latency and less availability.
      - *Strong*. While returning the most recent commited version of an item, you have more latency and less availability.
    - References: [Consistency levels in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels)
12. The result of executing a SQL query on Cosmos DB.
    - SELECT queries against a Cosmos SQL database using the SQL API returns data in JSON format.
    - Other options were:
      - *XML-formatted data*. None of the Cosmos DB API returns standard XML.
      - *A table structured as rows and columns*. When using the Table API, you will get this result.
      - *A syntax error*. It's normal SQL so no syntax errors in this example.
    - References: [Getting started with SQL queries](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/sql-query-getting-started), [Azure Cosmos DB: Designing your data structure](https://social.technet.microsoft.com/wiki/contents/articles/39421.azure-cosmos-db-designing-your-data-structure.aspx), [Tutorial: Query Azure Cosmos DB by using the Table API](https://docs.microsoft.com/en-us/azure/cosmos-db/table/tutorial-query-table), [Introduction to Azure Cosmos DB database and the SQL API](https://www.mssqltips.com/sqlservertip/5331/introduction-to-azure-cosmos-db-database-and-the-sql-api/)
13. Cosmos DB change feed questions.
    - Change feed is enabled in Cosmos DB by default. This is for all Cosmos DB accounts.
    - While change feed captures inserts and updates, it does *not* capture deletes. You can enable this by creating an additional property in you Cosmos DB items that functions as a marker and update it at the time of deletion.
    - While the change feed order is the guaranteed within each logical partition key value, it does *not* guarantee the order across the partition key values.
    - References: [Change feed in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/change-feed), [Change feed design patterns in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/change-feed-design-patterns), [Reading Azure Cosmos DB change feed](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/read-change-feed)
14. Access blob by code.
    - First, we should connect to our storage account by `CloudStorageAccount.TryParse(connectionString, out acct);`.
    - Then we create a client to communicate with `CloudBlobClient client = acct.CreateCloutBlobClient();`.
    - With the client we can access the container with `var container = client.GetContainerReference("thumbnails");`.
    - Now that we have the container, we want the blob with `var blob = container.GetBlobReferenceFromServer("personal/car.png");`.
    - And we can download that to a file with `blob.DownloadToFile("car.png", System.IO.FileMode.Create);`.
    - Don't forget that a Blob storage account URL has the following format: `https://{account name}.blob.core.windows.net/{container}/{blob}`.
    - References: [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/en-us/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata), [Quickstart: Azure Blob Storage client library v12 for .NET](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-dotnet)
15. Adding metadata to a container in an account.
    - You should always use *PUT* to add metadata to containers and blobs. The metadata for letting something identify as an image looks like `x-ms-meta-Category: Images`.
    - Other options were:
      - *GET*. Is used for retrieving metadata attributes.
      - *HEAD*. Is used for retrieving metadata attributes.
      - *POST*. Does not allow you to retrieve or set metadata attributes.
    - References: [Setting and retrieving properties and metadata for Blob service resources](https://docs.microsoft.com/en-us/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources), [Set Container Metadata](https://docs.microsoft.com/en-us/rest/api/storageservices/set-container-metadata), [Authorize requests to Azure Storage](https://docs.microsoft.com/en-us/rest/api/storageservices/authorize-requests-to-azure-storage), [HTTP Request Methods](https://www.w3schools.com/tags/ref_httpmethods.asp)
16. Adding metadata to specify a size of an image with Blobs.
    - First, the HTTP method should be *PUT* to add the metadata.
    - For actually updating the metadata, you need to use the following URL: `https://webdata.blob.core.windows.net/home/logo.png?comp=metadata`.
    - To define custom metadata, you must specify the HTTP header in format `x-ms-meta-{user defined name}`. In this example it will be `x-ms-meta-imagesize: Large`.
    - Other options were:
      - *GET/HEAD/POST*. HTTP methods that shouldn't be used for updating metadata.
      - *comp=properties*. Setting this inside the URL specifies that you want to update properties.
      - *Image-Size*. Using this as header doesn't follow the specific custom metadata format.
    - References: [Setting and retrieving properties and metadata for Blob service resources](https://docs.microsoft.com/en-us/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources), [Set Blob Metadata](https://docs.microsoft.com/en-us/rest/api/storageservices/set-blob-metadata), [Set Container Metadata](https://docs.microsoft.com/en-us/rest/api/storageservices/set-container-metadata), [Set Blob Properties](https://docs.microsoft.com/en-us/rest/api/storageservices/set-blob-properties), [HTTP Request Methods](https://www.w3schools.com/tags/ref_httpmethods.asp)
17. Determine what a certain HTTP request will do with a blob.
    - The HTTP request is a *blob leasing* request (can be seen at the `https://...?comp=lease` part). this will prevent the blob from being deleted/overwritten for a specific amount of time. There are three metadata headers that will also confirm this:
      - `x-ms-lease-action: acquire`. Causes a new lease to be requested or an existing lease to be extended.
      - `x-ms-proposed-lease-id: <guid here>`. Can be used if a lease is not already extended. The guid ID of the existing lease should be used.
      - `x-ms-lease-duration: 60`. Stands for 60 seconds.
    - Other options were:
      - *It sets properties on a blob that causes the blob to automatically be deleted after one hour*. You must write custom code to delete blobs after certain time period.
      - *It sets metadata on a blob that causes the blob to be read-write for only one hour*. You must write custom code make the blob modifiable for a specific time period.
      - *It uploads a blob to the tax returns container and times out after one minute*. To upload a blob, you must specify content in the request body, this scenario has no request body.
    - REferences: [Lease Blob](https://docs.microsoft.com/en-us/rest/api/storageservices/lease-blob), [Put Blob](https://docs.microsoft.com/en-us/rest/api/storageservices/put-blob), [Set Blob Properties](https://docs.microsoft.com/en-us/rest/api/storageservices/set-blob-properties), [Set Blob Metadata](https://docs.microsoft.com/en-us/rest/api/storageservices/set-blob-metadata)
18. Retrieving the blob snapshot earlier created.
    - When you first create a snapshot of a blob, you get back a header `x-ms-snapshot: <guid here>`. This guid must be used in the following url to set the blob back to the snapshot: `https://company1.blob.core.windows.net/books/azure.pdf?snapshot=<guid here>`.
    - Other options were:
      - `https://.../<guid here>`. This will retrieve a blob with the guid name.
      - `https://../x-ms-snapshot/<guid here>`. This will retrieve a blob named x-ms-snapshot/guid.
      - `https://..?x-ms-snapshot:<guid here>`. This query string is just incorrect.
    - References: [Snapshot Blob](https://docs.microsoft.com/en-us/rest/api/storageservices/snapshot-blob), [Blob snapshots](https://docs.microsoft.com/en-us/azure/storage/blobs/snapshots-overview)
19. Knowing what a specific blob request does.
    - By using *PUT*, you can update the metadata. If you want to upload a blob, you should use *POST* with a request body.
    - The request does not create a snapshot of the blob. This is because the query parameter has `comp=tier` in it and not `comp=snapshot`.
    - By specifying that it needs to go to the archive tier, it will go offline when the request is finished. It will need rehydration when you want to request it again.
    - Copying is still possible, but will take some time because of the rehydration process.
    - References: [Snapshot Blob](https://docs.microsoft.com/en-us/rest/api/storageservices/snapshot-blob), [Set Blob Tier](https://docs.microsoft.com/en-us/rest/api/storageservices/set-blob-tier), [Hot, Cool, and Archive access tiers for blob data](https://docs.microsoft.com/en-us/azure/storage/blobs/access-tiers-overview), [Blob rehydration from the Archive tier](https://docs.microsoft.com/en-us/azure/storage/blobs/archive-rehydrate-overview)
20. Configure a blob client that will retrieve images from a storage account with read access geozone-redundant storage (RA-GZRS) redundancy.
    - Because you want to first retrieve the data from your primary storage account, you want to choose for `blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;`. This will only read from the secondary storage when a regional outage is affecting the primary storage.
    - Other options were:
      - `... = LocationMode.PrimaryOnly;`. Default blob mode, but doesn't read from the secondary.
      - `... = LocationMode.SecondaryThenPrimary;`. Should read first from the primary one.
      - `... = LocationMode.SecondaryOnly;`. Should read first from the primary one.
    - References: [Use geo-redundancy to design highly available applications](https://docs.microsoft.com/en-us/azure/storage/common/geo-redundant-design?tabs=current), [Shows how to implement the Circuit Breaker pattern for HA apps using RA-GRS Storage](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-ha-ra-grs), [Azure Storage redundancy](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy)
21. Connect with multiple blob storage accounts questions.
    - You *can't* select the PrimaryThenSecondary mode without defining the default request option for the location mode. Without the default request location mode, `PrimaryOnly` mode (default) will be used.
    - You *can* configure the default request location mode as SecondaryThenPrimary for all Blob Storage with read access geo-redundant storage (RA-GRS) replication. The code checks if the blob storage is RA-GRS or RA-GZRS and configures the default request location mode as `SecondaryThenPrimary`.
    - You *cannot* configure the blob client default request location mode as `PrimaryThenSecondary` for Blob Storage with geo-zone-redundant storage (GZRS) replication. This is only enabled in RA-GRS or RA-GZRS.
    - References: [Use geo-redundancy to design highly available applications](https://docs.microsoft.com/en-us/azure/storage/common/geo-redundant-design?tabs=current), [Shows how to implement the Circuit Breaker pattern for HA apps using RA-GRS Storage](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-ha-ra-grs), [Azure Storage redundancy](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy), [Get storage account configuration information](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-get-info?tabs=portal)
22. Configure a data archiving strategy where you focus on the lowest storage cost and some other requirements.
    - We want to use **Blob storage lifecycle and access tiers**. Because of this, we need to upgrade our GPv1 account to a GPv2 account.
    - We also want to move receipts older than 90 days to the *archive tier*, because it has the lowest costs.
    - Other options were:
      - *Create a new Azure Storage Account with account-level cool tiering*. You can just use the same Storage Account and implement a storage lifecycle that archives.
      - *Move the receipts older than 90 days to the new storage account using AzCopy*. While `AzCopy` does the job, it does it one time. So this command should be ran every day.
      - *Move the receipts older than 90 days to the cool tier using the storage lifecycle*. Still expensive.
    - References: [Hot, Cool, and Archive access tiers for blob data](https://docs.microsoft.com/en-us/azure/storage/blobs/access-tiers-overview), [Optimize costs by automatically managing the data lifecycle](https://docs.microsoft.com/en-us/azure/storage/blobs/lifecycle-management-overview), [Get started with AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10)
23. Copy a blob asynchronously and if it fails, restart the copy or near the point of failure.
    - You should use the `AzCopy` command to copy the blob. It can move data into and out of storage in Azure including between storage accounts. When copying, it will leave the original blob in place and if done, it will delete the source blob.
    - Other options were:
      - *The `Start-AzureStorageBlobCopy` PowerShell cmdlet*. Can only copy a blob between containers but not between storage accounts.
      - *The `az storage blob` Azure CLI command*. It only supports synchronous copy and does not support restart at the point of failure.
      - *The `Start-AzureStorageBlobIncrementalCopy` PowerShell cmdlet*. Used to copy data from a Page blob snapshot to a specified destination Page blob.
    - References: [Start-AzureStorageBlobCopy](https://docs.microsoft.com/en-us/powershell/module/azure.storage/start-azurestorageblobcopy?view=azurermps-6.13.0), [Start-AzureStorageBlobIncrementalCopy](https://docs.microsoft.com/en-us/powershell/module/azure.storage/start-azurestorageblobincrementalcopy?view=azurermps-6.13.0), [azcopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-ref-azcopy), [Get started with AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10), [Choose a tool and strategy to copy blobs](https://docs.microsoft.com/en-us/learn/modules/copy-blobs-from-command-line-and-code/2-choosing-a-tool-and-strategy), [Move Azure storage blobs from the command line with the Azure CLI](https://docs.microsoft.com/en-us/learn/modules/copy-blobs-from-command-line-and-code/3-move-blobs-using-cli)
24. Configure a lifecycle policy of a blob.
    - First we need the JSON object `baseBlob`. This will contain the rules for enforcing our data storage.
    - The first rule should be `"tierToArchive" : { "daysAfterModificationGreaterThan": 90 }`. This is to archive blobs after 90 days.
    - The second rule should be `"delete" : { "daysAfterModificationGreaterThan": 365 }`. This will remove blobs after 365 days.
    - The last rule should be `"tierToCool" : { "daysAfterModificationGreaterThan": 30 }`. This will put blobs in the cool tier after 30 days.
    - References: [Optimize costs by automatically managing the data lifecycle](https://docs.microsoft.com/en-us/azure/storage/blobs/lifecycle-management-overview?tabs=azure-portal), [Blob rehydration from the Archive tier](https://docs.microsoft.com/en-us/azure/storage/blobs/archive-rehydrate-overview?tabs=azure-portal)

### Implement Azure Security

1. Select Azure AD app manifest attribute with value so Users can login from an Azure Web app.
    - You should configure the *groupMembershipClaims* attribute with the value *SecurityGroup*. This will retrieve group membership and Azure AD roles for the group claim.
    - Other options were:
      - *appRoles attribute*. This is used to specify a collection of roles that an app may declare.
      - *signInAudience attribute*. This is used to identify Microsoft accounts that are supported for the app.
      - *None value*. Will return no user security claims.
      - *All value*. Includes distribution groups along with group membership and Azure AD roles.
    - References: [Azure Active Directory app manifest](https://docs.microsoft.com/en-us/azure/active-directory/develop/reference-app-manifest), [Configure group claims for applications with Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-group-claims)
2. Configure members of a group as members of the contribute role for a web app.
    - By using `New-AzRoleAssignment` (or `az role assignment`), you can grant access based on role-based access control (RBAC) to a security principal, including a user or group.
    - Other options were:
      - `New-AzRoleDefinition`. Can be used to create a new RBAC role definition.
      - `Set-AzRoleDefinition`. Configure an existing RBAC role, but does not provide any way of setting the security principals to which the role is assigned.
      - `New-AzManagedApplicationDefinition`. Has something too do with managed applications.
    - References: [New-AzRoleAssignment](https://docs.microsoft.com/en-us/powershell/module/az.Resources/New-azRoleAssignment?view=azps-6.6.0&viewFallbackFrom=azps-4.2.0), [New-AzRoleDefinition](https://docs.microsoft.com/en-us/powershell/module/az.resources/new-azroledefinition?view=azps-6.6.0&viewFallbackFrom=azps-4.2.0), [Set-AzRoleDefinition](https://docs.microsoft.com/en-us/powershell/module/az.resources/set-azroledefinition?view=azps-6.6.0&viewFallbackFrom=azps-4.2.0), [New-AzManagedApplicationDefinition](https://docs.microsoft.com/en-us/powershell/module/az.resources/new-azmanagedapplicationdefinition?view=azps-6.6.0&viewFallbackFrom=azps-4.2.0), [az role assignment](https://docs.microsoft.com/en-us/cli/azure/role/assignment?view=azure-cli-latest#az-roleassignment-create), [Azure managed applications overview](https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/overview)
3. Modify Azure AD to allow users to login when they use your app with App Service Authentication.
    - You should create an *app registration*. This is similar of registering other identity providers such as Google, Facebook or Microsoft. It will define the app that is allowed to authenticate against Azure AD.
    - Other options were:
      - *Create an application proxy*. This allows users to access an on-premises application securely through Azure AD.
      - *Configure Azure AD Connect*. This allows you to synchronize and AD tenant with an on-premises AD domain.
      - *Create an enterprise application*. Enterprise applications allow you to integrate other applications with Azure AD, including your own apps.
    - References: [Configure your App Service or Azure Functions app to use Azure AD login](https://docs.microsoft.com/en-us/azure/app-service/configure-authentication-provider-aad), [Remote access to on-premises applications through Azure AD Application Proxy](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy), [What is hybrid identity with Azure Active Directory?](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-hybrid-identity), [Quickstart: Add an enterprise application](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/add-application-portal), [Tutorial: Azure Active Directory single sign-on (SSO) integration with Google Cloud (G Suite) Connector](https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/google-apps-tutorial)
4. Enabling a system-assigned identity on a new VM and let Managed Service Identity (MSI) allow the VM to access the Azure Resource Manager API.
    - You should grant the *Reader role* to the identity at the subscription scope. This applies to all resource groups in the subscription, which ensures that the VM can access resources in all resource groups.
    - Other options were:
      - *Grant the VM Contributor role to your personal Azure Active Directory (Azure AD) account at each resource group scope*. We have already permission because we created the VM, but we should also enable it for the system-assigned identity.
      - *Create an identity by running the az identity create Azure CLI command*. Run this command to set the name of a user identity, not a system identity.
      - *Retrieve an access token by running the Invoke-WebRequest PowerShell cmdlet*. You should run this after granting the Reader role, otherwise the access token can't be retrieved.
    - References: [What are managed identities for Azure resources?](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview), [Use a Windows VM system-assigned managed identity to access Resource Manager](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm), [Configure managed identities for Azure resources on an Azure VM using Azure CLI](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm), [Tutorial: Use a Windows VM system-assigned managed identity to access Azure Storage via a SAS credential](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-storage-sas)
5. Protect a web API by using OAuth 2.0 authentication with your company's Azure Active Directory (AD) tenant.
    - First, you should create an *API Managed instance*. This will allow a client application to use OAuth 2.0 authentication with an Azure AD tenant when accessing your web API.
    - You should also create an *app registration*. This is similar of registering other identity providers such as Google, Facebook or Microsoft. It will define the app that is allowed to authenticate against Azure AD.
    - Other options were:
      - *Create an application proxy*. This allows users to access an on-premises application securely through Azure AD.
      - *Create an Application Gateway*. This allows you to use routing to create a load balancer to backend web applications.
    - References: [Protect a web API backend in Azure API Management using OAuth 2.0 authorization with Azure Active Directory](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-protect-backend-with-aad), [What is Azure Application Gateway?](https://docs.microsoft.com/en-us/azure/application-gateway/overview), [Remote access to on-premises applications through Azure AD Application Proxy](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy), [Configure your App Service or Azure Functions app to use Azure AD login](https://docs.microsoft.com/en-us/azure/app-service/configure-authentication-provider-aad)
6. Developing a daemon application with .NET Core that uses Microsoft Authentication Library (MSAL).
    - Because a daemon application does not interact with users and you want to acquire tokens from the Microsoft identity platform, it needs to use the OAuth 2.0 client credential grant flow. This flow requires a *confidential client*: `IConfidentialClientApplication client;`.
    - In other code, we set up the configuration. Now we are retrieving the `config.clientId` from here.
    - We also need to specify the previously registered client secret in the Azure AD App registration. Without this, you cannot create a configdential client application: `.WithClientSecret(config.clientSecret);`.
    - Full client builder: `IConfidentialClientApplication client = ConfidentialClientApplicationBuilder.Create(config.clientId).WithClientSecret(config.clientSecret).WithAuthority(new Uri(authority)).Build();`
    - Other options were:
      - *Public client application*. Not trusted to safely keep application secrets, so they can only access Microsoft Graph API on behalf of the user.
      - *config.tenantId*. This information is already provided to the IConfidentialClientApplication.
      - *.WithCertificate(config.certificateName)*. It's possible to use a certificate, but in the description, it is not stated that there is one.
    - References: [Overview of the Microsoft Authentication Library (MSAL)](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-overview), [Scenario: Daemon application that calls web APIs](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-daemon-overview), [Microsoft identity platform and the OAuth 2.0 client credentials flow](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow), [Public client and confidential client applications](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-client-applications), [Exercise - Daemon and non-interactive apps](https://docs.microsoft.com/en-us/learn/modules/identity-application-types/7-exercise-daemon-non-interactive-apps)
7. Adding attributes to MVC controllers and actions.
    - On the controller, there should be two attributes: `[Authorize (Role="SubscriptionOwner")]` and `[Authorize (Role="SubscriptionUser)]`. Now only users with both roles can access the controller.
    - On the action, there should be one attribute: `[Authorize (Policy="Subscribers", Role="SubscriptionOwner")]`. This will ensure that a user is validated against the policy defining the acceptable UserTier property to fall either in tier2 or tier3.
    - Other options were:
      - `[Authorize (Role="SubscriptionOwner", "SubscriptionUser")]`. This will allow a user that has one of the two roles to execute actions.
      - `[Authorize (Policy="Subscribers")] [Authorize (Role="SubscriptionOwner")]`. We want to have the claims only to be validated to execute the Administrators action.
    - References: [Policy-based authorization in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/policies?view=aspnetcore-3.1), [Role-based authorization in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/roles?view=aspnetcore-3.1), [Claims-based authorization in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/claims?view=aspnetcore-3.1)
8. Create a shared access signature (SAS) token for a partner application respecting the principle of least privilege.
    - First, you should create the credentials with `StorageSharedKeyCredential key = new StorageSharedKeyCredential(accountName, accountKey);`.
    - Then we will build the SAS with `BlobSasBuilder sasBuilder = new BlobSasBuilder() { BlobContainerName = "container1", Resource = "c", StartsOn = DateTimeOffset.UtcNow, ExpiresOn = DateTimeOffset.UtcNow.AddHours(8) };`. This has one important property called `Resource = "c"`. This will scope the token to a blob **container level**, so that the partner application has the necessary permissions.
    - With that build, we can set permission on the builder with `sasBuilder.SetPermissions(BlobContainerSasPermissions.Read);`. Now the SAS token is scoped correctly to only reading.
    - And at last, we can create the token with `string sasToken = sasBuilder.ToSasQueryParameters(key).ToString();`.
    - Other options were:
      - `AccountSasBuilder` and `AccountSasPermissions.Read`. This will give too much permission than only scoping it to the blob container.
      - `Resource = "b"`. This will scope the SAS to single blob level.
    - References: [Grant limited access to Azure Storage resources using shared access signatures (SAS)](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview), [Create an account SAS with .NET](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-sas-create-dotnet?tabs=dotnet), [BlobSasBuilder Class](https://docs.microsoft.com/en-us/dotnet/api/azure.storage.sas.blobsasbuilder?view=azure-dotnet)
9. Enabling multi-factor authentication (MFA) for an internal website that uses Azure Active Directory Basic licenses.
    - It is a must to have an Azure AD Premium P1 license to implement MFA.
    - Furthermore, you should also create a new conditional access policy in Azure AD. This policy can be used to allow, deny, or require MFA authentication given specific conditions.
    - Other options were:
      - *Configure the website to use Azure AD business-to-business (B2B)*. This allows to share your applications with guest users from external organizations, while this is only for intern use.
      - *Create an application proxy*. This allows users to access an on-premises application securely through Azure AD.
      - *Enable security defaults in Azure AD*. This sets for all users pre-configured security settings, while we want it to be only for a group.
    - References: [Tutorial: Secure user sign-in events with Azure AD Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-azure-mfa), [Azure Active Directory (Azure AD) pricing](https://azure.microsoft.com/en-us/pricing/details/active-directory/), [What is guest user access in Azure Active Directory B2B?](https://docs.microsoft.com/en-us/azure/active-directory/external-identities/what-is-b2b), [Using Azure AD Application Proxy to publish on-premises apps for remote users](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/what-is-application-proxy), [What are security defaults?](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/concept-fundamentals-security-defaults)
10. Implement new Azure API Management policy where all calls should be secure based on access tokens from Azure Active Directory, while not passing credentials to the API.
    - You should use *managed identity* as the authentication mechanism. They will provide secure access required without passing credentials. It will use the managed identity to obtain an access token from Azure AD.
    - Other options were:
      - *Basic*. This will require to pass credentials and that is not what we want here.
      - *Client certificate*. This method uses a certificate that has been installed in API Management for authentication.
      - *Anonymous*. While supported in some scenario's, it is not supported as an API authentication mechanism policy.
    - References: [API Management authentication policies](https://docs.microsoft.com/en-us/azure/api-management/api-management-authentication-policies), [Anonymous Authentication <anonymousAuthentication>](https://docs.microsoft.com/en-us/iis/configuration/system.webserver/security/authentication/anonymousauthentication), [Authentication and authorization in Azure App Service and Azure Functions](https://docs.microsoft.com/en-us/azure/app-service/overview-authentication-authorization)
11. Implement API Management access restriction policy that can be passed as a request query parameter.
    - You should use the *validate JWT* Management access restriction policy. It can be retrieved and validated from Header or query parameter and can secure the entire API through Azure AD authentication.
    - Other options were:
      - *Check HTTP header*. Checks for an HTTP header, but that's it.
      - *Limit call rate by key*. While it can be used to limit the application somewhat in volume, it provides no authentication.
      - *JSONP*. It's used to add JSON with padding support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.
    - References: [API Management access restriction policies](https://docs.microsoft.com/en-us/azure/api-management/api-management-access-restriction-policies), [JSON Web Tokens](https://aadguide.azurewebsites.net/integration/jwts/), [API Management cross domain policies](https://docs.microsoft.com/en-us/azure/api-management/api-management-cross-domain-policies)
12. Give a developer a new role that allows to work on a resource, but not add new members.
    - Give him the *Contributor* role, so hey can work without the ability to add new members.
    - Other options were:
      - *Owner*. This will give him full access.
      - *Reader*. Now he can't work on the resource.
      - *User Access Administrator*. Gives him the ability to grant resource access to other users.
    - References: [What is Azure role-based access control (Azure RBAC)?](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview)
13. Using Azure Active Directory to authenticate app users with automatic registration.
    - To register your app for the AAD authentication automatically, you must:
      - Open Azure Portal and navigate to your app.
      - Select Authentication and click Add identity provider.
      - Set the identity provider to Microsoft.
      - Click the Add button.
    - Other options were:
      - *Open Azure Portal and navigate to Security Center*. While this can help you with the security, it won't help you with automatic AAD registration.
      - *Select Identity and set System assigned to On*. This is not designed to authenticate your web end users.
      - *Set the identity provider to Google*. We want Microsoft.
      - *Provide the CLient ID and secret*. If we selected Google, we need to assign these extra values.
    - References: [Configure your App Service or Azure Functions app to use Azure AD login](https://docs.microsoft.com/en-us/azure/app-service/configure-authentication-provider-aad), [What is Microsoft Defender for Cloud?](https://docs.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction), [What are managed identities for Azure resources?](https://docs.microsoft.com/en-gb/azure/active-directory/managed-identities-azure-resources/overview), [Configure your App Service or Azure Functions app to use Google login](https://docs.microsoft.com/en-us/azure/app-service/configure-authentication-provider-google)
14. Generate a service shared access signature (SAS) programmatically, using the storage account key.
    - The complete SAS URL is as follows: `https://myWeb.blob.core.windows.net/reports2021/may.txt?sv=2019-02-02&se=2021-05031T23%3A59%3A59Z&s=b&sp=r&sig=abc%DEFG%hijk%123`.
    - The first part of the URL is simple, it's just a blob that we are accessing to.
    - The second part contains of some cryptic shortcuts:
      - `sv` (signedVersion) contains the service version of the shared access signature.
      - `sr` (signedResource) must be set to **b**, to indicate that you grant access to the content and metadata of your blob.
      - `sp` (signedPermission) must be set to **r**, to indicate that user permission is restricted to the read operation.
    - Other options were:
      - `sr = c`. Gives access to the content and metadata of all the blobs in the container.
      - `sp = c`. No information.
    - References: [Grant limited access to Azure Storage resources using shared access signatures (SAS)](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview), [Create a service SAS](https://docs.microsoft.com/en-us/rest/api/storageservices/create-service-sas)
15. Retrieving a key stored in the Azure Key Vault.
    - First define the url: `string keyVaultUrl = "https://companyA.vault.azure.net/";`
    - Then define the object: `KeyClient client = new KeyClient(vaultUri: new Uri(keyVaultUrl), credential: new DefaultAzureCredential());`. This will set the url and the credentials for Azure. Because we used `new DefaultAzureCredential()`, it will use the environment variables `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET` and `AZURE_TENANT_ID`.
    - You can then retrieve the key with: `KeyVaultKey key = await client.GetKeyAsync("encryptionKey");`. Be careful that you only specify the key name when trying to retrieve the key and nothing else (like an url).
    - Other options were:
      - `CryptographyClient`. Only use this when we are doing some crypto stuff.
      - `keyVaultUrl + "encryptionKey"`. The parameter expects to only have the name of the key itself and not the url.
    - References: [Azure Key Vault key client library for .NET](https://github.com/Azure/azure-sdk-for-net/tree/main/sdk/keyvault/Azure.Security.KeyVault.Keys), [Azure Identity client library for .NET](https://github.com/Azure/azure-sdk-for-net/blob/main/sdk/identity/Azure.Identity/README.md)
16. Using Azure App Configuration questions.
    - Azure App Configuration is **not** designed as a more secure replacement for Azure Key Vault. While the data is encrypted, Azure Key Vault provides a more secure place through hardware-level encryption, granular access policies and management operations.
    - You **can** import configuration information directly from separate configuration files into Azure App Configuration. You can also export configuration information out from Azure App Configuration to separate configuration files.
    - You **can** configure separate configuration stores to support different environments including development, test and production.
    - References: [What is Azure App Configuration?](https://docs.microsoft.com/en-us/azure/azure-app-configuration/overview), [Azure App Configuration FAQ](https://docs.microsoft.com/en-us/azure/azure-app-configuration/faq)
17. Enabling the key vault to recognize and grant access to your application.
    - You should use the command: `Set-AzKeyVaultAccessPolicy -VaultName MyKeyVault -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get`. This command consists out a few parts:
      - *Set-AzKeyVaultAccessPolicy* grants or modifies permissions for a user, application or security group to perform operations with a key vault.
      - *Get* grants the permission to enable the application to access the key value.
    - Other options were:
      - *New-AzKeyVaultNetworkRuleSetObject*. Creates an object representing the network rule settings that can be used when creating a vault.
      - *Set-AzKeyVaultSecret*. Modifies the existing key vault secret, but not the permission.
      - *Update-AzKeyVaultKey*. Modifies editable attributes of a key vault.
      - *Recover*. Recovers a deleted secret.
      - *Restore*. Restores a backed-up secret.
      - *Set*. Create or modify a secret.
    - References: [Automate the rotation of a secret for resources that have two sets of authentication credentials](https://docs.microsoft.com/en-us/azure/key-vault/secrets/tutorial-rotation-dual?tabs=azure-cli), [About Azure Key Vault secrets](https://docs.microsoft.com/en-us/azure/key-vault/secrets/about-secrets), [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/en-us/powershell/module/az.keyvault/Set-AzKeyVaultAccessPolicy?view=azps-6.6.0&viewFallbackFrom=azps-4.2.0), [Set-AzKeyVaultSecret](https://docs.microsoft.com/en-us/powershell/module/az.keyvault/Set-AzKeyVaultSecret?view=azps-6.6.0&viewFallbackFrom=azps-4.2.0), [Update-AzKeyVaultKey](https://docs.microsoft.com/en-us/powershell/module/az.keyvault/Update-AzKeyVaultKey?view=azps-6.6.0&viewFallbackFrom=azps-4.2.0), [New-AzKeyVaultNetworkRuleSetObject](https://docs.microsoft.com/en-us/powershell/module/az.keyvault/New-AzKeyVaultNetworkRuleSetObject?view=azps-6.6.0&viewFallbackFrom=azps-4.2.0)
18. Configure disk encryption for a vm using an encryption key from a Key Vault created for that purpose.
    - You should first create the Key Vault with `az keyvault create --name "myKeyVault" --resource-group "myRG" --location "eastus"`.
    - Then update the Key Vault to support disk encryption with `az keyvault update --name "myKeyVault" --resource-group "myRG" --enabled-for-disk-encryption "true"`.
    - Next we want to create an key encryption key (KEK). This can be done with `az keyvault key create --name "myKEK" --vault-name "myKeyVault" --kty RSA-HSM`. Be warned that you should use an RSA key type because other options are not supported for disk encryption.
    - Finally, we can enable the encryption with `az vm encryption enable -g "myRG" --name "myLinuxVM" --disk-encryption-keyvault "myKeyVault" --key-encryption-key "myKEK"`.
    - References: [Creating and configuring a key vault for Azure Disk Encryption](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/disk-encryption-key-vault), [Quickstart: Create and encrypt a Linux VM with the Azure CLI](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/disk-encryption-cli-quickstart)
19. Copy data from the App Configuration store to a local JSON file.
    - You should use the `az appconfig kv export`. This will copy key-value pairs from the specified App Configuration file to a local file. A more complete command looks like this: `az appconfig kv export --name myDevAppConfigStore --file ~/DevFix.json`.
    - Other options were:
      - `az appconfig kv import`. This is used to import the configuration, not export.
      - `az keyvault key import`. Used for Key Vault to import a private key.
      - `az keyvault key backup`. Used for Key Vault to backup a private key.
      - `az keyvault secret`. Used for Key Vault to manage Key Vault secrets.
    - References: [az appconfig kv](https://docs.microsoft.com/en-us/cli/azure/appconfig/kv?view=azure-cli-latest), [az keyvault](https://docs.microsoft.com/en-us/cli/azure/keyvault?view=azure-cli-latest), [What is Azure App Configuration?](https://docs.microsoft.com/en-us/azure/azure-app-configuration/overview)
20. What does the code do that retrieves some secrets from the Key Vault.
    - It returns the secret without the label *Production*. This is because of some configuration that happened in the `CreateHostBuilder`. We are settings `.Select(KeyFilter.Any, "Production").Select(KeyFilter.Any, LabelFilter.Null)` and note that the Production filter is selected first. But because the `LabelFilter.Null` is second, it will overwrite the first select. This is why we are retrieving the secret without the label *Production*.
    - Other options were:
      - `{"uri":"https://vault1.vault.azure.net/secrets/DbPasswordProd"}`. This is the secret that will be referenced when App1 loads this configuration.
      - *The secret value of DbPasswordProd secret*. Because of the order of label setting, this isn't returned.
      - *myserver.database.windows.net*. If we load `App1:Database:Host`, this will be returned.
    - References: [Tutorial: Use Key Vault references in an ASP.NET Core app](https://docs.microsoft.com/en-us/azure/azure-app-configuration/use-key-vault-references-dotnet-core?tabs=core5x), [Use labels to provide per-environment configuration values.](https://docs.microsoft.com/en-us/azure/azure-app-configuration/howto-labels-aspnet-core?tabs=core5x), [Keys and values](https://docs.microsoft.com/en-us/azure/azure-app-configuration/concept-key-value)
21. Retrieving an existing certificate from an Azure Key Vault using the .Net client library.
    - By using the `GetCertificateAsync` method, ew are returning the latest version of the specified certificate from an Azure Key Vault. It will also  retrieve the current policy of the requested certificate.
    - Other options were:
      - `GetCertificatePolicyAsync`. It will retrieve the current policy of the certificate, but not the certificate itself.
      - `StartCreateCertificateAsync`. Creates a new certificate, but does not retrieve an existing one.
      - `UpdateCertificatePolicyAsync`. Updates the policy of the specified certificate.
    - References: [Quickstart: Azure Key Vault certificate client library for .NET (SDK v4)](https://docs.microsoft.com/en-us/azure/key-vault/certificates/quick-create-net), [CertificateClient.GetCertificateAsync(String, CancellationToken) Method](https://docs.microsoft.com/en-us/dotnet/api/azure.security.keyvault.certificates.certificateclient.getcertificateasync?view=azure-dotnet), [CertificateClient.GetCertificatePolicyAsync(String, CancellationToken) Method](https://docs.microsoft.com/en-us/dotnet/api/azure.security.keyvault.certificates.certificateclient.getcertificatepolicyasync?view=azure-dotnet), [CertificateClient.StartCreateCertificateAsync(String, CertificatePolicy, Nullable<Boolean>, IDictionary<String,String>, CancellationToken) Method](https://docs.microsoft.com/en-us/dotnet/api/azure.security.keyvault.certificates.certificateclient.startcreatecertificateasync?view=azure-dotnet), [CertificateClient.UpdateCertificatePolicyAsync(String, CertificatePolicy, CancellationToken) Method](https://docs.microsoft.com/en-us/dotnet/api/azure.security.keyvault.certificates.certificateclient.updatecertificatepolicyasync?view=azure-dotnet)

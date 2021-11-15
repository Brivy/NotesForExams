# Question list with explained anwsers

(Sorry Measureup and Microsoft for stealing this :))

In this chapter, I will go through all the practice exam questions and figure out what I still need to research/investigate:

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
3. Complete a multi-stage uild docker file that is fully optimized.
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
9. Deploying a single ASP.NET Core application on an Azure Container Instance (ACI). Sidecar a container on the same host that doesn't increase cost and requires minimal change.
    - We should use *ACI Container Groups* (yes, it's the same service). They provide the capability to host a set of containers on the same host machine, share the same resources and can communicate with each other. You can also install monitoring agents inside the separate container instances.
    - The other options were:
      - *Azure Container Registry*. This is where the container images are hosted.
      - *Kubernetes Pods*. Will work, but because we already have ACI working, this would only increase the cost and complexity.
      - *Azure App Services*. Will work, but because we already have ACI working, this would only increase the cost and complexity.
    - Resources: [Container groups in Azure Container Instances](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-container-groups), [Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/), [Tutorial: Deploy a multi-container group using a Resource Manager template](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-multi-container-group), [Introduction to private Docker container registries in Azure](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-intro)
10. Creating a Dockerfile that will build and publish the container image to an Azure Container Registry.
    - ...

# Recap

This document will be a small recap of the previous exams.

## Explore the Microsoft identity platform

There are several components that make up the Microsoft identity platform:

* **OAuth 2.0 and OpenID Connect standard-compliant authentication service** for your everyday use
* **Open-source libraries** like Microsoft Authentication Libraries (MSAL)
* **Application management portal** through the portal.
* **Application configuration API and PowerShell** to programmatically configure of your applications through the Microsoft Graph API and PowerShell so you can automate your DevOps tasks.

If you want to make an app that uses AD, you can choose:

* **Single tenant**: only accessible in your tenant
* **Multi-tenant**: accessible in other tenants

If you register an application in the portal, an **application object** (the globally unique instance of the app) as well as a **service principal object** are automatically created in your home tenant:

* An _application object_ is used as a template or blueprint to create one or more service principal objects. The application object describes three aspects of an application: how the service can issue tokens in order to access the application, resources that the application might need to access, and the actions that the application can take.
* The _security principal_ defines the access policy and permissions for the user/application in the Azure Active Directory tenant. This enables core features such as authentication of the user/application during sign-in, and authorization during resource access. There are three types of service principals:
  * **Application:** This type of service principal is the local representation, or application instance, of a global application object in a single tenant or directory.
  * **Managed identity:** This type of service principal is used to represent a managed identity. Managed identities provide an identity for applications to use when connecting to resources that support Azure Active Directory authentication.
  * **Legacy:** This type of service principal represents a legacy app, which is an app created before app registrations were introduced or an app created through legacy experiences.

The _application object_ is the global representation of your application for use across all tenants, and the _service principal_ is the local representation for use in a specific tenant. The application object serves as the template from which common and default properties are derived for use in creating corresponding service principal objects. An application object has:

* A 1:1 relationship with the software application, and
* A 1:many relationship with its corresponding service principal object(s).

The Microsoft identity platform implements the **OAuth 2.0 authorization protocol**. Any web-hosted resource that integrates with the Microsoft identity platform has a resource identifier, or application ID URI. For example:

* Microsoft Graph: `https://graph.microsoft.com`
* Microsoft 365 Mail API: `https://outlook.office.com`
* Azure Key Vault: `https://vault.azure.net`

In OAuth 2.0, these types of permission sets are called _scopes_. They're also often referred to as _permissions_. In the Microsoft identity platform, a permission is represented as a string value. An app requests the permissions it needs by specifying the permission in the scope query parameter like `https://graph.microsoft.com/Calendars.Read`.

The Microsoft identity platform supports two types of permissions:

* **Delegated permissions** are used by apps that have a signed-in user present. For these apps, either the user or an administrator consents to the permissions that the app requests. The app is delegated with the permission to act as a signed-in user when it makes calls to the target resource.
* **Application permissions** are used by apps that run without a signed-in user present, for example, apps that run as background services or daemons. Only an administrator can consent to application permissions.

There are a number of kinds of **consent** that your app may need to know about in order to be successful. There are three types:

* In the **static user consent** scenario, you must specify all the permissions it needs in the app's configuration in the Azure portal. It presents some possible issues for developers:
  * The app needs to request all the permissions it would ever need upon the user's first sign-in. This can lead to a long list of permissions that discourages end users from approving the app's access on initial sign-in.
  * The app needs to know all of the resources it would ever access ahead of time. It is difficult to create apps that could access an arbitrary number of resources.
* You can ask for a minimum set of permissions upfront and request more over time as the customer uses additional app features. To do so, you can specify the scopes your app needs at any time by including the new scopes in the scope parameter when requesting an access token - without the need to pre-define them in the application registration information. If the user hasn't yet consented to new scopes added to the request, they'll be prompted to consent only to the new permissions. **Incremental, or dynamic consent**, only applies to delegated permissions and not to application permissions.
* **Admin consent** is required when your app needs access to certain high-privilege permissions. Admin consent ensures that administrators have some additional controls before authorizing apps or users to access highly privileged data from the organization.

A request for permissions looks something like this:

```URL
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fcalendars.read%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

**Conditional Access** enables developers and enterprise customers to protect services in a multitude of ways including:

* Multifactor authentication
* Allowing only Intune enrolled devices to access specific services
* Restricting user locations and IP ranges

Most of the time, CA will not impact your app, only when you need someone to log in again. Specifically, the following scenarios require code to handle Conditional Access challenges:

* Apps performing the on-behalf-of flow
* Apps accessing multiple services/resources
* Single-page apps using MSAL.js
* Web apps calling a resource

## Implement authentication by using the Microsoft Authentication Library

The **Microsoft Authentication Library (MSAL)** can be used to provide secure access to Microsoft Graph, other Microsoft APIs, third-party web APIs, or your own web API. MSAL supports many different application architectures and platforms including .NET, JavaScript, Java, Python, Android, and iOS.

MSAL gives you many ways to get tokens, with a consistent API for a number of platforms. Using MSAL provides the following **benefits**:

* No need to directly use the OAuth libraries or code against the protocol in your application.
* Acquires tokens on behalf of a user or on behalf of an application (when applicable to the platform).
* Maintains a token cache and refreshes tokens for you when they are close to expire. You don't need to handle token expiration on your own.
* Helps you specify which audience you want your application to sign in.
* Helps you set up your application from configuration files.
* Helps you troubleshoot your app by exposing actionable exceptions, logging, and telemetry.

Here are some flows that can be used in a variety of different application scenarios:

* **Authorization code:** Native and web apps securely obtain tokens in the name of the user
* **Client credentials:** Service applications run without user interaction
* **On-behalf-of:** The application calls a service/web API, which in turns calls Microsoft Graph
* **Implicit:** Used in browser-based applications
* **Device code:** Enables sign-in to a device by using another device that has a browser
* **Integrated Windows:** Windows computers silently acquire an access token when they are domain joined
* **Interactive:** Mobile and desktops applications call Microsoft Graph in the name of a user
* **Username/password:** The application signs in a user by using their username and password

**Security tokens** can be acquired by multiple types of applications. These applications tend to be separated into the following two categories. Each is used with different libraries and objects:

* **Public client applications:** Are apps that run on devices or desktop computers or in a web browser. They're not trusted to safely keep application secrets, so they only access web APIs on behalf of the user.
* **Confidential client applications:** Are apps that run on servers (web apps, web API apps, or even service/daemon apps). They're considered difficult to access, and for that reason capable of keeping an application secret.

With MSAL.NET 3.x, the recommended way to instantiate an application is by using the application builders: _PublicClientApplicationBuilder_ and _ConfidentialClientApplicationBuilder_. They offer a powerful mechanism to configure the application either from the code, or from a configuration file, or even by mixing both approaches.

Signing-in users in the Microsoft Azure public cloud:

```C#
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId).Build();
```

In the same way, the following code instantiates a confidential application handling tokens from users in the Microsoft Azure public cloud, with their work and school accounts, or their personal Microsoft accounts:

```C#
string redirectUri = "https://myapp.azurewebsites.net";
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithClientSecret(clientSecret)
    .WithRedirectUri(redirectUri)
    .Build();
```

Other `.With` methods are:

* `.WithAuthority()`: Sets the application default authority to an Azure Active Directory authority, with the possibility of choosing the Azure Cloud, the audience, the tenant (tenant ID or domain name), or providing directly the authority URI.
* `.WithTenantId(string tenantId)`: Overrides the tenant ID, or the tenant description.
* `.WithClientId(string)`: Overrides the client ID.
* `.WithRedirectUri(string redirectUri)`: Overrides the default redirect URI. In the case of public client applications, this will be useful for scenarios requiring a broker.
* `.WithComponent(string)`: Sets the name of the library using MSAL.NET (for telemetry reasons).
* `.WithDebugLoggingCallback()`: If called, the application will call Debug.Write simply enabling debugging traces.
* `.WithLogging()`: If called, the application will call a callback with debugging traces.
* `.WithTelemetry(TelemetryCallback telemetryCallback)`: Sets the delegate used to send telemetry.

Some specifically for confidential apps:

* `.WithCertificate(X509Certificate2 certificate)`: Sets the certificate identifying the application with Azure Active Directory.
* `.WithClientSecret(string clientSecret)`: Sets the client secret (app password) identifying the application with Azure Active Directory.

## Explore Microsoft Graph

Microsoft Graph is the gateway to data and intelligence in Microsoft 365. It provides a unified programmability model that you can use to access the tremendous amount of data in Microsoft 365, Windows 11, and Enterprise Mobility + Security.

![Cool Picture](Pictures/microsoft-graph-data-connectors.png)

In the Microsoft 365 platform, three main components facilitate the access and flow of data:

* The Microsoft Graph API **offers a single endpoint**, `https://graph.microsoft.com`. You can use REST APIs or SDKs to access the endpoint. Microsoft Graph also includes a powerful set of services that manage user and device identity, access, compliance, security, and help protect organizations from data leakage or loss.
* **Microsoft Graph connectors** work in the incoming direction, delivering data external to the Microsoft cloud into Microsoft Graph services and applications, to enhance Microsoft 365 experiences such as Microsoft Search. Connectors exist for many commonly used data sources such as Box, Google Drive, Jira, and Salesforce.
* **Microsoft Graph Data Connect** provides a set of tools to streamline secure and scalable delivery of Microsoft Graph data to popular Azure data stores. The cached data serves as data sources for Azure development tools that you can use to build intelligent applications.

Microsoft Graph is a **RESTful web API** that enables you to access Microsoft Cloud service resources. After you register your app and get authentication tokens for a user or service, you can make requests to the Microsoft Graph API.

```URL
{HTTP method} https://graph.microsoft.com/{version}/{resource}?{query-parameters}
```

* The _version_ can be either `v1.0` or `beta`. Use `v1.0` in production apps.
* A _resource_ can be an entity or complex type, commonly defined with properties. Entities differ from complex types by always including an id property. Your URL will include the resource you are interacting with in the request, such as `me`, `user`, `group`, `drive`, and `site`. Often, top-level resources also include relationships, which you can use to access other resources, like `me/messages` or `me/drive`. You can also interact with resources using methods; for example, to send an email, use `me/sendMail`.
* _Query parameters_ can be **OData system query options**, or other strings that a method accepts to customize its response. You can use optional OData system query options to include more or fewer properties than the default response, filter the response for items that match a custom query, or provide another parameters for a method: `GET https://graph.microsoft.com/v1.0/me/messages?filter=emailAddress eq 'jon@contoso.com'`

After you make a request, a response is returned that includes:

* **Status code**: An HTTP status code that indicates success or failure.
* **Response message**: The data that you requested or the result of the operation. The response message can be empty for some operations.
* `nextLink`: If your request returns a lot of data, you need to page through it by using the URL returned in `@odata.nextLink`.

The Microsoft Graph .NET SDK is included in the following NuGet packages:

* **Microsoft.Graph** - Contains the models and request builders for accessing the v1.0 endpoint with the fluent API. Microsoft.Graph has a dependency on Microsoft.Graph.Core.
* **Microsoft.Graph.Beta** - Contains the models and request builders for accessing the beta endpoint with the fluent API. Microsoft.Graph.Beta has a dependency on Microsoft.Graph.Core.
* **Microsoft.Graph.Core** - The core library for making calls to Microsoft Graph. Provides embedded support for retry handling, secure redirects, transparent authentication, and payload compression, improve the quality of your application's interactions with Microsoft Graph, with no added complexity, while leaving you completely in control.
* **Microsoft.Graph.Auth** - Provides an authentication scenario-based wrapper of the Microsoft Authentication Library for use with the Microsoft Graph SDK.

To access the data in Microsoft Graph, your application will need to **acquire an OAuth 2.0 access token**, and present it to Microsoft Graph in either of the following:

* The HTTP Authorization request header, as a Bearer token
* The graph client constructor, when using a Microsoft Graph client library

_Use the Microsoft Authentication Library API, MSAL to acquire the access token to Microsoft Graph._ In code, this looks something like this:

```C#
// Build a client application.
IPublicClientApplication publicClientApplication = PublicClientApplicationBuilder
            .Create("INSERT-CLIENT-APP-ID")
            .Build();
// Create an authentication provider by passing in a client application and graph scopes.
DeviceCodeProvider authProvider = new DeviceCodeProvider(publicClientApplication, graphScopes);
// Create a new instance of GraphServiceClient with the authentication provider.
GraphServiceClient graphClient = new GraphServiceClient(authProvider);
```

And then you can do some other cool things with it:

```C#
// GET https://graph.microsoft.com/v1.0/me
var user = await graphClient.Me
    .Request()
    .GetAsync();

// GET https://graph.microsoft.com/v1.0/me/messages?$select=subject,sender&$filter=<some condition>&orderBy=receivedDateTime
var messages = await graphClient.Me.Messages
    .Request()
    .Select(m => new { m.Subject, m.Sender })
    .Filter("<filter condition>")
    .OrderBy("receivedDateTime")
    .GetAsync();

// DELETE https://graph.microsoft.com/v1.0/me/messages/{message-id}
string messageId = "AQMkAGUy...";
var message = await graphClient.Me.Messages[messageId]
    .Request()
    .DeleteAsync();

// POST https://graph.microsoft.com/v1.0/me/calendars
var calendar = new Calendar { Name = "Volunteer" };
var newCalendar = await graphClient.Me.Calendars
    .Request()
    .AddAsync(calendar);
```

Some best practices:

* Use least privilege.
* Use the correct permission type (delegated or application permissions) based on scenarios.
* Consider the end user and admin experience.
* Consider multi-tenant applications. Sometimes stuff is restricted by another AD admin.
* Your application should always handle the possibility that the responses are paged in nature, and use the `@odata.nextLink` property to obtain the next paged set of results, until all pages of the result set have been read.
* Adding members to existing enumerations can break applications already using these enums. Evolvable enums is a mechanism that Microsoft Graph API uses to add new members to existing enumerations without causing a breaking change for applications.
* You should only cache or store data locally if necessary for a specific scenario, and if that use case is covered by your terms of use and privacy policy, and does not violate the Microsoft APIs Terms of Use.

## Implement Azure Key Vault

**Azure Key Vault** is a way to store secrets. It enforces _Transport Layer Security (TLS) protocol_ to protect data when it’s traveling between Azure Key Vault and clients. It also uses _Perfect Forward Secrecy (PFS)_ protects connections between customers’ client systems and Microsoft cloud services by unique keys. 

It has two service tiers: Standard, which encrypts with a software key, and a Premium tier, which includes hardware security module(HSM)-protected keys. It helps by the following problems:

* **Secrets Management**: Azure Key Vault can be used to Securely store and tightly control access to tokens, passwords, certificates, API keys, and other secrets
* **Key Management**: Azure Key Vault can also be used as a Key Management solution. Azure Key Vault makes it easy to create and control the encryption keys used to encrypt your data.
* **Certificate Management**: Azure Key Vault is also a service that lets you easily provision, manage, and deploy public and private Secure Sockets Layer/Transport Layer Security (SSL/TLS) certificates for use with Azure and your internal connected resources.

And it has the following benefits:

* Centralized application secrets.
* Securely store secrets and keys.
* Monitor access and use.
* Simplified administration of application secrets.

Azure Keyvault authentication can be done in three ways:

* **Managed identities for Azure resources**: When you deploy an app on a virtual machine in Azure, you can assign an identity to your virtual machine that has access to Key Vault. You can also assign identities to other Azure resources. The benefit of this approach is that the app or service isn't managing the rotation of the first secret. Azure automatically rotates the service principal client secret associated with the identity. This approach is recommended.
* **Service principal and certificate**: You can use a service principal and an associated certificate that has access to Key Vault. We don't recommend this approach because the application owner or developer must rotate the certificate.
* **Service principal and secret**: Although you can use a service principal and a secret to authenticate to Key Vault, we don't recommend it. It's hard to automatically rotate the bootstrap secret that's used to authenticate to Key Vault.

For applications, there are two ways to obtain a **service principal**:

* Enable a system-assigned managed identity for the application. With managed identity, Azure internally manages the application's service principal and automatically authenticates the application with other Azure services. Managed identity is available for applications deployed to a variety of services.
* If you cannot use managed identity, you instead register the application with your Azure AD tenant. Registration also creates a second application object that identifies the app across all tenants.

You can use the REST API as well:

```HTTP
PUT /keys/MYKEY?api-version=<api_version>  HTTP/1.1  
Authorization: Bearer <access_token>
```

And in case of an invalid request, you'll get:

```HTTP
401 Not Authorized  
WWW-Authenticate: Bearer authorization="…", resource="…"
```

Which has two parameters:

* _authorization_: The address of the OAuth2 authorization service that may be used to obtain an access token for the request.
* _resource_: The name of the resource (`https://vault.azure.net`) to use in the authorization request.

Best practices:

* **Use separate key vaults**: Recommended to use a vault per application per environment (Development, Pre-Production and Production). This helps you not share secrets across environments and also reduces the threat in case of a breach.
* **Control access to your vault**: Key Vault data is sensitive and business critical, you need to secure access to your key vaults by allowing only authorized applications and users.
* **Backup**: Create regular back ups of your vault on update/delete/create of objects within a Vault.
* **Logging**: Be sure to turn on logging and alerts.
* **Recovery options**: Turn on soft-delete and purge protection if you want to guard against force deletion of the secret.

## Implement Azure App Configuration

Use **App Configuration** to store all the settings for your application and secure their access in one place. It offers the following benefits:

* A fully managed service that can be set up in minutes
* Flexible key representations and mappings
* Tagging with labels
* Point-in-time replay of settings
* Dedicated UI for feature flag management
* Comparison of two sets of configurations on custom-defined dimensions
* Enhanced security through Azure-managed identities
* Complete data encryptions, at rest or in transit
* Native integration with popular frameworks

App Configuration complements Azure Key Vault, which is used to store application secrets. App Configuration makes it easier to implement the following scenarios:

* Centralize management and distribution of hierarchical configuration data for different environments and geographies
* Dynamically change application settings without the need to redeploy or restart an application
* Control feature availability in real-time

_Keys_ serve as the name for key-value pairs and are used to store and retrieve corresponding values. It's a common practice to organize keys into a hierarchical namespace by using a character delimiter, such as `/` or `:`. They are case-sensitive and can contain every unicode character except `*`, `,`, and `\`. These can be escaped by using `\{Reserved Character}`.

There are two general approaches to naming keys used for configuration data: **flat** or **hierarchical**. While flat is just flat, hierarchical offers some benefits:

* **Easier to read**. Instead of one long sequence of characters, delimiters in a hierarchical key name function as spaces in a sentence.
* **Easier to manage**. A key name hierarchy represents logical groups of configuration data.
* **Easier to use**. It's simpler to write a query that pattern-matches keys in a hierarchical structure and retrieves only a portion of configuration data.

It looks something like this `AppName:Service1:ApiEndpoint`.

Key values in App Configuration can optionally have a label attribute. Labels are used to differentiate key values with the same key. A common use of labels is to specify multiple environments for the same key:

```Configuration
Key = AppName:DbEndpoint & Label = Test
Key = AppName:DbEndpoint & Label = Staging
Key = AppName:DbEndpoint & Label = Production
```

Note: There is **no** automatic versioning for keys.

_Values_ assigned to keys are also unicode strings. You can use all unicode characters for values. There's an optional user-defined content type associated with each value. Use this attribute to store information, for example an encoding scheme, about a value that helps your application to process it properly.

Feature management is a modern software-development practice that decouples feature release from code deployment and enables quick changes to feature availability on demand. It uses a technique called **feature flags** (also known as feature toggles, feature switches, and so on) to dynamically administer a feature's lifecycle. This has some special terms:

* **Feature flag**: A feature flag is a variable with a binary state of on or off. The feature flag also has an associated code block. The state of the feature flag triggers whether the code block runs or not.
* **Feature manager**: A feature manager is an application package that handles the lifecycle of all the feature flags in an application. The feature manager typically provides additional functionality, such as caching feature flags and updating their states.
* **Filter**: A filter is a rule for evaluating the state of a feature flag. A user group, a device or browser type, a geographic location, and a time window are all examples of what a filter can represent.

Feature flags are just booleans:

```C#
if (featureFlag) {
    // Run the following code
}
```

Each feature flag has two parts: **a name** and **a list of one or more filters** that are used to evaluate if a feature's state is on (that is, when its value is True). A filter defines a use case for when a feature should be turned on. When a feature flag has multiple filters, the filter list is traversed in order until one of the filters determines the feature should be enabled. At that point, the feature flag is on, and any remaining filter results are skipped. If no filter indicates the feature should be enabled, the feature flag is off.

To use feature flags effectively, you need to **externalize all the feature flags used in an application**. This approach allows you to change feature flag states without modifying and redeploying the application itself.

The feature manager supports `appsettings.json` as a configuration source for feature flags. The following example shows how to set up feature flags in a JSON file:

```JSON
"FeatureManagement": {
    "FeatureA": true, // Feature flag set to on
    "FeatureB": false, // Feature flag set to off
    "FeatureC": {
        "EnabledFor": [
            {
                "Name": "Percentage",
                "Parameters": {
                    "Value": 50
                }
            }
        ]
    }
}
```

Normally, all values are encrypted at rest with 256-bit AES. When **customer-managed key capability** is enabled, App Configuration uses a managed identity assigned to the App Configuration instance to authenticate with Azure Active Directory. The managed identity then calls Azure Key Vault and wraps the App Configuration instance's encryption key. The wrapped encryption key is then stored and the unwrapped encryption key is cached within App Configuration for one hour. App Configuration refreshes the unwrapped version of the App Configuration instance's encryption key hourly. This ensures availability under normal operating conditions. You can enable this by having the following components:

* Standard tier Azure App Configuration instance
* Azure Key Vault with soft-delete and purge-protection features enabled
* An RSA or RSA-HSM key within the Key Vault: The key must not be expired, it must be enabled, and it must have both wrap and unwrap capabilities enabled

Once these resources are configured, two steps remain to allow Azure App Configuration to use the Key Vault key:

* Assign a managed identity to the Azure App Configuration instance
* Grant the identity `GET`, `WRAP`, and `UNWRAP` permissions in the target Key Vault's access policy.

You can also use **private endpoints** for Azure App Configuration to allow clients on a virtual network (VNet) to securely access data over a private link. The private endpoint uses an IP address from the VNet address space for your App Configuration store. Some notes:

* When creating a private endpoint, you must specify the App Configuration store to which it connects. If you have multiple App Configuration stores, you need a separate private endpoint for each store.
* When you create a private endpoint, the DNS CNAME resource record for the configuration store is updated to an alias in a subdomain with the prefix `privatelink`. Azure also creates a private DNS zone corresponding to the `privatelink` subdomain, with the DNS A resource records for the private endpoints.

A **managed identity** from Azure Active Directory (AAD) allows Azure App Configuration to easily access other AAD-protected resources, such as Azure Key Vault. The identity is managed by the Azure platform. It does not require you to provision or rotate any secrets. There are two of them:

* A **system-assigned** identity is tied to your configuration store. It's deleted if your configuration store is deleted. A configuration store can only have one system-assigned identity.
* A **user-assigned** identity is a standalone Azure resource that can be assigned to your configuration store. A configuration store can have multiple user-assigned identities.

To set up a managed identity using the Azure CLI, use the `az appconfig identity assign` command against an existing configuration store:

```Bash
az appconfig identity assign \ 
    --name myTestAppConfigStore \ 
    --resource-group myResourceGroup
```

Creating an App Configuration store with a user-assigned identity requires that you create the identity and then assign its resource identifier to your store.

```Bash
az identity create --resource-group myResourceGroup --name myUserAssignedIdentity

az appconfig identity assign --name myTestAppConfigStore \ 
    --resource-group myResourceGroup \ 
    --identities /subscriptions/[subscription id]/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myUserAssignedIdentity
```
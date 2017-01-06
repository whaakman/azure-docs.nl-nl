---
title: Een elastische pool maken met C# | Microsoft Docs
description: Gebruik C#-databaseontwikkelingstechnieken om in Azure SQL Database een schaalbare elastische pool te maken, zodat u resources kunt delen tussen meerdere databases.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 2dedddbb-618d-462b-80dd-e4a57857c737
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: csharp
ms.workload: data-management
ms.date: 10/04/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 6fb71859d0ba2e0f2b39d71edd6d518b7a03bfe9
ms.openlocfilehash: 1d92c9a2dfae4a209ea95a33facd4b8e058fd866


---
# <a name="create-an-elastic-pool-with-cx23"></a>Een elastische pool maken met C&#x23;
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-elastic-pool-create-portal.md)
> * [PowerShell](sql-database-elastic-pool-create-powershell.md)
> * [C#](sql-database-elastic-pool-create-csharp.md)
> 
> 

In dit artikel wordt beschreven hoe u C# gebruikt om een elastische Azure SQL-pool te maken met de [Azure SQL Database-bibliotheek voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Zie [SQL Database uitproberen: een SQL-database maken met C# met de SQL Database-bibliotheek voor .NET](sql-database-get-started-csharp.md) als u een zelfstandige SQL-database wilt maken.

De Azure SQL Database-bibliotheek voor .NET biedt een op [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) gebaseerde API die de op [Resource Manager gebaseerde SQL Database-REST-API](https://msdn.microsoft.com/library/azure/mt163571.aspx) bevat.

> [!NOTE]
> Veel nieuwe functies van SQL Database worden alleen ondersteund als u het [Azure Resource Manager-implementatiemodel](../azure-resource-manager/resource-group-overview.md) gebruikt. Daarom moet u altijd de nieuwste **Azure SQL Database Management Library voor .NET gebruiken ([docs](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. De oudere [klassieke op implementatiemodellen gebaseerde bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) worden alleen ondersteund voor achterwaartse compatibiliteit. Wij raden daarom aan de nieuwere op Resource Manager gebaseerde bibliotheken te gebruiken.
> 
> 

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

* Een Azure-abonnement. Als u een Azure-abonnement nodig hebt, klikt u op **GRATIS ACCOUNT** boven aan deze pagina. Keer daarna hier terug om dit artikel te voltooien.
* Visual Studio. Ga naar de pagina [Visual Studio-downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) voor een gratis exemplaar van Visual Studio.

## <a name="create-a-console-app-and-install-the-required-libraries"></a>Een consoletoepassing maken en de vereiste bibliotheken installeren
1. Start Visual Studio.
2. Klik op **Bestand** > **Nieuw** > **Project**.
3. Maak een **consoletoepassing** met C# en noem deze *SqlElasticPoolConsoleApp*

Als u een SQL Database met C# wilt maken, laadt u de vereiste beheerbibliotheken (met behulp van de [Package Manager-console](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klik op **Hulpprogramma's** > **NuGet Package Manager** > **Package Manager-console**.
2. Typ `Install-Package Microsoft.Azure.Management.Sql –Pre` om de [Microsoft Azure SQL-beheerbibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql) te installeren.
3. Typ `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` om de [Microsoft Azure Resource Manager-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager) te installeren.
4. Typ `Install-Package Microsoft.Azure.Common.Authentication –Pre` om de [algemene Microsoft Azure-verificatiebibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication) te installeren. 

> [!NOTE]
> De voorbeelden in dit artikel gebruiken een synchrone vorm van elke API-aanvraag en blokkeren totdat de REST-aanroep van de onderliggende service is voltooid. Er zijn asynchrone methoden beschikbaar.
> 
> 

## <a name="create-a-sql-elastic-pool---c-example"></a>Een elastische pool maken - voorbeeld met C#
In het volgende voorbeeld worden een resourcegroep, een server, een firewallregel en een elastische pool gemaakt. Daarna wordt in die pool een SQL-database gemaakt. Zie [Een service-principal maken voor toegang tot resources](#create-a-service-principal-to-access-resources) om de variabelen `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` op te halen.

Vervang de inhoud van **Program.cs** met het volgende en werk de `{variables}` bij met uw app-waarden (de `{}` niet overnemen).

```
using Microsoft.Azure;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.Azure.Management.Sql;
using Microsoft.Azure.Management.Sql.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace SqlElasticPoolConsoleApp
{
    class Program
        {

        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _poolName = "{pool-name}";
        static string _poolEdition = "{Standard}";
        static int _poolDtus = {100};
        static int _databaseMinDtus = {10};
        static int _databaseMaxDtus = {100};

        static string _databaseName = "{elasticdbfromcsarticle}";



        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Elastic pool...");
            ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);
            Console.WriteLine("Elastic pool: " + epr.ElasticPool.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static ElasticPoolCreateOrUpdateResponse CreateOrUpdateElasticDatabasePool(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string poolName, string poolEdition, int poolDtus, int databaseMinDtus, int databaseMaxDtus)
        {
            // Retrieve the server that will host this elastic pool
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus,
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlMgmtClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }




        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string poolName)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    ElasticPoolName = poolName
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
    }
}
```





## <a name="create-a-service-principal-to-access-resources"></a>Een service-principal maken voor toegang tot resources
Het volgende PowerShell-script maakt de Active Directory-toepassing (AD) en de service-principal die we nodig hebben om onze C#-app te verifiëren. Het script voert de waarden uit die we nodig hebben voor het voorgaande C#-voorbeeld. Zie [Use Azure PowerShell to create a service principal to access resources](../azure-resource-manager/resource-group-authenticate-service-principal.md) (Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources) voor gedetailleerde informatie.

    # Sign in to Azure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="next-steps"></a>Volgende stappen
* [Uw groep beheren](sql-database-elastic-pool-manage-csharp.md)
* [Elastische taken maken](sql-database-elastic-jobs-overview.md): met elastische taken kunt u T-SQL-scripts uitvoeren op een willekeurig aantal databases in de groep.
* [Uitschalen met Azure SQL Database](sql-database-elastic-scale-introduction.md): gebruik elastische databasehulpprogramma's om uit te schalen.

## <a name="additional-resources"></a>Aanvullende resources
* [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)
* [Azure Resource Management API's](https://msdn.microsoft.com/library/azure/dn948464.aspx)




<!--HONumber=Dec16_HO4-->



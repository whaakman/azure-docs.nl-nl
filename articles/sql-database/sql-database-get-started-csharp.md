---
title: 'SQL Database uitproberen: een SQL-database maken met C# | Microsoft Docs'
description: Probeer SQL Database om SQL- en C#-apps te ontwikkelen en om een Azure SQL Database te maken met C# met behulp van de SQL Database-bibliotheek voor .NET.
keywords: sql, sql c proberen#
services: sql-database
documentationcenter: ''
author: stevestein
manager: jhubbard
editor: cgronlun

ms.service: sql-database
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: csharp
ms.workload: data-management
ms.date: 10/04/2016
ms.author: sstein

---
# SQL Database uitproberen: een SQL-database maken met C# met de SQL Database-bibliotheek voor .NET
> [!div class="op_single_selector"]
> * [Azure-portal](sql-database-get-started.md)
> * [C#](sql-database-get-started-csharp.md)
> * [PowerShell](sql-database-get-started-powershell.md)
> 
> 

Lees hoe u C# gebruikt om een Azure SQL Database te maken met de [Microsoft Azure SQL Management Library voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). In dit artikel wordt beschreven hoe u een individuele database met SQL en C# maakt. Zie [Een pool voor elastische database maken](sql-database-elastic-pool-create-csharp.md) als u een pool voor elastische database wilt maken.

De Azure SQL Database Management Library voor .NET biedt een op [Azure Resource Manager](../resource-group-overview.md) gebaseerde API die de op [Resource Manager gebaseerde SQL Database-REST-API](https://msdn.microsoft.com/library/azure/mt163571.aspx) bevat.

> [!NOTE]
> Veel nieuwe functies van SQL Database worden alleen ondersteund als u het [Azure Resource Manager-implementatiemodel](../resource-group-overview.md) gebruikt. Daarom moet u altijd de nieuwste **Azure SQL Database Management Library voor .NET gebruiken ([docs](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. De oudere [klassieke op implementatiemodellen gebaseerde bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) worden alleen ondersteund voor achterwaartse compatibiliteit. Wij raden daarom aan de nieuwere op Resource Manager gebaseerde bibliotheken te gebruiken.
> 
> 

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

* Een Azure-abonnement. Als u een Azure-abonnement nodig hebt, klikt u op **GRATIS ACCOUNT** boven aan deze pagina. Keer daarna hier terug om dit artikel te voltooien.
* Visual Studio. Ga naar de pagina [Visual Studio-downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) voor een gratis exemplaar van Visual Studio.

> [!NOTE]
> Dit artikel maakt een nieuwe, lege SQL-database. Wijzig de methode *CreateOrUpdateDatabase(...)* in het volgende voorbeeld voor het kopiëren van databases, het schalen van databases, het maken van een database in een pool enz. Zie de klassen [DatabaseCreateMode](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreatemode.aspx) en [DatabaseProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databaseproperties.aspx) voor meer informatie.
> 
> 

## Een consoletoepassing maken en de vereiste bibliotheken installeren
1. Start Visual Studio.
2. Klik op **Bestand** > **Nieuw** > **Project**.
3. Maak een **consoletoepassing** met C# en noem deze *SqlDbConsoleApp*

Als u een SQL Database met C# wilt maken, laadt u de vereiste beheerbibliotheken (met behulp van de [Package Manager-console](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klik op **Hulpprogramma's** > **NuGet Package Manager** > **Package Manager-console**.
2. Typ `Install-Package Microsoft.Azure.Management.Sql –Pre` om de nieuwste [Microsoft Azure SQL-beheerbibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql) te installeren.
3. Typ `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` om de [Microsoft Azure Resource Manager-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager) te installeren.
4. Typ `Install-Package Microsoft.Azure.Common.Authentication –Pre` om de [algemene Microsoft Azure-verificatiebibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication) te installeren. 

> [!NOTE]
> De voorbeelden in dit artikel gebruiken een synchrone vorm van elke API-aanvraag en blokkeren totdat de REST-aanroep van de onderliggende service is voltooid. Er zijn asynchrone methoden beschikbaar.
> 
> 

## Een SQL Database-server, firewallregel en SQL Database maken: C#-voorbeeld
In het volgende voorbeeld worden een resourcegroep, een server, een firewallregel en een SQL-database gemaakt. Zie [Een service-principal maken voor toegang tot resources](#create-a-service-principal-to-access-resources) om de variabelen `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` op te halen.

Vervang de inhoud van **Program.cs** met het volgende en werk de `{variables}` bij met uw app-waarden (de `{}` niet overnemen).

    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace SqlDbConsoleApp
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

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


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

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
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



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
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
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
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





## Een service-principal maken voor toegang tot resources
Het volgende PowerShell-script maakt de Active Directory-toepassing (AD) en de service-principal die we nodig hebben om onze C#-app te verifiëren. Het script voert de waarden uit die we nodig hebben voor het voorgaande C#-voorbeeld. Zie [Use Azure PowerShell to create a service principal to access resources](../resource-group-authenticate-service-principal.md) (Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources) voor gedetailleerde informatie.

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



## Volgende stappen
Nu u SQL Database hebt uitgeprobeerd en een database hebt ingesteld met C#, bent u klaar voor de volgende artikelen:

* [Verbinding maken met SQL Database met SQL Server Management Studio en een voorbeeld-T-SQL-query uitvoeren](sql-database-connect-query-ssms.md)

## Aanvullende resources
* [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)
* [Databaseklasse](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)

<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png



<!--HONumber=Oct16_HO3-->



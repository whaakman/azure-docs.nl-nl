---
title: Management .NET SDK v1.x voor Azure Stream Analytics
description: Aan de slag met Stream Analytics Management .NET SDK. Informatie over het instellen en analytics-taken uitvoeren. Maak een project, invoer, uitvoer en transformaties.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: bf4f7e8f5837aaa56745e60df5a4960a93097a92
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729624"
---
# <a name="set-up-and-run-analytics-jobs-using-azure-stream-analytics-api-for-net"></a>Instellen en uitvoeren van analysetaken met behulp van Azure Stream Analytics-API voor .NET
Informatie over het instellen en uitvoeren van analysetaken met behulp van de Stream Analytics-API voor .NET met behulp van de Management .NET-SDK. Instellen van een project, invoer en uitvoer bronnen, transformaties en start maken en taken stoppen. Voor de analytics-taken, kunt u gegevens uit Blob storage of van een event hub streamen.

Zie de [management referentiedocumentatie voor de Stream Analytics-API voor .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics is een volledig beheerde service met lage latentie, maximaal beschikbare, schaalbare, complexe gebeurtenisverwerking bieden over het streamen van gegevens in de cloud. Stream Analytics kan klanten voor het instellen van streamingtaken gegevensstromen te analyseren, en kan ze om bijna realtime analyses te stimuleren.  

> [!NOTE]
> Voorbeeldcode in dit artikel wordt nog steeds legacy (1.x) versie van Azure Stream Analytics Management .NET SDK. Zie voor voorbeelden van code met behulp van de bijgewerkte SDK-versie kunt [Management .NET SDK gebruiken voor Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-dotnet-management-sdk).

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* Installeer Visual Studio 2017 of 2015.
* Download en installeer [Azure .NET SDK](https://azure.microsoft.com/downloads/).
* Maak een Azure-resourcegroep in uw abonnement. Hier volgt een voorbeeld van Azure PowerShell-script. Zie voor meer informatie voor Azure PowerShell, [installeren en configureren van Azure PowerShell](/powershell/azure/overview);  

   ```powershell
   # Log in to your Azure account
   Add-AzureAccount
   # Select the Azure subscription you want to use to create the resource group
   Select-AzureSubscription -SubscriptionName <subscription name>
       # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the    Register-AzProvider cmdlet to register the provider namespace
       #Register-AzProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
   # Create an Azure resource group
   New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
   ```

* Instellen van een invoer bron en bestemming voor uitvoer voor de taak verbinding maken met.

## <a name="set-up-a-project"></a>Een project instellen
Voor het maken van een analytics-taak gebruik van de Stream Analytics-API voor .NET, eerst instellen van uw project.

1. Maak een Visual Studio C# .NET-consoletoepassing.
2. Voer de volgende opdrachten om de NuGet-pakketten te installeren in de Package Manager-Console. De eerste is de Azure Stream Analytics Management .NET SDK. De tweede waarde is de Azure Active Directory-client die wordt gebruikt voor verificatie.

```powershell
Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 1.8.3
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.4
```

3. Voeg de volgende **appSettings** sectie aan het bestand App.config:

   ```csharp
   <appSettings>
     <!--CSM Prod related values-->
     <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
     <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
     <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
     <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
     <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
     <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
     <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
   </appSettings>
   ```

    Vervang de waarden in **SubscriptionId** en **ActiveDirectoryTenantId** met uw Azure-abonnement en tenant-id. U kunt deze waarden kunt krijgen door de volgende Azure PowerShell-cmdlet:

        Get-AzureAccount

4. De volgende verwijzing toevoegen in uw .csproj-bestand:

   ```csharp
   <Reference Include="System.Configuration" />
   ```

1. Voeg de volgende **met behulp van** instructies toe aan het bronbestand (Program.cs) in het project:

```csharp
using System;
using System.Configuration;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.StreamAnalytics;
using Microsoft.Azure.Management.StreamAnalytics.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

2. Toevoegen van een Help-methode voor verificatie:

   ```csharp
   private static async Task<string> GetAuthorizationHeader()
   {
       var context = new AuthenticationContext(
           ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
           ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

        AuthenticationResult result = await context.AcquireTokenASync(
           resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
           clientId: ConfigurationManager.AppSettings["AsaClientId"],
           redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
           promptBehavior: PromptBehavior.Always);

        if (result != null)
            return result.AccessToken;

       throw new InvalidOperationException("Failed to acquire token");
   }
   ```  

## <a name="create-a-stream-analytics-management-client"></a>Een Stream Analytics management-client maken
Een **StreamAnalyticsManagementClient** object kunt u voor het beheren van de taak en de onderdelen van de taak, zoals invoer, uitvoer en transformatie.

Voeg de volgende code toe aan het begin van de **Main** methode:

   ```csharp
   string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
   string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
   string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
   string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
   string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";
   // Get authentication token
   TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
       ConfigurationManager.AppSettings["SubscriptionId"],
       GetAuthorizationHeader().Result);
   // Create Stream Analytics management client
   StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);
   ```

De **resourceGroupName** van de variabele waarde moet gelijk zijn aan de naam van de resourcegroep die u hebt gemaakt of opgenomen in de vereiste stappen.

Voor het automatiseren van de referentie-presentatie aspect van de taak te maken, Raadpleeg [verifiëren van een service-principal met Azure Resource Manager](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

De rest van dit artikel wordt ervan uitgegaan dat deze code aan het begin van de **Main** methode.

## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken
De volgende code maakt u een Stream Analytics-taak onder de resourcegroep die u hebt gedefinieerd. Aan de job wordt u een invoer, uitvoer en transformatie later toevoegen.

   ```csharp
   // Create a Stream Analytics job
   JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
   {
       Job = new Job()
       {
           Name = streamAnalyticsJobName,
           Location = "<LOCATION>",
           Properties = new JobProperties()
           {
               EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
               Sku = new Sku()
               {
                   Name = "Standard"
               }
           }
       }
   };
   JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);
   ```

## <a name="create-a-stream-analytics-input-source"></a>Een Stream Analytics-invoerbron maken
De volgende code maakt een Stream Analytics-invoerbron met de blob-invoerbron type en de CSV-serialisatie. Gebruik voor het maken van een event hub-invoerbron **EventHubStreamInputDataSource** in plaats van **BlobStreamInputDataSource**. Op deze manier kunt u het serialisatietype van de invoerbron aanpassen.

   ```csharp
   // Create a Stream Analytics input source
   InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
   {
       Input = new Input()
       {
           Name = streamAnalyticsInputName,
           Properties = new StreamInputProperties()
           {
               Serialization = new CsvSerialization
               {
                   Properties = new CsvSerializationProperties
                   {
                       Encoding = "UTF8",
                       FieldDelimiter = ","
                   }
               },
               DataSource = new BlobStreamInputDataSource
               {
                   Properties = new BlobStreamInputDataSourceProperties
                   {
                       StorageAccounts = new StorageAccount[]
                       {
                           new StorageAccount()
                           {
                               AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                               AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                           }
                       },
                       Container = "samples",
                       PathPattern = ""
                   }
               }
           }
       }
   };
   InputCreateOrUpdateResponse inputCreateResponse =
   client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);
   ```

Invoerbronnen, zijn hetzij van Blob-opslag of een event hub, gekoppeld aan een specifieke taak. U moet voor het gebruik van de dezelfde invoerbron voor verschillende taken, roept u de methode opnieuw en geef een andere taaknaam.

## <a name="test-a-stream-analytics-input-source"></a>Een Stream Analytics-invoerbron testen
De **TestConnection** methode wordt gecontroleerd of de Stream Analytics-taak is geen verbinding maken met de invoerbron, evenals andere aspecten die specifiek zijn voor het type invoerbron. Bijvoorbeeld, in de blob-invoerbron die u in een eerdere stap hebt gemaakt, wordt de methode gecontroleerd dat de Storage-accountnaam en het sleutelpaar kunnen worden gebruikt voor verbinding maken met de Storage-account, evenals controleren of de opgegeven container bestaat.

   ```csharp
   // Test input source connection
   DataSourceTestConnectionResponse inputTestResponse =
       client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a>Maak het doel van een Stream Analytics-uitvoer
Het maken van een bestemming voor uitvoer is vergelijkbaar met het maken van een Stream Analytics-invoerbron. Uitvoer doelen zijn, zoals invoerbronnen gekoppeld aan een specifieke taak. U moet voor het gebruik van de dezelfde bestemming voor uitvoer voor verschillende taken, roept u de methode opnieuw en geef een andere taaknaam.

De volgende code maakt een bestemming voor uitvoer (Azure SQL database). U kunt het gegevenstype van het uitvoerdoel en/of dit serialisatietype aanpassen.

   ```csharp
   // Create a Stream Analytics output target
   OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
   {
       Output = new Output()
       {
           Name = streamAnalyticsOutputName,
           Properties = new OutputProperties()
           {
               DataSource = new SqlAzureOutputDataSource()
               {
                   Properties = new SqlAzureOutputDataSourceProperties()
                   {
                       Server = "<YOUR DATABASE SERVER NAME>",
                       Database = "<YOUR DATABASE NAME>",
                       User = "<YOUR DATABASE LOGIN>",
                       Password = "<YOUR DATABASE LOGIN PASSWORD>",
                       Table = "<YOUR DATABASE TABLE NAME>"
                   }
               }
           }
       }
   };
   OutputCreateOrUpdateResponse outputCreateResponse =
       client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);
   ```

## <a name="test-a-stream-analytics-output-target"></a>Een Stream Analytics-bestemming voor uitvoer testen
Een Stream Analytics-bestemming voor uitvoer heeft ook de **TestConnection** methode voor het testen van verbindingen.

   ```csharp
   // Test output target connection
   DataSourceTestConnectionResponse outputTestResponse =
       client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a>Maken van een Stream Analytics-transformatie
De volgende code maakt u een Stream Analytics-transformatie met de query ' Selecteer * uit invoer ' en geeft u voor het toewijzen van één streaming-eenheid voor de Stream Analytics-taak. Zie voor meer informatie over het aanpassen van streaming-eenheden [Scale Azure Stream Analytics-taken](stream-analytics-scale-jobs.md).

   ```csharp
   // Create a Stream Analytics transformation
   TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
   {
       Transformation = new Transformation()
       {
           Name = streamAnalyticsTransformationName,
           Properties = new TransformationProperties()
           {
               StreamingUnits = 1,
               Query = "select * from Input"
           }
       }
   };
   var transformationCreateResp =
       client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);
   ```

Net als invoer en uitvoer, is ook een transformatie gekoppeld aan de specifieke Stream Analytics-taak die is gemaakt onder.

## <a name="start-a-stream-analytics-job"></a>Een Stream Analytics-taak starten
Na het maken van een Stream Analytics-taak en de invoerwaarde(n), output(s) en -transformatie, kunt u de taak starten door het aanroepen van de **Start** methode.

Voorbeeld van de volgende code wordt gestart een Stream Analytics-taak met een begintijd aangepaste uitvoer ingesteld op 12 December 2012 12:12:12 UTC:

   ```csharp
   // Start a Stream Analytics job
   JobStartParameters jobStartParameters = new JobStartParameters
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
   };
   
   LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName,    jobStartParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a>Een Stream Analytics-taak stoppen
U kunt een actieve Stream Analytics-taak stoppen door het aanroepen van de **stoppen** methode.

   ```csharp
   // Stop a Stream Analytics job
   LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a>Een Stream Analytics-taak verwijderen
De **verwijderen** methode de taak, evenals de onderliggende onderliggende resources, met inbegrip van invoerwaarde(n) output(s) en transformatie van de taak wordt verwijderd.

   ```csharp
   // Delete a Stream Analytics job
   LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);
   ```

## <a name="get-support"></a>Ondersteuning krijgen
Voor verdere ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
De basisbeginselen van het gebruik van een .NET-SDK maken en uitvoeren van analysetaken, hebt u geleerd. Raadpleeg de volgende onderwerpen voor meer informatie:

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: https://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: https://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: https://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: https://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

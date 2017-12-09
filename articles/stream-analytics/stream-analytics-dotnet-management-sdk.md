---
title: Beheer .NET SDK voor Azure Stream Analytics | Microsoft Docs
description: Aan de slag met Stream Analytics Management .NET SDK. Informatie over het instellen en analytics-taken uitvoeren. Maak een project, invoer, uitvoer en transformaties.
keywords: .NET SDK, analytics API
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 5e93de87-0c6f-4f4b-be98-08d63f832897
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/06/2017
ms.author: samacha
ms.openlocfilehash: 2ac5d305aae110eff46459ecb7d89ca50ae1823d
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Beheer van de .NET SDK: Instellen en uitvoeren van analytics-taken met de Azure Stream Analytics-API voor .NET
Informatie over het instellen en uitvoeren met behulp van de Stream Analytics-API voor .NET met de Management .NET SDK analytics-taken. Instellen van een project, invoer en uitvoer bronnen, transformaties en start maken en taken stoppen. U kunt gegevens uit Blob-opslag of van een event hub streamen voor uw analytics-taken.

Zie de [management-naslagdocumentatie voor de Stream Analytics-API voor .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics is een volledig beheerde service die de verwerking van gebeurtenissen met lage latentie, maximaal beschikbare, schaalbare, complexe geven via het streamen van gegevens in de cloud. Stream Analytics kan klanten voor het instellen van het streaming-taken voor het analyseren van gegevensstromen en kan ze bijna realtime analyses station.  

> [!NOTE]
> De voorbeeldcode in dit artikel hebt met Azure Stream Analytics Management .NET SDK v2.x versie is bijgewerkt. Zie voor een voorbeeld van code met behulp van de SDK-versie van het lagecy (1.x) gebruikt, [de v1.x Management .NET SDK gebruiken voor Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* Installeer Visual Studio 2017 of 2015.
* Download en installeer [Azure .NET SDK](https://azure.microsoft.com/downloads/).
* Maak een Azure-resourcegroep in uw abonnement. Hier volgt een voorbeeld Azure PowerShell-script. Zie voor informatie Azure PowerShell [installeren en configureren van Azure PowerShell](/powershell/azure/overview);  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* Instellen van een invoer bron- en uitvoer te gebruiken. Voor verdere instructies raadpleegt u [invoer toevoegen](stream-analytics-add-inputs.md) voor het instellen van een Voorbeeldinvoer en [uitvoer toevoegen](stream-analytics-add-outputs.md) voor het instellen van een voorbeeld van uitvoer.

## <a name="set-up-a-project"></a>Instellen van een project
U maakt met een analytics-taak de Stream Analytics-API voor .NET, Stel eerst uw project.

1. Maak een Visual Studio C# .NET-consoletoepassing.
2. Voer de volgende opdrachten om de NuGet-pakketten te installeren in de Package Manager-Console. De eerste is de Azure Stream Analytics Management .NET SDK. Het tweede is voor Azure clientverificatie.
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 2.0.0
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.3.1
3. Voeg de volgende **appSettings** sectie aan het bestand App.config:
   
        <appSettings>
          <add key="ClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR SUBSCRIPTION ID" />
          <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
        </appSettings>

    Vervang de waarden voor **SubscriptionId** en **ActiveDirectoryTenantId** met uw Azure-abonnement en tenant-id. U kunt deze waarden krijgen door de volgende Azure PowerShell-cmdlet:

        Get-AzureAccount

4. De volgende verwijzing toevoegen in uw .csproj-bestand:

        <Reference Include="System.Configuration" />

5. Voeg de volgende **met** instructies naar het bronbestand (Program.cs) in het project:
   
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.Threading;
        using System.Threading.Tasks;
        
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Rest;
6. Toevoegen van een Help-methode voor verificatie:

   ```
   private static async Task<ServiceClientCredentials> GetCredentials()
   {
       var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(ConfigurationManager.AppSettings["ClientId"], new Uri("urn:ietf:wg:oauth:2.0:oob"));
       ServiceClientCredentials credentials = await UserTokenProvider.LoginWithPromptAsync(ConfigurationManager.AppSettings["ActiveDirectoryTenantId"], activeDirectoryClientSettings);
       
       return credentials;
    }
   ```

## <a name="create-a-stream-analytics-management-client"></a>Maken van een Stream Analytics management-client
Een **StreamAnalyticsManagementClient** object kunt u voor het beheren van de taak en de onderdelen van de taak, zoals invoer, uitvoer en transformatie.

Voeg de volgende code toe aan het begin van de **Main** methode:

   ```
    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamingJobName = "<YOUR STREAMING JOB NAME>";
    string inputName = "<YOUR JOB INPUT NAME>";
    string transformationName = "<YOUR JOB TRANSFORMATION NAME>";
    string outputName = "<YOUR JOB OUTPUT NAME>";
    
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    
    // Get credentials
    ServiceClientCredentials credentials = GetCredentials().Result;
    
    // Create Stream Analytics management client
    StreamAnalyticsManagementClient streamAnalyticsManagementClient = new StreamAnalyticsManagementClient(credentials)
    {
        SubscriptionId = ConfigurationManager.AppSettings["SubscriptionId"]
    };
   ```

De **resourceGroupName** de waarde van variabele moet hetzelfde zijn als de naam van de resourcegroep die u hebt gemaakt of opgenomen in de vereiste stappen.

Raadpleeg voor het automatiseren van de referentie presentatie aspect van het maken van de taak [verifiëren van een service-principal met Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).

De resterende secties van dit artikel wordt ervan uitgegaan dat deze code aan het begin van de **Main** methode.

## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken
De volgende code maakt een Stream Analytics-taak onder de resourcegroep die u hebt gedefinieerd. Aan de job wordt u een invoer, uitvoer en transformatie later toevoegen.

   ```
   // Create a streaming job
   StreamingJob streamingJob = new StreamingJob()
   {
       Tags = new Dictionary<string, string>()
       {
           { "Origin", ".NET SDK" },
           { "ReasonCreated", "Getting started tutorial" }
       },
       Location = "West US",
       EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Drop,
       EventsOutOfOrderMaxDelayInSeconds = 5,
       EventsLateArrivalMaxDelayInSeconds = 16,
       OutputErrorPolicy = OutputErrorPolicy.Drop,
       DataLocale = "en-US",
       CompatibilityLevel = CompatibilityLevel.OneFullStopZero,
       Sku = new Sku()
       {
           Name = SkuName.Standard
       }
   };
   StreamingJob createStreamingJobResult = streamAnalyticsManagementClient.StreamingJobs.CreateOrReplace(streamingJob, resourceGroupName, streamingJobName);
   ```

## <a name="create-a-stream-analytics-input-source"></a>Maken van een Stream Analytics-invoerbron
De volgende code maakt een Stream Analytics-invoerbron blob-invoerbron type en met de CSV-serialisatie. Gebruik voor het maken van een event hub-invoerbron **EventHubStreamInputDataSource** in plaats van **BlobStreamInputDataSource**. Op deze manier kunt u het serialisatietype van de invoerbron aanpassen.

   ```
   // Create an input
   StorageAccount storageAccount = new StorageAccount()
   {
       AccountName = "<YOUR STORAGE ACCOUNT NAME>",
       AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
   };
   Input input = new Input()
   {
       Properties = new StreamInputProperties()
       {
           Serialization = new CsvSerialization()
           {
               FieldDelimiter = ",",
               Encoding = Encoding.UTF8
           },
           Datasource = new BlobStreamInputDataSource()
           {
               StorageAccounts = new[] { storageAccount },
               Container = "<YOUR STORAGE BLOB CONTAINER>",
               PathPattern = "{date}/{time}",
               DateFormat = "yyyy/MM/dd",
               TimeFormat = "HH",
               SourcePartitionCount = 16
           }
       }
   };
   Input createInputResult = streamAnalyticsManagementClient.Inputs.CreateOrReplace(input, resourceGroupName, streamingJobName, inputName);
   ```

Invoermethoden, zijn van Blob-opslag of een event hub, gekoppeld aan een specifieke taak. Voor het gebruik van dezelfde bron voor invoer voor andere taken, moet u Roep de methode opnieuw en geef een andere taaknaam.

## <a name="test-a-stream-analytics-input-source"></a>Een Stream Analytics-invoerbron testen
De **TestConnection** methode wordt gecontroleerd of de Stream Analytics-taak is geen verbinding maken met de invoerbron evenals andere aspecten die specifiek zijn voor het type van de invoerbron. Bijvoorbeeld, in de blob-invoerbron die u in een eerdere stap hebt gemaakt, wordt de methode gecontroleerd dat de naam van het Opslagaccount en een sleutelpaar kan worden gebruikt voor verbinding maken met de Storage-account, evenals te controleren of de opgegeven container bestaat.

   ```
   // Test the connection to the input
   ResourceTestStatus testInputResult = streamAnalyticsManagementClient.Inputs.Test(resourceGroupName, streamingJobName, inputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a>Maken van een Stream Analytics-uitvoer-doel
Maken van een uitvoerdoel is vergelijkbaar met het maken van een Stream Analytics-invoerbron. Uitvoer doelen zijn zoals invoermethoden gekoppeld aan een specifieke taak. Als u hetzelfde uitvoerdoel voor andere taken, moet u Roep de methode opnieuw en geef een andere taaknaam.

De volgende code maakt een uitvoerdoel (Azure SQL database). U kunt aanpassen van het uitvoerdoel-gegevenstype en/of serialisatietype.

   ```
   // Create an output
   Output output = new Output()
   {
       Datasource = new AzureSqlDatabaseOutputDataSource()
       {
           Server = "<YOUR DATABASE SERVER NAME>",
           Database = "<YOUR DATABASE NAME>",
           User = "<YOUR DATABASE LOGIN>",
           Password = "<YOUR DATABASE LOGIN PASSWORD>",
           Table = "<YOUR DATABASE TABLE NAME>"
       }
   };
   Output createOutputResult = streamAnalyticsManagementClient.Outputs.CreateOrReplace(output, resourceGroupName, streamingJobName, outputName);
   ```

## <a name="test-a-stream-analytics-output-target"></a>Testen van een Stream Analytics-uitvoer-doel
Een doel van de uitvoer Stream Analytics heeft ook de **TestConnection** methode voor het testen van verbindingen.

   ```
   // Test the connection to the output
   ResourceTestStatus testOutputResult = streamAnalyticsManagementClient.Outputs.Test(resourceGroupName, streamingJobName, outputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a>Maken van een Stream Analytics-transformatie
De volgende code maakt een Stream Analytics-transformatie met de query ' Selecteer * uit invoer ' en Hiermee geeft u op één streaming-eenheid toewijzen voor de Stream Analytics-taak. Zie voor meer informatie over het aanpassen van streaming-eenheden [Scale Azure Stream Analytics-taken](stream-analytics-scale-jobs.md).

   ```
   // Create a transformation
   Transformation transformation = new Transformation()
   {
       Query = "Select Id, Name from <your input name>", // '<your input name>' should be replaced with the value you put for the 'inputName' variable above or in a previous step
       StreamingUnits = 1
   };
   Transformation createTransformationResult = streamAnalyticsManagementClient.Transformations.CreateOrReplace(transformation, resourceGroupName, streamingJobName, transformationName);
   ```

Net als invoer en uitvoer, is ook een transformatie gekoppeld aan de specifieke Stream Analytics-taak die is gemaakt onder.

## <a name="start-a-stream-analytics-job"></a>Start een Stream Analytics-taak
Nadat een Stream Analytics-taak en de input(s), uitvoer en transformatie is gemaakt, kunt u de taak starten door het aanroepen van de **Start** methode.

De volgende code wordt gestart een Stream Analytics-taak met een begintijd aangepaste uitvoer ingesteld op 12 December 2012 12:12:12 steekproef UTC:

   ```
   // Start a streaming job
   StartStreamingJobParameters startStreamingJobParameters = new StartStreamingJobParameters()
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 12, 12, 12, DateTimeKind.Utc)
   };
   streamAnalyticsManagementClient.StreamingJobs.Start(resourceGroupName, streamingJobName, startStreamingJobParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a>Een Stream Analytics-taak stoppen
U kunt een actieve Stream Analytics-taak stoppen door het aanroepen van de **stoppen** methode.

   ```
   // Stop a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Stop(resourceGroupName, streamingJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a>Verwijderen van een Stream Analytics-taak
De **verwijderen** methode verwijdert de taak, evenals de onderliggende submappen resources, zoals input(s), uitvoer en transformatie van de taak.

   ```
   // Delete a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Delete(resourceGroupName, streamingJobName);
   ```

## <a name="get-support"></a>Ondersteuning krijgen
Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
U hebt de basisbeginselen van het gebruik van een .NET SDK maken en analytics-taken uitvoeren. Raadpleeg de volgende onderwerpen voor meer informatie:

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
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

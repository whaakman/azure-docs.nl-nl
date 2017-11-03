---
title: Programmatisch bewaken taken in de Stream Analytics | Microsoft Docs
description: Informatie over het bewaken van programmatisch Stream Analytics-taken die zijn gemaakt via de REST-API's, Azure SDK of PowerShell.
keywords: .NET-monitor, taak monitor, bewaking van app
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 2ec02cc9-4ca5-4a25-ae60-c44be9ad4835
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: 7e9d2f6f03fd539c59b105108fb46697bcd60f1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Een monitor Stream Analytics-taak maken

In dit artikel laat zien hoe bewaking voor een Stream Analytics-taak inschakelen. Stream Analytics-taken die zijn gemaakt via de REST-API's, Azure SDK of PowerShell beschikt niet over bewaking standaard ingeschakeld. U kunt handmatig inschakelen in de Azure portal door te gaan naar de pagina van de Monitor van de taak en te klikken op de knop inschakelen of u kunt dit proces automatiseren door de stappen in dit artikel. De bewakingsgegevens wordt weergegeven in het gebied van de metrische gegevens van de Azure-portal voor uw Stream Analytics-taak.

## <a name="prerequisites"></a>Vereisten

Voordat u deze procedure begint, moet u het volgende hebben:

* Visual Studio 2017 of 2015
* [Azure SDK voor .NET](https://azure.microsoft.com/downloads/) gedownload en geïnstalleerd
* Een bestaande Stream Analytics-taak die u moet beschikken over bewaking ingeschakeld

## <a name="create-a-project"></a>Een project maken

1. Maak een Visual Studio C# .NET-consoletoepassing.
2. Voer de volgende opdrachten om de NuGet-pakketten te installeren in de Package Manager-Console. De eerste is de Azure Stream Analytics Management .NET SDK. Het tweede is de Monitor-SDK van Azure die wordt gebruikt voor het inschakelen van bewaking. Laatste is de Azure Active Directory-client die wordt gebruikt voor verificatie.
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. Voeg de volgende sectie appSettings aan het bestand App.config.
   
   ```
   <appSettings>
     <!--CSM Prod related values-->
     <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
     <add key="JobName" value="YOUR JOB NAME" />
     <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
     <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
     <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
     <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
     <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
     <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
     <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
     <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
   </appSettings>
   ```
   Vervang de waarden voor *SubscriptionId* en *ActiveDirectoryTenantId* met uw Azure-abonnement en tenant-id. U kunt deze waarden krijgen door de volgende PowerShell-cmdlet:
   
   ```
   Get-AzureAccount
   ```
4. Voeg de volgende using-instructies aan het bronbestand (Program.cs) in het project.
   
   ```
     using System;
     using System.Configuration;
     using System.Threading;
     using Microsoft.Azure;
     using Microsoft.Azure.Management.Insights;
     using Microsoft.Azure.Management.Insights.Models;
     using Microsoft.Azure.Management.StreamAnalytics;
     using Microsoft.Azure.Management.StreamAnalytics.Models;
     using Microsoft.IdentityModel.Clients.ActiveDirectory;
   ```
5. Toevoegen van een Help-methode voor verificatie.
   
     openbare statische tekenreeks GetAuthorizationHeader()
   
         {
             AuthenticationResult result = null;
             var thread = new Thread(() =>
             {
                 try
                 {
                     var context = new AuthenticationContext(
                         ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                         ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
   
                     result = context.AcquireToken(
                         resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                         clientId: ConfigurationManager.AppSettings["AsaClientId"],
                         redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                         promptBehavior: PromptBehavior.Always);
                 }
                 catch (Exception threadEx)
                 {
                     Console.WriteLine(threadEx.Message);
                 }
             });
   
             thread.SetApartmentState(ApartmentState.STA);
             thread.Name = "AcquireTokenThread";
             thread.Start();
             thread.Join();
   
             if (result != null)
             {
                 return result.AccessToken;
             }
   
             throw new InvalidOperationException("Failed to acquire token");
     }

## <a name="create-management-clients"></a>Management-clients maken

De volgende code wordt de benodigde variabelen en van beheerclients instellen.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Bewaking voor een bestaande Stream Analytics-taak inschakelen

De volgende code kunt bewaking voor een **bestaande** Stream Analytics-taak. Het eerste deel van de code wordt een GET-aanvraag tegen de Stream Analytics-service voor het ophalen van informatie over de specifieke Stream Analytics-taak uitgevoerd. Dit maakt gebruik van de *Id* eigenschap (opgehaald uit de GET-aanvraag) als een parameter voor de Put-methode in de tweede helft van de code die u een PUT verzendt-controle voor de Stream Analytics-taak inschakelen verzoek aan de Insights-service.

>[!WARNING]
>Als u eerder hebt ingeschakeld bewaking voor een andere Stream Analytics-taak, via de Azure portal of programmatisch via de onderstaande code, **wordt aangeraden dat u dezelfde naam van het opslagaccount dat u hebt gebruikt toen u eerder hebt ingeschakeld bewaking bieden.**
> 
> Het opslagaccount is gekoppeld aan de regio die u hebt uw Stream Analytics-taak in, niet expliciet aan de taak zelf gemaakt.
> 
> Alle Stream Analytics-taken (en alle andere Azure-resources) in die dezelfde regio dit opslagaccount voor het opslaan van bewakingsgegevens delen. Als u een ander opslagaccount opgeeft, kan dit ertoe leiden dat onbedoelde bijwerkingen in de bewaking van uw Stream Analytics-taken of andere Azure-resources.
> 
> De opslagaccountnaam die u gebruikt ter vervanging van `<YOUR STORAGE ACCOUNT NAME>` in de volgende code moet een opslagaccount die zich in hetzelfde abonnement als de Stream Analytics-taak die u inschakelen wilt voor bewaking.
> 
> 

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Ondersteuning krijgen

Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)


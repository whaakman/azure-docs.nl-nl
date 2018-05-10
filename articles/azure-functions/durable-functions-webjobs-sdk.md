---
title: Het uitvoeren van duurzame fungeert als WebJobs - Azure
description: Ontdek hoe u de code en duurzame functies uitvoeren in WebJobs met de WebJobs SDK configureren.
services: functions
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 3fc84d1492d2855ffa3bb5538226da049a928339
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-run-durable-functions-as-webjobs"></a>Het uitvoeren van duurzame fungeert als WebJobs

[Azure Functions](functions-overview.md) en de [duurzame functies](durable-functions-overview.md) uitbreiding zijn gebouwd op de [WebJobs SDK](../app-service/web-sites-create-web-jobs.md). De `JobHost` in de WebJobs SDK is de Azure Functions-runtime. Als u nodig hebt om te bepalen `JobHost` gedrag in manieren niet mogelijk in Azure Functions, die u kunt ontwikkelen en duurzame functies uitvoeren met behulp van de WebJobs SDK. Vervolgens kunt u uw duurzame functies uitvoeren in een webtaak Azure of ergens een consoletoepassing die wordt uitgevoerd.

De chaining duurzame functies voorbeeld is beschikbaar in een WebJobs SDK-versie: downloaden of te klonen de [duurzame functies opslagplaats](https://github.com/azure/azure-functions-durable-extension/) en navigeer naar de *voorbeelden\\webjobssdk\\-koppeling* map.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend bent met de basisprincipes van de WebJobs SDK, C# class library ontwikkeld voor Azure Functions en duurzame functies. Als u een inleiding tot deze onderwerpen nodig, raadpleegt u de volgende bronnen:

* [Aan de slag met de WebJobs SDK](../app-service/webjobs-sdk-get-started.md)
* [Uw eerste functie maken met Visual Studio](functions-create-your-first-function-visual-studio.md)
* [Duurzame functies](durable-functions-sequence.md)

De stappen in dit artikel:

* [Visual Studio 2017: 15,6 of hoger installeren](https://docs.microsoft.com/visualstudio/install/) met de **ontwikkelen van Azure** werkbelasting.

  Als u al Visual Studio hebt, maar geen werkbelasting, de werkbelasting toevoegen door te selecteren **Extra > ophalen's en onderdelen**. 

  (U kunt [Visual Studio Code](https://code.visualstudio.com/) in plaats daarvan, maar sommige van de instructies zijn specifiek voor Visual Studio.)

* Installeren en uitvoeren [Azure-Opslagemulator](../storage/common/storage-use-emulator.md) versie 5.2 of hoger. Een alternatief is om bij te werken de *App.config* bestand met een Azure Storage-verbindingsreeks.

## <a name="webjobs-sdk-versions"></a>WebJobs SDK-versies

In dit artikel wordt uitgelegd hoe u voor het ontwikkelen van een WebJobs SDK 2.x-project (gelijk aan de versie van Azure Functions 1.x). Voor informatie over versie 3.x, Zie [WebJobs SDK 3.x](#webjobs-sdk-3x) verderop in dit artikel. 

## <a name="create-console-app"></a>Consoletoepassing maken

Een project WebJobs SDK is net een console-app-project met de juiste NuGet-pakketten dat is geïnstalleerd.

In de Visual Studio **nieuw Project** dialoogvenster Selecteer **Classic Windows Desktop > Console-App (.NET Framework)**. In het projectbestand de `TargetFrameworkVersion` moet `v4.6.1`.

Visual Studio heeft ook een webtaak projectsjabloon, waarmee u door te selecteren kunt **Cloud > Azure webtaak (.NET Framework)**. Deze sjabloon wordt veel pakketten, die waarvan sommige u niet hoeft mogelijk geïnstalleerd.

## <a name="install-nuget-packages"></a>NuGet-pakketten installeren

In dat geval moet u NuGet-pakketten in voor de WebJobs SDK, core bindingen framework voor logboekregistratie en de extensie duurzame taak. Hier volgen **Package Manager Console** -opdrachten voor die pakketten, met de meest recente stabiele versienummers vanaf de datum in dit artikel is geschreven:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions -version 2.2.0
Install-Package Microsoft.Extensions.Logging -version 2.0.1
Install-Package Microsoft.Azure.WebJobs.Extensions.DurableTask -version 1.4.0
```

U moet ook logboekregistratie providers. Installeer de Application Insights-provider van de volgende opdrachten en de `ConfigurationManager`. De `ConfigurationManager` kunt u de Application Insights-instrumentatiesleutel ophalen van app-instellingen.

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
Install-Package System.Configuration.ConfigurationManager -version 4.4.1
```

De console-provider wordt geïnstalleerd in de volgende opdracht:

```powershell
Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
```

## <a name="jobhost-code"></a>JobHost code

Voor het gebruik van de extensie duurzame functies aanroepen `UseDurableTask` op de `JobHostConfiguration` object in uw `Main` methode:

```cs
var config = new JobHostConfiguration();
config.UseDurableTask(new DurableTaskExtension
{
    HubName = "MyTaskHub",
};
```

Voor een lijst met eigenschappen die u kunt instellen in de `DurableTaskExtension` object, Zie [host.json](functions-host-json.md#durabletask).

De `Main` methode is ook de plaats voor het instellen van logboekregistratie providers. Het volgende voorbeeld configureert-console en de Application Insights-providers.

```cs
static void Main(string[] args)
{
    using (var loggerFactory = new LoggerFactory())
    {
        var config = new JobHostConfiguration();

        config.DashboardConnectionString = "";

        var instrumentationKey =
            ConfigurationManager.AppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"];

        config.LoggerFactory = loggerFactory
            .AddApplicationInsights(instrumentationKey, null)
            .AddConsole();

        config.UseTimers();
        config.UseDurableTask(new DurableTaskExtension
        {
            HubName = "MyTaskHub",
        });
        var host = new JobHost(config);
        host.RunAndBlock();
    }
}
```

## <a name="functions"></a>Functions

Er zijn een paar verschillen in de code voor WebJobs SDK-functies ten opzichte van schrijven wat u voor de service Azure Functions schrijven.

De WebJobs SDK biedt geen ondersteuning voor de volgende functies van Azure Functions:

* [Functienaam kenmerk](#functionname-attribute)
* [HTTP-trigger](#http-trigger)
* [Duurzame functies HTTP-API voor beheer](#http-management-api)

### <a name="functionname-attribute"></a>Functienaam kenmerk

De naam van de methode van een functie is in een project WebJobs SDK de functienaam. De `FunctionName` kenmerk wordt alleen gebruikt in Azure Functions.

### <a name="http-trigger"></a>HTTP-trigger

De WebJobs SDK beschikt niet over een HTTP-trigger. Het voorbeeldproject orchestration client gebruikt een timertrigger:

```cs
public static async Task CronJob(
    [TimerTrigger("0 */2 * * * *")] TimerInfo timer,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger logger)
{
  ...
}
```

### <a name="http-management-api"></a>HTTP-management-API

Omdat deze geen HTTP-trigger, de WebJobs SDK heeft geen [HTTP management API](durable-functions-http-api.md).

U kunt methoden aanroepen voor het object van de client orchestration HTTP-aanvragen te verzenden in plaats van in een WebJobs SDK-project. De volgende methoden overeenkomen met de drie taken die u met de HTTP-API doen kunt:

* `GetStatusAsync`
* `RaiseEventAsync`
* `TerminateAsync`

De orchestration-client-functie in het voorbeeldproject de orchestrator-functie wordt gestart en wordt vervolgens in een lus die aanroept `GetStatusAsync` elke 2 seconden:

```cs
string instanceId = await client.StartNewAsync(nameof(HelloSequence), input: null);
logger.LogInformation($"Started new instance with ID = {instanceId}.");

DurableOrchestrationStatus status;
while (true)
{
    status = await client.GetStatusAsync(instanceId);
    logger.LogInformation($"Status: {status.RuntimeStatus}, Last update: {status.LastUpdatedTime}.");

    if (status.RuntimeStatus == OrchestrationRuntimeStatus.Completed ||
        status.RuntimeStatus == OrchestrationRuntimeStatus.Failed ||
        status.RuntimeStatus == OrchestrationRuntimeStatus.Terminated)
    {
        break;
    }

    await Task.Delay(TimeSpan.FromSeconds(2));
}
```

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Deze sectie biedt een overzicht van het uitvoeren van de [voorbeeldproject](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/webjobssdk/chaining). Zie voor gedetailleerde instructies waarin wordt uitgelegd hoe een WebJobs SDK-project lokaal uitvoeren en deze implementeren in een Azure-webtaak [aan de slag met de WebJobs SDK](../app-service/webjobs-sdk-get-started.md#deploy-as-a-webjob).

### <a name="run-locally"></a>Lokaal uitvoeren

1. Controleer of de opslagemulator wordt uitgevoerd (Zie [vereisten](#prerequisites)).

1. Als u zien van Logboeken in Application Insights wilt wanneer u lokaal uitvoeren:

  a. Een Application Insights-resource, app-type maken **algemene**.

  b. Sla de instrumentatiesleutel wordt in de *App.config* bestand.

1. Voer het project.

### <a name="run-in-azure"></a>In Azure uitvoeren

1. Een web-app en een opslagaccount maken.

1. In de web-app opslaan in de verbindingsreeks voor opslag in een app-instelling met de naam AzureWebJobsStorage.

1. Een Application Insights-resource, app-type maken **algemene**.

1. De instrumentatiesleutel opslaan in een app-instelling met de naam APPINSIGHTS_INSTRUMENTATIONKEY.

1. Als een webtaak implementeren.

## <a name="webjobs-sdk-3x"></a>WebJobs SDK 3.x

De belangrijkste wijziging geïntroduceerd door 3.x is het gebruik van .NET Core in plaats van .NET Framework. Voor het maken van een project 3.x zijn de instructies hetzelfde, met de volgende uitzonderingen:

1. Maak een console-app voor .NET Core. In de Visual Studio **nieuw Project** dialoogvenster Selecteer **.NET Core > Console-App (.NET Core)**. Het projectbestand geeft aan dat `TargetFramework` is `netcoreapp2.0`.

1. Kies de prerelease versie 3.x van de volgende pakketten:

  * `Microsoft.Azure.WebJobs.Extensions`
  * `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`

2. Wijziging `Main` code van de methode voor het ophalen van de verbindingsreeks voor opslag en de Application Insights-instrumentatiesleutel van een *appsettings.json* -bestand, met .NET Core configuration framework.  Hier volgt een voorbeeld:

   ```cs
   static void Main(string[] args)
   {
       var builder = new ConfigurationBuilder()
           .SetBasePath(Directory.GetCurrentDirectory())
           .AddJsonFile("appsettings.json");

       var appSettingsConfig = builder.Build();

       using (var loggerFactory = new LoggerFactory())
       {
           var config = new JobHostConfiguration();

           config.DashboardConnectionString = "";
           config.StorageConnectionString = 
               appSettingsConfig.GetConnectionString("AzureWebJobsStorage");
           var instrumentationKey =
               appSettingsConfig["APPINSIGHTS_INSTRUMENTATIONKEY"];

           config.LoggerFactory = loggerFactory
               .AddApplicationInsights(instrumentationKey, null)
               .AddConsole();

           config.UseTimers();
           config.UseDurableTask(new DurableTaskExtension
           {
               HubName = "MyTaskHub",
           });
           var host = new JobHost(config);
           host.RunAndBlock();
       }
   }
   ```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de WebJobs SDK, [het gebruik van de WebJobs SDK](../app-service/webjobs-sdk-how-to.md).


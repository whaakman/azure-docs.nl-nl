---
title: Het uitvoeren van duurzame functies als WebJobs - Azure
description: Leer hoe u code en duurzame functies in WebJobs uitvoeren door met de WebJobs SDK configureren.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 18c35070707408f43fd0e5dfc5e3330ef62a914d
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343153"
---
# <a name="how-to-run-durable-functions-as-webjobs"></a>Het uitvoeren van duurzame functies als WebJobs

[Azure Functions](../functions-overview.md) en de [duurzame functies](durable-functions-overview.md) extensie zijn gebouwd op de [WebJobs SDK](../../app-service/web-sites-create-web-jobs.md). De `JobHost` in de WebJobs SDK is de runtime in Azure Functions. Als u nodig om te bepalen hebt `JobHost` gedrag op een manier die niet mogelijk in Azure Functions, die u kunt ontwikkelen en duurzame functies uitvoeren met behulp van de WebJobs SDK. Vervolgens kunt u uw duurzame functies uitvoeren in een Azure WebJob of een willekeurige plaats een consoletoepassing wordt uitgevoerd.

De chaining duurzame functies voorbeeld is beschikbaar in een WebJobs SDK-versie: download of kloon de [duurzame functies opslagplaats](https://github.com/azure/azure-functions-durable-extension/) en navigeer naar de *voorbeelden\\webjobssdk\\-koppeling* map.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend bent met de basisprincipes van de WebJobs-SDK, C#-klasse ontwikkelen voor Azure Functions en duurzame functies. Als u een inleiding tot deze onderwerpen nodig hebt, raadpleegt u de volgende bronnen:

* [Aan de slag met de WebJobs SDK](../../app-service/webjobs-sdk-get-started.md)
* [Uw eerste functie maken met Visual Studio](../functions-create-your-first-function-visual-studio.md)
* [Duurzame functies](durable-functions-sequence.md)

Voltooi de stappen in dit artikel:

* [Installeer Visual Studio 2017 versie: 15,6 of hoger](https://docs.microsoft.com/visualstudio/install/) met de **Azure-ontwikkeling** werkbelasting.

  Als u al van Visual Studio hebt maar geen die werkbelasting, het toevoegen van de werkbelasting door te selecteren **Tools > hulpprogramma's en onderdelen**.

  (U kunt [Visual Studio Code](https://code.visualstudio.com/) in plaats daarvan, maar sommige van de instructies zijn specifiek voor Visual Studio.)

* Installeren en uitvoeren [Azure-Opslagemulator](../../storage/common/storage-use-emulator.md) versie 5.2 of hoger. Een alternatief is om bij te werken de *App.config* bestand met een Azure Storage-verbindingsreeks.

## <a name="webjobs-sdk-versions"></a>WebJobs-SDK-versies

In dit artikel wordt uitgelegd hoe u voor het ontwikkelen van een WebJobs SDK 2.x-project (gelijk aan Azure Functions-versie 1.x). Voor informatie over versie 3.x, Zie [WebJobs SDK 3.x](#webjobs-sdk-3x) verderop in dit artikel.

## <a name="create-console-app"></a>Console-app maken

Een WebJobs SDK-project is alleen een console-app-project met de juiste NuGet-pakketten zijn geïnstalleerd.

In de Visual Studio **nieuw Project** dialoogvenster, selecteer **klassieke bureaublad van Windows > consoletoepassing (.NET Framework)**. In het projectbestand de `TargetFrameworkVersion` moet `v4.6.1`.

Visual Studio heeft ook een WebJob-projectsjabloon, waarin u hiervoor kunt **Cloud > Azure-webtaak (.NET Framework)**. Deze sjabloon wordt veel pakketten, die waarvan sommige u niet hoeft mogelijk geïnstalleerd.

## <a name="install-nuget-packages"></a>NuGet-pakketten installeren

In dat geval moet u NuGet-pakketten in voor de WebJobs SDK, core bindingen, het framework voor logboekregistratie en de extensie duurzame taak. Hier vindt u **Package Manager Console** -opdrachten voor deze pakketten, met de nieuwste stabiele versie getallen vanaf de datum die in dit artikel is geschreven:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions -version 2.2.0
Install-Package Microsoft.Extensions.Logging -version 2.0.1
Install-Package Microsoft.Azure.WebJobs.Extensions.DurableTask -version 1.4.0
```

U moet ook logboekregistratie providers. De volgende opdrachten de Application Insights-provider installeren en de `ConfigurationManager`. De `ConfigurationManager` kunt u de Application Insights-instrumentatiesleutel ophalen uit de app-instellingen.

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
Install-Package System.Configuration.ConfigurationManager -version 4.4.1
```

De console-provider wordt geïnstalleerd in de volgende opdracht uit:

```powershell
Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
```

## <a name="jobhost-code"></a>JobHost code

Voor het gebruik van de extensie duurzame functies aanroepen `UseDurableTask` op de `JobHostConfiguration` van het object in uw `Main` methode:

```cs
var config = new JobHostConfiguration();
config.UseDurableTask(new DurableTaskExtension
{
    HubName = "MyTaskHub",
};
```

Voor een lijst met eigenschappen die u kunt instellen in de `DurableTaskExtension` object, Zie [host.json](../functions-host-json.md#durabletask).

De `Main` methode is ook de plaats voor het instellen van logboekregistratie providers. Het volgende voorbeeld wordt de console en providers van Application Insights.

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

Er zijn enkele verschillen in de code die u voor WebJobs SDK-functies ten opzichte van schrijft wat u schrijven voor de service Azure Functions.

De WebJobs SDK biedt geen ondersteuning voor de volgende functies van Azure Functions:

* [Functienaam kenmerk](#functionname-attribute)
* [HTTP-trigger](#http-trigger)
* [Duurzame functies HTTP-API voor beheer](#http-management-api)

### <a name="functionname-attribute"></a>Functienaam kenmerk

In een WebJobs SDK-project is de naam van de methode van de functie de naam van de functie. De `FunctionName` kenmerk wordt alleen gebruikt in Azure Functions.

### <a name="http-trigger"></a>HTTP-trigger

De WebJobs SDK beschikt niet over een HTTP-trigger. Het voorbeeldproject orchestration-client een timertrigger gebruikt:

```cs
public static async Task CronJob(
    [TimerTrigger("0 */2 * * * *")] TimerInfo timer,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger logger)
{
  ...
}
```

### <a name="http-management-api"></a>HTTP-beheer-API

Omdat er geen HTTP-trigger, de WebJobs SDK heeft geen [HTTP-beheer-API](durable-functions-http-api.md).

In het WebJobs-SDK-project, kunt u methoden aanroepen op het object van de orchestration-client in plaats van HTTP-aanvragen te verzenden. De volgende methoden komen overeen met de drie taken die u met de HTTP-API doen kunt:

* `GetStatusAsync`
* `RaiseEventAsync`
* `TerminateAsync`

De orchestration-client-functie in het voorbeeldproject de orchestrator-functie wordt gestart en wordt vervolgens in een lus die worden aangeroepen `GetStatusAsync` elke 2 seconden:

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

In deze sectie biedt een overzicht van het uitvoeren van de [voorbeeldproject](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/webjobssdk/chaining). Zie voor gedetailleerde instructies waarin wordt uitgelegd hoe u een WebJobs SDK-project lokaal uitvoeren en deze implementeren in een Azure WebJob [aan de slag met de WebJobs SDK](../../app-service/webjobs-sdk-get-started.md#deploy-as-a-webjob).

### <a name="run-locally"></a>Lokaal uitvoeren

1. Zorg ervoor dat de opslagemulator wordt uitgevoerd (Zie [vereisten](#prerequisites)).

1. Als u zien van Logboeken in Application Insights wilt wanneer u lokaal uitvoeren:

    a. Maak een Application Insights-resource, app-type **algemene**.

    b. Sla de instrumentatiesleutel in de *App.config* bestand.

1. Voer het project.

### <a name="run-in-azure"></a>Uitvoeren in Azure

1. Een web-app en een opslagaccount maken.

1. Sla de verbindingsreeks voor opslag in een app-instelling met de naam AzureWebJobsStorage in de web-app.

1. Maak een Application Insights-resource, app-type **algemene**.

1. Sla de instrumentatiesleutel in een app-instelling met de naam APPINSIGHTS_INSTRUMENTATIONKEY.

1. Als een WebJob implementeren.

## <a name="webjobs-sdk-3x"></a>WebJobs SDK 3.x

De belangrijkste wijziging geïntroduceerd door 3.x is het gebruik van .NET Core in plaats van .NET Framework. Voor het maken van een project 3.x zijn de instructies hetzelfde, met de volgende uitzonderingen:

1. Een .NET Core-consoletoepassing maken. In de Visual Studio **nieuw Project** dialoogvenster, selecteer **.NET Core > consoletoepassing (.NET Core)**. Het projectbestand geeft aan dat `TargetFramework` is `netcoreapp2.0`.

1. Kies de prerelease versie 3.x van de volgende pakketten:

    * `Microsoft.Azure.WebJobs.Extensions`
    * `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`

1. Wijziging `Main` methode code voor het ophalen van de verbindingsreeks voor opslag en de Application Insights-instrumentatiesleutel uit een *appsettings.json* -bestand, met behulp van de configuratie van .NET Core framework.  Hier volgt een voorbeeld:

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

Zie voor meer informatie over de WebJobs SDK, [over het gebruik van de WebJobs SDK](../../app-service/webjobs-sdk-how-to.md).
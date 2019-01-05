---
title: Het scheiden van telemetrie van ontwikkeling, testen en vrijgeven in Azure Application Insights | Microsoft Docs
description: Directe telemetrie voor verschillende bronnen voor ontwikkelen, testen en productie stempels.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: mbullwin
ms.openlocfilehash: 592efa5cc6a4918c03680e9785f85792eaa90099
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54045815"
---
# <a name="separating-telemetry-from-development-test-and-production"></a>Het scheiden van telemetrie van ontwikkeling, testen en productie

Wanneer u de volgende versie van een web-App ontwikkelt, u niet wilt combineren om de [Application Insights](../../application-insights/app-insights-overview.md) telemetrie van de nieuwe versie en de reeds uitgebrachte versie. Om verwarring te voorkomen, door de telemetrie van de ontwikkeling van de verschillende fasen voor het scheiden van Application Insights-resources, met afzonderlijke instrumentatiesleutels (ikeys) te verzenden. Als u wilt maken het gemakkelijker om te wijzigen van de instrumentatiesleutel als u een versie van één fase worden verplaatst naar een andere, kan het nuttig zijn om in te stellen de ikey in code in plaats van in het configuratiebestand. 

(Als uw systeem een Azure-Cloud-Service is, wordt er [een andere methode van het instellen van afzonderlijke ikeys](../../azure-monitor/app/cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>Over resources en instrumentatiesleutels

Bij het instellen van Application Insights-bewaking voor uw web-app, maakt u een Application Insights *resource* in Microsoft Azure. U opent deze resource in Azure portal om te bekijken en analyseren van de telemetrie die vanuit uw app zijn verzameld. De resource wordt geïdentificeerd door een *instrumentatiesleutel* (ikey). Wanneer u de Application Insights-pakket voor het bewaken van uw app installeert, configureren u deze met de instrumentatiesleutel, zodat deze weet waar om de telemetrie te verzenden.

Doorgaans wilt u afzonderlijke resources of één gedeelde resource gebruiken in verschillende scenario's:

* Andere, onafhankelijke toepassingen - gebruik een afzonderlijke resource en de ikey voor elke app.
* Meerdere onderdelen of functies van een zakelijke toepassing - gebruiken een [één gedeelde resource](../../azure-monitor/app/app-map.md) voor alle apps in de component. Telemetrie kan worden gefilterd of door de eigenschap cloud_RoleName gesegmenteerd.
* Ontwikkelings-, Test- en Release - gebruik van een afzonderlijke resource en de ikey voor versies van het systeem in 'stempel' of fase van de productie.
* EEN | B-tests - gebruik van één resource. Maak een TelemetryInitializer als u wilt een eigenschap toevoegen aan de telemetrie die de varianten identificeert.


## <a name="dynamic-ikey"></a> Dynamische instrumentatiesleutel

Als u wilt maken het gemakkelijker om te wijzigen van de ikey als de code worden verplaatst tussen fasen van de productie, instellen in code in plaats van in het configuratiebestand.

Stel de sleutel in een initialiseringsmethode, zoals global.aspx.cs in een ASP.NET-service:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

In dit voorbeeld worden de ikeys voor de andere resources in verschillende versies van het webconfiguratiebestand geplaatst. Wisselen van het webconfiguratiebestand - die u kunt dit doen als onderdeel van het script release -, wordt de doelresource wisselen.

### <a name="web-pages"></a>Webpagina's
De iKey ook in webpagina's van uw app wordt gebruikt in de [script die u hebt verkregen via de blade snel starten](../../azure-monitor/app/javascript.md). In plaats van coding te letterlijk in het script, kunt u het genereren van de status van de server. Bijvoorbeeld in een ASP.NET-app:

*JavaScript in de Razor*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Aanvullende Application Insights-resources maken
Afzonderlijke telemetrie voor verschillende toepassingsonderdelen, of voor verschillende stempels (ontwikkeling/tests/productie) van hetzelfde onderdeel, u klikt u vervolgens hebt een nieuwe Application Insights-resource maken.

In de [portal.azure.com](https://portal.azure.com), een Application Insights-resource toevoegen:

![Klik op Nieuw > Application Insights](./media/separate-resources/01-new.png)

* **Toepassingstype** is van invloed op wat u ziet op de overzichtsblade en de eigenschappen die beschikbaar zijn in [metric explorer](../../azure-monitor/app/metrics-explorer.md). Als u het type van de app niet ziet, kiest u een van de webtypen voor webpagina's.
* **Resourcegroep** is nuttig voor het beheren van eigenschappen zoals [toegangsbeheer](../../azure-monitor/app/resources-roles-access-control.md). U kunt afzonderlijke resourcegroepen gebruiken voor ontwikkeling, testen en productie.
* **Abonnement** is uw account betaling in Azure.
* **Locatie** is waar we uw gegevens bewaren. Op dit moment worden niet gewijzigd. 
* **Toevoegen aan dashboard** hebt u een snelle toegang tegel voor uw resource op de startpagina van Azure. 

Het maken van de resource duurt een paar seconden. Hier ziet u een waarschuwing wanneer dit voltooid.

(U kunt schrijven een [PowerShell-script](../../azure-monitor/app/powershell-script-create-resource.md) voor het automatisch maken van een resource.)

### <a name="getting-the-instrumentation-key"></a>De instrumentatiesleutel ophalen
De instrumentatiesleutel identificeert de bron die u hebt gemaakt. 

![Klik op Essentials, klik op de Instrumentatiesleutel, CTRL + C](./media/separate-resources/02-props.png)

U moet de instrumentatiesleutels van alle resources waarop uw app de gegevens worden verzonden.

## <a name="filter-on-build-number"></a>Filteren op build-nummer
Wanneer u een nieuwe versie van uw app publiceert, moet u mogelijk het scheiden van de telemetrie van verschillende builds.

U kunt de eigenschap toepassingsversie instellen, zodat u kunt filteren [zoeken](../../azure-monitor/app/diagnostic-search.md) en [metric explorer](../../azure-monitor/app/metrics-explorer.md) resultaten.

![Filteren op een eigenschap](./media/separate-resources/050-filter.png)

Er zijn verschillende methoden van het instellen van de toepassingsversie-eigenschap.

* Rechtstreeks worden ingesteld:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Teruglopen van die regel in een [telemetrische initializer](../../azure-monitor/app/api-custom-events-metrics.md#defaults) om ervoor te zorgen dat alle exemplaren van de TelemetryClient consistent zijn ingesteld.
* [ASP.NET] Instellen van de versie in `BuildInfo.config`. De versie van het knooppunt BuildLabel haalt de webmodule. Dit bestand opnemen in uw project en vergeet niet om in te stellen de eigenschap altijd kopiëren in Solution Explorer.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] BuildInfo.config automatisch genereren in MSBuild. U doet dit door een paar Voeg regels toe aan uw `.csproj` bestand:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Hiermee wordt een bestand met de naam gegenereerd *yourProjectName*. BuildInfo.config. Het publicatieproces te BuildInfo.config naam wijzigt.

    Het label van de build bevat een tijdelijke aanduiding (AutoGen_...) wanneer u met Visual Studio maken. Maar wanneer gebouwd met MSBuild, wordt dit ingevuld met de juiste versienummer.

    Als wilt toestaan MSBuild voor het genereren van versienummers, stelt u de versie zoals `1.0.*` in AssemblyReference.cs

## <a name="version-and-release-tracking"></a>Versie en release bijhouden
Als u de toepassingsversie wilt bijhouden, zorgt u ervoor dat `buildinfo.config` wordt gegenereerd door het Microsoft Build Engine-proces. Voeg het volgende toe in uw .csproj-bestand:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Wanneer de buildgegevens beschikbaar zijn, voegt de Application Insights-webmodule automatisch **Toepassingsversie** als eigenschap toe aan elk telemetrie-item. Dit biedt de mogelijkheid om op versie te filteren wanneer u [diagnostische zoekopdrachten](../../azure-monitor/app/diagnostic-search.md) uitvoert of [metrische gegevens verkent](../../azure-monitor/app/metrics-explorer.md).

Let er echter op dat het buildversienummer alleen wordt gegenereerd door Microsoft Build Engine en niet door de ontwikkelaarsbuild in Visual Studio.

### <a name="release-annotations"></a>Release-aantekeningen
Als u met Azure DevOps, kunt u [ophalen van de markering van een aantekening](../../azure-monitor/app/annotations.md) toegevoegd aan uw grafieken wanneer u een nieuwe versie. De volgende afbeelding toont hoe deze markering wordt weergegeven.

![Schermopname van aantekening bij voorbeeldrelease in een grafiek](media/separate-resources/release-annotation.png)
## <a name="next-steps"></a>Volgende stappen

* [Gedeelde bronnen voor meerdere functies](../../azure-monitor/app/app-map.md)
* [Een telemetrische Initializer om een onderscheid te maken | B-varianten](../../azure-monitor/app/api-filtering-sampling.md#add-properties)

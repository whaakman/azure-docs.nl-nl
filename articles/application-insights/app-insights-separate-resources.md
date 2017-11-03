---
title: Telemetrie scheiden van ontwikkeling, testen en release in Azure Application Insights | Microsoft Docs
description: Directe telemetrie naar andere bronnen voor ontwikkeling, testen en productie stempels.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: mbullwin
ms.openlocfilehash: 8d95958bce0597bfb16ef1c6b99b72ce9134e66f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a>Telemetrie scheiden van ontwikkeling, testen en productie

Wanneer u de volgende versie van een webtoepassing ontwikkelt, u niet wilt combineren van de [Application Insights](app-insights-overview.md) telemetrie van de nieuwe versie en de al uitgebrachte versie. Om verwarring te voorkomen, de telemetrie van andere ontwikkeling fasen voor het scheiden van Application Insights-resources met afzonderlijke instrumentatiesleutels (ikeys) te verzenden. Dit kan nuttig zijn om in te stellen van de sleutel in de code in plaats van in het configuratiebestand zijn de instrumentatiesleutel wijzigen, zoals een versie van één fase worden verplaatst naar een andere te vereenvoudigen. 

(Als u een Azure Cloud Service er [een andere methode voor het instellen van afzonderlijke ikeys](app-insights-cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>Over de resources en instrumentatiesleutels

Als u bewaking van de Application Insights voor uw web-app instelt, maakt u een Application Insights *resource* in Microsoft Azure. U kunt deze bron in de Azure portal om te kunnen weergeven en analyseren van het verzamelen van telemetriegegevens van uw app openen. De resource wordt geïdentificeerd door een *instrumentatiesleutel* (ikey). Wanneer u de Application Insights-pakket voor het bewaken van uw app installeert, configureren u deze met de instrumentatiesleutel zodat deze weet waar de telemetrie verzenden.

U gaat doorgaans afzonderlijke resources of één gedeelde resource gebruiken in verschillende scenario's:

* Andere, onafhankelijke toepassingen - gebruik een afzonderlijke resource en ikey voor elke app.
* Meerdere onderdelen of rollen zijn van een zakelijke toepassing - gebruiken een [één gedeelde bron](app-insights-monitor-multi-role-apps.md) voor alle apps van het onderdeel. Telemetrie worden gefilterd of door de eigenschap cloud_RoleName gesegmenteerde.
* Ontwikkeling, testen en Release - gebruik een afzonderlijke resource en ikey voor versies van het systeem in de tijdstempel' of fase van de productie.
* EEN | B-tests - gebruik van één resource. Maak een TelemetryInitializer om een eigenschap toevoegen aan de telemetrie die de varianten identificeert.


## <a name="dynamic-ikey"></a>Dynamische instrumentatiesleutel

De sleutel wijzigen als u de code verplaatst tussen stadia van de productie te vereenvoudigen deze instellen in de code in plaats van in het configuratiebestand.

Stel de sleutel in een initialiseringsmethode, zoals global.aspx.cs in een ASP.NET-beheerservice:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

In dit voorbeeld worden de ikeys voor de verschillende bronnen in verschillende versies van het webconfiguratiebestand geplaatst. Het webconfiguratiebestand - die u kunt doen als onderdeel van het script release - wisselen, wordt de doelresource wisselen.

### <a name="web-pages"></a>Webpagina's
De sleutel wordt ook gebruikt in uw app webpagina's in de [script die u hebt verkregen uit de blade snel starten](app-insights-javascript.md). In plaats van een bekend letterlijk in het script, moet u het genereren van de serverstatus. Bijvoorbeeld in een ASP.NET-app:

*JavaScript in Razor*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Aanvullende bronnen voor Application Insights maken
Afzonderlijke telemetrie voor andere toepassingsonderdelen, of voor verschillende stempels (ontwikkeling/tests/productie) van hetzelfde onderdeel, klikt u vervolgens hebt u een nieuwe Application Insights-resource maken.

In de [portal.azure.com](https://portal.azure.com), een Application Insights-resource toevoegen:

![Klik op Nieuw > Application Insights](./media/app-insights-separate-resources/01-new.png)

* **Toepassingstype** is van invloed op wat er op de overzichtsblade en de eigenschappen die beschikbaar zijn in [metrische explorer](app-insights-metrics-explorer.md). Als u uw app-type niet ziet, kiest u een van de webtypen voor webpagina's.
* **Resourcegroep** is nuttig voor het beheren van eigenschappen zoals [toegangsbeheer](app-insights-resources-roles-access-control.md). U kunt afzonderlijke resourcegroepen voor ontwikkeling, testen en productie.
* **Abonnement** is uw betaling-account in Azure.
* **Locatie** is waar we uw gegevens wilt bewaren. Op dit moment worden niet gewijzigd. 
* **Toevoegen aan dashboard** plaatst u een snelle toegang tegel voor uw resource op de startpagina van Azure. 

Maken van de resource duurt een paar seconden. U ziet een waarschuwing wanneer deze wordt uitgevoerd.

(U kunt schrijven een [PowerShell-script](app-insights-powershell-script-create-resource.md) automatisch maken van een resource.)

### <a name="getting-the-instrumentation-key"></a>De instrumentatiesleutel ophalen
De instrumentatiesleutel identificeert de resource die u hebt gemaakt. 

![Klik op Essentials, klik op de Instrumentatiesleutel, CTRL + C](./media/app-insights-separate-resources/02-props.png)

U moet de instrumentatiesleutels van alle resources waarop uw app gegevens worden verzonden.

## <a name="filter-on-build-number"></a>Filteren op build-nummer
Wanneer u een nieuwe versie van uw app publiceert, moet u mogelijk de telemetrie scheiden van verschillende builds.

U kunt de eigenschap toepassingsversie zo instellen dat u kunt filteren [search](app-insights-diagnostic-search.md) en [metrische explorer](app-insights-metrics-explorer.md) resultaten.

![Filteren op een eigenschap](./media/app-insights-separate-resources/050-filter.png)

Er zijn verschillende methoden gebruiken voor het instellen van de eigenschap Version van toepassing.

* Rechtstreeks worden ingesteld:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Inpakken van die regel in een [telemetrie initialisatiefunctie](app-insights-api-custom-events-metrics.md#defaults) om ervoor te zorgen dat alle exemplaren van TelemetryClient consistent zijn ingesteld.
* [ASP.NET] Stel de versie `BuildInfo.config`. De versie van het knooppunt BuildLabel de webmodule opgehaald. Dit bestand opnemen in uw project en vergeet niet de eigenschap kopie altijd in Solution Explorer instellen.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] BuildInfo.config automatisch genereren in MSBuild. Om dit te doen, Voeg een paar regels voor uw `.csproj` bestand:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Hiermee wordt een bestand met de naam gegenereerd *yourProjectName*. BuildInfo.config. Het publicatieproces voor BuildInfo.config naam wijzigt.

    Het build-label bevat een tijdelijke aanduiding (AutoGen_...) wanneer u bij het maken van Visual Studio. Maar wanneer gebouwd met MSBuild, wordt dit ingevuld met de juiste versienummer.

    Stel de versie zoals wilt toestaan MSBuild genereren versienummers `1.0.*` in AssemblyReference.cs

## <a name="version-and-release-tracking"></a>Versie en release bijhouden
Als u de toepassingsversie wilt bijhouden, zorgt u ervoor dat `buildinfo.config` wordt gegenereerd door het Microsoft Build Engine-proces. Voeg het volgende toe in uw .csproj-bestand:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Wanneer de buildgegevens beschikbaar zijn, voegt de Application Insights-webmodule automatisch **Toepassingsversie** als eigenschap toe aan elk telemetrie-item. Dit biedt de mogelijkheid om op versie te filteren wanneer u [diagnostische zoekopdrachten](app-insights-diagnostic-search.md) uitvoert of [metrische gegevens verkent](app-insights-metrics-explorer.md).

Let er echter op dat het buildversienummer alleen wordt gegenereerd door Microsoft Build Engine en niet door de ontwikkelaarsbuild in Visual Studio.

### <a name="release-annotations"></a>Release-aantekeningen
Als u Visual Studio Team Services gebruikt, kunt u [een aantekeningenmarkering ophalen](app-insights-annotations.md) die aan uw grafieken wordt toegevoegd wanneer u een nieuwe versie uitgeeft. De volgende afbeelding toont hoe deze markering wordt weergegeven.

![Schermopname van aantekening bij voorbeeldrelease in een grafiek](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a>Volgende stappen

* [Gedeelde bronnen voor meerdere functies](app-insights-monitor-multi-role-apps.md)
* [Een initialisatiefunctie telemetrie als u een onderscheid wilt maken | Varianten B](app-insights-api-filtering-sampling.md#add-properties)

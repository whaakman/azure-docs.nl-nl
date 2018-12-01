---
title: Application Insights voor Java-web-apps die al live
description: De bewaking van een webtoepassing die al wordt uitgevoerd op uw server starten
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 12f3dbb9-915f-4087-87c9-807286030b0b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/10/2016
ms.author: mbullwin
ms.openlocfilehash: 8e8a2e19e97bc07ed481adb3ecc3ae1d34ea8368
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52720997"
---
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a>Application Insights voor Java-web-apps die al live

Als u een webtoepassing die al wordt uitgevoerd op uw J2EE-server hebt, kun u bewaking met [Application Insights](app-insights-overview.md) zonder codewijzigingen of compileer uw project. Met deze optie krijgt u informatie over HTTP-aanvragen verzonden naar uw server, niet-verwerkte uitzonderingen en prestatiemeteritems.

U hebt een abonnement op [Microsoft Azure](https://azure.com) nodig.

> [!NOTE]
> De procedure op deze pagina wordt de SDK toegevoegd aan uw web-app tijdens runtime. Deze instrumentatie runtime is handig als u niet wilt bijwerken of opnieuw opbouwen van uw broncode. Maar als u kunt, raden we u [de SDK toevoegen aan de broncode](app-insights-java-get-started.md) in plaats daarvan. Dat biedt u meer opties, zoals code schrijven voor het bijhouden van gebruikersactiviteit.
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Een Application Insights-instrumentatiesleutel ophalen
1. Aanmelden bij de [Microsoft Azure portal](https://portal.azure.com)
2. Maak een nieuwe Application Insights-resource en het type ingesteld op Java-webtoepassing.
   
    ![Een naam invoeren, Java-web-app kiezen en op Maken klikken](./media/app-insights-java-live/02-create.png)

    De resource is gemaakt in een paar seconden.

4. Open de nieuwe resource en de instrumentatiesleutel ophalen. U moet deze sleutel zo dadelijk in de code van uw project plakken.
   
    ![Op Eigenschappen klikken in het overzicht van de nieuwe resource en de instrumentatiesleutel kopiëren](./media/app-insights-java-live/03-key.png)

## <a name="2-download-the-sdk"></a>2. De SDK downloaden
1. Download de [Application Insights-SDK voor Java](https://aka.ms/aijavasdk). 
2. Pak de SDK-inhoud naar de map van waaruit de binaire bestanden van uw project worden geladen op uw server. Als u Tomcat, zou deze map meestal worden onder `webapps/<your_app_name>/WEB-INF/lib`

Houd er rekening mee dat u moet dit herhalen op elke server-exemplaar en voor elke app.

## <a name="3-add-an-application-insights-xml-file"></a>3. Een Application Insights-xml-bestand toevoegen
Maak ApplicationInsights.xml in de map waarin u de SDK hebt toegevoegd. Het volgende XML-bestand in het plaatsen.

Vervang de instrumentatiesleutel die u in de Azure Portal hebt verkregen.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

* De instrumentatiesleutel wordt samen met alle telemetrie-items verzonden en instrueert Application Insights om deze in de resource weer te geven.
* Het onderdeel voor de HTTP-aanvraag is optioneel. Het verzendt automatisch telemetrie over aanvragen en reactietijden naar de portal.
* Correlatie tussen gebeurtenissen is een aanvulling op het onderdeel voor de HTTP-aanvraag. Deze aanvulling wijst een id toe aan elke aanvraag die door de server wordt ontvangen en voegt deze id als de eigenschap 'Operation.Id' toe aan elk telemetrie-item. Hiermee kunt u correlaties zichtbaar maken tussen de telemetrie die is gekoppeld aan elke aanvraag door het instellen van een filter in [diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md).

## <a name="4-add-an-http-filter"></a>4. Een HTTP-filter toevoegen
Open het web.xml-bestand in uw project en samenvoegen van het volgende codefragment onder het knooppunt voor web-app, waar de toepassingsfilters zijn geconfigureerd.

Voor de nauwkeurigste resultaten moet het filter vóór alle andere filters worden toegewezen.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

## <a name="5-check-firewall-exceptions"></a>5. Controleer de firewall-uitzonderingen
U moet mogelijk [-uitzonderingen voor het verzenden van uitgaande gegevens instellen](app-insights-ip-addresses.md).

## <a name="6-restart-your-web-app"></a>6. Opnieuw opstarten van uw web-app
## <a name="7-view-your-telemetry-in-application-insights"></a>7. Uw telemetrie in Application Insights weergeven
Ga terug naar uw Application Insights-resource in de [Microsoft Azure Portal](https://portal.azure.com).

Telemetrie over HTTP-aanvragen wordt weergegeven op de overzichtsblade. (Als dit niet het geval is, wacht u een paar seconden en klikt u vervolgens op Vernieuwen.)

![voorbeeldgegevens](./media/app-insights-java-live/5-results.png)

Klik in een grafiek voor gedetailleerdere metrische gegevens. 

![](./media/app-insights-java-live/6-barchart.png)

En wanneer u de eigenschappen van een aanvraag bekijkt, ziet u de telemetriegebeurtenissen die zijn gekoppeld aan deze zoals aanvragen en uitzonderingen.

![](./media/app-insights-java-live/7-instance.png)

[Meer informatie over metrische gegevens.](app-insights-metrics-explorer.md)

## <a name="next-steps"></a>Volgende stappen
* [Voeg telemetrie toe aan uw webpagina's](app-insights-javascript.md) bewaken van paginaweergaven en metrische gegevens over gebruikers.
* [Stel webtests](app-insights-monitor-web-app-availability.md) om te controleren of live en responsief blijft van uw toepassing.
* [Logboektraceringen vastleggen](app-insights-java-trace-logs.md)
* [Doorzoek gebeurtenissen en logboeken](app-insights-diagnostic-search.md) om problemen vast te stellen.
* [Een Spring Boot-initialisatie-app configureren](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights)

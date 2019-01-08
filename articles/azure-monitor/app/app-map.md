---
title: Overzicht van de toepassing in Azure Application Insights | Microsoft Docs
description: Topologieën voor complexe toepassingen met het overzicht van de toepassing bewaken
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/17/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 238b75681ec525187ea27f60ac8b21b05b13954d
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063111"
---
# <a name="application-map-triage-distributed-applications"></a>Overzicht van de toepassing: Sorteren van gedistribueerde toepassingen

Overzicht van de toepassing kunt u spot knelpunten of fout hotspots voor alle onderdelen van de gedistribueerde toepassing. Elk knooppunt op de kaart vertegenwoordigt een toepassingsonderdeel of de bijbehorende afhankelijkheden; en health KPI heeft en de status van waarschuwingen. U kunt de via elk onderdeel klikken voor meer gedetailleerde diagnostische gegevens, zoals Application Insights-gebeurtenissen. Als uw app gebruikmaakt van Azure-services, kunt u ook verder klikken naar Azure diagnostics, zoals SQL Database Advisor-aanbevelingen.

## <a name="what-is-a-component"></a>Wat is een onderdeel?

Onderdelen zijn onafhankelijk implementeerbare onderdelen van uw toepassing gedistribueerd/microservices. Ontwikkelaars en uitvoerende teams hebben zichtbaarheid van de code op serverniveau of toegang tot telemetrie die is gegenereerd door de onderdelen van deze toepassing. 

* Onderdelen zijn anders dan 'waargenomen' externe afhankelijkheden, zoals SQL, EventHub enz. die uw team/organisatie mogelijk geen toegang tot (code of telemetrie).
* Onderdelen worden uitgevoerd op een willekeurig aantal exemplaren van de rol-server-container.
* Onderdelen kunnen worden afzonderlijke Application Insights-instrumentatiesleutels (zelfs als abonnementen verschillen) of verschillende rollen die rapporteren aan een enkele Application Insights-instrumentatiesleutel. De ervaring van de kaart voorbeeld ziet u de onderdelen onafhankelijk van de manier waarop ze zijn ingesteld.

## <a name="composite-application-map"></a>Samengestelde toepassingstoewijzing

U ziet de volledige toepassing-topologie voor meerdere niveaus van gerelateerde toepassingsonderdelen. Onderdelen mogelijk op verschillende Application Insights-resources of verschillende rollen in een enkele resource. Het toepassingsoverzicht vindt onderdelen door de volgende HTTP-afhankelijkheidsaanroepen tussen servers met de Application Insights-SDK geïnstalleerd. 

Deze ervaring wordt gestart met progressieve detectie van de onderdelen. Wanneer u eerst het toepassingsoverzicht laadt, wordt een set van query's worden geactiveerd voor het detecteren van de onderdelen die betrekking hebben op dit onderdeel. Een knop in de linkerbovenhoek wordt bijgewerkt met het aantal onderdelen in uw toepassing zodra ze worden gedetecteerd. 

Op 'Toewijzingsonderdelen bijwerken' klikt, wordt de kaart met alle onderdelen die zijn gedetecteerd tot vernieuwd. Dit kan een minuut laden duren afhankelijk van de complexiteit van uw toepassing.

Deze detectiestap is niet vereist als alle onderdelen van rollen binnen één Application Insights-resource. Het initiële laden voor dergelijke toepassing hebben alle onderdelen daarvan.

![Schermafbeelding van de toepassing-kaart](media/app-map/001.png)

Een van de belangrijkste doelstellingen met deze ervaring is het kunnen complexe topologieën met honderden onderdelen te visualiseren.

Klik op een onderdeel om te zien van verwante inzichten en gaat u naar de prestaties en de fout sorteren ervaring voor het betreffende onderdeel.

![Flyout](media/app-map/application-map-001.png)

### <a name="investigate-failures"></a>Mislukte pogingen onderzoeken

Selecteer **mislukte pogingen onderzoeken** om te starten van het deelvenster met fouten.

![Schermafbeelding van de knop fouten onderzoeken](media/app-map/investigate-failures.png)

![Schermafbeelding van de ervaring van fouten](media/app-map/failures.png)

### <a name="investigate-performance"></a>Prestaties onderzoeken

Selecteer voor het oplossen van problemen met prestaties, **prestaties onderzoeken**.

![Schermafdruk van knop prestaties onderzoeken](media/app-map/investigate-performance.png)

![Schermafbeelding van prestaties](media/app-map/performance.png)

### <a name="go-to-details"></a>Naar de details gaan

Selecteer **Ga naar details** verkennen van de end-to-end-transactie-ervaring is gereed om terug te het niveau van de call stack weergaven kan bieden.

![Schermafbeelding van de knop Ga voor meer informatie](media/app-map/go-to-details.png)

![Schermafbeelding van end-to-end-transactiedetails](media/app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>Weergeven in Analytics

Als u wilt opvragen en de gegevens van uw toepassingen verder te onderzoeken, klikt u op **weergeven in analytics**.

![Schermafbeelding van de weergave in de knop analytics](media/app-map/view-in-analytics.png)

![Schermafbeelding van de analytics-ervaring](media/app-map/analytics.png)

### <a name="alerts"></a>Waarschuwingen

Als u wilt weergeven van actieve waarschuwingen en de onderliggende regels die ervoor zorgen dat de waarschuwingen worden geactiveerd, selecteer **waarschuwingen**.

![Schermafbeelding van de knop waarschuwingen](media/app-map/alerts.png)

![Schermafbeelding van de analytics-ervaring](media/app-map/alerts-view.png)

## <a name="set-cloudrolename"></a>Set cloud_RoleName

Overzicht van de toepassing maakt gebruik van de `cloud_RoleName` eigenschap om de onderdelen op de kaart te identificeren. De Application Insights-SDK wordt automatisch toegevoegd de `cloud_RoleName` eigenschap in op de telemetrie die is verzonden door onderdelen. Bijvoorbeeld, de SDK wordt toegevoegd een naam van website of servicenaam die rol moet de `cloud_RoleName` eigenschap. Er zijn echter gevallen waar kunt u de standaardwaarde overschrijven. Cloud_RoleName overschrijven en wat wordt weergegeven op de kaart van de toepassing wijzigen:

### <a name="net"></a>.NET

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                //set custom role name here
                telemetry.Context.Cloud.RoleName = "RoleName";
            }
        }
    }
}
```

**De initialisatiefunctie laden**

In ApplicationInsights.config:

```xml
    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="CustomInitializer.Telemetry.MyTelemetryInitializer, CustomInitializer"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>
```

Een alternatieve methode is het instantiëren van de initialisatiefunctie in code, bijvoorbeeld in Global.aspx.cs:

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
    }
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();
appInsights.defaultClient.context.tags["ai.cloud.role"] = "your role name";
appInsights.defaultClient.context.tags["ai.cloud.roleInstance"] = "your role instance";
```

### <a name="alternate-method-for-nodejs"></a>Alternatieve methode voor Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();

appInsights.defaultClient.addTelemetryProcessor(envelope => {
    envelope.tags["ai.cloud.role"] = "your role name";
    envelope.tags["ai.cloud.roleInstance"] = "your role instance"
});
```

### <a name="java"></a>Java

Als u Spring Boot aan de Application Insights Spring Boot starter gebruiken, wordt de enige vereiste wijziging is het instellen van uw aangepaste naam voor de toepassing in de application.properties-bestand.

`spring.application.name=<name-of-app>`

De Spring Boot starter wordt cloudRoleName automatisch toegewezen aan de opgegeven waarde voor de eigenschap spring.application.name.

Voor meer informatie over Java correlatie- en cloudRoleName configureren voor niet-SpringBoot toepassingen afhandeling dit [sectie](https://docs.microsoft.com/azure/application-insights/application-insights-correlation#role-name) op correlatie.

### <a name="clientbrowser-side-javascript"></a>Browser-client-side-JavaScript

```javascript
appInsights.queue.push(() => {
appInsights.context.addTelemetryInitializer((envelope) => {
  envelope.tags["ai.cloud.role"] = "your role name";
  envelope.tags["ai.cloud.roleInstance"] = "your role instance";
});
});
```

Zie voor meer informatie over het onderdrukken van de eigenschap cloud_RoleName met telemetrie initializers, [eigenschappen toevoegen: ITelemetryInitializer](api-filtering-sampling.md#add-properties-itelemetryinitializer).

## <a name="troubleshooting"></a>Problemen oplossen

Als u problemen bij het ophalen van Toepassingsoverzicht ondervindt naar behoren werkt, probeert u deze stappen:

1. Zorg ervoor dat u een officieel ondersteunde SDK. Niet-ondersteunde/community SDK's bieden mogelijk geen ondersteuning voor correlatie.

    Verwijzen naar dit [artikel](https://docs.microsoft.com/azure/application-insights/app-insights-platforms) voor een lijst met ondersteunde SDK's.

2. Alle onderdelen van een upgrade uitvoert naar de nieuwste versie van de SDK.

3. Als u Azure Functions met C#, een upgrade uitvoeren naar [functies V2](https://docs.microsoft.com/azure/azure-functions/functions-versions).

4. Controleer of [cloud_RoleName](app-map.md#Set-cloud-RoleName) correct is geconfigureerd.

5. Als u een afhankelijkheid ontbreekt, zorgt ervoor dat deze in de lijst met [automatisch verzamelde afhankelijkheden](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies). Als niet zo is, kunt u nog steeds volgen deze handmatig aan een [afhankelijkheidsaanroep bijhouden](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency).

## <a name="portal-feedback"></a>Portal feedback
Als u feedback wilt, gebruikt u de feedbackoptie.

![Installatiekopie van het MapLink-1](./media/app-map/14-updated.png)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over correlatie](https://docs.microsoft.com/azure/application-insights/application-insights-correlation)

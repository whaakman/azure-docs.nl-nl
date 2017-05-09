---
title: Node.js-services bewaken met Azure Application Insights | Microsoft Docs
description: Prestaties bewaken en problemen detecteren in Node.js-services met Application Insights.
services: application-insights
documentationcenter: nodejs
author: joshgav
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/01/2017
ms.author: joshgav
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 76a8025cd2a67533beb321c88e924517c1977dfc
ms.contentlocale: nl-nl
ms.lasthandoff: 05/02/2017


---

# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a>Node.js-services en -apps bewaken met Application Insights

[Azure Application Insights](app-insights-overview.md) bewaakt uw back-endservices en onderdelen nadat u deze hebt geïmplementeerd om u te helpen [prestatieproblemen en andere problemen snel te detecteren en te onderzoeken](app-insights-detect-triage-diagnose.md). Gebruik het voor Node.js-services die overal worden gehost: uw datacenter, Azure VM's en web-apps en zelfs andere openbare clouds.

Om uw bewakingsgegevens te ontvangen, op te slaan en te onderzoeken, volgt u de volgende instructies om een agent op te nemen in uw code en een bijbehorende Application Insights-resource in Azure in te stellen. De agent verzendt gegevens naar die resource voor verdere analyse en onderzoek.

De Node.js-agent kan binnenkomende en uitgaande HTTP-aanvragen, verschillende systeemmeetgegevens en uitzonderingen automatisch bewaken. Vanaf v0.20 kan het ook sommige algemene pakketten van derden bewaken, zoals `mongodb`, `mysql` en `redis`. Alle gebeurtenissen met betrekking tot een binnenkomende HTTP-aanvraag worden gecorreleerd voor snellere probleemoplossing.

U kunt meer aspecten van uw app en het systeem bewaken door deze handmatig te instrumenteren met behulp van de agent-API die later wordt beschreven.

![Voorbeeld van grafieken met prestatiebewaking](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a>Aan de slag

We gaan het instellen van de bewaking voor een app of service stapsgewijs doornemen.

### <a name="resource"></a> Een App Insights-resource instellen

**Voordat u begint**, moet u ervoor zorgen dat u een Azure-abonnement hebt of moet u [een gratis nieuw abonnement aanvragen][azure-free-offer]. Als uw organisatie al een Azure-abonnement heeft, kan een beheerder [deze instructies][add-aad-user] volgen om u eraan toe te voegen.

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

Meld u nu aan bij [Azure Portal] [ portal] en maak een Application Insights-resource, zoals hieronder wordt geïllustreerd: klik op 'Nieuw' > 'Hulpprogramma's voor ontwikkelaars' > 'Application Insights'. De resource bevat een eindpunt voor het ontvangen van telemetriegegevens, opslag voor deze gegevens, opgeslagen rapporten en dashboards, regel- en waarschuwingsconfiguratie en meer.

![Een App Insights-resource maken](./media/app-insights-nodejs/03-new_appinsights_resource.png)

Kies op de pagina voor het maken van de resource 'Node.js-toepassing' in de vervolgkeuzelijst met toepassingssoorten. Het soort app bepaalt de standaardset van dashboards en rapporten die voor u worden gemaakt. Maakt u zich echter geen zorgen, want elke App Insights-resource kan gegevens verzamelen vanuit elke taal en elk platform.

![Nieuw App Insights-resourceformulier](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <a name="agent"></a> De Node.js-agent instellen

Het is nu tijd om de agent op te nemen in uw app zodat deze gegevens kan verzamelen.
Kopieer eerst de instrumentatiesleutel (hierna uw `ikey` genoemd) van uw resource vanuit de portal, zoals hieronder weergegeven. Het App Insights-systeem gebruikt deze sleutel om uw gegevens te koppelen aan uw Azure-resource, dus moet u deze opgeven in een omgevingsvariabele of de code die door de agent moet worden gebruikt.  

![Instrumentatiesleutel kopiëren](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

Voeg vervolgens de Node.js-agentbibliotheek toe aan de afhankelijkheden van uw app via package.json. Voer in de hoofdmap van uw app de volgende opdracht uit:

```bash
npm install applicationinsights --save
```

Nu moet u de bibliotheek expliciet in uw code laden. Omdat de agent instrumentatie in veel andere bibliotheken injecteert, moet u deze zo vroeg mogelijk laden, zelfs vóór andere `require`-instructies. Voeg eerst boven aan uw eerste .js-bestand het volgende toe:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

De `setup`-methode configureert de instrumentatiesleutel (en dus Azure-resource) die standaard moet worden gebruikt voor alle bijgehouden items. Roep `start` aan nadat de configuratie is voltooid om te beginnen met het verzamelen en verzenden van telemetriegegevens.

U kunt ook een ikey opgeven via de omgevingsvariabele APPINSIGHTS \_INSTRUMENTATIONKEY in plaats van deze handmatig door te geven naar `setup()` of `getClient()`. Met deze procedure kunt u ikeys buiten vastgelegde broncode houden en verschillende ikeys opgeven voor verschillende omgevingen.

Extra configuratieopties worden hieronder gedocumenteerd.

U kunt de agent uitproberen zonder telemetrie te verzenden. Stel de instrumentatiesleutel hiertoe in op een niet-lege tekenreeks.

### <a name="monitor"></a> Uw app bewaken

De agent verzamelt automatisch telemetrische informatie over de Node.js-runtime en een aantal algemene modules van derden. Gebruik nu uw toepassing om een aantal van deze gegevens te genereren.

Blader vervolgens in de [Azure Portal][portal] naar de Application Insights-resource die u eerder hebt gemaakt en zoek naar de eerste paar gegevenspunten in de Overzichtstijdlijn, zoals op de volgende afbeelding. Klik op de grafieken voor meer informatie.

![Eerste gegevenspunten](./media/app-insights-nodejs/12-first-perf.png)

Klik op de toepassingskaart om de gedetecteerde topologie voor uw app te bekijken, zoals op de volgende afbeelding wordt weergegeven. Klik op onderdelen op de kaart voor meer informatie.

![Eenvoudig app-kaart](./media/app-insights-nodejs/06-appinsights_appmap.png)

Meer informatie over uw app en het oplossen van problemen met behulp van de andere beschikbare weergaven vindt u in de sectie 'Onderzoeken'.

![Sectie onderzoeken](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a>Zijn er geen gegevens?

Omdat de agent gegevens in batches plaatst voor verzending, kan er een vertraging optreden voordat items worden weergegeven in de portal. Als u geen gegevens in uw resource ziet kunt u de volgende oplossingen proberen:

* De toepassing nog een paar keer gebruiken; voer meer acties uit om meer telemetrie te genereren.
* Klik op **Vernieuwen** in de resourceweergave van de portal. Grafieken worden automatisch periodiek vernieuwd, maar als u handmatig vernieuwt, vindt dit onmiddellijk plaats.
* Controleer of [benodigde uitgaande poorten](app-insights-ip-addresses.md) open zijn.
* Open de tegel [Zoeken](app-insights-diagnostic-search.md) en zoek naar afzonderlijke gebeurtenissen.
* Raadpleeg de [Veelgestelde vragen][].


## <a name="agent-configuration"></a>Agentconfiguratie

Hieronder vindt u de agentconfiguratiemethoden en hun standaardwaarden.

Om gebeurtenissen in een service volledig te correleren, moet u `.setAutoDependencyCorrelation(true)` instellen. Hierdoor kan de agent context tussen asynchrone callbacks in Node.js traceren.

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoDependencyCorrelation(false)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .start();
```

## <a name="agent-api"></a>Agent-API

<!-- TODO: Fully document agent API. -->

De .NET-agent-API wordt [hier](app-insights-api-custom-events-metrics.md) volledig beschreven.

U kunt elke aanvraag, gebeurtenis, metriek of uitzondering traceren met behulp van de Node.js-client van Application Insights. Het volgende voorbeeld toont enkele van de beschikbare API's.

```javascript
let appInsights = require("applicationinsights");
appInsights.setup().start(); // assuming ikey in env var
let client = appInsights.getClient();

client.trackEvent("my custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");

let http = require("http");
http.createServer( (req, res) => {
  client.trackRequest(req, res); // Place at the beginning of your request handler
});
```

### <a name="track-your-dependencies"></a>Afhankelijkheden bijhouden

```javascript
let appInsights = require("applicationinsights");
let client = appInsights.getClient();

var success = false;
let startTime = Date.now();
// execute dependency call here....
let duration = Date.now() - startTime;
success = true;

client.trackDependency("dependency name", "command name", duration, success);
```

### <a name="add-a-custom-property-to-all-events"></a>Een aangepaste eigenschap toevoegen aan alle gebeurtenissen

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a>HTTP GET-aanvragen traceren

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a>Serveropstarttijd traceren

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a>Meer bronnen

* [Uw telemetrie in de portal bewaken](app-insights-dashboards.md)
* [Analysequery’s schrijven over uw telemetrie](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[Veelgestelde vragen]: app-insights-troubleshoot-faq.md


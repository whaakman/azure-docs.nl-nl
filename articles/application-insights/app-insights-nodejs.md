---
title: Uw Node.js-app bewaken met de Azure Application Insights-SDK | Microsoft Docs
description: Analyseer het gebruik, de beschikbaarheid en de prestaties van uw on-premises webtoepassing of Microsoft Azure-webtoepassing met Application Insights.
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/23/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 46b829ce52994a5112494145a02e78859c5fae2d
ms.openlocfilehash: d4c7fa2058b1c07671329304c37630d2e6e8e8a7
ms.lasthandoff: 02/23/2017


---
# <a name="add-application-insights-sdk-to-monitor-your-nodejs-app"></a>De Application Insights-SDK toevoegen om uw Node.js-app te bewaken


Met [Azure Application Insights](app-insights-overview.md) wordt uw live-toepassing bewaakt om u te helpen bij het [detecteren en onderzoeken van prestatieproblemen en -uitzonderingen](app-insights-detect-triage-diagnose.md) en om [na te gaan hoe uw app wordt gebruikt](app-insights-overview-usage.md). De tool werkt voor apps die worden gehost op uw eigen on-premises IIS-servers, op virtuele Azure-machines en in Azure-webtoepassingen.

Met de SDK kunt u automatisch de snelheid en reacties van binnenkomende HTTP-aanvragen verzamelen, net als de prestatiemeteritems (CPU, geheugen, RPS) en onverwerkte uitzonderingen. Bovendien kunt u aangepaste aanroepen toevoegen om de afhankelijkheden, meetgegevens en andere gebeurtenissen bij te houden.

![Voorbeeld van grafieken met prestatiebewaking](./media/app-insights-nodejs/10-perf.png)

#### <a name="before-you-start"></a>Voordat u begint
U hebt de volgende zaken nodig:

* Een abonnement op [Microsoft Azure](http://azure.com). Als uw team of organisatie een Azure-abonnement heeft, kan de eigenaar u toevoegen met behulp van uw [Microsoft-account](http://live.com).

## <a name="a-nameaddacreate-an-application-insights-resource"></a><a name="add"></a> Een Application Insights-resource maken
Meld u aan bij [Azure Portal][portal] en maak een nieuwe Application Insights-resource. Een [resource][roles] in Azure is een exemplaar van een service. In deze resource wordt de telemetrie van uw app geanalyseerd en aan u gepresenteerd.

![Klik op Nieuw > Application Insights](./media/app-insights-nodejs/01-new-asp.png)

Kies Algemeen als het toepassingstype. Op basis van het gekozen toepassingstype wordt de standaardinhoud van de resourceblades bepaald, net als de eigenschappen die zichtbaar zijn in [Metrics Explorer][metrics].

#### <a name="copy-the-instrumentation-key"></a>De instrumentatiesleutel kopiëren
De sleutel geeft aan wat de resource is. U installeert de sleutel in het begin in de SDK om gegevens om te leiden naar de resource.

![Op Eigenschappen klikken, de sleutel selecteren en op Ctrl + C drukken](./media/app-insights-nodejs/02-props-asp.png)

## <a name="a-namesdka-install-the-sdk-in-your-application"></a><a name="sdk"></a> De SDK installeren in uw toepassing
```
npm install applicationinsights --save
```

## <a name="usage"></a>Gebruik
Hiermee schakelt u de bewaking van aanvragen, het bijhouden van onverwerkte uitzonderingen en de systeemprestatiebewaking (CPU/geheugen/RPS) in.

```javascript

var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>").start();
```

De instrumentatiesleutel kan ook worden ingesteld in de omgevingsvariabele APPINSIGHTS_INSTRUMENTATIONKEY. Als u dit doet, is er geen argument vereist bij het aanroepen van `appInsights.setup()` of `appInsights.getClient()`.

U kunt de SDK uitproberen zonder telemetrie te verzenden. Stel de instrumentatiesleutel hiertoe in op een niet-lege tekenreeks.

## <a name="a-nameruna-run-your-project"></a><a name="run"></a> Uw project uitvoeren
Start uw toepassing en probeer deze uit. Open verschillende pagina’s om telemetrie te genereren.

## <a name="a-namemonitora-view-your-telemetry"></a><a name="monitor"></a> Uw telemetrie weergeven
Ga naar de [Azure Portal](https://portal.azure.com) en blader naar uw Application Insights-resource.

Zoek naar gegevens op de pagina Overzicht. Aanvankelijk ziet u slechts één of twee punten. Bijvoorbeeld:

![Klik verder voor meer gegevens](./media/app-insights-nodejs/12-first-perf.png)

Klik in een grafiek voor gedetailleerdere metrische gegevens. [Meer informatie over metrische gegevens.][perf]

#### <a name="no-data"></a>Zijn er geen gegevens?
* Gebruik de toepassing om verschillende pagina's te openen, zodat er telemetrie wordt gegenereerd.
* Open de tegel [Zoeken](app-insights-diagnostic-search.md) om afzonderlijke gebeurtenissen te bekijken. Soms kan het even duren voordat de metrische gegevens van gebeurtenissen zijn opgehaald.
* Wacht een paar seconden en klik op **Vernieuwen**. De grafieken worden regelmatig vernieuwd, maar u kunt ze ook handmatig vernieuwen als u op bepaalde gegevens wacht.
* Zie [Probleemoplossing][qna].

## <a name="publish-your-app"></a>Uw app publiceren
Implementeer nu uw toepassing naar IIS of naar Azure en bekijk hoe de gegevens worden verzameld.

#### <a name="no-data-after-you-publish-to-your-server"></a>Ziet u geen gegevens nadat u uw app naar uw server hebt gepubliceerd?
Open deze poorten voor uitgaand verkeer in de firewall van uw server:

* `dc.services.visualstudio.com:443`
* `f5.services.visualstudio.com:443`

#### <a name="trouble-on-your-build-server"></a>Problemen op uw buildserver?
Raadpleeg dit artikel voor [Probleemoplossing](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

## <a name="customized-usage"></a>Aangepast gebruik
### <a name="disabling-auto-collection"></a>Automatisch verzamelen uitschakelen
```javascript
import appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false)
    .setAutoCollectPerformance(false)
    .setAutoCollectExceptions(false)
    // no telemetry will be sent until .start() is called
    .start();
```

### <a name="custom-monitoring"></a>Aangepaste bewaking
```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

client.trackEvent("custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");
```

[Meer informatie over de telemetrie-API](app-insights-api-custom-events-metrics.md).

### <a name="using-multiple-instrumentation-keys"></a>Meerdere instrumentatiesleutels gebruiken
```javascript
import appInsights = require("applicationinsights");

// configure auto-collection with one instrumentation key
appInsights.setup("<instrumentation_key>").start();

// get a client for another instrumentation key
var otherClient = appInsights.getClient("<other_instrumentation_key>");
otherClient.trackEvent("custom event");
```

## <a name="examples"></a>Voorbeelden
### <a name="tracking-dependency"></a>Afhankelijkheid bijhouden
```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

var startTime = Date.now();
// execute dependency call
var endTime = Date.now();

var elapsedTime = endTime - startTime;
var success = true;
client.trackDependency("dependency name", "command name", elapsedTime, success);
```



### <a name="manual-request-tracking-of-all-get-requests"></a>Handmatig alle GET-aanvragen bijhouden
```javascript
var http = require("http");
var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false) // disable auto-collection of requests for this example
    .start();

// assign common properties to all telemetry sent from the default client
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};

// track a system startup event
appInsights.client.trackEvent("server start");

// create server
var port = process.env.port || 1337
var server = http.createServer(function (req, res) {
    // track all "GET" requests
    if(req.method === "GET") {
        appInsights.client.trackRequest(req, res);
    }

    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello World\n");
}).listen(port);

// track startup time of the server as a custom metric
var start = +new Date;
server.on("listening", () => {
    var end = +new Date;
    var duration = end - start;
    appInsights.client.trackMetric("StartupTime", duration);
});
```

## <a name="next-steps"></a>Volgende stappen
* [Uw telemetrie in de portal bewaken](app-insights-dashboards.md)
* [Analysequery’s schrijven over uw telemetrie](app-insights-analytics-tour.md)

<!--Link references-->

[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[perf]: app-insights-web-monitor-performance.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md


---
title: Node.js-services bewaken met Azure Application Insights | Microsoft Docs
description: Prestaties bewaken en problemen detecteren in Node.js-services met Application Insights.
services: application-insights
documentationcenter: nodejs
author: mrbullwinkle
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/01/2017
ms.author: mbullwin
ms.openlocfilehash: 8f7a2344b6676a9067cf0adff04f49a73ce457fc
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a>Node.js-services en -apps bewaken met Application Insights

Met [Azure Application Insights](app-insights-overview.md) bewaakt u back-endservices en onderdelen na de implementatie. Dit helpt u om [prestatieproblemen en andere problemen snel te detecteren en te onderzoeken](app-insights-detect-triage-diagnose.md). U kunt Application Insights voor Node.js-services die worden gehost in uw datacenter, gebruiken in virtuele Azure-machines en web-apps en zelfs in andere openbare clouds.

Neem de SDK op in de code en stel vervolgens een bijbehorende Application Insights-resource in Azure in om uw bewakingsgegevens te ontvangen, op te slaan en te onderzoeken. Met de SDK worden gegevens naar deze resource verzonden voor verdere analyse en onderzoek.

De Node.js SDK kan binnenkomende en uitgaande HTTP-aanvragen, uitzonderingen en verschillende systeemmeetgegevens automatisch bewaken. Vanaf versie 0.20 kunnen met de SDK ook sommige algemene pakketten van derden worden bewaakt, zoals MongoDB, MySQL, bewaken en Redis. Alle gebeurtenissen met betrekking tot een binnenkomende HTTP-aanvraag worden gecorreleerd voor snellere probleemoplossing.

U kunt de TelemetryClient-API gebruiken om handmatig aanvullende aspecten van de app en het systeem te instrumenteren en te bewaken. De TelemetryClient-API wordt verderop in dit artikel meer gedetailleerd beschreven.

![Voorbeeld van grafieken met prestatiebewaking](./media/app-insights-nodejs/10-perf.png)

## <a name="get-started"></a>Aan de slag

Voltooi de volgende taken om de bewaking voor een app of service in te stellen.

### <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u ervoor zorgen dat u een Azure-abonnement hebt of moet u [een gratis nieuw abonnement aanvragen][azure-free-offer]. Als uw organisatie al een Azure-abonnement heeft, kan een beheerder [deze instructies][add-aad-user] volgen om u eraan toe te voegen.

[azure-free-offer]: https://azure.microsoft.com/free/
[add-aad-user]: https://docs.microsoft.com/azure/active-directory/active-directory-users-create-azure-portal


### <a name="resource"></a> Een Application Insights-resource instellen


1. Meld u aan bij [Azure Portal][portal].
2. Selecteer **Nieuw** > **Ontwikkelhulpprogramma's** > **Application Insights**. De resource bevat een eindpunt voor het ontvangen van telemetriegegevens, opslag voor deze gegevens, opgeslagen rapporten en dashboards, regel- en waarschuwingsconfiguratie en meer.

  ![Een Application Insights-resource maken](./media/app-insights-nodejs/03-new_appinsights_resource.png)

3. Selecteer op de pagina voor het maken van de resource in het vak **Toepassingstype** de optie **Node.js-toepassing**. Het apptype bepaalt de standaardashboards en -rapporten die worden gemaakt. (elke Application Insights-resource kan gegevens verzamelen vanuit elke taal en elk platform.)

  ![Nieuw Application Insights-resourceformulier](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <a name="sdk"></a> De Node.ja SDK instellen

Neem de SKD op in de app zodat gegevens kunnen worden verzameld. 

1. Kopieer de instrumentatiesleutel van de resource (ook wel *iKey* genoemd) vanuit Azure Portal. Application Insights gebruikt de iKey om de gegevens toe te wijzen aan de Azure-resource. Voordat de SDK uw iKey kan gebruiken, moet u de iKey opgeven in een omgevingsvariabele of in uw code.  

  ![Instrumentatiesleutel kopiëren](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

2. Voeg de Node.js SDK-bibliotheek toe aan de afhankelijkheden van de app via package.json. Voer in de hoofdmap van uw app de volgende opdracht uit:

  ```bash
  npm install applicationinsights --save
  ```

3. Laad de bibliotheek expliciet in de code. Laad de bibliotheek zo vroeg mogelijk, zelfs vóór andere `require`-instructies, omdat de SDK instrumentatie in veel andere bibliotheken injecteert. 

  Voeg boven aan het eerste .js-bestand de volgende code toe. De `setup`-methode configureert de iKey (en dus de Azure-resource) die standaard moet worden gebruikt voor alle bijgehouden items.

  ```javascript
  const appInsights = require("applicationinsights");
  appInsights.setup("<instrumentation_key>");
  appInsights.start();
  ```
   
  U kunt ook een iKey opgeven via de omgevingsvariabele APPINSIGHTS \_INSTRUMENTATIONKEY in plaats van deze handmatig door te geven naar `setup()` of `new appInsights.TelemetryClient()`. Met deze procedure kunt u iKeys buiten vastgelegde broncode houden en kunt u verschillende iKeys opgeven voor verschillende omgevingen.

  Zie de volgende secties voor extra configuratieopties.

  U kunt de SDK uitproberen zonder telemetrie te verzenden door `appInsights.defaultClient.config.disableAppInsights = true` in te stellen.

### <a name="monitor"></a> Uw app bewaken

Met de SDK wordt automatisch telemetrische informatie verzameld over de Node.js-runtime en over een aantal algemene modules van derden. Gebruik de toepassing om een aantal van deze gegevens te genereren.

Ga vervolgen in [Azure Portal][portal] naar de Application Insights-resource die u hebt gemaakt. Zoek in de **Overzichtstijdlijn** naar de eerste gegevenspunten. Selecteer verschillende onderdelen in de grafieken om meer gedetailleerde gegevens te zien.

![Eerste gegevenspunten](./media/app-insights-nodejs/12-first-perf.png)

Selecteer de knop **Toepassingskaart** om de gedetecteerde topologie voor te app weer te geven. Selecteer onderdelen in de kaart voor meer informatie.

![Eenvoudig app-kaart](./media/app-insights-nodejs/06-appinsights_appmap.png)

Selecteer in de sectie **ONDERZOEKEN** de andere beschikbare weergaven voor informatie over de app en het oplossen van problemen.

![Sectie onderzoeken](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a>Zijn er geen gegevens?

Er kan vertraging optreden voordat items worden weergegeven in de portal omdat met de SDK gegevens in batches worden geplaatst voor verzending. Als u geen gegevens ziet in de resource, kunt u de volgende oplossingen proberen:

* Ga door met het gebruiken van de toepassing. Neem meer acties om meer telemetrie te genereren.
* Klik op **Vernieuwen** in de resourceweergave van de portal. Grafieken worden periodiek automatisch gegeneerd, maar als u ze handmatig vernieuwt, worden ze onmiddellijk vernieuwd.
* Controleer of de [vereiste uitgaande poorten](app-insights-ip-addresses.md) open zijn.
* Gebruik [Zoeken](app-insights-diagnostic-search.md) om naar specifieke gebeurtenissen te zoeken.
* Raadpleeg de [Veelgestelde vragen][FAQ].


## <a name="sdk-configuration"></a>SDK-configuratie

De configuratiemethoden en standaardwaarden van de SDK worden vermeld in het volgende codevoorbeeld.

Om gebeurtenissen in een service volledig te correleren, moet u `.setAutoDependencyCorrelation(true)` instellen. Als deze optie is ingesteld, kan met de SDK de context tussen asynchrone callbacks in Node.js worden getraceerd.

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoDependencyCorrelation(true)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .setAutoCollectConsole(true)
    .setUseDiskRetryCaching(true)
    .start();
```

## <a name="telemetryclient-api"></a>TelemetryClient-API

Zie [Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens](app-insights-api-custom-events-metrics.md) voor een volledige beschrijving van de TelemetryClient-API.

U kunt elke aanvraag, gebeurtenis, metriek of uitzondering traceren met behulp van de Node.js SDK van Application Insights. Het volgende codevoorbeeld toont enkele API's die u kunt gebruiken:

```javascript
let appInsights = require("applicationinsights");
appInsights.setup().start(); // assuming ikey is in env var
let client = appInsights.defaultClient;

client.trackEvent({name: "my custom event", properties: {customProperty: "custom property value"}});
client.trackException({exception: new Error("handled exceptions can be logged with this method")});
client.trackMetric({name: "custom metric", value: 3});
client.trackTrace({message: "trace message"});
client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL"});
client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true});

let http = require("http");
http.createServer( (req, res) => {
  client.trackNodeHttpRequest({request: req, response: res}); // Place at the beginning of your request handler
});
```

### <a name="track-your-dependencies"></a>Afhankelijkheden bijhouden

Gebruik de volgende code om de afhankelijkheden bij te houden:

```javascript
let appInsights = require("applicationinsights");
let client = appInsights.defaultClient;

var success = false;
let startTime = Date.now();
// Execute dependency call here...
let duration = Date.now() - startTime;
success = true;

client.trackDependency({dependencyTypeName: "dependency name", name: "command name", duration: duration, success: success});
```

### <a name="add-a-custom-property-to-all-events"></a>Een aangepaste eigenschap toevoegen aan alle gebeurtenissen

Gebruik de volgende code om een aangepaste eigenschap toe te voegen aan alle gebeurtenissen:

```javascript
appInsights.defaultClient.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a>HTTP GET-aanvragen traceren

Gebruik de volgende code om HTTP GET-aanvragen bij te houden:

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.defaultClient.trackNodeHttpRequest({request: req, response: res});
    }
    // Other work here...
    res.end();
});
```

### <a name="track-server-startup-time"></a>Serveropstarttijd traceren

Gebruik de volgende code om serveropstarttijd bij te houden:

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.defaultClient.trackMetric({name: "server startup time", value: duration});
});
```

## <a name="next-steps"></a>Volgende stappen

* [Uw telemetrie in de portal bewaken](app-insights-dashboards.md)
* [Analysequery’s schrijven over uw telemetrie](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[FAQ]: app-insights-troubleshoot-faq.md


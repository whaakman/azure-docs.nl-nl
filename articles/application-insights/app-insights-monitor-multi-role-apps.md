---
title: Azure Application Insights-ondersteuning voor meerdere onderdelen, microservices en containers | Microsoft Docs
description: Bewaken van apps die bestaan uit meerdere onderdelen of functies voor prestaties en het gebruik.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: mbullwin
ms.openlocfilehash: dc776dcfe6fa2e03225649d856ae161c9f23c737
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52727267"
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a>Bewaken van toepassingen met meerdere onderdelen met Application Insights (preview)

U kunt de apps die bestaan uit meerdere server-onderdelen, functies of services met bewaken [Azure Application Insights](app-insights-overview.md). De status van de onderdelen en de relaties tussen deze worden weergegeven op één Toepassingsoverzicht. U kunt afzonderlijke bewerkingen via meerdere onderdelen met automatische HTTP correlatie traceren. Diagnostische gegevens over containers kunnen worden geïntegreerd en verband houden met de toepassingstelemetrie. Gebruik één Application Insights-resource voor alle onderdelen van uw toepassing. 

![Overzicht van de toepassing met meerdere onderdelen](./media/app-insights-monitor-multi-role-apps/application-map-001.png)

We gebruiken 'onderdeel' hier om een werkende deel van een grote toepassing. Bijvoorbeeld, een typische zakelijke toepassing kan bestaan uit de clientcode die wordt uitgevoerd in webbrowsers, communicatie met een of meer web-app services, die op zijn beurt back gebruiken-endservices. Server-onderdelen mogelijk on-premises gehost op in de cloud, of Azure-web- en werkrollen rollen kunnen worden, of kunnen uitvoeren in, zoals Docker of Service Fabric-containers. 

### <a name="sharing-a-single-application-insights-resource"></a>Een enkele Application Insights-resource delen 

De sleutel techniek hier is het verzenden van telemetrie van elk onderdeel in uw toepassing naar dezelfde Application Insights-resource, maar gebruikt u de `cloud_RoleName` eigenschap te onderscheiden van onderdelen indien nodig. De Application Insights-SDK wordt toegevoegd de `cloud_RoleName` eigenschap om de onderdelen van de telemetrie te verzenden. Bijvoorbeeld, de SDK wordt toegevoegd een naam van website of servicenaam die rol moet de `cloud_RoleName` eigenschap. U kunt deze waarde met een telemetryinitializer overschrijven. Het overzicht van de toepassing maakt gebruik van de `cloud_RoleName` eigenschap om de onderdelen op de kaart te identificeren.

Voor meer informatie over hoe overschrijf de `cloud_RoleName` eigenschap Zie [eigenschappen toevoegen: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).  

In sommige gevallen kan dit niet altijd geschikt en u mogelijk liever gebruik van afzonderlijke resources voor verschillende groepen van onderdelen. Bijvoorbeeld, moet u mogelijk verschillende bronnen worden gebruikt voor beheer of facturering.

Met deze voorbehoud veronderstellen we in de rest van dit document dat u wilt verzenden van gegevens uit meerdere onderdelen naar een Application Insights-resource.

## <a name="configure-multi-component-applications"></a>Toepassingen met meerdere onderdelen configureren

Als u een overzicht van de toepassing met meerdere onderdelen, moet u om deze doelstellingen te realiseren:

* **Installeer de meest recente voorlopige versie** Application Insights-pakket in elk onderdeel van de toepassing. 
* **Een enkele Application Insights-resource delen** voor alle onderdelen van uw toepassing.
* **Samengestelde toepassingstoewijzing inschakelen** in de blade Preview-versies.

Configureer elk onderdeel van uw toepassing met behulp van de juiste methode voor het betreffende type. ([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)

### <a name="1-install-the-latest-pre-release-package"></a>1. Het meest recente voorlopige versie-pakket installeren

Bijwerken of installeren van de Application Insights-pakketten in het project voor elk serveronderdeel. Als u Visual Studio:

1. Met de rechtermuisknop op een project en selecteer **NuGet-pakketten beheren**. 
2. Selecteer **Include prerelease**.
3. Als Application Insights-pakketten in de Updates worden weergegeven, selecteert u deze. 

    Anders, zoeken en installeren van het juiste pakket:
    
    * Microsoft.ApplicationInsights.WindowsServer
    * Microsoft.ApplicationInsights.ServiceFabric - voor onderdelen die worden uitgevoerd als uitvoerbare gastbestanden en Docker-containers in Service Fabric-toepassing uitgevoerd
    * Microsoft.ApplicationInsights.ServiceFabric.Native - voor betrouwbare services in service fabric-toepassingen
    * Microsoft.ApplicationInsights.Kubernetes voor onderdelen die in Docker worden uitgevoerd in Kubernetes

### <a name="2-share-a-single-application-insights-resource"></a>2. Een enkele Application Insights-resource delen

* In Visual Studio met de rechtermuisknop op een project en selecteer **Application Insights configureren**, of **Application Insights > configureren**. Gebruik de wizard voor het eerste project om een Application Insights-resource te maken. Voor volgende projecten, selecteert u dezelfde resource.
* Als er geen Application Insights-menu, handmatig configureren:

   1. In [Azure-portal](https://portal,azure.com), opent u de Application Insights-resource die u al hebt gemaakt voor een ander onderdeel.
   2. In de blade overzicht, open de vervolgkeuzelijst Essentials tabblad en kopieer de **Instrumentatiesleutel.**
   3. Open ApplicationInsights.config en invoegen in uw project: `<InstrumentationKey>your copied key</InstrumentationKey>`

![De instrumentatiesleutel kopiëren naar het .config-bestand](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-composite-application-map"></a>3. Samengestelde toepassingstoewijzing inschakelen

Open de resource voor uw toepassing in Azure portal. Onder de subkop configureren, klikt u op Preview-versies om de blade Preview-versies te openen. Schakel in de blade Preview-versies *samengestelde toepassingstoewijzing*.

### <a name="4-enable-docker-metrics-optional"></a>4. Docker metrische gegevens (optioneel) inschakelen 

Als een onderdeel in een Docker die worden gehost op een Windows VM in Azure wordt uitgevoerd, kunt u aanvullende metrische gegevens verzamelen uit de container. Voeg dit in uw [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) configuratiebestand:

```
"DiagnosticMonitorConfiguration": {
        ...
        "sinks": "applicationInsights",
        "DockerSources": {
                "Stats": {
                    "enabled": true,
                    "sampleRate": "PT1M"
                }
            },
        ...
    }
    ...   
    "SinksConfig": {
        "Sink": [{
            "name": "applicationInsights",
            "ApplicationInsights": "<your instrumentation key here>"
        }]
    }
    ...
}

```

## <a name="use-cloudrolename-to-separate-components"></a>Cloud_RoleName gebruiken voor het scheiden van onderdelen

De `cloud_RoleName` eigenschap is gekoppeld aan alle telemetrie. Verwijst naar de component - functie of service - dat afkomstig is van de telemetrie. (Dit is niet gelijk zijn aan cloud_RoleInstance identieke functies die parallel worden uitgevoerd op meerdere serverprocessen of machines onderscheid.)

U kunt filteren of segmenteren van uw telemetrie met behulp van deze eigenschap in de portal. In dit voorbeeld wordt de blade fouten gefilterd zodat alleen op de gegevens van de front-end webservice, gefilterd op fouten van de back-end van de CRM-API:

![Grafiek met metrische gegevens gesegmenteerd op Cloudrolnaam](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a>Transacties tussen onderdelen traceren

U kunt traceren van het ene naar de andere, de aanroepen tijdens het verwerken van een afzonderlijke bewerking.


![Telemetrie voor bewerking weergeven](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

Klik door naar een lijst met gecorreleerde telemetrie voor deze bewerking via de front-end-webserver en de back-end-API:

![Doorzoek onderdelen](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a>Volgende stappen

* [Afzonderlijke telemetrie van ontwikkeling, testen en productie](app-insights-separate-resources.md)

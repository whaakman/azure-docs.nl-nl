---
title: Azure Application Insights-ondersteuning voor meerdere onderdelen, microservices en containers | Microsoft Docs
description: Bewaken van apps die bestaan uit meerdere onderdelen of rollen voor prestaties en het gebruik.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: mbullwin
ms.openlocfilehash: 046661bf7903b4e5ea528282ad5170901a45b35c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a>De bewaking van meerdere onderdeel toepassingen met Application Insights (preview)

U kunt apps die bestaan uit meerdere server-onderdelen, functies of services met bewaken [Azure Application Insights](app-insights-overview.md). De status van de onderdelen en hun onderlinge relaties worden weergegeven op een kaart met één toepassing. U kunt afzonderlijke bewerkingen via meerdere onderdelen met automatische HTTP correlatie traceren. Container diagnostische gegevens kan worden geïntegreerd en worden gecorreleerd met de toepassingstelemetrie. Gebruik één Application Insights-resource voor de onderdelen van uw toepassing. 

![Meerdere onderdeel toepassingstoewijzing](./media/app-insights-monitor-multi-role-apps/app-map.png)

We gebruiken 'onderdeel' hier betekent een werkende deel van een grote aanvraag. Bijvoorbeeld: de meeste zakelijke toepassingen kan bestaan uit clientcode die wordt uitgevoerd in de webbrowser, praten met een of meer web-app services, die op zijn beurt back gebruiken-end services. Server-onderdelen in de cloud gehoste on-premises kunnen worden op Azure web- en werkrollen rollen kunnen worden of mogelijk worden uitgevoerd in containers zoals Docker of Service Fabric. 

### <a name="sharing-a-single-application-insights-resource"></a>Een enkele Application Insights-resource delen 

De belangrijke techniek hier is te verzenden van telemetrie van elk onderdeel in uw toepassing met de dezelfde Application Insights-bron, maar de `cloud_RoleName` eigenschap te onderscheiden van onderdelen indien nodig. Voegt de Application Insights-SDK de `cloud_RoleName` eigenschap in op de onderdelen telemetrie verzenden. Bijvoorbeeld: de SDK wordt toegevoegd een naam van de website of servicenaam rol is toegewezen aan de `cloud_RoleName` eigenschap. U kunt deze waarde met een telemetryinitializer overschrijven. De Map van de toepassing gebruikt de `cloud_RoleName` eigenschap om de onderdelen op de kaart te identificeren.

Voor meer informatie over het overschrijven de `cloud_RoleName` eigenschap Zie [eigenschappen toevoegen: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).  

In sommige gevallen kan dit niet altijd geschikt en u mogelijk liever Gebruik afzonderlijke bronnen voor verschillende groepen van onderdelen. U wilt bijvoorbeeld verschillende bronnen worden gebruikt voor beheer of facturering doeleinden. Met behulp van afzonderlijke resources betekent dat er geen alle onderdelen weergegeven op een kaart met één toepassing; en dat u geen query uitvoeren voor onderdelen in [Analytics](app-insights-analytics.md). U moet ook de afzonderlijke resources instellen.

Met dat hoewel we gaan ervan uit dat in de rest van dit document die u wilt gegevens uit meerdere onderdelen te verzenden naar een Application Insights-resource.

## <a name="configure-multi-component-applications"></a>Meerdere onderdeel toepassingen configureren

Als u een toepassing met meerdere onderdeel-kaart, moet u deze doelstellingen te bereiken:

* **Installeer de meest recente voorlopige versie** Application Insights-pakket in elk onderdeel van de toepassing. 
* **Een enkele Application Insights-resource delen** voor alle onderdelen van uw toepassing.
* **Inschakelen van de toepassing van meerdere rollenoverzicht** in de blade Previews.

Configureer elk onderdeel van uw toepassing met de juiste methode voor het betreffende type. ([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)

### <a name="1-install-the-latest-pre-release-package"></a>1. Het installatiepakket voor de meest recente voorlopige versie

Bijwerken of installeer de Application Insights-pakketten in het project voor elk serveronderdeel. Als u Visual Studio:

1. Met de rechtermuisknop op een project en selecteer **NuGet-pakketten beheren**. 
2. Selecteer **Include prerelease**.
3. Als de Application Insights-pakketten in de Updates worden weergegeven, selecteert u deze. 

    Anders zoeken en installeren van het juiste pakket:
    
    * Microsoft.ApplicationInsights.WindowsServer
    * Microsoft.ApplicationInsights.ServiceFabric - voor de onderdelen die worden uitgevoerd als gast uitvoerbare bestanden en Docker-containers in Service Fabric-toepassing wordt uitgevoerd
    * Microsoft.ApplicationInsights.ServiceFabric.Native - voor betrouwbare services in ServiceFabric toepassingen
    * Microsoft.ApplicationInsights.Kubernetes voor onderdelen die worden uitgevoerd in Docker op Kubernetes

### <a name="2-share-a-single-application-insights-resource"></a>2. Een enkele Application Insights-resource delen

* In Visual Studio met de rechtermuisknop op een project en selecteer **Configure Application Insights**, of **Application Insights > configureren**. Gebruik de wizard voor het maken van een Application Insights-resource voor de eerste project. Voor toekomstige projecten, selecteert u dezelfde resource.
* Als er geen Application Insights-menu, handmatig configureren:

   1. In [Azure-portal](https://portal,azure.com), opent u de Application Insights-resource die u al hebt gemaakt voor een ander onderdeel.
   2. In de blade overzicht, open de vervolgkeuzelijst Essentials tabblad en kopieer de **Instrumentatiesleutel.**
   3. Open ApplicationInsights.config en invoegen in uw project:`<InstrumentationKey>your copied key</InstrumentationKey>`

![De instrumentatiesleutel kopiëren naar het .config-bestand](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a>3. Toepassing van meerdere rollenoverzicht inschakelen

Open de resource voor uw toepassing in de Azure-portal. In de blade Previews inschakelen *toepassing van meerdere rollenoverzicht*.

### <a name="4-enable-docker-metrics-optional"></a>4. Docker metrische gegevens (optioneel) inschakelen 

Als een onderdeel in een Docker gehost op een Windows Azure VM wordt uitgevoerd, kunt u aanvullende gegevens verzamelen van de container. Voeg dit in uw [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) configuratiebestand:

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

De `cloud_RoleName` eigenschap is gekoppeld aan alle telemetrie. Het onderdeel dat afkomstig is van de telemetrie - functie of service - verwijst. (Dit is niet hetzelfde zijn als cloud_RoleInstance die identiek functies die parallel worden uitgevoerd op meerdere serverprocessen of machines scheidt.)

U kunt filteren of segmenteren van uw telemetrie met deze eigenschap in de portal. In dit voorbeeld wordt de blade fouten gefilterd zodat alleen op de gegevens van de front-webservice, worden uitgefilterd fouten van de CRM-API-back-end:

![Metrische gesegmenteerd op naam van de Cloud-rol-grafiek](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a>Trace-bewerkingen tussen onderdelen

U kunt traceren van het ene naar de andere, de aanroepen tijdens het verwerken van een afzonderlijke bewerking.


![Telemetrie voor bewerking weergeven](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

Klik op via een lijst gecorreleerde telemetrie voor deze bewerking op de front-endwebserver en de back-end-API:

![Zoeken in onderdelen](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a>Volgende stappen

* [Afzonderlijke telemetrie van ontwikkeling, testen en productie](app-insights-separate-resources.md)

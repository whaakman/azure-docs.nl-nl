---
title: Docker-toepassingen in Azure Application Insights bewaken | Microsoft Docs
description: Docker-prestatiemeteritems, gebeurtenissen en uitzonderingen kunnen worden weergegeven op de Application Insights, samen met de telemetrie van de apps in containers.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 27a3083d-d67f-4a07-8f3c-4edb65a0a685
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: mbullwin
ms.openlocfilehash: 4df6780fa61c1ed32279d882f383097dc0287716
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275912"
---
# <a name="monitor-docker-applications-in-application-insights"></a>Docker-toepassingen bewaken in Application Insights

Levenscyclus van gebeurtenissen en prestaties van tellers van [Docker](https://www.docker.com/) containers kunnen worden uitgezet op Application Insights. Installeer de [Application Insights](https://hub.docker.com/r/microsoft/applicationinsights/) installatiekopie in een container in de host en prestatiemeteritems worden weergegeven voor de host, evenals voor de andere afbeeldingen.

Met Docker, moet u uw apps in lichtgewicht containers, inclusief alle afhankelijkheden distribueren. Ze moeten uitvoeren op een hostmachine die wordt uitgevoerd een Docker-Engine.

Bij het uitvoeren van de [Application Insights-installatiekopie](https://hub.docker.com/r/microsoft/applicationinsights/) op uw Docker-host, krijgt deze voordelen:

* Levenscyclus van telemetrie over alle containers die worden uitgevoerd op de host - starten, stoppen, enzovoort.
* Prestatiemeteritems voor alle containers. CPU, geheugen, netwerkgebruik en meer.
* Als u [geïnstalleerd van Application Insights-SDK voor Java](app-insights-java-live.md) in de apps die worden uitgevoerd in de containers, de telemetrie van deze apps hebben extra eigenschappen voor het identificeren van de container en host-machine. Als u exemplaren van een app die wordt uitgevoerd in meer dan één host hebt, kunt u bijvoorbeeld eenvoudig de telemetrie van uw app door de host filteren.

> [!NOTE]
> Deze oplossing is afgeschaft. Voor meer informatie over onze huidige investeringen in de containerbewaking aangeraden uitchecken [Azure Monitor voor containers](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview).

## <a name="set-up-your-application-insights-resource"></a>Uw Application Insights-resource instellen

1. Meld u aan bij [Microsoft Azure portal](https://azure.com) en open de Application Insights-resource voor uw app; of [Maak een nieuwe](app-insights-create-new-resource.md). 
   
    *Welke resource moet ik gebruiken?* Als de apps die u op de host uitvoert zijn ontwikkeld door iemand anders, dan u moet [Maak een nieuwe Application Insights-resource](app-insights-create-new-resource.md). Dit is waar u weergeven en analyseren van de telemetrie. (Selecteer 'Algemeen' voor het app-type.)
   
    Maar als u de ontwikkelaar van de apps bent en we hopen u dat [Application Insights-SDK toegevoegd](app-insights-java-live.md) aan elk van deze. Als ze alle echt onderdelen van een enkele business-toepassing, wordt u al deze telemetrie verzenden naar één resource configureren en kunt u die dezelfde resource om de Docker-levenscyclus en prestaties van gegevens weer te geven mogelijk. 
   
    Een derde scenario is dat u de meeste van de apps ontwikkeld, maar u afzonderlijke resources worden gebruikt om de telemetrie weer te geven. In dat geval kunt u waarschijnlijk ook wilt maken van een afzonderlijke resource voor de Docker-gegevens.

2. Klik op de **Essentials** vervolgkeuzelijst en kopieer de Instrumentatiesleutel. U gebruikt dit aan dat de SDK waar om de telemetrie te verzenden.

Houd dat browservenster handig als u terug naar deze snel om te kijken naar uw telemetrie.

## <a name="run-the-application-insights-monitor-on-your-host"></a>De Application Insights-monitor op uw host worden uitgevoerd

Nu dat u hebt een locatie om de telemetrie weer te geven, kunt u de app die u verzamelt en verzendt deze instellen.

1. Verbinding maken met uw Docker-host.
2. Bewerk de instrumentatiesleutel in deze opdracht en vervolgens uit te voeren:
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

Slechts één afbeelding van Application Insights is vereist volgens de Docker-host. Als uw toepassing wordt geïmplementeerd op meerdere Docker-hosts, herhaalt u de opdracht op elke host.

## <a name="update-your-app"></a>Uw app bijwerken
Als uw toepassing is uitgerust met de [Application Insights-SDK voor Java](app-insights-java-get-started.md), voeg de volgende regel in het ApplicationInsights.xml-bestand in uw project, onder de `<TelemetryInitializers>` element:

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

Hiermee voegt u Docker-informatie, zoals container en host-id toe aan elk telemetrie-item dat is verzonden vanuit uw app.

## <a name="view-your-telemetry"></a>Uw telemetrie weergeven
Ga terug naar uw Application Insights-resource in Azure portal.

Klik op de Docker-tegel.

U ziet kort gegevens binnenkomen vanaf de Docker-app, met name als u andere containers die worden uitgevoerd op uw Docker-engine.

### <a name="docker-container-events"></a>Gebeurtenissen van docker-container
![Voorbeeld](./media/app-insights-docker/13.png)

Voor het onderzoeken van afzonderlijke gebeurtenissen, klikt u op [zoeken](app-insights-diagnostic-search.md). Zoeken en filteren om de gewenste gebeurtenissen te zoeken. Klik op een gebeurtenis als u meer informatie.

### <a name="exceptions-by-container-name"></a>Uitzonderingen met de containernaam
![Voorbeeld](./media/app-insights-docker/14.png)

### <a name="docker-context-added-to-app-telemetry"></a>Docker-context toegevoegd aan app-telemetrie
Aanvraagtelemetrie verzonden vanuit de toepassing die is geïnstrumenteerd met AI SDK, is uitgebreid met Docker contextinformatie.

## <a name="q--a"></a>Vragen en antwoorden
*Wat Application Insights uitleggen die ik bij Docker ophalen kan?*

* Gedetailleerd overzicht van de prestatiemeteritems van container- en afbeeldingsbestanden.
* Integreer containers en app-gegevens in één dashboard.
* [Telemetrie exporteren](app-insights-export-telemetry.md) voor verdere analyse naar een database, Power BI of andere dashboard.

*Hoe krijg ik telemetrie van de app zelf?*

* De Application Insights-SDK installeren in de app. Leer hoe u voor: [Java-web-apps](app-insights-java-get-started.md), [Windows web-apps](app-insights-asp-net.md).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Volgende stappen

* [Application Insights voor Java](app-insights-java-get-started.md)
* [Application Insights voor Node.js](app-insights-nodejs.md)
* [Application Insights voor ASP.NET](app-insights-asp-net.md)

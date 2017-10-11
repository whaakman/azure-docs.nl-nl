---
title: De bewaking van Docker-toepassingen in Azure Application Insights | Microsoft Docs
description: Docker-prestatiemeteritems, gebeurtenissen en uitzonderingen kunnen worden weergegeven op de Application Insights, samen met de telemetrie van de beperkte apps.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 27a3083d-d67f-4a07-8f3c-4edb65a0a685
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: b082e345ca1bb3b12c548e05e699474d3aa9306c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-docker-applications-in-application-insights"></a>Docker-toepassingen bewaken in Application Insights
Prestatiemeteritems van de levenscyclus van gebeurtenissen en de prestaties van [Docker](https://www.docker.com/) containers kunnen diagram op Application Insights. Installeer de [Application Insights](app-insights-overview.md) installatiekopie in een container in de host en prestatiemeteritems worden weergegeven voor de host en de andere afbeeldingen.

Met Docker, moet u uw apps in lightweight containers met alle afhankelijkheden voltooid distribueren. Ze kunnen worden uitgevoerd op elke host waarop een Docker-Engine wordt uitgevoerd.

Bij het uitvoeren van de [Application Insights installatiekopie](https://hub.docker.com/r/microsoft/applicationinsights/) op uw host Docker dat u deze voordelen:

* De levenscyclus van telemetrie over de containers die zijn uitgevoerd op de host - starten, stoppen, enzovoort.
* Prestatiemeteritems voor alle containers. CPU, geheugen, netwerkgebruik en meer.
* Als u [Application Insights-SDK voor Java geïnstalleerd](app-insights-java-live.md) de telemetrie van deze apps heeft in de apps die worden uitgevoerd in de containers, extra eigenschappen voor het identificeren van de container en host-machine. Als u exemplaren van een app in meer dan één host worden uitgevoerd, kunt u dus bijvoorbeeld eenvoudig uw app telemetrie door host filteren.

![Voorbeeld](./media/app-insights-docker/00.png)

## <a name="set-up-your-application-insights-resource"></a>Instellen van uw Application Insights-resource
1. Meld u aan bij [Microsoft Azure-portal](https://azure.com) en open de Application Insights-resource voor uw app of [Maak een nieuwe](app-insights-create-new-resource.md). 
   
    *Welke resource moet ik gebruiken?* Als de apps die u op de host uitvoert zijn ontwikkeld door iemand anders, moet u [Maak een nieuwe Application Insights-resource](app-insights-create-new-resource.md). Dit is waar u weergeven en analyseren van de telemetrie. (Selecteer algemene voor het type app.)
   
    Maar als u de ontwikkelaar van de apps bent, moet we hopen u dat [Application Insights-SDK toegevoegd](app-insights-java-live.md) aan elk van deze. Als ze alle echt onderdelen van een enkele business-toepassing, vervolgens u configureert deze voor het verzenden van telemetrie naar één resource en gebruikt u die dezelfde bron om de Docker en prestaties van de levenscyclus van de gegevens worden weergegeven. 
   
    Een derde scenario is dat u de meeste van de apps ontwikkeld, maar u afzonderlijke resources gebruikt om de telemetrie weer te geven. In dat geval u waarschijnlijk ook wilt maken van een afzonderlijke resource voor de Docker-gegevens. 
2. Toevoegen van de Docker-tegel: kies **toevoegen tegel**, sleept u de Docker-tegel in de galerie en klik vervolgens op **gedaan**. 
   
    ![Voorbeeld](./media/app-insights-docker/03.png)
3. Klik op de **Essentials** vervolgkeuzelijst en de Instrumentatiesleutel kopiëren. Met dit kunt u instellen dat de SDK waar u de telemetrie verzenden.

    ![Voorbeeld](./media/app-insights-docker/02-props.png)

Houd dat browservenster handig wanneer u zult keert u terug naar deze snel om te kijken naar uw telemetrie.

## <a name="run-the-application-insights-monitor-on-your-host"></a>De Application Insights-monitor op uw host worden uitgevoerd
Nu dat u hebt een andere locatie om de telemetrie weer te geven, kunt u de beperkte-app die wordt verzameld en verzonden instellen.

1. Verbinding maken met de Docker-host. 
2. De instrumentatiesleutel in deze opdracht bewerken en vervolgens uit te voeren:
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

U hoeft slechts één installatiekopie van de Application Insights per Docker-host. Als uw toepassing wordt geïmplementeerd op meerdere Docker-hosts, herhaalt u de opdracht op elke host.

## <a name="update-your-app"></a>Uw app bijwerken
Als uw toepassing is uitgerust met de [Application Insights-SDK voor Java](app-insights-java-get-started.md), voeg de volgende regel in het ApplicationInsights.xml-bestand in uw project, onder de `<TelemetryInitializers>` element:

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

Docker informatie zoals container en host-id wordt toegevoegd aan elk telemetrie-item dat is verzonden van uw app.

## <a name="view-your-telemetry"></a>Uw telemetrie weergeven
Ga terug naar uw Application Insights-resource in de Azure-portal.

Klik in de Docker-tegel.

Binnen enkele ogenblikken ziet u gegevens die binnenkomen vanuit de Docker-app, met name als u andere containers uitgevoerd op uw Docker-engine.

Hier zijn enkele van de weergaven die u kunt ophalen.

### <a name="perf-counters-by-host-activity-by-image"></a>Prestatiemeteritems door host activiteit door de installatiekopie
![Voorbeeld](./media/app-insights-docker/10.png)

![Voorbeeld](./media/app-insights-docker/11.png)

Klik op een host- of image-naam voor meer informatie.

Klik op een grafiek, het raster kop, of gebruik grafiek toevoegen voor het aanpassen van de weergave. 

[Meer informatie over metrics explorer](app-insights-metrics-explorer.md).

### <a name="docker-container-events"></a>Gebeurtenissen van docker-container
![Voorbeeld](./media/app-insights-docker/13.png)

Voor het onderzoeken van afzonderlijke gebeurtenissen, klikt u op [Search](app-insights-diagnostic-search.md). Zoeken en filteren op de gebeurtenissen die u wilt zoeken. Klik op een gebeurtenis als u meer informatie.

### <a name="exceptions-by-container-name"></a>Uitzonderingen op de naam van de container
![Voorbeeld](./media/app-insights-docker/14.png)

### <a name="docker-context-added-to-app-telemetry"></a>Docker-context toegevoegd aan app telemetrie
Aanvraagtelemetrie van de toepassing is uitgerust met AI-SDK, verrijkt met Docker-context verzonden:

![Voorbeeld](./media/app-insights-docker/16.png)

Tijd van processor en geheugen prestatiemeteritems, uitgebreid en gegroepeerd op de naam van de Docker-container:

![Voorbeeld](./media/app-insights-docker/15.png)

## <a name="q--a"></a>Vragen en antwoorden
*Wat Application Insights opleveren die ik bij Docker ophalen kan?*

* Detail van prestatiemeteritems door container en de installatiekopie.
* Integreer container en app-gegevens in één dashboard.
* [Exporteren van telemetrie](app-insights-export-telemetry.md) voor verdere analyse naar een database, Power BI of andere dashboard.

*Hoe krijg ik telemetrie van de app zelf?*

* De Application Insights-SDK installeren in de app. Meer informatie over hoe voor: [Java-web-apps](app-insights-java-get-started.md), [Windows web-apps](app-insights-asp-net.md).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Volgende stappen

* [Application Insights voor Java](app-insights-java-get-started.md)
* [Application Insights voor Node.js](app-insights-nodejs.md)
* [Application Insights voor ASP.NET](app-insights-asp-net.md)

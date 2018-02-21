---
title: Bewaren van gegevens in uw omgeving Azure Time Series Insights begrijpen | Microsoft Docs
description: In dit artikel beschrijft de twee instellingen voor bewaren van gegevens in uw omgeving Azure Time Series Insights.
services: time-series-insights
ms.service: time-series-insights
author: anshan
ms.author: anshan
manager: kfile
editor: MicrosoftDocs/tsidocs
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 02/09/2018
ms.openlocfilehash: 46e0c4fa25c7d8a56763b80bf7de97c775c7ee99
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="understand-data-retention-in-time-series-insights"></a>Bewaren van gegevens in het Time Series Insights begrijpen
In dit artikel beschrijft de twee instellingen die van invloed zijn bewaren van gegevens in uw omgeving Time Series Insights (TSI).

Elke omgeving TSI heeft een instelling die bepaalt **gegevensretentietijd**. De waarde begint op 1 en 400 dagen. De gegevens worden verwijderd op basis van de omgeving capaciteit of bewaren opslagduur (1-400), afhankelijk van wat zich het eerste voordoet.

Elke omgeving TSI heeft een extra instelling **opslaglimiet overschreden gedrag**. Deze instelling bepaalt inkomende en opschonen gedrag als de maximale capaciteit van een omgeving is bereikt. Er zijn twee gedrag kunt kiezen uit:
- **Oude gegevens verwijderen** (standaard)  
- **Inkomend onderbreken**

> [!NOTE]
> Standaard bij het maken van een nieuwe omgeving, de bewaartermijn is geconfigureerd voor **oude gegevens verwijderen**. Deze instelling kan worden gezet, indien nodig later maken met de Azure-portal op de **configureren** pagina van de TSI-omgeving.

Raadpleeg voor informatie over het gedrag van de retentie overschakelen [configuratie van de bewaartermijn in tijd reeks inzichten](time-series-insights-how-to-configure-retention.md).

Vergelijk het gedrag voor het bewaren van gegevens:

## <a name="purge-old-data"></a>Oude gegevens verwijderen
- Dit gedrag is het standaardgedrag voor TSI omgevingen en voorwerpen dezelfde gedrag TSI omgevingen heeft optrad omdat werd openbare preview.  
- Dit gedrag wordt aanbevolen wanneer gebruikers willen altijd hun *meest recente gegevens* in hun omgeving TSI. 
- Dit gedrag *verwijdert* gegevens eenmaal in de omgeving van limieten (bewaartijd, grootte of tellen, afhankelijk van wat zich het eerste) zijn bereikt. Bewaarbeleid is standaard ingesteld op 30 dagen. 
- De oudste opgenomen gegevens worden eerst (FIFO-principe aanpak) verwijderd.

### <a name="example-1"></a>Voorbeeld 1:
U kunt een Voorbeeldomgeving met bewaren gedrag **inkomend doorgaan en het verwijderen van oude gegevens**: In dit voorbeeld **bewaartijd van gegevens** is ingesteld op 400 dagen. **Capaciteit** is ingesteld op S1-eenheid waarin 30 GB van de totale capaciteit.   Stel de inkomende gegevens stelt samen tot 500 MB per dag gemiddeld. Deze omgeving kunt alleen 60 dagen gegevens opgegeven voor de snelheid van inkomende gegevens omdat de maximale capaciteit is bereikt op 60 dagen bewaren. De inkomende gegevens stelt samen als: 500 MB elke dag x 60 dagen = 30 GB. 

In dit voorbeeld op de 61st dag, de omgeving de nieuwste gegevens ziet, maar verwijdert de oudste gegevens die ouder zijn dan 60 dagen. Het opschonen maakt ruimte voor de nieuwe gegevens streaming in, zodat nieuwe gegevens mogelijk blijven worden onderzocht. 

Als de gebruiker wil gegevens langer bewaren, ze de grootte van de omgeving kunnen verhogen door aanvullende eenheden toe te voegen of minder gegevens kunnen push.  

### <a name="example-2"></a>Voorbeeld 2:
Houd rekening met een omgeving ook bewaren gedrag geconfigureerd **inkomend doorgaan en het verwijderen van oude gegevens**. In dit voorbeeld **gegevensretentietijd** is ingesteld op een lagere waarde van 180 dagen. **Capaciteit** is ingesteld op S1-eenheid waarin 30 GB van de totale capaciteit. Om het opslaan van gegevens voor de volledige 180 dagen, mag niet de dagelijkse inkomend 0.166 GB (166 MB) overschrijden per dag.  

Wanneer deze omgeving dagelijkse inkomend snelheid 0.166 GB per dag overschrijdt, worden niet gedurende 180 dagen gegevens opgeslagen omdat sommige gegevens opgehaald worden opgeschoond. U kunt deze dezelfde omgeving gedurende een periode bezet. Stel de omgeving inkomend snelheid toe tot een gemiddelde 0.189 GB per dag. In dat tijdsbestek bezet ongeveer 158 dagen aan gegevens blijven behouden (30GB/0.189 = 158.73 dagen retentieperiode). Deze tijd is kleiner dan het tijdsbestek van de gewenste gegevens bewaren.

## <a name="pause-ingress"></a>Inkomend onderbreken
- Dit gedrag is ontworpen om te controleren of gegevens worden niet verwijderd als de grootte en het aantal limieten zijn bereikt voordat de bewaarperiode.  
- Dit gedrag biedt extra tijd voor de gebruikers om de capaciteit van hun omgeving verhogen voordat gegevens worden verwijderd vanwege overtredingen van bewaarperiode
- Dit gedrag beschermt tegen verlies van gegevens, maar maakt u de gelegenheid uw meest recente gegevens verloren gaan als inkomende gegevens buiten de bewaarperiode van de bron van de gebeurtenis is onderbroken.
- Echter zodra maximale capaciteit van een omgeving is bereikt, de omgeving wordt onderbroken gegevens inkomend totdat aanvullende acties uitgevoerd: 
   - Verhoogt u de maximale capaciteit van de omgeving. Zie voor meer informatie [schalen van uw omgeving Time Series Insights](time-series-insights-how-to-scale-your-environment.md) meer schaaleenheden toevoegen.
   - De bewaartermijn voor gegevens is bereikt en gegevens worden verwijderd, waardoor de omgeving lager dan de maximale capaciteit.

### <a name="example-3"></a>Voorbeeld 3:
Houd rekening met een omgeving met bewaren gedrag is geconfigureerd voor **onderbreken inkomend**. In dit voorbeeld wordt de **Gegevensretentieperiode** is ingesteld op 60 dagen. **Capaciteit** is ingesteld op 3 eenheden van S1. Stel dat deze omgeving heeft instroom van 2 GB aan gegevens per dag. In deze omgeving inkomend onderbroken zodra de maximale capaciteit is bereikt. De omgeving geeft op dat moment wordt dezelfde gegevensset totdat het inkomend hervat of totdat 'continue inkomend' is ingeschakeld (zou oudere gegevens ruimte te maken voor nieuwe gegevens opschonen). 

Wanneer de inkomende gegevens hervatten:
- Gegevensstromen in de volgorde waarin die deze is ontvangen door de bron van gebeurtenis
- De gebeurtenissen worden geïndexeerd op basis van hun timestamp, tenzij het bewaarbeleid op uw gebeurtenisbron is overschreden. Voor meer informatie over de configuratie van gebeurtenis bron bewaren [Veelgestelde vragen over Event-Hubs](../event-hubs/event-hubs-faq.md)

> [!IMPORTANT]
> Waarschuwingen met de melding om te voorkomen inkomend wordt onderbroken, moet u instellen. Gegevensverlies is mogelijk omdat de bewaarperiode standaard 1 dag voor de bronnen van gebeurtenissen van Azure is. Daarom zodra inkomend is onderbroken, verliest waarschijnlijk u de meest recente gegevens tenzij verdere actie wordt ondernomen. U moet de capaciteit vergroten of overschakelen van het gedrag **oude gegevens verwijderen** om de potentiële gegevensverlies te voorkomen.

In de betrokken Event Hubs, kunt u aanpassen van de **bericht bewaren** eigenschap om gegevensverlies te minimaliseren als onderbreken inkomend in de Time Series Insights optreedt.

![Event hub bericht bewaren.](media/time-series-insights-contepts-retention/event-hub-retention.png)

Als er zijn geen eigenschappen zijn geconfigureerd op de bron van gebeurtenis (timeStampPropertyName), standaard TSI de tijdstempel van de aankomst in de event hub als de x-as. Als timeStampPropertyName is geconfigureerd als een iets andere betekenis, wordt de omgeving gezocht naar de geconfigureerde timeStampPropertyName in het gegevenspakket wanneer gebeurtenissen worden geparseerd. 

Als u uw omgeving moet te voorzien in extra capaciteit te verhogen van de lengte van de retentie, Zie opschalen [schalen van uw omgeving Time Series Insights](time-series-insights-how-to-scale-your-environment.md) voor meer informatie.  

## <a name="next-steps"></a>Volgende stappen
Raadpleeg voor informatie over het gedrag van de retentie overschakelen [configuratie van de bewaartermijn in tijd reeks inzichten](time-series-insights-how-to-configure-retention.md).

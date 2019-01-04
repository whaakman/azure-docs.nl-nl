---
title: Inzicht in Gegevensretentie in uw Azure Time Series Insights-omgeving | Microsoft Docs
description: Dit artikel beschrijft de twee instellingen voor bewaren van gegevens in uw Azure Time Series Insights-omgeving.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/09/2018
ms.custom: seodec18
ms.openlocfilehash: c44b09e15a227e11426d2798fc071778ca47ebd3
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53557460"
---
# <a name="understand-data-retention-in-time-series-insights"></a>Inzicht in Gegevensretentie in Time Series Insights

Dit artikel beschrijft de twee instellingen die van invloed zijn op het bewaren van gegevens in uw omgeving Time Series Insights (TSI).

## <a name="video"></a>Video: 

### <a name="in-this-video-we-cover-time-series-insights-data-retention-and-how-to-plan-for-itbr"></a>In deze video behandelen we bewaren van Time Series Insights-gegevens en het plannen voor deze.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

Elke TSI-omgeving heeft een instelling die regelt **gegevensretentietijd**. De waarde omvat van 1 tot 400 dagen. De gegevens worden verwijderd op basis van de omgeving capaciteit of bewaring opslagduur (1-400), afhankelijk van wat het eerste komt.

Elke TSI-omgeving heeft een instelling voor extra **-limiet voor opslag overschreden gedrag**. Deze instelling wordt bepaald gedrag van inkomend verkeer en opschonen na het verstrijken van de maximale capaciteit van een omgeving. Er zijn twee gedrag kiezen uit:
- **Verwijderen van oude gegevens** (standaard)  
- **Inkomende gegevens onderbreken**

> [!NOTE]
> Standaard bij het maken van een nieuwe omgeving, de bewaarperiode is geconfigureerd voor **oude gegevens opschonen**. Deze instelling kan worden gezet, indien nodig later maken met behulp van de Azure-portal op de **configureren** pagina van de TSI-omgeving.

Raadpleeg voor informatie over het overschakelen van retentie gedrag [configuratie van de bewaartermijn in Time Series Insights](time-series-insights-how-to-configure-retention.md).

Vergelijk het gedrag voor het bewaren van gegevens:

## <a name="purge-old-data"></a>Oude gegevens verwijderen
- Dit gedrag is het standaardgedrag voor TSI-omgevingen en stands hetzelfde gedrag TSI omgevingen heeft optrad omdat deze gestart openbare preview.  
- Dit gedrag wordt aanbevolen wanneer gebruikers willen altijd hun *meest recente gegevens* in hun omgeving TSI. 
- Dit gedrag *schoont* gegevens eenmaal in de omgeving van limieten (bewaartijd, grootte of count, afhankelijk van wat het eerste komt) zijn bereikt. Bewaarperiode is standaard ingesteld op 30 dagen. 
- De oudste opgenomen gegevens wordt eerst (FIFO-aanpak) verwijderd.

### <a name="example-1"></a>Voorbeeld 1:
Houd rekening met een Voorbeeldomgeving met retentie gedrag **doorgaan inkomend verkeer en verwijderen van oude gegevens**: In dit voorbeeld **gegevensretentietijd** is ingesteld op 400 dagen. **Capaciteit** is ingesteld op de S1-eenheid, waarmee 30 GB van de totale capaciteit bevat.   Stel de binnenkomende gegevens worden bij elkaar opgeteld tot 500 MB per dag op gemiddelde. Deze omgeving kan alleen 60 dagen aan gegevens toekennen aan het aantal inkomende gegevens, omdat de maximale capaciteit is bereikt op 60 dagen bewaren. De binnenkomende gegevens worden bij elkaar opgeteld als: 500 MB elke dag x 60 dagen = 30 GB. 

In dit voorbeeld op de 61st dag wordt de omgeving de meest recente gegevens worden weergegeven, maar Hiermee verwijdert u de oudste gegevens die ouder zijn dan 60 dagen. Het opschonen van Logboeken maakt ruimte voor de nieuwe gegevensstromen in, zodat nieuwe gegevens kunnen verder worden onderzocht. 

Als de gebruiker wil gegevens langer bewaren, ze kunnen de grootte van de omgeving verhogen door meer eenheden toe te voegen of minder gegevens te pushen.  

### <a name="example-2"></a>Voorbeeld 2:
Houd rekening met een omgeving wordt ook geconfigureerd retentie gedrag **doorgaan inkomend verkeer en verwijderen van oude gegevens**. In dit voorbeeld **gegevensretentietijd** is ingesteld op een lagere waarde van 180 dagen. **Capaciteit** is ingesteld op de S1-eenheid, waarmee 30 GB van de totale capaciteit bevat. Als u wilt opslaan van gegevens voor de volledige 180 dagen, overschrijden niet de dagelijkse invoer 0.166 GB (166 MB) per dag.  

Wanneer er dagelijks tarief dat inkomend verkeer van deze omgeving groter is dan van 0.166 GB per dag, kan niet gedurende 180 dagen gegevens worden opgeslagen omdat sommige gegevens wordt opgeschoond. Houd rekening met deze dezelfde omgeving gedurende een periode bezet. Wordt ervan uitgegaan dat van de omgeving inkomend snelheid verhogen tot een gemiddelde 0.189 GB per dag. In dat tijdsbestek bezet over 158 dagen aan gegevens worden bewaard (30GB/0.189 = 158,73 dagen retentie). Deze tijd is kleiner dan het tijdsbestek van de gewenste gegevens bewaren.

## <a name="pause-ingress"></a>Inkomende gegevens onderbreken
- Dit gedrag is zo ontworpen dat de gegevens is niet verwijderd als het aantal en grootte limieten zijn bereikt voordat de bewaarperiode.  
- Dit gedrag biedt extra tijd voor de gebruikers de capaciteit van hun omgeving vergroten voordat gegevens wordt leeggemaakt vanwege de bewaarperiode van inbreuk
- Dit gedrag helpt te beveiligen tegen verlies van gegevens, maar maakt u een kans op het verlies van gegevens van uw meest recente gegevens als inkomend buiten de bewaarperiode van uw gebeurtenisbron is onderbroken.
- Echter, als de maximale capaciteit van een omgeving is bereikt, de omgeving wordt onderbroken binnenkomende gegevens totdat aanvullende acties worden uitgevoerd: 
   - Verhoogt u de maximale capaciteit van de omgeving. Zie voor meer informatie, [uw Time Series Insights-omgeving schalen](time-series-insights-how-to-scale-your-environment.md) om toe te voegen meer schaaleenheden.
   - De bewaarperiode is bereikt en de gegevens is verwijderd, waardoor de omgeving dan de maximale capaciteit.

### <a name="example-3"></a>Voorbeeld 3:
Houd rekening met een omgeving met retentie gedrag is geconfigureerd voor het **onderbreken inkomend**. In dit voorbeeld wordt de **bewaartermijn voor gegevens** is geconfigureerd voor 60 dagen. **Capaciteit** is ingesteld op 3 eenheden van S1. Wordt ervan uitgegaan dat deze omgeving heeft invoer van 2 GB aan gegevens per dag. Inkomend verkeer is in deze omgeving worden onderbroken als de maximale capaciteit is bereikt. De omgeving bevat dezelfde gegevensset op dat moment totdat inkomend verkeer wordt hervat of totdat de 'continue inkomend' is ingeschakeld (dit zou oudere gegevens ruimte te maken voor nieuwe gegevens opschonen). 

Inkomend verkeer hervat wanneer:
- Gegevens stromen in de volgorde waarin die deze is ontvangen door de bron van gebeurtenis
- De gebeurtenissen worden geïndexeerd, op basis van hun timestamp, tenzij u hebt voor het bewaren van uw gebeurtenisbron overschreden. Voor meer informatie over de configuratie van gebeurtenis bron retentie [Veelgestelde vragen over Event-Hubs](../event-hubs/event-hubs-faq.md)

> [!IMPORTANT]
> Waarschuwingen om u te leveren om te voorkomen dat inkomend verkeer wordt onderbroken, moet u instellen. Verlies van gegevens is mogelijk omdat de bewaarperiode van de standaardwaarde 1 dag voor de bronnen van gebeurtenissen van Azure is. Daarom wanneer inkomend verkeer wordt onderbroken, verliest waarschijnlijk u de meest recente gegevens, tenzij er verdere actie wordt ondernomen. U moet de capaciteit vergroten of overschakelen van het gedrag **oude gegevens opschonen** om het mogelijke gegevensverlies te voorkomen.

Overweeg aan te passen in de betrokken Event Hubs, de **bewaarperiode van bericht** eigenschap om gegevensverlies te minimaliseren wanneer onderbreken inkomend verkeer in Time Series Insights plaatsvindt.

![Bewaartermijn voor berichten van Event hub.](media/time-series-insights-contepts-retention/event-hub-retention.png)

Als er geen eigenschappen zijn geconfigureerd op de bron van gebeurtenis (timeStampPropertyName), standaard TSI het tijdstempel van de aankomst in de event hub als de x-as. Als timeStampPropertyName is geconfigureerd voor een ander, wordt de omgeving voor de geconfigureerde timeStampPropertyName in het gegevenspakket eruitziet wanneer gebeurtenissen worden geparseerd. 

Als u uw omgeving moet omhoog te schalen om te voorzien in extra capaciteit of te verhogen van de lengte van retentie, Zie [uw Time Series Insights-omgeving schalen](time-series-insights-how-to-scale-your-environment.md) voor meer informatie.  

## <a name="next-steps"></a>Volgende stappen
Raadpleeg voor informatie over het overschakelen van retentie gedrag [configuratie van de bewaartermijn in Time Series Insights](time-series-insights-how-to-configure-retention.md).

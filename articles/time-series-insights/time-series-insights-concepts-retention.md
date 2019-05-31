---
title: Inzicht in Gegevensretentie in uw Azure Time Series Insights-omgeving | Microsoft Docs
description: Dit artikel beschrijft de twee instellingen voor bewaren van gegevens in uw Azure Time Series Insights-omgeving.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: e3336df30873b40d2b8a464d1f866b524f76776d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236999"
---
# <a name="understand-data-retention-in-azure-time-series-insights"></a>Inzicht in Gegevensretentie in Azure Time Series Insights

Dit artikel beschrijft de twee instellingen die van invloed zijn op het bewaren van gegevens in uw Azure Time Series Insights-omgeving.

## <a name="video"></a>Video

### <a name="the-following-video-summarizes-time-series-insights-data-retention-and-how-to-plan-for-itbr"></a>De volgende video geeft een overzicht van Time Series Insights het bewaren van gegevens en het plannen voor deze.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

Elk van uw Azure Time Series-omgevingen heeft een instelling die regelt **gegevensretentietijd**. De waarde omvat van 1 tot 400 dagen. De gegevens worden verwijderd op basis van de opslagcapaciteit van de omgeving of de bewaartermijn, afhankelijk van wat het eerste komt.

Bovendien uw Azure Time Series-omgeving heeft een **-limiet voor opslag overschreden gedrag** instelling. Deze besturingselementen inkomend verkeer en gedrag opschonen na het verstrijken van de maximale capaciteit van een omgeving. Er zijn twee gedrag om de verkeersbelasting te configureren:

- **Verwijderen van oude gegevens** (standaard)  
- **Inkomende gegevens onderbreken**

> [!NOTE]
> Standaard bij het maken van een nieuwe omgeving, de bewaarperiode is geconfigureerd voor **oude gegevens opschonen**. Deze instelling kan worden gezet, indien nodig later maken met behulp van de Azure-portal op de **configureren** pagina van de Time Series Insights-omgeving.

Raadpleeg voor informatie over het overschakelen van retentie gedrag [configuratie van de bewaartermijn in Time Series Insights](time-series-insights-how-to-configure-retention.md).

Vergelijk het gedrag voor het bewaren van gegevens:

## <a name="purge-old-data"></a>Oude gegevens verwijderen

- Dit gedrag is het standaardgedrag voor Time Series Insights-omgevingen.  
- Dit gedrag wordt aanbevolen wanneer gebruikers willen altijd hun *meest recente gegevens* in hun Time Series Insights-omgeving.
- Dit gedrag *schoont* gegevens eenmaal in de omgeving van limieten (bewaartijd, grootte of count, afhankelijk van wat het eerste komt) zijn bereikt. Bewaarperiode is standaard ingesteld op 30 dagen.
- De oudste opgenomen gegevens wordt eerst (FIFO-aanpak) verwijderd.

### <a name="example-one"></a>Voorbeeld van een

Houd rekening met een Voorbeeldomgeving met retentie gedrag **doorgaan inkomend verkeer en verwijderen van oude gegevens**:

**Bewaartijd van gegevens** is ingesteld op 400 dagen. **Capaciteit** is ingesteld op de S1-eenheid, waarmee 30 GB van de totale capaciteit bevat.   Stel de binnenkomende gegevens worden bij elkaar opgeteld tot 500 MB per dag op gemiddelde. Deze omgeving kan alleen 60 dagen aan gegevens toekennen aan het aantal inkomende gegevens, omdat de maximale capaciteit is bereikt op 60 dagen bewaren. De binnenkomende gegevens worden bij elkaar opgeteld als: 500 MB elke dag x 60 dagen = 30 GB.

Op de 61st dag wordt de omgeving de meest recente gegevens worden weergegeven, maar Hiermee verwijdert u de oudste gegevens die ouder zijn dan 60 dagen. Het opschonen van Logboeken maakt ruimte voor de nieuwe gegevensstromen in, zodat nieuwe gegevens kunnen verder worden onderzocht. Als de gebruiker wil gegevens langer bewaren, ze kunnen de grootte van de omgeving verhogen door meer eenheden toe te voegen of minder gegevens te pushen.  

### <a name="example-two"></a>Voorbeeld 2

Houd rekening met een omgeving wordt ook geconfigureerd retentie gedrag **doorgaan inkomend verkeer en verwijderen van oude gegevens**. In dit voorbeeld **gegevensretentietijd** is ingesteld op een lagere waarde van 180 dagen. **Capaciteit** is ingesteld op de S1-eenheid, waarmee 30 GB van de totale capaciteit bevat. Als u wilt opslaan van gegevens voor de volledige 180 dagen, overschrijden niet de dagelijkse invoer 0.166 GB (166 MB) per dag.  

Wanneer er dagelijks tarief dat inkomend verkeer van deze omgeving groter is dan van 0.166 GB per dag, kan niet gedurende 180 dagen gegevens worden opgeslagen omdat sommige gegevens wordt opgeschoond. Houd rekening met deze dezelfde omgeving gedurende een periode bezet. Wordt ervan uitgegaan dat van de omgeving inkomend snelheid verhogen tot een gemiddelde 0.189 GB per dag. In dat tijdsbestek bezet over 158 dagen aan gegevens worden bewaard (30GB/0.189 = 158,73 dagen retentie). Deze tijd is kleiner dan het tijdsbestek van de gewenste gegevens bewaren.

## <a name="pause-ingress"></a>Inkomende gegevens onderbreken

- De **onderbreken inkomend** instelling is ontworpen om te controleren of de gegevens is niet verwijderd als het aantal en grootte limieten zijn bereikt voordat de bewaarperiode.  
- **Onderbreken van inkomend verkeer** vindt u meer tijd voor de gebruikers de capaciteit van hun omgeving vergroten voordat gegevens wordt leeggemaakt vanwege de bewaarperiode van inbreuk
- Het beschermt tegen verlies van gegevens maar een kans op het verlies van gegevens van uw meest recente gegevens kunt maken als het inkomende gegevens buiten de bewaarperiode van uw gebeurtenisbron is onderbroken.
- Echter, als de maximale capaciteit van een omgeving is bereikt, de omgeving wordt onderbroken binnenkomende gegevens totdat de volgende aanvullende acties uitgevoerd:

   - Verhoogt u de maximale capaciteit van de omgeving om toe te voegen meer schaaleenheden, zoals beschreven in [uw Time Series Insights-omgeving schalen](time-series-insights-how-to-scale-your-environment.md).
   - De bewaarperiode is bereikt en de gegevens is verwijderd, waardoor de omgeving dan de maximale capaciteit.

### <a name="example-three"></a>Voorbeeld 3

Houd rekening met een omgeving met retentie gedrag is geconfigureerd voor het **onderbreken inkomend**. In dit voorbeeld wordt de **bewaartermijn voor gegevens** is geconfigureerd voor 60 dagen. **Capaciteit** is ingesteld op drie (3) eenheden van S1. Wordt ervan uitgegaan dat deze omgeving heeft invoer van 2 GB aan gegevens per dag. Inkomend verkeer is in deze omgeving worden onderbroken als de maximale capaciteit is bereikt.

Op dat moment wordt de omgeving bevat dezelfde gegevensset tot inkomend verkeer wordt hervat of tot **blijven inkomend** is ingeschakeld (die zou oudere gegevens te scheppen voor nieuwe gegevens opschonen).

Inkomend verkeer hervat wanneer:

- Gegevens stromen in de volgorde waarin die deze is ontvangen door de bron van gebeurtenis
- De gebeurtenissen worden geïndexeerd, op basis van hun timestamp, tenzij u hebt voor het bewaren van uw gebeurtenisbron overschreden. Voor meer informatie over de configuratie van gebeurtenis bron retentie [Veelgestelde vragen over Event-Hubs](../event-hubs/event-hubs-faq.md)

> [!IMPORTANT]
> Waarschuwingen om u te leveren om te voorkomen dat inkomend verkeer wordt onderbroken, moet u instellen. Verlies van gegevens is mogelijk omdat de bewaarperiode van de standaardwaarde 1 dag voor de bronnen van gebeurtenissen van Azure is. Daarom wanneer inkomend verkeer wordt onderbroken, verliest waarschijnlijk u de meest recente gegevens, tenzij er verdere actie wordt ondernomen. U moet de capaciteit vergroten of overschakelen van het gedrag **oude gegevens opschonen** om het mogelijke gegevensverlies te voorkomen.

Overweeg aan te passen in de betrokken Event Hubs, de **bewaarperiode van bericht** eigenschap om gegevensverlies te minimaliseren wanneer onderbreken inkomend verkeer in Time Series Insights plaatsvindt.

[![Bewaartermijn voor berichten van Event hub.](media/time-series-insights-contepts-retention/event-hub-retention.png)](media/time-series-insights-contepts-retention/event-hub-retention.png#lightbox)

Als er geen eigenschappen die zijn geconfigureerd op de bron van gebeurtenis (`timeStampPropertyName`), Time Series Insights standaard ingesteld op de tijdstempel van de aankomst in de event hub als de x-as. Als `timeStampPropertyName` is geconfigureerd voor iets anders, de omgeving zoekt de geconfigureerde `timeStampPropertyName` in het gegevenspakket wanneer gebeurtenissen worden geparseerd.

Als u uw omgeving moet omhoog te schalen om te voorzien in extra capaciteit of te verhogen van de lengte van retentie, Zie [uw Time Series Insights-omgeving schalen](time-series-insights-how-to-scale-your-environment.md) voor meer informatie.  

## <a name="next-steps"></a>Volgende stappen

- Lees voor informatie over het configureren van of het wijzigen van instellingen voor het bewaren van gegevens, [configuratie van de bewaartermijn in Time Series Insights](time-series-insights-how-to-configure-retention.md).

---
title: Controlepunt- en replayaanvallen taak concepten van gegevensherstel in Azure Stream Analytics
description: Dit artikel wordt beschreven controlepunt- en replayaanvallen taak concepten van gegevensherstel in Azure Stream Analytics.
services: stream-analytics
author: zhongc
ms.author: zhongc
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2018
ms.openlocfilehash: 1a7cb6c5d9c3383b127ce38ae21bb2dc811e1f2e
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="checkpoint-and-replay-concepts-in-azure-stream-analytics-jobs"></a>Controlepunt- en replayaanvallen concepten in Azure Stream Analytics-taken
In dit artikel beschrijft de interne controlepunt- en replayaanvallen concepten in Azure Stream Analytics en de impact die hebben op het herstel van de taak. Telkens wanneer wordt een Stream Analytics-taak wordt uitgevoerd, informatie over de status bijgehouden intern. Deze informatie over de status wordt opgeslagen in een controlepunt regelmatig. De informatie van het controlepunt wordt gebruikt voor herstel van de taak in bepaalde situaties, als een taak is mislukt of upgrade optreedt. In andere gevallen kan het controlepunt kan niet worden gebruikt voor herstel en een replay nodig is.

## <a name="stateful-query-logic-in-temporal-elements"></a>Stateful query logica in de tijdelijke elementen
Een van de unieke mogelijkheden van Azure Stream Analytics-taak is stateful processen, zoals functies in vensters en tijdelijke joins, tijdelijke analytische functies uitvoeren. Elk van deze operators statusinformatie bijgehouden wanneer de taak wordt uitgevoerd. De maximale venstergrootte voor deze query-elementen is zeven dagen. 

Het concept tijdelijk venster wordt weergegeven in verschillende elementen van de Stream Analytics query:
1. Functies in vensters (groep door van Tumbling, Hopping en Verschuivend windows)

2. Tijdelijke joins (JOIN met DATEDIFF)

3. Tijdelijke analytische functies (ISFIRST, LAST en LAG met LIMIT DURATION)


## <a name="job-recovery-from-node-failure-including-os-upgrade"></a>Herstel van de taak van storingen in knooppunten, met inbegrip van bijwerken van het besturingssysteem
Telkens wanneer die een Stream Analytics-taak wordt uitgevoerd, is intern deze uitgebreid om te werken op meerdere worker-knooppunten. Elk werkrolknooppunt status wordt gecontroleerd om de paar minuten, waarmee het systeem herstellen als er een fout optreedt.

Tijd tot tijd een bepaalde werkrolknooppunt mislukken of een upgrade kan optreden voor dat knooppunt worker. Als u wilt herstellen automatisch, Stream Analytics verkrijgt een nieuw knooppunt in orde en het voorafgaande werkrolknooppunt is hersteld vanaf het laatste controlepunt. Als u wilt doorgaan met het werk, is een kleine hoeveelheid replay nodig om de status herstellen vanaf het tijdstip waarop het controlepunt wordt genomen. De afstand van de herstelbewerking is meestal slechts een paar minuten. Wanneer er onvoldoende Streaming-eenheden voor de taak zijn geselecteerd, moet het opnieuw afspelen snel worden voltooid. 

In een volledig parallelle query is de tijd die nodig is na een storing van de werknemer knooppunt achterhalen die evenredig is met:

[de snelheid van de invoer gebeurtenissen] x [de lengte van een gap] / [nummer van het verwerken van partities]

Als u ooit aanzienlijke verwerking vertraging vanwege knooppuntfout van zien en besturingssysteem bijwerken, Overweeg de query volledig parallelle en schalen van de taak voor het toewijzen van meer Streaming-eenheden. Zie voor meer informatie [schalen van een Azure Stream Analytics-taak voor een betere doorvoer](stream-analytics-scale-jobs.md).

Huidige Stream Analytics biedt een rapport niet weergeven wanneer dit soort herstelproces plaatsvindt.

## <a name="job-recovery-from-a-service-upgrade"></a>Herstel van de taak van een service-upgrade 
Microsoft worden af en toe bijgewerkt van de binaire bestanden voor dat de Stream Analytics-taken in de Azure-service uitvoeren. Bij deze tijden gebruikers actieve taken worden bijgewerkt naar een nieuwere versie en de taak wordt automatisch opnieuw opgestart. 

De indeling van de controlepunt herstel is op dit moment niet behouden tussen upgrades. De status van de streaming-query moet als gevolg hiervan worden hersteld met volledig replay-techniek. Als u wilt toestaan dat de Stream Analytics-taken voor de replay van de exacte dezelfde invoer vanaf voordat het is belangrijk om in te stellen het bewaarbeleid voor de brongegevens ten minste het venster groottes in uw query. Niet doet, kan leiden tot onjuiste of gedeeltelijke resultaten tijdens de upgrade van de service, omdat de brongegevens worden niet genoeg ver vastgehouden mogelijk zodanig dat de volledige venstergrootte.

De hoeveelheid replay nodig is in het algemeen evenredig aan de grootte van het venster vermenigvuldigd met het gemiddelde snelheid van gebeurtenissen. Als voorbeeld voor een taak met een invoer tarief van 1000 gebeurtenissen per seconde, venster groter is dan een uur wordt beschouwd als een grote replay-grootte hebben. Voor query's met een grootte van de grote replay waarnemen u vertraagde uitvoer (geen uitvoer) gedurende een langere periode. 

## <a name="estimate-replay-catch-up-time"></a>Schatting replay achterstallige tijd
U kunt deze techniek volgen voor een schatting van de lengte van de vertraging veroorzaakt door een service-upgrade:

1. Laden van de invoer Event Hub met voldoende gegevens ten aanzien van de grootste venstergrootte in uw query, verwachte gebeurtenis snelheid. De gebeurtenissen met de tijdstempel moet dicht bij de kloktijd gedurende deze periode, als als is een live invoer feed. Als u een venster 3 dagen in de query hebt, gebeurtenissen verzenden naar Event Hub voor drie dagen en doorgaan met het verzenden van gebeurtenissen. 

2. Start de taak met behulp van **nu** als de begintijd. 

3. Meten van de tijd tussen de begintijd en wanneer de eerste uitvoer wordt gegenereerd. De tijd is ruwe hoeveel vertraging de taak tijdens de upgrade van een service veroorzaken zal.

4. Als de vertraging te lang is, probeert u voor het partitioneren van de taak en verhogen aantal SUs, zodat de belasting wordt verdeeld op meer knooppunten. U kunt ook Verklein de grootte van het venster in de query en verdere uitvoeren aggregatie of andere stateful-verwerking op de uitvoer geproduceerd door de Stream Analytics-taak in de downstream-sink (bijvoorbeeld met behulp van Azure SQL database).

Voor algemene service stabiliteit is tijdens de upgrade van een missie kritieke taken, kunt u dubbele taken uitgevoerd in gekoppelde Azure-regio's. Zie voor meer informatie [garantie Stream Analytics-taak betrouwbaarheid tijdens het bijwerken van de service](stream-analytics-job-reliability.md).

## <a name="job-recovery-from-a-user-initiated-stop-and-start"></a>Herstel van de taak van een gebruiker gestarte stoppen en starten
De syntaxis van de Query op een streaming-taak bewerken of om aan te passen in- en uitgangen, moet de taak stoppen om de wijzigingen aanbrengen en upgrade van het ontwerp van de taak. In dergelijke scenario's, wanneer een gebruiker de streaming-taak wordt gestopt en opnieuw in en start is de herstelscenario vergelijkbaar met service-upgrade. 

Controlepuntgegevens kan niet worden gebruikt voor een gebruiker gestarte taak opnieuw starten. Gebruik dezelfde procedure zoals beschreven in de vorige sectie voor een schatting van de vertraging van uitvoer tijdens het opnieuw opstarten, en vergelijkbare risicobeperking van toepassing als de vertraging te lang is.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de betrouwbaarheid en -schaalbaarheid in deze artikelen:
- [Zelfstudie: Waarschuwingen instellen voor Azure Stream Analytics-taken](stream-analytics-set-up-alerts.md)
- [Een Azure Stream Analytics-taak voor een betere doorvoer schalen](stream-analytics-scale-jobs.md)
- [Stream Analytics-taak betrouwbaarheid garanderen tijdens de service-updates](stream-analytics-job-reliability.md)

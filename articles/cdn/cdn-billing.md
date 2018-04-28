---
title: Wat is Azure CDN facturering? | Microsoft Docs
description: Deze Veelgestelde vragen wordt de werking van Azure CDN facturering beschreven.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: v-deasim
ms.openlocfilehash: 218c493c772dc8fd212efaf60a0599fa2e896411
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="understanding-azure-cdn-billing"></a>Wat is Azure CDN facturering?

Deze Veelgestelde vragen over de facturering structuur voor inhoud die wordt gehost door Azure inhoud Delivery Network (CDN) wordt beschreven.

## <a name="what-is-a-billing-region"></a>Wat is er een facturering regio?
Een facturering regio is een geografische regio gebruikt om te bepalen welk percentage van Azure CDN gebruikt voor het leveren van objecten. De huidige facturering zones en hun regio's zijn:

- Zone 1: Noord-Amerika, Europa, Midden-Oosten en Afrika

- Zone 2: Azië en Stille Oceaan (inclusief Japan)

- Zone 3: Zuid-Amerika

- Zone 4: Australië en Nieuw-Zeeland

- Zone 5: India

Zie voor meer informatie over punt-of-presence (POP) regio's [Azure CDN POP-locaties per regio](https://docs.microsoft.com/azure/cdn/cdn-pop-locations). Bijvoorbeeld, een pop-server zich bevindt in Mexico is in de regio Noord-Amerika en daarom is opgenomen in de zone 1. 

Zie voor meer informatie over prijzen voor Azure CDN [Content Delivery Network prijzen](https://azure.microsoft.com/is-is/pricing/details/cdn/).

## <a name="how-are-delivery-charges-calculated-by-region"></a>Hoe worden levering kosten berekend per regio?
De Azure CDN facturering regio is gebaseerd op de locatie van de bronserver leveren van de inhoud voor de eindgebruiker. Het doel (fysieke locatie) van de client wordt niet beschouwd als de facturering regio.

Bijvoorbeeld, als een gebruiker op Mexico een aanvraag indient en deze aanvraag wordt uitgevoerd door een server die zich in een POP-Verenigde Staten vanwege peering of verkeer voorwaarden, de facturering regio worden de Verenigde Staten.

## <a name="what-is-a-billable-azure-cdn-transaction"></a>Wat is een factureerbare Azure CDN-transactie?
Elk verzoek HTTP (S) die op de CDN eindigt is een factureerbare gebeurtenis, waaronder alle antwoordtypen: geslaagd, mislukt of andere. Verschillende antwoorden mogelijk echter ander verkeer bedragen te genereren. Bijvoorbeeld: *304 niet gewijzigd* en andere reacties alleen een header weinig verkeer genereren omdat ze een klein header-reactie; op deze manier foutberichten (bijvoorbeeld *404 niet gevonden*) zijn factureerbare maar een kleine kosten in rekening worden vanwege de nettolading van de kleine antwoord.

## <a name="what-other-azure-costs-are-associated-with-azure-cdn-use"></a>Welke Azure kosten zijn gekoppeld aan Azure CDN gebruiken?
Met behulp van Azure CDN ook leidt ertoe dat sommige gebruikskosten voor de services voor uw objecten aan de oorsprong gebruikt. Deze kosten zijn meestal een klein deel van de totale kosten van CDN-gebruik.

Als u voor uw inhoud Azure Blob-opslag als de oorsprong gebruikt, u ook de volgende worden opslagkosten in rekening voor de opvulling van de cache:

- Werkelijke GB gebruikt: de werkelijke opslag van de bronobjecten.

- Overdrachten in GB: de hoeveelheid gegevens overgedragen naar de CDN-caches te vullen.

- Transacties: indien nodig voor het vervullen van de cache.

Zie voor meer informatie over Azure Storage facturering [wat Azure Storage facturering – bandbreedte, transacties en capaciteit](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

Als u *gehoste service levering*, u wordt kosten als volgt:

- Azure compute-tijd: de compute-exemplaren die als de oorsprong fungeren.

- Azure compute-overdracht: de gegevens worden overgedragen van de compute-exemplaren voor het vervullen van de Azure CDN-caches.

Als de client gebruikt byte-bereikaanvragen (ongeacht de oorsprong-service), wordt de volgende overwegingen toepassen:

- Een *byte bereikaanvraag* is een factureerbare transactie bij de CDN. Wanneer een client een aanvraag van het bereik aan bytes uitgeeft, wordt deze aanvraag is voor een subset (bereik) van het object. De CDN reageert met slechts een gedeeltelijke gedeelte van de inhoud die is aangevraagd. Deze gedeeltelijk antwoord is een factureerbare transactie en de mate waarin de overdracht is beperkt tot de grootte van het bereik antwoord (plus headers).

- Als u een aanvraag ontvangt voor alleen deel uitmaken van een object (door het opgeven van een koptekst bereik in bytes), kan de CDN het gehele object ophalen in de cache. Als gevolg hiervan, hoewel de factureerbare transactie vanaf de CDN voor een gedeeltelijk antwoord is, de factureerbare transactie vanuit de oorsprong u mogelijk de volledige grootte van het object.

## <a name="how-much-transfer-activity-occurs-to-support-the-cache"></a>Hoeveel overdrachtactiviteit gebeurt ter ondersteuning van de cache?
Telkens wanneer die een CDN pop-locatie moet voor het vervullen van de cache, maakt een aanvraag naar de oorsprong voor het object in de cache wordt opgeslagen. Als gevolg hiervan een factureerbare transactie op elke Cachemisser leidt ertoe dat de oorsprong. Het aantal missers in cache is afhankelijk van een aantal factoren:

- Hoe caching geschikte de inhoud is: als de inhoud hoge TTL heeft (time-to-live) / verlopen waarden en wordt regelmatig geopend zodat deze blijft populaire in cache, wordt de grote meerderheid van de belasting wordt verwerkt door het CDN. Een typische goede Cachetreffer ratio is en 90%, wat betekent dat er minder dan 10% van aanvragen van clients hebben om terug te keren naar de oorsprong, die voor een cache ontbreekt of het object vernieuwen.

- Het aantal knooppunten moet het object te laden: telkens wanneer een knooppunt wordt een object vanuit de oorsprong geladen, worden er accountkosten in een factureerbare transactie. Als gevolg hiervan resulteert meer globale inhoud (toegankelijk via meer knooppunten) in meer factureerbare transacties.

- De invloed van de TTL: een hogere TTL-waarde voor een object betekent dat deze moeten worden opgehaald uit de oorsprong minder vaak. Het betekent ook dat in cache-clients, zoals browsers, het object langer, waardoor de transacties naar de CDN kan verminderen.

## <a name="how-do-i-manage-my-costs-most-effectively"></a>Hoe kan ik mijn kosten meest effectief beheren?
De langste TTL mogelijk instellen op uw inhoud. 

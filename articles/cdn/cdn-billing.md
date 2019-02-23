---
title: Inzicht in facturering voor Azure CDN | Microsoft Docs
description: Deze Veelgestelde vragen wordt beschreven hoe de facturering voor Azure CDN werkt.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: magattus
ms.openlocfilehash: af8e57f39b5b83b1d1be09c29d8b6eb5d49c7b6c
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56735797"
---
# <a name="understanding-azure-cdn-billing"></a>Inzicht in facturering voor Azure CDN

Deze Veelgestelde vragen over de facturering structuur voor inhoud die wordt gehost door Azure Content Delivery Network (CDN) wordt beschreven.

## <a name="what-is-a-billing-region"></a>Wat is er een regio voor facturering?
Een regio voor facturering is een geografische gebied gebruikt om te bepalen welk tarief wordt in rekening gebracht voor het leveren van objecten uit Azure CDN. De huidige factureringsperiode zones en hun regio's zijn als volgt:

- Zone 1: Noord-Amerika, Europa, Midden-Oosten en Afrika

- Zone 2: Azië en Stille Oceaan (inclusief Japan)

- Zone 3: Zuid-Amerika

- Zone 4: Australië en Nieuw-Zeeland

- Zone 5: India

Zie voor meer informatie over point-of-presence (POP)-regio's [Azure CDN POP-locaties op regio](https://docs.microsoft.com/azure/cdn/cdn-pop-locations). Bijvoorbeeld, een pop-server zich bevindt in Mexico is in de regio Noord-Amerika en daarom is opgenomen in zone 1. 

Zie voor meer informatie over de prijzen voor Azure CDN [prijzen van Content Delivery Network](https://azure.microsoft.com/pricing/details/cdn/).

## <a name="how-are-delivery-charges-calculated-by-region"></a>Hoe worden de levering van kosten per regio berekend?
De regio voor facturering van Azure CDN is gebaseerd op de locatie van de bronserver leveren van de inhoud voor de eindgebruiker. De bestemming (fysieke locatie) van de client wordt niet beschouwd als de regio voor facturering.

Bijvoorbeeld, als een gebruiker zich in Mexico Hiermee wordt een aanvraag en deze aanvraag wordt uitgevoerd door een server die zich in een POP-Verenigde Staten vanwege peering of verkeer voorwaarden, is de regio voor facturering de Verenigde Staten.

## <a name="what-is-a-billable-azure-cdn-transaction"></a>Wat is een factureerbare Azure CDN-transacties?
Een HTTP (S)-aanvraag die op het CDN eindigt is een factureerbare gebeurtenis, waaronder alle antwoordtypen: geslaagd, mislukt of andere. Verschillende reacties kunnen echter een ander verkeer bedragen genereren. Bijvoorbeeld, *304 niet gewijzigd* en andere alleen-header antwoorden genereren weinig verkeer, omdat ze een kleine header-reactie; op dezelfde manier foutberichten (bijvoorbeeld *404 niet gevonden*) zijn factureerbaar maar vanwege de kleine responslading een kleine kosten.

## <a name="what-other-azure-costs-are-associated-with-azure-cdn-use"></a>Welke andere Azure-kosten zijn gekoppeld aan Azure CDN gebruiken?
Met behulp van Azure CDN ook leidt tot enige kosten voor het gebruik van de services die worden gebruikt als de oorsprong van de objecten. Deze kosten zijn doorgaans een fractie van de totale kosten van de CDN-gebruik.

Als u voor de inhoud van uw Azure Blob-opslag als de oorsprong gebruikt, worden er ook de volgende kosten voor opslag voor de cache opvullingen:

- Werkelijke GB gebruikt: De werkelijke opslag van uw bronobjecten.

- Overdrachten in GB: De hoeveelheid gegevens overgebracht naar het vullen van de caches van het CDN.

- Transacties: Indien nodig om in te vullen van de cache.

Zie voor meer informatie over de facturering voor Azure Storage, [inzicht in Azure Storage Billing-bandbreedte, transacties en capaciteit](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

Als u *leveren van services die worden gehost*, u kosten in rekening gebracht als volgt:

- Azure-rekentijd: De compute-exemplaren die als de oorsprong fungeren.

- Azure-rekenen overdracht: De gegevens worden overgedragen van de compute-exemplaren om in te vullen van de Azure CDN-cache.

Als de client gebruikt de byte-bereikaanvragen (ongeacht de oorsprong service), de volgende overwegingen zijn van toepassing:

- Een *bytebereik aanvraag* is een factureerbare transactie op het CDN. Wanneer een client een bytebereik aanvraag uitgeeft, wordt deze aanvraag is voor een subset (bereik) van het object. Het CDN reageert met slechts een gedeeltelijke gedeelte van de inhoud die is aangevraagd. Het gedeeltelijke antwoord is een factureerbare transactie en de overdracht is beperkt tot de grootte van het bereik antwoord (plus headers).

- Wanneer een aanvraag is ontvangen voor alleen die deel uitmaken van een object (door het opgeven van een byte-range-header), kan het CDN het gehele object ophalen in de cache. Als gevolg hiervan, zelfs als de factureerbare transactie uit het CDN een gedeeltelijk antwoord is, de factureerbare transactie uit de oorsprong kan betrekking hebben op volledige grootte van het object.

## <a name="how-much-transfer-activity-occurs-to-support-the-cache"></a>Hoeveel overdrachtactiviteit plaatsvindt voor de ondersteuning van de cache?
Telkens wanneer die een CDN pop-locatie moet invullen van de cache, wordt een aanvraag naar de oorsprong van het object in de cache worden opgeslagen. Als gevolg hiervan is de oorsprong leidt tot een factureerbare transactie op elke cachemisser. Het aantal missers in cache is afhankelijk van een aantal factoren:

- Hoe gecachet kan worden de inhoud is: Als de inhoud hoge TTL heeft (time-to-live) / vervaldatum waardeert en is wordt regelmatig geopend, zodat deze blijft populair zijn in de cache en vervolgens het overgrote deel van de belasting verwerkt door het CDN. Een typische goede Cachetreffer verhouding is ook dat meer dan 90%, wat betekent dat er minder dan 10% van de aanvragen van clients hebben om terug te keren naar de oorsprong, voor een cache ontbreekt of het object vernieuwen.

- Het aantal knooppunten moeten het object te laden: Telkens wanneer die een knooppunt een object van de oorsprong laadt, worden er accountkosten een factureerbare transactie. Als gevolg hiervan resulteert meer globale inhoud (toegankelijk vanaf meer knooppunten) in meer gefactureerde transacties.

- De invloed van de TTL-waarde: Een hogere TTL-waarde voor een object betekent dat nodig is om te worden opgehaald uit de oorsprong minder vaak. Het betekent ook clients zoals browsers, kunnen het object langer, waardoor de transacties naar het CDN kan verminderen in de cache.

## <a name="how-do-i-manage-my-costs-most-effectively"></a>Hoe kan ik mijn kosten meest effectief beheren?
De langste TTL mogelijk instellen voor uw inhoud. 

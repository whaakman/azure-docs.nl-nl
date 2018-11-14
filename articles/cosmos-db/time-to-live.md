---
title: Gegevens in Azure Cosmos DB met Time to Live laten verlopen
description: TTL-waarde biedt Microsoft Azure Cosmos DB de mogelijkheid om documenten automatisch verwijderd uit het systeem na een bepaalde periode.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: mjbrown
ms.openlocfilehash: c08c171e3a95b0d0f408660a7ec9021ca0323fbd
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621270"
---
# <a name="time-to-live-for-azure-cosmos-db-data"></a>Time to Live voor Azure Cosmos DB-gegevens

Met 'Time to Live' of een TTL-waarde biedt Azure Cosmos DB de mogelijkheid items om automatisch te verwijderen uit een container na een bepaalde periode. Standaard kunt u tijd te bevinden zich op het niveau van de container en de waarde op basis van de per-item overschrijven instellen. Nadat de TTL-waarde is ingesteld op een container of op het itemniveau van een, worden Azure Cosmos DB automatisch deze items verwijderd na de tijd sinds de tijd dat ze het laatst zijn gewijzigd. Waarde time to live is geconfigureerd in een paar seconden. Wanneer u TTL configureert, worden de verlopen items op basis van de TTL-waarde, in tegenstelling tot een verwijderbewerking dat expliciet is uitgegeven door de clienttoepassing automatisch verwijderd door het systeem.

## <a name="time-to-live-for-containers-and-items"></a>Time to live voor containers en objecten

De waarde time to live in seconden wordt ingesteld en dit wordt geïnterpreteerd als een delta vanaf het moment waarop een item voor het laatst is gewijzigd. U kunt de time to live van een container of een item in de container van instellen:

1. **Time to Live voor een container** (ingesteld met behulp van `DefaultTimeToLive`):

   - Indien ontbrekend (of is ingesteld op null), items worden niet automatisch verlopen.

   - Als het aanwezig zijn en de waarde is ingesteld op '-1', is gelijk aan oneindige – items verloopt niet standaard.

   - Als aanwezig zijn en de waarde is ingesteld op een nummer ("n"): verlopen items "n" aantal seconden na de laatste keer is gewijzigd.

2. **Time to Live op een item** (ingesteld met behulp van `TimeToLive`):

   - Deze eigenschap geldt alleen als `DefaultTimeToLive` aanwezig is en deze niet is ingesteld op null voor de bovenliggende container.

   - Indien aanwezig, dit vervangt de `DefaultTimeToLive` waarde van de bovenliggende container.

## <a name="time-to-live-configurations"></a>Tijd voor Live-configuraties

* Als de TTL-waarde is ingesteld op "n" voor een container, klikt u vervolgens verloopt de items in deze container na n seconden.  Er zijn items in dezelfde container die hun eigen tijd hebt om live, ingesteld op-1 (indicatie dat ze niet verlopen) of als sommige items waarvoor time to live instellen met een ander nummer worden genegeerd, deze items verloopt op basis van de geconfigureerde TTL-waarde. 

* Als de TTL is niet ingesteld voor een container, heeft geen effect met time to live op een item in deze container. 

* Als de TTL voor een container is ingesteld op-1, een item in deze container met time to live is ingesteld op n, verloopt na n seconden en overige items verloopt niet. 

Verwijderen van objecten op basis van de TTL is gratis. Er zijn geen extra kosten verbonden (dat wil zeggen, er zijn geen extra ru's worden verbruikt) wanneer het item wordt verwijderd als gevolg van de TTL is verlopen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het configureren van de tegel om Live in de volgende artikelen:

* [Time to Live configureren](how-to-time-to-live.md)

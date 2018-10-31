---
title: Schaal doorvoer in Azure Cosmos DB
description: Dit artikel wordt beschreven hoe Azure Cosmos DB schaalt de doorvoer elastisch
services: cosmos-db
author: dharmas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: adbac964e6654e16f6c405b9a5b8669326583f3e
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244155"
---
# <a name="scaling-throughput-in-azure-cosmos-db"></a>Schaal doorvoer in Azure Cosmos DB

In Azure Cosmos DB, ingerichte doorvoer wordt weergegeven als aanvraag eenheden per seconde (RU/s, plural: ru's). RU's meten van de kosten voor zowel uit lees- en schrijfbewerkingen op basis van uw Cosmos-container.

![Aanvraageenheden](./media/scale-throughput/figure1.png)

U kunt ru's op een Cosmos-container of een Cosmos-database inrichten. Voor een container ingerichte ru's zijn uitsluitend beschikbaar voor bewerkingen die worden uitgevoerd op die container. Voor een database ingerichte ru's worden gedeeld tussen alle containers in die database (met uitzondering van alle containers met uitsluitend toegewezen ru's).

Voor het schalen van doorvoer elastisch omhoog of omlaag, kunt u vergroten of verkleinen van de ingerichte RU/s op elk gewenst moment. Voor meer informatie, Zie hoe kan ik inrichten doorvoer en flexibel schalen Cosmos-containers en databases. U kunt voor het wereldwijd schalen van doorvoer, toevoegen of verwijderen van regio's op uw Cosmos-account op elk gewenst moment. Zie voor meer informatie, procedures voor het toevoegen of verwijderen van regio's aan uw Cosmos-account. Meerdere regio's koppelen aan een Cosmos-account is het belangrijk is in veel scenario's voor een lage latentie en [hoge beschikbaarheid](high-availability.md) over de hele wereld.

## <a name="throughput-scaling-details"></a>Doorvoer vergroten/verkleinen details

Als u R ru's op een Cosmos-container (of database) inricht, Cosmos DB zorgt ervoor dat R ru's zijn beschikbaar in *elke* van de regio's die zijn gekoppeld aan uw Cosmos-account. Telkens wanneer die u een nieuwe regio aan uw Cosmos-account toevoegen, inricht Cosmos DB automatisch R ru's in de toegevoegde regio. De bewerkingen die worden uitgevoerd op basis van uw Cosmos-container worden op te halen van R ru's in elk van de regio's die zijn gekoppeld aan uw Cosmos-account gegarandeerd. U kunt selectief ru's toewijzen aan een specifieke regio - het voor een Cosmos-container (of de database) ingerichte ru's worden ingericht voor alle regio's die zijn gekoppeld aan uw Cosmos-account.

Ervan uitgaande dat een Cosmos-container is geconfigureerd met R ru's en er zijn N regio's die zijn gekoppeld aan de Cosmos-account, klikt u vervolgens:

- Als de Cosmos-account is geconfigureerd met een enkele schrijfregio, het totale aantal ru's beschikbaar voor de container wereldwijd R = x N.
- Als de Cosmos-account is geconfigureerd met meerdere schrijfregio's, het totale aantal ru's beschikbaar voor de container wereldwijd R = x (N + 1). De extra R-ru's worden automatisch ingericht op update veroorzaakt een conflict en anti entropie verkeer in de regio's.

Uw eigen keuze aan [consistentiemodel](consistency-levels.md) ook van invloed op de doorvoer. U kunt ongeveer 2 x leesdoorvoer ophalen voor de sessie, consistent voorvoegsel en uiteindelijke consistentie in vergelijking met gebonden veroudering of sterke consistentie.

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u informatie over het configureren van doorvoer met behulp van het volgende artikel:

* [Ophalen en instellen van de doorvoer voor containers en -databases](set-throughput.md) 


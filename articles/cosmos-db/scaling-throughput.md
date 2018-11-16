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
ms.openlocfilehash: 597d94ce7be077dda6571048b560c9414ceb35ff
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705239"
---
# <a name="scaling-throughput-in-azure-cosmos-db"></a>Schaal doorvoer in Azure Cosmos DB

In Azure Cosmos DB, ingerichte doorvoer wordt weergegeven als aanvraag eenheden per seconde (RU/s, plural: ru's). RU's meten van de kosten voor zowel uit lees- en schrijfbewerkingen op basis van de container van Cosmos zoals wordt weergegeven in de volgende afbeelding:

![Aanvraageenheden](./media/scale-throughput/figure1.png)

U kunt ru's op een Cosmos-container of een Cosmos-database inrichten. Voor een container ingerichte ru's zijn uitsluitend beschikbaar voor bewerkingen die worden uitgevoerd op die container. Voor een database ingerichte ru's worden gedeeld tussen alle containers in die database (met uitzondering van alle containers met uitsluitend toegewezen ru's).

Voor doorvoer elastisch schalen, kunt u vergroten of verkleinen van de ingerichte RU/s op elk gewenst moment. Zie voor meer informatie, [hoe kan ik inrichten doorvoer](set-throughput.md) en flexibel schalen Cosmos-containers en databases. U kunt voor het wereldwijd schalen van doorvoer, toevoegen of verwijderen van regio's op uw Cosmos-account op elk gewenst moment. Zie voor meer informatie, [toevoegen of verwijderen-regio's van uw Azure Cosmos DB-account](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Meerdere regio's koppelen aan een Cosmos-account is het belangrijk is in veel scenario's voor een lage latentie en [hoge beschikbaarheid](high-availability.md) over de hele wereld.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>hoe de ingerichte doorvoer is verdeeld over regio's

Als u 'R'-me voor een Cosmos-container (of database) inricht, Cosmos DB zorgt ervoor dat 'R' ru's zijn beschikbaar in *elke* regio die is gekoppeld aan uw Cosmos-account. Telkens wanneer die u een nieuwe regio aan uw account toevoegen, inricht Cosmos DB automatisch 'R' ru's in de toegevoegde regio. De bewerkingen die worden uitgevoerd op basis van uw Cosmos-container worden ophalen 'R' ru's in elke regio gegarandeerd. U kunt selectief ru's toewijzen aan een specifieke regio. Het voor een Cosmos-container (of de database) ingerichte ru's worden ingericht voor alle regio's die zijn gekoppeld aan uw Cosmos-account.

Ervan uitgaande dat een Cosmos-container is geconfigureerd met 'R' ru's en er zijn "n" regio's die zijn gekoppeld aan de Cosmos-account, klikt u vervolgens:

- Als de Cosmos-account is geconfigureerd met een enkele schrijfregio, het totale aantal ru's beschikbaar voor de container wereldwijd R = x N.

- Als de Cosmos-account is geconfigureerd met meerdere schrijfregio's, het totale aantal ru's beschikbaar voor de container wereldwijd R = x (N + 1). De extra R-ru's worden automatisch ingericht op update veroorzaakt een conflict en anti entropie verkeer in de regio's.

Uw eigen keuze aan [consistentiemodel](consistency-levels.md) ook van invloed op de doorvoer. U kunt ongeveer 2 x leesdoorvoer ophalen voor de sessie, consistent voorvoegsel en uiteindelijke consistentie in vergelijking met gebonden veroudering of sterke consistentie.

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u informatie over het configureren van doorvoer met behulp van het volgende artikel:

* [Ophalen en instellen van de doorvoer voor containers en -databases](set-throughput.md) 


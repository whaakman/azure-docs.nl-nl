---
title: Schaal doorvoer in Azure Cosmos DB
description: Dit artikel wordt beschreven hoe Azure Cosmos DB schaalt de doorvoer elastisch
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: f930b5c478cc880952b4559be4c6647b260efcf2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243494"
---
# <a name="globally-scale-provisioned-throughput"></a>Wereldwijd schalen van ingerichte doorvoer 

In Azure Cosmos DB, ingerichte doorvoer wordt weergegeven als aanvraag eenheden per seconde (RU/s of de meervoudsvorm ru's). RU's meten van de kosten voor zowel uit lees- en schrijfbewerkingen op basis van de container van Cosmos zoals wordt weergegeven in de volgende afbeelding:

![Aanvraageenheden](./media/scaling-throughput/request-unit-charge-of-read-and-write-operations.png)

U kunt ru's op een Cosmos-container of een Cosmos-database inrichten. Voor een container ingerichte ru's zijn uitsluitend beschikbaar voor de bewerkingen die worden uitgevoerd op die container. Voor een database ingerichte ru's worden gedeeld tussen alle containers in die database (met uitzondering van alle containers met uitsluitend toegewezen ru's).

Voor de ingerichte doorvoer elastisch schalen, kunt u vergroten of verkleinen van de ingerichte RU/s op elk gewenst moment. Zie voor meer informatie, [hoe kan ik inrichten doorvoer](set-throughput.md) en flexibel schalen Cosmos-containers en databases. U kunt voor het wereldwijd schalen van doorvoer, toevoegen of verwijderen van regio's van uw Cosmos-account op elk gewenst moment. Zie voor meer informatie, [regio's toevoegen/verwijderen uit uw databaseaccount](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Meerdere regio's koppelen aan een Cosmos-account is het belangrijk is in veel scenario's - te bereiken met lage latentie en [hoge beschikbaarheid](high-availability.md) over de hele wereld.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>hoe de ingerichte doorvoer is verdeeld over regio's

Als u inricht *'R'* ru's voor een Cosmos-container (of database), Cosmos DB zorgt u ervoor dat *'R'* ru's zijn beschikbaar in *elke* regio die is gekoppeld aan uw Cosmos-account. Telkens wanneer u een nieuwe regio aan uw account toevoegen, Cosmos DB automatisch inricht *'R'* ru's in de toegevoegde regio. De bewerkingen die worden uitgevoerd op basis van uw Cosmos-container worden gegarandeerd om op te halen *'R'* ru's in elke regio. U kunt selectief ru's toewijzen aan een specifieke regio. Het op een Cosmos-container (of de database) ingerichte ru's worden ingericht in alle regio's die zijn gekoppeld aan uw Cosmos-account.

Ervan uitgaande dat een Cosmos-container is geconfigureerd met *'R'* ru's en er zijn *"n"* regio's die zijn gekoppeld aan de Cosmos-account, klikt u vervolgens:

- Als de Cosmos-account is geconfigureerd met een enkele schrijfregio, het totale aantal ru's beschikbaar voor de container wereldwijd = *R* x *N*.

- Als de Cosmos-account is geconfigureerd met meerdere schrijfregio's, het totale aantal ru's beschikbaar voor de container wereldwijd = *R* x (*N*+ 1). De extra *R* ru's worden automatisch ingericht op update veroorzaakt een conflict en anti entropie verkeer in de regio's.

Uw eigen keuze aan [consistentiemodel](consistency-levels.md) ook van invloed op de doorvoer. Voor de meer beperkte consistentieniveaus kunt u ongeveer 2 x leesdoorvoer krijgen (bijvoorbeeld *sessie*, *consistent voorvoegsel* en *uiteindelijke* consistentie) vergeleken met sterkere consistentieniveaus (bijvoorbeeld *gebonden veroudering* of *sterke* consistentie).

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u informatie over het configureren van doorvoer in een container of de database:

* [Ophalen en instellen van de doorvoer voor containers en -databases](set-throughput.md) 


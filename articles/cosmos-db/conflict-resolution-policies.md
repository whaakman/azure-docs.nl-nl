---
title: Conflict resolutie typen en resolutie beleidsregels in Azure Cosmos DB
description: In dit artikel beschrijft de categorieën van conflicten en het conflict resolutie beleidsregels in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 4af3f4c60f186c34d0f5522461ba70c68b59d1ab
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54033890"
---
# <a name="conflict-types-and-resolution-policies"></a>Conflicttypen en oplossingsbeleid

Een conflict veroorzaakt en conflictoplossing beleidsregels zijn van toepassing als uw Azure Cosmos DB-account is geconfigureerd met meerdere regio's worden geschreven.

Voor Azure Cosmos DB-accounts die zijn geconfigureerd met meerdere regio's voor schrijven, worden Bijwerkconflicten kunnen optreden wanneer schrijvers hetzelfde item in meerdere regio's tegelijkertijd bijwerken. Update veroorzaakt een conflict in de volgende drie categorieën ingedeeld:

* **Invoegen van conflicten**: Deze conflicten kunnen optreden wanneer een toepassing tegelijkertijd twee of meer items met dezelfde unieke index van twee of meer regio's voegt. Een voorbeeld: dit conflict optreden met een ID-eigenschap. Alle schrijfbewerkingen mogelijk in eerste instantie mislukt in hun respectieve lokale regio's. Maar op basis van het conflict resolutie beleid dat u kiest, slechts één item met de ID van de oorspronkelijke is definitief doorgevoerd.

* **Vervang conflicten**: Deze conflicten kunnen optreden wanneer een item tegelijk uit twee of meer regio's wordt bijgewerkt.

* **Verwijderen van conflicten**: Deze conflicten kunnen optreden wanneer een toepassing tegelijkertijd wordt een item wordt verwijderd van de ene regio en wordt deze vanaf een andere regio bijgewerkt.

## <a name="conflict-resolution-policies"></a>Beleid voor conflictoplossing

Azure Cosmos DB biedt een flexibel beleid-gebaseerd mechanisme om op te lossen Bijwerkconflicten. U kunt kiezen uit twee conflict resolutie beleidsregels op een Azure Cosmos DB-container:

- **Laatste schrijfbewerking Wins (LWW)**: Dit beleid resolutie gebruikt standaard een eigenschap van het systeem gedefinieerde timestamp. Deze gebaseerd op het clock voor tijdsynchronisatie protocol. Als u de Azure Cosmos DB SQL API gebruikt, kunt u een andere aangepaste numerieke eigenschap moet worden gebruikt voor conflictoplossing opgeven. Een aangepaste numerieke eigenschap wordt ook het pad van de oplossing conflict genoemd. 

  Als twee of meer items conflict op invoegen of vervangen, wordt het item met de hoogste waarde voor het pad van de oplossing conflict de winnaar. Als meerdere items dezelfde numerieke waarde voor het pad van de oplossing conflict hebben, bepaalt het systeem de winnaar. Alle regio's worden te convergeren met een één winnaar en eindigen met dezelfde versie van de toegezegde item gegarandeerd. Wanneer er conflicten zijn betrokken, verwijder de verwijderde versie altijd wins boven invoegen of vervangen van conflicten. Dit resultaat optreedt, ongeacht de waarde van het conflict resolutie-pad.

  > [!NOTE]
  > Laatste schrijven Wins, is het standaardbeleid voor de oplossing van conflicten. Het is beschikbaar voor SQL, Azure Cosmos DB Table, MongoDB, Cassandra en Gremlin-API-accounts.

  Zie voor meer informatie, [voorbeelden die gebruikmaken van LWW resolutie beleidsregels conflicteren](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Aangepaste**: Dit beleid van de oplossing is ontworpen voor toepassingsspecifieke semantiek voor afstemming van conflicten. Als u dit beleid voor uw Azure Cosmos DB-container instelt, moet u ook een samenvoegen opgeslagen procedure registreren. Deze procedure wordt automatisch geactiveerd wanneer er conflicten zijn gedetecteerd in een databasetransactie op de server. Het systeem biedt exact één keer voor het uitvoeren van een procedure samenvoegen als onderdeel van het protocol toezegging garanderen.  

  Er zijn twee punten om te weten als u de container met de aangepaste resolutie-optie configureren. Als u een failover naar het registreren van een merge-procedure voor de container of de samenvoegen-procedure een uitzondering tijdens runtime genereert, worden de conflicten worden geschreven naar de feed conflicten. Uw toepassing wordt vervolgens moet de conflicten in de feed conflicten handmatig oplossen. Zie voor meer informatie, [voorbeelden van hoe u het beleid van de oplossing op maat en het gebruik van de feed conflicten](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

  > [!NOTE]
  > Aangepaste conflict resolutie beleid is alleen beschikbaar voor SQL-API-accounts.

## <a name="next-steps"></a>Volgende stappen

Informatie over het configureren van conflict resolutie beleidsregels. Zie de volgende artikelen:

* [Gebruik de LWW conflict resolutie beleid](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Gebruik de aangepaste conflict resolutie beleid](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Gebruik de conflicten feed](how-to-manage-conflicts.md#read-from-conflict-feed)

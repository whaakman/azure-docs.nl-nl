---
title: Conflict resolutie typen en resolutie beleidsregels met meerdere schrijven regio's in Azure Cosmos DB
description: In dit artikel beschrijft de categorieën van conflicten en het conflict resolutie beleidsregels in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/24/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 24201cfd657d4f23eb962b7407ed20262d780cf7
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407416"
---
# <a name="conflict-types-and-resolution-policies"></a>Conflicttypen en oplossingsbeleid

Een conflict veroorzaakt en conflictoplossing beleidsregels zijn van toepassing als uw Azure Cosmos DB-account is geconfigureerd met meerdere regio's worden geschreven.

Voor Azure Cosmos-accounts die zijn geconfigureerd met meerdere regio's voor schrijven, worden Bijwerkconflicten kunnen optreden wanneer schrijvers hetzelfde item in meerdere regio's tegelijkertijd bijwerken. Update veroorzaakt een conflict kunnen zijn van de volgende drie typen:

* **Invoegen van conflicten**: Deze conflicten kunnen optreden wanneer een toepassing tegelijkertijd twee of meer items met dezelfde unieke index wordt ingevoegd in twee of meer regio's. Bijvoorbeeld, optreden dit conflict met een ID-eigenschap.

* **Vervang conflicten**: Deze conflicten kunnen optreden wanneer een toepassing hetzelfde item tegelijk in twee of meer regio's werkt.

* **Verwijderen van conflicten**: Deze conflicten kunnen optreden wanneer een toepassing Hiermee verwijdert u een item in één regio tegelijk en updates in een andere regio.

## <a name="conflict-resolution-policies"></a>Beleid voor conflictoplossing

Azure Cosmos DB biedt een flexibel op beleid gebaseerde mechanisme om op te lossen kunt oplossen. U kunt kiezen uit twee conflict resolutie beleidsregels op een Azure Cosmos-container:

- **Laatste schrijfbewerking Wins (LWW)**: Dit beleid resolutie gebruikt standaard een eigenschap van het systeem gedefinieerde timestamp. Deze gebaseerd op het clock voor tijdsynchronisatie protocol. Als u de SQL-API gebruikt, kunt u een andere aangepaste numerieke eigenschap (bijvoorbeeld uw eigen begrip van een tijdstempel) om te worden gebruikt voor conflictoplossing opgeven. Een aangepaste numerieke eigenschap wordt ook wel de *conflict resolutie pad*. 

  Als twee of meer items conflict op invoegen of vervangen, wordt het item met de hoogste waarde voor het pad van de oplossing conflict de winnaar. Als meerdere items dezelfde numerieke waarde voor het pad van de oplossing conflict hebben, bepaalt het systeem de winnaar. Alle regio's worden te convergeren met een één winnaar en eindigen met dezelfde versie van de toegezegde item gegarandeerd. Wanneer er conflicten zijn betrokken, verwijder de verwijderde versie altijd wins boven invoegen of vervangen van conflicten. Dit resultaat optreedt, ongeacht wat de waarde van het pad van de oplossing conflict is.

  > [!NOTE]
  > Laatste schrijven Wins, is het standaardbeleid voor de oplossing van conflicten. Het is beschikbaar voor de volgende API's: SQL, MongoDB, Cassandra, Gremlin en tabel.

  Zie voor meer informatie, [voorbeelden die gebruikmaken van LWW resolutie beleidsregels conflicteren](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Aangepast**: Dit beleid van de oplossing is ontworpen voor toepassingsspecifieke semantiek voor afstemming van conflicten. Als u dit beleid voor uw Azure Cosmos-container instelt, moet u ook registreren een *samenvoegen van opgeslagen procedure*. Deze procedure wordt automatisch geactiveerd wanneer er conflicten zijn gedetecteerd in een databasetransactie op de server. Het systeem biedt exact één keer voor het uitvoeren van een procedure samenvoegen als onderdeel van het protocol toezegging garanderen.  

  Als u uw container met de optie oplossing op maat configureren, en u een failover naar het registreren van een merge-procedure voor de container of de samenvoegen-procedure een uitzondering tijdens runtime genereert, de conflicten worden geschreven naar de *conflicten feed*. Uw toepassing wordt vervolgens moet de conflicten in de feed conflicten handmatig oplossen. Zie voor meer informatie, [voorbeelden van hoe u het beleid van de oplossing op maat en het gebruik van de feed conflicten](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

  > [!NOTE]
  > Aangepaste conflict resolutie beleid is alleen beschikbaar voor SQL-API-accounts.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het configureren van conflict resolutie beleidsregels:

* [Meerdere masters in uw toepassingen configureren](how-to-multi-master.md)
* [Het gebruik van de LWW conflict resolutie beleid](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Het gebruik van de aangepaste conflict resolutie beleid](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Het lezen van de feed veroorzaakt een conflict](how-to-manage-conflicts.md#read-from-conflict-feed)

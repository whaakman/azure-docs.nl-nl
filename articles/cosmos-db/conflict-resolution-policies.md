---
title: Conflict oplossings typen en oplossings beleid met meerdere schrijf regio's in Azure Cosmos DB
description: In dit artikel worden de conflict categorieën en beleids regels voor conflict oplossing in Azure Cosmos DB beschreven.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 45b7257f67be8ba5c134717d73488916056b7a7d
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68384206"
---
# <a name="conflict-types-and-resolution-policies"></a>Conflicttypen en oplossingsbeleid

Conflicten en beleids regels voor conflict oplossing zijn van toepassing als uw Azure Cosmos DB-account is geconfigureerd met meerdere schrijf regio's.

Voor Azure Cosmos-accounts die zijn geconfigureerd met meerdere schrijf regio's, kunnen update conflicten optreden wanneer schrijvers hetzelfde item in meerdere regio's gelijktijdig bijwerken. Update conflicten kunnen van de volgende drie typen zijn:

* **Conflicten bij invoegen**: Deze conflicten kunnen zich voordoen wanneer een toepassing tegelijkertijd twee of meer items invoegt met dezelfde unieke index in twee of meer regio's. Dit conflict kan bijvoorbeeld optreden met een ID-eigenschap.

* **Vervangings conflicten**: Deze conflicten kunnen zich voordoen wanneer een toepassing hetzelfde item tegelijkertijd in twee of meer regio's bijwerkt.

* **Conflicten verwijderen**: Deze conflicten kunnen zich voordoen wanneer een toepassing tegelijkertijd een item uit de ene regio verwijdert en dit in een andere regio bijwerkt.

## <a name="conflict-resolution-policies"></a>Beleid voor conflictoplossing

Azure Cosmos DB biedt een flexibel mechanisme op basis van beleid om schrijf conflicten op te lossen. U kunt kiezen uit twee beleids regels voor conflict oplossing in een Azure Cosmos-container:

- **Laatste write-WINS (LWW)** : Dit oplossings beleid maakt standaard gebruik van een door het systeem gedefinieerde tijds tempel eigenschap. Het is gebaseerd op het klok protocol met tijd synchronisatie. Als u de SQL-API gebruikt, kunt u een andere aangepaste numerieke eigenschap (bijvoorbeeld uw eigen begrip van een tijds tempel) opgeven die moet worden gebruikt voor het oplossen van conflicten. Een aangepaste numerieke eigenschap wordt ook wel het pad voor *conflict oplossing*genoemd. 

  Als er twee of meer items conflicteren bij het invoegen of vervangen van bewerkingen, wordt het item met de hoogste waarde voor het pad naar het oplossen van conflicten de winnaar. Het systeem bepaalt de winnaar als meerdere items dezelfde numerieke waarde hebben voor het pad van de conflict oplossing. Alle regio's zijn gegarandeerd een enkele winnaar te convergeren en eindigen op dezelfde versie van het doorgevoerde item. Wanneer er sprake is van conflicterende verwijderingen, wint de verwijderde versie altijd WINS over het invoegen of vervangen van conflicten. Dit resultaat is, ongeacht de waarde van het pad voor het oplossen van conflicten.

  > [!NOTE]
  > Laatste schrijf-WINS is het standaard beleid voor conflict oplossing. Deze is beschikbaar voor de volgende Api's: SQL, MongoDB, Cassandra, Gremlin en Table.

  Voor meer informatie, Zie [voor beelden die gebruikmaken van LWW-conflictoplossings beleid](how-to-manage-conflicts.md).

- **Aangepast**: Dit oplossings beleid is ontworpen voor door de toepassing gedefinieerde semantiek voor het afstemmen van conflicten. Wanneer u dit beleid instelt op de Azure Cosmos-container, moet u ook een *opgeslagen procedure voor samen voegen*registreren. Deze procedure wordt automatisch aangeroepen wanneer conflicten worden gedetecteerd onder een database transactie op de server. Het systeem biedt exact één keer voor het uitvoeren van een procedure samenvoegen als onderdeel van het protocol toezegging garanderen.  

  Als u de container configureert met de optie voor de aangepaste resolutie en u een samenvoeg procedure op de container niet registreert of als tijdens het samen voegen een uitzonde ring wordt gegenereerd tijdens runtime, worden de conflicten naar de *feed conflicten*geschreven. Uw toepassing moet vervolgens de conflicten in de feed conflicten hand matig oplossen. Voor meer informatie, Zie [voor beelden van het gebruik van het beleid voor aangepaste resolutie en het gebruik van de feed conflicten](how-to-manage-conflicts.md).

  > [!NOTE]
  > Het aangepaste beleid voor conflict oplossing is alleen beschikbaar voor SQL-API-accounts.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het configureren van beleids regels voor conflict oplossing:

* [Multi-Master configureren in uw toepassingen](how-to-multi-master.md)
* [Conflict oplossings beleid beheren](how-to-manage-conflicts.md)
* [De invoer van de conflicten lezen](how-to-manage-conflicts.md#read-from-conflict-feed)

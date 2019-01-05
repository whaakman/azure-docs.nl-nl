---
title: Azure Cosmos DB-gegevens herstellen vanaf een back-up
description: In dit artikel wordt beschreven hoe u Azure Cosmos DB-gegevens herstellen vanaf een back-up, contact opnemen met ondersteuning van Azure voor het herstellen van gegevens, de stappen om uit te voeren nadat de gegevens is hersteld.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: e21a3ae23ca6a856c9524e52e80f33362f53cae1
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54040843"
---
# <a name="how-to-restore-azure-cosmos-db-data-from-a-backup"></a>Azure Cosmos DB-gegevens herstellen vanaf een back-up

Als u per ongeluk de database of een container verwijdert, kunt u [een ondersteuningsticket]( https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) of [contact opnemen met ondersteuning voor Azure]( https://azure.microsoft.com/support/options/) om terug te zetten van de gegevens van automatische online back-ups. Azure-ondersteuning is alleen beschikbaar voor geselecteerde abonnementen zoals **Standard**, ** Developer en hoger is dan deze abonnementen. Azure-ondersteuning is niet beschikbaar bij **Basic** plan. Zie voor meer informatie over de verschillende ondersteuningsplannen, de [ondersteuningsabonnementen voor Azure](https://azure.microsoft.com/support/plans/) pagina. 

Voor het herstellen van een momentopname van de back-up van een specifieke Azure Cosmos DB is vereist dat de gegevens beschikbaar voor de duur van de back-cyclus die momentopname te maken zijn.

## <a name="request-a-restore"></a>Aanvraag voor een herstelpunt

U hebt de volgende informatie voordat u aanvraagt van een herstelpunt:

* Uw abonnement-ID gereed hebben.

* Op basis van hoe uw gegevens per ongeluk is verwijderd of gewijzigd, moet u bereid om aanvullende informatie. Het wordt aanbevolen dat u de beschikbare informatie vooruit hebt om te minimaliseren, de back-en-weer die schadelijk zijn in sommige gevallen voor het gevoelige van tijd.

* Als het gehele Azure Cosmos DB-account wordt verwijderd, moet u de naam van het verwijderde account op te geven. Als u een ander account met dezelfde naam als het account verwijderd maakt, deelt u die met het ondersteuningsteam, omdat de service helpt om te bepalen het juiste account om te kiezen. Het verdient bestand verschillende ondersteuningstickets voor elke account verwijderd omdat de verwarring van de status van het herstel wordt.

* Als een of meer databases zijn verwijderd, moet u opgeven van de Azure Cosmos-account, evenals de namen van de Azure Cosmos-database en opgeven of een nieuwe database met dezelfde naam bestaat.

* Als een of meer containers zijn verwijderd, moet u de naam van de Azure Cosmos-account, databasenamen en de namen van containers opgeven. En geef als een container met dezelfde naam bestaat.

Wanneer gegevensbeschadiging optreedt en de documenten in een container worden gewijzigd of verwijderd, **zo snel mogelijk verwijderen van de container**. Door het verwijderen van de container, kunt u Azure Cosmos DB van het overschrijven van de back-ups voorkomen. Als de verwijdering niet mogelijk om een bepaalde reden is, moet u zo snel mogelijk een ticket indienen. Naast de naam van de Azure Cosmos-account, databasenamen, namen van verzamelingen, moet u het punt in tijd waarop de gegevens kunnen worden hersteld. Het is belangrijk om te worden zo nauwkeurig mogelijk om te bepalen van de beste beschikbare back-ups op dat moment. Het is ook belangrijk op te geven van de tijd in UTC. 

De volgende schermafbeelding ziet u hoe u een ondersteuningsaanvraag voor een container(collection/graph/table) om gegevens te herstellen met behulp van Azure portal maken. Geef aanvullende informatie zoals het type gegevens, het doel van het herstel tijd wanneer de gegevens naar help ons prioriteren van de aanvraag is verwijderd.

![Maak een back-ondersteuningsaanvraag met behulp van Azure portal](./media/how-to-backup-and-restore/backup-support-request-portal.png)

## <a name="post-restore-actions"></a>Acties na herstellen

Nadat u de gegevens herstelt, krijgt u een melding over de naam van het nieuwe account (deze bevindt zich doorgaans in de indeling `<original-name>-restored1`) en het tijdstip waarop het account is hersteld om. De herstelde account heeft de dezelfde ingerichte doorvoer, indexering en deze zich in dezelfde regio bevinden als de oorspronkelijke account. Een gebruiker die de beheerder van abonnement of een coadmin ziet het herstelde account.

Nadat de gegevens is hersteld, moet u controleren en valideren van de gegevens in de herstelde account en zorg ervoor dat deze bevat de versie die u verwacht. Als alles er goed uitziet, moet u de gegevens migreren naar uw oorspronkelijke account met [Azure Cosmos DB-wijzigingenfeed](change-feed.md) of [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md).

Het wordt aanbevolen dat u de container of de database verwijdert onmiddellijk na de migratie van de gegevens. Als u de herstelde databases of containers niet verwijderen, ze worden kosten in rekening gebracht voor aanvraageenheden-, opslag- en uitgaand verkeer.

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u meer informatie over het migreren van de gegevens terug naar uw oorspronkelijke account met behulp van de volgende artikelen:

* Om te maken van een herstelpunt aanvragen, contact op met ondersteuning voor Azure, [bestand een ticket van de Azure-portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
* [Gebruik Cosmos DB-wijzigingenfeed](change-feed.md) om gegevens te verplaatsen met Azure Cosmos DB.

* [Gebruik Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) om gegevens te verplaatsen met Azure Cosmos DB.

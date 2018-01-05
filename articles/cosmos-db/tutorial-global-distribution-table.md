---
title: Zelfstudie voor Azure DB Cosmos globale distributie voor tabel-API | Microsoft Docs
description: Informatie over het instellen van Azure DB die Cosmos globale distributie op basis van de tabel-API.
services: cosmos-db
keywords: globale distributie, tabel
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 40c0bfe913e1396194de00cf6fa1d1ff823b1d0e
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Het instellen van Azure DB die Cosmos globale distributie op basis van de tabel-API

In dit artikel bevat informatie over de volgende taken: 

> [!div class="checklist"]
> * Globale distributie op basis van de Azure-portal configureren
> * Configureren globale distributie met behulp van de [tabel-API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Verbinding maken met een voorkeursregio met behulp van de tabel-API

Om te profiteren van [globale distributie](distribute-data-globally.md), clienttoepassingen de voorkeur geordende lijst met regio's moeten worden gebruikt voor het document bewerkingen uitvoeren kunnen opgeven. Dit kan worden gedaan door het instellen van de [TableConnectionPolicy.PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.table.tableconnectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_CosmosDB_Table_TableConnectionPolicy_PreferredLocations) eigenschap. De SDK van Azure Cosmos DB tabel API kiest het beste endpoint om te communiceren met op basis van de accountconfiguratie, huidige regionale beschikbaarheid en de opgegeven lijst.

De PreferredLocations moet een door komma's gescheiden lijst met aanbevolen (multihoming) locaties bevatten voor leesbewerkingen. Elk clientexemplaar kunt u een subset van deze gebieden in de gewenste volgorde voor lage latentie leesbewerkingen opgeven. De regio's moeten de namen van hun [weergavenamen](https://msdn.microsoft.com/library/azure/gg441293.aspx), bijvoorbeeld `West US`.

Alle leesbewerkingen worden verzonden naar de eerste beschikbare regio in de lijst PreferredLocations. Als de aanvraag is mislukt, zal de client mislukken omlaag in de lijst voor de volgende regio, enzovoort.

De SDK probeert te lezen uit de regio's opgegeven in PreferredLocations. Dus bijvoorbeeld kunnen als het databaseaccount beschikbaar in drie gebieden is, maar de client alleen twee van de niet-schrijven regio's voor PreferredLocations bevat, klikt u vervolgens geen leesbewerkingen worden geleverd buiten de regio schrijven, zelfs in het geval van failover.

De SDK verzendt automatisch alle schrijfbewerkingen naar de huidige schrijven regio.

Als de eigenschap PreferredLocations niet is ingesteld, worden alle aanvragen van de huidige schrijven regio worden geleverd.

Dat is, die in deze zelfstudie is voltooid. U kunt informatie over het beheren van de consistentie van uw account globaal gerepliceerde door te lezen [consistentieniveaus in Azure Cosmos DB](consistency-levels.md). En Zie voor meer informatie over hoe globale databasereplicatie in Azure Cosmos DB werkt, [gegevens globaal met Azure Cosmos DB distribueren](distribute-data-globally.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Globale distributie op basis van de Azure-portal configureren
> * Globale distributie op basis van de API's van Azure Cosmos DB tabel configureren


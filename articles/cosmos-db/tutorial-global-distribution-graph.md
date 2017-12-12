---
title: Azure DB Cosmos algemene distributie-zelfstudie voor Graph API | Microsoft Docs
description: Informatie over het instellen van Azure DB die Cosmos globale distributie op basis van de Graph API.
services: cosmos-db
keywords: globale distributie, grafiek, gremlin
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: 12e1ab5f57d217537ba14183500efb099985ff1e
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-graph-api"></a>Het instellen van Azure DB die Cosmos globale distributie op basis van de Graph API

In dit artikel, laten we zien hoe de Azure portal gebruiken om setup Azure Cosmos DB globale distributie en maak verbinding met de Graph-API (preview).

In dit artikel bevat informatie over de volgende taken: 

> [!div class="checklist"]
> * Globale distributie op basis van de Azure-portal configureren
> * Configureren globale distributie met behulp van de [Graph API's](graph-introduction.md) (preview)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a>Verbinding maken met een voorkeursregio met behulp van de API van de grafiek met de .NET SDK

De Graph API wordt weergegeven als een extensiebibliotheek boven op de SQL-API.

Om te profiteren van [globale distributie](distribute-data-globally.md), clienttoepassingen de voorkeur geordende lijst met regio's moeten worden gebruikt voor het document bewerkingen uitvoeren kunnen opgeven. Dit kan worden gedaan door het instellen van het verbindingsbeleid voor de. Op basis van de Azure DB die Cosmos-accountconfiguratie, huidige regionale beschikbaarheid en de voorkeur lijst is opgegeven, wordt het meest optimale eindpunt door de SDK schrijven uitvoeren en leesbewerkingen gekozen.

Deze lijst voorkeur is opgegeven bij het initialiseren van een verbinding met de SDK's. De SDK's accepteren een optionele parameter 'PreferredLocations' is een geordende lijst met Azure-regio's.

* **Schrijft**: de SDK wordt automatisch verzonden naar alle schrijfbewerkingen naar de huidige regio schrijven.
* **Leest**: alle leesbewerkingen wordt verzonden naar de eerste beschikbare regio in de lijst PreferredLocations. Als de aanvraag is mislukt, zal de client mislukken omlaag in de lijst voor de volgende regio, enzovoort. De SDK's wordt alleen poging tot lezen van de gebieden die zijn opgegeven in PreferredLocations. Dus bijvoorbeeld kunnen als de Cosmos-DB-account beschikbaar in drie gebieden is, maar de client alleen twee van de niet-schrijven regio's voor PreferredLocations bevat, klikt u vervolgens geen leesbewerkingen worden geleverd buiten de regio schrijven, zelfs in het geval van failover.

De toepassing kunt controleren of de huidige schrijven endpoint en lezen eindpunt gekozen door de SDK door het controleren van de twee eigenschappen, WriteEndpoint en ReadEndpoint beschikbaar in de SDK-versie 1.8 en hoger. Als de eigenschap PreferredLocations niet is ingesteld, worden alle aanvragen van de huidige schrijven regio worden geleverd.

### <a name="using-the-sdk"></a>Met behulp van de SDK

In de .NET SDK, bijvoorbeeld de `ConnectionPolicy` parameter voor de `DocumentClient` constructor heeft een eigenschap genaamd `PreferredLocations`. Deze eigenschap kan worden ingesteld op een lijst met regionamen. De namen van de weergave voor [Azure-gebieden] [ regions] kan worden opgegeven als onderdeel van `PreferredLocations`.

> [!NOTE]
> De URL's voor de eindpunten niet beschouwd als lange levensduur constanten. De service kan deze op elk gewenst moment bijwerken. De SDK verwerkt deze wijziging automatisch.
>
>

```cs
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to Azure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

Dat is, die in deze zelfstudie is voltooid. U kunt informatie over het beheren van de consistentie van uw account globaal gerepliceerde door te lezen [consistentieniveaus in Azure Cosmos DB](consistency-levels.md). En Zie voor meer informatie over hoe globale databasereplicatie in Azure Cosmos DB werkt, [gegevens globaal met Azure Cosmos DB distribueren](distribute-data-globally.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Globale distributie op basis van de Azure-portal configureren
> * Globale distributie op basis van de SQL-API's configureren

U kunt nu doorgaan met de volgende zelfstudie voor meer informatie over het ontwikkelen van lokaal via de lokale Azure DB die Cosmos-emulator.

> [!div class="nextstepaction"]
> [Lokaal ontwikkelen met de emulator](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/


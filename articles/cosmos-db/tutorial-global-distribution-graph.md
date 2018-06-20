---
title: Zelfstudie over globale distributie van Azure DB Cosmos voor de Graph-API | Microsoft Docs
description: Lees hoe u wereldwijde distributie met Azure Cosmos DB kunt instellen met behulp van de Graph API.
services: cosmos-db
keywords: globale distributie, graaf, gremlin
author: luisbosquez
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: 61004a735f731cfd1303cf40b6e60cba496e942a
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763677"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-graph-api"></a>Wereldwijde distributie met Azure Cosmos DB instellen met behulp van de Graph API

In dit artikel laten we zien hoe Azure Portal kan worden gebruikt voor het instellen van globale distributie van Azure DB Cosmos en hoe u daarmee verbinding kunt maken met behulp van de Graph-API.

Dit artikel behandelt de volgende taken: 

> [!div class="checklist"]
> * Wereldwijde distributie configureren met behulp van Azure Portal
> * Globale distributie configureren met behulp van de [Graph-API's](graph-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a>Verbinding maken met een voorkeursregio met behulp van de Graph-API met behulp van de .NET-SDK

De Graph-API wordt beschikbaar gemaakt als een extensiebibliotheek op de SQL-API.

Om te profiteren van [wereldwijde distributie](distribute-data-globally.md), kunnen clienttoepassingen de geordende voorkeurslijst met regio's opgeven die moet worden gebruikt voor het uitvoeren van documentbewerkingen. Dat kan worden gedaan door het verbindingsbeleid in te stellen. Op basis van de Azure Cosmos DB-accountconfiguratie, de huidige regionale beschikbaarheid en de opgegeven voorkeurslijst wordt het optimale eindpunt door de SDK gekozen voor het uitvoeren van schrijf- en leesbewerkingen.

Deze voorkeurslijst wordt opgegeven bij het initialiseren van een verbinding met de SDK's. De SDK's accepteren de optionele parameter 'PreferredLocations', die een geordende lijst met Azure-regio's bevat.

* **Schrijfbewerkingen**: de SDK verzendt alle schrijfbewerkingen automatisch naar de huidige schrijfregio.
* **Leesbewerkingen**: alle leesbewerkingen worden verzonden naar de eerst beschikbare regio in de lijst PreferredLocations. Als de aanvraag mislukt, gaat de client naar de volgende regio in de lijst, enzovoort. De SDK's proberen alleen de regio's te lezen die zijn opgegeven in PreferredLocations. Dus als bijvoorbeeld het Cosmos DB-account in drie regio's beschikbaar is, maar de client alleen twee regio's waarnaar niet kan worden geschreven opgeeft als PreferredLocations, worden er geen leesbewerkingen verwerkt vanuit de schrijfregio, zelfs in geval van een failover.

De toepassing kan het huidige, door de SDK gekozen eindpunt voor lezen en eindpunt voor schrijven verifiëren door het controleren van de twee eigenschappen, WriteEndpoint en ReadEndpoint, die beschikbaar zijn in SDK-versie 1.8 en hoger. Als de eigenschap PreferredLocations niet is ingesteld, worden alle aanvragen verwerkt vanuit de huidige schrijfregio.

### <a name="using-the-sdk"></a>De SDK gebruiken

In de .NET-SDK heeft de parameter `ConnectionPolicy` voor de `DocumentClient`-constructor bijvoorbeeld een eigenschap met de naam `PreferredLocations`. Deze eigenschap kan worden ingesteld op een lijst met regionamen. De weergavenamen voor [Azure-regio’s][regions] kunnen worden opgegeven als onderdeel van `PreferredLocations`.

> [!NOTE]
> De URL's voor de eindpunten moeten niet worden beschouwd als constanten met een lange levensduur. De service kan deze op elk gewenst moment bijwerken. De SDK verwerkt deze wijziging automatisch.
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

En daarmee is deze zelfstudie voltooid. Informatie over het beheren van de consistentie van uw wereldwijd gerepliceerde account kunt u vinden in [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md). En voor meer informatie over hoe wereldwijde databasereplicatie werkt in Azure Cosmos DB, gaat u naar [Gegevens wereldwijd distribueren met Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Wereldwijde distributie configureren met behulp van Azure Portal
> * Wereldwijde distributie configureren met behulp van de SQL-API's

U kunt nu doorgaan met de volgende zelfstudie voor informatie over lokaal ontwikkelen met behulp van de lokale Azure Cosmos DB-emulator.

> [!div class="nextstepaction"]
> [Lokaal ontwikkelen met de emulator](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/


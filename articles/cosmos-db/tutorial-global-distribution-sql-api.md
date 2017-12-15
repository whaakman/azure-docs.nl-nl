---
title: Zelfstudie voor Azure DB Cosmos globale distributiepunt voor de SQL-API | Microsoft Docs
description: Informatie over het instellen van Azure DB die Cosmos globale distributie op basis van de SQL-API.
services: cosmos-db
keywords: Globale distributie
documentationcenter: 
author: rafats
manager: jhubbard
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: rafats
ms.custom: mvc
ms.openlocfilehash: 0cee55673c8abca29b7e389fa4fd62a48566904b
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-sql-api"></a>Het instellen van Azure DB die Cosmos globale distributie op basis van de SQL-API

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

In dit artikel, laten we zien hoe de Azure portal instellen van Azure DB die Cosmos globale distributie en vervolgens verbinding met de SQL-API gebruiken.

In dit artikel bevat informatie over de volgende taken: 

> [!div class="checklist"]
> * Globale distributie op basis van de Azure-portal configureren
> * Configureren globale distributie met behulp van de [SQL-API's](sql-api-introduction.md)

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-sql-api"></a>Verbinding maken met een voorkeursregio met behulp van de SQL-API

Om te profiteren van [globale distributie](distribute-data-globally.md), clienttoepassingen de voorkeur geordende lijst met regio's moeten worden gebruikt voor het document bewerkingen uitvoeren kunnen opgeven. Dit kan worden gedaan door het instellen van het verbindingsbeleid voor de. Op basis van de Azure DB die Cosmos-accountconfiguratie, huidige regionale beschikbaarheid en de voorkeur lijst is opgegeven, wordt het meest optimale eindpunt door de SQL-SDK schrijven uitvoeren en leesbewerkingen gekozen.

Deze lijst voorkeur is opgegeven bij het initialiseren van een verbinding met de SQL-SDK's. De SDK's accepteren een optionele parameter 'PreferredLocations' is een geordende lijst met Azure-regio's.

Alle schrijfbewerkingen naar de huidige schrijven regio worden automatisch verzonden door de SDK.

Alle leesbewerkingen wordt verzonden naar de eerste beschikbare regio in de lijst PreferredLocations. Als de aanvraag is mislukt, zal de client mislukken omlaag in de lijst voor de volgende regio, enzovoort.

De SDK's wordt alleen poging tot lezen van de gebieden die zijn opgegeven in PreferredLocations. Dus bijvoorbeeld kunnen als het databaseaccount beschikbaar in vier verschillende regio's is, maar de client alleen de twee read(non-write) regio's voor PreferredLocations opgeeft, klikt u vervolgens geen leesbewerkingen worden geleverd buiten de lezen regio die niet is opgegeven in PreferredLocations. Als de lezen gebieden die zijn opgegeven in de PreferredLocations niet beschikbaar zijn, wordt gelezen buiten schrijven regio worden geleverd.

De toepassing kunt controleren of de huidige schrijven endpoint en lezen eindpunt gekozen door de SDK door het controleren van de twee eigenschappen, WriteEndpoint en ReadEndpoint beschikbaar in de SDK-versie 1.8 en hoger.

Als de eigenschap PreferredLocations niet is ingesteld, worden alle aanvragen van de huidige schrijven regio worden geleverd.

## <a name="net-sdk"></a>.NET SDK
De SDK kan worden gebruikt zonder codewijzigingen. In dit geval de SDK automatisch wordt verwezen beide leest en schrijft naar het huidige schrijven gebied.

In versie 1.8 en hoger van de .NET SDK heeft de ConnectionPolicy-parameter voor de DocumentClient-constructor de eigenschap Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations aangeroepen. Deze eigenschap is van het type verzameling `<string>` en een lijst met regionamen moeten bevatten. De string-waarden zijn geformatteerd per regio-naamkolom op de [Azure-gebieden] [ regions] pagina, zonder spaties vóór of na de eerste en laatste teken respectievelijk.

De huidige geschreven en gelezen eindpunten zijn respectievelijk beschikbaar in DocumentClient.WriteEndpoint en DocumentClient.ReadEndpoint.

> [!NOTE]
> De URL's voor de eindpunten niet beschouwd als lange levensduur constanten. De service kan deze op elk gewenst moment bijwerken. De SDK verwerkt deze wijziging automatisch.
>
>

```csharp
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

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript en Python SDK 's
De SDK kan worden gebruikt zonder codewijzigingen. De SDK wordt in dit geval automatisch doorverwezen zowel lees- en schrijfbewerkingen naar de huidige regio schrijven.

In versie 1.8 en hoger van elke SDK, de ConnectionPolicy-parameter voor de constructor DocumentClient een nieuwe eigenschap aangeroepen DocumentClient.ConnectionPolicy.PreferredLocations. Dit is de parameter is een matrix met tekenreeksen die een lijst met regionamen neemt. De namen zijn geformatteerd per de regionaam kolom in de [Azure-gebieden] [ regions] pagina. U kunt ook de vooraf gedefinieerde constanten gebruiken in het gemak object AzureDocuments.Regions

De huidige geschreven en gelezen eindpunten zijn respectievelijk beschikbaar in DocumentClient.getWriteEndpoint en DocumentClient.getReadEndpoint.

> [!NOTE]
> De URL's voor de eindpunten niet beschouwd als lange levensduur constanten. De service kan deze op elk gewenst moment bijwerken. De SDK is, wordt deze wijziging automatisch verwerkt.
>
>

Hieronder volgt een voorbeeld van code voor NodeJS/Javascript. Python en Java volgen hetzelfde patroon.

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in the following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize the connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a>REST
Zodra een databaseaccount beschikbaar is gesteld in meerdere regio's, kunnen clients de beschikbaarheid opvragen door het uitvoeren van een aanvraag voor ophalen naar de volgende URI.

    https://{databaseaccount}.documents.azure.com/

De service retourneert een lijst met regio's en hun bijbehorende Azure DB die Cosmos-eindpunt URI's voor de replica's. De huidige schrijven regio wordt aangegeven in het antwoord. De client kan vervolgens als volgt selecteren het juiste eindpunt voor alle verdere REST-API-aanvragen.

Voorbeeld van een antwoord

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* PUT, POST en DELETE-aanvragen, moeten verwijzen naar de opgegeven URI voor schrijven
* Alle opgehaald en andere aanvragen (bijvoorbeeld query's) met het kenmerk alleen-lezen mag verwijzen naar een willekeurig eindpunt van de keuze van de client

Schrijven naar aanvragen voor alleen-lezen gebieden mislukt met foutcode HTTP 403 ('verboden').

Als de regio schrijven gewijzigd na de initiële detectie-fase van de client, mislukt schrijft naar de vorige schrijven regio met foutcode HTTP 403 ('verboden'). De client moet vervolgens de lijst met regio's om opnieuw op te halen van de bijgewerkte schrijven regio ophalen.

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


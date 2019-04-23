---
title: Na de migratie optimalisatie stappen bij het gebruik van Azure Cosmos DB-API voor MongoDB
description: Dit document biedt de optimalisatietechnieken na de migratie van MongoDB naar Azure Cosmos DB-APi voor Mongo DB.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: roaror
ms.openlocfilehash: c0c761fef481a1fdaa027f1329e9a4e72d62985a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013535"
---
# <a name="post-migration-optimization-steps-when-using-azure-cosmos-dbs-api-for-mongodb"></a>Na de migratie optimalisatie stappen bij het gebruik van Azure Cosmos DB-API voor MongoDB 

Nadat u de gegevens die zijn opgeslagen in de MongoDB-database naar Azure Cosmos DB-API voor MongoDB migreert, kunt u verbinding maken met Azure Cosmos DB en beheren van de gegevens. Deze handleiding bevat de stappen die u moet rekening houden met na de migratie. Zie de [MongoDB migreren naar Azure Cosmos DB-API voor MongoDB-zelfstudie](../dms/tutorial-mongodb-cosmos-db.md) voor de migratiestappen.

In deze handleiding leert u het volgende:
- [Verbinding maken met uw toepassing](#connect-account)
- [Het indexeringsbeleid optimaliseren](#indexing)
- [Wereldwijde distributie voor Azure Cosmos DB-API voor MongoDB configureren](#distribute-data)
- [Set-consistentieniveau](#consistency)

> [!NOTE]
> De alleen verplichte stap na de migratie op het toepassingsniveau van uw verandert de verbindingsreeks in uw toepassing om te verwijzen naar uw nieuwe Azure Cosmos DB-account. Alle andere stappen van de migratie worden optimalisaties voor aanbevolen.
>

## <a id="connect-account"></a>Verbinding maken met uw toepassing 

1. In een nieuw venster aanmelden bij de [Azure-portal](https://www.portal.azure.com/)
2. Uit de [Azure-portal](https://www.portal.azure.com/), in het linkerdeelvenster open de **alle resources** menu en zoek de Azure Cosmos DB-account waarmee u uw gegevens hebt gemigreerd.
3. Open de **Connection String** blade. Het rechterdeelvenster bevat alle informatie die u nodig hebt om verbinding te maken met uw account.
4. Gebruik de verbindingsgegevens in de configuratie van uw toepassing (of andere relevante locaties) in overeenstemming met de Azure Cosmos DB-API voor MongoDB-verbinding in uw app. 
![Connection-String](./media/mongodb-post-migration/connection-string.png)

Zie voor meer informatie de [verbinding maken met een MongoDB-toepassing met Azure Cosmos DB](connect-mongodb-account.md) pagina.

## <a id="indexing"></a>Het indexeringsbeleid optimaliseren

Alle gegevensvelden worden automatisch geïndexeerd, standaard, tijdens de migratie van gegevens naar Azure Cosmos DB. In veel gevallen is dit standaard indexeringsbeleid acceptabel. In het algemeen schrijfaanvragen verwijderen van indexen optimaliseert en optimaliseert de leesaanvragen met de standaardbeleidsregels voor indexering van beleid (dat wil zeggen, automatische indexering).

Zie voor meer informatie over het indexeren [gegevens indexeren in Azure Cosmos DB-API voor MongoDB](mongodb-indexing.md) , evenals de [indexeren in Azure Cosmos DB](index-overview.md) artikelen.

## <a id="distribute-data"></a>Uw gegevens wereldwijd distribueren

Azure Cosmos DB is beschikbaar in alle [Azure-regio's](https://azure.microsoft.com/regions/#services) over de hele wereld. Na het selecteren van het standaardconsistentieniveau voor uw Azure Cosmos DB-account, kunt u een of meer Azure-regio's koppelen (afhankelijk van de behoeften van uw wereldwijde distributie). Voor hoge beschikbaarheid en bedrijfscontinuïteit altijd het beste uitvoeren in ten minste 2 regio's. U kunt de tips voor het bekijken [optimaliseren van de kosten van implementaties in meerdere regio's in Azure Cosmos DB](optimize-cost-regions.md).

Als u wilt uw gegevens wereldwijd distribueren, Zie [gegevens wereldwijd over Azure Cosmos DB-API voor MongoDB distribueren](tutorial-global-distribution-mongodb.md). 

## <a id="consistency"></a>Set-consistentieniveau
Azure Cosmos DB biedt 5 goed gedefinieerde [consistentieniveaus](consistency-levels.md). Lees voor meer informatie over de toewijzing tussen MongoDB en Azure Cosmos DB-consistentieniveaus, [consistentieniveaus en Azure Cosmos DB-API's](consistency-levels-across-apis.md). Het standaardconsistentieniveau is het niveau van de consistentie sessie. Wijzigen van de consistentie is optioneel en u deze kunt optimaliseren voor uw app. Met behulp van Azure portal consistentieniveau wijzigen:

1. Ga naar de **Standaardconsistentie** blade onder instellingen.
2. Selecteer uw [consistentieniveau](consistency-levels.md)

De meeste gebruikers laat hun consistentieniveau op de standaardinstelling voor consistentie-sessie. Er zijn echter [beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md). 

## <a name="next-steps"></a>Volgende stappen

* [Een MongoDB-toepassing verbinden met Azure Cosmos DB](connect-mongodb-account.md)
* [Verbinding maken met Azure Cosmos DB-account Studio 3T gebruiken](mongodb-mongochef.md)
* [Wereldwijd distribueren wordt gelezen met behulp van Azure Cosmos DB-API voor MongoDB](mongodb-readpreference.md)
* [Verlopen van gegevens met Azure Cosmos DB-API voor MongoDB](mongodb-time-to-live.md)
* [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md)
* [Indexering in Azure Cosmos DB](index-overview.md)
* [Aanvraageenheden in Azure Cosmos DB](request-units.md)






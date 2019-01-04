---
title: Werken met Azure Cosmos DB-databases, containers en objecten
description: In dit artikel wordt beschreven hoe u het maken en gebruiken van Azure Cosmos DB-databases, containers en objecten
author: dharmas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: 6757f887376e1b399d6af18f114e203991c16a67
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53807683"
---
# <a name="working-with-azure-cosmos-databases-containers-and-items"></a>Werken met Azure Cosmos-databases, containers en objecten

Nadat u hebt gemaakt een [Azure Cosmos DB-account](account-overview.md) onder uw Azure-abonnement, kunt u gegevens beheren in uw account met het maken van databases, containers en objecten. Elk van deze entiteiten in dit artikel wordt beschreven: databases, containers en objecten. De volgende afbeelding ziet u de hiërarchie van verschillende entiteiten in een Azure Cosmos-account:

![Azure Cosmos-account-entiteiten](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Azure Cosmos-databases

U kunt een of meer Azure Cosmos-databases maken onder uw account. Een database is vergelijkbaar met een naamruimte, is de eenheid van het beheer voor een set met Azure Cosmos-containers. De volgende tabel ziet u hoe een Azure Cosmos-database is toegewezen aan verschillende API-specifieke entiteiten:

| **Azure Cosmos-entiteit** | **SQL-API** | **Cassandra-API** | **Van Azure Cosmos DB-API voor MongoDB** | **Gremlin-API** | **Tabel-API** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-database | Database | Keyspace | Database | Database | N.v.t. |

> [!NOTE]
> Met Table-API's-accounts, wanneer u de eerste tabel maakt wordt een standaard-database automatisch gemaakt in uw Azure Cosmos-account.

### <a name="operations-on-an-azure-cosmos-database"></a>Bewerkingen op een Azure Cosmos-database

U kunt werken met een Azure Cosmos-database met behulp van de volgende Azure Cosmos-API's:

| **Bewerking** | **Azure-CLI**|**SQL-API** | **Cassandra-API** | **Van Azure Cosmos DB-API voor MongoDB** | **Gremlin-API** | **Tabel-API** |
| --- | --- | --- | --- | --- | --- | --- |
|Het inventariseren van alle databases| Ja | Ja | Ja (database is toegewezen aan een keyspace) | Ja | N.v.t. | N.v.t. |
|Database lezen| Ja | Ja | Ja (database is toegewezen aan een keyspace) | Ja | N.v.t. | N.v.t. |
|Niewe database maken| Ja | Ja | Ja (database is toegewezen aan een keyspace) | Ja | N.v.t. | N.v.t. |
|Database bijwerken| Ja | Ja | Ja (database is toegewezen aan een keyspace) | Ja | N.v.t. | N.v.t. |


## <a name="azure-cosmos-containers"></a>Azure Cosmos-containers

Een Azure Cosmos-container is de eenheid van de schaalbaarheid voor zowel ingerichte doorvoer en opslag van items. Een container is horizontaal gepartitioneerd en vervolgens gerepliceerd in meerdere regio's. De items die u aan de container en de doorvoer toevoegen dat u richten op het beide automatisch worden verdeeld over een set met logische partities op basis van de partitiesleutel. Zie voor meer informatie over partitionering en partitiesleutel, [logische partities](partition-data.md) artikel. 

Bij het maken van een Azure Cosmos-container, kunt u de doorvoer configureren in een van de volgende modi:

* **Toegewezen ingerichte doorvoer** modus: Voor een container ingerichte doorvoer is exclusief zijn gereserveerd voor deze en wordt ondersteund door de SLA's. Zie voor meer informatie, [over het inrichten van doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).

* **Gedeelde ingerichte doorvoer** modus: De ingerichte doorvoer delen deze containers met andere containers in dezelfde database (met uitzondering van die containers die zijn geconfigureerd met een toegewezen ingerichte doorvoer). Met andere woorden, wordt de ingerichte doorvoer voor de database gedeeld tussen alle 'gedeeld' containers. Zie voor meer informatie, [ingerichte doorvoer op een Azure Cosmos-database configureren](how-to-provision-database-throughput.md).

Een Azure Cosmos-container elastisch schalen, of het maken van containers met een 'gedeeld' of 'toegewezen' ingerichte doorvoer modi.

Een Azure Cosmos-container is een container schema-agnostische van items. Items in een container kunnen willekeurige schema's hebben. Bijvoorbeeld, kan een item voor een persoon, een item voor een auto worden geplaatst in dezelfde container. Alle items die u aan een container toevoegt ophalen standaard automatisch geïndexeerd zonder een expliciete index of Schemabeheer. U kunt de indexering gedrag aanpassen door het indexeringsbeleid configureren voor een container. 

U kunt de Time To Live (TTL) instellen op de geselecteerde items in een Azure Cosmos-container of voor de volledige container verwijderen zonder problemen de items uit het systeem. Azure Cosmos DB worden de items automatisch verwijderd wanneer ze zijn verlopen. Daarnaast garandeert u dat een query uitgevoerd op de container niet verlopen items in een vaste gebonden retourneert. Zie voor meer informatie, [TTL configureren voor uw container](how-to-time-to-live.md).

Met behulp van feed wijzigen, kunt u zich abonneert op het bewerkingenlogboek die wordt beheerd voor elk van de logische partitie van de container. Het logboek van alle updates die worden uitgevoerd op de container samen met de Change Feed biedt de vóór en na afbeeldingen van de items. Zie [over het bouwen van reactieve toepassingen met behulp van change feed](change-feed.md). U kunt ook de bewaartermijn voor de change feed met behulp van het beleid voor de container-wijzigingenfeed configureren. 

U kunt opgeslagen procedures, triggers, gebruiker gedefinieerde functies (UDF's) en merge procedures registreren met uw Azure Cosmos-container. 

U kunt een unieke sleutel opgeven voor uw Azure Cosmos-container. Zorg ervoor dat een of meer waarden per partitiesleutel logische met een unieke sleutel beleid. Nadat een container is gemaakt met een unieke sleutel beleid, voorkomt u dat het maken van een nieuwe of bijgewerkte items met waarden die de waarden die zijn opgegeven door de unieke key-beperking te dupliceren. Zie voor meer informatie, [Unique key-beperkingen](unique-keys.md).

Een Azure Cosmos-container is gespecialiseerd in API-specifieke entiteiten als volgt:

| **Azure Cosmos-entiteit** | **SQL-API** | **Cassandra-API** | **Van Azure Cosmos DB-API voor MongoDB** | **Gremlin-API** | **Tabel-API** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-container | Verzameling | Tabel | Verzameling | Graph | Tabel |

### <a name="properties-of-an-azure-cosmos-container"></a>Eigenschappen van een Azure Cosmos-container

Een Azure Cosmos-container is een set door het systeem gedefinieerde eigenschappen. Afhankelijk van de keuze van de API, sommige fouten mogelijk niet rechtstreeks worden blootgesteld. De volgende tabel beschrijft de lijst met ondersteunde door het systeem gedefinieerde eigenschappen:

| **Door het systeem gedefinieerde eigenschap** | **Gegenereerde of gebruiker worden ingesteld van het systeem** | **Doel** | **SQL-API** | **Cassandra-API** | **Van Azure Cosmos DB-API voor MongoDB** | **Gremlin-API** | **Tabel-API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|__rid | Door het systeem gegenereerde | De unieke id van de container | Ja | Nee | Nee | Nee | Nee |
|__etag | Door het systeem gegenereerde | Entity-tag voor Optimistisch gelijktijdigheidbeheer gebruikt | Ja | Nee | Nee | Nee | Nee |
|__ts | Door het systeem gegenereerde | De laatst bijgewerkte timestamp van de container | Ja | Nee | Nee | Nee | Nee |
|__self | Door het systeem gegenereerde | Adresseerbare URI van de container | Ja | Nee | Nee | Nee | Nee |
|id | Gebruiker worden geconfigureerd | Gebruiker gedefinieerde unieke naam van de container | Ja | Ja | Ja | Ja | Ja |
|indexingPolicy | Gebruiker worden geconfigureerd | Biedt de mogelijkheid om te wijzigen van het pad van de index, hun precisie en de consistentiemodel. | Ja | Nee | Nee | Nee | Ja |
|TimeToLive | Gebruiker worden geconfigureerd | Biedt de mogelijkheid items om automatisch te verwijderen uit een container na een bepaalde periode. Zie voor meer informatie de [Time To Live](time-to-live.md) artikel. | Ja | Nee | Nee | Nee | Ja |
|changeFeedPolicy | Gebruiker worden geconfigureerd | Gebruikt om te lezen van wijzigingen in de items in een container. Zie voor meer informatie de [wijzigingenfeed](change-feed.md) artikel. | Ja | Nee | Nee | Nee | Ja |
|uniqueKeyPolicy | Gebruiker worden geconfigureerd | Controleer of de uniekheid van een of meer waarden in een logische partitie met unieke sleutels. Zie voor meer informatie de [unieke sleutels](unique-keys.md) artikel. | Ja | Nee | Nee | Nee | Ja |

### <a name="operations-on-an-azure-cosmos-container"></a>Bewerkingen op een Azure Cosmos-container

De volgende bewerkingen uit met behulp van een Azure Cosmos-API biedt ondersteuning voor een Azure Cosmos-container.

| **Bewerking** | **Azure-CLI** | **SQL-API** | **Cassandra-API** | **Van Azure Cosmos DB-API voor MongoDB** | **Gremlin-API** | **Tabel-API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Het inventariseren van containers in een database | Ja* | Ja | Ja | Ja | N.v.t. | N.v.t. |
| Lezen van een container | Ja | Ja | Ja | Ja | N.v.t. | N.v.t. |
| Nieuwe container maken | Ja | Ja | Ja | Ja | N.v.t. | N.v.t. |
| Update-container | Ja | Ja | Ja | Ja | N.v.t. | N.v.t. |
| Container verwijderen | Ja | Ja | Ja | Ja | N.v.t. | N.v.t. |

## <a name="azure-cosmos-items"></a>Azure Cosmos-items

Afhankelijk van de keuze van de API kan een Azure Cosmos-item ofwel een document in een verzameling, een rij in een tabel of een knooppunt/rand in een grafiek vertegenwoordigen. De volgende tabel ziet u de toewijzing tussen de API-specifieke entiteiten aan een Azure Cosmos-item:

| **Cosmos-entiteit** | **SQL-API** | **Cassandra-API** | **Van Azure Cosmos DB-API voor MongoDB** | **Gremlin-API** | **Tabel-API** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-item | Document | Rij | Document | Knooppunt of randtabel | Item |

### <a name="properties-of-an-item"></a>Eigenschappen van een item

Elke Azure Cosmos-item heeft de volgende eigenschappen van het systeem gedefinieerd. Afhankelijk van de keuze van de API, sommige fouten mogelijk niet rechtstreeks worden blootgesteld.

|**Door het systeem gedefinieerde eigenschap** | **Gegenereerde of gebruiker worden ingesteld van het systeem**| **Doel** | **SQL-API** | **Cassandra-API** | **Van Azure Cosmos DB-API voor MongoDB** | **Gremlin-API** | **Tabel-API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|__id | Door het systeem gegenereerde | De unieke id van het item | Ja | Nee | Nee | Nee | Nee |
|__etag | Door het systeem gegenereerde | Entity-tag voor Optimistisch gelijktijdigheidbeheer gebruikt | Ja | Nee | Nee | Nee | Nee |
|__ts | Door het systeem gegenereerde | De laatst bijgewerkte timestamp van het item | Ja | Nee | Nee | Nee | Nee |
|__self | Door het systeem gegenereerde | Adresseerbare URI van het item | Ja | Nee | Nee | Nee | Nee |
|id | Een van beide | Gebruiker gedefinieerde unieke naam binnen een logische partitie. Als de gebruiker niet de id opgeeft, wordt een automatisch gegenereerd. | Ja | Ja | Ja | Ja | Ja |
|Willekeurige, door de gebruiker gedefinieerde eigenschappen | Door de gebruiker gedefinieerde routes | Gebruiker gedefinieerde eigenschappen die worden weergegeven in de API-eigen weergave (JSON, BSON, CQL, enz.) | Ja | Ja | Ja | Ja | Ja |

### <a name="operations-on-items"></a>Bewerkingen op items

De volgende bewerkingen die kunnen worden uitgevoerd met behulp van een Azure Cosmos-API biedt ondersteuning voor Azure Cosmos-item.

| **Bewerking** | **Azure-CLI** | **SQL-API** | **Cassandra-API** | **Van Azure Cosmos DB-API voor MongoDB** | **Gremlin-API** | **Tabel-API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Invoegen, vervangen, verwijderen, Upsert, lezen | Nee | Ja | Ja | Ja | Ja | Ja |

## <a name="next-steps"></a>Volgende stappen

U kunt nu doorgaan om worden ingericht met doorvoer in Azure Cosmos-account of naar andere concepten te leren:

* [Ingerichte doorvoer op een Azure Cosmos-database configureren](how-to-provision-database-throughput.md)
* [Ingerichte doorvoer voor een Azure Cosmos-container configureren](how-to-provision-container-throughput.md)
* [Logische partities](partition-data.md)
* [TTL op Azure Cosmos-container configureren](how-to-time-to-live.md)
* [Over het bouwen van reactieve toepassingen met behulp van Change Feed](change-feed.md)
* [De unieke key-beperking voor uw Azure Cosmos-container configureren](unique-keys.md)

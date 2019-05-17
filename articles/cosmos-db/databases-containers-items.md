---
title: Werken met databases, containers en objecten in Azure Cosmos DB
description: In dit artikel wordt beschreven hoe u maken en gebruiken van databases, containers en objecten in Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: ea3ba91859bbfb1a7c589cdb36e9fb87b52a89b8
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560287"
---
# <a name="work-with-databases-containers-and-items-in-azure-cosmos-db"></a>Werken met databases, containers en objecten in Azure Cosmos DB

Nadat u hebt gemaakt een [Azure Cosmos DB-account](account-overview.md) onder uw Azure-abonnement, kunt u gegevens beheren in uw account met het maken van databases, containers en objecten. Dit artikel wordt beschreven elk van deze entiteiten. 

De volgende afbeelding ziet u de hiërarchie van verschillende entiteiten in een Azure Cosmos DB-account:

![Azure Cosmos-account-entiteiten](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Azure Cosmos-databases

U kunt een of meerdere Azure-Cosmos-databases maken onder uw account. Een database is vergelijkbaar met een naamruimte. Een database is de eenheid van het beheer voor een set met Azure Cosmos-containers. De volgende tabel ziet u hoe een Azure Cosmos-database is toegewezen aan verschillende API-specifieke entiteiten:

| Azure Cosmos-entiteit | SQL-API | Cassandra-API | Azure Cosmos DB-API voor MongoDB | Gremlin-API | Tabel-API |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-database | Database | Keyspace | Database | Database | N.V.T. |

> [!NOTE]
> Met Table-API-accounts kunt bij het maken van de eerste tabel, wordt een standaard-database automatisch gemaakt in uw Azure Cosmos-account.

### <a name="operations-on-an-azure-cosmos-database"></a>Bewerkingen op een Azure Cosmos-database

U kunt met een Azure Cosmos-database met Azure Cosmos-API's werken zoals beschreven in de volgende tabel:

| Bewerking | Azure-CLI | SQL-API | Cassandra-API | Azure Cosmos DB-API voor MongoDB | Gremlin-API | Tabel-API |
| --- | --- | --- | --- | --- | --- | --- |
|Het inventariseren van alle databases| Ja | Ja | Ja (database is toegewezen aan een keyspace) | Ja | N.v.t. | N.v.t. |
|Database lezen| Ja | Ja | Ja (database is toegewezen aan een keyspace) | Ja | N.v.t. | N.v.t. |
|Niewe database maken| Ja | Ja | Ja (database is toegewezen aan een keyspace) | Ja | N.v.t. | N.v.t. |
|Database bijwerken| Ja | Ja | Ja (database is toegewezen aan een keyspace) | Ja | N.v.t. | N.v.t. |


## <a name="azure-cosmos-containers"></a>Azure Cosmos-containers

Een Azure Cosmos-container is de schaalbaarheid voor ingerichte doorvoer en opslag. Een container is horizontaal gepartitioneerd en vervolgens gerepliceerd in meerdere regio's. De items die u toevoegt aan de container en de doorvoer die u in te op het richten worden automatisch verdeeld over een set met logische partities op basis van de partitiesleutel. Zie voor meer informatie over partitionering en partitiesleutels [partitiegegevens](partition-data.md). 

Wanneer u een Azure Cosmos-container maakt, kunt u de doorvoer configureren in een van de volgende modi:

* **Toegewezen ingerichte doorvoermodus**: De doorvoer die is ingericht in een container is exclusief zijn gereserveerd voor die container en wordt ondersteund door de SLA's. Zie voor meer informatie, [over het inrichten van doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).

* **De modus gedeelde ingerichte doorvoer**: Deze containers delen de ingerichte doorvoer met de andere containers in dezelfde database (met uitzondering van containers die zijn geconfigureerd met een toegewezen ingerichte doorvoer). Met andere woorden, wordt de ingerichte doorvoer voor de database gedeeld tussen alle 'gedeelde doorvoer'-containers. Zie voor meer informatie, [hoe u de doorvoer voor een Azure Cosmos-database inrichten](how-to-provision-database-throughput.md).

Een Azure Cosmos-container kunt flexibel, schalen of u containers maken met behulp van toegewezen of gedeelde ingerichte doorvoer modi.

Een Azure Cosmos-container is een container schema-agnostische van items. Items in een container kunnen willekeurige schema's hebben. Bijvoorbeeld, een item dat staat voor een persoon en een item dat staat voor een auto kunnen worden geplaatst de *dezelfde container*. Standaard worden alle items die u aan een container toevoegt automatisch geïndexeerd zonder expliciete index of Schemabeheer. U kunt de indexering gedrag aanpassen door het configureren van de [indexeringsbeleid](index-overview.md) voor een container. 

U kunt instellen [Time to Live (TTL)](time-to-live.md) op geselecteerde items in een Azure Cosmos-container of voor de volledige container verwijderen zonder problemen de items uit het systeem. Azure Cosmos DB worden de items automatisch verwijderd wanneer ze zijn verlopen. Daarnaast garandeert u dat een query uitgevoerd op de container verlopen items in een vaste gebonden niet retourneren. Zie voor meer informatie, [TTL configureren voor uw container](how-to-time-to-live.md).

U kunt [wijzigingenfeed](change-feed.md) abonneren op het bewerkingenlogboek die voor elke logische partitie van de container wordt beheerd. Wijzigingenfeed bevat het logboek van alle updates die worden uitgevoerd op de container, samen met de vóór en na de installatiekopieën van de items. Zie voor meer informatie, [reactieve toepassingen bouwen met behulp van de wijzigingenfeed](serverless-computing-database.md). U kunt ook de bewaartermijn voor de change feed met behulp van het beleid voor de container-wijzigingenfeed configureren. 

U kunt registreren [opgeslagen procedures, triggers, de gebruiker gedefinieerde functies (UDF's)](stored-procedures-triggers-udfs.md), en [samenvoegen procedures](how-to-manage-conflicts.md) voor uw Azure Cosmos-container. 

Kunt u een [unique key-beperking](unique-keys.md) voor uw Azure Cosmos-container. Zorg ervoor dat een of meer waarden per partitiesleutel logische met een unieke sleutel beleid. Als u een container met behulp van een unieke sleutel beleid maakt, kunnen geen nieuwe of bijgewerkte items met waarden die de waarden die zijn opgegeven door de unieke key-beperking dubbele worden gemaakt. Zie voor meer informatie, [Unique key-beperkingen](unique-keys.md).

Een Azure Cosmos-container is gespecialiseerd in API-specifieke entiteiten zoals wordt weergegeven in de volgende tabel:

| Azure Cosmos-entiteit | SQL-API | Cassandra-API | Azure Cosmos DB-API voor MongoDB | Gremlin-API | Tabel-API |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-container | Verzameling | Tabel | Verzameling | Graph | Tabel |

### <a name="properties-of-an-azure-cosmos-container"></a>Eigenschappen van een Azure Cosmos-container

Een Azure Cosmos-container is een set van het systeem gedefinieerde eigenschappen. Afhankelijk van welke API die u gebruikt, enkele eigenschappen kunnen niet rechtstreeks worden blootgesteld. De volgende tabel beschrijft de lijst met eigenschappen van het systeem gedefinieerde:

| Het systeem gedefinieerde eigenschap | Het systeem gegenereerde of de gebruiker configureerbare | Doel | SQL-API | Cassandra-API | Azure Cosmos DB-API voor MongoDB | Gremlin-API | Tabel-API |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_id | Het systeem gegenereerde | De unieke id van de container | Ja | Nee | Nee | Nee | Nee |
|\_etag | Het systeem gegenereerde | Entity-tag voor Optimistisch gelijktijdigheidbeheer gebruikt | Ja | Nee | Nee | Nee | Nee |
|\_TS | Het systeem gegenereerde | De laatst bijgewerkte timestamp van de container | Ja | Nee | Nee | Nee | Nee |
|\_Selfservice | Het systeem gegenereerde | Adresseerbare URI van de container | Ja | Nee | Nee | Nee | Nee |
|id | Gebruiker configureerbare | Gebruiker gedefinieerde unieke naam van de container | Ja | Ja | Ja | Ja | Ja |
|indexingPolicy | Gebruiker configureerbare | Biedt de mogelijkheid om te wijzigen van het pad van de index, het indextype en de index-modus | Ja | Nee | Nee | Nee | Ja |
|TimeToLive | Gebruiker configureerbare | Biedt de mogelijkheid items om automatisch te verwijderen uit een container na een bepaalde tijd instellen. Zie voor meer informatie, [Time to Live](time-to-live.md). | Ja | Nee | Nee | Nee | Ja |
|changeFeedPolicy | Gebruiker configureerbare | Gebruikt om te lezen van wijzigingen in de items in een container. Zie voor meer informatie, [Wijzigingenfeed](change-feed.md). | Ja | Nee | Nee | Nee | Ja |
|uniqueKeyPolicy | Gebruiker configureerbare | Gebruikt om ervoor te zorgen dat een of meer waarden in een logische partitie. Zie voor meer informatie, [Unique key-beperkingen](unique-keys.md). | Ja | Nee | Nee | Nee | Ja |

### <a name="operations-on-an-azure-cosmos-container"></a>Bewerkingen op een Azure Cosmos-container

Een Azure Cosmos-container ondersteunt de volgende bewerkingen uit als u een van de Azure Cosmos-API's gebruiken:

| Bewerking | Azure-CLI | SQL-API | Cassandra-API | Azure Cosmos DB-API voor MongoDB | Gremlin-API | Tabel-API |
| --- | --- | --- | --- | --- | --- | --- |
| Het inventariseren van containers in een database | Ja | Ja | Ja | Ja | N.v.t. | N.v.t. |
| Lezen van een container | Ja | Ja | Ja | Ja | N.v.t. | N.V.T. |
| Een nieuwe container maken | Ja | Ja | Ja | Ja | N.v.t. | N.V.T. |
| Bijwerken van een container | Ja | Ja | Ja | Ja | N.v.t. | N.V.T. |
| Een container verwijderen | Ja | Ja | Ja | Ja | N.v.t. | N.v.t. |

## <a name="azure-cosmos-items"></a>Azure Cosmos-items

Afhankelijk van welke API die u gebruikt, kan een Azure Cosmos-item op een document in een verzameling, een rij in een tabel of een knooppunt of randtabel in een grafiek vertegenwoordigen. De volgende tabel ziet u de toewijzing van de API-specifieke entiteiten aan een Azure Cosmos-item:

| Cosmos-entiteit | SQL-API | Cassandra-API | Azure Cosmos DB-API voor MongoDB | Gremlin-API | Tabel-API |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-item | Document | Rij | Document | Knooppunt of randtabel | Item |

### <a name="properties-of-an-item"></a>Eigenschappen van een item

Elke Azure Cosmos-item heeft de volgende eigenschappen voor het systeem gedefinieerde. Afhankelijk van welke API die u gebruikt, sommige fouten mogelijk niet rechtstreeks worden blootgesteld.

| Het systeem gedefinieerde eigenschap | Het systeem gegenereerde of de gebruiker configureerbare| Doel | SQL-API | Cassandra-API | Azure Cosmos DB-API voor MongoDB | Gremlin-API | Tabel-API |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_id | Het systeem gegenereerde | De unieke id van het item | Ja | Nee | Nee | Nee | Nee |
|\_etag | Het systeem gegenereerde | Entity-tag voor Optimistisch gelijktijdigheidbeheer gebruikt | Ja | Nee | Nee | Nee | Nee |
|\_TS | Het systeem gegenereerde | Tijdstempel van de laatste update van het item | Ja | Nee | Nee | Nee | Nee |
|\_Selfservice | Het systeem gegenereerde | Adresseerbare URI van het item | Ja | Nee | Nee | Nee | Nee |
|id | Een van beide | Gebruiker gedefinieerde unieke naam in een logische partitie. Als de gebruiker niet de ID opgeeft, genereert het systeem automatisch een. | Ja | Ja | Ja | Ja | Ja |
|Willekeurige, door de gebruiker gedefinieerde eigenschappen | Door de gebruiker gedefinieerd | Gebruiker gedefinieerde eigenschappen die worden weergegeven in de API-eigen weergave (met inbegrip van JSON en BSON CQL) | Ja | Ja | Ja | Ja | Ja |

### <a name="operations-on-items"></a>Bewerkingen op items

Azure Cosmos-items ondersteunen de volgende bewerkingen. U kunt een van de Azure Cosmos-API's gebruiken de bewerkingen uit te voeren.

| Bewerking | Azure-CLI | SQL-API | Cassandra-API | Azure Cosmos DB-API voor MongoDB | Gremlin-API | Tabel-API |
| --- | --- | --- | --- | --- | --- | --- |
| Invoegen, vervangen, verwijderen, Upsert, lezen | Nee | Ja | Ja | Ja | Ja | Ja |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over deze taken en concepten:

* [Doorvoer van een Azure Cosmos-database inrichten](how-to-provision-database-throughput.md)
* [Inrichten doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md)
* [Werken met logische partities](partition-data.md)
* [TTL-waarde op een Azure Cosmos-container configureren](how-to-time-to-live.md)
* [Reactieve toepassingen bouwen met behulp van de feed wijzigen](change-feed.md)
* [Een unieke key-beperking voor uw Azure Cosmos-container configureren](unique-keys.md)

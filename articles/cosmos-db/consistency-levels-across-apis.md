---
title: Consistentieniveaus en Azure Cosmos DB-API's
description: Informatie over de consistentieniveaus voor API's in Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: 902303a8f55f4494e0cc6c21b0438e41437c0567
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620662"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Consistentieniveaus en Azure Cosmos DB-API's

Azure Cosmos DB biedt ingebouwde ondersteuning voor wire-protocol-compatibele API's voor populaire-databases. Het gaat hierbij om MongoDB, Apache Cassandra, Gremlin en Azure Table storage. Deze databases bieden nauwkeurig omschreven consistentiemodellen of SLA's gesteunde garanties met betrekking tot de consistentieniveaus. Ze bieden doorgaans slechts een subset van de vijf consistentiemodellen aangeboden door Azure Cosmos DB. 

Wanneer u SQL-API, Gremlin-API en Table-API, wordt het standaardconsistentieniveau geconfigureerd op de Azure Cosmos-account wordt gebruikt. 

Wanneer u de Cassandra-API of Azure Cosmos DB-API voor MongoDB, ophalen van de toepassingen een volledige set van consistentieniveaus die Apache Cassandra en MongoDB, respectievelijk, met nog krachtiger consistentie en garanties voor duurzaamheid. Dit document bevat de bijbehorende Azure Cosmos DB-consistentieniveaus voor Apache Cassandra en MongoDB-consistentieniveaus.


## <a id="cassandra-mapping"></a>Toewijzing tussen Apache Cassandra en Azure Cosmos DB-consistentieniveaus

In tegenstelling tot AzureCosmos DB biedt Apache Cassandra systeemeigen geen garanties voor nauwkeurig gedefinieerd consistentie.  Apache Cassandra biedt in plaats daarvan een consistentieniveau schrijven en een niveau lezen van consistentie, zodat de hoge beschikbaarheid, consistentie en latentie compromissen. Als u Azure Cosmos DB Cassandra-API: 

* Het niveau van de consistentie schrijven van Apache Cassandra wordt toegewezen aan het standaardconsistentieniveau geconfigureerd in uw Azure Cosmos-account. 

* Azure Cosmos DB wordt dynamisch toegewezen voor het lezen van consistentieniveau dat is opgegeven door het stuurprogramma van de Cassandra-client op een van de Azure Cosmos DB-consistentieniveaus dynamisch geconfigureerd op een leesaanvraag. 

De volgende tabel ziet u hoe de systeemeigen Cassandra-consistentieniveaus zijn toegewezen aan de Azure Cosmos DB-consistentieniveaus bij het gebruik van de Cassandra-API:  

[ ![Toewijzing van Cassandra consistentie-model](./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png) ](./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png#lightbox)

## <a id="mongo-mapping"></a>Toewijzing tussen MongoDB en Azure Cosmos DB-consistentieniveaus

In tegenstelling tot Azure Cosmos DB biedt de systeemeigen MongoDB geen nauwkeurig gedefinieerde consistentiegarantie. In plaats daarvan systeemeigen MongoDB kan gebruikers de volgende garanties voor consistentie configureren: een probleem schrijven, lezen belang en de richtlijn isMaster - om de leesbewerkingen op de primaire of secundaire replica's om de gewenste consistentieniveau te regelen. 

Wanneer u Azure Cosmos DB-API voor MongoDB, de MongoDB-stuurprogramma wordt uw schrijfregio beschouwd als de primaire replica en alle andere regio's worden gelezen replica. U kunt kiezen welke regio die is gekoppeld aan uw Azure Cosmos-account als een primaire replica. 

Tijdens het gebruik van Azure Cosmos DB-API voor MongoDB:

* De bezorgdheid schrijven is toegewezen aan het standaardconsistentieniveau geconfigureerd in uw Azure Cosmos-account.
 
* Azure Cosmos DB wordt dynamisch toegewezen voor het lezen aandachtspunt opgegeven door het stuurprogramma van de MongoDB-client op een van de Azure Cosmos DB-consistentieniveaus die dynamisch is geconfigureerd op een leesaanvraag. 

* U kunt aantekeningen toevoegen aan een specifieke regio die is gekoppeld aan uw Azure Cosmos-account als 'Master' door de regio bevinden als de eerste beschrijfbare regio. 

De volgende tabel ziet u hoe de systeemeigen MongoDB schrijven/lezen problemen worden toegewezen aan de Azure Cosmos-consistentieniveaus bij het gebruik van Azure Cosmos DB-API voor MongoDB:

[ ![Toewijzing van MongoDB consistentie-model](./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png) ](./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over consistentieniveaus en compatibiliteit tussen Azure Cosmos DB-API's met de open-source-API's. Zie de volgende artikelen:

* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [MongoDB-functies worden ondersteund door de Azure Cosmos DB-API voor MongoDB](mongodb-feature-support.md)
* [Apache Cassandra-functies worden ondersteund door de Cassandra-API van Azure Cosmos DB](cassandra-support.md)
---
title: Met behulp van Azure Cosmos DB meerdere masters met open-source NoSQL-databases
description: ''
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: ea3a16cf6b5aa4e46ad401e59aacb4d936a7429a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963897"
---
# <a name="using-azure-cosmos-db-multi-master-with-open-source-nosql-databases"></a>Met behulp van Azure Cosmos DB meerdere masters met open-source NoSQL-databases

Meerdere masters ondersteuning van Azure Cosmos DB is een systeemeigen, server-side '-implementatie die beschikbaar is voor alle de open-source NoSQL-aanbiedingen wordt ondersteund door Cosmos DB. Het is ook toegankelijk is voor alle Cosmos DB SDK's ondersteund.

Azure Cosmos DB is de eerste service in de hele wereld voor meerdere masters ondersteuning voor deze open-source NoSQL-databases.

|Model  |Ondersteuning  |
|---------|---------|
|MongoDB  | Actief-actief  |
|Graph  | Actief-actief |
|Cassandra  | Actief-actief   |

## <a name="use-mongodb-clients-with-multi-master"></a>MongoDB-clients gebruiken met meerdere masters

De functie voor meerdere masters is ingeschakeld voor MongoDB-API-accounts op dezelfde manier als die voor andere Azure Cosmos DB-API's is ingeschakeld. Na het inschakelen van meerdere masters voor MongoDB-API-accounts kan elk exemplaar van een clienttoepassing de gewenste regio selecteren voor lees- en schrijfbewerkingen. Vanuit het perspectief van het stuurprogramma voor MongoDB verschijnt de gewenste regio naar de client als de primaire replicaset. Elke regio van uw gedistribueerde database kan op deze manier fungeren als primaire replicaset. Meerdere masters van Azure Cosmos DB kunt u aanzienlijk verkorten schrijven latentie voor uw wereldwijd gedistribueerde toepassingen van MongoDB. 

### <a name="set-the-primary-region"></a>Instellen van de primaire regio

Elk exemplaar van een MongoDB-client kan de primaire regio selecteren door toe te voegen `@<preferred_primary_region_name>` naar het veld ' application ' geaccepteerd door de MongoDB-clientstuurprogramma. Stuurprogramma's voor de meeste accepteren dit in de verbindingsreeks, zoals:

`mongodb://fabrikam:KEY@fabrikam.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

Nadat u verbinding maakt, kan een van de volgende gevallen optreden:

* Als de regionaam van de gewenste beschikbaar als een regio voor schrijven is, Azure Cosmos DB wordt deze vervolgens geselecteerd als de primaire regio.

* Als de naam van een voorkeursregio niet opgegeven is, selecteert Azure Cosmos DB een standaardregio als de primaire regio.

* Als de naam van een voorkeursregio is opgegeven, maar als deze niet beschikbaar als een regio voor schrijven voor het account van de database is, selecteert Azure Cosmos DB de dichtstbijzijnde regio voor schrijven die beschikbaar is als de primaire regio.

Bepaalde stuurprogramma's, zoals de NodeJS-stuurprogramma wordt altijd eerst probleem schrijft naar de host de eerste verbindingstekenreeks opgegeven. Voor deze stuurprogramma's, om ervoor te zorgen schrijfbewerkingen worden doorgestuurd naar de gewenste regio, naast de naam van de app, moet u de DNS-naam in de verbindingsreeks om op te nemen van de regionaam van de wijzigen. Zorg ervoor dat u de naam van het gebied zonder spaties. Bijvoorbeeld:

`mongodb://fabrikam:KEY@fabrikam-westus.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

### <a name="conflict-resolution-mode"></a>Modus voor het conflict oplossen

De modus voor het oplossen van conflicten voor Azure Cosmos DB MongoDB API-accounts is altijd laatste schrijver wins, met behulp van de servertijdstempel in regionale die de schrijven geaccepteerd.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over meerdere masters ondersteuning voor MongoDB-API van Azure Cosmos DB-accounts. Bekijk vervolgens de volgende bronnen:

* [Het inschakelen van meerdere masters voor Azure Cosmos DB-accounts](enable-multi-master.md)

* [Informatie over conflictoplossing in Azure Cosmos DB](multi-master-conflict-resolution.md)

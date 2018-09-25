---
title: Belangrijkste voordelen van Azure DB Cosmos globale distributie
description: Meer informatie over Azure Cosmos DB-meerdere masters, belangrijke voordelen die worden aangeboden door geo-replicatie, meerdere masters en gebruik gevallen waarin het nuttig.
services: cosmos-db
author: markjbrown
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 11d70a648bfc1882753688e5352835b04cee6b9f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968340"
---
# <a name="distribute-data-globally-with-azure-cosmos-db"></a>Gegevens globaal distribueren met Azure Cosmos DB

Dit artikel beschrijft de belangrijkste voordelen van gegevens wereldwijd distribueren en sommige realtime scenario's waar de verdeling van de globale gegevens is vereist.

## <a name="key-benefits"></a>Belangrijkste voordelen

Hier volgen de belangrijkste voordelen beschikbaar voor accounts die gebruikmaken van mogelijkheden voor de distributie van Azure Cosmos DB globale gegevens:

* **Latentie van één cijfer** : Azure Cosmos DB biedt < 10 ms lezen en schrijven van latentie in het 99e percentiel, gegarandeerd door [met financiële Sla's ondersteund](https://azure.microsoft.com/support/legal/sla/cosmos-db/).

* **5-9 beschikbaarheid** : Azure Cosmos DB biedt 99,999% lezen en schrijven van beschikbaarheid, gegarandeerd door [met financiële Sla's ondersteund](https://azure.microsoft.com/support/legal/sla/cosmos-db/).

* **Flexibele conflictoplossing** – voor klanten maken gebruik van mogelijkheden voor meerdere masters, Azure Cosmos DB drie modi biedt voor het verwerken van conflicten om ervoor te zorgen voor globale gegevensintegriteit en consistentie.

* **Instelbare consistentie** : Azure Cosmos DB biedt ondersteuning voor verschillende 5 [consistentieniveaus](consistency-levels.md) met wereldwijde distributie in gedachten met ondersteuning voor alle sterke consistentie voor accounts met leren werken met multi mogelijkheid.

* **Impliciete fouttolerantie** -met gegevens die zijn gerepliceerd in meerdere regio's, toepassingen kunnen profiteren van hoge fouttolerantie tegen regionale uitval.

## <a name="use-cases"></a>Gebruiksvoorbeelden

Dit is slechts een voorbeeld van de scenario's, die van profiteren kunnen de wereldwijd distributie mogelijkheden van Azure Cosmos DB.

* **Socialemedia-apps** : sociale mediatoepassingen vereisen met lage latentie, hoge beschikbaarheid en schaalbaarheid voor een fantastische ervaring voor gebruikers overal ter wereld.

* **IoT** -edge geografisch gedistribueerde implementaties vaak nodig hebt om bij te houden van time series-gegevens van meerdere locaties. Elk apparaat kan worden homed naar de dichtstbijzijnde regio. Wanneer apparaten naar verschillende locaties verplaatst, kunnen die apparaten opnieuw om te schrijven naar de dichtstbijzijnde beschikbare regio worden ondergebracht.

* **E-Commerce** -E-Commerce vereist zeer lage latentie, evenals hoge beschikbaarheid. Geo-distributie van gegevens met lees- en schrijfbewerkingen plaatst de gegevens die het dichtst bij gebruikers, het verhogen van de reactietijd van toepassingen. Beschikbaar voor zowel lees- en schrijfbewerkingen in meerdere regio's biedt een hogere beschikbaarheid.

* **Fraude/Anomaliedetectie** -vaak toepassingen die gebruikersactiviteit of accountactiviteit controleren wereldwijd worden gedistribueerd en moet bijhouden van meerdere gebeurtenissen tegelijkertijd om bij te werken van scores om te voorkomen dat de metrische gegevens risico inline.

* **Softwarelicentiecontrole** - telling en het regelen van gebruik (zoals API-aanroepen, transacties per seconde, minuten gebruikt) wereldwijd kan worden geïmplementeerd met eenvoudig te houden met behulp van Azure Cosmos DB meerdere masters. Ingebouwde conflictoplossing zorgt ervoor dat beide nauwkeurigheid van de aantallen en regelgeving in realtime.

## <a name="next-steps"></a>Volgende stappen  

In dit artikel hebt u geleerd over de belangrijkste voordelen en gebruiksvoorbeelden voor de globale gegevens distributie-mogelijkheden van Azure Cosmos DB. Bekijk vervolgens de volgende bronnen:

* [Hoe Azure Cosmos DB kunt kant en klare wereldwijde distributie](distribute-data-globally-turnkey.md)

* [Azure Cosmos DB globale databasereplicatie configureren](tutorial-global-distribution-sql-api.md)

* [Het inschakelen van meerdere masters voor Azure Cosmos DB-accounts](enable-multi-master.md)

* [Hoe automatische en handmatige failover in Azure Cosmos DB werkt](regional-failover.md)

* [Informatie over conflictoplossing in Azure Cosmos DB](multi-master-conflict-resolution.md)

* [Met behulp van meerdere masters met open-source NoSQL-databases](multi-master-oss-nosql.md)

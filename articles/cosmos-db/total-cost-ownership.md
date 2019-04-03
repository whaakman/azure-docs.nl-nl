---
title: Totale kosten van Ownership(TCO) met Azure Cosmos DB
description: In dit artikel vergelijkt de totale eigendomskosten van Azure Cosmos DB met IaaS- en on-premises databases
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: c3a3305197802906077dab330a6b51c1195c6c36
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879468"
---
# <a name="total-cost-of-ownershiptco-with-azure-cosmos-db"></a>Totale kosten van Ownership(TCO) met Azure Cosmos DB

Azure Cosmos DB is ontworpen met de fijnmazige multitenancy en resource governance. Dit ontwerp biedt Azure Cosmos DB op gebruikers van aanzienlijk lagere kosten en help opslaan werken. Azure Cosmos DB ondersteunt momenteel meer dan 280 klant workloads op één computer met de dichtheid continu verhogen en duizenden klanten workloads binnen een cluster. Deze verdeelt de belasting van replica's van klanten workloads op verschillende computers in een cluster en tussen meerdere clusters binnen een datacenter voor meer informatie Zie [Azure Cosmos DB: De grens van wereldwijd gedistribueerde databases pushen](https://azure.microsoft.com/blog/azure-cosmos-db-pushing-the-frontier-of-globally-distributed-databases/). Vanwege resourcebeheer multitenancy en systeemeigen integratie met de rest van de Azure-infrastructuur is Azure Cosmos DB 4 tot 6 keer goedkoper dan MongoDB, Cassandra of andere OSS-NoSQL uitgevoerd op IaaS en maximaal 10 keer goedkoper dan de database gemiddeld de engines dat on-premises. Zie het document op [de totale eigendomskosten (niet) van een cloudservice voor NoSQL-database](https://documentdbportalstorage.blob.core.windows.net/papers/11.15.2017/NoSQL%20TCO%20paper.pdf).

De OSS-NoSQL-database-oplossingen, zoals Apache Cassandra, MongoDB, HBase, -engines zijn ontworpen voor on-premises. Ze zijn gelijk aan een Resource Manager-sjabloon met een tenantdatabase voor de ingerichte clusters beheren en controleren van ondersteuning wanneer die worden aangeboden als een beheerde service. OSS-NoSQL-architecturen vereisen aanzienlijke operationele overhead en de expertise die kan worden moeilijk en kostbaar zijn om te zoeken. Aan de andere kant, is Azure Cosmos DB een volledig beheerde cloudservice, waardoor ontwikkelaars zich richten op zakelijke innovatie in plaats van op beheer en onderhoud van database-infrastructuur. 

In tegenstelling tot een cloud-eigen databaseservice Azure Cosmos DB, OSS-NoSQL-database-engines niet ontworpen en gebouwd met de resourcebeheer of fijnmazig multitenancy als de fundamentele architectonische principes. OSS-NoSQL-database-engines, zoals Cassandra en MongoDB maken een fundamentele ervan uitgegaan dat de resources van de virtuele machine waarop ze worden uitgevoerd beschikbaar voor het gebruik ervan zijn. Veel van deze database-engines kunnen niet worden gebruikt als de hoeveelheid resources onder een bepaalde drempelwaarde komt. Bijvoorbeeld: voor kleine VM-instanties, en ze zijn beschikbaar met de leverancier van de aanbevolen configuraties doorgaans grootschalige VM's met hogere kosten voorstellen. Dus het is niet mogelijk voor het hosten van een OSS-NoSQL of een andere on-premises database-engine en beschikbaar maken met behulp van een op consumptie gebaseerd Bezig met laden model, zoals aanvragen per seconde of verbruikt opslag.

## <a name="total-cost-of-ownership-of-azure-cosmos-db"></a>Totale eigendomskosten van Azure Cosmos DB 

Het serverloze inrichtingsmodel van Azure Cosmos DB elimineert de noodzaak voor het inrichten te veel van de database-infrastructuur. Azure Cosmos DB-resources zijn opgegeven hoeft u geen voor speciale configuraties of licenties. Als gevolg hiervan, de Azure Cosmos DB ondersteunde toepassingen kunnen worden uitgevoerd met minder dan een 70 procent totale kosten van eigendom besparen in vergelijking met OSS-NoSQL-databases. Zie voor enkele voorbeelden van realtime [klant use cases](https://customers.microsoft.com/en-us/search?sq=Cosmos%20DB&ff=&p=0&so=story_publish_date%20desc). Andere voordelen van het prijsmodel van Azure Cosmos DB zijn:

* **Uitstekende waarde voor de prijs:** Handel analisten, klanten en partners hebt bevestigd dat een hogere waarde van de functies die Azure Cosmos DB voor een veel lagere prijs vergeleken biedt met klanten krijgt bij het implementeren van deze oplossingen op hun eigen of via andere leveranciers. De database-functies die wereldwijde distributie, consistentiemodellen meerdere masters, goed gedefinieerde en intuïtieve, automatische indexering zijn aanzienlijk vereenvoudigd met Azure Cosmos DB zonder overhead, complexiteit of downtime.

* **Er is geen NoSQL DevOps-beheer is vereist:** Met Azure Cosmos DB hoeft een niet te maken met het gebruik van DevOps-implementaties beheren, voert u onderhoud, de schaal of patch. U kunt de alle de werkbelastingen die u met OSS NoSQL cluster gehost on-premises of op cloud-infrastructuur doen zou kunt uitvoeren.

![Azure Cosmos DB-eigendomskosten](./media/total-cost-ownership/tco.png)

* **De mogelijkheid om elastisch te schalen:** Doorvoer van Azure Cosmos DB kan omhoog of omlaag worden geschaald zodat u kunt de kosten van eigendom tijdens piek verlagen. OSS-NoSQL-clusters die zijn geïmplementeerd op cloud-infrastructuur beperkt flexibiliteit bieden, en on-premises implementaties worden elastische per definitie niet. In Azure Cosmos DB, als u meer doorvoer inrichten is uw doorvoer gegarandeerd lineair worden geschaald. Deze garantie is back-ups door financiële Sla's en in het 99e percentiel, op elke schaal.

* **Schaalvoordelen:** Een beheerde service zoals Azure Cosmos DB werkt met een groot aantal knooppunten, geïntegreerd systeemeigen met netwerken, opslag en berekeningen. Vanwege de Azure Cosmos DB-grootschalige, standaardisatie u kosten kunt besparen.

* **Geoptimaliseerd voor de cloud:** Azure Cosmos DB is ontworpen vanaf de grond-up met fijnmazig isolatie voor meerdere tenants en prestaties. Dit maakt optimaal plaatsen, uitvoeren en duizenden tenants en hun werklast te verdelen over clusters en datacentrums. De huidige generatie van OSS-NoSQL-databases worden daarentegen, on-premises uitgevoerd met de volledige virtuele machine is ervan uitgegaan dat de workload van één tenant wordt uitgevoerd. Deze databases zijn niet ontworpen om gebruikmaken van de infrastructuur en hardware naar de volledige omvang van de cloudprovider. Bijvoorbeeld, een OSS-NoSQL-database-engine is niet op de hoogte van de verschillen tussen een virtuele machine in Visual Studio een routine-installatiekopie bijwerken of het feit dat premium-schijf is al drie richtingen gerepliceerd. Het kan niet profiteren van deze voordelen en over de voordelen en besparingen doorgeven aan klanten.

* **U betaalt per uur:** Voor grootschalige workloads, die nodig hebt om te schalen op elk gewenst moment in-time, betaalt u alleen per uur. De werkbelastingen in een toepassing is doorgaans kunnen variëren per keer van het jaar, en door de gegevens die wordt doorzocht. Met Azure Cosmos DB, kunt u omhoog of omlaag schalen naar behoefte en betaal alleen voor wat u nodig hebt. U mag niet overeenkomen met dit model, omdat er een manier om het uit bedrijf nemen de hardware voor elk uur met on-premises of gehoste IaaS-systemen. In dergelijke gevallen kunt u mogelijk opslaan tussen 10 tot 14 keer op een gemiddelde met Azure Cosmos DB.

* **Krijgt u gratis talrijke functies:** Schrijf workloads aanzienlijk goedkoper zijn vergeleken met alternatieve database-services in Azure Cosmos DB. Bovendien biedt Azure Cosmos DB functies zoals zoals [automatische indexering](indexing-policies.md), [Time to Live (TTL)](time-to-live.md), [Change Feed](change-feed.md) en anderen zonder eventuele extra kosten iets dat andere databaseservices doorgaans kosten in rekening gebracht.

* **Gebruik unified valuta voor diverse werkbelastingen:** In tegenstelling tot andere aanbiedingen, in Azure Cosmos DB kunt wilt u niet segment werkbelastingen, bijvoorbeeld in lees- en schrijfbewerkingen. Of de doorvoer inrichten op een per type werkbelasting die doorvoer versus de doorvoer van schrijfbewerkingen wordt gelezen. In Azure Cosmos DB, ingerichte doorvoer is gereserveerd met behulp van een uniforme en genormaliseerde valuta in termen van Aanvraageenheden of RU/sec. Azure Cosmos DB u prioriteit toewijzen aan uw workloads, capaciteitsplanning of betalen voor elk type van de capaciteit niet afdwingen afzonderlijk. Deze benadering kunt u eenvoudig interchange de dezelfde RU/s tussen verschillende bewerkingen en type workload.

* **Geen vereist de inrichting van virtuele machines om te schalen:** De meeste operationele databases, moet u met grote virtuele machines om te voorkomen dat luidruchtige buren en voor losse resourcebeheer, gaat u als u schalen wilt. Dit hebt u de werkbelasting en de toezegging van kosten op de klanten. U kunt klein beginnen en groeien in de grootten van de werkbelasting grote schaal naadloos, en zonder uitvaltijd of invloed op de gegevensbeschikbaarheid met Azure Cosmos DB.

* **U kunt gebruikmaken van ingerichte doorvoer naar een maximumlimiet voor:** Door het feit van onderliggende core multiplexing in Azure Cosmos DB, kunt u de ingerichte doorvoer naar een grotere mate dan IaaS opties of aanbiedingen van derden gehost verzadigen. Deze methode bespaart veel meer dan de alternatieve oplossingen.

* **Diepe integratie van Azure Cosmos DB met andere Azure-services.** Azure Cosmos DB heeft een systeemeigen integratie met netwerken, berekening, Azure Functions (serverloze), Azure IoT en andere Azure-services. Met deze integratie kunt u de beste prestaties, de snelheid van de replicatie van gegevens over de hele wereld met robuuste garanties. De oplossingen van derden is niet mogelijk zodat deze overeenkomen met of een premium dergelijke functies bieden doorgaans zou kosten.

* **Krijgt u automatisch een hoge beschikbaarheid, met ten minste 10-20-foutdomeinen standaard:** Azure Cosmos DB ondersteunt de distributie van workloads in domeinen met fouten, een functie die essentieel zijn voor hoge beschikbaarheid. Het biedt 99.999 hoge beschikbaarheid voor leesbewerkingen en schrijft in het 99e percentiel voor elke locatie ter wereld. De kosten van de implementatie van ongeveer het volgende op uw eigen of via een oplossing van derden zijn hoge.

* **Alle enterprise-mogelijkheden, krijgt u automatisch zonder extra kosten.** Azure Cosmos DB biedt de meest uitgebreide set nalevingscertificeringen, beveiliging en versleuteling in rust en in-motion zonder extra kosten (vergeleken met aan onze concurrentie). Regionale beschikbaarheid overal krijgt u automatisch in de hele wereld. U kunt uw database naar een willekeurig aantal Azure-regio's omvatten en toevoegen of verwijderen van regio's op elk gewenst moment.

* **U bespaart tot 65% van de kosten met gereserveerde capaciteit:** Azure Cosmos DB [gereserveerde capaciteit](cosmos-db-reserved-capacity.md) helpt u geld besparen door vooraf betalen voor Azure Cosmos DB-resources voor één jaar of drie jaar. U kunt aanzienlijk verminderen de kosten met een eenjarige of driejarige vooraf toezeggingen en opslaan tussen 20 en 65% kortingen in vergelijking met de reguliere prijzen. U kunt betere Sla's in termen van inrichting van capaciteit krijgen op uw essentiële workloads.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [hoe Azure Cosmos-DB prijsmodel is kostenefficiënt voor klanten](total-cost-ownership.md)
* Meer informatie over [optimaliseren voor de ontwikkeling en testen](optimize-dev-test.md)
* Meer informatie over [doorvoer kosten optimaliseren](optimize-cost-throughput.md)
* Meer informatie over [opslagkosten te optimaliseren](optimize-cost-storage.md)
* Meer informatie over [optimaliseren van de kosten van lees- en schrijfbewerkingen](optimize-cost-reads-writes.md)
* Meer informatie over [de kosten van query's optimaliseren](optimize-cost-queries.md)
* Meer informatie over [optimaliseren van de kosten van de Cosmos-accounts voor meerdere regio's](optimize-cost-regions.md)
* Meer informatie over [de totale eigendomskosten (niet) van een Cloudservice voor NoSQL-Database](https://documentdbportalstorage.blob.core.windows.net/papers/11.15.2017/NoSQL%20TCO%20paper.pdf)

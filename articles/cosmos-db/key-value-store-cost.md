---
title: Azure Cosmos DB als een opslag met sleutelwaarden – kosten-overzicht
description: Meer informatie over de lage kosten van het gebruik van Azure Cosmos DB als een sleutel-waardearchief.
keywords: opslag met sleutelwaarden
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 7331db23757ab4eaae054c7fe640952fe22a7917
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840863"
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure Cosmos DB als een opslag met sleutelwaarden – kosten-overzicht

Azure Cosmos DB is een wereldwijd gedistribueerde, multi-model databaseservice voor het bouwen van maximaal beschikbare, grootschalige toepassingen eenvoudig. Azure Cosmos DB indexeert automatisch alle gegevens, efficiënt neemt standaard. Hiermee maakt u snel en consistent [SQL](how-to-sql-query.md) (en [JavaScript](programming.md))-query's op elk soort gegevens. 

Dit artikel worden de kosten van Azure Cosmos DB voor eenvoudige schrijven en bewerkingen lezen wanneer deze wordt gebruikt als een sleutel/waarde-archief. Voorbeelden van bewerkingen zijn ingevoegd, vervangt, verwijderingen en upsert-bewerking van documenten worden geschreven. Naast het garanderen van een 99,99% beschikbaarheids-SLA voor alle accounts voor één regio en alle accounts voor meerdere regio's met soepele consistentie en 99,999% beschikbaarheid voor lezen op alle accounts voor meerdere regio's, Azure Cosmos DB biedt gegarandeerd < 10 ms latentie voor leesbewerkingen en < 15 ms latentie voor de (geïndexeerde) schrijfbewerkingen respectievelijk in het 99e percentiel. 

## <a name="why-we-use-request-units-rus"></a>Waarom we Aanvraageenheden (ru's) gebruiken

Azure Cosmos DB-prestaties is gebaseerd op het bedrag van de ingerichte [Aanvraageenheden](request-units.md) (RU) voor de partitie. De inrichting is op een tweede granulariteit en wordt aangeschaft in ru's per seconde ([niet te verwarren met de facturering per uur](https://azure.microsoft.com/pricing/details/cosmos-db/)). RU's moeten worden beschouwd als een valuta die vereenvoudigt het inrichten van de vereiste doorvoer voor de toepassing. Onze klanten hoeft niet te beschouwen verschillen tussen lezen en schrijven van capaciteitseenheden. Het model voor één valuta ru's maakt efficiëntie voor het delen van de ingerichte capaciteit tussen de lees- en schrijfbewerkingen. Dit model ingerichte capaciteit kan de service voor een voorspelbare en consistente doorvoer, gegarandeerde lage latentie en hoge beschikbaarheid. Ten slotte RU doorvoer voor het model worden gebruikt, maar elke ingerichte RU heeft ook een vooraf bepaalde hoeveelheid resources (geheugen, Core). RU/sec. is niet alleen IOPS.

Als een wereldwijd gedistribueerde databasesysteem is Cosmos DB het alleen Azure-service een SLA voor latentie, doorvoer en consistentie naast de hoge beschikbaarheid biedt. De doorvoer die u inricht wordt toegepast op elk van de regio's die zijn gekoppeld aan uw Cosmos DB-databaseaccount. Voor leesbewerkingen, Cosmos DB biedt meerdere, duidelijk gedefinieerde [consistentieniveaus](consistency-levels.md) voor u om uit te kiezen. 

De volgende tabel ziet u het aantal ru's die zijn vereist voor het uitvoeren van lezen en schrijven van transacties op basis van de grootte van het document van 1 KB en 100 kB's.

|Grootte van item|1 lezen|1 schrijven|
|-------------|------|-------|
|1 KB|1 RU|5 ru 's|
|100 KB|Tien aanvraageenheden|50 ru 's|

## <a name="cost-of-reads-and-writes"></a>Kosten van lees- en schrijfbewerkingen

Als u 1000 inricht RU/sec., deze hoeveelheden 3.6 m RU/uur en worden kosten $0,08 voor het uur (in de Verenigde Staten en Europa). Voor een document van 1 KB grootte, dit betekent dat 3.6-m leesbewerkingen kunnen worden gebruikt of 0,72-m schrijft (3,6 m RU / 5) met behulp van de ingerichte doorvoer. Genormaliseerd voor miljoen lees- en schrijfbewerkingen, zijn de kosten $0,022 /m leesbewerkingen ($0,08 / 3.6) en $0.111/ m schrijft ($0,08 / 0,72). De kosten per miljoen wordt minimale zoals wordt weergegeven in de onderstaande tabel.

|Grootte van item|1-m lezen|1-m schrijven|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


De meeste van de standaard blob of het object winkels services kosten $0,40 per miljoen lezen transactie en $5 per miljoen Schrijf-transactie. Als u optimaal gebruikt, kan de Cosmos DB 98% goedkoper dan deze andere oplossingen (voor transacties van 1 KB) zijn.

## <a name="next-steps"></a>Volgende stappen

Blijf ons volgen voor nieuwe artikelen over het optimaliseren van de inrichting van Azure Cosmos DB-resources. In de tussentijd kunt u gratis gebruik van onze [RU calculator](https://www.documentdb.com/capacityplanner).


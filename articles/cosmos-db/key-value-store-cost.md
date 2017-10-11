---
title: "Azure Cosmos-database als een winkel sleutelwaarde – kosten overzicht | Microsoft Docs"
description: Meer informatie over de lage kosten van het gebruik van Azure DB die Cosmos als een winkel sleutelwaarde.
keywords: sleutel-waardearchief
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: 
documentationcenter: 
ms.assetid: 7f765c17-8549-4509-9475-46394fc3a218
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: mimig
ms.openlocfilehash: 33eef1b51a5ee00b0fa67096030ed9ce92cf768e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure Cosmos-database als een winkel sleutelwaarde – kosten-overzicht

Azure Cosmos-database is een globaal gedistribueerd en modellen database-service voor het bouwen van maximaal beschikbare, grote schaal toepassingen eenvoudig. Standaard indexeert Azure Cosmos DB automatisch alle gegevens die zijn opgenomen, efficiënt. Hierdoor kunnen snel en consistent [SQL](documentdb-sql-query.md) (en [JavaScript](programming.md)) query's op alle soorten gegevens. 

Dit artikel beschrijft de kosten van Azure DB die Cosmos voor eenvoudige schrijven en leesbewerkingen wanneer deze wordt gebruikt als een sleutel/waarde-archief. Schrijven bewerkingen behoren invoeg-, vervangt, verwijderingen en upserts van documenten. Naast het 99,99% beschikbaarheid te garanderen, biedt Azure Cosmos DB gegarandeerd < 10 ms latentie voor leesbewerkingen en < 15 ms latentie voor de (geïndexeerde) respectievelijk op het 99th percentiel schrijft. 

## <a name="why-we-use-request-units-rus"></a>Waarom we Aanvraageenheden (RUs) gebruiken

Prestaties van Azure DB Cosmos is gebaseerd op de hoeveelheid ingerichte [Aanvraageenheden](request-units.md) (RU) voor de partitie. Het inrichten is op een tweede granulatie en is gekocht in RUs per seconde en RUs/min ([niet te verwarren met het per uur facturering](https://azure.microsoft.com/pricing/details/cosmos-db/)). RUs moeten worden beschouwd als een andere valuta vereenvoudigt de inrichting van de doorvoer vereist voor de toepassing. Onze klanten hoeft niet te zien van de verschillen tussen lezen en schrijven capaciteitseenheden. Het model één valuta van RUs maakt efficiëntie als u wilt delen van de ingerichte capaciteit tussen lees- en schrijfbewerkingen. Dit Capaciteitsmodel ingerichte kan de service bieden voorspelbare en consistente doorvoer, lage latentie en hoge beschikbaarheid worden gegarandeerd. Ten slotte wij RU gebruiken voor het model doorvoer, maar elke ingerichte RU heeft ook een gedefinieerde hoeveelheid resources (geheugen, Core). RU per seconde is niet alleen IOPS.

Als een systeem globaal gedistribueerde database is Cosmos DB het alleen Azure-service die hier wordt een SLA latentie, doorvoer en consistentie naast de hoge beschikbaarheid. De doorvoer die u inricht wordt toegepast op elk van de regio's die zijn gekoppeld aan uw databaseaccount Cosmos DB. Cosmos DB biedt voor leesbewerkingen meerdere, goed gedefinieerde [consistentieniveaus](consistency-levels.md) u kunt kiezen uit. 

De volgende tabel toont dat het aantal RUs vereist uitvoeren lezen en schrijven op basis van de grootte van het document van 1KB en 100KBs transacties.

|De grootte van artikel|1 lezen|1 schrijven|
|-------------|------|-------|
|1 KB|1 RU|5 RUs|
|100 KB|10 RUs|50 RUs|

## <a name="cost-of-reads-and-writes"></a>Kosten van lees- en schrijfbewerkingen

Als u 1.000 inrichten kosten RU per seconde, 3,6 m RU per uur en wordt deze bedragen $0.08 voor het uur (in de Verenigde Staten en Europa). Voor een document van de grootte van 1KB dit betekent dat u 3,6 m leesbewerkingen gebruiken kan of 0.72m schrijft (3.6mRU / 5) met behulp van de ingerichte doorvoer. Genormaliseerd tot miljoen lees- en schrijfbewerkingen, zijn de kosten $0,022 /m leesbewerkingen ($0.08 / 3,6) en $0.111/ m schrijft ($0.08 / 0.72). De kosten per miljoen wordt minimale zoals weergegeven in de onderstaande tabel.

|De grootte van artikel|1m lezen|1m schrijven|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


De meeste van de standaard blob of object winkels services kosten $0,40 per miljoen lezen transactie en $5 per transactie miljoen schrijven. Als u optimaal gebruikt, zijn Cosmos DB 98% goedkoper dan deze andere oplossingen (van 1KB transacties).

## <a name="next-steps"></a>Volgende stappen

Bezoek ons regelmatig voor nieuwe artikelen voor het optimaliseren van Azure DB die Cosmos resources worden ingericht. In de tussentijd gerust gebruiken onze [RU Rekenmachine](https://www.documentdb.com/capacityplanner).


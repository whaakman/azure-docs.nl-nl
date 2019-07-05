---
title: Gegevens in Azure Cosmos DB met Time to Live laten verlopen
description: TTL-waarde biedt Microsoft Azure Cosmos DB de mogelijkheid om documenten automatisch verwijderd uit het systeem na een bepaalde periode.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 0b32665b09eb02c337a12ac3cfc2b474fa82711a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447251"
---
# <a name="time-to-live-ttl-in-azure-cosmos-db"></a>Time to Live (TTL) in Azure Cosmos DB 

Met **Time to Live** of TTL, Azure Cosmos DB biedt de mogelijkheid items om automatisch te verwijderen uit een container na een bepaalde periode. Standaard kunt u tijd te bevinden zich op het niveau van de container en de waarde op basis van de per-item overschrijven instellen. Nadat de TTL-waarde is ingesteld op een container of op het itemniveau van een, worden Azure Cosmos DB automatisch deze items verwijderd na de tijd sinds de tijd dat ze het laatst zijn gewijzigd. Waarde time to live is geconfigureerd in een paar seconden. Wanneer u TTL configureert, worden de verlopen items op basis van de TTL-waarde, zonder een verwijderbewerking dat expliciet is uitgegeven door de clienttoepassing automatisch verwijderd door het systeem.

## <a name="time-to-live-for-containers-and-items"></a>Time to live voor containers en objecten

De waarde time to live in seconden is ingesteld en dit wordt geïnterpreteerd als een delta vanaf het moment waarop een item voor het laatst is gewijzigd. U kunt de time to live van een container of een item in de container van instellen:

1. **Time to Live voor een container** (ingesteld met behulp van `DefaultTimeToLive`):

   - Indien ontbrekend (of is ingesteld op null), items worden niet automatisch verlopen.

   - Als het aanwezig zijn en de waarde is ingesteld op '-1', is gelijk aan oneindig en items verloopt niet standaard.

   - Als aanwezig zijn en de waarde is ingesteld op een nummer *"n"* – items verloopt *"n"* seconden na de laatste keer is gewijzigd.

2. **Time to Live op een item** (ingesteld met behulp van `ttl`):

   - Deze eigenschap geldt alleen als `DefaultTimeToLive` aanwezig is en deze niet is ingesteld op null voor de bovenliggende container.

   - Indien aanwezig, dit vervangt de `DefaultTimeToLive` waarde van de bovenliggende container.

## <a name="time-to-live-configurations"></a>Tijd voor Live-configuraties

* Als de TTL-waarde is ingesteld op *"n"* voor een container, klikt u vervolgens de items in deze container verloopt na *n* seconden.  Er zijn items in dezelfde container die hun eigen tijd hebt om live, ingesteld op-1 (indicatie dat ze niet verlopen) of als sommige items waarvoor time to live instellen met een ander nummer worden genegeerd, deze items verloopt op basis van hun eigen geconfigureerde TTL-waarde. 

* Als de TTL is niet ingesteld voor een container, heeft geen effect met time to live op een item in deze container. 

* Als de TTL voor een container is ingesteld op-1, een item in deze container met time to live is ingesteld op n, verloopt na n seconden en overige items verloopt niet. 

Verwijderen van objecten op basis van de TTL is gratis. Er zijn geen extra kosten verbonden (dat wil zeggen, er zijn geen extra ru's worden verbruikt) wanneer het item wordt verwijderd als gevolg van de TTL is verlopen.

## <a name="examples"></a>Voorbeelden

In deze sectie ziet u enkele voorbeelden met andere time to live van waarden die zijn toegewezen aan de container en items:

### <a name="example-1"></a>Voorbeeld 1

TTL voor de container is ingesteld op null (DefaultTimeToLive = null)

|TTL voor item| Resultaat|
|---|---|
|TTL = null|    TTL is uitgeschakeld. Het item verloopt nooit (standaard).|
|ttl = -1   |TTL is uitgeschakeld. Het item verloopt nooit.|
|ttl = 2000 |TTL is uitgeschakeld. Het item verloopt nooit.|


### <a name="example-2"></a>Voorbeeld 2

TTL voor de container is ingesteld op-1 (DefaultTimeToLive =-1)

|TTL voor item| Resultaat|
|---|---|
|TTL = null |TTL is ingeschakeld. Het item verloopt nooit (standaard).|
|ttl = -1   |TTL is ingeschakeld. Het item verloopt nooit.|
|ttl = 2000 |TTL is ingeschakeld. Het item verloopt na 2000 seconden.|


### <a name="example-3"></a>Voorbeeld 3

TTL voor de container is ingesteld op 1000 (DefaultTimeToLive = 1000)

|TTL voor item| Resultaat|
|---|---|
|TTL = null|    TTL is ingeschakeld. Het item verloopt na 1000 seconden (standaard).|
|ttl = -1   |TTL is ingeschakeld. Het item verloopt nooit.|
|ttl = 2000 |TTL is ingeschakeld. Het item verloopt na 2000 seconden.|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het configureren van Time to Live in de volgende artikelen:

* [Time to Live configureren](how-to-time-to-live.md)

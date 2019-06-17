---
title: Veelvoorkomende problemen oplossen in Azure Cosmos DB-API voor Mongodb
description: Dit document worden beschreven van de manieren om het oplossen van algemene problemen aangetroffen in Azure Cosmos DB-API voor MongoDB.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: roaror
ms.openlocfilehash: 5b3d3993a497240c1ea18f0fcf852c0e834f6e79
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735704"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>Oplossen van veelvoorkomende problemen in Azure Cosmos DB-API voor MongoDB

Azure Cosmos DB implementeert de wire-protocollen van veelvoorkomende NoSQL-databases, met inbegrip van MongoDB. Vanwege de wire-protocol-implementatie, kunt u transparant werken met Azure Cosmos DB met behulp van de bestaande client-SDK's, stuurprogramma's en hulpprogramma's die met NoSQL-databases werken. Azure Cosmos DB maakt geen gebruik van alle broncode van de databases voor het ontwikkelen van wire-compatibele API's voor een van de NoSQL-databases. Een MongoDB-clientstuurprogramma die werkt met de versies wire-protocol kan verbinding maken met Azure Cosmos DB.

Azure Cosmos DB-API voor MongoDB is compatibel met 3.2 versie van de MongoDB wire-protocol (de standaardoperators voor query's en onderdelen die zijn toegevoegd in versie 3.4 zijn momenteel beschikbaar als preview), maar er zijn bepaalde aangepaste foutcodes die overeenkomen met de Azure Cosmos DB specifieke fouten. In dit artikel wordt uitgelegd voor verschillende fouten, foutcodes en de stappen voor het oplossen van deze fouten.

## <a name="common-errors-and-solutions"></a>Veelvoorkomende fouten en oplossingen

| Fout               | Code  | Description  | Oplossing  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Het totale aantal aanvraageenheden verbruikt meer is dan de frequentie van de ingerichte aanvraageenheid voor de verzameling en is beperkt. | Houd rekening met schaal de doorvoer die is toegewezen aan een container of een set van containers in Azure portal of u kunt de bewerking opnieuw. |
| ExceededMemoryLimit | 16501 | Als een service met meerdere tenants, heeft de bewerking van de client geheugen toegewezen overschreden. | Verklein het bereik van de bewerking door meer beperkende querycriteria of neem contact op met ondersteuning van de [Azure-portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Voorbeeld: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| Problemen met de versie van de MongoDB kabel | - | De oudere versies van de MongoDB-stuurprogramma's kan niet voor het detecteren van de naam van de Azure Cosmos-account in de verbindingsreeksen worden. | Toevoeg- *appName = @**accountName** @*  aan het einde van uw Cosmos DB-API voor MongoDB-verbindingsreeks, waar ***accountName*** is de naam van uw Cosmos DB-account . |


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Studio 3T](mongodb-mongochef.md) met de API voor MongoDB van Azure Cosmos DB.
- Meer informatie over het [gebruik van Robo 3T](mongodb-robomongo.md) met de API voor MongoDB van Azure Cosmos DB.
- Verken [voorbeelden](mongodb-samples.md) van MongoDB met de API voor MongoDB van Azure Cosmos DB.


---
title: Over het maken van meerdere onafhankelijke Azure Cosmos DB-Triggers
description: Informatie over het configureren van meerdere onafhankelijke Azure Cosmos DB-Triggers voor het maken van gebeurtenisgestuurde architecturen voor Azure Functions.
author: ealsur
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: maquaran
ms.openlocfilehash: 722da9f0112d63af52be8c9c3a746f6da9638bac
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241955"
---
# <a name="create-multiple-azure-cosmos-db-triggers"></a>Meerdere Azure Cosmos DB-Triggers maken

Dit artikel wordt beschreven hoe u meerdere Cosmos DB-Triggers voor het parallel werken en onafhankelijk te reageren op wijzigingen kunt configureren.

![Serverloze op gebeurtenissen gebaseerde-functies werken met de Azure Cosmos DB-Trigger en het delen van een container leases](./media/change-feed-functions/multi-trigger.png)

## <a name="event-based-architecture-requirements"></a>Vereisten voor architectuur op basis van gebeurtenissen

Bij het bouwen van serverloze architecturen met een [Azure Functions](../azure-functions/functions-overview.md), heeft [aanbevolen](../azure-functions/functions-best-practices.md#avoid-long-running-functions) om kleine functie sets die in plaats van grote langlopende functies samenwerken te maken.

Tijdens het ontwikkelen van serverloze stromen op basis van gebeurtenissen met behulp van de [Azure Cosmos DB-Trigger](./change-feed-functions.md), moet u uitvoeren in het scenario waar u wilt meerdere dingen doen wanneer er sprake is van een nieuwe gebeurtenis in een bepaalde [Azure Cosmos-container](./databases-containers-items.md#azure-cosmos-containers). Als u wilt activeren, acties zijn onafhankelijk van elkaar, de ideale oplossing zou zijn om te **maken van een Cosmos DB-Trigger per actie** u wilt doen, alle luisteren naar wijzigingen in de dezelfde Azure-Cosmos-container.

## <a name="optimizing-containers-for-multiple-triggers"></a>Containers optimaliseren voor meerdere Triggers

Gezien de *vereisten* van de Cosmos DB-Trigger, moeten we een tweede container voor het opslaan van status, ook wel genoemd, de *leases container*. Betekent dit dat u een afzonderlijke leases container voor elke Azure-functie moet?

Hier hebt u twee opties:

* Maak **een lease-container per functie**: Deze benadering kunt vertalen in extra kosten, tenzij u een [gedeelde doorvoer database](./set-throughput.md#set-throughput-on-a-database). Denk eraan dat dat de minimale doorvoer op het niveau van de container is 400 [Aanvraageenheden](./request-units.md), en in het geval van de container leases, deze alleen worden gebruikt voor het controlepunt van de voortgang en status onderhouden.
* Hebben **een container van de lease en deel het** voor uw functies: Deze tweede optie beter gebruik maakt van de ingerichte Aanvraageenheden voor de container, meerdere Azure Functions om te delen en gebruiken van de dezelfde ingerichte doorvoer is ingeschakeld.

Het doel van dit artikel is om u te helpen u om uit te voeren van de tweede optie.

## <a name="configuring-a-shared-leases-container"></a>Een gedeelde leases-container configureren

Voor het configureren van de container gedeelde leases, wordt het alleen extra configuratie die u wilt aanbrengen in uw triggers is het toevoegen van de `LeaseCollectionPrefix` [kenmerk](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---c-attributes) als u C# of `leaseCollectionPrefix` [kenmerk](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---javascript-example)als u van JavaScript gebruikmaakt. De waarde van het kenmerk moet een logische descriptor van welke dat bepaalde trigger.

Bijvoorbeeld, hebt u drie Triggers: een stuurt e-mailberichten, die een samenvoeging voor het maken van een gerealiseerde weergave heeft en dat de wijzigingen naar een andere opslag verzendt voor latere analyse kunt u toewijzen de `LeaseCollectionPrefix` 'e-mailadressen van' naar de eerste, " gerealiseerde' voor de tweede waarde en 'analytics' op de derde.

Het belangrijkste is dat alle drie wordt geactiveerd **kunt dezelfde configuratie van de container leases** (account, database en container-naam).

Een zeer eenvoudige codevoorbeelden met behulp van de `LeaseCollectionPrefix` kenmerk in C#, zou er als volgt:

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

[FunctionName("SendEmails")]
public static void SendEmails([CosmosDBTrigger(
    databaseName: "ToDoItems",
    collectionName: "Items",
    ConnectionStringSetting = "CosmosDBConnection",
    LeaseCollectionName = "leases",
    LeaseCollectionPrefix = "emails")]IReadOnlyList<Document> documents,
    ILogger log)
{
    ...
}

[FunctionName("MaterializedViews")]
public static void MaterializedViews([CosmosDBTrigger(
    databaseName: "ToDoItems",
    collectionName: "Items",
    ConnectionStringSetting = "CosmosDBConnection",
    LeaseCollectionName = "leases",
    LeaseCollectionPrefix = "materialized")]IReadOnlyList<Document> documents,
    ILogger log)
{
    ...
}
```

En voor JavaScript, kunt u de configuratie toepassen op de `function.json` -bestand met de `leaseCollectionPrefix` kenmerk:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "CosmosDBConnection",
    "databaseName": "ToDoItems",
    "collectionName": "Items",
    "leaseCollectionPrefix": "emails"
},
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "CosmosDBConnection",
    "databaseName": "ToDoItems",
    "collectionName": "Items",
    "leaseCollectionPrefix": "materialized"
}
```

> [!NOTE]
> Altijd controleren op de Aanvraageenheden ingericht voor uw container gedeelde leases. Elke Trigger die deelt, wordt het gemiddelde gebruik van de doorvoer verhoogd, zodat u mogelijk de ingerichte doorvoer te verhogen als u het aantal Azure-functies die van het gebruikmaken verhogen.

## <a name="next-steps"></a>Volgende stappen

* Zie de volledige configuratie voor de [Azure Cosmos DB-Trigger](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration)
* Controleer de uitgebreide [lijst met voorbeelden](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---example) voor alle talen.
* Ga naar de Serverloze recepten met Azure Cosmos DB en Azure Functions [GitHub-opslagplaats](https://github.com/ealsur/serverless-recipes/tree/master/cosmosdbtriggerscenarios) voor meer voorbeelden.
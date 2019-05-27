---
title: Het gebruik van Azure Cosmos DB-wijzigingenfeed met Azure Functions
description: Gebruik Azure Cosmos DB-wijzigingenfeed met Azure Functions
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 08429ca76823b9e6c80a197cc390a5964c4198e6
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969005"
---
# <a name="serverless-event-based-architectures-with-azure-cosmos-db-and-azure-functions"></a>Serverloze op gebeurtenissen gebaseerde architecturen met een Azure Cosmos DB en Azure Functions

Azure Functions biedt de eenvoudigste manier om verbinding maken met de [wijzigingenfeed](change-feed.md). Bij elke nieuwe gebeurtenis in uw Azure Cosmos-container wijzigingenfeed kunt u kleine reactieve Azure Functions die automatisch wordt geactiveerd.

![Serverloze functies maken op basis van gebeurtenissen werken met de Azure Cosmos DB-Trigger](./media/change-feed-functions/functions.png)

Met de [Azure Cosmos DB-Trigger](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger), kunt u gebruikmaken van de [Change Feed Processor](./change-feed-processor.md)het schalen en betrouwbare gebeurtenis detectie functionaliteit zonder de noodzaak om te onderhouden een [worker infrastructuur](./change-feed-processor.md#implementing-the-change-feed-processor-library). Concentreert zich alleen op uw Azure-functie logische zonder u zorgen te maken over de rest van de pijplijn ' event sourcing '. U kunt ook de Trigger combineren met andere [Azure Functions-bindingen](../azure-functions/functions-triggers-bindings.md#supported-bindings).

> [!NOTE]
> De Azure Cosmos DB-trigger wordt momenteel ondersteund voor gebruik met de Core (SQL) API alleen.

## <a name="requirements"></a>Vereisten

Voor het implementeren van een serverloze stroom op basis van gebeurtenissen, hebt u het volgende nodig:

* **De bewaakte container**: De bewaakte container is de Azure Cosmos-container die worden bewaakt en de gegevens op basis waarvan de wijzigingenfeed is gegenereerd worden opgeslagen. Alle toevoegingen en wijzigingen (bijvoorbeeld CRUD) naar de bewaakte container worden weerspiegeld in de wijzigingenfeed van de container.
* **De container lease**: De lease-container de status over meerdere bijgehouden en dynamische serverloze Azure Function-exemplaren en kunnen dynamische schaalbaarheid. Deze container lease kan worden handmatig of automatisch gemaakt door de Azure Cosmos DB Trigger.To automatisch maken van de lease-container, stelt u de *CreateLeaseCollectionIfNotExists* markering waarmee wordt aangegeven de [configuratie](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration). Gepartitioneerde lease-containers zijn vereist om een `/id` sleuteldefinitie partitioneren.

## <a name="create-your-azure-cosmos-db-trigger"></a>Uw Azure Cosmos DB-Trigger maken

Het maken van uw Azure-functie met een Azure Cosmos DB-Trigger wordt nu ondersteund in alle Azure-functies IDE en integraties CLI:

* [Visual Studio-extensie](../azure-functions/functions-develop-vs.md) voor gebruikers van Visual Studio.
* [Extensie van Visual Studio-Core](https://code.visualstudio.com/tutorials/functions-extension/create-function) voor gebruikers van Visual Studio Code.
* En ten slotte [Core CLI tooling](../azure-functions/functions-run-local.md#create-func) voor een platformoverschrijdende IDE agnostische ervaring.

## <a name="run-your-azure-cosmos-db-trigger-locally"></a>Uw Azure Cosmos DB-Trigger lokaal uitvoeren

U kunt uitvoeren uw [lokaal Azure-functie](../azure-functions/functions-develop-local.md) met de [Azure Cosmos DB-Emulator](./local-emulator.md) wilt maken en ontwikkelen van uw serverloze stromen op basis van gebeurtenissen zonder een Azure-abonnement of er kosten in rekening.

Als u testen van scenario's voor live in de cloud wilt, kunt u [Cosmosdb gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/) zonder creditcard of Azure-abonnement nodig.

## <a name="next-steps"></a>Volgende stappen

U kunt nu doorgaan naar meer informatie over de wijzigingenfeed in de volgende artikelen:

* [Overzicht van de wijzigingenfeed](change-feed.md)
* [Manieren om te lezen wijzigingenfeed](read-change-feed.md)
* [Met behulp van de change feed processor-bibliotheek](change-feed-processor.md)
* [Over het werken met change feed processor-bibliotheek](change-feed-processor.md)
* [Serverless database computing met behulp van Azure Cosmos DB en Azure Functions](serverless-computing-database.md)

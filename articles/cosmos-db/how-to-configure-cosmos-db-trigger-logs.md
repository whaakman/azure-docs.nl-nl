---
title: Logboeken in Azure Cosmos DB-Trigger
description: Meer informatie over het weergeven van de Azure Cosmos DB-Trigger logboeken naar uw Azure-Functions pijplijn logboekregistratie
author: ealsur
ms.service: cosmos-db
ms.topic: sample
ms.date: 5/2/2019
ms.author: maquaran
ms.openlocfilehash: a598842ecde9508a6c2185a6097f689252fda60c
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65515079"
---
# <a name="how-to-configure-and-read-the-azure-cosmos-db-trigger-logs"></a>Over het configureren en het lezen van de logboeken van Azure Cosmos DB-Trigger

Dit artikel wordt beschreven hoe u uw Azure Functions-omgeving voor het verzenden van de Azure Cosmos DB-Trigger-logboeken naar uw geconfigureerde kunt configureren [bewakingsoplossing](../azure-functions/functions-monitoring.md).

## <a name="included-logs"></a>Opgenomen Logboeken

De Azure Cosmos DB-Trigger gebruikt de [Change Feed Processor-bibliotheek](./change-feed-processor.md) intern, en de bibliotheek genereert een set van health-logboeken die kan worden gebruikt voor het bewaken van interne bewerkingen voor [oplossen van problemen](./troubleshoot-changefeed-functions.md).

De health-Logboeken wordt beschreven hoe de Azure Cosmos DB-Trigger zich gedraagt wanneer wordt geprobeerd bewerkingen tijdens de taakverdelings-scenario's of de initialisatie.

## <a name="enabling-logging"></a>Logboekregistratie inschakelen

Azure Cosmos DB-Trigger als logboekregistratie wilt inschakelen, zoek de `host.json` -bestand in uw Azure Functions-project of Azure Functions-App en [configureren van het niveau van logboekregistratie vereist](../azure-functions/functions-monitoring.md#log-configuration-in-hostjson). U moet de traceringen van inschakelen `Host.Triggers.CosmosDB` zoals wordt weergegeven in het volgende voorbeeld:

```js
{
  "version": "2.0",
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "Host.Triggers.CosmosDB": "Trace"
    }
  }
}
```

Nadat de Azure-functie is geïmplementeerd met de bijgewerkte configuratie, ziet u de logboeken van Azure Cosmos DB-Trigger als onderdeel van uw traceringen. U kunt de logboeken bekijken in de geconfigureerde logboekregistratie provider onder de *categorie* `Host.Triggers.CosmosDB`.

## <a name="query-the-logs"></a>Query uitvoeren op de logboeken

Voer de volgende query-query de logboeken die worden gegenereerd door de Azure Cosmos DB-Trigger in [Azure Application Insights Analytics](../azure-monitor/app/analytics.md):

```sql
traces
| where customDimensions.Category == "Host.Triggers.CosmosDB"
```

## <a name="next-steps"></a>Volgende stappen

* [Schakel de bewaking](../azure-functions/functions-monitoring.md) in uw Azure Functions-toepassingen.
* Meer informatie over het [vaststellen en oplossen van algemene problemen](./troubleshoot-changefeed-functions.md) bij het gebruik van Azure Cosmos DB-Trigger in Azure Functions.
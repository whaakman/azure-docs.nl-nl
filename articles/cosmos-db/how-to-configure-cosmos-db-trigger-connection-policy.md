---
title: Azure Cosmos DB-Trigger verbindingsbeleid
description: Informatie over het configureren van het verbindingsbeleid die worden gebruikt door Azure Cosmos DB-Trigger
author: ealsur
ms.service: cosmos-db
ms.topic: sample
ms.date: 06/05/2019
ms.author: maquaran
ms.openlocfilehash: 584d59884b70d2ee8243216e6f907fc9ec2d8ad4
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755330"
---
# <a name="how-to-configure-the-connection-policy-used-by-azure-cosmos-db-trigger"></a>De verbindingsbeleid die worden gebruikt door Azure Cosmos DB-Trigger configureren

Dit artikel wordt beschreven hoe u het verbindingsbeleid kunt configureren bij het gebruik van de Azure Cosmos DB-Trigger voor verbinding met uw Azure Cosmos-account.

## <a name="why-is-the-connection-policy-important"></a>Waarom is het verbindingsbeleid belangrijk?

Er zijn twee verbinding modi - Direct en Gateway-modus. Zie voor meer informatie over deze verbinding-modi, de [tips voor betere prestaties](./performance-tips.md#networking) artikel. Standaard **Gateway** wordt gebruikt voor alle verbindingen op de Azure Cosmos DB-Trigger. Het kan echter niet de beste optie voor prestaties gebaseerde scenario's zijn.

## <a name="changing-the-connection-mode-and-protocol"></a>Wijzigen van de verbindingsmodus en protocol

Er zijn twee belangrijke configuratie-instellingen die beschikbaar zijn voor het configureren van het beleid van de client verbinding – de **verbindingsmodus** en de **verbindingsprotocol**. U kunt de standaardmodus voor de verbinding en protocol wordt gebruikt door de Azure Cosmos DB-Trigger en alle wijzigen de [Azure Cosmos DB-bindingen](../azure-functions/functions-bindings-cosmosdb-v2.md#output)). Als u wilt de standaardinstellingen wijzigen, moet u zoekt de `host.json` bestand in uw Azure Functions-project of Azure Functions-App en voeg de volgende [extra instelling](../azure-functions/functions-bindings-cosmosdb-v2.md#hostjson-settings):

```js
{
  "cosmosDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

Waar `connectionMode` moet de gewenste verbindingsmodus (Direct of Gateway) en `protocol` de gewenste verbindingsprotocol (TCP- of Https). 

Als uw Azure Functions-project werkt met Azure Functions V1-runtime, de configuratie heeft een verschil lichte naam, moet u `documentDB` in plaats van `cosmosDB`:

```js
{
  "documentDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

> [!NOTE]
> Als u werkt met Azure Functions Hosting via een Verbruiksabonnement plan, heeft elk exemplaar een limiet van socketverbindingen die deze kunt beheren. Als u werkt met Direct / TCP-modus door ontwerp meer verbindingen zijn gemaakt en kan er een druk op de [Verbruiksabonnement limiet](../azure-functions/manage-connections.md#connection-limit), in dat geval kunt u Gateway-modus gebruiken of uw Azure-functies uitvoeren in [App Service-modus](../azure-functions/functions-scale.md#app-service-plan).

## <a name="next-steps"></a>Volgende stappen

* [Limieten voor de verbindingen in Azure Functions](../azure-functions/manage-connections.md#connection-limit)
* [Azure Cosmos DB tips voor betere prestaties](./performance-tips.md)
* [Codevoorbeelden](https://github.com/ealsur/serverless-recipes/tree/master/connectionmode)

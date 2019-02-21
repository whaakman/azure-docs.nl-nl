---
title: Verbindingsreeks - Azure Event Hubs ophalen | Microsoft Docs
description: In dit artikel vindt u instructies voor het ophalen van een verbindingsreeks die clients verbinding maken met Azure Event Hubs kunnen gebruiken.
services: event-hubs
documentationcenter: na
author: spelluru
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 02/19/2019
ms.author: spelluru
ms.openlocfilehash: edd197fb6d578df064c67a422767e3e70a0c8142
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56445097"
---
# <a name="get-an-event-hubs-connection-string"></a>Een Event Hubs-verbindingsreeks ophalen

Voor het gebruik van Event Hubs, moet u een Event Hubs-naamruimte maken. Een naamruimte is een scoping container voor meerdere eventhubs of Kafka-onderwerpen. Deze naamruimte biedt u een unieke [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Zodra een naamruimte is gemaakt, kunt u de verbindingsreeks die nodig zijn voor communicatie met Event Hubs kunt verkrijgen.

De verbindingsreeks voor Azure Event Hubs bevat de volgende onderdelen die zijn ingesloten in het

* FQDN-naam = de FQDN-naam van de Event hubs-naamruimte die u hebt gemaakt (deze bevat de naam van de Event hubs-naamruimte gevolgd door servicebus.windows.net)
* SharedAccessKeyName = de naam die u hebt gekozen voor de SAS-sleutels van uw toepassing
* SharedAccessKey = de gegenereerde waarde van de sleutel.

De connection string sjabloon ziet eruit als
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

Een voorbeeld-verbindingsreeks uitzien als volgt `Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

In dit artikel leidt u door de verschillende manieren voor het verkrijgen van de verbindingsreeks.

## <a name="get-connection-string-from-the-portal"></a>Verbindingsreeks ophalen uit de portal
1. Meld u aan bij de [Azure-portal](https://portal.azure.com). 
2. Selecteer **alle services** in het linkermenu van de navigatie. 
3. Selecteer **Event Hubs** in de **Analytics** sectie. 
4. Selecteer in de lijst van eventhubs, de event hub.
6. Op de **Event Hubs Namespace** weergeeft, schakelt **gedeeld toegangsbeleid** in het menu links.

    ![Gedeeld toegangsbeleid menu-item](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)
7. Selecteer een **gedeeld toegangsbeleid** in de lijst met beleidsregels. De standaard één met de naam: **RootManageSharedAccessPolicy**. U kunt een beleid met de juiste machtigingen (lezen, schrijven) toevoegen, en dat beleid. 

    ![Eventhubs gedeelde toegangsbeleid](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)
8. Selecteer de **kopie** naast de **verbindingsreeks-primaire sleutel** veld. 

    ![Eventhubs - ophalen verbindingsreeks](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>De verbindingsreeks met Azure PowerShell ophalen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt de [Get-AzEventHubNamespaceKey](/powershell/module/az.eventhub/get-azeventhubkey) om op te halen van de verbindingsreeks voor de naam van de specifieke beleidsregel, zoals hieronder wordt weergegeven:

```azurepowershell-interactive
Get-AzEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

## <a name="getting-the-connection-string-with-azure-cli"></a>Ophalen van de verbindingsreeks met Azure CLI
U kunt het volgende gebruiken om op te halen van de verbindingsreeks voor de naamruimte:

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Zie voor meer informatie over Azure CLI-opdrachten voor Event Hubs, [Azure CLI voor Event Hubs](/cli/azure/eventhubs).

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)

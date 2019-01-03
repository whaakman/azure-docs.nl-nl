---
title: Verbindingsreeks - Azure Event Hubs ophalen | Microsoft Docs
description: In dit artikel vindt u instructies voor het ophalen van een verbindingsreeks die clients verbinding maken met Azure Event Hubs kunnen gebruiken.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 31220002f8529fd31407470e7650a4c97b62f2b4
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53535266"
---
# <a name="get-an-event-hubs-connection-string"></a>Een Event Hubs-verbindingsreeks ophalen

Voor het gebruik van Event Hubs, moet u een Event Hubs-naamruimte maken. Een naamruimte is een scoping container dat met meerdere Event Hubs werken kan / Kafka-onderwerpen. Deze naamruimte biedt u een unieke [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Zodra een naamruimte is gemaakt, kunt u de verbindingsreeks die nodig zijn voor communicatie met Event Hubs kunt verkrijgen.

De verbindingsreeks voor Azure Event Hubs bevat de volgende onderdelen die zijn ingesloten in het

* FQDN-naam = de FQDN-naam van de Event hubs-naamruimte die u hebt gemaakt (dit is inclusief de naam van de Event hubs-naamruimte gevolgd door servicebus.windows.net)
* SharedAccessKeyName = de naam die u hebt gekozen voor de SAS-sleutels van uw toepassing
* SharedAccessKey = de gegenereerde waarde van de sleutel.

De connection string sjabloon ziet eruit als
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

Een voorbeeld-verbindingsreeks uitzien als volgt `Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

In dit artikel leidt u door de verschillende manieren voor het verkrijgen van de verbindingsreeks.

## <a name="get-connection-string-from-the-portal"></a>Verbindingsreeks ophalen uit de portal

Zodra u de Event Hubs-naamruimte hebt, geeft de sectie overzicht van de portal u de verbindingsreeks, zoals hieronder weergegeven:

![Event Hubs-verbindingsreeks](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)

Als u op de koppeling van de connection string in de overzichtssectie klikt, wordt deze geopend het SAS-beleid tabblad zoals wordt weergegeven in de afbeelding hieronder:

![Eventhubs SAS-beleid](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)

U kunt een nieuwe SAS-beleid toevoegen en de verbindingsreeks ophalen of gebruiken van het standaardbeleid dat al voor u is gemaakt. Wanneer het beleid wordt geopend, de verbindingsreeks wordt verkregen, zoals wordt weergegeven in de onderstaande afbeelding:

![Eventhubs ophalen-verbindingsreeks](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>De verbindingsreeks met Azure PowerShell ophalen
U kunt de Get-AzureRmEventHubNamespaceKey gebruiken om op te halen van de verbindingsreeks voor de naam van de specifieke beleidsregel zoals hieronder wordt weergegeven:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

Raadpleeg [Azure Event Hubs PowerShell-module](https://docs.microsoft.com/powershell/module/azurerm.eventhub/get-azurermeventhubkey) voor meer informatie.

## <a name="getting-the-connection-string-with-azure-cli"></a>Ophalen van de verbindingsreeks met Azure CLI
U kunt het volgende gebruiken om op te halen van de verbindingsreeks voor de naamruimte:

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Raadpleeg [Azure CLI voor Event Hubs](https://docs.microsoft.com/cli/azure/eventhubs) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)

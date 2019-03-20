---
title: Query uitvoeren op Azure Event Grid-abonnementen
description: Hierin wordt beschreven hoe u Azure Event Grid-abonnementen.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: spelluru
ms.openlocfilehash: ad9c2d492f70a697ef0e7dc3b7ed03b9938f2468
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58181687"
---
# <a name="query-event-grid-subscriptions"></a>Query uitvoeren op Event Grid-abonnementen 

Dit artikel wordt beschreven hoe u de Event Grid-abonnementen in uw Azure-abonnement. Bij het opvragen van uw bestaande Event Grid-abonnementen, is het belangrijk om te begrijpen welke verschillende typen abonnementen. U opgeven verschillende parameters die op basis van het type abonnement die u wilt ophalen.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="resource-groups-and-azure-subscriptions"></a>Resourcegroepen en Azure-abonnementen

Azure-abonnementen en resourcegroepen zijn niet de Azure-resources. Daarom event grid-abonnementen aan resourcegroepen of Azure-abonnementen beschikt niet over dezelfde eigenschappen als event grid-abonnementen voor Azure-resources. Event grid-abonnementen aan resourcegroepen of Azure-abonnementen worden beschouwd als global.

Voor event grid-abonnementen voor een Azure-abonnement en bijbehorende resourcegroepen, moet u niet alle parameters opgeven. Zorg ervoor dat u hebt geselecteerd dat het Azure-abonnement dat u wilt zoeken. De volgende voorbeelden ophalen niet van event grid-abonnementen voor aangepaste onderwerpen of Azure-resources.

Gebruik voor Azure CLI:

```azurecli-interactive
az account set -s "My Azure Subscription"
az eventgrid event-subscription list
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Set-AzContext -Subscription "My Azure Subscription"
Get-AzEventGridSubscription
```

Voor event grid-abonnementen voor een Azure-abonnement, bieden het Onderwerptype **Microsoft.Resources.Subscriptions**.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.Subscriptions"
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.Resources.Subscriptions"
```

Voor event grid-abonnementen voor alle resourcegroepen binnen een Azure-abonnement, bieden het Onderwerptype **Microsoft.Resources.ResourceGroups**.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.ResourceGroups"
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.Resources.ResourceGroups"
```

Voor event grid-abonnementen voor een opgegeven resourcegroep gemaakt, moet u de naam van de resourcegroep opgeven als een parameter.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --resource-group myResourceGroup
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzEventGridSubscription -ResourceGroupName myResourceGroup
```

## <a name="custom-topics-and-azure-resources"></a>Aangepaste onderwerpen en Azure-resources

Aangepast Event grid-onderwerpen zijn Azure-resources. U kunt daarom event grid-abonnementen voor aangepaste onderwerpen en andere resources, zoals Blob storage-account, query op dezelfde manier. Als u event grid-abonnementen voor aangepaste onderwerpen, moet u de parameters die de bron identificeren of geef de locatie van de resource opgeven. Het is niet mogelijk is te breed query event grid-abonnementen voor resources in uw Azure-abonnement.

Geef voor event grid-abonnementen voor aangepaste onderwerpen en andere bronnen in een locatie, de naam van de locatie.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --location westus2
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzEventGridSubscription -Location westus2
```

Voor abonnementen op aangepaste onderwerpen om een locatie, geef de locatie en het Onderwerptype **Microsoft.EventGrid.Topics**.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.EventGrid.Topics" --location "westus2"
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.EventGrid.Topics" -Location westus2
```

Voor abonnementen op storage-accounts om een locatie, geef de locatie en het Onderwerptype **Microsoft.Storage.StorageAccounts**.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --topic-type "Microsoft.Storage.StorageAccounts" --location westus2
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.Storage.StorageAccounts" -Location westus2
```

Als u event grid-abonnementen voor een aangepast onderwerp, bieden u de naam van het aangepaste onderwerp en de naam van de resourcegroep.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --topic-name myCustomTopic --resource-group myResourceGroup
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicName myCustomTopic -ResourceGroupName myResourceGroup
```

Als u event grid-abonnementen voor een bepaalde resource, bieden de resource-ID.

Gebruik voor Azure CLI:

```azurecli-interactive
resourceid=$(az resource show -n mystorage -g myResourceGroup --resource-type "Microsoft.Storage/storageaccounts" --query id --output tsv)
az eventgrid event-subscription list --resource-id $resourceid
```

Gebruik voor PowerShell:

```azurepowershell-interactive
$resourceid = (Get-AzResource -Name mystorage -ResourceGroupName myResourceGroup).ResourceId
Get-AzEventGridSubscription -ResourceId $resourceid
```

## <a name="next-steps"></a>Volgende stappen

* Voor informatie over de bezorging van gebeurtenissen en nieuwe pogingen, [bezorging van berichten van Event Grid en probeer het opnieuw](delivery-and-retry.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).

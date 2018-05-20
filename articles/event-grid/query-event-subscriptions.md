---
title: Query uitvoeren op Azure gebeurtenis raster abonnementen
description: Hierin wordt beschreven hoe Azure gebeurtenis raster abonnementen.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: tomfitz
ms.openlocfilehash: 2b46cde4a352e647ee97669f116a6c1926879fa0
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="query-event-grid-subscriptions"></a>Gebeurtenis raster abonnementen query 

Dit artikel wordt beschreven hoe u de gebeurtenis raster abonnementen in uw Azure-abonnement. Tijdens het opvragen van uw bestaande gebeurtenis raster abonnementen, is het belangrijk om te begrijpen van de verschillende typen abonnementen. U opgeven verschillende parameters op basis van het type van het abonnement dat u wilt ophalen.

## <a name="resource-groups-and-azure-subscriptions"></a>Resourcegroepen en Azure-abonnementen

Azure-abonnementen en resourcegroepen zijn niet de Azure-resources. Gebeurtenis raster abonnementen aan resourcegroepen of Azure-abonnementen hoeft niet dus dezelfde eigenschappen als gebeurtenis raster abonnementen op Azure-resources. Gebeurtenis raster abonnementen aan resourcegroepen of Azure-abonnementen worden beschouwd als global.

Als u abonnementen voor het raster van gebeurtenissen voor een Azure-abonnement en de resourcegroepen, hoeft u niet te bieden van parameters. Zorg ervoor dat u hebt geselecteerd dat het Azure-abonnement dat u wilt zoeken. De volgende voorbeelden ophalen niet van gebeurtenis raster abonnementen voor aangepaste onderwerpen of Azure-resources.

Gebruik voor Azure CLI:

```azurecli-interactive
az account set -s "My Azure Subscription"
az eventgrid event-subscription list
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Set-AzureRmContext -Subscription "My Azure Subscription"
Get-AzureRmEventGridSubscription
```

Als u de gebeurtenis raster abonnementen voor een Azure-abonnement, geef het Onderwerptype **Microsoft.Resources.Subscriptions**.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.Subscriptions"
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Resources.Subscriptions"
```

Als u de gebeurtenis raster abonnementen voor alle resourcegroepen binnen een Azure-abonnement, geef het Onderwerptype **Microsoft.Resources.ResourceGroups**.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.ResourceGroups"
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Resources.ResourceGroups"
```

Als u abonnementen voor het raster van gebeurtenissen voor een opgegeven resourcegroep, geef de naam van de resourcegroep als parameter.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --resource-group myResourceGroup
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -ResourceGroupName myResourceGroup
```

## <a name="custom-topics-and-azure-resources"></a>Aangepaste onderwerpen en Azure-resources

Gebeurtenis raster aangepaste onderwerpen zijn Azure-resources. U kunt daarom gebeurtenis raster abonnementen voor aangepaste onderwerpen en andere resources, zoals Blob storage-account opvragen op dezelfde manier. Als u de gebeurtenis raster abonnementen voor aangepaste onderwerpen, moet u de parameters die de bron identificeren of geef de locatie van de resource opgeven. Het is niet mogelijk met breed query gebeurtenis raster abonnementen voor resources in uw Azure-abonnement.

Als u abonnementen raster voor de aangepaste-onderwerpen en andere bronnen in een locatie, geef de naam van de locatie.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --location westus2
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -Location westus2
```

Als u abonnementen op aangepaste onderwerpen voor een locatie, geef de locatie en het Onderwerptype **Microsoft.EventGrid.Topics**.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.EventGrid.Topics" --location "westus2"
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.EventGrid.Topics" -Location westus2
```

Als u abonnementen op opslagaccounts voor een locatie, geef de locatie en het Onderwerptype **Microsoft.Storage.StorageAccounts**.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --topic-type "Microsoft.Storage.StorageAccounts" --location westus2
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Storage.StorageAccounts" -Location westus2
```

Als u abonnementen voor het raster van gebeurtenissen voor een aangepaste onderwerp, geef de naam van de aangepaste onderwerp en de naam van de resourcegroep.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription list --topic-name myCustomTopic --resource-group myResourceGroup
```

Gebruik voor PowerShell:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicName myCustomTopic -ResourceGroupName myResourceGroup
```

Als u de gebeurtenis raster abonnementen voor een bepaalde bron, bieden de resource-ID.

Gebruik voor Azure CLI:

```azurecli-interactive
resourceid=$(az resource show -n mystorage -g myResourceGroup --resource-type "Microsoft.Storage/storageaccounts" --query id --output tsv)
az eventgrid event-subscription list --resource-id $resourceid
```

Gebruik voor PowerShell:

```azurepowershell-interactive
$resourceid = (Get-AzureRmResource -Name mystorage -ResourceGroupName myResourceGroup).ResourceId
Get-AzureRmEventGridSubscription -ResourceId $resourceid
```

## <a name="next-steps"></a>Volgende stappen

* Voor informatie over de levering van de gebeurtenis en nieuwe pogingen, [gebeurtenis raster berichtbezorging en probeer het opnieuw](delivery-and-retry.md).
* Zie voor een inleiding tot gebeurtenis raster, [over gebeurtenis raster](overview.md).
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).

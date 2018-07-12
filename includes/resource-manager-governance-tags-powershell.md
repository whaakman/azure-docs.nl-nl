---
title: bestand opnemen
description: bestand opnemen
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 05/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 5dc4ce00685c74b2974cf1bfb5e8606eb3063e8d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740536"
---
Gebruik de opdracht [Set-AzureRmResourceGroup](/powershell/module/azurerm.resources/set-azurermresourcegroup) om twee tags toe te voegen aan een resourcegroep:

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Stel dat u een derde tag wilt toevoegen. Wanneer u tags op een resource of resourcegroep toepast, overschrijft u de bestaande tags van die resource of resourcegroep. Als u een nieuwe tag wilt toevoegen zonder de bestaande tags kwijt te raken, moet u de bestaande tags ophalen, een nieuwe tag toevoegen en de verzameling tags opnieuw toepassen:

```azurepowershell-interactive
# Get existing tags and add a new tag
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags.Add("Project", "Documentation")

# Reapply the updated set of tags 
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

Resources nemen geen tags over van de resourcegroep. Op dit moment heeft uw resourcegroep drie tags, maar hebben de resources geen tags. Gebruik het volgende script als u alle tags van een resourcegroep op de bijbehorende resources wilt toepassen en de bestaande tags voor de resources die geen dubbele waarden zijn, wilt behouden:

```azurepowershell-interactive
# Get the resource group
$group = Get-AzureRmResourceGroup myResourceGroup

if ($group.Tags -ne $null) {
    # Get the resources in the resource group
    $resources = Get-AzureRmResource -ResourceGroupName $group.ResourceGroupName

    # Loop through each resource
    foreach ($r in $resources)
    {
        # Get the tags for this resource
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        
        # If the resource has existing tags, add new ones
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }

            # Reapply the updated tags to the resource 
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzureRmResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

U kunt ook tags van de resourcegroep toepassen op de resources zonder de bestaande tags te houden:

```azurepowershell-interactive
# Get the resource group
$g = Get-AzureRmResourceGroup -Name myResourceGroup

# Find all the resources in the resource group, and for each resource apply the tags from the resource group
Get-AzureRmResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Gebruik een JSON-tekenreeks om meerdere waarden met elkaar te combineren in een enkele tag.

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Geef een lege hash-tabel door om alle tags te verwijderen.

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```

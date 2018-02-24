---
title: bestand opnemen
description: bestand opnemen
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 21216d19fb8a37d3e9e02e410d39b2a999d3935e
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
U kunt twee tags toevoegen aan een resourcegroep met de [Set-AzureRmResourceGroup](/powershell/module/azurerm.resources/set-azurermresourcegroup) opdracht:

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Stel dat u wilt toevoegen van een derde label. Wanneer u tags op een resource of resourcegroep toepast, overschrijft u de bestaande tags van die resource of resourcegroep. Om een nieuwe code zonder verlies van de bestaande labels toevoegt, moet u de bestaande labels ophalen, een nieuw label toevoegen en opnieuw toepassen van de verzameling van labels:

```azurepowershell-interactive
# Get existing tags and add a new tag
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags += @{Project="Documentation"}

# Reapply the updated set of tags 
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

Resources overnemen niet labels van de resourcegroep. Op dit moment uw resourcegroep heeft drie tags, maar de bronnen hebben geen labels. Als u alle tags toepassen vanuit een resourcegroep tot de bronnen en bestaande tags voor resources die geen dubbele waarden behouden, gebruikt u het volgende script:

```azurepowershell-interactive
# Get the resource group
$group = Get-AzureRmResourceGroup myResourceGroup

if ($group.Tags -ne $null) {
    # Get the resources in the resource group
    $resources = $group | Find-AzureRmResource

    # Loop through each resource
    foreach ($r in $resources)
    {
        # Get the tags for this resource
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags

        # Loop through each tag from the resource group
        foreach ($key in $group.Tags.Keys)
        {
            # Check if the resource already has a tag for the key from the resource group. If so, remove it from the resource
            if (($resourcetags) -AND ($resourcetags.ContainsKey($key))) { $resourcetags.Remove($key) }
        }

        # Add the tags from the resource group to the resource tags
        $resourcetags += $group.Tags

        # Reapply the updated tags to the resource 
        Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
    }
}
```

U kunt ook labels van de resourcegroep toepassen op de bronnen zonder de bestaande labels te houden:

```azurepowershell-interactive
# Get the resource group
$g = Get-AzureRmResourceGroup -Name myResourceGroup

# Find all the resources in the resource group, and for each resource apply the tags from the resource group
Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Gebruiken om te combineren meerdere waarden in een enkel label, een JSON-tekenreeks.

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Als u wilt verwijderen van alle codes, moet u een leeg hash-tabel doorgeven.

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```

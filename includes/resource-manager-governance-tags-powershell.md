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
ms.openlocfilehash: 87b9bd74fc59c86bf177e45c18bfc4e104d9c996
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
Als u twee labels toevoegen aan een resourcegroep, wilt gebruiken:

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Stel dat u wilt toevoegen van een derde label. Wanneer u tags op een resource of resourcegroep toepast, overschrijft u de bestaande tags van die resource of resourcegroep. Om een nieuwe code zonder verlies van de bestaande labels toevoegt, moet u de bestaande labels ophalen, een nieuw label toevoegen en opnieuw toepassen van de verzameling van labels:

```powershell
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags += @{Project="Documentation"}
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

Resources overnemen niet labels van de resourcegroep. Op dit moment uw resourcegroep heeft drie tags, maar de bronnen hebben geen labels. Als u alle tags toepassen vanuit een resourcegroep tot de bronnen en bestaande tags voor resources die geen dubbele waarden behouden, gebruikt u het volgende script:

```powershell
$group = Get-AzureRmResourceGroup myResourceGroup
if ($group.Tags -ne $null) {
    $resources = $group | Find-AzureRmResource
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        foreach ($key in $group.Tags.Keys)
        {
            if (($resourcetags) -AND ($resourcetags.ContainsKey($key))) { $resourcetags.Remove($key) }
        }
        $resourcetags += $group.Tags
        Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
    }
}
```

U kunt ook labels van de resourcegroep toepassen op de bronnen zonder de bestaande labels te houden:

```powershell
$g = Get-AzureRmResourceGroup -Name myResourceGroup
Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Gebruiken om te combineren meerdere waarden in een enkel label, een JSON-tekenreeks.

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Als u wilt verwijderen van alle codes, moet u een leeg hash-tabel doorgeven.

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```

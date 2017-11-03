---
title: Azure-resource beleidsregels voor tags | Microsoft Docs
description: Voorbeelden van bronbeleid biedt voor het beheren van tags voor bronnen
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: tomfitz
ms.openlocfilehash: 469bd8d637337e5900ea84c6bfaf88064695fb7e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="apply-resource-policies-for-tags"></a>Resource-beleid voor tags toepassen

Dit onderwerp bevat algemene beleidsregels dat kunt u consistent gebruik van labels op bronnen toepassen.

Een label-beleid toepassen op een resourcegroep of abonnement met bestaande bronnen geldt met terugwerkende kracht niet het beleid voor deze resources. Om af te dwingen de beleidsregels op deze bronnen, activeert u een update aan de bestaande resources. Dit artikel bevat een voorbeeld van PowerShell voor activering van een update.

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a>Zorg ervoor dat alle bronnen in een resourcegroep hebben een tagwaarde

Een algemene vereiste is dat alle bronnen in een resourcegroep een bepaalde tag en een waarde hebben. Deze vereiste is vaak nodig om kosten te houden per afdeling. De volgende voorwaarden is voldaan:

* Het vereiste label en de waarde worden toegevoegd aan de nieuwe en bijgewerkte bronnen die geen van de tag.
* Het vereiste label en de waarde kunnen niet worden verwijderd uit een bestaande resources.

U kunt deze vereiste uitvoeren door twee ingebouwde beleidsregels toepassen op een resourcegroep.

| Id | Beschrijving |
| ---- | ---- |
| 2a0e14a6-b0a6-4fab-991a-187a4f81c498 | Past een vereiste label en de standaardwaarde als deze niet is opgegeven door de gebruiker. |
| 1e30110a-5ceb-460c-a204-c1c3969c6d62 | Hiermee wordt het een vereiste label en de waarde ervan. |

### <a name="powershell"></a>PowerShell

De volgende PowerShell-script wordt de twee ingebouwde beleidsdefinities toegewezen aan een resourcegroep. Voordat u het script uitvoert, toewijzen alle vereiste tags aan de resourcegroep. Elke tag voor de resourcegroep is vereist voor de bronnen in de groep. Als u wilt toewijzen aan alle resourcegroepen in uw abonnement, bieden niet de `-Name` parameter bij het ophalen van de resourcegroepen.

```powershell
$appendpolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '2a0e14a6-b0a6-4fab-991a-187a4f81c498'}
$denypolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '1e30110a-5ceb-460c-a204-c1c3969c6d62'}

$rgs = Get-AzureRMResourceGroup -Name ExampleGroup

foreach($rg in $rgs)
{
    $tags = $rg.Tags
    foreach($key in $tags.Keys){
        $key 
        $tags[$key]
        New-AzureRmPolicyAssignment -Name ("append"+$key+"tag") -PolicyDefinition $appendpolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
        New-AzureRmPolicyAssignment -Name ("denywithout"+$key+"tag") -PolicyDefinition $denypolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
    }
}
```

Na het toewijzen van het beleid, kunt u een update aan voor alle bestaande resources om af te dwingen de tag-beleidsregels die u hebt toegevoegd activeren. Het volgende script behoudt andere codes die beschikbaar op de bronnen waren:

```powershell
$group = Get-AzureRmResourceGroup -Name "ExampleGroup" 

$resources = Find-AzureRmResource -ResourceGroupName $group.ResourceGroupName 

foreach($r in $resources)
{
    try{
        $r | Set-AzureRmResource -Tags ($a=if($r.Tags -eq $NULL) { @{}} else {$r.Tags}) -Force -UsePatchSemantics
    }
    catch{
        Write-Host  $r.ResourceId + "can't be updated"
    }
}
```

## <a name="require-tags-for-a-resource-type"></a>Codes voor een resourcetype vereisen
Het volgende voorbeeld laat zien hoe nesten logische operators zo in dat een toepassing tag voor alleen een type opgegeven resource (in dit geval storage-accounts).

```json
{
    "if": {
        "allOf": [
          {
            "not": {
              "field": "tags",
              "containsKey": "application"
            }
          },
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          }
        ]
    },
    "then": {
        "effect": "audit"
    }
}
```

## <a name="require-tag"></a>Tag vereisen
Het volgende beleid weigert aanvragen die niet over een label met 'costCenter' sleutel (elke waarde kan worden toegepast):

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

## <a name="next-steps"></a>Volgende stappen
* Na het definiëren van een beleidsregel (zoals weergegeven in de voorgaande voorbeelden), moet u de beleidsdefinitie maken en toewijzen aan een bereik. Het bereik mag een abonnement, resourcegroep of resource. Als u wilt toewijzen beleid via de portal, Zie [gebruik Azure-portal toewijzen en beheren van bronbeleid](resource-manager-policy-portal.md). Als u wilt toewijzen beleid via REST API, PowerShell of Azure CLI, Zie [toewijzen en beheren van beleid via script](resource-manager-policy-create-assign.md).
* Zie voor een inleiding tot bronbeleid, [Resource overzicht](resource-manager-policy.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).


---
title: Toewijzen en beheren van Azure bronbeleid | Microsoft Docs
description: Beschrijft hoe Azure-resource beleid toepast op abonnementen en resourcegroepen en het bronbeleid weergeven.
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
ms.date: 09/19/2017
ms.author: tomfitz
ms.openlocfilehash: 64bdd6ed41e98079c8d4112e895aaeddcd629282
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-and-manage-resource-policies"></a>Toewijzen en beheren van bronbeleid

Als u wilt een beleid implementeren, moet u deze stappen uitvoeren:

1. Controleer de beleidsdefinities (inclusief ingebouwde beleid verstrekt door Azure) om te zien als er al een bestaat in uw abonnement die voldoet aan uw vereisten.
2. Als dit bestaat, krijgen de naam ervan.
3. Als deze niet bestaat, de beleidsregel met JSON definiëren en toevoegen als de beleidsdefinitie van een in uw abonnement. Deze stap maakt het beleid beschikbaar voor toewijzing, maar niet de regels van toepassing op uw abonnement.
4. Voor beide gevallen wijst u het beleid toe aan een bereik (zoals een abonnement of de resource-groep). De regels van het beleid worden nu afgedwongen.

Dit artikel is gericht op de stappen voor het maken van de beleidsdefinitie van een en die definitie toewijzen aan een scope via REST API, PowerShell of Azure CLI. Als u liever de portal gebruiken voor het toewijzen van beleid, Zie [gebruik Azure-portal toewijzen en beheren van bronbeleid](resource-manager-policy-portal.md). In dit artikel is niet gericht op de syntaxis voor het maken van de beleidsdefinitie. Zie voor meer informatie over de syntaxis van het beleid [Resource overzicht](resource-manager-policy.md).

## <a name="exclusion-scopes"></a>Uitsluiting scopes

U kunt een scope uitsluiten bij het toewijzen van een beleid. Deze mogelijkheid vereenvoudigt beleidstoewijzingen omdat u kunt een beleid op abonnementsniveau toewijzen, maar waarin het beleid niet is toegepast. Bijvoorbeeld, in uw abonnement hebt u een resourcegroep die is bedoeld voor de infrastructuur van het netwerk. App-teams implementeren hun resources naar andere resourcegroepen. U wilt niet dat deze teams netwerkbronnen die tot beveiligingsproblemen leiden kunnen te maken. Echter, in de resourcegroep netwerk u wilt toestaan netwerkbronnen. U wijst het beleid op abonnementsniveau toe, maar uitsluiten van de resourcegroep netwerk. U kunt meerdere sub scopes opgeven.

```json
{
    "properties":{
        "policyDefinitionId":"<ID for policy definition>",
        "notScopes":[
            "/subscriptions/<subid>/resourceGroups/networkresourceGroup1"
        ]
    }
}
```

Als u een uitsluitingsbereik voor in de toewijzing opgeeft, gebruikt u de **2017-06-01-preview** API-versie.

## <a name="rest-api"></a>REST API

### <a name="create-policy-definition"></a>Beleidsdefinitie maken

Kunt u een beleid met de [REST-API voor beleidsdefinities](/rest/api/resources/policydefinitions). De REST-API kunt u maken en verwijderen van beleidsdefinities en informatie over bestaande definities.

Voor het maken van een beleidsdefinitie uitvoeren:

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Vergelijkbaar met het volgende voorbeeld wordt een aanvraagtekst omvatten:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### <a name="assign-policy"></a>Toewijzen van beleid

U kunt toepassen op het gewenste bereik via beleidsdefinitie de [REST-API voor beleidstoewijzingen](/rest/api/resources/policyassignments). De REST-API kunt u maken en verwijderen van beleidstoewijzingen en informatie over bestaande toewijzingen.

Voor het maken van een beleidstoewijzing uitvoeren:

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

De {-beleidstoewijzing} is de naam van de beleidstoewijzing.

Vergelijkbaar met het volgende voorbeeld wordt een aanvraagtekst omvatten:

```json
{
  "properties":{
    "displayName":"West US only policy assignment on the subscription ",
    "description":"Resources can only be provisioned in West US regions",
    "parameters": {
      "allowedLocations": { "value": ["northeurope", "westus"] }
     },
    "policyDefinitionId":"/subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}",
      "scope":"/subscriptions/{subscription-id}"
  },
}
```

### <a name="view-policy"></a>Weergave-beleid
Als u een beleid, gebruikt de [ophalen van de beleidsdefinitie](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) bewerking.

### <a name="get-aliases"></a>Aliassen ophalen
U kunt aliassen via de REST-API ophalen:

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

Het volgende voorbeeld ziet een definitie van een alias. Zoals u ziet, definieert een alias paden in verschillende API-versies, zelfs wanneer er een wijziging van de eigenschap. 

```json
"aliases": [
    {
      "name": "Microsoft.Storage/storageAccounts/sku.name",
      "paths": [
        {
          "path": "properties.accountType",
          "apiVersions": [
            "2015-06-15",
            "2015-05-01-preview"
          ]
        },
        {
          "path": "sku.name",
          "apiVersions": [
            "2016-01-01"
          ]
        }
      ]
    }
]
```

## <a name="powershell"></a>PowerShell

Controleer of u hebt voordat u doorgaat met de PowerShell-voorbeelden [de nieuwste versie geïnstalleerd](/powershell/azure/install-azurerm-ps) van Azure PowerShell. Beleidsparameters zijn toegevoegd in versie 3.6.0. Als er een eerdere versie, retourneren in de voorbeelden foutmelding dat de parameter kan niet worden gevonden.

### <a name="view-policy-definitions"></a>Definities van beleid weergeven
Als alle beleidsdefinities in uw abonnement wilt weergeven, gebruikt u de volgende opdracht:

```powershell
Get-AzureRmPolicyDefinition
```

Retourneert alle beschikbare door beleidsdefinities, met inbegrip van ingebouwde beleid. Elk beleid wordt geretourneerd in de volgende indeling:

```powershell
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

Voordat u doorgaat een beleidsdefinitie maken, bekijkt u de ingebouwde beleidsregels. Als u een ingebouwde beleid dat van toepassing is de grenzen die u nodig hebt gevonden, kunt u het maken van een beleidsdefinitie overslaan. In plaats daarvan de ingebouwde beleid toewijzen aan het gewenste bereik.

### <a name="create-policy-definition"></a>Beleidsdefinitie maken
U kunt een beleid maakt definitie met de `New-AzureRmPolicyDefinition` cmdlet.

Als u wilt een beleidsdefinitie maken uit een bestand, geeft u het pad naar het bestand. Gebruik voor een extern bestand:

```powershell
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Gebruik voor een lokaal bestand:

```powershell
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

Als de beleidsdefinitie van een maken met een inline-regel, gebruiken:

```powershell
$definition = New-AzureRmPolicyDefinition -Name denyCoolTiering -Description "Deny cool access tiering for storage" -Policy '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```            

De uitvoer wordt opgeslagen in een `$definition` -object, dat wordt gebruikt tijdens de toewijzing van configuratiebeleid. 

Het volgende voorbeeld wordt de beleidsdefinitie van een met parameters:

```powershell
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}' 

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters 
```

### <a name="assign-policy"></a>Toewijzen van beleid

U het beleid op het gewenste bereik toepassen met behulp van de `New-AzureRmPolicyAssignment` cmdlet. Het volgende voorbeeld wordt het beleid aan een resourcegroep.

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Als u een beleid dat parameters vereist toewijzen, maken en deze waarden van objecten. Het volgende voorbeeld wordt een ingebouwde beleid opgehaald en wordt doorgegeven in parameterwaarden:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a>De toewijzing van beleid weergeven

Als u de toewijzing van een specifiek beleid, gebruikt u:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

Als u wilt de beleidsregel voor de beleidsdefinitie van een weergeven, gebruikt u:

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a>Beleidstoewijzing verwijderen 

Als u wilt een beleidstoewijzing verwijderen, gebruikt u:

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a>Azure CLI

### <a name="view-policy-definitions"></a>Definities van beleid weergeven
Als alle beleidsdefinities in uw abonnement wilt weergeven, gebruikt u de volgende opdracht:

```azurecli
az policy definition list
```

Retourneert alle beschikbare door beleidsdefinities, met inbegrip van ingebouwde beleid. Elk beleid wordt geretourneerd in de volgende indeling:

```azurecli
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

Voordat u doorgaat een beleidsdefinitie maken, bekijkt u de ingebouwde beleidsregels. Als u een ingebouwde beleid dat van toepassing is de grenzen die u nodig hebt gevonden, kunt u het maken van een beleidsdefinitie overslaan. In plaats daarvan de ingebouwde beleid toewijzen aan het gewenste bereik.

### <a name="create-policy-definition"></a>Beleidsdefinitie maken

U kunt een definitie voor Azure CLI gebruiken met de opdracht van de definitie beleid maken.

Als de beleidsdefinitie van een maken met een inline-regel, gebruiken:

```azurecli
az policy definition create --name denyCoolTiering --description "Deny cool access tiering for storage" --rules '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'    
```

### <a name="assign-policy"></a>Toewijzen van beleid

U kunt het beleid toepassen op het gewenste bereik met de opdracht van de toewijzing van beleid. Het volgende voorbeeld wordt een beleid aan een resourcegroep.

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a>De toewijzing van beleid weergeven

Als u wilt weergeven van de beleidstoewijzing van een, bieden de beleidstoewijzingsnaam en het bereik:

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a>Beleidstoewijzing verwijderen 

Als u wilt een beleidstoewijzing verwijderen, gebruikt u:

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a>Volgende stappen
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).


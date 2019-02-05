---
title: Beheren van toegang met RBAC en Azure Resource Manager-sjablonen | Microsoft Docs
description: Informatie over het beheren van toegang voor gebruikers, groepen en toepassingen die gebruikmaken van op rollen gebaseerd toegangsbeheer (RBAC) en Azure Resource Manager-sjablonen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b8c6ac78447a4e4db79ed75100222eee8d528b58
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696894"
---
# <a name="manage-access-using-rbac-and-azure-resource-manager-templates"></a>Toegang met RBAC en Azure Resource Manager-sjablonen beheren

[Op rollen gebaseerde toegangsbeheer (RBAC)](overview.md) is de manier waarop u de toegang tot resources in Azure beheert. Naast het gebruik van Azure PowerShell of Azure CLI kunt u toegang tot Azure-resources met RBAC kunt beheren en [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md). Sjablonen kunnen handig zijn als u nodig hebt voor het implementeren van resources consistent en herhaaldelijk uitmaken. Dit artikel wordt beschreven hoe u toegang met RBAC en sjablonen kunt beheren.

## <a name="example-template-to-create-a-role-assignment"></a>Van de voorbeeldsjabloon voor het maken van een roltoewijzing

In RBAC verleent u toegang door een roltoewijzing te maken. Ziet u de volgende sjabloon:
- Een rol toewijzen aan een gebruiker, groep of toepassing in het groepsbereik van de resource
- Hoe kunt u de rollen eigenaar, Inzender en lezer opgeven als parameter

De sjabloon wilt gebruiken, moet u de volgende invoer opgeven:
- De naam van een resourcegroep
- De unieke id van een gebruiker, groep of toepassing van de rol toewijzen
- De rol toewijzen
- Een unieke id die wordt gebruikt voor de roltoewijzing

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The principal to assign the role to"
      }
    },
    "builtInRoleType": {
      "type": "string",
      "allowedValues": [
        "Owner",
        "Contributor",
        "Reader"
      ],
      "metadata": {
        "description": "Built-in role to assign"
      }
    },
    "roleNameGuid": {
      "type": "string",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    }
  },
  "variables": {
    "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
    "scope": "[resourceGroup().id]"
  },
  "resources": [
    {
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2017-05-01",
      "name": "[parameters('roleNameGuid')]",
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]",
        "scope": "[variables('scope')]"
      }
    }
  ]
}
```

Hieronder ziet u een voorbeeld van een lezer roltoewijzing aan een gebruiker na de implementatie van de sjabloon.

![Roltoewijzing met behulp van een sjabloon](./media/role-assignments-template/role-assignment-template.png)

## <a name="deploy-template-using-azure-powershell"></a>Met behulp van Azure PowerShell-sjabloon implementeren

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Volg deze stappen voor het implementeren van de vorige sjabloon met behulp van Azure PowerShell.

1. Maak een nieuw bestand met de naam rbac-rg.json en kopieer de vorige sjabloon.

1. Meld u aan bij [Azure PowerShell](/powershell/azure/authenticate-azureps).

1. De unieke id van een gebruiker, groep of toepassing ophalen Bijvoorbeeld, kunt u de [Get-AzADUser](/powershell/module/az.resources/get-azaduser) opdracht om een lijst van Azure AD-gebruikers.

    ```azurepowershell
    Get-AzADUser
    ```

1. Een GUID-hulpprogramma gebruiken voor het genereren van een unieke id die wordt gebruikt voor de roltoewijzing. De id heeft de indeling: `11111111-1111-1111-1111-111111111111`

1. Maak een voorbeeld van de resourcegroep.

    ```azurepowershell
    New-AzResourceGroup -Name ExampleGroup -Location "Central US"
    ```

1. Gebruik de [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) opdracht om de implementatie te starten.

    ```azurepowershell
    New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json
    ```

    U wordt gevraagd om op te geven van de vereiste parameters. Hieronder ziet u een voorbeeld van de uitvoer.

    ```Output
    PS /home/user> New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json
    
    cmdlet New-AzResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    principalId: 22222222-2222-2222-2222-222222222222
    builtInRoleType: Reader
    roleNameGuid: 11111111-1111-1111-1111-111111111111
    
    DeploymentName          : rbac-rg
    ResourceGroupName       : ExampleGroup
    ProvisioningState       : Succeeded
    Timestamp               : 7/17/2018 7:46:32 PM
    Mode                    : Incremental
    TemplateLink            :
    Parameters              :
                              Name             Type                       Value
                              ===============  =========================  ==========
                              principalId      String                     22222222-2222-2222-2222-222222222222
                              builtInRoleType  String                     Reader
                              roleNameGuid     String                     11111111-1111-1111-1111-111111111111
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

## <a name="deploy-template-using-the-azure-cli"></a>Met de Azure CLI-sjabloon implementeren

Volg deze stappen voor het implementeren van de vorige sjabloon met de Azure CLI.

1. Maak een nieuw bestand met de naam rbac-rg.json en kopieer de vorige sjabloon.

1. Aanmelden bij de [Azure CLI](/cli/azure/authenticate-azure-cli).

1. De unieke id van een gebruiker, groep of toepassing ophalen Bijvoorbeeld, kunt u de [az ad gebruikerslijst](/cli/azure/ad/user#az-ad-user-list) opdracht om een lijst van Azure AD-gebruikers.

    ```azurecli
    az ad user list
    ```

1. Een GUID-hulpprogramma gebruiken voor het genereren van een unieke id die wordt gebruikt voor de roltoewijzing. De id heeft de indeling: `11111111-1111-1111-1111-111111111111`

1. Maak een voorbeeld van de resourcegroep.

    ```azurecli
    az group create --name ExampleGroup --location "Central US"
    ```

1. Gebruik de [az group deployment maken](/cli/azure/group/deployment#az-group-deployment-create) opdracht om de implementatie te starten.

    ```azurecli
    az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json
    ```

    U wordt gevraagd om op te geven van de vereiste parameters. Hieronder ziet u een voorbeeld van de uitvoer.

    ```Output
    C:\Azure\Templates>az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json
    Please provide string value for 'principalId' (? for help): 22222222-2222-2222-2222-222222222222
    Please provide string value for 'builtInRoleType' (? for help):
     [1] Owner
     [2] Contributor
     [3] Reader
    Please enter a choice [1]: 3
    Please provide string value for 'roleNameGuid' (? for help): 11111111-1111-1111-1111-111111111111
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/rbac-rg",
      "name": "rbac-rg",
      "properties": {
        "additionalProperties": {
          "duration": "PT9.5323924S",
          "outputResources": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
              "resourceGroup": "ExampleGroup"
            }
          ],
          "templateHash": "0000000000000000000"
        },
        "correlationId": "33333333-3333-3333-3333-333333333333",
        "debugSetting": null,
        "dependencies": [],
        "mode": "Incremental",
        "outputs": null,
        "parameters": {
          "builtInRoleType": {
            "type": "String",
            "value": "Reader"
          },
          "principalId": {
            "type": "String",
            "value": "22222222-2222-2222-2222-222222222222"
          },
          "roleNameGuid": {
            "type": "String",
            "value": "11111111-1111-1111-1111-111111111111"
          }
        },
        "parametersLink": null,
        "providers": [
          {
            "id": null,
            "namespace": "Microsoft.Authorization",
            "registrationState": null,
            "resourceTypes": [
              {
                "aliases": null,
                "apiVersions": null,
                "locations": [
                  null
                ],
                "properties": null,
                "resourceType": "roleAssignments"
              }
            ]
          }
        ],
        "provisioningState": "Succeeded",
        "template": null,
        "templateLink": null,
        "timestamp": "2018-07-17T19:00:31.830904+00:00"
      },
      "resourceGroup": "ExampleGroup"
    }
    ```
    
## <a name="next-steps"></a>Volgende stappen

- [Uw eerste Azure Resource Manager-sjabloon maken en implementeren](../azure-resource-manager/resource-manager-create-first-template.md)
- [Informatie over de structuur en de syntaxis van Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md)
- [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=rbac)

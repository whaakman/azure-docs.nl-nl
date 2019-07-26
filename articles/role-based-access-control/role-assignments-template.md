---
title: Toegang tot Azure-resources beheren met RBAC en Azure Resource Manager sjablonen | Microsoft Docs
description: Meer informatie over het beheren van de toegang tot Azure-resources voor gebruikers, groepen en toepassingen met behulp van RBAC (op rollen gebaseerd toegangs beheer) en Azure Resource Manager sjablonen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e6511ff84c251577a5ff483f892387ab7d3d4d41
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68360453"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-resource-manager-templates"></a>Toegang tot Azure-resources beheren met RBAC en Azure Resource Manager sjablonen

[Op rollen gebaseerd toegangsbeheer (RBAC)](overview.md) is de manier waarop u de toegang tot Azure-resources beheert. Naast het gebruik van Azure PowerShell of de Azure CLI kunt u de toegang tot Azure-resources beheren met RBAC en [Azure Resource Manager sjablonen](../azure-resource-manager/resource-group-authoring-templates.md). Sjablonen kunnen nuttig zijn als u resources consistent en herhaaldelijk wilt implementeren. In dit artikel wordt beschreven hoe u toegang kunt beheren met RBAC en sjablonen.

## <a name="assign-role-to-resource-group-or-subscription"></a>Rol toewijzen aan de resource groep of het abonnement

In RBAC verleent u toegang door een roltoewijzing te maken. In de volgende sjabloon ziet u:
- Een rol toewijzen aan een gebruiker, groep of toepassing in de resource groep of het abonnements bereik
- De rol van eigenaar, bijdrager en lezer opgeven als een para meter

Als u de sjabloon wilt gebruiken, moet u de volgende invoer opgeven:
- De unieke id van een gebruiker, groep of toepassing waaraan de rol moet worden toegewezen
- De rol die moet worden toegewezen
- Een unieke id die wordt gebruikt voor de roltoewijzing, of u kunt de standaard-id gebruiken

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
      "defaultValue": "[newGuid()]",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    }
  },
  "variables": {
    "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
  },
  "resources": [
    {
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2018-09-01-preview",
      "name": "[parameters('roleNameGuid')]",
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]"
      }
    }
  ]
}
```

Hieronder ziet u een voor beeld van een toewijzing van een lees functie aan een gebruiker voor een resource groep na het implementeren van de sjabloon.

![Roltoewijzing met behulp van een sjabloon](./media/role-assignments-template/role-assignment-template.png)

Het bereik van de roltoewijzing wordt bepaald op basis van het niveau van de implementatie. In dit artikel worden de implementatie opdrachten voor de resource groep en het abonnements niveau weer gegeven.

## <a name="assign-role-to-resource"></a>Rol toewijzen aan resource

Als u een roltoewijzing op het niveau van een resource wilt maken, is de indeling van de roltoewijzing afwijkend. U geeft de naam ruimte van de resource provider en het resource type van de resource waaraan u de rol wilt toewijzen. U neemt ook de naam van de resource op in de naam van de roltoewijzing.

Voor het type en de naam van de roltoewijzing gebruikt u de volgende indeling:

```json
"type": "{resource-provider-namespace}/{resource-type}/providers/roleAssignments",
"name": "{resource-name}/Microsoft.Authorization/{role-assign-GUID}"
```

Met de volgende sjabloon wordt een opslag account geïmplementeerd en wordt er een rol aan toegewezen. U implementeert deze met de opdrachten van de resource groep.

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
      "defaultValue": "[newGuid()]",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    },
    "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]"
    }
  },
  "variables": {
    "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "apiVersion": "2019-04-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[parameters('location')]",
      "sku": {
          "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    },
    {
      "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
      "apiVersion": "2018-09-01-preview",
      "name": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleNameGuid'))]",
      "dependsOn": [
          "[variables('storageName')]"
      ],
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]"
      }
    }
  ]
}
```

Hieronder ziet u een voor beeld van een rol voor Inzender toewijzing aan een gebruiker voor een opslag account na de implementatie van de sjabloon.

![Roltoewijzing met behulp van een sjabloon](./media/role-assignments-template/role-assignment-template-resource.png)

## <a name="deploy-template-using-azure-powershell"></a>Een sjabloon implementeren met behulp van Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Voer de volgende stappen uit om de vorige sjabloon te implementeren naar een resource groep of een abonnement met behulp van Azure PowerShell.

1. Maak een nieuw bestand met de naam RBAC-RG. json en kopieer de vorige sjabloon.

1. Meld u aan bij [Azure PowerShell](/powershell/azure/authenticate-azureps).

1. De unieke id van een gebruiker, groep of toepassing ophalen. U kunt bijvoorbeeld de opdracht [Get-AzADUser](/powershell/module/az.resources/get-azaduser) gebruiken om Azure AD-gebruikers weer te geven.

    ```azurepowershell
    $userid = (Get-AzADUser -DisplayName "{name}").id
    ```

1. De sjabloon genereert een standaard waarde voor de GUID die wordt gebruikt om de roltoewijzing te identificeren. Als u een specifieke GUID moet opgeven, geeft u die waarde op voor de para meter roleNameGuid. De id heeft de volgende indeling:`11111111-1111-1111-1111-111111111111`

Voer de volgende stappen uit om de rol toe te wijzen op het niveau van een resource of resource groep:

1. Maak een voorbeeld resource groep.

    ```azurepowershell
    New-AzResourceGroup -Name ExampleGroup -Location "Central US"
    ```

1. Gebruik de opdracht [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) om de implementatie te starten.

    ```azurepowershell
    New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
    ```

    Hieronder ziet u een voor beeld van de uitvoer.

    ```Output
    PS /home/user> New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
    
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

Als u de rol wilt toewijzen op het niveau van een abonnement, gebruikt u de opdracht [New-AzDeployment](/powershell/module/az.resources/new-azdeployment) en geeft u een locatie op voor de implementatie.

```azurepowershell
New-AzDeployment -Location centralus -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
```

Het bevat een vergelijk bare uitvoer naar de implementatie opdracht voor resource groepen.

## <a name="deploy-template-using-the-azure-cli"></a>Een sjabloon implementeren met behulp van Azure CLI

Als u de vorige sjabloon wilt implementeren met behulp van de Azure CLI naar een resource groep of een abonnement, voert u de volgende stappen uit.

1. Maak een nieuw bestand met de naam RBAC-RG. json en kopieer de vorige sjabloon.

1. Meld u aan bij de [Azure cli](/cli/azure/authenticate-azure-cli).

1. De unieke id van een gebruiker, groep of toepassing ophalen. U kunt bijvoorbeeld de opdracht [AZ AD User show](/cli/azure/ad/user#az-ad-user-show) gebruiken om een Azure AD-gebruiker weer te geven.

    ```azurecli
    userid=$(az ad user show --upn-or-object-id "{email}" --query objectId --output tsv)
    ```

1. De sjabloon genereert een standaard waarde voor de GUID die wordt gebruikt om de roltoewijzing te identificeren. Als u een specifieke GUID moet opgeven, geeft u die waarde op voor de para meter roleNameGuid. De id heeft de volgende indeling:`11111111-1111-1111-1111-111111111111`

Voer de volgende stappen uit om de rol toe te wijzen op het niveau van een resource of resource groep:

1. Maak een voorbeeld resource groep.

    ```azurecli
    az group create --name ExampleGroup --location "Central US"
    ```

1. Gebruik de opdracht [AZ Group Deployment Create](/cli/azure/group/deployment#az-group-deployment-create) om de implementatie te starten.

    ```azurecli
    az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
    ```

    Hieronder ziet u een voor beeld van de uitvoer.

    ```Output
    C:\Azure\Templates>az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
    
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

Als u de rol wilt toewijzen op het niveau van een abonnement, gebruikt u de opdracht [AZ Deployment Create](/cli/azure/deployment#az-deployment-create) en geeft u een locatie op voor de implementatie.

```azurecli
az deployment create --location centralus --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
```

Het bevat een vergelijk bare uitvoer naar de implementatie opdracht voor resource groepen.

## <a name="next-steps"></a>Volgende stappen

- [Snelstart: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal](../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md)
- [Informatie over de structuur en de syntaxis van Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md)
- [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=rbac)

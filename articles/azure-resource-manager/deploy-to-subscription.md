---
title: 'Resourcegroep en resources op abonnement: Azure Resource Manager-sjabloon maken'
description: Beschrijft hoe u een resourcegroep maken in een Azure Resource Manager-sjabloon. U ziet ook over het implementeren van resources in het bereik van de Azure-abonnement.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: ce95406cbcb6d9514159d84d653a9ff361713aec
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55744395"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>Maken van resourcegroepen en resources op het abonnementsniveau

Normaal gesproken implementeren u Azure-resources in een resourcegroep in uw Azure-abonnement. Echter, u kunt ook Azure-resourcegroepen maken en Azure-resources maken op het abonnementsniveau. Voor het implementeren van sjablonen op abonnementsniveau, gebruikt u Azure CLI en Azure PowerShell. De Azure-portal biedt geen ondersteuning voor implementatie op het abonnementsniveau.

Voor het maken van een resourcegroep in een Azure Resource Manager-sjabloon, definieert een [ **Microsoft.Resources/resourceGroups** ](/azure/templates/microsoft.resources/allversions) resource met een naam en locatie voor de resourcegroep. U kunt een resourcegroep maken en implementeren van resources in die resourcegroep in dezelfde sjabloon. De resources die u op het abonnementsniveau van het implementeren kunt zijn onder andere: [Beleid](../azure-policy/azure-policy-introduction.md), en [Role-based access control van](../role-based-access-control/overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

Niveau abonnementsimplementatie wijkt af van de implementatie van resourcegroep in de volgende aspecten:

### <a name="schema-and-commands"></a>Schema- en -opdrachten

Het schema en de opdrachten die u voor implementaties op abonnementsniveau gebruiken zijn anders dan brongroepimplementaties. 

Gebruik voor het schema `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`.

Gebruik voor de implementatie van Azure CLI-opdracht, [az-implementatie maken](/cli/azure/deployment?view=azure-cli-latest#az-deployment-create). De volgende CLI-opdracht implementeert bijvoorbeeld een sjabloon om een resourcegroep te maken:

```azurecli
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```

Gebruik voor de implementatie van PowerShell-opdracht, [New-AzDeployment](/powershell/module/az.resources/new-azdeployment). De volgende PowerShell-opdracht implementeert bijvoorbeeld een sjabloon om een resourcegroep te maken:

```azurepowershell
New-AzDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

### <a name="deployment-name-and-location"></a>Implementatienaam en locatie

Bij het implementeren van uw abonnement, moet u een locatie voor de implementatie opgeven. U kunt ook een naam voor de implementatie opgeven. Als u een naam voor de implementatie niet opgeeft, wordt de naam van de sjabloon wordt gebruikt als de implementatienaam van de. Bijvoorbeeld, het implementeren van een sjabloon die met de naam **azuredeploy.json** maakt u een standaardnaam voor de implementatie van **azuredeploy**.

De locatie van niveau abonnementimplementaties is onveranderbaar. U kunt een implementatie in één locatie wanneer er een bestaande implementatie met dezelfde naam maar een andere locatie maken. Als u de foutcode `InvalidDeploymentLocation`, gebruik een andere naam of dezelfde locatie als de vorige implementatie voor deze naam.

### <a name="use-template-functions"></a>Sjabloonfuncties gebruiken

Voor implementaties op abonnementsniveau zijn er enkele belangrijke overwegingen bij het gebruik van functies van sjablonen:

* De [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) functie **niet** ondersteund.
* De [resourceId()](resource-group-template-functions-resource.md#resourceid) functie wordt ondersteund. Gebruik dit voor de resource-ID niet ophalen voor resources die worden gebruikt op abonnement niveau implementaties. Bijvoorbeeld: de resource-ID niet ophalen voor een beleidsdefinitie met `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))`
* De [reference()](resource-group-template-functions-resource.md#reference) en [list()](resource-group-template-functions-resource.md#list) functies worden ondersteund.

## <a name="create-resource-groups"></a>Maken van resourcegroepen

De volgende sjabloon maakt een lege resourcegroep.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Het sjabloonschema kan worden gevonden op [hier](/azure/templates/microsoft.resources/allversions). Vergelijkbare sjablonen kunnen u vinden op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments).

## <a name="create-multiple-resource-groups"></a>Maken van meerdere resourcegroepen

Gebruik de [kopie element](resource-group-create-multiple.md) met resourcegroepen om meer dan één resourcegroep te maken. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgNamePrefix": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "instanceCount": {
            "type": "int"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
            "copy": {
                "name": "rgCopy",
                "count": "[parameters('instanceCount')]"
            },
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Zie voor meer informatie over resource iteratie [implementeren van meer dan één instantie van een resource of eigenschap in Azure Resource Manager-sjablonen](./resource-group-create-multiple.md), en [zelfstudie: Meerdere exemplaren van resources maken met Resource Manager-sjablonen](./resource-manager-tutorial-create-multiple-instances.md).

## <a name="create-resource-group-and-deploy-resources"></a>Resourcegroep maken en implementeren van resources

Voor het maken van de resourcegroep en resources te implementeren, moet u een geneste sjabloon gebruiken. De geneste sjabloon definieert de resources te implementeren in de resourcegroep. Stel de geneste sjabloon als afhankelijk van de resourcegroep om te controleren of dat de resourcegroep bestaat voor het implementeren van de resources.

Het volgende voorbeeld wordt een resourcegroep en implementeert een opslagaccount in de resourcegroep.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        }
    },
    "variables": {
        "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "storageDeployment",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
            ],
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "apiVersion": "2017-10-01",
                            "name": "[variables('storageName')]",
                            "location": "[parameters('rgLocation')]",
                            "kind": "StorageV2",
                            "sku": {
                                "name": "Standard_LRS"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

## <a name="create-policies"></a>Beleidsregels maken

### <a name="assign-policy"></a>Beleid toewijzen

Het volgende voorbeeld wordt een beleidsdefinitie van een bestaand toegewezen aan het abonnement. Als het beleid parameters nodig heeft, kunt u deze als een object verstrekken. Als het beleid niet parameters, gebruikt u de standaard leeg object.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "policyDefinitionID": {
            "type": "string"
        },
        "policyName": {
            "type": "string"
        },
        "policyParameters": {
            "type": "object",
            "defaultValue": {}
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "[parameters('policyName')]",
            "apiVersion": "2018-03-01",
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[parameters('policyDefinitionID')]",
                "parameters": "[parameters('policyParameters')]"
            }
        }
    ]
}
```

Als u wilt een ingebouwde beleid toepassen op uw Azure-abonnement, moet u de volgende Azure CLI-opdrachten gebruiken:

```azurecli
# Built-in policy that does not accept parameters
definition=$(az policy definition list --query "[?displayName=='Audit resource location matches resource group location'].id" --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=auditRGLocation
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit resource location matches resource group location' }

New-AzDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName auditRGLocation
```

Als u wilt een ingebouwde beleid toepassen op uw Azure-abonnement, moet u de volgende Azure CLI-opdrachten gebruiken:

```azurecli
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="define-and-assign-policy"></a>Definieer en toewijzen van beleid

U kunt [definiëren](../azure-policy/policy-definition.md) en toewijzen van een beleid in dezelfde sjabloon.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-05-01",
            "properties": {
                "policyType": "Custom",
                "parameters": {},
                "policyRule": {
                    "if": {
                        "field": "location",
                        "equals": "northeurope"
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2018-05-01",
            "dependsOn": [
                "locationpolicy"
            ],
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
            }
        }
    ]
}
```

Het maken van de beleidsdefinitie in uw abonnement, en pas deze toe aan het abonnement, gebruikt u de volgende CLI-opdracht:

```azurecli
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell
New-AzDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="create-roles"></a>Rollen maken

### <a name="assign-role-at-subscription"></a>Rol op abonnement toewijzen

Het volgende voorbeeld wordt een rol toegewezen aan een gebruiker of groep voor het abonnement. In dit voorbeeld opgeven niet u een bereik voor de toewijzing omdat het bereik is automatisch ingesteld op het abonnement.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string"
        },
        "roleDefinitionId": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "name": "[guid(parameters('principalId'), deployment().name)]",
            "apiVersion": "2017-09-01",
            "properties": {
                "roleDefinitionId": "[resourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

Als u wilt een Active Directory-groep toewijzen aan een rol voor uw abonnement, moet u de volgende Azure CLI-opdrachten gebruiken:

```azurecli
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json \
  --parameters principalId=$principalid roleDefinitionId=$role
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell
$role = Get-AzRoleDefinition -Name Contributor

$adgroup = Get-AzADGroup -DisplayName demogroup

New-AzDeployment `
  -Name demoRole `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id
```

### <a name="assign-role-at-scope"></a>Rol in het bereik toe te wijzen

De volgende abonnementsniveau sjabloon wijst een rol toe aan een gebruiker of groep die is afgestemd op een resourcegroep binnen het abonnement. Het bereik moet op of onder het niveau van de implementatie. U kunt implementeren voor een abonnement en geef een roltoewijzing binnen het bereik van een resourcegroep in dat abonnement. U kunt geen echter implementeren in een resourcegroep en geef een bereik van de roltoewijzing aan het abonnement.

Als u wilt de rol op een bereik toewijst, kunt u geen geneste implementatie gebruiken. U ziet dat de naam van de resourcegroep is opgegeven in de eigenschappen voor de bron van de implementatie en in de eigenschap voor bereik van de roltoewijzing.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "principalId": {
            "type": "string"
        },
        "roleDefinitionId": {
            "type": "string"
        },
        "rgName": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "assignRole",
            "resourceGroup": "[parameters('rgName')]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Authorization/roleAssignments",
                            "name": "[guid(parameters('principalId'), deployment().name)]",
                            "apiVersion": "2017-09-01",
                            "properties": {
                                "roleDefinitionId": "[resourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                                "principalId": "[parameters('principalId')]",
                                "scope": "[concat(subscription().id, '/resourceGroups/', parameters('rgName'))]"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

Als u wilt een Active Directory-groep toewijzen aan een rol voor uw abonnement, moet u de volgende Azure CLI-opdrachten gebruiken:

```azurecli
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scopedRoleAssign.json \
  --parameters principalId=$principalid roleDefinitionId=$role rgName demoRg
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell
$role = Get-AzRoleDefinition -Name Contributor

$adgroup = Get-AzADGroup -DisplayName demogroup

New-AzDeployment `
  -Name demoRole `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scopedRoleAssign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id `
  -rgName demoRg
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor een voorbeeld van de implementatie van de werkruimte-instellingen voor Azure Security Center, [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Zie voor meer informatie over het maken van Azure Resource Manager-sjablonen, [-sjablonen maken](resource-group-authoring-templates.md). 
* Zie voor een lijst van de beschikbare functies in een sjabloon, [sjabloonfuncties](resource-group-template-functions.md).

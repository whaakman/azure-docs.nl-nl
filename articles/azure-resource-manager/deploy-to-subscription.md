---
title: Resources implementeren op Azure-abonnement | Microsoft Docs
description: Beschrijft het maken van een Azure Resource Manager-sjabloon die de resources op het abonnementsbereik implementeert.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: 1d281ebe80c6089c559cfaa77f4875a856566092
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079375"
---
# <a name="deploy-resources-to-an-azure-subscription"></a>Resources implementeren op een Azure-abonnement

Normaal gesproken implementeren u resources in een resourcegroep in uw Azure-abonnement. Sommige resources kunnen echter worden geïmplementeerd op het niveau van uw Azure-abonnement. Deze resources toepassen voor uw abonnement. [Beleid](../azure-policy/azure-policy-introduction.md), [rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md), en [Azure Security Center](../security-center/security-center-intro.md) zijn services die u mogelijk wilt toepassen op het abonnementsniveau, in plaats van het niveau van de resource.

Dit artikel wordt gebruikgemaakt van Azure CLI en PowerShell om de sjablonen te implementeren.

## <a name="name-and-location-for-deployment"></a>Naam en locatie voor de implementatie

Bij het implementeren van uw abonnement, moet u een locatie voor de implementatie opgeven. U kunt ook een naam voor de implementatie opgeven. Als u een naam voor de implementatie niet opgeeft, wordt de naam van de sjabloon wordt gebruikt als de implementatienaam van de. Bijvoorbeeld, het implementeren van een sjabloon die met de naam **azuredeploy.json** maakt u een standaardnaam voor de implementatie van **azuredeploy**.

De locatie van niveau abonnementimplementaties is onveranderbaar. U kunt een implementatie in één locatie wanneer er een bestaande implementatie met dezelfde naam maar een andere locatie maken. Als u de foutcode `InvalidDeploymentLocation`, gebruik een andere naam of dezelfde locatie als de vorige implementatie voor deze naam.

## <a name="using-template-functions"></a>Met behulp van functies van sjablonen

Voor abonnement niveau implementaties zijn er enkele belangrijke overwegingen bij het gebruik van functies van sjablonen:

* De [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) functie **niet** ondersteund.
* De [resourceId()](resource-group-template-functions-resource.md#resourceid) functie wordt ondersteund. Gebruik dit voor de resource-ID niet ophalen voor resources die worden gebruikt op abonnement niveau implementaties. Bijvoorbeeld: de resource-ID niet ophalen voor een beleidsdefinitie met `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))`
* De [reference()](resource-group-template-functions-resource.md#reference) en [list()](resource-group-template-functions-resource.md#list) functies worden ondersteund.

## <a name="assign-policy"></a>Beleid toewijzen

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

Als u wilt een ingebouwde beleid toepassen op uw Azure-abonnement, moet u de volgende Azure CLI-opdrachten gebruiken. In dit voorbeeld hebben niet het beleid voor parameters.

```azurecli-interactive
# Built-in policy that does not accept parameters
definition=$(az policy definition list --query "[?displayName=='Audit resource location matches resource group location'].id" --output tsv)

az deployment create \
  -n policyassign \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=auditRGLocation
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit resource location matches resource group location' }

New-AzureRmDeployment `
  -Name policyassign `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName auditRGLocation
```

Als u wilt een ingebouwde beleid toepassen op uw Azure-abonnement, moet u de volgende Azure CLI-opdrachten gebruiken. In dit voorbeeld heeft het beleid voor parameters.

```azurecli-interactive
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  -n policyassign \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzureRmDeployment `
  -Name policyassign `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

## <a name="define-and-assign-policy"></a>Definieer en toewijzen van beleid

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

Het maken van de beleidsdefinitie in uw abonnement, en pas deze toe aan het abonnement, gebruikt u de volgende CLI-opdracht.

```azurecli-interactive
az deployment create \
  -n definePolicy \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name definePolicy `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="assign-role"></a>Rol toewijzen

Het volgende voorbeeld wordt een rol toegewezen aan een gebruiker of groep.

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

Als u wilt een Active Directory-groep toewijzen aan een rol voor uw abonnement, moet u de volgende Azure CLI-opdrachten gebruiken.

```azurecli-interactive
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  -n demoRole \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json \
  --parameters principalId=$principalid roleDefinitionId=$role
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell-interactive
$role = Get-AzureRmRoleDefinition -Name Contributor

$adgroup = Get-AzureRmADGroup -DisplayName demogroup

New-AzureRmDeployment `
  -Name demoRole `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een voorbeeld van de implementatie van de werkruimte-instellingen voor Azure Security Center, [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Zie voor het maken van een resourcegroep, [resourcegroepen in Azure Resource Manager-sjablonen maken](create-resource-group-in-template.md).
* Zie voor meer informatie over het maken van Azure Resource Manager-sjablonen, [-sjablonen maken](resource-group-authoring-templates.md). 
* Zie voor een lijst van de beschikbare functies in een sjabloon, [sjabloonfuncties](resource-group-template-functions.md).


---
title: Azure Resource Manager-sjabloonfuncties - implementatie | Microsoft Docs
description: Beschrijft de functies in een Azure Resource Manager-sjabloon gebruikt voor het ophalen van informatie over de implementatie.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2018
ms.author: tomfitz
ms.openlocfilehash: d802af1d48405518f26f4b52ecc3023cbb15caff
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407351"
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a>Van implementatiefuncties voor Azure Resource Manager-sjablonen 

Resource Manager biedt de volgende functies voor het ophalen van waarden uit de secties van de sjabloon en de waarden die betrekking hebben op de implementatie:

* [Implementatie](#deployment)
* [parameters](#parameters)
* [Variabelen](#variables)

Zie voor waarden van resources, resourcegroepen of abonnementen, [resourcefuncties](resource-group-template-functions-resource.md).

<a id="deployment" />

## <a name="deployment"></a>implementatie
`deployment()`

Retourneert informatie over de huidige implementatiebewerking.

### <a name="return-value"></a>Retourwaarde

Deze functie geeft als resultaat van het object dat wordt doorgegeven tijdens de implementatie. De eigenschappen in het geretourneerde object verschillen afhankelijk van of het implementatieobject wordt doorgegeven als een koppeling of als een inline-object. Wanneer het implementatieobject wordt doorgegeven in lijn, zoals bij het gebruik van de **- sjabloonbestand** parameter in Azure PowerShell om te verwijzen naar een lokaal bestand, het geretourneerde object heeft de volgende indeling:

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Wanneer het object wordt doorgegeven als een koppeling, zoals bij het gebruik van de **- TemplateUri** parameter om te verwijzen naar een externe object het object wordt geretourneerd in de volgende indeling: 

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Wanneer u [implementeren op een Azure-abonnement](deploy-to-subscription.md), in plaats van een resourcegroep, het geretourneerde object bevat een `location` eigenschap. De locatie-eigenschap is inbegrepen bij het implementeren van een lokale sjabloon of een externe-sjabloon.

### <a name="remarks"></a>Opmerkingen

U kunt deployment() gebruiken om te koppelen aan een andere sjabloon op basis van de URI van de bovenliggende sjabloon.

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deployment.json) retourneert het implementatieobject:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

Het vorige voorbeeld retourneert de volgende object:

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "subscriptionOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/deployment.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/deployment.json
```

Zie voor een abonnement sjabloon die gebruikmaakt van de implementatie-functie, [abonnement implementatie functie](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deploymentsubscription.json). Deze geïmplementeerd in beide gevallen `az deployment create` of `New-AzureRmDeployment` opdrachten.

<a id="parameters" />

## <a name="parameters"></a>parameters
`parameters(parameterName)`

Retourneert een parameterwaarde. De opgegeven parameternaam moet worden gedefinieerd in de parametersectie van de sjabloon.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| parameterName |Ja |string |De naam van de parameter om terug te keren. |

### <a name="return-value"></a>Retourwaarde

De waarde van de opgegeven parameter.

### <a name="remarks"></a>Opmerkingen

Meestal gebruikt u parameters kunt u resource-waarden instellen. Het volgende voorbeeld wordt de naam van website aan de waarde van parameter doorgegeven tijdens de implementatie.

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/parameters.json) ziet u een vereenvoudigde gebruik van de parameters-functie.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Waarde |
| ---- | ---- | ----- |
| stringOutput | Reeks | Optie 1 |
| intOutput | Int | 1 |
| objectOutput | Object | {"een": "a", "2": "b"} |
| arrayOutput | Matrix | [1, 2, 3] |
| crossOutput | Reeks | Optie 1 |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/parameters.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/parameters.json
```

<a id="variables" />

## <a name="variables"></a>Variabelen
`variables(variableName)`

Retourneert de waarde van variabele. De naam van de opgegeven variabele moet worden gedefinieerd in de sectie met variabelen van de sjabloon.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| variableName |Ja |Reeks |De naam van de variabele om terug te keren. |

### <a name="return-value"></a>Retourwaarde

De waarde van de opgegeven variabele.

### <a name="remarks"></a>Opmerkingen

Normaal gesproken gebruikt u variabelen voor de sjabloon vereenvoudigen door slechts één keer complexe waarden samen te stellen. Het volgende voorbeeld wordt een unieke naam voor een opslagaccount.

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/variables.json) retourneert de waarden van verschillende variabelen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Waarde |
| ---- | ---- | ----- |
| exampleOutput1 | Reeks | myVariable |
| exampleOutput2 | Matrix | [1, 2, 3, 4] |
| exampleOutput3 | Reeks | myVariable |
| exampleOutput4 |  Object | {"Eigenschap1": "value1", "eigenschap 2": "value2"} |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/variables.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/variables.json
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon, [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* U kunt verschillende sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Op een opgegeven aantal keren herhalen bij het maken van een type resource, Zie [meerdere exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor meer informatie over het implementeren van de sjabloon die u hebt gemaakt, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).


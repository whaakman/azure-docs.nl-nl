---
title: Azure Resource Manager-sjabloonfuncties - implementatie | Microsoft Docs
description: Beschrijft de functies te gebruiken in een Azure Resource Manager-sjabloon voor het ophalen van informatie over de implementatie.
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
ms.date: 09/05/2017
ms.author: tomfitz
ms.openlocfilehash: 17fe2bc467acc5542d021961a066940dbecf6120
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a>Implementatie-functies voor Azure Resource Manager-sjablonen 

Resource Manager biedt de volgende functies voor het ophalen van waarden van de secties van de sjabloon en de waarden die betrekking hebben op de implementatie:

* [implementatie](#deployment)
* [parameters](#parameters)
* [variabelen](#variables)

Als u de waarden van resources, resourcegroepen of abonnementen, Zie [Resource functies](resource-group-template-functions-resource.md).

<a id="deployment" />

## <a name="deployment"></a>implementatie
`deployment()`

Retourneert informatie over de huidige implementatiebewerking.

### <a name="return-value"></a>Retourwaarde

Deze functie retourneert het object dat wordt doorgegeven tijdens de implementatie. De eigenschappen in het geretourneerde object verschillen op basis van of het implementatieobject wordt doorgegeven als een koppeling of als een in-line-object. Wanneer het implementatieobject wordt doorgegeven in de regel, zoals wanneer u de **- sjabloonbestand** parameter in Azure PowerShell om te verwijzen naar een lokaal bestand, het geretourneerde object heeft de volgende notatie:

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

Wanneer het object is doorgegeven als een koppeling, zoals wanneer u de **- TemplateUri** parameter om te verwijzen naar een extern object het object wordt geretourneerd in de volgende indeling: 

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

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/deployment.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/deployment.json
```

<a id="parameters" />

## <a name="parameters"></a>parameters
`parameters(parameterName)`

Retourneert een parameterwaarde. De opgegeven parameternaam moet worden gedefinieerd in het gedeelte parameters van de sjabloon.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| parameterName |Ja |Tekenreeks |De naam van de parameter om terug te keren. |

### <a name="return-value"></a>Retourwaarde

De waarde van de opgegeven parameter.

### <a name="remarks"></a>Opmerkingen

Doorgaans kunt u parameters resource waarden instellen. Het volgende voorbeeld wordt de naam van de website met de parameterwaarde doorgegeven tijdens de implementatie.

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

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| stringOutput | Tekenreeks | Optie 1 |
| intOutput | int | 1 |
| objectOutput | Object | {"een": "a", "twee": "b"} |
| arrayOutput | Matrix | [1, 2, 3] |
| crossOutput | Tekenreeks | Optie 1 |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/parameters.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/parameters.json
```

<a id="variables" />

## <a name="variables"></a>variabelen
`variables(variableName)`

Retourneert de waarde van variabele. De opgegeven naam van de variabele moet worden gedefinieerd in het gedeelte variabelen van de sjabloon.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Variabelenaam |Ja |Tekenreeks |De naam van de variabele retourneren. |

### <a name="return-value"></a>Retourwaarde

De waarde van de opgegeven variabele.

### <a name="remarks"></a>Opmerkingen

Doorgaans kunt u variabelen uw sjabloon vereenvoudigen door slechts één keer complexe waarden samen te stellen. Het volgende voorbeeld wordt een unieke naam voor een opslagaccount.

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

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/variables.json) resulteert in verschillende variabele waarden.

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

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| exampleOutput1 | Tekenreeks | myVariable |
| exampleOutput2 | Matrix | [1, 2, 3, 4] |
| exampleOutput3 | Tekenreeks | myVariable |
| exampleOutput4 |  Object | {{'1': 'value1', 'eigenschap 2': 'waarde2'} |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/variables.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/variables.json
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md).
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Voor een opgegeven aantal keer herhalen bij het maken van een type resource, Zie [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor het implementeren van de sjabloon die u hebt gemaakt, [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).


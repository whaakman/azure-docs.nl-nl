---
title: Azure Resource Manager-sjabloonfuncties - logische | Microsoft Docs
description: Beschrijft de functies in een Azure Resource Manager-sjabloon gebruiken om te bepalen van de logische waarden.
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
ms.openlocfilehash: d16264abf64ef88dfb24948fc04e33de619f4e3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Logische functies voor Azure Resource Manager-sjablonen

Resource Manager biedt een aantal functies voor het maken van vergelijkingen in uw sjablonen.

* [en](#and)
* [BOOL](#bool)
* [Als](#if)
* [niet](#not)
* [of](#or)

## <a name="and"></a>en
`and(arg1, arg2)`

Controleert of beide parameterwaarden true zijn.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |Booleaanse waarde |De eerste waarde om te controleren of is ingesteld op true. |
| Arg2 |Ja |Booleaanse waarde |De tweede waarde om te controleren of is ingesteld op true. |

### <a name="return-value"></a>Retourwaarde

Retourneert **True** als beide waarden true, anders wordt zijn **False**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) laat zien hoe u logische functies.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

De uitvoer van het vorige voorbeeld is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| andExampleOutput | BOOL | False |
| orExampleOutput | BOOL | True |
| notExampleOutput | BOOL | False |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

## <a name="bool"></a>BOOL
`bool(arg1)`

De parameter converteert naar een Booleaanse waarde.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |String of int. |De waarde te converteren naar een Booleaanse waarde. |

### <a name="return-value"></a>Retourwaarde
Een Booleaanse waarde van de geconverteerde waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/bool.json) bool gebruiken met een tekenreeks of een geheel getal bevat.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| trueString | BOOL | True |
| falseString | BOOL | False |
| trueInt | BOOL | True |
| falseInt | BOOL | False |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/bool.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/bool.json
```

## <a name="if"></a>Als
`if(condition, trueValue, falseValue)`

Retourneert een waarde op basis van of u een voorwaarde is true of false.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Voorwaarde |Ja |Booleaanse waarde |De waarde om te controleren of deze voorwaarden voldaan wordt. |
| trueValue |Ja | String, int, object of matrix |De waarde wilt retourneren wanneer de voorwaarde waar is. |
| falseValue |Ja | String, int, object of matrix |De waarde wilt retourneren wanneer de voorwaarde onwaar is. |

### <a name="return-value"></a>Retourwaarde

Retourneert een tweede parameter als eerste parameter is **True**; anders retourneert de derde parameter.

### <a name="remarks"></a>Opmerkingen

Deze functie kunt u een resource-eigenschap voorwaardelijk instellen. Het volgende voorbeeld is niet een volledige sjabloon, maar de relevante delen voor het instellen van de beschikbaarheidsset voorwaardelijk te zien.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "availabilitySet": {
            "type": "string",
            "allowedValues": [
                "yes",
                "no"
            ]
        }
    },
    "variables": {
        ...
        "availabilitySetName": "availabilitySet1",
        "availabilitySet": {
            "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
        }
    },
    "resources": [
        {
            "condition": "[equals(parameters('availabilitySet'),'yes')]",
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            ...
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Compute/virtualMachines",
            "properties": {
                "availabilitySet": "[if(equals(parameters('availabilitySet'),'yes'), variables('availabilitySet'), json('null'))]",
                ...
            }
        },
        ...
    ],
    ...
}
```

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/if.json) laat zien hoe u de `if` functie.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| yesOutput | Tekenreeks | ja |
| noOutput | Tekenreeks | nee |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/if.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/if.json
```

## <a name="not"></a>niet
`not(arg1)`

Booleaanse waarde omgezet in de tegengestelde waarde.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |Booleaanse waarde |De waarde te converteren. |

### <a name="return-value"></a>Retourwaarde

Retourneert **True** wanneer de parameter is **False**. Retourneert **False** wanneer de parameter is **True**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) laat zien hoe u logische functies.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

De uitvoer van het vorige voorbeeld is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| andExampleOutput | BOOL | False |
| orExampleOutput | BOOL | True |
| notExampleOutput | BOOL | False |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) gebruikt **niet** met [gelijk is aan](resource-group-template-functions-comparison.md#equals).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
```

De uitvoer van het vorige voorbeeld is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| checkNotEquals | BOOL | True |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/not-equals.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/not-equals.json
```

## <a name="or"></a>of
`or(arg1, arg2)`

Controleert of de waarde voor de parameter ingesteld op true is.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |Booleaanse waarde |De eerste waarde om te controleren of is ingesteld op true. |
| Arg2 |Ja |Booleaanse waarde |De tweede waarde om te controleren of is ingesteld op true. |

### <a name="return-value"></a>Retourwaarde

Retourneert **True** als de waarde true, anders wordt is **False**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) laat zien hoe u logische functies.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

De uitvoer van het vorige voorbeeld is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| andExampleOutput | BOOL | False |
| orExampleOutput | BOOL | True |
| notExampleOutput | BOOL | False |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md).
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Voor een opgegeven aantal keer herhalen bij het maken van een type resource, Zie [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor het implementeren van de sjabloon die u hebt gemaakt, [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).


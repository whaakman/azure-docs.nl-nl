---
title: Azure Resource Manager-sjabloonfuncties - logische | Microsoft Docs
description: Beschrijft de functies in een Azure Resource Manager-sjabloon gebruiken om te bepalen van de logische waarden.
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
ms.date: 09/24/2018
ms.author: tomfitz
ms.openlocfilehash: 8850ef68c665efcf9e66315af20b7d1e8492fc5f
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55493759"
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Logische functies voor Azure Resource Manager-sjablonen

Resource Manager biedt verschillende functies voor het maken van vergelijkingen in uw sjablonen.

* [en](#and)
* [bool](#bool)
* [if](#if)
* [niet](#not)
* [or](#or)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="and"></a>en
`and(arg1, arg2)`

Controleert of de waarden van beide parameters waar zijn.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |booleaans |De eerste waarde om te controleren of de waarde true is. |
| arg2 |Ja |booleaans |De tweede waarde om te controleren of de waarde true is. |

### <a name="return-value"></a>Retourwaarde

Retourneert **waar** als beide waarden ' True ', anders wordt zijn **False**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) ziet u hoe u logische functies.

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

De uitvoer uit het vorige voorbeeld is:

| Name | Type | Value |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

## <a name="bool"></a>bool
`bool(arg1)`

De parameter converteert naar een Booleaanse waarde.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |tekenreeks of int |De waarde te converteren naar een Booleaanse waarde. |

### <a name="return-value"></a>Retourwaarde
Een Booleaanse waarde van de geconverteerde waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/bool.json) ziet u hoe u bool met een tekenreeks of een geheel getal zijn.

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

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| trueString | Bool | True |
| falseString | Bool | False |
| trueInt | Bool | True |
| falseInt | Bool | False |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/bool.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/bool.json
```

## <a name="if"></a>if
`if(condition, trueValue, falseValue)`

Retourneert een waarde op basis van of u een voorwaarde is true of false.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| voorwaarde |Ja |booleaans |De waarde om te controleren of dit het geval is. |
| trueValue |Ja | String, int, object of matrix |De waarde moet worden geretourneerd wanneer de voorwaarde waar is. |
| falseValue |Ja | String, int, object of matrix |De waarde moet worden geretourneerd wanneer de voorwaarde onwaar is. |

### <a name="return-value"></a>Retourwaarde

Retourneert een tweede parameter als eerste parameter is **waar**; anders retourneert de derde parameter.

### <a name="remarks"></a>Opmerkingen

Deze functie kunt u een resource-eigenschap voorwaardelijk instellen. Het volgende voorbeeld is niet een volledige sjabloon, maar hier ziet u de relevante onderdelen voor het instellen van voorwaardelijk de beschikbaarheidsset.

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

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/if.json) ziet u hoe u de `if` functie.

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
        },
        "objectOutput": {
            "type": "object",
            "value": "[if(equals('a', 'a'), json('{\"test\": \"value1\"}'), json('null'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld is:

| Name | Type | Value |
| ---- | ---- | ----- |
| yesOutput | String | ja |
| noOutput | String | nee |
| objectOutput | Object | { "test": "value1" } |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/if.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/if.json
```

## <a name="not"></a>niet
`not(arg1)`

Booleaanse waarde omgezet in de tegengestelde waarde.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |booleaans |De waarde te converteren. |

### <a name="return-value"></a>Retourwaarde

Retourneert **waar** als parameter is **False**. Retourneert **False** als parameter is **waar**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) ziet u hoe u logische functies.

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

De uitvoer uit het vorige voorbeeld is:

| Name | Type | Value |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) maakt gebruik van **niet** met [gelijk is aan](resource-group-template-functions-comparison.md#equals).

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

De uitvoer uit het vorige voorbeeld is:

| Name | Type | Value |
| ---- | ---- | ----- |
| checkNotEquals | Bool | True |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/not-equals.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/not-equals.json
```

## <a name="or"></a>of
`or(arg1, arg2)`

Controleert of de waarde voor de parameter ' True '.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |booleaans |De eerste waarde om te controleren of de waarde true is. |
| arg2 |Ja |booleaans |De tweede waarde om te controleren of de waarde true is. |

### <a name="return-value"></a>Retourwaarde

Retourneert **waar** als beide waarden waar zijn; anders heeft **False**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) ziet u hoe u logische functies.

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

De uitvoer uit het vorige voorbeeld is:

| Name | Type | Value |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon, [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Op een opgegeven aantal keren herhalen bij het maken van een type resource, Zie [meerdere exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor meer informatie over het implementeren van de sjabloon die u hebt gemaakt, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).


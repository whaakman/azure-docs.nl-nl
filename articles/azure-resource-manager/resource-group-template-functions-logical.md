---
title: Azure Resource Manager-sjabloonfuncties - logische | Microsoft Docs
description: Beschrijft de functies in een Azure Resource Manager-sjabloon gebruiken om te bepalen van de logische waarden.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 04/15/2019
ms.author: tomfitz
ms.openlocfilehash: cf6874512557bc44efe978708c78e3d98db4110b
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205465"
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Logische functies voor Azure Resource Manager-sjablonen

Resource Manager biedt verschillende functies voor het maken van vergelijkingen in uw sjablonen.

* [en](#and)
* [bool](#bool)
* [if](#if)
* [niet](#not)
* [of](#or)

## <a name="and"></a>en

`and(arg1, arg2, ...)`

Controleert of alle parameterwaarden ' True zijn '.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |boolean |De eerste waarde om te controleren of de waarde true is. |
| arg2 |Ja |boolean |De tweede waarde om te controleren of de waarde true is. |
| aanvullende argumenten |Nee |boolean |Extra argumenten om te controleren of wordt voldaan. |

### <a name="return-value"></a>Retourwaarde

Retourneert **waar** als alle waarden ' True ', anders wordt zijn **False**.

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

## <a name="if"></a>if

`if(condition, trueValue, falseValue)`

Retourneert een waarde op basis van of u een voorwaarde is true of false.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| condition |Ja |boolean |De waarde moet worden gecontroleerd of dit waar of ONWAAR. |
| trueValue |Ja | String, int, object of matrix |De waarde moet worden geretourneerd wanneer de voorwaarde waar is. |
| falseValue |Ja | String, int, object of matrix |De waarde moet worden geretourneerd wanneer de voorwaarde onwaar is. |

### <a name="return-value"></a>Retourwaarde

Retourneert een tweede parameter als eerste parameter is **waar**; anders retourneert de derde parameter.

### <a name="remarks"></a>Opmerkingen

Wanneer de voorwaarde is **waar**, alleen de waarde waar wordt geëvalueerd. Wanneer de voorwaarde is **False**, alleen de waarde false wordt geëvalueerd. Met de **als** functie, kunt u expressies die alleen voorwaardelijk geldig zijn opgenomen. U kunt bijvoorbeeld verwijzen naar een resource die onder één voorwaarde hebben, maar niet in de andere voorwaarde bestaat. Een voorbeeld van het evalueren van voorwaardelijk expressies wordt weergegeven in de volgende sectie.

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

De volgende [voorbeeldsjabloon](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/conditionWithReference.json) laat zien hoe u deze functie wilt gebruiken met expressies die alleen voorwaardelijk geldig zijn.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "logAnalytics": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "condition": "[not(empty(parameters('logAnalytics')))]",
            "name": "[concat(parameters('vmName'),'/omsOnboarding')]",
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2017-03-30",
            "properties": {
                "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                "type": "MicrosoftMonitoringAgent",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "workspaceId": "[if(not(empty(parameters('logAnalytics'))), reference(parameters('logAnalytics'), '2015-11-01-preview').customerId, json('null'))]"
                },
                "protectedSettings": {
                    "workspaceKey": "[if(not(empty(parameters('logAnalytics'))), listKeys(parameters('logAnalytics'), '2015-11-01-preview').primarySharedKey, json('null'))]"
                }
            }
        }
    ],
    "outputs": {
        "mgmtStatus": {
            "type": "string",
            "value": "[if(not(empty(parameters('logAnalytics'))), 'Enabled monitoring for VM!', 'Nothing to enable')]"
        }
    }
}
```

## <a name="not"></a>not

`not(arg1)`

Booleaanse waarde omgezet in de tegengestelde waarde.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |boolean |De waarde te converteren. |

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

## <a name="or"></a>of

`or(arg1, arg2, ...)`

Controleert of een waarde voor parameter ' True '.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |boolean |De eerste waarde om te controleren of de waarde true is. |
| arg2 |Ja |boolean |De tweede waarde om te controleren of de waarde true is. |
| aanvullende argumenten |Nee |boolean |Extra argumenten om te controleren of wordt voldaan. |

### <a name="return-value"></a>Retourwaarde

Retourneert **waar** als een willekeurige waarde waar zijn; anders heeft **False**.

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

## <a name="next-steps"></a>Volgende stappen

* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon, [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Op een opgegeven aantal keren herhalen bij het maken van een type resource, Zie [meerdere exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor meer informatie over het implementeren van de sjabloon die u hebt gemaakt, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).


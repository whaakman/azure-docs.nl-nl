---
title: Azure Resource Manager-sjabloon fungeert - matrices en objecten | Microsoft Docs
description: Beschrijft de functies te gebruiken in een Azure Resource Manager-sjabloon voor het werken met matrices en objecten.
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
ms.openlocfilehash: 7d040fe55cb46665c97668a76ccbc66adc002f89
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a>Matrix en object-functies voor Azure Resource Manager-sjablonen 

Resource Manager biedt een aantal functies voor het werken met matrices en objecten.

* [matrix](#array)
* [coalesce](#coalesce)
* [concat](#concat)
* [bevat](#contains)
* [createArray](#createarray)
* [leeg](#empty)
* [eerste](#first)
* [snijpunt](#intersection)
* [JSON](#json)
* [laatste](#last)
* [lengte](#length)
* [maximale](#max)
* [min](#min)
* [bereik](#range)
* [overslaan](#skip)
* [duren](#take)
* [Union](#union)

Als u een matrix van tekenreekswaarden gescheiden door een waarde, Zie [splitsen](resource-group-template-functions-string.md#split).

<a id="array" />

## <a name="array"></a>matrix
`array(convertToArray)`

De waarde wordt omgezet in een matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| convertToArray |Ja |int, string, matrix of object |De waarde te converteren naar een matrix. |

### <a name="return-value"></a>Retourwaarde

Een matrix.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/array.json) laat zien hoe u de functie array met verschillende typen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "intToConvert": {
            "type": "int",
            "defaultValue": 1
        },
        "stringToConvert": {
            "type": "string",
            "defaultValue": "a"
        },
        "objectToConvert": {
            "type": "object",
            "defaultValue": {"a": "b", "c": "d"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "intOutput": {
            "type": "array",
            "value": "[array(parameters('intToConvert'))]"
        },
        "stringOutput": {
            "type": "array",
            "value": "[array(parameters('stringToConvert'))]"
        },
        "objectOutput": {
            "type": "array",
            "value": "[array(parameters('objectToConvert'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| intOutput | Matrix | [1] |
| stringOutput | Matrix | ["a"] |
| objectOutput | Matrix | [{"a": "b", "c": "d"}] |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/array.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/array.json
```

<a id="coalesce" />

## <a name="coalesce"></a>Coalesce
`coalesce(arg1, arg2, arg3, ...)`

Retourneert de eerste niet-null-waarde van de parameters. Lege tekenreeksen, matrices leeg en lege objecten zijn niet null.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |int, string, matrix of object |De eerste waarde om te controleren op null. |
| Als u meer argumenten |Nee |int, string, matrix of object |Aanvullende waarden testen op null. |

### <a name="return-value"></a>Retourwaarde

De waarde van de eerste niet-null-parameters, dit kan een string, int, matrix of object. Null als alle parameters leeg zijn. 

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/coalesce.json) ziet u de uitvoer van verschillende coalesce worden gebruikt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {
                "null1": null, 
                "null2": null,
                "string": "default",
                "int": 1,
                "object": {"first": "default"},
                "array": [1]
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').string)]"
        },
        "intOutput": {
            "type": "int",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').int)]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').object)]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').array)]"
        },
        "emptyOutput": {
            "type": "bool",
            "value": "[empty(coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| stringOutput | Tekenreeks | Standaard |
| intOutput | int | 1 |
| objectOutput | Object | {{'eerste': 'standaard'} |
| arrayOutput | Matrix | [1] |
| emptyOutput | BOOL | True |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/coalesce.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/coalesce.json
```

<a id="concat" />

## <a name="concat"></a>concat
`concat(arg1, arg2, arg3, ...)`

Combineert meerdere matrices en retourneert de aaneengeschakelde matrix of combineert meerdere tekenreekswaarden en retourneert de samengevoegde tekenreeks. 

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix of tekenreeks |De eerste matrix of tekenreeks op voor de samenvoeging. |
| Extra argumenten |Nee |matrix of tekenreeks |Aanvullende matrices of tekenreeksen in opeenvolgende volgorde voor de samenvoeging. |

Deze functie kan duren voordat een willekeurig aantal argumenten en tekenreeksen of matrices voor de parameters kan accepteren.

### <a name="return-value"></a>Retourwaarde
Een tekenreeks of matrix met samengevoegde waarden.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-array.json) ziet u twee matrices combineren.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| retourneren | Matrix | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-array.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-array.json
```

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-string.json) ziet u het combineren van twee tekenreekswaarden zoals en een samengevoegde tekenreeks retourneren.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), '-', uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| concatOutput | Tekenreeks | voorvoegsel 5yj4yjf5mbg72 |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-string.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-string.json
```

<a id="contains" />

## <a name="contains"></a>Bevat
`contains(container, itemToFind)`

Controleert of een matrix een waarde bevat, een object een sleutel bevat of een tekenreeks een subtekenreeks bevat.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| container |Ja |Array, object of een tekenreeks |De waarde met de waarde om te zoeken. |
| itemToFind |Ja |String of int. |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

**De waarde True** als het item gevonden, anders wordt is **False**.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/contains.json) toont hoe u bevat met verschillende typen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| stringTrue | BOOL | True |
| stringFalse | BOOL | False |
| objectTrue | BOOL | True |
| objectFalse | BOOL | False |
| arrayTrue | BOOL | True |
| arrayFalse | BOOL | False |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/contains.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/contains.json
```

<a id="createarray" />

## <a name="createarray"></a>createarray
`createArray (arg1, arg2, arg3, ...)`

Maakt een matrix van de parameters.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |Tekenreeks, geheel getal, matrix of Object |De eerste waarde in de matrix. |
| Extra argumenten |Nee |Tekenreeks, geheel getal, matrix of Object |Aanvullende waarden in de matrix. |

### <a name="return-value"></a>Retourwaarde

Een matrix.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/createarray.json) laat zien hoe u createArray met verschillende typen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringArray": {
            "type": "array",
            "value": "[createArray('a', 'b', 'c')]"
        },
        "intArray": {
            "type": "array",
            "value": "[createArray(1, 2, 3)]"
        },
        "objectArray": {
            "type": "array",
            "value": "[createArray(parameters('objectToTest'))]"
        },
        "arrayArray": {
            "type": "array",
            "value": "[createArray(parameters('arrayToTest'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| stringArray | Matrix | ["a", "b", "c"] |
| intArray | Matrix | [1, 2, 3] |
| objectArray | Matrix | [{"een": "a", "twee": "b", "drie": "c"}] |
| arrayArray | Matrix | [[' een', 'twee', 'drie']] |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/createarray.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/createarray.json
```

<a id="empty" />

## <a name="empty"></a>leeg

`empty(itemToTest)`

Hiermee wordt bepaald of een matrix, een object of een tekenreeks leeg is.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| itemToTest |Ja |Array, object of een tekenreeks |De waarde om te controleren als deze leeg is. |

### <a name="return-value"></a>Retourwaarde

Retourneert **True** als de waarde leeg, anders wordt is **False**.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/empty.json) controleert of een matrix, het object en de tekenreeks leeg zijn.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| arrayEmpty | BOOL | True |
| objectEmpty | BOOL | True |
| stringEmpty | BOOL | True |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/empty.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/empty.json
```

<a id="first" />

## <a name="first"></a>eerste
`first(arg1)`

Retourneert het eerste element van de matrix of het eerste teken van de tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix of tekenreeks |De waarde voor het ophalen van het eerste element of teken. |

### <a name="return-value"></a>Retourwaarde

Het type (string, int, matrix of object) van het eerste element in een matrix of het eerste teken van een tekenreeks.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/first.json) laat zien hoe u de eerste functie met een matrix en de tekenreeks.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| arrayOutput | Tekenreeks | een |
| stringOutput | Tekenreeks | O |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/first.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/first.json
```

<a id="intersection" />

## <a name="intersection"></a>snijpunt
`intersection(arg1, arg2, arg3, ...)`

Retourneert een matrix of object met de algemene elementen van de parameters.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix of object |De eerste waarde voor het zoeken naar algemene elementen. |
| Arg2 |Ja |matrix of object |De tweede waarde voor het zoeken naar algemene elementen. |
| Extra argumenten |Nee |matrix of object |Aanvullende waarden gebruiken voor het zoeken naar algemene elementen. |

### <a name="return-value"></a>Retourwaarde

Een matrix of object met de algemene elementen.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/intersection.json) laat zien hoe u snijpunt met arrays en objecten:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "z", "three": "c"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[intersection(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[intersection(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| objectOutput | Object | {"een": "a", "3": "c"} |
| arrayOutput | Matrix | ["twee", "drie"] |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/intersection.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/intersection.json
```

## <a name="json"></a>JSON
`json(arg1)`

Retourneert een JSON-object.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |Tekenreeks |De waarde niet converteren naar JSON. |


### <a name="return-value"></a>Retourwaarde

De JSON-object van de opgegeven tekenreeks of een leeg object wanneer **null** is opgegeven.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/json.json) laat zien hoe de json-functie gebruiken met matrices en objecten:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "jsonOutput": {
            "type": "object",
            "value": "[json('{\"a\": \"b\"}')]"
        },
        "nullOutput": {
            "type": "bool",
            "value": "[empty(json('null'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| jsonOutput | Object | {"a": "b"} |
| nullOutput | Booleaanse waarde | True |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/json.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/json.json
```

<a id="last" />

## <a name="last"></a>laatste
`last (arg1)`

Retourneert het laatste element van de matrix of laatste teken van de tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix of tekenreeks |De waarde voor het ophalen van het laatste element of teken. |

### <a name="return-value"></a>Retourwaarde

Het type (string, int, matrix of object) van het laatste element in een matrix of het laatste teken van een tekenreeks.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/last.json) laat zien hoe u de laatste functie met een matrix en de tekenreeks.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| arrayOutput | Tekenreeks | drie |
| stringOutput | Tekenreeks | E |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/last.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/last.json
```

<a id="length" />

## <a name="length"></a>lengte
`length(arg1)`

Retourneert het aantal elementen in een matrix of de tekens in een tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix of tekenreeks |De matrix te gebruiken voor het ophalen van het aantal elementen of de tekenreeks moet worden gebruikt voor het ophalen van het aantal tekens. |

### <a name="return-value"></a>Retourwaarde

Een int. 

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/length.json) lengte gebruiken met een matrix en de tekenreeks bevat:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| arrayLength | int | 3 |
| stringLength | int | 13 |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/length.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/length.json
```

U kunt deze functie gebruiken met een matrix te geven voor het aantal iteraties maken van resources. In het volgende voorbeeld wordt de parameter **siteNames** verwijzen naar een matrix van namen te gebruiken bij het maken van websites.

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

Zie voor meer informatie over het gebruik van deze functie met een matrix [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md).

<a id="max" />

## <a name="max"></a>Maximum aantal
`max(arg1)`

Retourneert de maximumwaarde van een matrix van gehele getallen of een door komma's gescheiden lijst met gehele getallen zijn.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix van gehele getallen of door komma's gescheiden lijst met gehele getallen |De verzameling die de maximale waarde op te halen. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal dat de maximale waarde vertegenwoordigt.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json) laat zien hoe u max met een matrix en een lijst met gehele getallen zijn:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| arrayOutput | int | 5 |
| intOutput | int | 5 |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

<a id="min" />

## <a name="min"></a>min.
`min(arg1)`

Retourneert de minimumwaarde van een matrix van gehele getallen of een door komma's gescheiden lijst met gehele getallen zijn.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix van gehele getallen of door komma's gescheiden lijst met gehele getallen |De verzameling voor de minimumwaarde. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal dat de minimale waarde vertegenwoordigt.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json) laat zien hoe min. gebruik met een matrix en een lijst met gehele getallen zijn:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| arrayOutput | int | 0 |
| intOutput | int | 0 |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

<a id="range" />

## <a name="range"></a>bereik
`range(startingInteger, numberOfElements)`

Maakt een matrix van gehele getallen van een geheel getal beginnen en met een aantal items.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| startingInteger |Ja |int |De eerste geheel getal in de matrix. |
| numberofElements |Ja |int |Het aantal getallen in de matrix. |

### <a name="return-value"></a>Retourwaarde

Een matrix van gehele getallen.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/range.json) laat zien hoe de bereikfunctie gebruiken:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "startingInt": {
            "type": "int",
            "defaultValue": 5
        },
        "numberOfElements": {
            "type": "int",
            "defaultValue": 3
        }
    },
    "resources": [],
    "outputs": {
        "rangeOutput": {
            "type": "array",
            "value": "[range(parameters('startingInt'),parameters('numberOfElements'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| rangeOutput | Matrix | [5, 6, 7] |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/range.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/range.json
```

<a id="skip" />

## <a name="skip"></a>Overslaan
`skip(originalValue, numberToSkip)`

Retourneert een matrix met alle elementen na het opgegeven getal in de matrix of retourneert een tekenreeks met de tekens na het opgegeven getal in de tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |matrix of tekenreeks |De matrix of tekenreeks moet worden gebruikt voor het overslaan. |
| numberToSkip |Ja |int |Het aantal elementen of tekens over te slaan. Als deze waarde 0 of minder is, worden alle elementen of tekens in de waarde geretourneerd. Als deze groter dan de lengte van de matrix of tekenreeks is, een lege matrix of tekenreeks geretourneerd. |

### <a name="return-value"></a>Retourwaarde

Een matrix of tekenreeks zijn.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/skip.json) slaat het opgegeven aantal elementen in de matrix en het opgegeven aantal tekens in een tekenreeks.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| arrayOutput | Matrix | ["drie"] |
| stringOutput | Tekenreeks | twee drie |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/skip.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/skip.json
```

<a id="take" />

## <a name="take"></a>duren
`take(originalValue, numberToTake)`

Retourneert een matrix met het opgegeven aantal elementen vanaf het begin van de matrix of een tekenreeks zijn met het opgegeven aantal tekens vanaf het begin van de tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |matrix of tekenreeks |De matrix of tekenreeks op voor het nemen van elementen uit. |
| numberToTake |Ja |int |Het aantal elementen of tekens te laten worden. Als deze waarde 0 of minder is, wordt een lege matrix of tekenreeks geretourneerd. Als deze groter is dan de lengte van de opgegeven matrix of tekenreeks is, worden alle elementen in de matrix of tekenreeks geretourneerd. |

### <a name="return-value"></a>Retourwaarde

Een matrix of tekenreeks zijn.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/take.json) duurt het opgegeven aantal elementen vanaf de matrix en tekens uit een tekenreeks.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| arrayOutput | Matrix | ['een', 'twee'] |
| stringOutput | Tekenreeks | op |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/take.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/take.json
```

<a id="union" />

## <a name="union"></a>Union
`union(arg1, arg2, arg3, ...)`

Retourneert een matrix of object met alle elementen van de parameters. Dubbele waarden of sleutels zijn slechts één keer wordt opgenomen.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix of object |De eerste waarde moet worden gebruikt om lid te worden elementen. |
| Arg2 |Ja |matrix of object |De tweede waarde moet worden gebruikt om lid te worden elementen. |
| Extra argumenten |Nee |matrix of object |Aanvullende waarden worden gebruikt om lid te worden elementen. |

### <a name="return-value"></a>Retourwaarde

Een matrix of object.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/union.json) laat zien hoe u union met arrays en objecten:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c1"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"three": "c2", "four": "d", "five": "e"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["three", "four"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[union(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[union(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| objectOutput | Object | {"een": "a", "2": "b", 'drie': 'c2', "vier": 'd","vijf': 'e'} |
| arrayOutput | Matrix | ['een', 'twee', 'drie', "vier"] |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/union.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/union.json
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md).
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Voor een opgegeven aantal keer herhalen bij het maken van een type resource, Zie [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor het implementeren van de sjabloon die u hebt gemaakt, [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).


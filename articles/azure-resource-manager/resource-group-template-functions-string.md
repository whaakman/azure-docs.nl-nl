---
title: Azure Resource Manager-sjabloonfuncties - string | Microsoft Docs
description: Beschrijft de functies in een Azure Resource Manager-sjabloon gebruiken om te werken met tekenreeksen.
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
ms.openlocfilehash: eeb3e46d9b8a5822b1aea3cc62bb214f3c3fec43
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="string-functions-for-azure-resource-manager-templates"></a>Tekenreeks-functies voor Azure Resource Manager-sjablonen

Resource Manager biedt de volgende functies voor het werken met tekenreeksen:

* [Base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [concat](#concat)
* [bevat](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [leeg](#empty)
* [endsWith](#endswith)
* [eerste](#first)
* [GUID](#guid)
* [indexOf](#indexof)
* [laatste](#last)
* [lastIndexOf](#lastindexof)
* [lengte](#length)
* [padLeft](#padleft)
* [vervangen](#replace)
* [overslaan](#skip)
* [split](#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [tekenreeks](#string)
* [de subtekenreeks](#substring)
* [duren](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [Trim](#trim)
* [uniqueString](#uniquestring)
* [URI](#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a>Base64
`base64(inputString)`

Retourneert de base64-weergave van de invoertekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| inputString |Ja |Tekenreeks |De waarde moet worden geretourneerd als een base64-weergave. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met de base64-weergave.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) laat zien hoe u de functie base64.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| base64Output | Tekenreeks | b25lLCB0d28sIHRocmVl |
| toStringOutput | Tekenreeks | Een twee drie |
| toJsonOutput | Object | {"een": "a", "twee": "b"} |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/base64.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/base64.json
```

<a id="base64tojson" />

## <a name="base64tojson"></a>base64ToJson
`base64tojson`

Converteert een base64-weergave naar een JSON-object.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| base64Value |Ja |Tekenreeks |De weergave base64 converteren naar een JSON-object. |

### <a name="return-value"></a>Retourwaarde

Een JSON-object.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) wordt van de functie base64ToJson gebruikt om een base64-waarde te converteren:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| base64Output | Tekenreeks | b25lLCB0d28sIHRocmVl |
| toStringOutput | Tekenreeks | Een twee drie |
| toJsonOutput | Object | {"een": "a", "twee": "b"} |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/base64.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/base64.json
```

<a id="base64tostring" />

## <a name="base64tostring"></a>base64ToString
`base64ToString(base64Value)`

Converteert een base64-weergave naar een tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| base64Value |Ja |Tekenreeks |De weergave base64 converteren naar een tekenreeks. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks van de geconverteerde base64-waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) wordt van de functie base64ToString gebruikt om een base64-waarde te converteren:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| base64Output | Tekenreeks | b25lLCB0d28sIHRocmVl |
| toStringOutput | Tekenreeks | Een twee drie |
| toJsonOutput | Object | {"een": "a", "twee": "b"} |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/base64.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/base64.json
```

<a id="concat" />

## <a name="concat"></a>concat
`concat (arg1, arg2, arg3, ...)`

Combineert meerdere tekenreekswaarden en retourneert de samengevoegde tekenreeks of combineert meerdere matrices en retourneert de aaneengeschakelde matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |tekenreeks of matrix |De eerste waarde voor de samenvoeging. |
| Extra argumenten |Nee |Tekenreeks |Aanvullende waarden in opeenvolgende volgorde voor de samenvoeging. |

### <a name="return-value"></a>Retourwaarde
Een tekenreeks of matrix met samengevoegde waarden.

### <a name="examples"></a>Voorbeelden

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

<a id="contains" />

## <a name="contains"></a>Bevat
`contains (container, itemToFind)`

Controleert of een matrix een waarde bevat, een object een sleutel bevat of een tekenreeks een subtekenreeks bevat.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| container |Ja |Array, object of een tekenreeks |De waarde met de waarde om te zoeken. |
| itemToFind |Ja |String of int. |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

**De waarde True** als het item gevonden, anders wordt is **False**.

### <a name="examples"></a>Voorbeelden

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

<a id="datauri" />

## <a name="datauri"></a>dataUri
`dataUri(stringToConvert)`

Converteert een waarde met een gegevens-URI.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| stringToConvert |Ja |Tekenreeks |De waarde converteren naar een gegevens-URI. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks zijn opgemaakt als een gegevens-URI.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/datauri.json) converteert een waarde naar een gegevens-URI en een gegevens-URI converteert naar een tekenreeks:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| dataUriOutput | Tekenreeks | gegevens: tekst / onbewerkte; charset = utf8; base64, SGVsbG8 = |
| toStringOutput | Tekenreeks | Hallo mensen! |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/datauri.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/datauri.json
```

<a id="datauritostring" />

## <a name="datauritostring"></a>dataUriToString
`dataUriToString(dataUriToConvert)`

Een gegevens-URI converteert geformatteerd waarde naar een tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |Ja |Tekenreeks |De gegevens-URI-waarde te converteren. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met de geconverteerde waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/datauri.json) converteert een waarde naar een gegevens-URI en een gegevens-URI converteert naar een tekenreeks:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| dataUriOutput | Tekenreeks | gegevens: tekst / onbewerkte; charset = utf8; base64, SGVsbG8 = |
| toStringOutput | Tekenreeks | Hallo mensen! |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/datauri.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/datauri.json
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

### <a name="examples"></a>Voorbeelden

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

<a id="endswith" />

## <a name="endswith"></a>endsWith
`endsWith(stringToSearch, stringToFind)`

Hiermee wordt bepaald of een tekenreeks met een waarde eindigt. De vergelijking is niet hoofdlettergevoelig.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |Tekenreeks |De waarde die het item vinden bevat. |
| stringToFind |Ja |Tekenreeks |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

**De waarde True** als het laatste teken of van de tekenreeks overeen met de waarde; anders **False**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/startsendswith.json) laat zien hoe de functies startsWith en endsWith gebruiken:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| startsTrue | BOOL | True |
| startsCapTrue | BOOL | True |
| startsFalse | BOOL | False |
| endsTrue | BOOL | True |
| endsCapTrue | BOOL | True |
| endsFalse | BOOL | False |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/startsendswith.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/startsendswith.json
```

<a id="first" />

## <a name="first"></a>eerste
`first(arg1)`

Retourneert het eerste teken van de tekenreeks of het eerste element van de matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix of tekenreeks |De waarde voor het ophalen van het eerste element of teken. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks van het eerste teken of het type (string, int, matrix of object) van het eerste element in een matrix.

### <a name="examples"></a>Voorbeelden

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

## <a name="guid"></a>GUID

`guid (baseString, ...)`

Maakt een waarde in de indeling van een globally unique identifier op basis van de waarden als parameters.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| baseString |Ja |Tekenreeks |De waarde die wordt gebruikt in de hash-functie voor het maken van de GUID. |
| extra parameters indien nodig |Nee |Tekenreeks |U kunt toevoegen die nodig zijn voor het maken van de waarde die het niveau van de uniekheid van tekenreeksen. |

### <a name="remarks"></a>Opmerkingen

Deze functie is handig als u moet een waarde in de indeling van een globally unique identifier maken. U opgeven parameterwaarden die het uniciteitsbereik voor het resultaat beperken. U kunt opgeven of de naam uniek naar beneden op abonnement, resourcegroep of implementatie is.

De geretourneerde waarde is niet een willekeurige tekenreeks op, maar in plaats daarvan het resultaat van een hash-functie. De geretourneerde waarde is 36 tekens lang. Het is niet uniek.

De volgende voorbeelden laten zien hoe guid gebruiken voor het maken van een unieke waarde voor vaak gebruikte niveaus.

Uniek binnen het bereik van abonnement

```json
"[guid(subscription().subscriptionId)]"
```

Uniek binnen het bereik van resourcegroep

```json
"[guid(resourceGroup().id)]"
```

Uniek binnen het bereik van implementatie voor een resourcegroep

```json
"[guid(resourceGroup().id, deployment().name)]"
```

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met de indeling van een globally unique identifier 36 tekens.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/guid.json) resultaten retourneert uit guid:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [],
    "outputs": {
        "guidPerSubscription": {
            "value": "[guid(subscription().subscriptionId)]",
            "type": "string"
        },
        "guidPerResourceGroup": {
            "value": "[guid(resourceGroup().id)]",
            "type": "string"
        },
        "guidPerDeployment": {
            "value": "[guid(resourceGroup().id, deployment().name)]",
            "type": "string"
        }
    }
}
```

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/guid.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/guid.json
```

<a id="indexof" />

## <a name="indexof"></a>indexOf
`indexOf(stringToSearch, stringToFind)`

Retourneert de eerste positie van een waarde in een tekenreeks. De vergelijking is niet hoofdlettergevoelig.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |Tekenreeks |De waarde die het item vinden bevat. |
| stringToFind |Ja |Tekenreeks |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal dat de positie van het item te vinden. De waarde is nul. Als het item niet gevonden is, wordt -1 geretourneerd.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/indexof.json) laat zien hoe de functies indexOf en lastIndexOf gebruiken:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| firstT | int | 0 |
| lastT | int | 3 |
| firstString | int | 2 |
| lastString | int | 0 |
| notFound | int | -1 |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/indexof.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/indexof.json
```

<a id="last" />

## <a name="last"></a>laatste
`last (arg1)`

Retourneert de laatste teken van de tekenreeks of het laatste element van de matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix of tekenreeks |De waarde voor het ophalen van het laatste element of teken. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks van het laatste teken of het type (string, int, matrix of object) van het laatste element in een matrix.

### <a name="examples"></a>Voorbeelden

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

<a id="lastindexof" />

## <a name="lastindexof"></a>lastIndexOf
`lastIndexOf(stringToSearch, stringToFind)`

Retourneert de laatste positie van een waarde in een tekenreeks. De vergelijking is niet hoofdlettergevoelig.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |Tekenreeks |De waarde die het item vinden bevat. |
| stringToFind |Ja |Tekenreeks |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal dat staat voor de laatste positie van het item te vinden. De waarde is nul. Als het item niet gevonden is, wordt -1 geretourneerd.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/indexof.json) laat zien hoe de functies indexOf en lastIndexOf gebruiken:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| firstT | int | 0 |
| lastT | int | 3 |
| firstString | int | 2 |
| lastString | int | 0 |
| notFound | int | -1 |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/indexof.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/indexof.json
```

<a id="length" />

## <a name="length"></a>lengte
`length(string)`

Retourneert het aantal tekens in een tekenreeks of elementen in een matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| Arg1 |Ja |matrix of tekenreeks |De matrix te gebruiken voor het ophalen van het aantal elementen of de tekenreeks moet worden gebruikt voor het ophalen van het aantal tekens. |

### <a name="return-value"></a>Retourwaarde

Een int. 

### <a name="examples"></a>Voorbeelden

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

<a id="padleft" />

## <a name="padleft"></a>PadLeft
`padLeft(valueToPad, totalLength, paddingCharacter)`

Retourneert een rechts uitgelijnde tekenreeks met tekens toe te voegen aan de linkerkant tot het bereiken van de totale lengte van de opgegeven.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| valueToPad |Ja |String of int. |De waarde rechts uitlijnen. |
| totalLength |Ja |int |Het totale aantal tekens in de tekenreeks geretourneerd. |
| paddingCharacter |Nee |willekeurig teken |Het teken dat moet worden gebruikt voor links-opvulling tot de totale lengte is bereikt. De standaardwaarde is een spatie. |

Als de oorspronkelijke reeks langer dan het aantal tekens is worden opgevuld, worden geen tekens toegevoegd.

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met ten minste het aantal tekens opgegeven.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/padleft.json) laat zien hoe de gebruiker opgegeven parameterwaarde opgevuld door het teken nul toe te voegen totdat het totale aantal tekens is bereikt. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),10,'0')]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| stringOutput | Tekenreeks | 0000000123 |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/padleft.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/padleft.json
```

<a id="replace" />

## <a name="replace"></a>vervangen
`replace(originalString, oldString, newString)`

Retourneert een nieuwe tekenreeks met alle exemplaren van de ene tekenreeks vervangen door een andere tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| originalString |Ja |Tekenreeks |De waarde die is van alle exemplaren van de ene tekenreeks vervangen door een andere tekenreeks. |
| oldString |Ja |Tekenreeks |De tekenreeks die moet worden verwijderd uit de oorspronkelijke reeks. |
| met de newString |Ja |Tekenreeks |De tekenreeks om toe te voegen in plaats van de verwijderde tekenreeks. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met de vervangende tekens.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/replace.json) toont alle streepjes verwijderen van de gebruiker opgegeven tekenreeks en deel uitmaken van de tekenreeks vervangen door een andere tekenreeks.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secodeOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| firstOutput | Tekenreeks | 1231231234 |
| secodeOutput | Tekenreeks | 123-123-xxxx |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/replace.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/replace.json
```

<a id="skip" />

## <a name="skip"></a>Overslaan
`skip(originalValue, numberToSkip)`

Retourneert een tekenreeks met de tekens na het opgegeven aantal tekens of een matrix met alle elementen na het opgegeven aantal elementen.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |matrix of tekenreeks |De matrix of tekenreeks moet worden gebruikt voor het overslaan. |
| numberToSkip |Ja |int |Het aantal elementen of tekens over te slaan. Als deze waarde 0 of minder is, worden alle elementen of tekens in de waarde geretourneerd. Als deze groter dan de lengte van de matrix of tekenreeks is, een lege matrix of tekenreeks geretourneerd. |

### <a name="return-value"></a>Retourwaarde

Een matrix of tekenreeks zijn.

### <a name="examples"></a>Voorbeelden

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

<a id="split" />

## <a name="split"></a>split
`split(inputString, delimiter)`

Retourneert een matrix met tekenreeksen die de subtekenreeksen de invoerreeks die worden gescheiden door de opgegeven scheidingstekens bevat.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| inputString |Ja |Tekenreeks |De tekenreeks om op te splitsen. |
| Scheidingsteken |Ja |tekenreeks of matrix met tekenreeksen |Het scheidingsteken moet worden gebruikt voor het splitsen van de tekenreeks. |

### <a name="return-value"></a>Retourwaarde

Een matrix met tekenreeksen.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/split.json) splitst de invoerreeks met een komma en met een komma of puntkomma's.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| firstOutput | Matrix | ['een', 'twee', 'drie'] |
| secondOutput | Matrix | ['een', 'twee', 'drie'] |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/split.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/split.json
```

<a id="startswith" />

## <a name="startswith"></a>StartsWith
`startsWith(stringToSearch, stringToFind)`

Hiermee wordt bepaald of een tekenreeks met een waarde begint. De vergelijking is niet hoofdlettergevoelig.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |Tekenreeks |De waarde die het item vinden bevat. |
| stringToFind |Ja |Tekenreeks |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

**De waarde True** als het eerste teken of van de tekenreeks overeen met de waarde; anders **False**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/startsendswith.json) laat zien hoe de functies startsWith en endsWith gebruiken:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| startsTrue | BOOL | True |
| startsCapTrue | BOOL | True |
| startsFalse | BOOL | False |
| endsTrue | BOOL | True |
| endsCapTrue | BOOL | True |
| endsFalse | BOOL | False |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/startsendswith.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/startsendswith.json
```

<a id="string" />

## <a name="string"></a>Tekenreeks
`string(valueToConvert)`

De opgegeven waarde converteert naar een tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ja | Alle |De waarde niet converteren naar tekenreeks. Elk type waarde kan worden geconverteerd, met inbegrip van objecten en -matrices. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks van de geconverteerde waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/string.json) laat zien hoe u verschillende soorten waarden converteren naar tekenreeksen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| objectOutput | Tekenreeks | {{'valueA': 10, 'valueB': "Voorbeeldtekst"} |
| arrayOutput | Tekenreeks | ["a", "b", "c"] |
| intOutput | Tekenreeks | 5 |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/string.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/string.json
```

<a id="substring" />

## <a name="substring"></a>de subtekenreeks
`substring(stringToParse, startIndex, length)`

Retourneert een subtekenreeks die begint bij de opgegeven tekenpositie en het opgegeven aantal tekens bevat.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| stringToParse |Ja |Tekenreeks |De oorspronkelijke tekenreeks weer waaruit de subtekenreeks wordt opgehaald. |
| startIndex |Nee |int |De op nul gebaseerde beginpositie voor de subtekenreeks. |
| lengte |Nee |int |Het aantal tekens voor de subtekenreeks. Moet verwijzen naar een locatie binnen de tekenreeks. |

### <a name="return-value"></a>Retourwaarde

De subtekenreeks.

### <a name="remarks"></a>Opmerkingen

De functie mislukt wanneer de subtekenreeks breder is dan het einde van de tekenreeks. Het volgende voorbeeld mislukt met de fout 'de parameters van de index en lengte moeten naar een locatie binnen de tekenreeks verwijzen. De Indexparameter: '0', de lengteparameter: 11, de lengte van de tekenreeksparameter: "10". ".

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/substring.json) een subtekenreeks uit een parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| substringOutput | Tekenreeks | twee |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/substring.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/substring.json
```

<a id="take" />

## <a name="take"></a>duren
`take(originalValue, numberToTake)`

Retourneert een tekenreeks zijn met het opgegeven aantal tekens vanaf het begin van de tekenreeks of een matrix met het opgegeven aantal elementen vanaf het begin van de matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |matrix of tekenreeks |De matrix of tekenreeks op voor het nemen van elementen uit. |
| numberToTake |Ja |int |Het aantal elementen of tekens te laten worden. Als deze waarde 0 of minder is, wordt een lege matrix of tekenreeks geretourneerd. Als deze groter is dan de lengte van de opgegeven matrix of tekenreeks is, worden alle elementen in de matrix of tekenreeks geretourneerd. |

### <a name="return-value"></a>Retourwaarde

Een matrix of tekenreeks zijn.

### <a name="examples"></a>Voorbeelden

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

<a id="tolower" />

## <a name="tolower"></a>toLower
`toLower(stringToChange)`

De opgegeven tekenreeks converteren naar kleine letters.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| stringToChange |Ja |Tekenreeks |De waarde te converteren naar kleine letters. |

### <a name="return-value"></a>Retourwaarde

De tekenreeks wordt omgezet in kleine letters.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/tolower.json) een parameterwaarde converteert naar kleine letters en hoofdletters.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| toLowerOutput | Tekenreeks | Een twee drie |
| toUpperOutput | Tekenreeks | EEN TWEE DRIE |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/tolower.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/tolower.json
```

<a id="toupper" />

## <a name="toupper"></a>toUpper
`toUpper(stringToChange)`

De opgegeven tekenreeks die wordt omgezet in hoofdletters.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| stringToChange |Ja |Tekenreeks |De waarde converteren naar hoofdletters. |

### <a name="return-value"></a>Retourwaarde

De tekenreeks wordt omgezet in hoofdletters.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/tolower.json) een parameterwaarde converteert naar kleine letters en hoofdletters.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| toLowerOutput | Tekenreeks | Een twee drie |
| toUpperOutput | Tekenreeks | EEN TWEE DRIE |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/tolower.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/tolower.json
```

<a id="trim" />

## <a name="trim"></a>Trim
`trim (stringToTrim)`

Verwijdert alle voorloop- en volgspaties spatietekens bestaan uit de opgegeven tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| stringToTrim |Ja |Tekenreeks |De waarde trim. |

### <a name="return-value"></a>Retourwaarde

De tekenreeks zonder voorloopspaties en afsluitende spatietekens bestaan.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/trim.json) verwijdert de spaties uit de parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| retourneren | Tekenreeks | Een twee drie |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/trim.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/trim.json
```

<a id="uniquestring" />

## <a name="uniquestring"></a>uniqueString
`uniqueString (baseString, ...)`

Maakt een deterministische hash-tekenreeks op basis van de waarden als parameters. 

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| baseString |Ja |Tekenreeks |De waarde die wordt gebruikt in de hash-functie voor het maken van een unieke tekenreeks. |
| extra parameters indien nodig |Nee |Tekenreeks |U kunt toevoegen die nodig zijn voor het maken van de waarde die het niveau van de uniekheid van tekenreeksen. |

### <a name="remarks"></a>Opmerkingen

Deze functie is handig als u moet een unieke naam voor een resource te maken. U opgeven parameterwaarden die het uniciteitsbereik voor het resultaat beperken. U kunt opgeven of de naam uniek naar beneden op abonnement, resourcegroep of implementatie is. 

De geretourneerde waarde is niet een willekeurige tekenreeks op, maar in plaats daarvan het resultaat van een hash-functie. De geretourneerde waarde is 13 tekens bevatten. Het is niet uniek. U kunt de waarde combineren met een voorvoegsel van de naamconventie voor het maken van een beschrijvende naam. Het volgende voorbeeld ziet de indeling van de geretourneerde waarde. De werkelijke waarde verschilt per de opgegeven parameters.

    tcvhiyu5h2o5o

De volgende voorbeelden laten zien hoe uniqueString gebruiken voor het maken van een unieke waarde voor vaak gebruikte niveaus.

Uniek binnen het bereik van abonnement

```json
"[uniqueString(subscription().subscriptionId)]"
```

Uniek binnen het bereik van resourcegroep

```json
"[uniqueString(resourceGroup().id)]"
```

Uniek binnen het bereik van implementatie voor een resourcegroep

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

Het volgende voorbeeld laat zien hoe een unieke naam voor een opslagaccount op basis van de resourcegroep maken. Binnen de resourcegroep is de naam niet uniek als op dezelfde manier samengesteld.

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met 13 tekens.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uniquestring.json) resultaten retourneert uit uniquestring:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "uniqueRG": {
            "value": "[uniqueString(resourceGroup().id)]",
            "type" : "string"
        },
        "uniqueDeploy": {
            "value": "[uniqueString(resourceGroup().id, deployment().name)]",
            "type" : "string"
        }
    }
}
```

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/uniquestring.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/uniquestring.json
```

<a id="uri" />

## <a name="uri"></a>URI
`uri (baseUri, relativeUri)`

Maakt een absolute URI door baseUri en de tekenreeks relativeUri te combineren.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| baseUri |Ja |Tekenreeks |De basis-uri-tekenreeks. |
| relativeUri |Ja |Tekenreeks |De relatieve uri-tekenreeks om toe te voegen aan de basis-uri-tekenreeks. |

De waarde voor de **baseUri** parameter kan een specifiek bestand bevatten, maar alleen het basispad wordt gebruikt bij het maken van de URI. Bijvoorbeeld, doorgeven `http://contoso.com/resources/azuredeploy.json` als de resultaten van de parameter baseUri in een basis-URI van `http://contoso.com/resources/`.

### <a name="return-value"></a>Retourwaarde

Een tekenreeks die de absolute URI voor de basis- en relatieve waarden vertegenwoordigt.

### <a name="examples"></a>Voorbeelden

Het volgende voorbeeld laat zien hoe een koppeling naar een geneste sjabloon op basis van de waarde van de bovenliggende sjabloon maken.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) laat zien hoe uri, uriComponent en uriComponentToString te gebruiken:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| uriOutput | Tekenreeks | http://contoso.com/resources/Nested/azuredeploy.JSON |
| componentOutput | Tekenreeks | HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Tekenreeks | http://contoso.com/resources/Nested/azuredeploy.JSON |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/uri.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/uri.json
```

<a id="uricomponent" />

## <a name="uricomponent"></a>uriComponent
`uricomponent(stringToEncode)`

Codeert een URI.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| stringToEncode |Ja |Tekenreeks |De waarde voor het coderen. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks van de URI gecodeerde waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) laat zien hoe uri, uriComponent en uriComponentToString te gebruiken:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| uriOutput | Tekenreeks | http://contoso.com/resources/Nested/azuredeploy.JSON |
| componentOutput | Tekenreeks | HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Tekenreeks | http://contoso.com/resources/Nested/azuredeploy.JSON |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/uri.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/uri.json
```

<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a>uriComponentToString
`uriComponentToString(uriEncodedString)`

Retourneert dat een tekenreeks van een URI-gecodeerde waarde.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Ja |Tekenreeks |De URI gecodeerde waarde converteren naar een tekenreeks. |

### <a name="return-value"></a>Retourwaarde

Een gecodeerde tekenreeks van URI gecodeerde waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) laat zien hoe uri, uriComponent en uriComponentToString te gebruiken:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| uriOutput | Tekenreeks | http://contoso.com/resources/Nested/azuredeploy.JSON |
| componentOutput | Tekenreeks | HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Tekenreeks | http://contoso.com/resources/Nested/azuredeploy.JSON |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/uri.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/uri.json
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md).
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Voor een opgegeven aantal keer herhalen bij het maken van een type resource, Zie [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor het implementeren van de sjabloon die u hebt gemaakt, [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).


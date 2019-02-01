---
title: Azure Resource Manager-sjabloonfuncties - numerieke | Microsoft Docs
description: Beschrijft de functies in een Azure Resource Manager-sjabloon gebruiken om te werken met getallen.
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
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 5ed3a0a57dad61a5fe783790eba4cb89ce19c660
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55496700"
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a>Numerieke functies voor Azure Resource Manager-sjablonen

Resource Manager biedt de volgende functies voor het werken met gehele getallen:

* [add](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [float](#float)
* [int](#int)
* [max](#max)
* [min](#min)
* [Mod](#mod)
* [mul](#mul)
* [sub](#sub)

<a id="add" />

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="add"></a>add
`add(operand1, operand2)`

Retourneert de som van de twee opgegeven gehele getallen.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- | 
|operand1 |Ja |int |Nummer van de eerste om toe te voegen. |
|operand2 |Ja |int |Tweede getal om toe te voegen. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal dat de som van de parameters bevat.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/add.json) voegt twee parameters.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to add"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to add"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| addResult | Int | 8 |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/add.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/add.json 
```

<a id="copyindex" />

## <a name="copyindex"></a>copyIndex
`copyIndex(loopName, offset)`

Retourneert de index van een lus iteratie. 

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| loopName | Nee | string | De naam van de lus voor het ophalen van de iteratie. |
| offset |Nee |int |Het aantal om toe te voegen aan de waarde op nul gebaseerde iteratie. |

### <a name="remarks"></a>Opmerkingen

Deze functie wordt altijd gebruikt met een **kopie** object. Als er geen waarde is opgegeven voor **offset**, de huidige iteratie-waarde wordt geretourneerd. De waarde van de herhaling start bij nul. U kunt iteratie lussen gebruiken bij het definiëren van resources of variabelen.

De **loopName** eigenschap kunt u opgeven of copyIndex wordt verwezen naar een iteratie van de resource of de eigenschap iteratie. Als er geen waarde is opgegeven voor **loopName**, de huidige iteratie van de resource-type wordt gebruikt. Geef een waarde voor **loopName** wanneer op een eigenschap. 
 
Voor een volledige beschrijving van hoe u **copyIndex**, Zie [meerdere exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md).

Voor een voorbeeld van het gebruik van **copyIndex** bij het definiëren van een variabele, Zie [variabelen](resource-group-authoring-templates.md#variables).

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld ziet een lus kopiëren en de indexwaarde die is opgenomen in de naam. 

```json
"resources": [ 
  { 
    "name": "[concat('examplecopy-', copyIndex())]", 
    "type": "Microsoft.Web/sites", 
    "copy": { 
      "name": "websitescopy", 
      "count": "[parameters('count')]" 
    }, 
    ...
  }
]
```

### <a name="return-value"></a>Retourwaarde

Een geheel getal die de huidige index van de iteratie vertegenwoordigt.

<a id="div" />

## <a name="div"></a>div
`div(operand1, operand2)`

Retourneert de afdeling geheel getal van de twee opgegeven gehele getallen.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Het getal wordt gedeeld. |
| operand2 |Ja |int |Het nummer dat wordt gebruikt om te delen. Mag niet 0 zijn. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal dat aangeeft van de afdeling.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/div.json) wordt één parameter door een andere parameter verdeeld.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| divResult | Int | 2 |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/div.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/div.json 
```

<a id="float" />

## <a name="float"></a>float
`float(arg1)`

De waarde omgezet in een drijvende-kommagetal zijn. U deze functie alleen gebruiken als aangepaste parameters doorgeven aan een toepassing, zoals een logische App.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |tekenreeks of int |De waarde moet worden geconverteerd naar een drijvende-kommagetal zijn. |

### <a name="return-value"></a>Retourwaarde
Een drijvende-kommagetal zijn.

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld ziet u hoe u parameters kunt toevoegen aan een logische App met drijvende komma:

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
            "custom1": {
                "value": "[float('3.0')]"
            },
            "custom2": {
                "value": "[float(3)]"
            },
```

<a id="int" />

## <a name="int"></a>int
`int(valueToConvert)`

De opgegeven waarde omgezet in een geheel getal zijn.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ja |tekenreeks of int |De waarde moet worden geconverteerd naar een geheel getal zijn. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal van de geconverteerde waarde.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/int.json) waarde van de gebruiker opgegeven parameter converteert naar geheel getal zijn.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": { 
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| intResult | Int | 4 |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/int.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/int.json
```

<a id="max" />

## <a name="max"></a>max
`max (arg1)`

Retourneert de maximumwaarde van een matrix met gehele getallen of een door komma's gescheiden lijst met gehele getallen.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matrix van gehele getallen of een door komma's gescheiden lijst met gehele getallen |De verzameling die de maximale waarde te halen. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal voor de maximale waarde uit de verzameling.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json) ziet u hoe u max met een matrix en een lijst met gehele getallen:

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

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| arrayOutput | Int | 5 |
| intOutput | Int | 5 |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

<a id="min" />

## <a name="min"></a>min.
`min (arg1)`

Retourneert de minimumwaarde van een matrix met gehele getallen of een door komma's gescheiden lijst met gehele getallen.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matrix van gehele getallen of een door komma's gescheiden lijst met gehele getallen |De verzameling om de minimale waarde. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal voor de minimale waarde uit de verzameling.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json) ziet u hoe u min met een matrix en een lijst met gehele getallen:

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

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| arrayOutput | Int | 0 |
| intOutput | Int | 0 |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

<a id="mod" />

## <a name="mod"></a>Mod
`mod(operand1, operand2)`

Retourneert de rest van de delen door geheel getal met behulp van de twee opgegeven gehele getallen.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Het getal wordt gedeeld. |
| operand2 |Ja |int |Het getal dat wordt gebruikt voor het delen, mag niet 0 zijn. |

### <a name="return-value"></a>Retourwaarde
Een geheel getal dat aangeeft van de rest.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mod.json) retourneert de rest van het delen van één parameter door een andere parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| modResult | Int | 1 |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mod.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mod.json
```

<a id="mul" />

## <a name="mul"></a>mul
`mul(operand1, operand2)`

Retourneert de vermenigvuldiging van de twee opgegeven gehele getallen.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Het eerste dat moet worden vermenigvuldigd. |
| operand2 |Ja |int |Tweede getal waarmee moet worden vermenigvuldigd. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal dat de vermenigvuldiging vertegenwoordigt.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mul.json) wordt één parameter door een andere parameter vermenigvuldigd.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to multiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to multiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| mulResult | Int | 15 |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mul.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mul.json
```

<a id="sub" />

## <a name="sub"></a>Sub
`sub(operand1, operand2)`

Retourneert de aftrekken van de twee opgegeven gehele getallen.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Het nummer dat wordt afgetrokken van de. |
| operand2 |Ja |int |Het nummer dat wordt afgetrokken. |

### <a name="return-value"></a>Retourwaarde
Een geheel getal dat de aftrekking vertegenwoordigt.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/sub.json) wordt één parameter van een andere parameter afgetrokken.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer to subtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| subResult | Int | 4 |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/sub.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/sub.json
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon, [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Op een opgegeven aantal keren herhalen bij het maken van een type resource, Zie [meerdere exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor meer informatie over het implementeren van de sjabloon die u hebt gemaakt, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).


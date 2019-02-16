---
title: Variabelen van Azure Resource Manager-sjabloon | Microsoft Docs
description: Beschrijft hoe u variabelen definiëren in een Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2019
ms.author: tomfitz
ms.openlocfilehash: 50feca90d375d6afd3b04afe019ad9f9025f19dc
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308567"
---
# <a name="variables-section-of-azure-resource-manager-templates"></a>Sectie met variabelen van Azure Resource Manager-sjablonen
In het gedeelte variabelen kunt u waarden die kunnen worden gebruikt in uw sjabloon maken. U hoeft niet te definiëren, variabelen, maar ze vaak de sjabloon vereenvoudigen door te verminderen van complexe expressies.

## <a name="define-and-use-a-variable"></a>Definiëren en een variabele te gebruiken

Het volgende voorbeeld ziet de definitie van een variabele. Hiermee maakt u een string-waarde voor de naam van een opslagaccount. Verschillende sjabloonfuncties wordt gebruikt om te halen van een waarde voor parameter en het samenvoegen tot een unieke tekenreeks.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

U kunt de variabele gebruiken bij het definiëren van de resource.

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    ...
```

## <a name="available-definitions"></a>Beschikbare definities

Het vorige voorbeeld hebt u geleerd één manier om te definiëren van een variabele. U kunt een van de volgende definities:

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    },
    "<variable-object-name>": {
        "copy": [
            {
                "name": "<name-of-array-property>",
                "count": <number-of-iterations>,
                "input": <object-or-value-to-repeat>
            }
        ]
    },
    "copy": [
        {
            "name": "<variable-array-name>",
            "count": <number-of-iterations>,
            "input": <object-or-value-to-repeat>
        }
    ]
}
```

## <a name="configuration-variables"></a>Configuratievariabelen voor de

U kunt complexe JSON-typen gebruiken om gerelateerde waarden voor een omgeving te definiëren. 

```json
"variables": {
    "environmentSettings": {
        "test": {
            "instanceSize": "Small",
            "instanceCount": 1
        },
        "prod": {
            "instanceSize": "Large",
            "instanceCount": 4
        }
    }
},
```

In de parameters maakt u een waarde die welke configuratiewaarden aangeeft te gebruiken.

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
```

Ophalen van de huidige instellingen met:

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="use-copy-element-in-variable-definition"></a>Kopiëren-element in de definitie van de variabele gebruiken

U kunt meerdere exemplaren van een variabele maken met de `copy` eigenschap in de sectie met variabelen. U maakt een matrix van elementen die zijn samengesteld uit de waarde in de `input` eigenschap. U kunt de `copy` eigenschap in een variabele, of op het hoogste niveau van de sectie met variabelen. Bij het gebruik van `copyIndex` in een variabele iteratie moet u de naam van de iteratie opgeven.

Het volgende voorbeeld laat zien hoe u kopiëren:

```json
"variables": {
  "disk-array-on-object": {
    "copy": [
      {
        "name": "disks",
        "count": 3,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('disks')]"
        }
      }
    ]
  },
  "copy": [
    {
      "name": "disks-top-level-array",
      "count": 3,
      "input": {
        "name": "[concat('myDataDisk', copyIndex('disks-top-level-array', 1))]",
        "diskSizeGB": "1",
        "diskIndex": "[copyIndex('disks-top-level-array')]"
      }
    },
    {
      "name": "top-level-string-array",
      "count": 5,
      "input": "[concat('myDataDisk', copyIndex('top-level-string-array', 1))]"
    }
  ]
},
```

Nadat de kopie-expressie is geëvalueerd, de variabele **schijf matrix op object** bevat de volgende object met een matrix met de naam **schijven**:

```json
{
  "disks": [
    {
      "name": "myDataDisk1",
      "diskSizeGB": "1",
      "diskIndex": 0
    },
    {
      "name": "myDataDisk2",
      "diskSizeGB": "1",
      "diskIndex": 1
    },
    {
      "name": "myDataDisk3",
      "diskSizeGB": "1",
      "diskIndex": 2
    }
  ]
}
```

De variabele **schijven-bovenaan-niveau-matrix** bevat de volgende matrix:

```json
[
  {
    "name": "myDataDisk1",
    "diskSizeGB": "1",
    "diskIndex": 0
  },
  {
    "name": "myDataDisk2",
    "diskSizeGB": "1",
    "diskIndex": 1
  },
  {
    "name": "myDataDisk3",
    "diskSizeGB": "1",
    "diskIndex": 2
  }
]
```

De variabele **top-niveau-string-matrix** bevat de volgende matrix:

```json
[
  "myDataDisk1",
  "myDataDisk2",
  "myDataDisk3",
  "myDataDisk4",
  "myDataDisk5"
]
```

Via het kopiëren van werkt goed als u wilt nemen parameterwaarden op te geven en toe te wijzen aan de bronwaarden. Het volgende voorbeeld wordt de parameterwaarden voor gebruik bij het definiëren van de beveiligingsregels voor verbindingen:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "securityRules": {
      "type": "array"
    }
  },
  "variables": {
    "copy": [
      {
        "name": "securityRules",
        "count": "[length(parameters('securityRules'))]",
        "input": {
          "name": "[parameters('securityRules')[copyIndex('securityRules')].name]",
          "properties": {
            "description": "[parameters('securityRules')[copyIndex('securityRules')].description]",
            "priority": "[parameters('securityRules')[copyIndex('securityRules')].priority]",
            "protocol": "[parameters('securityRules')[copyIndex('securityRules')].protocol]",
            "sourcePortRange": "[parameters('securityRules')[copyIndex('securityRules')].sourcePortRange]",
            "destinationPortRange": "[parameters('securityRules')[copyIndex('securityRules')].destinationPortRange]",
            "sourceAddressPrefix": "[parameters('securityRules')[copyIndex('securityRules')].sourceAddressPrefix]",
            "destinationAddressPrefix": "[parameters('securityRules')[copyIndex('securityRules')].destinationAddressPrefix]",
            "access": "[parameters('securityRules')[copyIndex('securityRules')].access]",
            "direction": "[parameters('securityRules')[copyIndex('securityRules')].direction]"
          }
        }
      }
    ]
  },
  "resources": [
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "NSG1",
      "location": "[resourceGroup().location]",
      "properties": {
        "securityRules": "[variables('securityRules')]"
      }
    }
  ],
  "outputs": {}
}
```

## <a name="example-templates"></a>Voorbeeldsjablonen

Deze voorbeeldsjablonen laten zien voor sommige scenario's voor het gebruik van variabelen. Te testen hoe variabelen worden verwerkt in verschillende scenario's implementeren. 

|Template  |Description  |
|---------|---------|
| [definities van variabele](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Ziet u de verschillende typen variabelen. De sjabloon implementeren niet alle resources. Deze waarden van variabelen constructs en die waarden retourneert. |
| [van configuratievariabele](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Ziet u het gebruik van een variabele die de configuratiewaarden definieert. De sjabloon implementeren niet alle resources. Deze waarden van variabelen constructs en die waarden retourneert. |
| [netwerkbeveiligingsregels](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) en [parameterbestand](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Vormt het een matrix in de juiste indeling voor het beveiligingsregels toewijzen aan een netwerkbeveiligingsgroep. |


## <a name="next-steps"></a>Volgende stappen
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Zie voor meer informatie over de functies die u uit in een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Zie voor aanbevelingen over het maken van sjablonen, [aanbevolen procedures voor Azure Resource Manager-sjabloon](template-best-practices.md).
* Mogelijk moet u de resources die zijn opgeslagen in een andere resourcegroep gebruiken. In dit scenario is gebruikelijk bij het werken met opslagaccounts of virtuele netwerken die worden gedeeld door meer dan één resourcegroep. Zie voor meer informatie de [resourceId functie](resource-group-template-functions-resource.md#resourceid).
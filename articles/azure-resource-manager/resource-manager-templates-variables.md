---
title: Variabelen van Azure Resource Manager-sjabloon | Microsoft Docs
description: "Beschrijft hoe variabelen definiëren in een Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: tomfitz
ms.openlocfilehash: 8d9f227ad1f450cf6cdfca1dafb1b51bc6f6c9f9
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="variables-section-of-azure-resource-manager-templates"></a>Sectie met sjabloonvariabelen van Azure Resource Manager-sjablonen
In het gedeelte variabelen kunt u waarden die kunnen worden gebruikt in uw sjabloon opstellen. U hoeft geen variabelen definiëren, maar ze vaak uw sjabloon vereenvoudigen doordat complexe expressies.

## <a name="define-and-use-a-variable"></a>Definieer en het gebruik van een variabele

Het volgende voorbeeld toont de definitie van een variabele. Een tekenreekswaarde voor de naam van een opslagaccount wordt gemaakt. Verschillende sjabloonfuncties wordt gebruikt voor een parameterwaarde ophalen en deze samenvoegen tot een unieke tekenreeks.

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

Het vorige voorbeeld hebt u geleerd een manier om een variabele definiëren. U kunt een van de volgende definities gebruiken:

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
                "input": {
                    <properties-to-repeat>
                }
            }
        ]
    },
    "copy": [
        {
            "name": "<variable-array-name>",
            "count": <number-of-iterations>,
            "input": {
                <properties-to-repeat>
            }
        }
    ]
}
```

## <a name="configuration-variables"></a>Configuratievariabelen voor de

Complexe JSON-typen kunt u gerelateerde waarden voor een omgeving definiëren. 

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

In parameters maakt u een waarde die welke configuratiewaarden aangeeft te gebruiken.

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

## <a name="use-copy-element-in-variable-definition"></a>Gebruik van kopiëren-element in de definitie van variabele

U kunt de **kopie** syntaxis van een variabele maken met een matrix van meerdere elementen. U kunt een aantal opgeven voor het aantal elementen. Elk element bevat de eigenschappen in de **invoer** object. U kunt kopiëren binnen een variabele of de variabele maken. Wanneer u een variabele definiëren en gebruikt **kopie** binnen die variabele maakt u een object waarvoor de matrixeigenschap van een. Als u werkt met **kopie** op het hoogste niveau en definieert u een of meer variabelen binnen het hebt u een of meer matrices. Beide benaderingen worden weergegeven in het volgende voorbeeld:

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
        }
    ]
},
```

De variabele **schijf matrix op object** bevat de volgende object met een matrix met de naam **schijven**:

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

De variabele **schijven top-niveau matrix** bevat de volgende matrix:

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

U kunt ook meer dan één object opgeven wanneer zij exemplaar gebruiken voor het maken van variabelen. Het volgende voorbeeld definieert twee matrices als variabelen. Een heet **schijven top-niveau matrix** en vijf elementen heeft. De andere heet **een andere-matrix** en drie elementen heeft.

```json
"variables": {
    "copy": [
        {
            "name": "disks-top-level-array",
            "count": 5,
            "input": {
                "name": "[concat('oneDataDisk', copyIndex('disks-top-level-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('disks-top-level-array')]"
            }
        },
        {
            "name": "a-different-array",
            "count": 3,
            "input": {
                "name": "[concat('twoDataDisk', copyIndex('a-different-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('a-different-array')]"
            }
        }
    ]
},
```

Deze methode is geschikt als u wilt nemen parameterwaarden en zorg ervoor dat ze zich in de juiste notatie voor een waarde van sjabloon. Het volgende voorbeeld wordt de parameterwaarden voor gebruik bij het definiëren van de beveiligingsregels voor verbindingen:

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

## <a name="recommendations"></a>Aanbevelingen
De volgende informatie kan nuttig zijn wanneer u met variabelen werkt:

* Variabelen voor waarden die u moet meer dan één keer gebruiken in een sjabloon gebruiken. Als een waarde slechts één keer gebruikt wordt, een vastgelegde waarde maakt de sjabloon gemakkelijker te lezen.
* U kunt geen gebruiken de [verwijzing](resource-group-template-functions-resource.md#reference) werken in de **variabelen** gedeelte van de sjabloon. De **verwijzing** functie is afgeleid van de waarde van de runtimestatus van de resource. Variabelen zijn echter opgelost bij het eerste parseren van de sjabloon. Constructie waarden die moeten de **verwijzing** werken rechtstreeks in de **resources** of **levert** gedeelte van de sjabloon.
* Variabelen voor de resourcenamen uniek moeten zijn.

## <a name="example-templates"></a>Voorbeeld-sjablonen

Deze sjablonen voorbeeld ziet u enkele scenario's voor het gebruik van variabelen. Deze als u wilt testen hoe variabelen worden verwerkt in verschillende scenario's implementeren. 

|Template  |Beschrijving  |
|---------|---------|
| [definities van variabele](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Demonstreert de verschillende soorten variabelen. De sjabloon implementeert geen bronnen. Deze waarden van variabelen constructs en die waarden retourneert. |
| [van configuratievariabele](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Demonstreert het gebruik van een variabele die configuratiewaarden definieert. De sjabloon implementeert geen bronnen. Deze waarden van variabelen constructs en die waarden retourneert. |
| [netwerk-beveiligingsregels](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) en [parameterbestand](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Hiermee wordt een matrix in de juiste indeling voor het beveiligingsregels toewijzen aan een netwerkbeveiligingsgroep. |


## <a name="next-steps"></a>Volgende stappen
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Zie voor meer informatie over de functies die u van binnen een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Als u wilt combineren meerdere sjablonen tijdens de implementatie, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Wellicht moet u bronnen gebruiken die zijn opgeslagen in een andere resourcegroep. Dit scenario is gebruikelijk dat wanneer u werkt met opslagaccounts of virtuele netwerken die worden gedeeld door meerdere resourcegroepen. Zie voor meer informatie de [resourceId functie](resource-group-template-functions-resource.md#resourceid).
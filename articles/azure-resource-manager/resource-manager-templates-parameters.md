---
title: Azure Resource Manager-sjabloon parametersectie | Microsoft Docs
description: Hierin wordt beschreven in de parametersectie van Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis.
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
ms.date: 12/18/2018
ms.author: tomfitz
ms.openlocfilehash: fd6fcff6ac556abe3b2d34c7e8b1b0290208f5b0
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53722139"
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>De parametersectie van Azure Resource Manager-sjablonen
U opgeven welke waarden u invoeren kunt bij het implementeren van de resources in de parametersectie van de sjabloon. De parameterwaarden van deze kunnen u de implementatie aanpassen door het verstrekken van waarden die zijn aangepast voor een bepaalde omgeving (zoals ontwikkelen, testen en productie). U hoeft te bieden van parameters in de sjabloon, maar zonder parameters de sjabloon altijd dezelfde resources met dezelfde namen, locaties en eigenschappen wilt implementeren.

U bent beperkt tot 256 parameters in een sjabloon. U kunt het aantal parameters verminderen door middel van objecten die meerdere eigenschappen bevat, zoals weergegeven in dit artikel.

## <a name="define-and-use-a-parameter"></a>Definiëren en gebruiken van een parameter

Het volgende voorbeeld ziet een eenvoudige parameterdefinitie. De naam van de parameter definieert, en geeft aan dat het duurt een string-waarde voordat. De parameter accepteert alleen waarden die geschikt zijn voor het beoogde gebruik. Het geeft een standaardwaarde wanneer er geen waarde is opgegeven tijdens de implementatie. Ten slotte bevat de parameter een beschrijving van het gebruik ervan. 

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }   
}
```

In de sjabloon, verwijst u naar de waarde voor de parameter met de volgende syntaxis:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="available-properties"></a>Beschikbare eigenschappen

Het vorige voorbeeld hebt u slechts een deel van de eigenschappen die u in de sectie gebruiken kunt geleerd. De beschikbare eigenschappen zijn:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| De naam van element | Vereist | Description |
|:--- |:--- |:--- |
| parameterName |Ja |Naam van de parameter. Moet een geldige JavaScript-id. |
| type |Ja |Het type van de waarde van parameter. De toegestane typen en de waarden zijn **tekenreeks**, **securestring**, **int**, **bool**, **object**, **secureObject**, en **matrix**. |
| defaultValue |Nee |De standaardwaarde voor de parameter, als er geen waarde is opgegeven voor de parameter. |
| allowedValues |Nee |Matrix van toegestane waarden voor de parameter om ervoor te zorgen dat de juiste waarde is opgegeven. |
| Minimumwaarde |Nee |De minimumwaarde voor de parameters van het type int, deze waarde wordt ook meegerekend. |
| Maximumwaarde |Nee |De maximale waarde voor de parameters van het type int, deze waarde wordt ook meegerekend. |
| minLength |Nee |De minimale lengte van tekenreeks, een beveiligde tekenreeks en parameters van het type matrix, deze waarde wordt ook meegerekend. |
| maxLength |Nee |De maximale lengte voor string, beveiligde tekenreeks en parameters van het type matrix, deze waarde wordt ook meegerekend. |
| description |Nee |Beschrijving van de parameter die wordt weergegeven voor gebruikers via de portal. |

## <a name="template-functions-with-parameters"></a>Sjabloonfuncties met parameters

Wanneer u de standaardwaarde voor een parameter opgeeft, kunt u de meeste sjabloonfuncties. Een andere waarde voor de parameter kunt u een standaardwaarde te bouwen. De volgende sjabloon ziet u het gebruik van functies in de standaard-waarde:

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

U kunt geen gebruiken de `reference` functie in de parametersectie. Parameters worden geëvalueerd vóór de implementatie zodat de `reference` functie kan de runtimestatus van een resource niet ophalen. 

## <a name="objects-as-parameters"></a>Objecten als parameters

Kan het zijn beter te organiseren van gerelateerde waarden doordat ze worden doorgegeven als een object. Deze aanpak vermindert ook het aantal parameters in de sjabloon.

De parameter in de sjabloon definieert en geeft u een JSON-object in plaats van een enkele waarde tijdens de implementatie. 

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Vervolgens, verwijzen naar de subeigenschappen van de parameter met behulp van de punt.

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="example-templates"></a>Voorbeeldsjablonen

Deze voorbeeldsjablonen laten zien voor sommige scenario's voor het gebruik van parameters. Te testen hoe parameters worden verwerkt in verschillende scenario's implementeren.

|Template  |Description  |
|---------|---------|
|[parameters met functies voor standaardwaarden](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Demonstreert hoe u sjabloonfuncties gebruiken bij het definiëren van de standaardwaarden voor parameters. De sjabloon implementeren niet alle resources. Deze parameterwaarden constructs en die waarden retourneert. |
|[Parameter-object](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Ziet u met behulp van een object voor een parameter. De sjabloon implementeren niet alle resources. Deze parameterwaarden constructs en die waarden retourneert. |

## <a name="next-steps"></a>Volgende stappen

* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Voor informatie over het invoeren van de parameterwaarden tijdens de implementatie, Zie [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md). 
* Zie voor aanbevelingen over het maken van sjablonen, [aanbevolen procedures voor Azure Resource Manager-sjabloon](template-best-practices.md).
* Zie voor meer informatie over het gebruik van een parameter-object [een object gebruiken als een parameter in een Azure Resource Manager-sjabloon](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

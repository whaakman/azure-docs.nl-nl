---
title: Structuur van Azure Resource Manager-sjabloon en syntaxis | Microsoft Docs
description: Beschrijft de structuur en eigenschappen van Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2017
ms.author: tomfitz
ms.openlocfilehash: b0bc5abd768be0fa5876aaef108cd71a15d94510
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Overzicht van de structuur en de syntaxis van Azure Resource Manager-sjablonen
In dit artikel beschrijft de structuur van een Azure Resource Manager-sjabloon. Dit geeft de verschillende secties van een sjabloon en de eigenschappen die beschikbaar in deze secties zijn. De sjabloon bestaat uit JSON en uitdrukkingen die u gebruiken kunt om waarden voor uw implementatie samen te stellen. Zie voor een stapsgewijze zelfstudie over het maken van een sjabloon, [maken van uw eerste Azure Resource Manager-sjabloon](resource-manager-create-first-template.md).

## <a name="template-format"></a>Sjabloon
In de meest eenvoudige structuur bevat een sjabloon voor de volgende elementen:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| Elementnaam | Vereist | Beschrijving |
|:--- |:--- |:--- |
| $schema |Ja |Locatie van het JSON-schema-bestand dat de versie van de taal van de sjabloon beschrijft. Gebruik de URL die wordt weergegeven in het voorgaande voorbeeld. |
| contentVersion |Ja |De versie van de sjabloon (zoals 1.0.0.0). U kunt een waarde opgeven voor dit element. Bij het implementeren van resources met behulp van de sjabloon, kan deze waarde kan worden gebruikt om ervoor te zorgen dat de juiste sjabloon wordt gebruikt. |
| parameters |Nee |De waarden die beschikbaar zijn wanneer de implementatie wordt uitgevoerd voor het aanpassen van de resource-implementatie. |
| variabelen |Nee |De waarden die worden gebruikt als JSON-fragmenten in de sjabloon voor sjabloontaalexpressies vereenvoudigen. |
| bronnen |Ja |Brontypen die worden geïmplementeerd of bijgewerkt in een resourcegroep. |
| uitvoer |Nee |De waarden die na de implementatie worden geretourneerd. |

Elk element bevat eigenschappen die u kunt instellen. Het volgende voorbeeld bevat de volledige syntaxis van een sjabloon:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
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
    },
    "variables": {
        "<variable-name>": "<variable-value>",
        "<variable-object-name>": {
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
    },
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

Dit artikel wordt beschreven in de secties van de sjabloon in meer detail.

## <a name="syntax"></a>Syntaxis
De syntaxis van de basis van de sjabloon is JSON. Expressies en functies echter uitbreiden de JSON-waarden die beschikbaar zijn in de sjabloon.  Expressies zijn geschreven in JSON-letterlijke waarvan de eerste en laatste tekens zijn de vierkante haken: `[` en `]`respectievelijk. De waarde van de expressie wordt geëvalueerd wanneer de sjabloon wordt geïmplementeerd. Hoewel geschreven als een letterlijke tekenreeks, zijn het resultaat van evaluatie van de expressie van een ander JSON-type, zoals een matrix of een geheel getal, afhankelijk van de werkelijke expressie.  Een letterlijke tekenreeks beginnen met een haakje hebben `[`, maar dit wordt geïnterpreteerd als een expressie niet, Voeg een extra haakje voor het starten van de tekenreeks met `[[`.

Normaal gesproken u expressies gebruiken met functies bewerkingen voor het configureren van de implementatie uit te voeren. Net zoals in JavaScript-functieaanroepen die zijn opgemaakt als `functionName(arg1,arg2,arg3)`. U verwijzen naar eigenschappen met behulp van de punt en [index] operators.

Het volgende voorbeeld ziet u hoe u verschillende functies gebruikt bij het maken van een waarde:

```json
"variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
}
```

Zie voor een volledige lijst van sjabloonfuncties [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md). 

## <a name="parameters"></a>Parameters
U opgeven welke waarden u invoeren kunt bij het implementeren van de resources in het gedeelte parameters van de sjabloon. De parameterwaarden van deze kunnen u de implementatie aanpassen door het verstrekken van waarden die zijn aangepast voor een bepaalde omgeving (zoals ontwikkelen, testen en productie). U hoeft niet te bieden parameters in de sjabloon, maar zonder parameters uw sjabloon altijd dezelfde resources met dezelfde namen, locaties en eigenschappen zou implementeren.

Het volgende voorbeeld ziet u een eenvoudige parameterdefinitie:

```json
"parameters": {
  "siteNamePrefix": {
    "type": "string",
    "metadata": {
      "description": "The name prefix of the web app that you wish to create."
    }
  },
},
```

Zie voor meer informatie over het definiëren van parameters [sectie opdrachtregelparameters van Azure Resource Manager-sjablonen](resource-manager-templates-parameters.md).

## <a name="variables"></a>Variabelen
In het gedeelte variabelen kunt u waarden die kunnen worden gebruikt in uw sjabloon opstellen. U hoeft geen variabelen definiëren, maar ze vaak uw sjabloon vereenvoudigen doordat complexe expressies.

Het volgende voorbeeld ziet u een eenvoudige variabele-definitie:

```json
"variables": {
  "webSiteName": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
},
```

Zie voor meer informatie over het definiëren van variabelen [gedeelte variabelen van Azure Resource Manager-sjablonen](resource-manager-templates-variables.md).

## <a name="resources"></a>Resources
In de bronnensectie definieert u de resources die worden geïmplementeerd of bijgewerkt. Deze sectie kunt krijgen ingewikkeld omdat de typen die u implementeert de juiste waarden opgeven dat u begrijpt.

```json
"resources": [
  {
    "apiVersion": "2016-08-01",
    "name": "[variables('webSiteName')]",
    "type": "Microsoft.Web/sites",
    "location": "[resourceGroup().location]",
    "properties": {
      "serverFarmId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Web/serverFarms/<plan-name>"
    }
  }
],
```

Zie voor meer informatie [bronnensectie van Azure Resource Manager-sjablonen](resource-manager-templates-resources.md).

## <a name="outputs"></a>Uitvoer
In de sectie uitvoer geeft u de waarden die worden geretourneerd van de implementatie. U kan bijvoorbeeld de URI voor toegang tot een geïmplementeerde resource geretourneerd.

```json
"outputs": {
  "newHostName": {
    "type": "string",
    "value": "[reference(variables('webSiteName')).defaultHostName]"
  }
}
```

Zie voor meer informatie [levert sectie van Azure Resource Manager-sjablonen](resource-manager-templates-outputs.md).

## <a name="template-limits"></a>Limieten voor sjabloon

Beperkt de omvang van uw sjabloon 1 MB en elk parameterbestand 64 kB. De limiet van 1 MB geldt voor de definitieve status van de sjabloon nadat deze is uitgebreid met iteratieve resourcedefinities en waarden voor parameters en variabelen. 

Ook bent u beperkt tot:

* 256 parameters
* 256 variabelen
* 800 bronnen (zoals aantal kopieën)
* 64 uitvoerwaarden
* 24.576 tekens in een sjabloonexpressie

U kunt sommige limieten sjabloon met een geneste sjabloon overschrijdt. Zie voor meer informatie [gekoppelde sjablonen gebruiken bij het implementeren van Azure-resources](resource-group-linked-templates.md). Als u het aantal parameters en variabelen en uitvoer, kunt u verschillende waarden combineren in een object. Zie voor meer informatie [objecten als parameters](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Volgende stappen
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Zie voor meer informatie over de functies die u van binnen een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Als u wilt combineren meerdere sjablonen tijdens de implementatie, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Wellicht moet u bronnen gebruiken die zijn opgeslagen in een andere resourcegroep. Dit scenario is gebruikelijk dat wanneer u werkt met opslagaccounts of virtuele netwerken die worden gedeeld door meerdere resourcegroepen. Zie voor meer informatie de [resourceId functie](resource-group-template-functions-resource.md#resourceid).

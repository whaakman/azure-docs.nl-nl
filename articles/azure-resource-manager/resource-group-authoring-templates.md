---
title: Sjabloonstructuur van Azure Resource Manager-en de syntaxis | Microsoft Docs
description: Beschrijft de structuur en de eigenschappen van Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/22/2018
ms.author: tomfitz
ms.openlocfilehash: 28ef94113c76cd70e12a9682e1c523afc3f0a233
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945872"
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Informatie over de structuur en de syntaxis van Azure Resource Manager-sjablonen
Dit artikel beschrijft de structuur van een Azure Resource Manager-sjabloon. Deze geeft de verschillende secties van een sjabloon en de eigenschappen die beschikbaar in deze secties zijn. De sjabloon bestaat uit JSON en uitdrukkingen die u gebruiken kunt om waarden voor uw implementatie samen te stellen. Zie voor een stapsgewijze zelfstudie over het maken van een sjabloon, [uw eerste Azure Resource Manager-sjabloon maken](resource-manager-create-first-template.md).

## <a name="quickstarts-and-tutorials"></a>Snelstartgidsen en zelfstudies

Gebruik de volgende snelstartgidsen en zelfstudies voor meer informatie over het ontwikkelen van Resource Manager-sjablonen:

- Snelstartgidsen

  	|Titel|Beschrijving|
  	|------|-----|
  	|[Azure Portal gebruiken](./resource-manager-quickstart-create-templates-use-the-portal.md)|Genereren van een sjabloon met behulp van de portal en het proces van bewerken en de sjabloon te implementeren.|
  	|[Gebruik Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)|Met Visual Studio Code kunt maken en bewerken van sjablonen en het gebruik van de Azure Cloud shell om sjablonen te implementeren.|
  	|[Gebruik Visual Studio](./vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)|Visual Studio gebruiken om te maken, bewerken en sjablonen implementeren.|

- Zelfstudies

  	|Titel|Beschrijving|
  	|------|-----|
  	|[Verwijzing naar de sjabloon gebruiken](./resource-manager-tutorial-create-encrypted-storage-accounts.md)|Gebruikmaken van het referentiemateriaal voor sjablonen voor het ontwikkelen van sjablonen. In de zelfstudie, kunt u de storage-account schema vinden en gebruik de informatie voor het maken van een versleuteld opslagaccount.|
  	|[Meerdere exemplaren maken](./resource-manager-tutorial-create-multiple-instances.md)|Meerdere exemplaren van Azure-resources maken. In de zelfstudie maakt u meerdere exemplaren van de storage-account.|
  	|[Implementatievolgorde van de resource instellen](./resource-manager-tutorial-create-templates-with-dependent-resources.md)|Afhankelijkheden van resources definiëren. In de zelfstudie maakt u een virtueel netwerk, een virtuele machine en het afhankelijke Azure-resources. Leert u hoe de afhankelijkheden worden gedefinieerd.|
  	|[Voorwaarden gebruiken](./resource-manager-tutorial-use-conditions.md)|Implementeren van resources op basis van bepaalde parameterwaarden. In de zelfstudie hebt definiëren u een sjabloon voor het maken van een nieuw opslagaccount of gebruik een bestaand opslagaccount op basis van de waarde van een parameter.|
  	|[Key vault integreren](./resource-manager-tutorial-use-key-vault.md)|Ophalen van geheimen/wachtwoorden van Azure Key Vault. In de zelfstudie maakt u een virtuele machine.  Het beheerderswachtwoord voor de virtuele machine wordt opgehaald uit een Key Vault.|
  	|[Gekoppelde sjablonen maken](./resource-manager-tutorial-create-linked-templates.md)|Modularize sjablonen en andere sjablonen aanroepen vanuit een sjabloon. In de zelfstudie maakt u een virtueel netwerk, een virtuele machine en de afhankelijke resources.  Het afhankelijke opslagaccount is gedefinieerd in een gekoppelde sjabloon. |
  	|[Veilige implementatiemethoden gebruiken](./deployment-manager-tutorial.md)|Gebruik Azure Deployment manager. |

## <a name="template-format"></a>Sjabloonindeling

In de meest eenvoudige structuur heeft een sjabloon voor de volgende elementen:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "functions": [  ],
    "resources": [  ],
    "outputs": {  }
}
```

| De naam van element | Vereist | Beschrijving |
|:--- |:--- |:--- |
| $schema |Ja |Locatie van het JSON-schema-bestand dat de versie van de taal van de sjabloon beschrijft. Gebruik de URL die wordt weergegeven in het voorgaande voorbeeld. |
| Sjablooneigenschap |Ja |De versie van de sjabloon (bijvoorbeeld 1.0.0.0). U kunt een waarde opgeven voor dit element. Gebruik deze waarde aan aanzienlijke wijzigingen in het document in de sjabloon. Bij het implementeren van resources met behulp van de sjabloon, kan deze waarde kan worden gebruikt om ervoor te zorgen dat de juiste sjabloon wordt gebruikt. |
| parameters |Nee |De waarden die zijn opgegeven wanneer de implementatie wordt uitgevoerd om aan te passen van de resource-implementatie. |
| Variabelen |Nee |De waarden die worden gebruikt als JSON-fragmenten in de sjabloon voor het vereenvoudigen van sjabloontaalexpressies. |
| functions |Nee |Gebruiker gedefinieerde functies die beschikbaar in de sjabloon zijn. |
| bronnen |Ja |De resourcetypen die worden geïmplementeerd of bijgewerkt in een resourcegroep. |
| uitvoer |Nee |De waarden die zijn geretourneerd na de implementatie. |

Elk element heeft eigenschappen die u kunt instellen. Het volgende voorbeeld ziet u de volledige syntaxis voor een sjabloon:

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
    "functions": [
      {
        "namespace": "<namespace-for-your-function>",
        "members": {
          "<function-name>": {
            "parameters": [
              {
                "name": "<parameter-name>",
                "type": "<type-of-parameter-value>"
              }
            ],
            "output": {
              "type": "<type-of-output-value>",
              "value": "<function-expression>"
            }
          }
        }
      }
    ],
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
De algemene syntaxis van de sjabloon is JSON. Expressies en functies echter uitbreiden de JSON-waarden die beschikbaar zijn in de sjabloon.  Expressies zijn geschreven in JSON-letterlijke waarvan de eerste en laatste tekens zijn de vierkante haken: `[` en `]`, respectievelijk. De waarde van de expressie wordt geëvalueerd wanneer de sjabloon wordt geïmplementeerd. Terwijl geschreven als een letterlijke tekenreeks, kan het resultaat van evaluatie van de expressie van een ander JSON-type, zoals een matrix of een geheel getal, afhankelijk van de werkelijke expressie zijn.  Moet een letterlijke tekenreeks begint met een haakje `[`, maar niet het geïnterpreteerd als een expressie, toevoegen van een extra haakje voor het starten van de tekenreeks met `[[`.

Meestal gebruikt u expressies met functions bewerkingen voor het configureren van de implementatie uit te voeren. Net zoals in JavaScript-functieaanroepen die zijn opgemaakt als `functionName(arg1,arg2,arg3)`. U verwijzen naar eigenschappen met behulp van de operators stip en [index].

Het volgende voorbeeld laat zien hoe u verschillende functies bij het maken van een waarde:

```json
"variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
}
```

Zie voor de volledige lijst van de sjabloonvariabelen [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md). 

## <a name="parameters"></a>Parameters
U opgeven welke waarden u invoeren kunt bij het implementeren van de resources in de parametersectie van de sjabloon. De parameterwaarden van deze kunnen u de implementatie aanpassen door het verstrekken van waarden die zijn aangepast voor een bepaalde omgeving (zoals ontwikkelen, testen en productie). U hoeft te bieden van parameters in de sjabloon, maar zonder parameters de sjabloon altijd dezelfde resources met dezelfde namen, locaties en eigenschappen wilt implementeren.

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
In het gedeelte variabelen kunt u waarden die kunnen worden gebruikt in uw sjabloon maken. U hoeft niet te definiëren, variabelen, maar ze vaak de sjabloon vereenvoudigen door te verminderen van complexe expressies.

Het volgende voorbeeld ziet u een eenvoudige definitie van de variabele:

```json
"variables": {
  "webSiteName": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
},
```

Zie voor meer informatie over het definiëren van variabelen [sectie met variabelen van Azure Resource Manager-sjablonen](resource-manager-templates-variables.md).

## <a name="functions"></a>Functions

U kunt uw eigen functies maken in uw sjabloon. Deze functies zijn beschikbaar voor gebruik in uw sjabloon. Normaal gesproken definieert u een complexe expressie die u niet wilt herhalen in uw sjabloon. U maakt de gebruiker gedefinieerde functies van expressies en [functies](resource-group-template-functions.md) die worden ondersteund in de sjablonen.

Bij het definiëren van de functie van een gebruiker, zijn er enkele beperkingen:

* De functie heeft geen toegang tot variabelen.
* De functie heeft geen toegang tot Sjabloonparameters. Dat wil zeggen, de [parameters functie](resource-group-template-functions-deployment.md#parameters) is beperkt tot functieparameters.
* De functie kan geen andere door de gebruiker gedefinieerde functies aanroepen.
* De functie niet gebruiken de [verwijzen naar de functie](resource-group-template-functions-resource.md#reference).
* Parameters voor de functie kunnen geen standaardwaarden hebben.

Uw functies vereisen een naamruimtewaarde naamconflicten met sjabloonfuncties worden voorkomen. Het volgende voorbeeld ziet u een functie die resulteert in een storage-accountnaam:

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

U kunt de functie met aanroepen:

```json
"resources": [
  {
    "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "location": "South Central US",
    "tags": {},
    "properties": {}
  }
]
```

## <a name="resources"></a>Resources
In de sectie resources definieert u de resources die worden geïmplementeerd of bijgewerkt. In deze sectie krijgt ingewikkeld omdat u de typen die u implementeert zodat de juiste waarden moet begrijpen.

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

Voorwaardelijk opnemen of uitsluiten van een resource tijdens de implementatie, gebruikt u de [voorwaarde element](resource-manager-templates-resources.md#condition). Zie voor meer informatie over de sectie met resources [bronnensectie van Azure Resource Manager-sjablonen](resource-manager-templates-resources.md).

## <a name="outputs"></a>Uitvoer
In de sectie uitvoer geeft u waarden die zijn geretourneerd na de implementatie. U kunt bijvoorbeeld de URI voor toegang tot een geïmplementeerde resource retourneren.

```json
"outputs": {
  "newHostName": {
    "type": "string",
    "value": "[reference(variables('webSiteName')).defaultHostName]"
  }
}
```

Zie voor meer informatie, [gedeelte van Azure Resource Manager-sjablonen levert](resource-manager-templates-outputs.md).

## <a name="template-limits"></a>Limieten voor sjabloon

Beperkt de grootte van uw sjabloon 1 MB en elk parameterbestand aan 64 KB. De limiet van 1 MB is van toepassing op de laatste status van de sjabloon nadat deze is uitgebreid met iteratieve resourcedefinities en waarden voor parameters en variabelen. 

U bent ook beperkt tot:

* 256-parameters
* 256 variabelen
* 800 bronnen (zoals aantal kopieën)
* 64 uitvoerwaarden
* 24.576 tekens in een sjabloonexpressie voor een

U kunt sommige limieten sjabloon met behulp van een geneste sjabloon overschrijdt. Zie voor meer informatie, [gekoppelde sjablonen gebruiken bij het implementeren van Azure-resources](resource-group-linked-templates.md). Als u wilt verkleinen het aantal parameters, variabelen of uitvoer, kunt u verschillende waarden combineren in een object. Zie voor meer informatie, [objecten als parameters](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Volgende stappen
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Zie voor meer informatie over de functies die u uit in een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Als u wilt combineren meerdere sjablonen tijdens de implementatie, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Raadpleeg [Develop Azure Resource Manager templates for cloud consistency](templates-cloud-consistency.md) (Azure Resource Manager-sjablonen ontwikkelen voor consistentie in de cloud) voor aanbevelingen voor het maken van Resource Manager-sjablonen die u kunt gebruiken in Azure, in onafhankelijke Azure-clouds en in Azure Stack.

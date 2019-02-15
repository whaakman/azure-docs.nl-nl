---
title: Sjabloonstructuur van Azure Resource Manager-en de syntaxis | Microsoft Docs
description: Beschrijft de structuur en de eigenschappen van Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2019
ms.author: tomfitz
ms.openlocfilehash: 8b3d097d2ec3639a829d3c3bb5056b353eef0763
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56300173"
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Informatie over de structuur en de syntaxis van Azure Resource Manager-sjablonen

Dit artikel beschrijft de structuur van een Azure Resource Manager-sjabloon. Deze geeft de verschillende secties van een sjabloon en de eigenschappen die beschikbaar in deze secties zijn. De sjabloon bestaat uit JSON en uitdrukkingen die u gebruiken kunt om waarden voor uw implementatie samen te stellen. Zie voor een stapsgewijze zelfstudie over het maken van een sjabloon, [uw eerste Azure Resource Manager-sjabloon maken](resource-manager-create-first-template.md).

## <a name="template-format"></a>Sjabloonindeling

In de meest eenvoudige structuur heeft een sjabloon voor de volgende elementen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "functions": [  ],
    "resources": [  ],
    "outputs": {  }
}
```

| De naam van element | Vereist | Description |
|:--- |:--- |:--- |
| $schema |Ja |Locatie van het JSON-schema-bestand dat de versie van de taal van de sjabloon beschrijft.<br><br> Voor implementaties van resource-groep, gebruiken: `https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#`<br><br>Gebruik voor abonnementimplementaties van: `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#` |
| contentVersion |Ja |De versie van de sjabloon (bijvoorbeeld 1.0.0.0). U kunt een waarde opgeven voor dit element. Gebruik deze waarde aan aanzienlijke wijzigingen in het document in de sjabloon. Bij het implementeren van resources met behulp van de sjabloon, kan deze waarde kan worden gebruikt om ervoor te zorgen dat de juiste sjabloon wordt gebruikt. |
| parameters |Nee |De waarden die zijn opgegeven wanneer de implementatie wordt uitgevoerd om aan te passen van de resource-implementatie. |
| Variabelen |Nee |De waarden die worden gebruikt als JSON-fragmenten in de sjabloon voor het vereenvoudigen van sjabloontaalexpressies. |
| functions |Nee |Gebruiker gedefinieerde functies die beschikbaar in de sjabloon zijn. |
| bronnen |Ja |De resourcetypen die worden geïmplementeerd of bijgewerkt in een resourcegroep of abonnement. |
| uitvoer |Nee |De waarden die zijn geretourneerd na de implementatie. |

Elk element heeft eigenschappen die u kunt instellen. Het volgende voorbeeld ziet u de volledige syntaxis voor een sjabloon:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
            "condition": "<boolean-value-whether-to-output-value>",
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
* De functie kan de parameters die zijn gedefinieerd in de functie alleen gebruiken. Wanneer u gebruikt de [parameters functie](resource-group-template-functions-deployment.md#parameters) binnen een door de gebruiker gedefinieerde functie, u bent beperkt tot de parameters voor deze functie.
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

<a id="comments" />

## <a name="comments-and-metadata"></a>Opmerkingen en metagegevens

U hebt een aantal opties voor het toevoegen van opmerkingen en metagegevens aan uw sjabloon.

U kunt toevoegen een `metadata` object vrijwel overal in uw sjabloon. Resource Manager worden genegeerd voor het object, maar uw JSON-editor kan waarschuwt u dat de eigenschap is niet geldig. In het object definieert de eigenschappen die u nodig hebt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "metadata": {
        "comments": "This template was developed for demonstration purposes.",
        "author": "Example Name"
    },
```

Voor **parameters**, Voeg een `metadata` object met een `description` eigenschap.

```json
"parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "User name for the Virtual Machine."
      }
    },
```

Bij het implementeren van de sjabloon die via de portal, wordt de tekst die u in de beschrijving opgeeft automatisch gebruikt als een tip voor deze parameter.

![Tip van de parameter weergeven](./media/resource-group-authoring-templates/show-parameter-tip.png)

Voor **resources**, Voeg een `comments` element of een metagegevensobject. Het volgende voorbeeld ziet zowel een element opmerkingen en een metagegevensobject.

```json
"resources": [
  {
    "comments": "Storage account used to store VM disks",
    "apiVersion": "2018-07-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[concat('storage', uniqueString(resourceGroup().id))]",
    "location": "[parameters('location')]",
    "metadata": {
      "comments": "These tags are needed for policy compliance."
    },
    "tags": {
      "Dept": "[parameters('deptName')]",
      "Environment": "[parameters('environment')]"
    },
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

Voor **levert**, een metagegevensobject toevoegen aan de uitvoerwaarde.

```json
"outputs": {
    "hostname": {
      "type": "string",
      "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]",
      "metadata": {
        "comments": "Return the fully qualified domain name"
      }
    },
```

U kunt een metagegevensobject niet toevoegen aan de gebruiker gedefinieerde functies.

Voor Inlineopmerkingen, kunt u `//` , maar deze syntaxis werkt niet met alle hulpprogramma's. U kunt Azure CLI niet gebruiken om de sjabloon met inlineopmerkingen te implementeren. En u niet de sjabloon-editor gebruiken voor het werken met sjablonen met inlineopmerkingen. Als u de stijl van de opmerking toevoegt, moet de hulpprogramma's waarmee u Inlineopmerkingen van ondersteuning voor JSON.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[parameters('location')]", //defaults to resource group location
  "apiVersion": "2018-10-01",
  "dependsOn": [ // storage account and network interface must be deployed first
      "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
      "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

In VS Code, kunt u de taalmodus instellen naar JSON met opmerkingen. De Inlineopmerkingen zijn niet langer als ongeldig gemarkeerd. De modus te wijzigen:

1. Open modus taalselectie (Ctrl + K min.)

1. Selecteer **JSON met opmerkingen**.

   ![Taalmodus selecteren](./media/resource-group-authoring-templates/select-json-comments.png)

## <a name="template-limits"></a>Limieten voor sjabloon

Beperkt de grootte van uw sjabloon 1 MB en elk parameterbestand aan 64 KB. De limiet van 1 MB is van toepassing op de laatste status van de sjabloon nadat deze is uitgebreid met iteratieve resourcedefinities en waarden voor parameters en variabelen. 

U bent ook beperkt tot:

* 256-parameters
* 256 variabelen
* 800 bronnen (zoals aantal kopieën)
* 64 uitvoerwaarden
* 24.576 tekens in een sjabloonexpressie voor een

U kunt sommige limieten sjabloon met behulp van een geneste sjabloon overschrijdt. Zie voor meer informatie, [gekoppelde sjablonen gebruiken bij het implementeren van Azure-resources](resource-group-linked-templates.md). Als u wilt verkleinen het aantal parameters, variabelen of uitvoer, kunt u verschillende waarden combineren in een object. Zie voor meer informatie, [objecten als parameters](resource-manager-objects-as-parameters.md).

[!INCLUDE [arm-tutorials-quickstarts](../../includes/resource-manager-tutorials-quickstarts.md)]

## <a name="next-steps"></a>Volgende stappen
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Zie voor meer informatie over de functies die u uit in een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Als u wilt combineren verschillende sjablonen tijdens de implementatie, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Zie voor aanbevelingen over het maken van sjablonen, [aanbevolen procedures voor Azure Resource Manager-sjabloon](template-best-practices.md).
* Zie voor aanbevelingen voor het maken van Resource Manager-sjablonen die u voor alle Azure-omgevingen en Azure Stack gebruiken kunt [ontwikkelen van Azure Resource Manager-sjablonen voor de consistentie van de cloud](templates-cloud-consistency.md).

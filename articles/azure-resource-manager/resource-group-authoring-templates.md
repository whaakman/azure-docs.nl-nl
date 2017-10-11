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
ms.date: 06/14/2017
ms.author: tomfitz
ms.openlocfilehash: dc9b64062d7f68c83aa090eec96744819a5ca423
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Overzicht van de structuur en de syntaxis van Azure Resource Manager-sjablonen
Dit onderwerp beschrijft de structuur van een Azure Resource Manager-sjabloon. Dit geeft de verschillende secties van een sjabloon en de eigenschappen die beschikbaar in deze secties zijn. De sjabloon bestaat uit JSON en uitdrukkingen die u gebruiken kunt om waarden voor uw implementatie samen te stellen. Zie voor een stapsgewijze zelfstudie over het maken van een sjabloon, [maken van uw eerste Azure Resource Manager-sjabloon](resource-manager-create-first-template.md).

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
| Parameters |Nee |De waarden die beschikbaar zijn wanneer de implementatie wordt uitgevoerd voor het aanpassen van de resource-implementatie. |
| variabelen |Nee |De waarden die worden gebruikt als JSON-fragmenten in de sjabloon voor sjabloontaalexpressies vereenvoudigen. |
| Resources |Ja |Brontypen die worden geïmplementeerd of bijgewerkt in een resourcegroep. |
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
        "<variable-name>": { 
            <variable-complex-type-value> 
        }
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

We onderzoeken in de secties van de sjabloon in meer detail verderop in dit onderwerp.

## <a name="expressions-and-functions"></a>Expressies en functies
De syntaxis van de basis van de sjabloon is JSON. Expressies en functies echter uitbreiden de JSON-waarden die beschikbaar zijn in de sjabloon.  Expressies zijn geschreven in JSON-letterlijke waarvan de eerste en laatste tekens zijn de vierkante haken: `[` en `]`respectievelijk. De waarde van de expressie wordt geëvalueerd wanneer de sjabloon wordt geïmplementeerd. Hoewel geschreven als een letterlijke tekenreeks, zijn het resultaat van evaluatie van de expressie van een ander JSON-type, zoals een matrix of een geheel getal, afhankelijk van de werkelijke expressie.  Een letterlijke tekenreeks beginnen met een haakje hebben `[`, maar dit wordt geïnterpreteerd als een expressie niet, Voeg een extra haakje voor het starten van de tekenreeks met `[[`.

Normaal gesproken u expressies gebruiken met functies bewerkingen voor het configureren van de implementatie uit te voeren. Net zoals in JavaScript-functieaanroepen die zijn opgemaakt als `functionName(arg1,arg2,arg3)`. U verwijzen naar eigenschappen met behulp van de punt en [index] operators.

Het volgende voorbeeld ziet u hoe u verschillende functies gebruikt bij het maken van de waarden:

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

Zie voor een volledige lijst van sjabloonfuncties [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md). 

## <a name="parameters"></a>Parameters
U opgeven welke waarden u invoeren kunt bij het implementeren van de resources in het gedeelte parameters van de sjabloon. De parameterwaarden van deze kunnen u de implementatie aanpassen door het verstrekken van waarden die zijn aangepast voor een bepaalde omgeving (zoals ontwikkelen, testen en productie). U hoeft niet te bieden parameters in de sjabloon, maar zonder parameters uw sjabloon altijd dezelfde resources met dezelfde namen, locaties en eigenschappen zou implementeren.

U definieert parameters met de volgende structuur:

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

| Elementnaam | Vereist | Beschrijving |
|:--- |:--- |:--- |
| parameterName |Ja |De naam van de parameter. Moet een geldige JavaScript-id. |
| type |Ja |Type van de waarde van parameter. Zie de lijst met toegestane typen na deze tabel. |
| Standaardwaarde |Nee |De standaardwaarde voor de parameter als u geen waarde is opgegeven voor de parameter. |
| allowedValues |Nee |Matrix van toegestane waarden voor de parameter om ervoor te zorgen dat de juiste waarde is opgegeven. |
| MinValue |Nee |De minimumwaarde voor de parameters van het type int, deze waarde is liggen. |
| MaxValue |Nee |De maximale waarde voor de parameters van het type int, deze waarde is liggen. |
| minLength |Nee |De minimale lengte voor string, secureString en array typeparameters deze waarde is liggen. |
| maxLength |Nee |De maximale lengte voor string, secureString en array typeparameters deze waarde is liggen. |
| Beschrijving |Nee |Beschrijving van de parameter die wordt weergegeven voor gebruikers via de portal. |

De toegestane typen en waarden zijn:

* **tekenreeks**
* **secureString**
* **int**
* **BOOL**
* **object** 
* **secureObject**
* **matrix**

Als u een parameter als een optionele, bieden een defaultValue (kunnen geen lege tekenreeks zijn). 

Als u een parameternaam in de sjabloon die overeenkomt met een parameter in de opdracht om de sjabloon te implementeren opgeeft, is het mogelijke verwarring over de waarden die u opgeeft. Resource Manager wordt deze verwarring opgelost doordat de postfix **FromTemplate** voor de sjabloonparameter. Bijvoorbeeld, als u een parameter genaamd opnemen **ResourceGroupName** in uw sjabloon een conflict met de **ResourceGroupName** parameter in de [New-AzureRmResourceGroupDeployment ](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet. Tijdens de implementatie, moet u een waarde opgeven voor **ResourceGroupNameFromTemplate**. In het algemeen moet u deze verwarring niet door de naam geen parameters met dezelfde naam als parameters die worden gebruikt voor implementatiebewerkingen.

> [!NOTE]
> Alle wachtwoorden, sleutels en andere geheime informatie moeten gebruiken de **secureString** type. Als u gevoelige gegevens in een JSON-object doorgeven, gebruikt u de **secureObject** type. Sjabloonparameters met secureString of secureObject typen kunnen niet worden gelezen na de implementatie van de resource. 
> 
> De volgende vermelding in de implementatiegeschiedenis van de ziet u bijvoorbeeld de waarde voor een tekenreeks en object maar niet voor secureString en secureObject.
>
> ![waarden van de implementatie weergeven](./media/resource-group-authoring-templates/show-parameters.png)  
>

Het volgende voorbeeld ziet u hoe parameters definiëren:

```json
"parameters": {
    "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
    },
    "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
    },
    "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
    },
    "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
    }
}
```

Voor het invoeren van de parameterwaarden die tijdens de implementatie, Zie [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md). 

## <a name="variables"></a>Variabelen
In het gedeelte variabelen kunt u waarden die kunnen worden gebruikt in uw sjabloon opstellen. U hoeft geen variabelen definiëren, maar ze vaak uw sjabloon vereenvoudigen doordat complexe expressies.

U definieert variabelen met de volgende structuur:

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

Het volgende voorbeeld ziet u hoe een variabele die is samengesteld uit twee parameterwaarden definiëren:

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

Het volgende voorbeeld ziet u een variabele die is complex type JSON en variabelen die zijn samengesteld uit andere variabelen:

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
"variables": {
    "environmentSettings": {
        "test": {
            "instancesSize": "Small",
            "instancesCount": 1
        },
        "prod": {
            "instancesSize": "Large",
            "instancesCount": 4
        }
    },
    "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
    "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
    "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
}
```

## <a name="resources"></a>Resources
In de bronnensectie definieert u de resources die worden geïmplementeerd of bijgewerkt. Deze sectie kunt krijgen ingewikkeld omdat de typen die u implementeert de juiste waarden opgeven dat u begrijpt. Zie voor de resource-specifieke waarden (apiVersion, type en eigenschappen) die u nodig hebt om in te stellen [resources in Azure Resource Manager-sjablonen definiëren](/azure/templates/). 

U definieert resources met de volgende structuur:

```json
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
]
```

| Elementnaam | Vereist | Beschrijving |
|:--- |:--- |:--- |
| Voorwaarde | Nee | Booleaanse waarde die aangeeft of de resource is geïmplementeerd. |
| apiVersion |Ja |De versie van de REST-API gebruiken voor het maken van de resource. |
| type |Ja |Type van de resource. Deze waarde is een combinatie van de naamruimte van de resourceprovider en het resourcetype (zoals **Microsoft.Storage/storageAccounts**). |
| naam |Ja |De naam van de resource. De naam moet URI onderdeel beperkingen gedefinieerd in RFC3986 volgen. Bovendien Azure-services die beschikbaar om te valideren buiten partijen de naam om te controleren of deze de naam van de resource is niet een poging tot een andere identiteit vervalsen. |
| location |Varieert |Ondersteunde geografische locaties van de opgegeven bron. U kunt een van de beschikbare locaties selecteren, maar meestal is het verstandig om te selecteren die dicht bij uw gebruikers. Meestal is het ook handig om resources die in dezelfde regio met elkaar communiceren. De meeste brontypen die een locatie vereist, maar sommige typen (zoals een roltoewijzing) hoeven niet een locatie. Zie [Resourcelocatie instellen in Azure Resource Manager-sjablonen](resource-manager-template-location.md). |
| tags |Nee |Labels die gekoppeld aan de resource zijn. Zie [resources in Azure Resource Manager-sjablonen taggen](resource-manager-template-tags.md). |
| Opmerkingen |Nee |De notities voor de resources in uw sjabloon documenteren |
| Kopiëren |Nee |Als meer dan één exemplaar is vereist, het aantal resources om te maken. Er is de standaardmodus voor parallelle. Seriële modus wanneer u niet dat alle wilt of de resources te implementeren op hetzelfde moment opgeven. Zie voor meer informatie [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nee |Resources die moeten worden geïmplementeerd voordat u deze bron wordt geïmplementeerd. Resource Manager evalueert de afhankelijkheden tussen resources en ze worden geïmplementeerd in de juiste volgorde. Wanneer u resources zijn niet afhankelijk van elkaar, worden ze geïmplementeerd parallel. De waarde kan een door komma's gescheiden lijst van een resource zijn namen of unieke id's voor een resource. Alleen de lijst van resources die zijn geïmplementeerd in deze sjabloon. Bronnen die niet in deze sjabloon zijn gedefinieerd, moeten al bestaan. Vermijd toe te voegen onnodige afhankelijkheden als ze kunnen uw implementatie vertragen en circulaire afhankelijkheden maken. Zie voor instructies over de afhankelijkheden van de instelling [afhankelijkheden definiëren in Azure Resource Manager-sjablonen](resource-group-define-dependencies.md). |
| properties |Nee |Resource-specifieke configuratie-instellingen. De waarden voor de eigenschappen zijn hetzelfde als de waarden die u in de aanvraagtekst voor de REST-API-bewerking (PUT-methode opgeeft) om de resource te maken. U kunt ook een matrix kopiëren voor het maken van meerdere exemplaren van een eigenschap opgeven. Zie voor meer informatie [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md). |
| Resources |Nee |Onderliggende resources die afhankelijk zijn van de bron wordt gedefinieerd. Geef alleen brontypen die worden toegestaan door het schema van de bovenliggende resource. Het volledig gekwalificeerde type van de onderliggende resource bevat het type van de bovenliggende resource, zoals **Microsoft.Web/sites/extensions**. Afhankelijkheid van de bovenliggende resource niet geïmpliceerd. U moet deze afhankelijkheid expliciet definiëren. |

De sectie resources bevat een matrix van de resources te implementeren. U kunt ook een matrix van onderliggende resources binnen elke resource definiëren. Het gedeelte van uw resources kan daarom een structuur zoals hebben:

```json
"resources": [
  {
      "name": "resourceA",
  },
  {
      "name": "resourceB",
      "resources": [
        {
            "name": "firstChildResourceB",
        },
        {   
            "name": "secondChildResourceB",
        }
      ]
  },
  {
      "name": "resourceC",
  }
]
```      

Zie voor meer informatie over het definiëren van de onderliggende resources [naam en type voor onderliggende resource in het Resource Manager-sjabloon instellen](resource-manager-template-child-resource.md).

De **voorwaarde** element geeft aan of de resource is geïmplementeerd. De waarde voor dit element wordt omgezet in true of false. Bijvoorbeeld: als u wilt opgeven of een nieuw opslagaccount wordt geïmplementeerd, gebruiken:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Zie voor een voorbeeld van het gebruik van een nieuwe of bestaande resourcegroep [nieuwe of bestaande voorwaarde sjabloon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

Als u wilt opgeven of een virtuele machine wordt geïmplementeerd met een wachtwoord of SSH-sleutel, definieert u twee versies van de virtuele machine in de sjabloon en het gebruik **voorwaarde** te onderscheiden van informatie over het gebruik. Hiermee geeft u een parameter die welke scenario aangeeft te implementeren.

```json
{
    "condition": "[equals(parameters('passwordOrSshKey'),'password')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'password')]",
    "properties": {
        "osProfile": {
            "computerName": "[variables('vmName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
        },
        ...
    },
    ...
},
{
    "condition": "[equals(parameters('passwordOrSshKey'),'sshKey')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'ssh')]",
    "properties": {
        "osProfile": {
            "linuxConfiguration": {
                "disablePasswordAuthentication": "true",
                "ssh": {
                    "publicKeys": [
                        {
                            "path": "[variables('sshKeyPath')]",
                            "keyData": "[parameters('adminSshKey')]"
                        }
                    ]
                }
            }
        },
        ...
    },
    ...
}
``` 

Zie voor een voorbeeld van het gebruik van een wachtwoord of SSH-sleutel voor het implementeren van virtuele machine [gebruikersnaam of SSH voorwaarde sjabloon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="outputs"></a>uitvoer
In de sectie uitvoer geeft u de waarden die worden geretourneerd van de implementatie. U kan bijvoorbeeld de URI voor toegang tot een geïmplementeerde resource geretourneerd.

Het volgende voorbeeld ziet u de structuur van de definitie van een uitvoer:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Elementnaam | Vereist | Beschrijving |
|:--- |:--- |:--- |
| outputName |Ja |De naam van de uitvoerwaarde. Moet een geldige JavaScript-id. |
| type |Ja |Type van de uitvoerwaarde. Uitvoerwaarden ondersteuning voor dezelfde typen als sjabloon invoerparameters. |
| waarde |Ja |De sjabloontaalexpressie dat wordt geëvalueerd en geretourneerd als de waarde voor uitvoer. |

Het volgende voorbeeld ziet een waarde die wordt geretourneerd in de sectie uitvoer.

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

Zie voor meer informatie over het werken met uitvoer [delen status in Azure Resource Manager-sjablonen](best-practices-resource-manager-state.md).

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

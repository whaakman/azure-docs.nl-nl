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
ms.date: 03/04/2019
ms.author: tomfitz
ms.openlocfilehash: f67741417c6d31c4adf1d063aac3bd3ccc310fde
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57440247"
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Informatie over de structuur en de syntaxis van Azure Resource Manager-sjablonen

Dit artikel beschrijft de structuur van een Azure Resource Manager-sjabloon. Deze geeft de verschillende secties van een sjabloon en de eigenschappen die beschikbaar in deze secties zijn. De sjabloon bestaat uit JSON en uitdrukkingen die u gebruiken kunt om waarden voor uw implementatie samen te stellen.

In dit artikel is bedoeld voor gebruikers die bekend zijn met Resource Manager-sjablonen. Het biedt gedetailleerde informatie over de structuur en de syntaxis van de sjabloon. Als u een inleiding wilt tot het maken van een sjabloon, Zie [uw eerste Azure Resource Manager-sjabloon maken](resource-manager-create-first-template.md).

## <a name="template-format"></a>Sjabloonindeling

In de meest eenvoudige structuur heeft een sjabloon voor de volgende elementen:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "",
  "apiProfile": "",
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
| apiProfile |Nee | Een API-versie die als een verzameling van API-versies voor het woord brontypen fungeert. Gebruik deze waarde om te voorkomen dat om op te geven van API-versies voor elke resource in de sjabloon. Wanneer u de versie van een API-profiel opgeven en een API-versie voor het resourcetype niet opgeeft, wordt de API-versie van het profiel in Resource Manager gebruikt voor dat resourcetype. Zie voor meer informatie, [versies met behulp van API-profielen bijhouden](templates-cloud-consistency.md#track-versions-using-api-profiles). |
| [parameters](#parameters) |Nee |De waarden die zijn opgegeven wanneer de implementatie wordt uitgevoerd om aan te passen van de resource-implementatie. |
| [Variabelen](#variables) |Nee |De waarden die worden gebruikt als JSON-fragmenten in de sjabloon voor het vereenvoudigen van sjabloontaalexpressies. |
| [Functies](#functions) |Nee |Gebruiker gedefinieerde functies die beschikbaar in de sjabloon zijn. |
| [resources](#resources) |Ja |De resourcetypen die worden geïmplementeerd of bijgewerkt in een resourcegroep of abonnement. |
| [uitvoer](#outputs) |Nee |De waarden die zijn geretourneerd na de implementatie. |

Elk element heeft eigenschappen die u kunt instellen. Dit artikel wordt beschreven in de secties van de sjabloon in meer detail.

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

U bent beperkt tot 256 parameters in een sjabloon. U kunt het aantal parameters verminderen door middel van objecten die meerdere eigenschappen bevat, zoals weergegeven in dit artikel.

### <a name="available-properties"></a>Beschikbare eigenschappen

De beschikbare eigenschappen voor een parameter zijn:

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
| minValue |Nee |De minimumwaarde voor de parameters van het type int, deze waarde wordt ook meegerekend. |
| maxValue |Nee |De maximale waarde voor de parameters van het type int, deze waarde wordt ook meegerekend. |
| minLength |Nee |De minimale lengte van tekenreeks, een beveiligde tekenreeks en parameters van het type matrix, deze waarde wordt ook meegerekend. |
| maxLength |Nee |De maximale lengte voor string, beveiligde tekenreeks en parameters van het type matrix, deze waarde wordt ook meegerekend. |
| description |Nee |Beschrijving van de parameter die wordt weergegeven voor gebruikers via de portal. Zie voor meer informatie, [opmerkingen in sjablonen](#comments). |

### <a name="define-and-use-a-parameter"></a>Definiëren en gebruiken van een parameter

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

### <a name="template-functions-with-parameters"></a>Sjabloonfuncties met parameters

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

### <a name="objects-as-parameters"></a>Objecten als parameters

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

### <a name="parameter-example-templates"></a>Voorbeeldsjablonen van parameter

Deze voorbeeldsjablonen laten zien voor sommige scenario's voor het gebruik van parameters. Te testen hoe parameters worden verwerkt in verschillende scenario's implementeren.

|Template  |Description  |
|---------|---------|
|[parameters met functies voor standaardwaarden](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Demonstreert hoe u sjabloonfuncties gebruiken bij het definiëren van de standaardwaarden voor parameters. De sjabloon implementeren niet alle resources. Deze parameterwaarden constructs en die waarden retourneert. |
|[Parameter-object](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Ziet u met behulp van een object voor een parameter. De sjabloon implementeren niet alle resources. Deze parameterwaarden constructs en die waarden retourneert. |

## <a name="variables"></a>Variabelen

In het gedeelte variabelen kunt u waarden die kunnen worden gebruikt in uw sjabloon maken. U hoeft niet te definiëren, variabelen, maar ze vaak de sjabloon vereenvoudigen door te verminderen van complexe expressies.

### <a name="available-definitions"></a>Beschikbare definities

Het volgende voorbeeld ziet u de beschikbare opties voor het definiëren van een variabele:

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

Voor informatie over het gebruik van `copy` voor het maken van verschillende waarden voor een variabele, Zie [variabele iteratie](resource-group-create-multiple.md#variable-iteration).

### <a name="define-and-use-a-variable"></a>Definiëren en een variabele te gebruiken

Het volgende voorbeeld ziet de definitie van een variabele. Hiermee maakt u een string-waarde voor de naam van een opslagaccount. Het maakt gebruik van verschillende sjabloonfuncties een parameterwaarde op te halen en tot een unieke tekenreeks worden samengevoegd.

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

### <a name="configuration-variables"></a>Configuratievariabelen voor de

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

### <a name="variable-example-templates"></a>Van de variabele voorbeeldsjablonen

Deze voorbeeldsjablonen laten zien voor sommige scenario's voor het gebruik van variabelen. Te testen hoe variabelen worden verwerkt in verschillende scenario's implementeren. 

|Template  |Description  |
|---------|---------|
| [definities van variabele](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Ziet u de verschillende typen variabelen. De sjabloon implementeren niet alle resources. Deze waarden van variabelen constructs en die waarden retourneert. |
| [van configuratievariabele](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Ziet u het gebruik van een variabele die de configuratiewaarden definieert. De sjabloon implementeren niet alle resources. Deze waarden van variabelen constructs en die waarden retourneert. |
| [netwerkbeveiligingsregels](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) en [parameterbestand](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Vormt het een matrix in de juiste indeling voor het beveiligingsregels toewijzen aan een netwerkbeveiligingsgroep. |


## <a name="functions"></a>Functions

U kunt uw eigen functies maken in uw sjabloon. Deze functies zijn beschikbaar voor gebruik in uw sjabloon. Normaal gesproken definieert u een complexe expressie die u niet wilt herhalen in uw sjabloon. U maakt de gebruiker gedefinieerde functies van expressies en [functies](resource-group-template-functions.md) die worden ondersteund in de sjablonen.

Bij het definiëren van de functie van een gebruiker, zijn er enkele beperkingen:

* De functie heeft geen toegang tot variabelen.
* De functie kan de parameters die zijn gedefinieerd in de functie alleen gebruiken. Wanneer u gebruikt de [parameters functie](resource-group-template-functions-deployment.md#parameters) binnen een door de gebruiker gedefinieerde functie, bent u beperkt tot de parameters voor deze functie.
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
In de sectie resources definieert u de resources die worden geïmplementeerd of bijgewerkt.

### <a name="available-properties"></a>Beschikbare eigenschappen

Definieert u resources met de volgende structuur:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
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
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
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
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| De naam van element | Vereist | Description |
|:--- |:--- |:--- |
| voorwaarde | Nee | Booleaanse waarde die aangeeft of de resource tijdens deze implementatie worden ingericht. Wanneer `true`, de resource is gemaakt tijdens de implementatie. Wanneer `false`, de bron voor deze implementatie wordt overgeslagen. |
| apiVersion |Ja |De versie van de REST-API moet worden gebruikt voor het maken van de resource. Zie het vaststellen van de beschikbare waarden [sjabloonverwijzing](/azure/templates/). |
| type |Ja |Het type van de resource. Deze waarde is een combinatie van de naamruimte van de resourceprovider en het resourcetype (zoals **Microsoft.Storage/storageAccounts**). Zie het vaststellen van de beschikbare waarden [sjabloonverwijzing](/azure/templates/). |
| naam |Ja |Naam van de resource De naam moet URI-onderdeel beperkingen gedefinieerd in RFC3986 volgen. Azure-services die beschikbaar maken van de naam van de resource buiten partijen valideren de naam om te controleren of het is bovendien een poging tot het vervalsen van een andere identiteit niet. |
| location |Varieert |Geografische locaties van de opgegeven resource wordt ondersteund. U kunt een van de beschikbare locaties selecteren, maar meestal is het zinvol om te kiezen die zich in de buurt van uw gebruikers. Meestal is het ook verstandig om de resources die met elkaar in dezelfde regio communiceren te plaatsen. De meeste resourcetypen een locatie vereist, maar sommige typen (zoals een roltoewijzing) vereisen een locatie. |
| tags |Nee |Tags die gekoppeld aan de resource zijn. Labels toevoegen om in te delen logisch resources in uw abonnement. |
| opmerkingen |Nee |Uw notities voor het documenteren van de resources in uw sjabloon. Zie voor meer informatie, [opmerkingen in sjablonen](resource-group-authoring-templates.md#comments). |
| kopiëren |Nee |Als meer dan één exemplaar is vereist, het aantal resources om te maken. Er is de standaardmodus voor parallelle. Seriële modus wanneer u niet dat alle wilt of de resources om te implementeren op hetzelfde moment opgeven. Zie voor meer informatie, [verschillende exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nee |Resources die moeten worden geïmplementeerd voordat deze resource is geïmplementeerd. Resource Manager evalueert de afhankelijkheden tussen resources en ze implementeert in de juiste volgorde. Als resources niet van elkaar afhankelijk zijn, zijn ze parallel geïmplementeerd. De waarde kan een door komma's gescheiden lijst van een resource zijn namen of resource-id's uniek. Alleen lijst met resources die in deze sjabloon zijn geïmplementeerd. Resources die niet zijn gedefinieerd in deze sjabloon moeten al bestaan. Vermijd onnodige afhankelijkheden toevoegen als ze kunnen uw implementatie vertragen en circulaire afhankelijkheden maken. Zie voor meer informatie over de afhankelijkheden van de instelling [afhankelijkheden definiëren in Azure Resource Manager-sjablonen](resource-group-define-dependencies.md). |
| properties |Nee |Resource-specifieke configuratie-instellingen. De waarden voor de eigenschappen zijn hetzelfde als de waarden die u in de hoofdtekst van de aanvraag voor de REST-API-bewerking (PUT-methode opgeeft) om de resource te maken. U kunt ook een matrix kopiëren voor het maken van meerdere exemplaren van een eigenschap opgeven. Zie het vaststellen van de beschikbare waarden [sjabloonverwijzing](/azure/templates/). |
| sku | Nee | Sommige resources zijn waarden toegestaan die definiëren van de SKU om het te implementeren. Bijvoorbeeld, kunt u het type redundantie voor een opslagaccount. |
| type | Nee | Sommige resources kunnen een waarde die bepaalt het type resource dat u implementeert. Bijvoorbeeld, kunt u het type van de Cosmos DB te maken. |
| plan | Nee | Sommige resources zijn waarden toegestaan die in de planning definiëren wilt implementeren. U kunt bijvoorbeeld opgeven dat de marketplace-installatiekopie voor een virtuele machine. | 
| bronnen |Nee |Onderliggende resources die afhankelijk zijn van de resource wordt gedefinieerd. Geef alleen resourcetypen die zijn toegestaan door het schema van de bovenliggende resource. De volledig gekwalificeerde type van de onderliggende resource bevat het type van de bovenliggende resource, zoals **Microsoft.Web/sites/extensions**. Afhankelijkheid van de bovenliggende resource is niet impliciet. Afhankelijkheid zijn opgetreden, moet u expliciet definiëren. |

### <a name="condition"></a>Voorwaarde

Wanneer u tijdens de implementatie van al dan niet om een resource te maken beslissen moet, gebruikt u de `condition` element. De waarde voor dit element wordt omgezet in waar of ONWAAR. Wanneer de waarde true is, wordt de resource is gemaakt. Wanneer de waarde false is, wordt de resource is niet gemaakt. De waarde kan alleen worden toegepast op de hele resource.

Meestal gebruikt u deze waarde als u wilt maken van een nieuwe resource of gebruik een bestaande resourcegroep. Bijvoorbeeld, om op te geven of een nieuw opslagaccount wordt geïmplementeerd of een bestaand opslagaccount wordt gebruikt, gebruikt u het:

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

Voor een compleet voorbeeld-sjabloon die gebruikmaakt van de `condition` -element, Zie [VM met een nieuwe of bestaande Virtueelnetwerk, opslag- en openbare IP-adres](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

### <a name="resource-names"></a>Namen van voorbeeldresources

In het algemeen, werkt u met drie typen resourcenamen in Resource Manager:

* Namen van voorbeeldresources die moeten uniek zijn.
* Namen van voorbeeldresources die niet zijn vereist voor het uniek zijn, maar u kiest een naam waarmee u identificeren van de resource op te geven.
* Namen van voorbeeldresources die algemene worden kunnen.

Geef een **unieke resourcenaam** voor elk resourcetype dat een eindpunt van de toegang tot gegevens heeft. Sommige algemene resourcetypen waarvoor een unieke naam zijn onder andere:

* Azure Storage<sup>1</sup> 
* Web Apps-functie van Azure App Service
* SQL Server
* Azure Key Vault
* Azure Cache voor Redis
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> opslagaccountnamen ook moet een kleine letter, 24 tekens of korter is, en niet alle afbreekstreepjes bevatten.

Bij het instellen van de naam, u kunt handmatig maken van een unieke naam of gebruik de [uniqueString()](resource-group-template-functions-string.md#uniquestring) functie voor het genereren van een naam. Ook kunt u voegt u een voorvoegsel of achtervoegsel aan de **uniqueString** resultaat. De unieke naam wijzigen, kunt u eenvoudig kunt identificeren het brontype van de naam. U kunt bijvoorbeeld een unieke naam voor een opslagaccount genereren met behulp van de volgende variabele:

```json
"variables": {
  "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

Voor sommige resourcetypen, kunt u voor een **naam voor de identificatie**, maar de naam hoeft niet uniek te zijn. Geef een naam met een beschrijving ervan gebruik of de kenmerken voor deze resourcetypen.

```json
"parameters": {
  "vmName": { 
    "type": "string",
    "defaultValue": "demoLinuxVM",
    "metadata": {
      "description": "The name of the VM to create."
    }
  }
}
```

Voor resource die u voornamelijk toegang via een andere resource gegevenstypen, kunt u een **algemene naam** die is vastgelegd in de sjabloon. U kunt bijvoorbeeld een algemene naam voor de firewall-regels instellen op een SQL server:

```json
{
  "type": "firewallrules",
  "name": "AllowAllWindowsAzureIps",
  ...
}
```

### <a name="resource-location"></a>Resourcelocatie

Bij het implementeren van een sjabloon, moet u een locatie voor elke resource opgeven. Andere resource-typen worden ondersteund op verschillende locaties. Als u de ondersteunde locaties voor een resourcetype, Zie [Azure resourceproviders en typen](resource-manager-supported-services.md).

Gebruik een parameter om op te geven van de locatie voor resources en de standaardwaarde ingesteld op `resourceGroup().location`.

Het volgende voorbeeld ziet u een opslagaccount dat wordt geïmplementeerd op een locatie die is opgegeven als parameter:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('storage', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "apiVersion": "2018-07-01",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

### <a name="child-resources"></a>Onderliggende resources

In sommige resourcetypen, kunt u ook een reeks onderliggende resources definiëren. Onderliggende resources zijn resources die alleen zijn opgeslagen in de context van een andere resource. Een SQL-database niet kan bijvoorbeeld bestaan zonder een SQL-server, zodat de database een onderliggend element van de server is. U kunt de database in de definitie voor de server definiëren.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Wanneer genest, het type is ingesteld op `databases` , maar het type volledige resource `Microsoft.Sql/servers/databases`. U geen opgeeft `Microsoft.Sql/servers/` omdat ervan wordt uitgegaan van het type van de bovenliggende resource. De naam van de onderliggende bron is ingesteld op `exampledatabase` , maar de volledige naam bevat de naam van de bovenliggende. U geen opgeeft `exampleserver` omdat ervan wordt uitgegaan van de bovenliggende resource.

De indeling van het type van de onderliggende bron is: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

De indeling van de naam van de onderliggende bron is: `{parent-resource-name}/{child-resource-name}`

Maar u hoeft de database in de server. U kunt de onderliggende resource op het hoogste niveau definiëren. U kunt deze methode gebruiken als de bovenliggende resource is niet geïmplementeerd in dezelfde sjabloon of wilt gebruiken `copy` om meer dan één onderliggende resource te maken. Met deze methode moet u het type volledige resource en omvatten de naam van de bovenliggende resource in de naam van de onderliggende resource.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Bij het maken van een volledig gekwalificeerde verwijzing naar een resource, is de volgorde te combineren segmenten van het type en de naam niet gewoon een samenvoeging van de twee. In plaats daarvan nadat de naamruimte van het gebruik van een reeks *typenaam/* paren van minst specifiek voor meest specifieke:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Bijvoorbeeld:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` klopt `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` is niet correct

## <a name="outputs"></a>Uitvoer

In de sectie uitvoer geeft u waarden die zijn geretourneerd na de implementatie. Normaal gesproken retourneren u waarden van resources die zijn geïmplementeerd.

### <a name="available-properties"></a>Beschikbare eigenschappen

Het volgende voorbeeld ziet u de structuur van de uitvoerdefinitie van een:

```json
"outputs": {
  "<outputName>" : {
    "condition": "<boolean-value-whether-to-output-value>",
    "type" : "<type-of-output-value>",
    "value": "<output-value-expression>"
  }
}
```

| De naam van element | Vereist | Description |
|:--- |:--- |:--- |
| outputName |Ja |De naam van de uitvoerwaarde. Moet een geldige JavaScript-id. |
| voorwaarde |Nee | Booleaanse waarde die aangeeft of deze uitvoer waarde wordt geretourneerd. Wanneer `true`, de waarde is opgenomen in de uitvoer voor de implementatie. Wanneer `false`, de uitvoerwaarde wordt overgeslagen voor deze implementatie. Als niet is opgegeven, is de standaardwaarde `true`. |
| type |Ja |Type van de uitvoerwaarde. Uitvoerwaarden ondersteuning van de dezelfde typen als sjabloon invoerparameters die zijn opgegeven. |
| waarde |Ja |De sjabloontaalexpressie dat wordt geëvalueerd en geretourneerd als de uitvoerwaarde. |

### <a name="define-and-use-output-values"></a>Definiëren en uitvoerwaarden

Het volgende voorbeeld laat zien hoe geretourneerd van de resource-ID voor een openbaar IP-adres:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Het volgende voorbeeld laat zien hoe voorwaardelijk geretourneerd de resource-ID voor een openbaar IP-adres op basis van of een nieuw een is geïmplementeerd:

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Zie voor een eenvoudig voorbeeld van uitvoer voorwaardelijke [voorwaardelijke uitvoer sjabloon](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json).

Na de implementatie, kunt u de waarde met het script ophalen. Gebruik voor PowerShell:

```powershell
(Get-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -Name <deployment-name>).Outputs.resourceID.value
```

Gebruik voor Azure CLI:

```azurecli-interactive
az group deployment show -g <resource-group-name> -n <deployment-name> --query properties.outputs.resourceID.value
```

U kunt de uitvoerwaarde van een gekoppelde sjabloon ophalen met behulp van de [verwijzing](resource-group-template-functions-resource.md#reference) functie. Als u een uitvoerwaarde op basis van een gekoppelde sjabloon, halen de waarde van de eigenschap met de syntaxis, zoals: `"[reference('deploymentName').outputs.propertyName.value]"`.

Bij het ophalen van een eigenschap van een uitvoer van een gekoppelde sjabloon, kan niet de eigenschapsnaam van de een streepje bestaan.

Het volgende voorbeeld ziet hoe u het IP-adres op een load balancer instellen met het ophalen van een waarde van een gekoppelde sjabloon.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

U kunt geen gebruiken de `reference` functie in de uitvoersectie van een [geneste sjabloon](resource-group-linked-templates.md#link-or-nest-a-template). Als u wilt de waarden voor een geïmplementeerde resource in een geneste sjabloon, uw geneste sjabloon te converteren naar een gekoppelde sjabloon.

### <a name="output-example-templates"></a>Voorbeeldsjablonen van uitvoer

|Template  |Description  |
|---------|---------|
|[Kopieer variabelen](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Complexe variabelen maakt en deze waarden weergeeft. Niet alle resources niet implementeren. |
|[Openbaar IP-adres](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Hiermee maakt u een openbaar IP-adres en de uitvoer van de resource-ID. |
|[Load balancer](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Koppelingen naar de voorgaande sjabloon. Maakt gebruik van de resource-ID in de uitvoer bij het maken van de load balancer. |


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

[!INCLUDE [arm-tutorials-quickstarts](../../includes/resource-manager-tutorials-quickstarts.md)]

## <a name="next-steps"></a>Volgende stappen
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Zie voor meer informatie over de functies die u uit in een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Als u wilt combineren verschillende sjablonen tijdens de implementatie, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Zie voor aanbevelingen over het maken van sjablonen, [aanbevolen procedures voor Azure Resource Manager-sjabloon](template-best-practices.md).
* Zie voor aanbevelingen voor het maken van Resource Manager-sjablonen die u voor alle Azure-omgevingen en Azure Stack gebruiken kunt [ontwikkelen van Azure Resource Manager-sjablonen voor de consistentie van de cloud](templates-cloud-consistency.md).

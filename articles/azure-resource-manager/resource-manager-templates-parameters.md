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
ms.date: 10/30/2018
ms.author: tomfitz
ms.openlocfilehash: 83ba1b94413990c0eb8dff42c49d46456a658d5a
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50417766"
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

| De naam van element | Vereist | Beschrijving |
|:--- |:--- |:--- |
| parameterName |Ja |Naam van de parameter. Moet een geldige JavaScript-id. |
| type |Ja |Het type van de waarde van parameter. De toegestane typen en de waarden zijn **tekenreeks**, **securestring**, **int**, **bool**, **object**, **secureObject**, en **matrix**. |
| defaultValue |Nee |De standaardwaarde voor de parameter, als er geen waarde is opgegeven voor de parameter. |
| allowedValues |Nee |Matrix van toegestane waarden voor de parameter om ervoor te zorgen dat de juiste waarde is opgegeven. |
| Minimumwaarde |Nee |De minimumwaarde voor de parameters van het type int, deze waarde wordt ook meegerekend. |
| Maximumwaarde |Nee |De maximale waarde voor de parameters van het type int, deze waarde wordt ook meegerekend. |
| minLength |Nee |De minimale lengte voor string, securestring en parameters van het type matrix deze waarde wordt ook meegerekend. |
| maxLength |Nee |De maximale lengte voor string, securestring en parameters van het type matrix, deze waarde wordt ook meegerekend. |
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

## <a name="recommendations"></a>Aanbevelingen
De volgende informatie kan nuttig zijn wanneer u met parameters werkt:

* Uw gebruik van parameters minimaliseren. Gebruik indien mogelijk een variabele of een letterlijke waarde. Parameters gebruiken om alleen voor deze scenario's:
   
   * Instellingen die u wilt gebruik van variaties op basis van de omgeving (SKU, grootte, capaciteit).
   * Namen van voorbeeldresources die u wilt opgeven voor de eenvoudige identificatie.
   * De waarden die u vaak gebruikt om andere taken (zoals de naam van een beheerder-gebruiker) te voltooien.
   * Geheimen (zoals wachtwoorden).
   * Het nummer of de matrix met waarden om te gebruiken bij het maken van meer dan één exemplaar van een resourcetype.
* Gebruik kamelen voor namen van parameters.
* Geef een beschrijving van elke parameter in de metagegevens:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* Standaardwaarden voor parameters (met uitzondering van wachtwoorden en SSH-sleutels) definiëren. Door op te geven een standaardwaarde is opgegeven, wordt er tijdens de implementatie van de parameter optioneel. De standaardwaarde mag geen lege tekenreeks zijn. 
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* Gebruik **securestring** voor alle wachtwoorden en geheimen. Als u gevoelige gegevens in een JSON-object doorgeven, gebruikt u de **secureObject** type. Sjabloonparameters met securestring of secureObject typen kunnen niet worden gelezen na de implementatie van de resource. 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Een parameter om op te geven locatie gebruiken en delen die zoveel mogelijk van de parameterwaarde met de resources die kunnen worden op dezelfde locatie. Deze aanpak minimaliseert het aantal keer dat gebruikers wordt gevraagd om locatie-informatie te geven. Als een resourcetype dat wordt ondersteund in een beperkt aantal locaties, is het raadzaam Geef een geldige locatie rechtstreeks in de sjabloon of een andere locatieparameter toevoegen. Wanneer een organisatie de toegestane regio's voor de gebruikers, beperkt de **resourceGroup () .location** expressie mogelijk te voorkomen dat een gebruiker van de sjabloon te implementeren. Een gebruiker wordt bijvoorbeeld een resourcegroep gemaakt in een regio. Een tweede gebruiker in deze resourcegroep moet implementeren, maar heeft geen toegang tot het gebied. 
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[parameters('location')]",
         ...
     }
   ]
   ```
    
* Vermijd het gebruik van een parameter of variabele voor de API-versie voor een resourcetype. Resource-eigenschappen en waarden kunnen variëren per versienummer. IntelliSense in een code-editor kan niet het juiste schema bepalen wanneer de API-versie is ingesteld op een parameter of variabele. In plaats daarvan programmeren de API-versie in de sjabloon.
* Vermijd een parameternaam in de sjabloon die overeenkomt met een parameter in de implementatieopdracht op te geven. Resource Manager deze naamgevingsconventie conflict opgelost doordat de postfix **FromTemplate** aan de sjabloonparameter. Bijvoorbeeld, als u een parameter met de naam opgeeft **ResourceGroupName** in uw sjabloon, deze conflicteert met de **ResourceGroupName** parameter in de [New-AzureRmResourceGroupDeployment ](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet. Tijdens de implementatie van u wordt gevraagd om een waarde voor **ResourceGroupNameFromTemplate**.

## <a name="example-templates"></a>Voorbeeldsjablonen

Deze voorbeeldsjablonen laten zien voor sommige scenario's voor het gebruik van parameters. Te testen hoe parameters worden verwerkt in verschillende scenario's implementeren.

|Template  |Beschrijving  |
|---------|---------|
|[parameters met functies voor standaardwaarden](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Demonstreert hoe u sjabloonfuncties gebruiken bij het definiëren van de standaardwaarden voor parameters. De sjabloon implementeren niet alle resources. Deze parameterwaarden constructs en die waarden retourneert. |
|[Parameter-object](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Ziet u met behulp van een object voor een parameter. De sjabloon implementeren niet alle resources. Deze parameterwaarden constructs en die waarden retourneert. |

## <a name="next-steps"></a>Volgende stappen

* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Voor informatie over het invoeren van de parameterwaarden tijdens de implementatie, Zie [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md). 
* Zie voor meer informatie over de functies die u uit in een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Zie voor meer informatie over het gebruik van een parameter-object [een object gebruiken als een parameter in een Azure Resource Manager-sjabloon](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

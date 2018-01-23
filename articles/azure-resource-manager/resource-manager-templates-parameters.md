---
title: Azure Resource Manager parameter sjabloonsectie | Microsoft Docs
description: Hierin wordt beschreven in de sectie opdrachtregelparameters van Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis.
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
ms.date: 01/19/2018
ms.author: tomfitz
ms.openlocfilehash: 5a519908f43193e41da9237a236d720fe2db58eb
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>Sectie opdrachtregelparameters van Azure Resource Manager-sjablonen
U opgeven welke waarden u invoeren kunt bij het implementeren van de resources in het gedeelte parameters van de sjabloon. De parameterwaarden van deze kunnen u de implementatie aanpassen door het verstrekken van waarden die zijn aangepast voor een bepaalde omgeving (zoals ontwikkelen, testen en productie). U hoeft niet te bieden parameters in de sjabloon, maar zonder parameters uw sjabloon altijd dezelfde resources met dezelfde namen, locaties en eigenschappen zou implementeren.

U bent beperkt tot 255 parameters in een sjabloon. Verminder het aantal parameters kunt u met behulp van de objecten die meerdere eigenschappen als weergeven in dit artikel bevatten.

## <a name="define-and-use-a-parameter"></a>Definieer en het gebruik van parameter

Het volgende voorbeeld ziet een eenvoudige parameterdefinitie. Definieert de naam van de parameter, en geeft aan dat het duurt een string-waarde voordat. De parameter accepteert alleen waarden die geschikt zijn voor het beoogde gebruik. Dit geeft een standaardwaarde als geen waarde is opgegeven tijdens de implementatie. Ten slotte bevat de parameter een beschrijving van het gebruik ervan. 

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

In de sjabloon verwijzen naar de waarde voor de parameter met de volgende syntaxis:

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

Het vorige voorbeeld hebt u geleerd slechts een deel van de eigenschappen die kunt u in de parametersectie. De beschikbare eigenschappen zijn:

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
| type |Ja |Type van de waarde van parameter. De toegestane typen en waarden zijn **tekenreeks**, **secureString**, **int**, **bool**, **object**, **secureObject**, en **matrix**. |
| defaultValue |Nee |De standaardwaarde voor de parameter als u geen waarde is opgegeven voor de parameter. |
| allowedValues |Nee |Matrix van toegestane waarden voor de parameter om ervoor te zorgen dat de juiste waarde is opgegeven. |
| MinValue |Nee |De minimumwaarde voor de parameters van het type int, deze waarde is liggen. |
| maxValue |Nee |De maximale waarde voor de parameters van het type int, deze waarde is liggen. |
| minLength |Nee |De minimale lengte voor string, secureString en array typeparameters deze waarde is liggen. |
| maxLength |Nee |De maximale lengte voor string, secureString en array typeparameters deze waarde is liggen. |
| description |Nee |Beschrijving van de parameter die wordt weergegeven voor gebruikers via de portal. |

## <a name="template-functions-with-parameters"></a>Sjabloonfuncties met parameters

Als u de standaardwaarde voor een parameter, kunt u de meeste sjabloonfuncties. U kunt een andere parameterwaarde gebruiken voor het bouwen van een standaardwaarde. De volgende sjabloon ziet u het gebruik van functies in de standaardwaarde:

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

U kunt geen gebruiken de `reference` functie in het gedeelte parameters. Parameters worden geëvalueerd vóór de implementatie zodat de `reference` functie de runtimestatus van een resource kan niet worden verkregen. 

## <a name="objects-as-parameters"></a>Objecten als parameters

Het kan zijn eenvoudiger te organiseren gerelateerde waarden door deze in als een object worden doorgegeven. Deze aanpak vermindert ook het aantal parameters in de sjabloon.

De parameter definiëren in uw sjabloon en geef een JSON-object in plaats van een enkele waarde tijdens de implementatie. 

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

Vervolgens verwijzen naar de subeigenschappen van de parameter met de puntoperator.

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

* Beperk het gebruik van parameters. Gebruik indien mogelijk een variabele of een letterlijke waarde. Parameters gebruiken om alleen voor deze scenario's:
   
   * Instellingen die u wilt gebruiken variaties van de basis van de omgeving (SKU, grootte, capaciteit).
   * Resourcenamen die u wilt opgeven voor eenvoudige identificatie.
   * De waarden die u vaak gebruikt om andere taken (zoals een beheerdersgebruikersnaam) te voltooien.
   * Geheimen (zoals wachtwoorden).
   * Het nummer of de matrix met waarden te gebruiken als u meerdere exemplaren van een resourcetype.
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

* Standaardwaarden voor parameters (met uitzondering van wachtwoorden en SSH-sleutels) definiëren. Dankzij een standaardwaarde, wordt er tijdens de implementatie van de parameter optioneel. De standaardwaarde kan een lege tekenreeks zijn. 
   
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

* Gebruik **SecureString** voor alle wachtwoorden en geheimen. Als u gevoelige gegevens in een JSON-object doorgeven, gebruikt u de **secureObject** type. Sjabloonparameters met secureString of secureObject typen kunnen niet worden gelezen na de implementatie van de resource. 
   
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

* Gebruik een parameter locatie op te geven en die zo veel mogelijk parameterwaarde met bronnen die zich waarschijnlijk op dezelfde locatie delen. Hierdoor minimaliseert het aantal keren dat gebruikers worden gevraagd om locatie-informatie te geven. Als u een resourcetype wordt ondersteund in een beperkt aantal locaties, is het raadzaam een geldige locatie rechtstreeks opgeven in de sjabloon of het toevoegen van een andere locatieparameter. Wanneer een organisatie de toegestane regio's voor de gebruikers beperkt de **resourceGroup () .location** expressie mogelijk te voorkomen dat een gebruiker kunnen worden geïmplementeerd, de sjabloon. Een gebruiker maakt bijvoorbeeld een resourcegroep in een regio. Een tweede gebruiker aan deze resourcegroep moet implementeren, maar heeft geen toegang tot het gebied. 
   
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
    
* Vermijd het gebruik van een parameter of variabele voor de API-versie voor een resourcetype. Resource-eigenschappen en waarden kunnen variëren door versienummer. IntelliSense in een code-editor kan niet het juiste schema bepalen wanneer de API-versie is ingesteld op een parameter of variabele. In plaats daarvan harde code de API-versie in de sjabloon.
* Geef een parameternaam in de sjabloon die overeenkomt met een parameter in de implementatieopdracht. Resource Manager deze naming conflict opgelost doordat de postfix **FromTemplate** voor de sjabloonparameter. Bijvoorbeeld, als u een parameter genaamd opnemen **ResourceGroupName** in uw sjabloon een conflict met de **ResourceGroupName** parameter in de [New-AzureRmResourceGroupDeployment ](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet. Tijdens de implementatie, moet u een waarde opgeven voor **ResourceGroupNameFromTemplate**.

## <a name="example-templates"></a>Voorbeeld-sjablonen

Deze sjablonen voorbeeld ziet u enkele scenario's voor het gebruik van parameters. Deze als u wilt testen hoe parameters worden verwerkt in verschillende scenario's implementeren.

|Template  |Beschrijving  |
|---------|---------|
|[parameters met functies voor standaardwaarden](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Demonstreert hoe u sjabloonfuncties gebruiken bij het definiëren van de standaardwaarden voor parameters. De sjabloon implementeert geen bronnen. Het constructs parameterwaarden en die waarden retourneert. |
|[parameter-object](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Demonstreert met behulp van een object voor een parameter. De sjabloon implementeert geen bronnen. Het constructs parameterwaarden en die waarden retourneert. |

## <a name="next-steps"></a>Volgende stappen

* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Voor het invoeren van de parameterwaarden die tijdens de implementatie, Zie [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md). 
* Zie voor meer informatie over de functies die u van binnen een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Zie voor meer informatie over het gebruik van een parameter-object [gebruik van een object als parameter voor een Azure Resource Manager-sjabloon](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).
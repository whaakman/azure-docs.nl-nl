---
title: Azure Machine Learning-experimenten maken met een Azure Resource Manager-sjabloon | Microsoft Docs
description: Dit artikel bevat een voorbeeld om de account van een Azure Machine Learning experimenteren met een Azure Resource Manager-sjabloon te maken.
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/14/2017
ms.openlocfilehash: c7f4bf0fed35ffb2c03e8b983260c6093032fe79
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="configure-the-azure-machine-learning-experimentation-service"></a>Configureren van de Service Azure Machine Learning-experimenten

## <a name="overview"></a>Overzicht
Azure Machine Learning-experimenten Service-account, werkruimte en project zijn Azure-Resources. Als zodanig kunnen ze worden geïmplementeerd met behulp van sjablonen Resources Manager. Resource Manager-sjablonen zijn JSON-bestanden die de resources definiëren die u voor uw oplossing moet implementeren. Zie [Overzicht van Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) voor inzicht in de concepten die gerelateerd zijn aan het implementeren en beheren van uw Azure-oplossingen.

## <a name="deploy-a-template"></a>Een sjabloon implementeren
Implementeren van een sjabloon vereist een aantal stappen in de Azure-opdrachtregelinterface of in de Azure portal.

### <a name="deploy-a-template-from-the-command-line"></a>Een sjabloon implementeren vanuit de opdrachtregel
Via de opdrachtregelinterface, implementeren één opdracht een sjabloon om een bestaande resourcegroep te.
Zie hieronder voor meer informatie over het maken van een sjabloon.

```sh
# Login and validate your are in the right subscription context
az login

# Create a new resource group (you can use an existing one)
az group create --name <resource group name> --location <supported Azure region>
az group deployment create -n testdeploy -g <resource group name> --template-file <template-file.json> --parameters <parameters.json>
```

### <a name="deploy-a-template-from-the-azure-portal"></a>Een sjabloon implementeren vanuit de Azure-portal
Als u liever, kunt u Azure-portal ook maken en implementeren van een sjabloon gebruiken. Ga als volgt:

1. Ga naar de [Azure-portal](https://portal.azure.com).
2. Selecteer **alle Services** en zoek naar 'sjablonen'.
3. Selecteer **sjablonen**.
4. Klik op **+ toevoegen** en kopieer de sjabloongegevens van uw. 
5. Selecteer de sjabloon hebt gemaakt in stap &#4; en klik op **implementeren**.


## <a name="create-a-template-from-an-existing-azure-resource-in-the-azure-portal"></a>Een sjabloon maken van een bestaande Azure resource in de Azure portal
Als u al een Azure-Machine experimenteren account beschikbaar in [Azure-portal](https://portal.azure.com), kunt u een sjabloon van die bron genereren. 

1. Navigeer naar een Azure-Account voor experimenteren in [Azure-portal](https://portal.azure.com).
2. Onder **instellingen**, klikt u op **automatiseringsscript**.
3. De sjabloon downloaden. 

U kunt ook handmatig de sjabloonbestanden bewerken. Zie het volgende voor een voorbeeld van een sjabloon en parameters bestanden. 

### <a name="template-file-example"></a>Voorbeeld van een sjabloon
Maak een bestand met de naam 'sjabloon file.json' met onderstaande inhoud. 

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "Name of the machine learning experimentation team account"
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of the machine learning experimentation account and other dependent resources."
            }
        },
        "storageAccountSku": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "metadata": {
                "description": "Sku of the storage account"
            }
        }
    },
    "variables": {
        "mlexpVersion": "2017-05-01-preview",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
    },
    "resources": [
        {
            "name": "[parameters('accountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2016-12-01",
            "tags": {
                "mlteamAccount": "[parameters('accountName')]"
            },
            "sku": {
                "name": "[parameters('storageAccountSku')]"
            },
            "kind": "Storage",
            "properties": {
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            }
        },
        {
            "apiVersion": "[variables('mlexpVersion')]",
            "type": "Microsoft.MachineLearningExperimentation/accounts",
            "name": "[parameters('accountName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
            ],
            "properties": {
                "storageAccount": {
                    "storageAccountId": "[variables('stgResourceId')]",
                    "accessKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('accountName')), '2016-12-01').keys[0].value]"
                }
            }
        }
    ]
}
```

### <a name="parameters"></a>Parameters 
Maak een bestand met de onderstaande inhoud en sla deze op als < parameters.json >. 

Er zijn drie waarden die u kunt wijzigen. 
* AccountName: De naam van het account experimenteren.
* Locatie: Een van de ondersteunde Azure-regio's.
* SKU-Opslagaccount: De Azure ML ondersteunt alleen de standard-opslag, niet premium. Zie voor meer informatie over opslag [opslag inleiding](https://docs.microsoft.com/azure/storage/common/storage-introduction). 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "accountName": {
         "value": "MyExperimentationAccount"
     },
     "location": {
         "value": "eastus2"
     },
     "storageAccountSku": {
         "value": "Standard_LRS"
     }
  }
}
```

## <a name="next-steps"></a>Volgende stappen
* [Maak en installeer Azure Machine Learning](quickstart-installation.md)

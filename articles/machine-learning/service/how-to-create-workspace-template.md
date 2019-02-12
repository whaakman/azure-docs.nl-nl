---
title: Een sjabloon gebruiken om een werkruimte te maken
titleSuffix: Azure Machine Learning service
description: Leer hoe u een Azure resource manager-sjabloon gebruiken om te maken van een nieuwe werkruimte voor Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: haining
author: hning86
ms.date: 02/11/2019
ms.openlocfilehash: 71513c91d83f54359e43a41e178d91fe97a4ce82
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56101123"
---
# <a name="create-an-azure-machine-learning-service-workspace-by-using-a-template"></a>Een werkruimte van Azure Machine Learning-service met behulp van een sjabloon maken

In dit artikel leert u enkele manieren om te maken van een werkruimte van Azure Machine Learning-service met behulp van Azure Resource Manager-sjablonen. 

Zie voor meer informatie, [een toepassing implementeren met Azure Resource Manager-sjabloon](../../azure-resource-manager/resource-group-template-deploy.md).

## <a name="prerequisites"></a>Vereisten

* Een **Azure-abonnement**. Als u een hebt, kunt u de [gratis of betaalde versie van Azure Machine Learning-service](http://aka.ms/AMLFree).

* Een sjabloon van een CLI wilt gebruiken, moet u een [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) of de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="resource-manager-template"></a>Resource Manager-sjabloon

Resource Manager-sjabloon kunt u eenvoudig is om resources te maken als een enkele, gecoördineerde bewerking. Een sjabloon is een JSON-document waarin de resources die nodig zijn voor een implementatie. Het kan ook implementatieparameters opgeven. Parameters worden gebruikt voor invoerwaarden bij het gebruik van de sjabloon.

De volgende sjabloon kan worden gebruikt om een werkruimte van Azure Machine Learning-service en de gekoppelde Azure-resources te maken:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "The name of the Azure Machine Learning service workspace."
            }
        },
        "location": {
            "type": "string",
            "allowedValues": [
                "eastus",
                "eastus2",
                "southcentralus",
                "southeastasia",
                "westcentralus",
                "westeurope",
                "westus2"
            ],
            "metadata": {
                "description": "The location where the workspace will be created."
            }
        }
    },
    "variables": {
        "storageAccount": {
            "name": "[concat('sa',uniqueString(resourceGroup().id))]",
            "type": "Standard_LRS"
        },
        "keyVault": {
            "name": "[concat('kv',uniqueString(resourceGroup().id))]",
            "tenantId": "[subscription().tenantId]"
        },
        "applicationInsightsName": "[concat('ai',uniqueString(resourceGroup().id))]",
        "containerRegistryName": "[concat('cr',uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "name": "[variables('storageAccount').name]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2018-07-01",
            "sku": {
                "name": "[variables('storageAccount').type]"
            },
            "kind": "StorageV2",
            "properties": {
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "supportHttpsTrafficOnly": true
            }
        },
        {
            "name": "[variables('keyVault').name]",
            "type": "Microsoft.KeyVault/vaults",
            "apiVersion": "2018-02-14",
            "location": "[parameters('location')]",
            "properties": {
                "tenantId": "[variables('keyVault').tenantId]",
                "sku": {
                    "name": "standard",
                    "family": "A"
                },
                "accessPolicies": []
            }
        },
        {
            "name": "[variables('applicationInsightsName')]",
            "type": "Microsoft.Insights/components",
            "apiVersion": "2015-05-01",
            "location": "[if(or(equals(parameters('location'),'eastus2'),equals(parameters('location'),'westcentralus')),'southcentralus',parameters('location'))]",
            "kind": "web",
            "properties": {
                "Application_Type": "web"
            }
        },
        {
            "name": "[variables('containerRegistryName')]",
            "type": "Microsoft.ContainerRegistry/registries",
            "apiVersion": "2017-10-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "Standard"
            },
            "properties": {
                "adminUserEnabled": true
            }
        },
        {
            "name": "[parameters('workspaceName')]",
            "type": "Microsoft.MachineLearningServices/workspaces",
            "apiVersion": "2018-11-19",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[variables('storageAccount').name]",
                "[variables('keyVault').name]",
                "[variables('applicationInsightsName')]",
                "[variables('containerRegistryName')]"
            ],
            "identity": {
                "type": "systemAssigned"
            },
            "properties": {
                "friendlyName": "[parameters('workspaceName')]",
                "keyVault": "[resourceId('Microsoft.KeyVault/vaults',variables('keyVault').name)]",
                "applicationInsights": "[resourceId('Microsoft.Insights/components',variables('applicationInsightsName'))]",
                "containerRegistry": "[resourceId('Microsoft.ContainerRegistry/registries',variables('containerRegistryName'))]",
                "storageAccount": "[resourceId('Microsoft.Storage/storageAccounts/',variables('storageAccount').name)]"
            }
        }
    ]
}
```

Deze sjabloon maakt u de volgende Azure-services:

* Azure-resourcegroep
* Azure Storage-account
* Azure Key Vault
* Azure Application Insights
* Azure Container Registry
* Azure Machine Learning-werkruimte

De resourcegroep is de container met de services. De verschillende services zijn vereist voor de Azure Machine Learning-werkruimte.

De voorbeeldsjabloon heeft twee parameters:

* De **locatie** waar de resourcegroep en de services worden gemaakt.

    De sjabloon wordt de locatie die u selecteert voor de meeste resources gebruiken. De uitzondering hierop is de Application Insights-service is niet beschikbaar in alle locaties zijn de andere services. Als u een locatie waar deze niet beschikbaar is selecteert, wordt de service worden gemaakt in de locatie Zuid-centraal VS.

* De **Werkruimtenaam**, dit is de beschrijvende naam van de Azure Machine Learning-werkruimte.

    De namen van de andere services worden willekeurig gegenereerd.

Zie de volgende artikelen voor meer informatie over sjablonen:

* [Azure Resource Manager-sjablonen maken](../../azure-resource-manager/resource-group-authoring-templates.md)
* [Een toepassing implementeren met Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-template-deploy.md)
* [Microsoft.MachineLearningServices resource types](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

1. Volg de stappen in [resources implementeren vanuit een aangepaste sjabloon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template). Als u aankomen bij de __template bewerken__ scherm, in de sjabloon van dit document plakken.
1. Selecteer __opslaan__ de sjabloon wilt gebruiken. Geef de volgende informatie en ga akkoord met de vermelde voorwaarden:

    * Abonnement: Selecteer het Azure-abonnement moet worden gebruikt voor deze resources.
    * Resourcegroep: Selecteer of maak een resourcegroep voor de services bevatten.
    * Werkruimtenaam: De naam moet worden gebruikt voor de Azure Machine Learning-werkruimte die worden gemaakt. Naam van de werkruimte moet tussen 3 en 33 tekens lang zijn. Deze mag alleen alfanumerieke tekens bevatten en '-'.
    * Locatie: Selecteer de locatie waar de resources worden gemaakt.

    ![De sjabloonparameters in Azure portal](media/how-to-create-workspace-template/template-parameters.png)

Zie voor meer informatie, [resources implementeren vanuit een aangepaste sjabloon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

In dit voorbeeld wordt ervan uitgegaan dat u de sjabloon hebt opgeslagen naar een bestand met de naam `azuredeploy.json` in de huidige map:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace"
```

Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md) en [persoonlijke Resource Manager-sjabloon implementeren met SAS-token en Azure PowerShell](../../azure-resource-manager/resource-manager-powershell-sas-token.md).

## <a name="use-azure-cli"></a>Azure CLI gebruiken

In dit voorbeeld wordt ervan uitgegaan dat u de sjabloon hebt opgeslagen naar een bestand met de naam `azuredeploy.json` in de huidige map:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace
```

Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md) en [persoonlijke Resource Manager-sjabloon implementeren met SAS-token en Azure CLI](../../azure-resource-manager/resource-manager-cli-sas-token.md).

## <a name="next-steps"></a>Volgende stappen

* [Resources implementeren met Resource Manager-sjablonen en Resource Manager REST API](../../azure-resource-manager/resource-group-template-deploy-rest.md).
* [Het maken en implementeren van Azure-resourcegroepen met Visual Studio](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
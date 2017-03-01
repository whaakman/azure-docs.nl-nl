---
title: Resources implementeren op Azure | Microsoft Docs
description: Azure PowerShell of Azure CLI gebruiken voor het implementeren van resources op Azure. De resources zijn gedefinieerd in een Resource Manager-sjabloon.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 41d9476d4ce6b7afc2ba9c757e96db7e8e72d5ae
ms.openlocfilehash: a77354d0719240e5916b5ec945dad75e534802d3


---
# <a name="deploy-resources-to-azure"></a>Resources implementeren op Azure

In dit onderwerp wordt uitgelegd hoe u resources kunt implementeren op uw Azure-abonnement. U kunt Azure PowerShell of Azure CLI gebruiken voor het implementeren van een Resource Manager-sjabloon die de infrastructuur voor uw oplossing definieert.

Zie [Overzicht van Azure Resource Manager](resource-group-overview.md) voor een inleiding tot de concepten van Resource Manager.

## <a name="steps-for-deployment"></a>Stappen voor implementatie

In dit onderwerp wordt ervan uitgegaan dat u de [voorbeeldopslagsjabloon](#example-storage-template) in dit onderwerp implementeert. U kunt een ander sjabloon gebruiken, maar de parameters die u doorgeeft wijken af van wat wordt weergegeven in dit onderwerp.

Nadat u een sjabloon hebt gemaakt, volgt u de volgende algemene stappen voor het implementeren van de sjabloon:

1. Meld u aan bij uw account
2. Het abonnement selecteren dat u wilt gebruiken (alleen nodig als u meerdere abonnementen hebt en u een abonnement wilt gebruiken dat niet het standaardabonnement is)
3. Een resourcegroep maken
4. De sjabloon implementeren
5. De implementatiestatus controleren

De volgende secties tonen hoe u deze stappen uitvoert met [PowerShell](#powershell) of [Azure CLI](#azure-cli).

## <a name="powershell"></a>PowerShell

1. Zie [Get started with Azure PowerShell cmdlets](/powershell/azureps-cmdlets-docs) (Aan de slag met Azure PowerShell-cmdlets) om Azure PowerShell te installeren.

2. Gebruik de volgende cmdlets om snel aan de slag te gaan met implementeren:

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  De cmdlet `Set-AzureRmContext` is alleen nodig als u een ander abonnement wilt gebruiken dan uw standaardabonnement. Gebruik het volgende om al uw abonnementen en de bijbehorende id's weer te geven:

  ```powershell
  Get-AzureRmSubscription
  ```

3. De implementatie kan enkele minuten duren. Als deze is voltooid, ziet u een bericht dat lijkt op:

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. Als u wilt controleren of uw resourcegroep en opslagaccount zijn geïmplementeerd op uw abonnement, gebruikt u:

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. U kunt sjabloonparameters opgeven als PowerShell-parameters bij het implementeren van een sjabloon. Het vorige voorbeeld bevat geen sjabloonparameters, waardoor de standaardwaarden in de sjabloon zijn gebruikt. Als u een ander opslagaccount wilt implementeren en parameterwaarden wilt opgeven voor het voorvoegsel van de opslagnaam en de SKU van het opslagaccount, gebruikt u:

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  U hebt nu twee opslagaccounts in de resourcegroep. 

## <a name="azure-cli"></a>Azure CLI

1. Zie [Azure CLI 2.0 installeren](/cli/azure/install-az-cli2) voor het installeren van Azure CLI.

2. Gebruik de volgende opdrachten om snel aan de slag te gaan met implementeren:

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  De opdracht `az account set` is alleen nodig als u een ander abonnement wilt gebruiken dan uw standaardabonnement. Gebruik het volgende om al uw abonnementen en de bijbehorende id's weer te geven:

  ```azurecli
  az account list
  ```

3. De implementatie kan enkele minuten duren. Als deze is voltooid, ziet u een bericht dat lijkt op:

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. Als u wilt controleren of uw resourcegroep en opslagaccount zijn geïmplementeerd op uw abonnement, gebruikt u:

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. U kunt sjabloonparameters opgeven als PowerShell-parameters bij het implementeren van een sjabloon. Het vorige voorbeeld bevat geen sjabloonparameters, waardoor de standaardwaarden in de sjabloon zijn gebruikt. Als u een ander opslagaccount wilt implementeren en parameterwaarden wilt opgeven voor het voorvoegsel van de opslagnaam en de SKU van het opslagaccount, gebruikt u:

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  U hebt nu twee opslagaccounts in de resourcegroep. 

## <a name="example-storage-template"></a>Voorbeeldopslagsjabloon

Gebruik de volgende voorbeeldsjabloon voor het implementeren van een opslagaccount op uw abonnement:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
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
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Volgende stappen

* Zie [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](/azure/azure-resource-manager/resource-group-template-deploy) voor meer informatie over hoe u PowerShell gebruikt voor het implementeren van sjablonen.
* Zie [Resources implementeren met Resource Manager-sjablonen en Azure CLI](/azure/azure-resource-manager/resource-group-template-deploy-cli) voor meer informatie over hoe u Azure CLI gebruikt voor het implementeren van sjablonen.






<!--HONumber=Feb17_HO3-->



---
title: Koppelen van een volume van de Azure-bestanden in Azure Containerexemplaren
description: Meer informatie over het koppelen van een Azure-bestanden volume om te blijven behouden status met exemplaren van Azure-Container
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 41c3a449b39d6ef77e1dd0cf10699f8debcad475
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2017
---
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a>Koppelen van een Azure-bestandsshare met exemplaren van Azure-Container

Standaard zijn exemplaren van Azure-Container staatloze. Als de container vastloopt of stopt, gaat alle van de status verloren. Om te blijven behouden status afgezien van de levensduur van de container, moet u een volume koppelen vanuit een externe winkel. In dit artikel laat zien hoe een Azure-bestandsshare voor gebruik met Azure Containerexemplaren te koppelen.

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken

Voordat u een Azure-bestandsshare met exemplaren van Azure-Container, moet u deze maken. Voer het volgende script voor het maken van een opslagaccount voor het hosten van de bestandsshare en de share zelf. De opslagaccountnaam moet globaal uniek zijn, zodat het script voegt een willekeurige waarde toe aan de basis-tekenreeks.

```azurecli-interactive
# Change these four parameters
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a>De accountdetails toegang tot opslag verkrijgen

Een Azure-bestandsshare als een volume in Azure Containerexemplaren koppelen, moet u drie waarden: naam van het opslagaccount, de sharenaam van de en de toegangssleutel voor opslag.

Als u het bovenstaande script gebruikt, wordt de naam van het opslagaccount is gemaakt met een willekeurige waarde aan het einde. Om te vragen van de laatste tekenreeks (inclusief het willekeurig gedeelte), gebruikt u de volgende opdrachten:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

Al bekend is de sharenaam (is *acishare* in het bovenstaande script), zodat alle dat u hoeft alleen nog de opslagaccountsleutel die kan worden gevonden met de volgende opdracht:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a>Storage-account toegangsgegevens met Azure sleutelkluis opslaan

Toegangscodes voor opslag beveiligen toegang tot uw gegevens, zodat we het beste opslaan in een Azure sleutelkluis.

Een sleutelkluis maken met de Azure CLI:

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g $ACI_PERS_RESOURCE_GROUP
```

De `enabled-for-template-deployment` switch kunt Azure Resource Manager pull geheimen van de sleutelkluis tijdens de implementatie.

De opslagaccountsleutel opslaan als een nieuwe geheim in de sleutelkluis:

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-the-volume"></a>Het volume koppelen

Koppelen van een Azure-bestandsshare als een volume in een container is een proces. Eerst u de details van de share als onderdeel van het definiëren van de containergroep opgeeft, vervolgens u opgeven hoe u het volume dat is gekoppeld in een of meer van de containers in de groep wilt gebruiken.

Om te definiëren van de volumes die u beschikbaar wilt maken voor het koppelen, Voeg een `volumes` matrix naar de definitie van de container in de Azure Resource Manager-sjabloon en klik vervolgens in de definitie van de afzonderlijke containers verwijzing.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "type": "string"
    },
    "storageaccountkey": {
      "type": "securestring"
    }
  },
  "resources":[{
    "name": "hellofiles",
    "type": "Microsoft.ContainerInstance/containerGroups",
    "apiVersion": "2017-08-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
      "containers": [{
        "name": "hellofiles",
        "properties": {
          "image": "seanmckenna/aci-hellofiles",
          "resources": {
            "requests": {
              "cpu": 1,
              "memoryInGb": 1.5
            }
          },
          "ports": [{
            "port": 80
          }],
          "volumeMounts": [{
            "name": "myvolume",
            "mountPath": "/aci/logs/"
          }]
        }
      }],
      "osType": "Linux",
      "ipAddress": {
        "type": "Public",
        "ports": [{
          "protocol": "tcp",
          "port": "80"
        }]
      },
      "volumes": [{
        "name": "myvolume",
        "azureFile": {
          "shareName": "acishare",
          "storageAccountName": "[parameters('storageaccountname')]",
          "storageAccountKey": "[parameters('storageaccountkey')]"
        }
      }]
    }
  }]
}
```

De sjabloon bevat de naam van het opslagaccount en de sleutel als parameters die in een afzonderlijke parameterbestand kunnen worden opgegeven. Om te voorzien van het parameterbestand, moet u drie waarden: naam van het opslagaccount, de bron-ID van uw Azure sleutelkluis en de geheime naam van sleutelkluis die u hebt gebruikt voor het opslaan van de opslagsleutel. Als u de vorige stappen hebt gevolgd, kunt u deze waarden met het volgende script kunt krijgen:

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

Voeg de waarden in het parameterbestand:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "value": "<my_storage_account_name>"
    },
   "storageaccountkey": {
      "reference": {
        "keyVault": {
          "id": "<my_keyvault_id>"
        },
        "secretName": "<my_storage_account_key_secret_name>"
      }
    }
  }
}
```

## <a name="deploy-the-container-and-manage-files"></a>De container implementeren en beheren van bestanden

Met de sjabloon is gedefinieerd, kunt u de container maken en koppelen van het volume met de Azure CLI. Ervan uitgaande dat de naam van het sjabloonbestand *azuredeploy.json* en met de naam van het parameterbestand *azuredeploy.parameters.json*, dan is de opdrachtregel:

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

Nadat de container wordt gestart, kunt u de eenvoudige web-app geïmplementeerd de **aci-seanmckenna-hellofiles** afbeelding voor het beheren van de bestanden in de Azure-bestandsshare op de koppelpad die u hebt opgegeven. Het IP-adres voor de web-app via de volgende verkrijgen:

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

U kunt een hulpprogramma zoals de [Microsoft Azure Storage Explorer](http://storageexplorer.com) op te halen en het controleren van het bestand is geschreven naar de bestandsshare.

>[!NOTE]
> Zie voor meer informatie over het gebruik van Azure Resource Manager-sjablonen, parameterbestanden en implementeren met de Azure CLI [implementeren van resources met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

## <a name="next-steps"></a>Volgende stappen

- Implementeren voor uw eerste container met de Azure-Container instanties [Quick Start](container-instances-quickstart.md)
- Meer informatie over de [relatie tussen Azure Containerexemplaren en container orchestrators](container-instances-orchestrator-relationship.md)

---
title: Azure Storage gebruiken als een back-end van Terraform
description: Een inleiding tot het opslaan van Terraform-status in Azure Storage.
services: terraform
author: neilpeterson
ms.service: terraform
ms.topic: article
ms.date: 09/13/2018
ms.author: nepeters
ms.openlocfilehash: 2bee9f73f430e18fe159eed142b265cc1934860e
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984964"
---
# <a name="store-terraform-state-in-azure-storage"></a>Status van de Store Terraform in Azure Storage

Terraform-status wordt gebruikt voor het afstemmen van geïmplementeerde resources met Terraform-configuraties. Met behulp van de status, weet Terraform welke Azure-resources toevoegen, bijwerken of verwijderen. Standaard lokaal bij het uitvoeren van Terraform status opgeslagen *Terraform toepassen*. Deze configuratie is niet ideaal voor een aantal oorzaken hebben:

- Lokale status werkt niet goed in een team of een samenwerkingsomgeving
- Terraform-status kan onder andere gevoelige informatie
- Status lokaal opslaan verhoogt de kans op onbedoeld verwijderen

Terraform bevat het concept van een status-back-end die externe opslag voor de status van Terraform is. Wanneer u een back-end van de staat, wordt de statusbestand opgeslagen in een gegevensarchief, zoals Azure Storage. Dit document wordt uitgelegd hoe configureren en gebruiken van Azure Storage als een back-end Terraform staat.

## <a name="configure-storage-account"></a>Storage-account configureren

Voordat u een back-end Azure Storage, moet een storage-account worden gemaakt. Het storage-account kan worden gemaakt met de Azure-portal, PowerShell, de Azure CLI of Terraform zelf. Gebruik het volgende voorbeeld om te configureren van het storage-account met de Azure CLI.

```azurecli-interactive
#!/bin/bash

RESOURCE_GROUP_NAME=tfstatestorage
STORAGE_ACCOUNT_NAME=tfstatestorage$RANDOM
CONTAINER_NAME=tfstatestorage

# Create resource group
az group create --name $RESOURCE_GROUP_NAME --location eastus

# Create storage account
az storage account create --resource-group $RESOURCE_GROUP_NAME --name $STORAGE_ACCOUNT_NAME --sku Standard_LRS --encryption-services blob

# Get storage account key
ACCOUNT_KEY=$(az storage account keys list --resource-group $RESOURCE_GROUP_NAME --account-name $STORAGE_ACCOUNT_NAME --query [0].value -o tsv)

# Create blob container
az storage container create --name $CONTAINER_NAME --account-name $STORAGE_ACCOUNT_NAME --account-key $ACCOUNT_KEY

echo "storage_account_name: $STORAGE_ACCOUNT_NAME"
echo "container_name: $CONTAINER_NAME"
echo "access_key: $ACCOUNT_KEY"
```

Noteer de naam van het opslagaccount, de containernaam en de toegangssleutel voor opslag. Deze waarden nodig zijn bij het configureren van de externe status.

## <a name="configure-state-backend"></a>Back-end van status configureren

De back-end status Terraform is geconfigureerd bij het uitvoeren van *Terraform init*. Als u wilt de status van back-end configureert, zijn de volgende gegevens zijn vereist.

- storage_account_name - de naam van de Azure Storage-account.
- container_name - de naam van de blob-container.
- sleutel - de naam van de status opslaan bestand dat moet worden gemaakt.
- access_key - de toegangssleutel voor opslag.

Elk van deze waarden kan worden opgegeven in het configuratiebestand Terraform of op de opdrachtregel, maar het is raadzaam te gebruiken van een omgevingsvariabele voor de `access_key`. Met behulp van een omgevingsvariabele voorkomt dat de sleutel wordt geschreven naar schijf.

Maken van een omgevingsvariabele met de naam `ARM_ACCESS_KEY` met de waarde van de Azure Storage-toegangssleutel.

```console
export ARM_ACCESS_KEY=<storage access key>
```

Bescherming van de toegangssleutel van de Azure Storage-account, kunt u het opslaan in Azure Key Vault. De omgevingsvariabele kan vervolgens worden ingesteld met behulp van een vergelijkbaar met de volgende opdracht. Zie voor meer informatie over Azure Key Vault, de [Azure Key Vault-documentatie][azure-key-vault].

```console
export ARM_ACCESS_KEY=$(az keyvault secret show --name terraform-backend-key --vault-name myKeyVault --query value -o tsv)
```

Voor het configureren van Terraform voor het gebruik van de back-end, omvatten een *back-end* configuratie met een type *azurerm* binnen de Terraform-configuratie. Voeg de *storage_account_name*, *container_name*, en *sleutel* waarden naar het configuratie-blok.

Het volgende voorbeeld configureert u een back-end van Terraform en maakt en Azure-resourcegroep.

```json
terraform {
  backend "azurerm" {
    storage_account_name  = "tstate"
    container_name        = "tfstate"
    key                   = "terraform.tfstate"
  }
}

resource "azurerm_resource_group" "state-demo-secure" {
  name     = "state-demo"
  location = "eastus"
}
```

Nu de configuratie met initialiseren *Terraform init* en voer vervolgens de configuratie met *Terraform toepassen*. Wanneer dit is voltooid, vindt u het bestand staat in de Azure Storage-Blob.

## <a name="state-locking"></a>Status van de vergrendeling

Wanneer u een Azure Storage-Blob voor status-opslag, wordt de blob is automatisch vergrendeld voordat u een bewerking waarbij de status schrijft. Deze configuratie wordt voorkomen dat meerdere gelijktijdige bewerkingen, die leiden beschadiging tot kunnen. Zie voor meer informatie, [status vergrendelen] [ terraform-state-lock] op de Terraform-documentatie.

De vergrendeling kan worden gezien bij het onderzoek van de blob al de Azure portal of andere hulpprogramma's voor Azure management.

![Azure-blob met vergrendeling](media/terraform-backend/lock.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Terraform back-configuratie op de [Terraform back-end documentatie][terraform-backend].

<!-- LINKS - external -->
[azure-key-vault]: ../key-vault/quick-create-cli.md
[terraform-azurerm]: https://www.terraform.io/docs/backends/types/azurerm.html
[terraform-backend]: https://www.terraform.io/docs/backends/
[terraform-state-lock]: https://www.terraform.io/docs/state/locking.html

---
title: Een Azure-bestanden volume in Azure Containerexemplaren koppelen
description: Meer informatie over het koppelen van een Azure-bestanden volume om te blijven behouden status met exemplaren van Azure-Container
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: be502e6aef39ee4ed8cfc1f8926cb556dc1defb1
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="mount-an-azure-file-share-with-azure-container-instances"></a>Koppelen van een Azure-bestandsshare met exemplaren van Azure-Container

Standaard zijn exemplaren van Azure-Container staatloze. Als de container vastloopt of stopt, gaat alle van de status verloren. Om te blijven behouden status afgezien van de levensduur van de container, moet u een volume koppelen vanuit een externe winkel. In dit artikel laat zien hoe een Azure-bestandsshare voor gebruik met Azure Containerexemplaren te koppelen.

> [!NOTE]
> Koppelen van een Azure-bestandsshare is momenteel beperkt tot Linux containers. Terwijl we proberen te zorgen dat alle functies op Windows-containers, vindt u de huidige platform verschillen in [quota en beschikbaarheid in regio's voor exemplaren van Azure-Container](container-instances-quotas.md).

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken

Voordat u een Azure-bestandsshare met exemplaren van Azure-Container, moet u deze maken. Voer het volgende script voor het maken van een opslagaccount voor het hosten van de bestandsshare en de share zelf. De opslagaccountnaam moet globaal uniek zijn, zodat het script voegt een willekeurige waarde toe aan de basis-tekenreeks.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Export the connection string as an environment variable. The following 'az storage share create' command
# references this environment variable when creating the Azure file share.
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $ACI_PERS_RESOURCE_GROUP --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`

# Create the file share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="get-storage-credentials"></a>Storage-referenties ophalen

Een Azure-bestandsshare als een volume in Azure Containerexemplaren koppelen, moet u drie waarden: naam van het opslagaccount, de sharenaam van de en de toegangssleutel voor opslag.

Als u het bovenstaande script gebruikt, wordt de naam van het opslagaccount is gemaakt met een willekeurige waarde aan het einde. Om te vragen van de laatste tekenreeks (inclusief het willekeurig gedeelte), gebruikt u de volgende opdrachten:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

Al bekend is de sharenaam (gedefinieerd als *acishare* in het bovenstaande script), zodat alle dat u hoeft alleen nog de opslagaccountsleutel die kan worden gevonden met de volgende opdracht:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Container en koppelpunten volume implementeren

Geef de share- en volume koppelpunt bij het maken van de container met een Azure-bestandsshare koppelen als een volume in een container, [az container maken][az-container-create]. Als u de vorige stappen hebt gevolgd, kunt u de share die u eerder hebt gemaakt met behulp van de volgende opdracht voor het maken van een container koppelen:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image seanmckenna/aci-hellofiles \
    --ip-address Public \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

## <a name="manage-files-in-mounted-volume"></a>Bestanden in gekoppelde volume beheren

Nadat de container wordt gestart, kunt u de eenvoudige web-app geïmplementeerd de [aci-seanmckenna-hellofiles] [ aci-hellofiles] afbeelding voor het beheren van de bestanden in de Azure-bestandsshare op het opgegeven koppelpad. Het IP-adres voor de web-app met de [az container weergeven] [ az-container-show] opdracht:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --output table
```

U kunt de [Azure-portal] [ portal] of een hulpprogramma zoals de [Microsoft Azure Storage Explorer] [ storage-explorer] op te halen en het bestand is geschreven naar controleren de bestandsshare.

## <a name="mount-multiple-volumes"></a>Meerdere volumes koppelen

Meerdere volumes in een container-exemplaar koppelen, moet u implementeren met behulp van een [Azure Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups).

Eerst, geef de details van de share en de volumes definiëren door in te vullen de `volumes` -matrix in de `properties` gedeelte van de sjabloon. Bijvoorbeeld, als u twee bestanden met Azure-shares met de naam hebt gemaakt *share1* en *share2* in opslagaccount *myStorageAccount*, wordt de `volumes` matrix wordt weergegeven de volgende strekking:

```json
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

Vervolgens voor elke container in de containergroep waarin u wilt koppelen van de volumes vullen de `volumeMounts` -matrix in de `properties` sectie van de definitie van de container. Bijvoorbeeld: dit koppelt de twee volumes *myvolume1* en *myvolume2*, die is eerder gedefinieerd:

```json
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

Zie voor een voorbeeld van de implementatie van de container-exemplaar met een Azure Resource Manager-sjabloon [implementeren meerdere container groepen in Azure Containerexemplaren](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de relatie tussen [exemplaren van Azure-Container en container orchestrators](container-instances-orchestrator-relationship.md).

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/r/seanmckenna/aci-hellofiles/
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az_container_create
[az-container-show]: /cli/azure/container#az_container_show
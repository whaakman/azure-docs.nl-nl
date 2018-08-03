---
title: Koppelen van een Azure Files-volume in Azure Container Instances
description: Informatie over het koppelen van een Azure Files-volume als u wilt persisteren van statuswaarden met Azure Container Instances
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 02/20/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 83c86d8310aff80f148e878261ba33b01846006b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441320"
---
# <a name="mount-an-azure-file-share-in-azure-container-instances"></a>Een Azure-bestandsshare in Azure Container Instances koppelen

Azure Container Instances zijn standaard staatloos. Als de container vastloopt of stopt, wordt alle van de staat van verloren gaan. Als u wilt behouden status, buiten de levensduur van de container, moet u een volume koppelen vanuit een externe opslag. In dit artikel laat zien hoe een Azure-bestandsshare voor gebruik met Azure Container Instances koppelen.

> [!NOTE]
> Een Azure-bestandsshare koppelen is momenteel beperkt tot Linux-containers. Terwijl we werken om alle functies op Windows-containers, vindt u de huidige platform verschillen in [quota en beschikbaarheid in regio's voor Azure Container Instances](container-instances-quotas.md).

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken

Voordat u een Azure-bestandsshare gebruikt met Azure Container Instances, moet u deze maken. Voer het volgende script om een opslagaccount voor het hosten van de bestandsshare en de share zelf te maken. Naam van het opslagaccount moet globaal uniek zijn, zodat het script wordt een willekeurige waarde toegevoegd aan de tekenreeks met base.

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

Voor het koppelen van een Azure-bestandsshare als een volume in Azure Container Instances, moet u drie waarden: naam van het opslagaccount, de sharenaam en de toegangssleutel voor opslag.

Als u het bovenstaande script gebruikt, wordt de naam van opslagaccount is gemaakt met een willekeurige waarde aan het einde. Om te vragen de laatste tekenreeks (met inbegrip van het willekeurig gedeelte), gebruikt u de volgende opdrachten:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

De sharenaam is al bekend (gedefinieerd als *acishare* in het bovenstaande script), zodat alle dat blijft is de opslagaccountsleutel die kan worden gevonden met de volgende opdracht:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Container en koppelpunten volume implementeren

Voor het koppelen van een Azure-bestandsshare als een volume in een container, geven de share en het volume koppelpunt bij het maken van de container met [az container maken][az-container-create]. Als u de vorige stappen hebt gevolgd, kunt u de share die u eerder hebt gemaakt met behulp van de volgende opdracht uit om een container te maken kunt koppelen:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image microsoft/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

De waarde `--dns-name-label` moet uniek zijn voor de Azure-regio waar u de containerinstallatiekopie maakt. Werk de waarde in de voorgaande opdracht als u ontvangt een **DNS-naamlabel** foutbericht weergegeven wanneer u de opdracht uitvoert.

## <a name="manage-files-in-mounted-volume"></a>Beheren van bestanden in een gekoppeld volume

Zodra de container wordt gestart, kunt u de eenvoudige web-app geïmplementeerd de [aci-microsoft-hellofiles] [ aci-hellofiles] afbeelding voor het beheren van de bestanden in de Azure-bestandsshare op het koppelingspad die u hebt opgegeven. De web-app volledig gekwalificeerde domeinnaam (FQDN) verkrijgen met de [az container show] [ az-container-show] opdracht:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --query ipAddress.fqdn
```

U kunt de [Azure-portal] [ portal] of een hulpprogramma zoals de [Microsoft Azure Storage Explorer] [ storage-explorer] ophalen en inspecteren van het bestand geschreven naar de bestandsshare.

## <a name="mount-multiple-volumes"></a>Meerdere volumes koppelen

Voor het koppelen van meerdere volumes in een containerexemplaar, moet u implementeren met behulp van een [Azure Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups).

Eerst geven gegevens van de share en de volumes definiëren door in te vullen de `volumes` matrix in de `properties` gedeelte van de sjabloon. Bijvoorbeeld, als u hebt twee Azure-bestandsshares met de naam *share1* en *share2* in storage-account *myStorageAccount*, wordt de `volumes` matrix wordt weergegeven vergelijkbaar met het volgende:

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

Vervolgens voor elke container in containergroep waarin u wilt de volumes koppelen, vullen het `volumeMounts` matrix in de `properties` sectie van de containerdefinitie van de. Bijvoorbeeld, dit koppelt u de twee volumes *myvolume1* en *myvolume2*, die is eerder gedefinieerd:

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

Zie voor een voorbeeld van de implementatie van de containers-instantie met een Azure Resource Manager-sjabloon [groepen met meerdere containers in Azure Container Instances implementeren](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Volgende stappen

Informatie over het koppelen van andere typen in Azure Container Instances:

* [Een volume emptyDir in Azure Containerexemplaren koppelen](container-instances-volume-emptydir.md)
* [Koppelen van een volume gitRepo in Azure Containerexemplaren](container-instances-volume-gitrepo.md)
* [Koppelen van een geheime volume in Azure Container Instances](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/r/microsoft/aci-hellofiles/
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
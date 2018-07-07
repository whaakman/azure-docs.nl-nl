---
title: Gebruik Azure-bestand met AKS
description: Azure-schijven gebruiken met AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 0479e4d80b7490db170255d47ef3190bb744d2d8
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901490"
---
# <a name="volumes-with-azure-files"></a>Volumes met Azure files

Op containers gebaseerde toepassingen moeten vaak voor toegang tot en behoud van gegevens in een volume dat externe gegevens. Azure files kan worden gebruikt als deze extern gegevensarchief. Dit artikel vindt u met behulp van Azure files als een volume Kubernetes in Azure Kubernetes Service.

Zie voor meer informatie over Kubernetes volumes [Kubernetes volumes][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken

Voordat u een Azure-bestandsshare als een Kubernetes-volume, moet u een Azure Storage-account en de bestandsshare maken. Het volgende script kan worden gebruikt om deze taken uit te voeren. Let op of werk de parameterwaarden, enkele van deze nodig zijn bij het maken van het Kubernetes-volume.

```azurecli-interactive
#!/bin/bash

# Change these four parameters
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create the Resource Group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create the storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)

# Echo storage account name and key
echo Storage account name: $AKS_PERS_STORAGE_ACCOUNT_NAME
echo Storage account key: $STORAGE_KEY
```

## <a name="create-kubernetes-secret"></a>Kubernetes-geheim maken

Kubernetes moet referenties voor toegang tot de bestandsshare. Deze referenties worden opgeslagen een [Kubernetes-geheim][kubernetes-secret], die bij het maken van een Kubernetes-schil wordt verwezen.

Gebruik de volgende opdracht om het geheim te maken. Vervang `STORAGE_ACCOUNT_NAME` met de naam van uw opslagaccount, en `STORAGE_ACCOUNT_KEY` met accountsleutel van uw opslagaccount.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=STORAGE_ACCOUNT_KEY
```

## <a name="mount-file-share-as-volume"></a>Koppel de bestandsshare als volume

Koppel uw Azure-bestandsshare in uw schil door het configureren van het volume in de specificatie. Maak een nieuw bestand met de naam `azure-files-pod.yaml` met de volgende inhoud. Update `aksshare` met de naam van de Azure-bestanden delen.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: microsoft/sample-aks-helloworld
    name: azure
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
 volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: aksshare
      readOnly: false
```

Kubectl gebruiken om te maken van een schil.

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

U hebt nu een container die wordt uitgevoerd met uw Azure-bestandsshare die is gekoppeld in de `/mnt/azure` directory.  U kunt zien dat het volume koppelen tijdens de controle van uw schil via `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes-volumes met behulp van Azure Files.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure Files][kubernetes-files]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create

---
title: Azure-bestand met AKS gebruiken
description: Azure-schijven met AKS gebruiken
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 25ae0e508223b50219e40245cc9dfbc407b96bba
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096693"
---
# <a name="volumes-with-azure-files"></a>Volumes met Azure-bestanden

Container gebaseerde toepassingen moeten vaak voor toegang tot en het behouden van gegevens in een externe gegevensbron-volume. Azure-bestanden kunnen worden gebruikt als deze externe gegevensarchief. Dit artikel gegevens als een volume Kubernetes in Azure Kubernetes Service met behulp van Azure files.

Zie voor meer informatie over Kubernetes volumes [Kubernetes volumes][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken

Voordat u een Azure-bestandsshare als een volume Kubernetes, moet u een Azure Storage-account en de bestandsshare te maken. Het volgende script kan worden gebruikt om deze taken te voltooien. Let op of de parameterwaarden die zijn bijgewerkt, sommige van deze nodig zijn bij het maken van het volume Kubernetes.

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
echo Storgae account key: $STORAGE_KEY
```

## <a name="create-kubernetes-secret"></a>Kubernetes geheim maken

Kubernetes moet referenties voor toegang tot de bestandsshare. Deze referenties worden opgeslagen een [Kubernetes geheim][kubernetes-secret], die bij het maken van een schil Kubernetes wordt verwezen.

Gebruik de volgende opdracht voor het maken van het geheim. Vervang `STORAGE_ACCOUNT_NAME` met de naam van uw opslagaccount, en `STORAGE_ACCOUNT_KEY` met de opslagsleutel van uw.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=STORAGE_ACCOUNT_KEY
```

## <a name="mount-file-share-as-volume"></a>Bestandsshare als een volume koppelen

Koppel uw Azure-bestanden delen in uw schil met het configureren van het volume in de specificatie. Maak een nieuw bestand met de naam `azure-files-pod.yaml` met de volgende inhoud. Update `aksshare` met de naam van de Azure-bestanden delen.

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

Kubectl gebruik te maken van een schil.

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

U hebt nu een actieve container met uw Azure-bestandsshare gekoppeld in de `/mnt/azure` directory.  U kunt zien dat het volume dat is gekoppeld bij de inspectie van uw schil via `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes volumes met behulp van Azure-bestanden.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure-bestanden][kubernetes-files]

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

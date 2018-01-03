---
title: Azure-bestand met AKS gebruiken
description: Azure-schijven met AKS gebruiken
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b6267dd2bc1b29229b2e8016e2429ed88b7bf676
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="using-azure-files-with-kubernetes"></a>Azure-bestanden gebruiken in Kubernetes

Container gebaseerde toepassingen moeten vaak voor toegang tot en het behouden van gegevens in een externe gegevensbron-volume. Azure-bestanden kunnen worden gebruikt als deze externe gegevensarchief. Dit artikel gegevens als een volume Kubernetes in Azure Container Service met behulp van Azure files.

Zie voor meer informatie over Kubernetes volumes [Kubernetes volumes][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken

Voordat u een Azure-bestandsshare als een volume Kubernetes, moet u een Azure Storage-account en de bestandsshare te maken. Het volgende script kan worden gebruikt om deze taken te voltooien. Let op of de parameterwaarden die zijn bijgewerkt, sommige van deze nodig zijn bij het maken van het volume Kubernetes.

```azurecli-interactive
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
```

## <a name="create-kubernetes-secret"></a>Kubernetes geheim maken

Kubernetes moet referenties voor toegang tot de bestandsshare. Deze referenties worden opgeslagen een [Kubernetes geheim][kubernetes-secret], die bij het maken van een schil Kubernetes wordt verwezen.

Wanneer u een Kubernetes geheime maakt, moet de geheime waarden base64-gecodeerd.

Codeer eerst de naam van het opslagaccount. Indien nodig, Vervang `$AKS_PERS_STORAGE_ACCOUNT_NAME` met de naam van de Azure storage-account.

```azurecli-interactive
echo -n $AKS_PERS_STORAGE_ACCOUNT_NAME | base64
```

Codeer vervolgens de sleutel van het opslagaccount. Indien nodig, Vervang `$STORAGE_KEY` met de naam van de sleutel van de Azure storage-account.

```azurecli-interactive
echo -n $STORAGE_KEY | base64
```

Maak een bestand met de naam `azure-secret.yaml` en kopieer de volgende YAML. Update de `azurestorageaccountname` en `azurestorageaccountkey` waarden met de base64-gecodeerd waarden in de laatste stap opgehaald.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: azure-secret
type: Opaque
data:
  azurestorageaccountname: <base64_encoded_storage_account_name>
  azurestorageaccountkey: <base64_encoded_storage_account_key>
```

Gebruik de [kubectl maken] [ kubectl-create] opdracht voor het maken van het geheim.

```azurecli-interactive
kubectl create -f azure-secret.yaml
```

## <a name="mount-file-share-as-volume"></a>Bestandsshare als een volume koppelen

U kunt de share van uw Azure-bestanden in uw schil koppelen door het configureren van het volume in de specificatie. Maak een nieuw bestand met de naam `azure-files-pod.yaml` met de volgende inhoud. Update `aksshare` met de naam van de Azure-bestanden delen.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: kubernetes/pause
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

U hebt nu een actieve container met uw Azure-bestandsshare gekoppeld in de `/mnt/azure` directory. U kunt zien dat het volume dat is gekoppeld bij de inspectie van uw schil via `kubectl describe pod azure-files-pod`.

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

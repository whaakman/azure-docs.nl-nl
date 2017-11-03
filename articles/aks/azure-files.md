---
title: Azure-bestand gebruiken met AKS | Microsoft Docs
description: Azure-schijven met AKS gebruiken
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6e88c590e11aa8d2f4ae17e8b5e164483f0a6820
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="using-azure-files-with-kubernetes"></a>Azure-bestanden gebruiken in Kubernetes

Op basis van containertoepassingen moeten vaak voor toegang tot en het behouden van gegevens in een externe gegevensbron-volume. Azure-bestanden kunnen worden gebruikt als deze externe gegevensarchief. Dit artikel gegevens als een volume Kubernetes in Azure Container Service met behulp van Azure files.

Zie voor meer informatie over Kubernetes volumes [Kubernetes volumes][kubernetes-volumes].

## <a name="creating-a-file-share"></a>Een bestandsshare maken

Een bestaande Azure-bestandsshare kan worden gebruikt met Azure Container Service. Als u maken wilt, gebruikt u de volgende reeks opdrachten.

Maak een resourcegroep voor de Azure File share met de [az groep maken] [ az-group-create] opdracht. De resourcegroep van de storage-account en het cluster Kubernetes moet zich bevinden in dezelfde regio.

```azurecli-interactive
az group create --name myResourceGroup --location westus2
```

Gebruik de [az storage-account maken] [ az-storage-create] opdracht voor het maken van een Azure Storage-account. Naam van het opslagaccount moet uniek zijn. Werk de waarde van de `--name` argument met een unieke waarde.

```azurecli-interactive
az storage account create --name mystorageaccount --resource-group myResourceGroup --sku Standard_LRS
```

Gebruik de [lijst met opslagaccounts die sleutels az ] [ az-storage-key-list] opdracht voor het retourneren van de opslagsleutel. Werk de waarde van de `--account-name` argument met de unieke opslagaccountnaam.

Let op één van de belangrijkste waarden, dit wordt gebruikt bij volgende stappen.

```azurecli-interactive
az storage account keys list --account-name mystorageaccount --resource-group myResourceGroup --output table
```

Gebruik de [az storage-share maken] [ az-storage-share-create] opdracht om de Azure-bestandsshare te maken. Update de `--account-key` waarde met de waarde in de laatste stap verzameld.

```azurecli-interactive
az storage share create --name myfileshare --account-name mystorageaccount --account-key <key>
```

## <a name="create-kubernetes-secret"></a>Kubernetes geheim maken

Kubernetes moet referenties voor toegang tot de bestandsshare. In plaats van de Azure Storage-accountnaam en de sleutel met elke schil worden opgeslagen, wordt deze opgeslagen eenmaal in een [Kubernetes geheim] [ kubernetes-secret] en waarnaar wordt verwezen door elk volume van de Azure-bestanden. 

De waarden in een Kubernetes geheime manifest moet base64-gecodeerd. Gebruik de volgende opdrachten om gecodeerde waarden te retourneren.

Codeer eerst de naam van het opslagaccount. Vervang `storage-account` met de naam van uw Azure storage-account.

```azurecli-interactive
echo -n <storage-account> | base64
```

Vervolgens wordt is de toegangssleutel voor opslagaccount vereist. Voer de volgende opdracht om te retourneren van de gecodeerde sleutel. Vervang `storage-key` met de sleutel die wordt verzameld in een eerdere stap

```azurecli-interactive
echo -n <storage-key> | base64
```

Maak een bestand met de naam `azure-secret.yml` en kopieer de volgende YAML. Update de `azurestorageaccountname` en `azurestorageaccountkey` waarden met de base64-gecodeerd waarden in de laatste stap opgehaald.

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

Gebruik de [kubectl toepassen] [ kubectl-apply] opdracht voor het maken van het geheim.

```azurecli-interactive
kubectl apply -f azure-secret.yml
```

## <a name="mount-file-share-as-volume"></a>Bestandsshare als een volume koppelen

U kunt de share van uw Azure-bestanden in uw schil koppelen door het configureren van het volume in de specificatie. Maak een nieuw bestand met de naam `azure-files-pod.yml` met de volgende inhoud. Update `share-name` met de naam van de Azure-bestanden delen.

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
      shareName: <share-name>
      readOnly: false
```

Kubectl gebruik te maken van een schil.

```azurecli-interactive
kubectl apply -f azure-files-pod.yml
```

U hebt nu een actieve container met uw Azure-bestandsshare gekoppeld in de `/mnt/azure` directory. U kunt zien dat het volume dat is gekoppeld bij de inspectie van uw schil via `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes volumes met behulp van Azure-bestanden.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure-bestanden](https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md)

<!-- LINKS -->
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
[kubectl-apply]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#apply
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[az-group-create]: /cli/azure/group#az_group_create
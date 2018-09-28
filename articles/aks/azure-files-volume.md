---
title: Maak een statische volume voor meerdere schillen zijn in Azure Kubernetes Service (AKS)
description: Meer informatie over het handmatig maken van een volume met Azure Files voor gebruik met meerdere gelijktijdige schillen in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: e5518ebb2985635507368943774e6be803cfffa8
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47409048"
---
# <a name="manually-create-and-use-an-azure-files-share-in-azure-kubernetes-service-aks"></a>Handmatig maken en gebruiken van een Azure-bestandsshare in Azure Kubernetes Service (AKS)

Op containers gebaseerde toepassingen moeten vaak voor toegang tot en behoud van gegevens in een volume dat externe gegevens. Als meerdere schillen gelijktijdige toegang tot de dezelfde opslagvolume moeten, kunt u Azure Files verbinding maken met behulp van de [Server Message Block (SMB)-protocol][smb-overview]. Dit artikel ziet u hoe u handmatig maken van een Azure-bestandsshare en deze koppelen aan een schil in AKS.

Zie voor meer informatie over Kubernetes volumes [Kubernetes volumes][kubernetes-volumes].

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

U ook moet de Azure CLI versie 2.0.46 of later geïnstalleerd en geconfigureerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][install-azure-cli].

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken

Voordat u Azure Files als een Kubernetes-volume gebruiken kunt, moet u een Azure Storage-account en de bestandsshare maken. Het volgende script maakt een resourcegroep met de naam *myAKSShare*, een opslagaccount en een bestandsshare met de naam *aksshare*:

```azurecli
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

Noteer de naam van het opslagaccount en de sleutel die wordt weergegeven aan het einde van de uitvoer van het script. Deze waarden zijn nodig als u het Kubernetes-volume maken in een van de volgende stappen uit.

## <a name="create-a-kubernetes-secret"></a>Een Kubernetes-geheim maken

Kubernetes moet referenties voor toegang tot de bestandsshare in de vorige stap hebt gemaakt. Deze referenties worden opgeslagen een [Kubernetes-geheim][kubernetes-secret], die bij het maken van een Kubernetes-schil wordt verwezen.

Gebruik de `kubectl create secret` opdracht om het geheim te maken. Het volgende voorbeeld wordt een gedeeld met de naam *azure-secret*. Vervang *STORAGE_ACCOUNT_NAME* met de naam van uw opslagaccount wordt weergegeven in de uitvoer van de vorige stap en *STORAGE_ACCOUNT_KEY* met de opslagsleutel van uw:

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=STORAGE_ACCOUNT_KEY
```

## <a name="mount-the-file-share-as-a-volume"></a>Koppel de bestandsshare als volume

Voor het koppelen van de Azure-bestandsshare in uw schil, configureert u het volume in de container-specificaties. Maak een nieuw bestand met de naam `azure-files-pod.yaml` met de volgende inhoud. Als u de naam van het delen van bestanden of de geheime naam gewijzigd, werkt u de *shareName* en *secretName*. Als gewenst is, werkt u de `mountPath`, dit is het pad waar de bestanden delen in de schil is gekoppeld.

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

Gebruik de `kubectl` opdracht voor het maken van de schil.

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

U hebt nu een actieve schil met een Azure-bestandsshare aan gekoppeld */mnt/azure*. U kunt `kubectl describe pod azure-files-pod` om te controleren of de share is gekoppeld.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over AKS clusters communiceren met Azure Files, de [Kubernetes-invoegtoepassing voor Azure Files][kubernetes-files].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli

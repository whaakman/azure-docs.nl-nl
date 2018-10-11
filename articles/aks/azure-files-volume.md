---
title: Maak een statische volume voor meerdere schillen zijn in Azure Kubernetes Service (AKS)
description: Meer informatie over het handmatig maken van een volume met Azure Files voor gebruik met meerdere gelijktijdige schillen in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: iainfou
ms.openlocfilehash: 1a8609dbf5fa1c1e7d5f4e35b081ecaa09994eb6
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068074"
---
# <a name="manually-create-and-use-a-volume-with-azure-files-share-in-azure-kubernetes-service-aks"></a>Handmatig maken en gebruiken van een volume met de Azure-bestandsshare in Azure Kubernetes Service (AKS)

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

Gebruik de `kubectl create secret` opdracht om het geheim te maken. Het volgende voorbeeld wordt een gedeeld met de naam *azure-secret* en vult de *azurestorageaccountname* en *azurestorageaccountkey* uit de vorige stap. Geef de accountnaam en de sleutel voor het gebruik van een bestaand Azure storage-account.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$AKS_PERS_STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=$STORAGE_KEY
```

## <a name="mount-the-file-share-as-a-volume"></a>Koppel de bestandsshare als volume

Voor het koppelen van de Azure-bestandsshare in uw schil, configureert u het volume in de container-specificaties. Maak een nieuw bestand met de naam `azure-files-pod.yaml` met de volgende inhoud. Als u de naam van het delen van bestanden of de geheime naam gewijzigd, werkt u de *shareName* en *secretName*. Als gewenst is, werkt u de `mountPath`, dit is het pad waar de bestanden delen in de schil is gekoppeld.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.5
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
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

U hebt nu een actieve schil met een Azure-bestandsshare aan gekoppeld */mnt/azure*. U kunt `kubectl describe pod mypod` om te controleren of de share is gekoppeld. De uitvoer van de volgende verkorte voorbeeld ziet u het volume is gekoppeld in de container:

```
Containers:
  mypod:
    Container ID:   docker://86d244cfc7c4822401e88f55fd75217d213aa9c3c6a3df169e76e8e25ed28166
    Image:          nginx:1.15.5
    Image ID:       docker-pullable://nginx@sha256:9ad0746d8f2ea6df3a17ba89eca40b48c47066dfab55a75e08e2b70fc80d929e
    State:          Running
      Started:      Mon, 08 Oct 2018 19:28:34 +0000
    Ready:          True
    Mounts:
      /mnt/azure from azure (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-z5sd7 (ro)
[...]
Volumes:
  azure:
    Type:        AzureFile (an Azure File Service mount on the host and bind mount to the pod)
    SecretName:  azure-secret
    ShareName:   aksshare
    ReadOnly:    false
  default-token-z5sd7:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-z5sd7
[...]
```

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

---
title: Azure-bestand met AKS gebruiken
description: Azure-schijven met AKS gebruiken
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/06/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a5126bc4c5e7c9cd9832f33fc908e6c8b9e02b91
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="persistent-volumes-with-azure-files"></a>Permanente volumes met Azure-bestanden

Een permanente volume vertegenwoordigt een onderdeel van de opslag die is ingericht voor gebruik in een cluster Kubernetes. Een permanente volume kan worden gebruikt door een of meer gehele product en worden dynamisch of statisch ingericht. In dit document worden dynamisch inrichten van een Azure-bestandsshare als een permanente volume Kubernetes in een cluster AKS. 

Zie voor meer informatie over Kubernetes permanente volumes [Kubernetes permanente volumes][kubernetes-volumes].

## <a name="create-storage-account"></a>Een opslagaccount maken

Bij het dynamisch inrichten van een Azure-bestandsshare als een volume Kubernetes, kan een opslagaccount worden gebruikt als deze is opgenomen in dezelfde resourcegroep bevinden als het cluster AKS. Maak een opslagaccount in dezelfde resourcegroep bevinden als het cluster AKS indien nodig. 

Om de juiste resourcegroep hebt geïdentificeerd, gebruiken de [az groepslijst] [ az-group-list] opdracht.

```azurecli-interactive
az group list --output table
```

Zoekt u naar een resourcegroep met een naam die lijkt op `MC_clustername_clustername_locaton`, waarbij clustername de naam van uw cluster AKS en locatie van de Azure-regio waar het cluster is geïmplementeerd.

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Gebruik de [az storage-account maken] [ az-storage-account-create] opdracht voor het maken van het opslagaccount. 

Het volgende voorbeeld bijwerken `--resource-group` met de naam van de resourcegroep en `--name` in een naam van uw keuze.

```azurecli-interactive
az storage account create --resource-group MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Opslagklasse maken

Een opslagklasse wordt gebruikt om te definiëren hoe een Azure-bestandsshare wordt gemaakt. Een specifieke storage-account kan worden opgegeven in de klasse. Als u een opslagaccount niet is opgegeven, een `skuName` en `location` moet worden opgegeven, en alle opslagaccounts in de gekoppelde resourcegroep worden geëvalueerd voor een overeenkomst.

Zie voor meer informatie over Kubernetes Opslagklassen voor Azure files [Kubernetes Opslagklassen][kubernetes-storage-classes].

Maak een bestand met de naam `azure-file-sc.yaml` en kopieer het volgende manifest. Update de `storageAccount` met de naam van uw doelopslagaccount.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: mystorageaccount
```

Maken van de opslagklasse met de [kubectl maken] [ kubectl-create] opdracht.

```azurecli-interactive
kubectl create -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Permanente volume claim maken

Een claim permanente volume (PVC) maakt gebruik van het opslagobject klasse te richten op dynamische wijze een Azure-bestandsshare. 

Het volgende manifest kan worden gebruikt voor het maken van een claim permanente volume `5GB` aan de grootte van `ReadWriteOnce` toegang.

Maak een bestand met de naam `azure-file-pvc.yaml` en kopieer het volgende manifest. Zorg ervoor dat de `storageClassName` overeenkomt met de opslagklasse in de vorige stap hebt gemaakt.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Maken van de claim permanente volume met de [kubectl maken] [ kubectl-create] opdracht.

```azurecli-interactive
kubectl create -f azure-file-pvc.yaml
```

Zodra de voltooid, kunt u de bestandsshare wordt gemaakt. Een geheim Kubernetes wordt ook gemaakt waarin de verbindingsgegevens en referenties.

## <a name="using-the-persistent-volume"></a>Met behulp van de permanente volume

Het volgende manifest maakt een schil die gebruikmaakt van de claim permanente volume `azurefile` koppelen van de Azure-bestandsshare op de `/mnt/azure` pad.

Maak een bestand met de naam `azure-pvc-files.yaml`, en kopieer het volgende manifest. Zorg ervoor dat de `claimName` overeenkomt met het PVC in de vorige stap hebt gemaakt.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

Maken van de schil met de [kubectl maken] [ kubectl-create] opdracht.

```azurecli-interactive
kubectl create -f azure-pvc-files.yaml
```

U hebt nu een actieve schil met uw Azure-schijf gekoppeld in de `/mnt/azure` directory. U kunt zien dat het volume dat is gekoppeld bij de inspectie van uw schil via `kubectl describe pod mypod`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes permanente volumes met behulp van Azure-bestanden.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure-bestanden][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create

---
title: Azure-schijven met AKS gebruiken
description: Azure-schijven met AKS gebruiken
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: cfad5ebd1212df03cee86b71340d8a73c2594f57
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096090"
---
# <a name="volumes-with-azure-disks"></a>Volumes met Azure-schijven

Container gebaseerde toepassingen moeten vaak voor toegang tot en het behouden van gegevens in een externe gegevensbron-volume. Azure-schijven kunnen worden gebruikt als deze externe gegevensarchief. Dit artikel gegevens met een Azure-schijf als een volume Kubernetes in een Azure Kubernetes Service (AKS)-cluster.

Zie voor meer informatie over Kubernetes volumes [Kubernetes volumes][kubernetes-volumes].

## <a name="create-an-azure-disk"></a>Een Azure-schijf maken

Voordat het koppelen van een beheerde Azure-schijf als een volume Kubernetes de schijf moet aanwezig zijn in de AKS **knooppunt** resourcegroep. Ophalen van de naam van de resourcegroep met de [az resource weergeven] [ az-resource-show] opdracht.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Gebruik de [az schijf maken] [ az-disk-create] opdracht voor het maken van de schijf van Azure.

Update `--resource-group` verzameld met de naam van de resourcegroep in de laatste stap en `--name` in een naam van uw keuze.

```azurecli-interactive
az disk create \
  --resource-group MC_myResourceGroup_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

Als de schijf is gemaakt, ziet u uitvoer ziet er als volgt. Deze waarde is de schijf-ID wordt gebruikt bij het koppelen van de schijf.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

## <a name="mount-disk-as-volume"></a>Schijf als een volume koppelen

Koppel de Azure-schijf in uw schil door het configureren van het volume in de container-specificaties.

Maak een nieuw bestand met de naam `azure-disk-pod.yaml` met de volgende inhoud. Update `diskName` met de naam van de nieuwe schijf en `diskURI` met de id van de schijf. Let ook op de `mountPath`, dit is het pad waar de schijf van Azure in schil is gekoppeld.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-disk-pod
spec:
 containers:
  - image: microsoft/sample-aks-helloworld
    name: azure
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
 volumes:
      - name: azure
        azureDisk:
          kind: Managed
          diskName: myAKSDisk
          diskURI: /subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

Gebruik kubectl schil maken.

```azurecli-interactive
kubectl apply -f azure-disk-pod.yaml
```

U hebt nu een actieve schil met een Azure schijf die is gekoppeld aan de `/mnt/azure`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes volumes met behulp van Azure-schijven.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure-schijven][kubernetes-disks]

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az_disk_list
[az-disk-create]: /cli/azure/disk#az_disk_create
[az-group-list]: /cli/azure/group#az_group_list
[az-resource-show]: /cli/azure/resource#az-resource-show

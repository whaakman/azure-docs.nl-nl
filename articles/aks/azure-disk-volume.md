---
title: Azure-schijven gebruiken met AKS
description: Azure-schijven gebruiken met AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: f807264dc2c2e07ccd175fb1b0427b7ce9e9f524
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868242"
---
# <a name="volumes-with-azure-disks"></a>Volumes met Azure-schijven

Op containers gebaseerde toepassingen moeten vaak voor toegang tot en behoud van gegevens in een volume dat externe gegevens. Azure-schijven kunnen worden gebruikt als deze extern gegevensarchief. Dit artikel vindt u met behulp van een Azure-schijf als een Kubernetes-volume in een cluster Azure Kubernetes Service (AKS).

Zie voor meer informatie over Kubernetes volumes [Kubernetes volumes][kubernetes-volumes].

## <a name="create-an-azure-disk"></a>Maken van een Azure-schijf

Voor het koppelen van een Azure-beheerde schijf als een Kubernetes-volume, de schijf moet aanwezig zijn in de AKS **knooppunt** resourcegroep. De naam van de resource met de [az resource show] [ az-resource-show] opdracht.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Gebruik de [az schijf maken] [ az-disk-create] opdracht voor het maken van de Azure-schijf.

Update `--resource-group` verzameld door de naam van de resourcegroep in de vorige stap en `--name` naar een naam van uw keuze.

```azurecli-interactive
az disk create \
  --resource-group MC_myResourceGroup_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

Zodra de schijf is gemaakt, ziet u uitvoer die vergelijkbaar is met de volgende. Deze waarde is de schijf-ID, die wordt gebruikt tijdens het koppelen van de schijf.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```
> [!NOTE]
> Azure-beheerde schijven worden gefactureerd op basis van de SKU voor een specifieke grootte. Deze SKU's variëren van 32GiB voor S4 of P4 schijven tot 4TiB voor S50 of P50 schijven. Daarnaast wordt de doorvoer en IOPS-prestaties van een Premium-beheerde schijf is afhankelijk van de SKU en de exemplaargrootte van de knooppunten in het AKS-cluster. Zie [prijzen en de prestaties van beheerde schijven][managed-disk-pricing-performance].

> [!NOTE]
> Als u de schijf in een afzonderlijke resourcegroep te maken wilt, moet u ook de Azure Kubernetes Service (AKS) service-principal voor uw cluster toevoegen aan de resourcegroep met de schijf met de `Contributor` rol. 
>

## <a name="mount-disk-as-volume"></a>Schijf als een volume koppelen

Koppel de Azure-schijf in uw schil door het configureren van het volume in de container-specificaties.

Maak een nieuw bestand met de naam `azure-disk-pod.yaml` met de volgende inhoud. Update `diskName` met de naam van de zojuist gemaakte schijf en `diskURI` met de id van de schijf. Let ook op de `mountPath`, dit is het pad waar de Azure-schijf in de schil is gekoppeld.

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

Kubectl gebruiken om te maken van de schil.

```azurecli-interactive
kubectl apply -f azure-disk-pod.yaml
```

U hebt nu een actieve schil met een Azure-schijf geplaatst in de `/mnt/azure`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes-volumes met behulp van Azure-schijven.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure-schijven][kubernetes-disks]

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az_disk_list
[az-disk-create]: /cli/azure/disk#az_disk_create
[az-group-list]: /cli/azure/group#az_group_list
[az-resource-show]: /cli/azure/resource#az-resource-show

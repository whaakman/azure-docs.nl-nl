---
title: Maak een statische volume voor de schillen in Azure Kubernetes Service (AKS)
description: Meer informatie over het handmatig maken van een volume met de Azure-schijven voor gebruik met een schil in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: iainfou
ms.openlocfilehash: 9c5879474568885d9a705e7bfd16e2a4e2304b96
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068177"
---
# <a name="manually-create-and-use-a-volume-with-azure-disks-in-azure-kubernetes-service-aks"></a>Handmatig maken en gebruiken van een volume met de Azure-schijven in Azure Kubernetes Service (AKS)

Op containers gebaseerde toepassingen moeten vaak voor toegang tot en behoud van gegevens in een volume dat externe gegevens. Als één pod nodig heeft voor toegang tot opslag, kunt u Azure-schijven kunt gebruiken om weer te geven van een systeemeigen volume voor gebruik van de toepassing. Dit artikel ziet u hoe u handmatig een Azure-schijf maakt en deze koppelen aan een schil in AKS.

> [!NOTE]
> Een Azure-schijf kan alleen worden gekoppeld aan één pod op een tijdstip. Als u een permanent volume delen tussen meerdere schillen zijn wilt, gebruikt u [Azure Files][azure-files-volume].

Zie voor meer informatie over Kubernetes volumes [Kubernetes volumes][kubernetes-volumes].

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

U ook moet de Azure CLI versie 2.0.46 of later geïnstalleerd en geconfigureerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][install-azure-cli].

## <a name="create-an-azure-disk"></a>Maken van een Azure-schijf

Wanneer u een Azure-schijf voor gebruik met AKS maakt, kunt u de resource van de schijf in de **knooppunt** resourcegroep. Met deze methode kan het AKS-cluster openen en beheren van de resource van de schijf. Als u in plaats daarvan de schijf in een afzonderlijke resourcegroep maakt, moet u de service-principal voor Azure Kubernetes Service (AKS) verlenen voor uw cluster de `Contributor` rol aan de resourcegroep van de schijf.

In dit artikel de schijf te maken in de resourcegroep van het knooppunt. Haal eerst de naam van de resourcegroep met de [az aks show] [ az-aks-show] opdracht en voeg de `--query nodeResourceGroup` queryparameter. Het volgende voorbeeld wordt de resourcegroep van het knooppunt voor de naam van het AKS-cluster *myAKSCluster* in naam van de resourcegroep *myResourceGroup*:

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Maak nu een schijf met de [az schijf maken] [ az-disk-create] opdracht. Geef de Resourcegroepnaam van knooppunt hebt verkregen in de vorige opdracht, en vervolgens een naam voor de resource van de schijf, zoals *myAKSDisk*. Het volgende voorbeeld wordt een *20*GiB schijf- en uitvoer de ID van de schijf eenmaal hebt gemaakt:

```azurecli-interactive
az disk create \
  --resource-group MC_myResourceGroup_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

> [!NOTE]
> Azure-schijven worden gefactureerd op basis van de SKU voor een specifieke grootte. Deze SKU's variëren van 32GiB voor S4 of P4 schijven tot 8TiB voor S60 of P60 schijven. De doorvoer en IOPS-prestaties van een Premium-beheerde schijf is afhankelijk van de SKU en de exemplaargrootte van de knooppunten in het AKS-cluster. Zie [prijzen en de prestaties van beheerde schijven][managed-disk-pricing-performance].

De schijf resource-ID wordt weergegeven wanneer de opdracht is voltooid, zoals wordt weergegeven in de volgende voorbeelduitvoer. Deze schijf-ID wordt gebruikt voor het koppelen van de schijf in de volgende stap.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

## <a name="mount-disk-as-volume"></a>Schijf als een volume koppelen

Voor het koppelen van de Azure-schijf in uw schil, configureert u het volume in de container-specificaties. Maak een nieuw bestand met de naam `azure-disk-pod.yaml` met de volgende inhoud. Update `diskName` met de naam van de schijf in de vorige stap hebt gemaakt en `diskURI` -opdracht met de ID van de schijf die wordt weergegeven in de uitvoer van de schijf te maken. Als gewenst is, werkt u de `mountPath`, dit is het pad waar de Azure-schijf in de schil is gekoppeld.

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
        azureDisk:
          kind: Managed
          diskName: myAKSDisk
          diskURI: /subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

Gebruik de `kubectl` opdracht voor het maken van de schil.

```console
kubectl apply -f azure-disk-pod.yaml
```

U hebt nu een actieve schil met een Azure-schijf geplaatst in `/mnt/azure`. U kunt `kubectl describe pod mypod` om te controleren of de schijf met succes is gekoppeld. De uitvoer van de volgende verkorte voorbeeld ziet u het volume is gekoppeld in de container:

```
[...]
Volumes:
  azure:
    Type:         AzureDisk (an Azure Data Disk mount on the host and bind mount to the pod)
    DiskName:     myAKSDisk
    DiskURI:      /subscriptions/<subscriptionID/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
    Kind:         Managed
    FSType:       ext4
    CachingMode:  ReadWrite
    ReadOnly:     false
  default-token-z5sd7:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-z5sd7
    Optional:    false
[...]
Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              1m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-z5sd7"
  Normal  SuccessfulMountVolume  41s   kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "azure"
[...]
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over AKS clusters communiceren met Azure-schijven, Zie de [Kubernetes-invoegtoepassing voor Azure Disks][kubernetes-disks].

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/resource#az-resource-show
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[install-azure-cli]: /cli/azure/install-azure-cli
[azure-files-volume]: azure-files-volume.md
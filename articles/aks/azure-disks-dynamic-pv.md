---
title: Permanente volumes maken met Azure Kubernetes Service
description: Informatie over het gebruik van Azure-schijven permanente om volumes te maken voor de schillen in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/20/2018
ms.author: iainfou
ms.openlocfilehash: 129a39294d52a312c91fc6a4fd009d76e88b4ff7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185219"
---
# <a name="create-persistent-volumes-with-azure-disks-for-azure-kubernetes-service-aks"></a>Permanente volumes maken met Azure-schijven voor Azure Kubernetes Service (AKS)

Een permanent volume vertegenwoordigt een stukje opslag die is ingericht voor gebruik met Kubernetes-schillen. Een permanent volume kan worden gebruikt door een of meer schillen en kan worden dynamisch of statisch ingericht. Zie voor meer informatie over Kubernetes permanente volumes [Kubernetes permanente volumes][kubernetes-volumes]. In dit artikel leest u hoe het gebruik van permanente volumes met Azure-schijven in een cluster Azure Kubernetes Service (AKS).

> [!NOTE]
> Een Azure-schijf kan alleen worden gekoppeld met *toegangsmodus* type *ReadWriteOnce*, waardoor het beschikbaar voor slechts één AKS-knooppunt. Als dat het delen van een permanent volume op meerdere knooppunten, kunt u overwegen [Azure Files][azure-files-pvc].

## <a name="built-in-storage-classes"></a>Ingebouwde Opslagklassen

Een opslagklasse wordt gebruikt om te definiëren hoe een opslageenheid dynamisch wordt gemaakt met een permanent volume. Zie voor meer informatie over Kubernetes-Opslagklassen [Kubernetes Opslagklassen][kubernetes-storage-classes].

Een AKS-cluster bevat twee vooraf gemaakte Opslagklassen, beide geconfigureerd om te werken met Azure-schijven:

* De *standaard* opslagklasse richt een standaardschijf in Azure.
    * Standard-opslag wordt ondersteund door HDD's en biedt een rendabele opslag terwijl u nog steeds goed presterende. Standard-schijven zijn ideaal voor een kostenefficiënte werkbelasting voor ontwikkelen en testen.
* De *beheerde premium* opslagklasse richt een premium Azure-schijf.
    * Premium-schijven worden ondersteund door hoogwaardige schijven met een lage latentie op basis van SSD. Ideaal voor virtuele machines met een productiewerkbelasting. Als de AKS-knooppunten in uw cluster premiumopslag gebruikt, selecteert u de *beheerde premium* klasse.

Gebruik de [kubectl ophalen sc] [ kubectl-get] opdracht om te zien van de vooraf gemaakte Opslagklassen. Het volgende voorbeeld wordt de Opslagklassen die beschikbaar zijn in een AKS-cluster vooraf te maken:

```
$ kubectl get sc

NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

> [!NOTE]
> Permanent volume claims zijn opgegeven in GiB maar Azure managed disks worden gefactureerd op basis van de SKU voor een specifieke grootte. Deze SKU's variëren van 32GiB voor S4 of P4 schijven tot 4TiB voor S50 of P50 schijven. De doorvoer en IOPS-prestaties van een Premium-beheerde schijf is afhankelijk van zowel de SKU en de exemplaargrootte van de knooppunten in het AKS-cluster. Zie voor meer informatie, [prijs en prestaties van Managed Disks][managed-disk-pricing-performance].

## <a name="create-a-persistent-volume-claim"></a>Maken van een claim permanent volume

Een claim permanent volume (PVC) wordt gebruikt voor het automatisch inrichten van opslag op basis van een storage-klasse. In dit geval een PVC kunt een van de vooraf gemaakte Opslagklassen te maken van een Azure standard- of premium beheerde schijven.

Maak een bestand met de naam `azure-premium.yaml`, en kopieer de volgende manifest. De claim vraagt een schijf met de naam `azure-managed-disk` dat wil zeggen *5GB* in grootte met *ReadWriteOnce* toegang. De *beheerde premium* opslagklasse wordt opgegeven als de opslagklasse.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

> [!TIP]
> Gebruik voor het maken van een schijf die gebruikmaakt van standard-opslag `storageClassName: default` in plaats van *beheerde premium*.

Maken van de claim permanent volume met de [kubectl maken] [ kubectl-create] opdracht en geeft u uw *azure premium.yaml* bestand:

```
$ kubectl create -f azure-premium.yaml

persistentvolumeclaim/azure-managed-disk created
```

## <a name="use-the-persistent-volume"></a>Gebruik de permanent volume

Als de claim permanent volume is gemaakt en de schijf is ingericht, een schil kan worden gemaakt met toegang tot de schijf. De volgende manifest maakt u een eenvoudige NGINX-schil die gebruikmaakt van de permanent volume claim met de naam *azure-beheerde schijf* te koppelen van de Azure-schijf in het pad `/mnt/azure`.

Maak een bestand met de naam `azure-pvc-disk.yaml`, en kopieer de volgende manifest.

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
        claimName: azure-managed-disk
```

Maken van de schil met de [kubectl maken] [ kubectl-create] opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```
$ kubectl create -f azure-pvc-disk.yaml

pod/mypod created
```

U hebt nu een pod uitgevoerd met uw Azure-schijf is gekoppeld in de `/mnt/azure` directory. Deze configuratie kan worden weergegeven bij de inspectie van uw schil via `kubectl describe pod mypod`, zoals weergegeven in het volgende verkorte voorbeeld:

```
$ kubectl describe pod mypod

[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azure-managed-disk
    ReadOnly:   false
  default-token-smm2n:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-smm2n
    Optional:    false

Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              2m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  2m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-smm2n"
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "pvc-faf0f176-8b8d-11e8-923b-deb28c58d242"
[...]
```

## <a name="back-up-a-persistent-volume"></a>Back-up van een permanent volume

Als u wilt back-up van de gegevens in uw permanent volume, een momentopname van de beheerde schijf voor het volume uit te voeren. U kunt vervolgens deze momentopname te maken van een herstelde schijf koppelen aan schillen als middel om te herstellen van de gegevens.

Haal eerst de naam van het volume met de `kubectl get pvc` opdracht, zoals voor het Circuit met de naam *azure-beheerde schijf*:

```
$ kubectl get pvc azure-managed-disk

NAME                 STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS      AGE
azure-managed-disk   Bound     pvc-faf0f176-8b8d-11e8-923b-deb28c58d242   5Gi        RWO            managed-premium   3m
```

De naam van dit volume vormt de naam van de onderliggende Azure-schijf. Query voor de schijf-ID met [az Schijflijst] [ az-disk-list] en geef de naam van uw PVC-volume, zoals wordt weergegeven in het volgende voorbeeld:

```
$ az disk list --query '[].id|[?contains(@,`pvc-faf0f176-8b8d-11e8-923b-deb28c58d242`)]' -o tsv

/subscriptions/<guid>/resourceGroups/MC_MYRESOURCEGROUP_MYAKSCLUSTER_EASTUS/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

De schijf-ID gebruiken om te maken van een momentopname-schijf met [maken van een momentopname van de az][az-snapshot-create]. Het volgende voorbeeld wordt een momentopname met de naam *pvcSnapshot* in dezelfde resourcegroep bevinden als het AKS-cluster (*MC_myResourceGroup_myAKSCluster_eastus*). U kunt problemen met machtigingen kan optreden als u momentopnamen maken en herstellen van schijven in resourcegroepen die het AKS-cluster heeft geen toegang tot.

```azurecli
$ az snapshot create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name pvcSnapshot \
    --source /subscriptions/<guid>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

Afhankelijk van de hoeveelheid gegevens op de schijf duurt het enkele minuten om de momentopname te maken.

## <a name="restore-and-use-a-snapshot"></a>Herstellen en een momentopname gebruiken

Als u wilt de schijf herstellen en deze gebruiken met een Kubernetes-schil, gebruikt u de momentopname als een bron bij het maken van een schijf met [az schijf maken][az-disk-create]. Met deze bewerking blijft de oorspronkelijke bron behouden als u vervolgens nodig hebt voor toegang tot de oorspronkelijke gegevens te maken. Het volgende voorbeeld wordt een schijf met de naam *pvcRestored* vanuit de momentopname met de naam *pvcSnapshot*:

```azurecli
az disk create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --source pvcSnapshot
```

Voor het gebruik van de herstelde schijf met een schil, geeft u de ID van de schijf in het manifest. Haal de ID van de schijf met de [az disk show] [ az-disk-show] opdracht. Het volgende voorbeeld wordt de schijf-ID voor *pvcRestored* in de vorige stap hebt gemaakt:

```azurecli
az disk show --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --query id -o tsv
```

Maken van een pod-manifest met de naam `azure-restored.yaml` en geeft u de schijf-URI in de vorige stap hebt verkregen. Het volgende voorbeeld wordt een standaard NGINX-webserver met de herstelde schijf gekoppeld als een volume op */mnt/azure*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypodrestored
spec:
  containers:
    - name: myfrontendrestored
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      azureDisk:
        kind: Managed
        diskName: pvcRestored
        diskURI: /subscriptions/<guid>/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
```

Maken van de schil met de [kubectl maken] [ kubectl-create] opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```
$ kubectl create -f azure-restored.yaml

pod/mypodrestored created
```

U kunt `kubectl describe pod mypodrestored` om details van de schil, zoals het volgende verkorte voorbeeld, waarin de volume-informatie weer te geven:

```
$ kubectl describe pod mypodrestored

[...]
Volumes:
  volume:
    Type:         AzureDisk (an Azure Data Disk mount on the host and bind mount to the pod)
    DiskName:     pvcRestored
    DiskURI:      /subscriptions/19da35d3-9a1a-4f3b-9b9c-3c56ef409565/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
    Kind:         Managed
    FSType:       ext4
    CachingMode:  ReadWrite
    ReadOnly:     false
[...]
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes permanente volumes met behulp van Azure-schijven.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure-schijven][azure-disk-volume]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-snapshot-create]: /cli/azure/snapshot#az-snapshot-create
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-disk-show]: /cli/azure/disk#az-disk-show

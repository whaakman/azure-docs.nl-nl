---
title: Permanente volumes maken met Azure Kubernetes Service
description: Informatie over het gebruik van Azure-schijven permanente om volumes te maken voor de schillen in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/10/2018
ms.author: iainfou
ms.openlocfilehash: 34d3a5dbccf2cad7873bf6166e406c7c4817ac09
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158706"
---
# <a name="create-persistent-volumes-with-azure-disks-for-azure-kubernetes-service-aks"></a>Permanente volumes maken met Azure-schijven voor Azure Kubernetes Service (AKS)

Een permanent volume vertegenwoordigt een stukje opslag die is ingericht voor gebruik met Kubernetes-schillen. Een permanent volume kan worden gebruikt door een of meer schillen en kan worden dynamisch of statisch ingericht. Zie voor meer informatie over Kubernetes permanente volumes [Kubernetes permanente volumes][kubernetes-volumes]. In dit artikel leest u hoe het gebruik van permanente volumes met Azure-schijven in een cluster Azure Kubernetes Service (AKS).

> [!NOTE]
> Een Azure-schijf kan alleen worden gekoppeld met *toegangsmodus* type *ReadWriteOnce*, waardoor het beschikbaar voor slechts één AKS-knooppunt. Als dat het delen van een permanent volume op meerdere knooppunten, kunt u overwegen [Azure Files][azure-files-pvc].

## <a name="built-in-storage-classes"></a>Ingebouwde Opslagklassen

Een opslagklasse wordt gebruikt om te definiëren hoe een opslageenheid dynamisch wordt gemaakt met een permanent volume. Zie voor meer informatie over Kubernetes-Opslagklassen [Kubernetes Opslagklassen][kubernetes-storage-classes].

Een AKS-cluster bevat twee vooraf gemaakte Opslagklassen, beide geconfigureerd om te werken met Azure-schijven. De *standaard* opslagklasse richt een standaardschijf in Azure. De *beheerde premium* opslagklasse richt een premium Azure-schijf. Als de AKS-knooppunten in uw cluster premiumopslag gebruikt, selecteert u de *beheerde premium* klasse.

Gebruik de [kubectl ophalen sc] [ kubectl-get] opdracht om te zien van de vooraf gemaakte Opslagklassen. Het volgende voorbeeld wordt de Opslagklassen die beschikbaar zijn in een AKS-cluster vooraf te maken:

```
$ kubectl get sc

NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

> [!NOTE]
> Permanent volume claims zijn opgegeven in GiB maar Azure managed disks worden gefactureerd op basis van de SKU voor een specifieke grootte. Deze SKU's variëren van 32GiB voor S4 of P4 schijven tot 4TiB voor S50 of P50 schijven. Bovendien de doorvoer en IOPS-prestaties van een Premium-beheerde schijf is afhankelijk van zowel de SKU en de exemplaargrootte van de knooppunten in het AKS-cluster. Zie voor meer informatie, [prijs en prestaties van Managed Disks][managed-disk-pricing-performance].

## <a name="create-a-persistent-volume-claim"></a>Maken van een claim permanent volume

Een claim permanent volume (PVC) wordt gebruikt voor het automatisch inrichten van opslag op basis van een storage-klasse. In dit geval een PVC kunt een van de vooraf gemaakte Opslagklassen te maken van een Azure standard- of premium beheerde schijven.

Maak een bestand met de naam `azure-premium.yaml`, en kopieer de volgende manifest.

Houd er rekening mee houden dat de *beheerde premium* opslagklasse is opgegeven in de aantekening en de claim vraagt een schijf *5GB* in grootte met *ReadWriteOnce* toegang.

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

Maken van de claim permanent volume met de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u uw *azure premium.yaml* bestand:

```console
kubectl apply -f azure-premium.yaml
```

## <a name="use-the-persistent-volume"></a>Gebruik de permanent volume

Als de claim permanent volume is gemaakt en de schijf is ingericht, een schil kan worden gemaakt met toegang tot de schijf. De volgende manifest maakt een schil die gebruikmaakt van de claim permanent volume *azure-beheerde schijf* te koppelen van de Azure-schijf op de `/mnt/azure` pad.

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

Maken van de schil met de [kubectl toepassen] [ kubectl-apply] opdracht.

```console
kubectl apply -f azure-pvc-disk.yaml
```

U hebt nu een pod uitgevoerd met uw Azure-schijf is gekoppeld in de `/mnt/azure` directory. Deze configuratie kan worden weergegeven bij de inspectie van uw schil via `kubectl describe pod mypod`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes permanente volumes met behulp van Azure-schijven.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure-schijven][azure-disk-volume]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md

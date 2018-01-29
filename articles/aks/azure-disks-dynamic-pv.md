---
title: De schijf van Azure gebruiken met AKS
description: Azure-schijven met AKS gebruiken
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/25/2018
ms.author: nepeters
ms.openlocfilehash: e1f5b68d5d39dd846ebec525d1e83a6c0ef4971a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="persistent-volumes-with-azure-disks---dynamic-provisioning"></a>Permanente volumes met Azure-schijven - dynamische inrichten

Een permanente volume vertegenwoordigt een onderdeel van de opslag die is ingericht voor gebruik in een cluster Kubernetes. Een permanente volume kan worden gebruikt door een of meer gehele product en worden dynamisch of statisch ingericht. In dit document worden dynamisch inrichten van een Azure-schijf als een permanente volume Kubernetes in een cluster AKS. 

Zie voor meer informatie over Kubernetes permanente volumes [Kubernetes permanente volumes][kubernetes-volumes].

## <a name="built-in-storage-classes"></a>Ingebouwde Opslagklassen

Een opslagklasse wordt gebruikt om te definiëren hoe een dynamisch gemaakte permanente volume is geconfigureerd. Zie voor meer informatie over Kubernetes Opslagklassen [Kubernetes Opslagklassen][kubernetes-storage-classes].

Elk cluster AKS bevat twee vooraf gemaakte Opslagklassen, beide geconfigureerd om te werken met Azure-schijven. Gebruik de `kubectl get storageclass` opdracht om te zien deze.

```console
NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

Als deze Opslagklassen voor uw behoeften werkt, hoeft u niet aan een nieuwe maken.

## <a name="create-storage-class"></a>Opslagklasse maken

Als u maken van een nieuwe opslagklasse geconfigureerd voor Azure-schijven wilt, kunt u doen met behulp van het volgende voorbeeld-manifest. 

De `storageaccounttype` waarde van `Standard_LRS` betekent dat een normale schijf wordt gemaakt. Deze waarde kan worden gewijzigd in `Premium_LRS` maken een [premium schijf][premium-storage]. Voor het gebruik van premium-schijven, moet de AKS knooppunt virtuele machine een grootte die compatibel is met premium-schijven hebben. Zie [dit document] [ premium-storage] voor een lijst met compatibele grootten.

```yaml
apiVersion: storage.k8s.io/v1beta1
kind: StorageClass
metadata:
  name: azure-managed-disk
provisioner: kubernetes.io/azure-disk
parameters:
  kind: Managed
  storageaccounttype: Standard_LRS
```

## <a name="create-persistent-volume-claim"></a>Permanente volume claim maken

Een claim permanente volume maakt gebruik van een opslagobject van de klasse te richten op dynamische wijze een stukje opslag. Wanneer u een Azure-schijf gebruikt, wordt de schijf gemaakt in dezelfde resourcegroep bevinden als de AKS resources.

Dit voorbeeld manifest maakt een permanente volume claim met de `azure-managed-disk` te maken van een schijf van opslagklasse `5GB` aan de grootte van `ReadWriteOnce` toegang. Zie voor meer informatie over de toegangsmodi PVC [toegangsmodi][access-modes].

> [!NOTE]
> Een Azure-schijf kan alleen worden gekoppeld met de modus toegangstype ReadWriteOnce waardoor beschikbaar is op slechts één AKS knooppunt. Als dat een permanente volume delen op meerdere knooppunten, overweeg dan [Azure Files][azure-files-pvc]. 

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
  annotations:
    volume.beta.kubernetes.io/storage-class: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

## <a name="using-the-persistent-volume"></a>Met behulp van de permanente volume

Nadat de claim permanente volume is gemaakt en de schijf is ingericht, een schil kan worden gemaakt met toegang tot de schijf. Het volgende manifest maakt een schil die gebruikmaakt van de claim permanente volume `azure-managed-disk` koppelen van de Azure-schijf op de `/var/www/html` pad. 

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
      - mountPath: "/var/www/html"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes permanente volumes met behulp van Azure-schijven.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure-schijven][kubernetes-disk]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubernetes-disk]: https://kubernetes.io/docs/concepts/storage/storage-classes/#new-azure-disk-storage-class-starting-from-v172
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md
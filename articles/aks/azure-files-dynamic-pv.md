---
title: Gebruik Azure-bestand met AKS
description: Azure-schijven gebruiken met AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bac80e354a4d629bfbfc8207b9fed7c69c765839
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429264"
---
# <a name="persistent-volumes-with-azure-files"></a>Permanente volumes met Azure files

Een permanent volume is een onderdeel van de opslag die is gemaakt voor gebruik in een Kubernetes-cluster. Een permanent volume kan worden gebruikt door een of meer schillen en kan worden dynamisch of statisch gemaakt. De Documentdetails van dit **maken van dynamische** van een Azure-bestandsshare als een permanent volume.

Zie voor meer informatie over Kubernetes permanente volumes, met inbegrip van het maken van statische [Kubernetes permanente volumes][kubernetes-volumes].

## <a name="create-storage-account"></a>Een opslagaccount maken

Bij het maken van een Azure-bestandsshare als een Kubernetes-volume dynamisch, een opslagaccount kan worden gebruikt als deze zich in de AKS **knooppunt** resourcegroep. Dit is de categorie met de `MC_` voorvoegsel op dat is gemaakt door de inrichting van de resources voor het AKS-cluster. De naam van de resource met de [az resource show] [ az-resource-show] opdracht.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Gebruik de [az storage-account maken] [ az-storage-account-create] opdracht om het opslagaccount te maken.

Update `--resource-group` verzameld door de naam van de resourcegroep in de vorige stap en `--name` naar een naam van uw keuze.

```azurecli-interactive
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

> Azure Files is momenteel alleen werkt alleen met standard storage. Als u premium-opslag gebruikt, mislukt het volume om in te richten.

## <a name="create-storage-class"></a>Opslagklasse maken

Een opslagklasse wordt gebruikt om te definiëren hoe een Azure-bestandsshare wordt gemaakt. Een specifiek opslagaccount kan worden opgegeven in de klasse. Als een storage-account niet opgegeven is, een `skuName` en `location` moet worden opgegeven, en alle opslagaccounts in de bijbehorende resourcegroep worden geëvalueerd voor een overeenkomst.

Zie voor meer informatie over Kubernetes-Opslagklassen voor Azure files [Kubernetes Opslagklassen][kubernetes-storage-classes].

Maak een bestand met de naam `azure-file-sc.yaml` en kopieer de volgende manifest. Update de `storageAccount` met de naam van uw doel-opslagaccount. Zie de sectie [koppelingsopties] voor meer informatie over `mountOptions`.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Maak de opslagklasse met de [kubectl toepassen] [ kubectl-apply] opdracht.

```azurecli-interactive
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Maken van de claim van permanent volume

Een claim permanent volume (PVC) maakt gebruik van het opslagobject klasse om u te richten op dynamische wijze een Azure-bestandsshare.

De volgende YAML kan worden gebruikt voor het maken van een claim permanent volume `5GB` in grootte met `ReadWriteMany` toegang. Zie voor meer informatie over toegangsmodi in de [Kubernetes permanent volume] [ access-modes] documentatie.

Maak een bestand met de naam `azure-file-pvc.yaml` en kopieer de volgende YAML. Zorg ervoor dat de `storageClassName` komt overeen met de opslagklasse in de laatste stap hebt gemaakt.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Maken van de claim permanent volume met de [kubectl toepassen] [ kubectl-apply] opdracht.

```azurecli-interactive
kubectl apply -f azure-file-pvc.yaml
```

Als voltooid, kunt u de bestandsshare wordt gemaakt. Een Kubernetes-geheim wordt ook gemaakt met verbindingsgegevens en referenties.

## <a name="using-the-persistent-volume"></a>Met behulp van de permanent volume

De volgende YAML maakt een schil die gebruikmaakt van de claim permanent volume `azurefile` te koppelen van de Azure-bestandsshare op de `/mnt/azure` pad.

Maak een bestand met de naam `azure-pvc-files.yaml`, en kopieer de volgende YAML. Zorg ervoor dat de `claimName` komt overeen met het PVC in de laatste stap hebt gemaakt.

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

Maken van de schil met de [kubectl toepassen] [ kubectl-apply] opdracht.

```azurecli-interactive
kubectl apply -f azure-pvc-files.yaml
```

U hebt nu een pod uitgevoerd met uw Azure-schijf is gekoppeld in de `/mnt/azure` directory. Deze configuratie kan worden weergegeven bij de inspectie van uw schil via `kubectl describe pod mypod`.

## <a name="mount-options"></a>Koppelingsopties

FileMode en dirMode standaardwaarden verschillen tussen de Kubernetes-versies, zoals beschreven in de volgende tabel.

| versie | waarde |
| ---- | ---- |
| V1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 of hoger | 0755 |
| v1.9.0 | 0700 |
| V1.9.1 of hoger | 0755 |

Als een cluster van versie 1.8.5 of hoger en het dynamisch maken van het volume persistant met een opslagklasse, koppelingsopties kunnen worden opgegeven voor het opslagobject klasse. Het volgende voorbeeld wordt `0777`.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Als een cluster van versie 1.8.5 of hoger en het maken van het object van het volume persistant statisch, koppelingsopties moeten worden opgegeven op de `PersistentVolume` object. Zie voor meer informatie over het maken van een volume persistant statisch [statische permanente Volumes][pv-static].

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  azureFile:
    secretName: azure-secret
    shareName: azurefile
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  ```

Als een cluster van versie 1.8.0 - 1.8.4, een beveiligingscontext kan worden opgegeven met de `runAsUser` waarde ingesteld op `0`. Zie voor meer informatie over de beveiligingscontext Pod [configureren van een beveiligingscontext][kubernetes-security-context].

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes permanente volumes met behulp van Azure Files.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure Files][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-storage-account-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[mount-options]: #mount-options

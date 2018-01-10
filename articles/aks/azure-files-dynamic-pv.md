---
title: Azure-bestand met AKS gebruiken
description: Azure-schijven met AKS gebruiken
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/04/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 4873b98c8ba4f1e574be20baebef3b6860341529
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="persistent-volumes-with-azure-files---dynamic-provisioning"></a>Permanente volumes met Azure files - dynamische inrichten

Een permanente volume vertegenwoordigt een onderdeel van de opslag die is ingericht voor gebruik in een cluster Kubernetes. Een permanente volume kan worden gebruikt door een of meer gehele product en worden dynamisch of statisch ingericht. In dit document worden dynamisch inrichten van een Azure-bestandsshare als een permanente volume Kubernetes in een cluster AKS. 

Zie voor meer informatie over Kubernetes permanente volumes [Kubernetes permanente volumes][kubernetes-volumes].

## <a name="prerequisites"></a>Vereisten

Bij het dynamisch inrichten van een Azure-bestandsshare als een volume Kubernetes, kan een opslagaccount worden gebruikt als deze is opgenomen in dezelfde resourcegroep bevinden als het cluster AKS. Maak een opslagaccount in dezelfde resourcegroep bevinden als het cluster AKS indien nodig. 

Om de juiste resourcegroep hebt geïdentificeerd, gebruiken de [az groepslijst] [ az-group-list] opdracht.

```azurecli-interactive
az group list --output table
```

De volgende voorbeelduitvoer wordt weergegeven in de resourcegroepen beide die zijn gekoppeld aan een cluster AKS. De resourcegroep met een naam, zoals *MC_myAKSCluster_myAKSCluster_eastus* de clusterbronnen AKS bevat en waarin het storage-account moet worden gemaakt. 

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Als de resourcegroep zijn vastgesteld, maken de storage-account met de [az storage-account maken] [ az-storage-account-create] opdracht.

```azurecli-interactive
az storage account create --resource-group  MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Opslagklasse maken

Een opslagklasse wordt gebruikt om te definiëren hoe een dynamisch gemaakte permanente volume is geconfigureerd. Items, zoals de naam van de Azure storage-account, SKU en regio worden gedefinieerd in het opslagobject klasse. Zie voor meer informatie over Kubernetes Opslagklassen [Kubernetes Opslagklassen][kubernetes-storage-classes].

Het volgende voorbeeld wordt opgegeven dat een opslagaccount van SKU typen `Standard_LRS` in de `eastus` regio kan worden gebruikt bij het aanvragen van opslag. 

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  skuName: Standard_LRS
```

Gebruik van een specifieke storage-account, de `storageAccount` parameter kan worden gebruikt.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: azure_storage_account_name
```

## <a name="create-persistent-volume-claim"></a>Permanente volume claim maken

Een claim permanente volume maakt gebruik van het opslagobject klasse te richten op dynamische wijze een stukje opslag. Wanneer u een Azure-bestanden, wordt een Azure-bestandsshare wordt gemaakt in de storage-account zijn geselecteerd of opgegeven in het opslagobject klasse.

>  [!NOTE]
>   Ervoor zorgen dat een geschikte storage-account is vooraf gemaakte in dezelfde resourcegroep bevinden als de clusterbronnen AKS. Deze resourcegroep heeft een naam zoals *MC_myAKSCluster_myAKSCluster_eastus*. De claim permanente volume niet inrichten van de Azure-bestandsshare als een opslagaccount is niet beschikbaar. 

Het volgende manifest kan worden gebruikt voor het maken van een claim permanente volume `5GB` aan de grootte van `ReadWriteOnce` toegang. Zie voor meer informatie over de toegangsmodi PVC [toegangsmodi][access-modes].

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

## <a name="using-the-persistent-volume"></a>Met behulp van de permanente volume

Nadat de claim permanente volume is gemaakt en de opslag is ingericht, een schil kan worden gemaakt met toegang tot het volume. Het volgende manifest maakt een schil die gebruikmaakt van de claim permanente volume `azurefile` koppelen van de Azure-bestandsshare op de `/var/www/html` pad. 

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
        claimName: azurefile
```

## <a name="mount-options"></a>Koppelingsopties

Standaardwaarden fileMode en dirMode verschillen tussen versies Kubernetes zoals beschreven in de volgende tabel. 

| versie | waarde |
| ---- | ---- |
| V1.6.x, v1.7.x | 0777 |
| V1.8.0 v1.8.5 | 0700 |
| V1.8.6 of hoger | 0755 |
| V1.9.0 | 0700 |
| V1.9.1 of hoger | 0755 |

Als u een cluster van versie 1.8.5 of hoger, koppelpunt opties kunnen worden opgegeven op het opslagobject klasse. Het volgende voorbeeld wordt `0777`. 

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

Als u een cluster van versie 1.8.0 - 1.8.4, een beveiligingscontext kan worden opgegeven met de `runAsUser` waarde ingesteld op `0`. Zie voor meer informatie over schil beveiligingscontext [configureren van een beveiligingscontext][kubernetes-security-context].

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
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
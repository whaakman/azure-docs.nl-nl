---
title: Voeg SSH toe aan de clusterknooppunten Azure Container Service (AKS)
description: Een SSH-verbinding maken met een Azure Container Service (AKS)-cluster met knooppunten
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/28/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 00affc3d1c02c477826261aeac6e092934037e81
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="ssh-into-azure-container-service-aks-cluster-nodes"></a>Voeg SSH toe aan de clusterknooppunten Azure Container Service (AKS)

Soms moet u mogelijk toegang krijgen tot een Azure Container Service (AKS)-knooppunt voor onderhoud, logboekgegevens verzameld of andere bewerkingen voor het oplossen van problemen. Azure Container Service (AKS) knooppunten worden niet blootgesteld aan internet. Gebruik de stappen uiteengezet in dit document een SSH-verbinding maken met een AKS-knooppunt.

## <a name="configure-ssh-access"></a>SSH-toegang configureren

 Aan SSH in een specifiek knooppunt een schil wordt gemaakt met `hostNetwork` toegang. Een service wordt ook gemaakt voor schil toegang. Deze configuratie heeft bepaalde bevoegdheden, en moet worden verwijderd na gebruik.

Maak een bestand met de naam `aks-ssh.yaml` en kopieer dit manifest. Naam van het knooppunt bijwerken met de naam van het doelknooppunt AKS.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-ssh
spec:
  selector:
    app: aks-ssh
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 22
    targetPort: 22
---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: aks-ssh
  labels:
    app: aks-ssh
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-ssh
  template:
    metadata:
      labels:
        app: aks-ssh
    spec:
      containers:
      - name: alpine
        image: alpine:latest
        ports:
        - containerPort: 22
        command: ["/bin/sh", "-c", "--"]
        args: ["while true; do sleep 30; done;"]
      hostNetwork: true
      nodeName: aks-nodepool1-42032720-0
```

Het manifest voor het maken van de schil en de service worden uitgevoerd.

```azurecli-interactive
$ kubectl apply -f aks-ssh.yaml
```

Het externe IP-adres van de zichtbare service ophalen. Het duurt een paar minuten voor de configuratie van IP-adressen te voltooien. 

```azurecli-interactive
$ kubectl get service

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
kubernetes         ClusterIP      10.0.0.1      <none>          443/TCP        1d
aks-ssh            LoadBalancer   10.0.51.173   13.92.154.191   22:31898/TCP   17m
```

Maken van de ssh verbinding. 

De standaardnaam van de gebruiker voor een cluster AKS is `azureuser`. Als dit account is gewijzigd tijdens het maken van een cluster, vervangen door de naam van de juiste admin-gebruiker. 

Als uw sleutel is niet op `~/ssh/id_rsa`, bieden de juiste locatie via de `ssh -i` argument.

```azurecli-interactive
$ ssh azureuser@13.92.154.191

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

48 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Tue Feb 27 20:10:38 2018 from 167.220.99.8
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@aks-nodepool1-42032720-0:~$
```

## <a name="remove-ssh-access"></a>SSH-toegang verwijderen

Wanneer u klaar bent, de schil voor SSH-toegang en de service verwijderen.

```azurecli-interactive
kubectl delete -f aks-ssh.yaml
```
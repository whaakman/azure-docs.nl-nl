---
title: Voeg SSH toe aan de clusterknooppunten Azure Kubernetes Service (AKS)
description: Een SSH-verbinding maken met een Azure Kubernetes Service (AKS)-cluster met knooppunten
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 04/06/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c2b77e558db0e323370c24b87a75357235677f7e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="ssh-into-azure-kubernetes-service-aks-cluster-nodes"></a>Voeg SSH toe aan de clusterknooppunten Azure Kubernetes Service (AKS)

Soms moet u mogelijk toegang krijgen tot een knooppunt Azure Kubernetes Service (AKS) voor onderhoud, logboekgegevens verzameld of andere bewerkingen voor het oplossen van problemen. Azure Kubernetes Service (AKS) knooppunten worden niet blootgesteld aan internet. Gebruik de stappen uiteengezet in dit document een SSH-verbinding maken met een AKS-knooppunt.

## <a name="get-aks-node-address"></a>AKS knooppuntadres ophalen

Het IP-adres van een AKS cluster knooppunt met behulp van de `az vm list-ip-addresses` opdracht. Naam van de resourcegroep met de naam van de resourcegroep AKS vervangen.

```console
$ az vm list-ip-addresses --resource-group MC_myAKSCluster_myAKSCluster_eastus -o table

VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-42032720-0  10.240.0.6
aks-nodepool1-42032720-1  10.240.0.5
aks-nodepool1-42032720-2  10.240.0.4
```

## <a name="create-ssh-connection"></a>SSH-verbinding maken

Voer de `debian` container installatiekopie en een terminalsessie gekoppeld. De container kan vervolgens worden gebruikt voor het maken van een SSH-sessie met een willekeurig knooppunt in het cluster AKS.

```console
kubectl run -it --rm aks-ssh --image=debian
```

Een SSH-client installeren in de container.

```console
apt-get update && apt-get install openssh-client -y
```

Open een tweede terminal en alle gehele product als u de naam van de zojuist gemaakte schil lijst.

```console
$ kubectl get pods

NAME                       READY     STATUS    RESTARTS   AGE
aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
```

Kopieer uw SSH-sleutel naar de schil, de naam van de schil vervangen door de juiste waarde.

```console
kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
```

Update de `id_rsa` zodat deze gebruiker alleen-lezen bestand.

```console
chmod 0600 id_rsa
```

Maak nu een SSH-verbinding naar het knooppunt AKS. De standaardnaam van de gebruiker voor een cluster AKS is `azureuser`. Als dit account is gewijzigd tijdens het maken van een cluster, vervangen door de naam van de juiste admin-gebruiker.

```console
$ ssh -i id_rsa azureuser@10.240.0.6

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

Wanneer u klaar bent, sluit u de SSH-sessie en vervolgens de interactieve container-sessie. Deze actie worden gebruikt voor SSH-toegang van het cluster AKS schil verwijderd.
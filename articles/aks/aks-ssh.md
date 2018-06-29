---
title: Voeg SSH toe aan de clusterknooppunten Azure Kubernetes Service (AKS)
description: Een SSH-verbinding maken met een Azure Kubernetes Service (AKS)-cluster met knooppunten
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/06/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 8f4c70814566a963d86c119044f9ed3ef3238483
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099395"
---
# <a name="ssh-into-azure-kubernetes-service-aks-cluster-nodes"></a>Voeg SSH toe aan de clusterknooppunten Azure Kubernetes Service (AKS)

Soms moet u mogelijk toegang krijgen tot een knooppunt Azure Kubernetes Service (AKS) voor onderhoud, logboekgegevens verzameld of andere bewerkingen voor het oplossen van problemen. AKS knooppunten worden niet blootgesteld aan internet. Gebruik de stappen uiteengezet in dit document een SSH-verbinding maken met een AKS-knooppunt.

## <a name="reset-ssh-keys"></a>SSH-sleutels opnieuw instellen

Als u een AKS zonder SSH-sleutels hebt geïmplementeerd, of u geen toegang tot de juiste SSH-sleutels hebt, kunnen u deze opnieuw met de Azure portal.

Blader naar uw cluster AKS, selecteer een knooppunt AKS (virtuele machine) en selecteer **wachtwoord opnieuw instellen** opnieuw instellen van de openbare SSH-sleutel.

![AKS VM met de knop wachtwoord opnieuw instellen](media/aks-ssh/reset-password.png)

Selecteer **openbare opnieuw SSH-sleutel**, geef de gebruikersnaam van de cluster AKS, namelijk **azueruser** standaard en de kopie in een openbare SSH-sleutel. Selecteer **Update** wanneer u klaar.

![AKS portal VM met de knop wachtwoord opnieuw instellen](media/aks-ssh/reset-password-2.png)

Als de SSH-sleutel is opnieuw ingesteld, kunt u een SSH-verbinding met de bijbehorende persoonlijke sleutel maken.

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

De persoonlijke SSH-sleutel naar de schil kopiëren, de naam van de schil vervangen door de juiste waarde.

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

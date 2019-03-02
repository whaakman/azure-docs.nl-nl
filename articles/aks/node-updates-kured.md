---
title: Bijwerken en opnieuw opstarten van knooppunten met kured in Azure Kubernetes Service (AKS)
description: Meer informatie over het bijwerken van knooppunten en automatisch opnieuw opgestart met kured in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/28/2019
ms.author: iainfou
ms.openlocfilehash: 75057f6bd92fbdc805da2e0e36dc2bff7b069f26
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243326"
---
# <a name="apply-security-and-kernel-updates-to-nodes-in-azure-kubernetes-service-aks"></a>Beveiliging en kernel-updates toepassen op knooppunten in Azure Kubernetes Service (AKS)

Ter bescherming van uw clusters, worden beveiligingsupdates automatisch toegepast op de knooppunten in AKS. Deze updates bevatten oplossingen OS of kernelupdates. Sommige van deze updates moet een knooppunt opnieuw opstarten om het proces te voltooien. AKS niet automatisch opnieuw opgestart knooppunten om de updateproces te voltooien.

Dit artikel laat u het gebruik van de open-source [kured (KUbernetes opnieuw opstarten Daemon)] [ kured] om te bekijken voor het afhandelen van knooppunten die worden opgestart, klikt u vervolgens automatisch moeten het plannen van het uitvoeren van schillen en knooppunt opnieuw opstarten het proces.

> [!NOTE]
> `Kured` is een open source-project door Weaveworks. Ondersteuning voor dit project in AKS wordt geleverd op basis van best-effort. Aanvullende ondersteuning kunt u vinden in de #weave-community slack-kanaal,

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

U ook moet de Azure CLI versie 2.0.59 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

## <a name="understand-the-aks-node-update-experience"></a>Inzicht in de update-ervaring van de AKS-knooppunten

In een AKS-cluster, uw Kubernetes-knooppunten worden uitgevoerd als Azure virtual machines (VM's). Deze op basis van Linux-VM's gebruiken een Ubuntu-installatiekopie van het besturingssysteem die is geconfigureerd voor het automatisch controleren op updates elke nacht. Als u beveiligings- of kernel-updates beschikbaar zijn, worden ze automatisch gedownload en geïnstalleerd.

![AKS-knooppunt bijwerken en opnieuw opstarten verwerken met kured](media/node-updates-kured/node-reboot-process.png)

Sommige beveiligingsupdates, zoals kernel-updates vereisen het opnieuw opstarten van een knooppunt aan het proces is voltooid. Een knooppunt dat worden opgestart moet maakt u een bestand met de naam */var/run/reboot-required*. Dit proces opnieuw opstarten plaatsvindt niet automatisch.

U kunt uw eigen werkstromen en processen gebruiken om verwerken knooppunt opnieuw wordt opgestart, of gebruik `kured` voor het indelen van het proces. Met `kured`, een [DaemonSet] [ DaemonSet] wordt geïmplementeerd die een pod op elk knooppunt in het cluster wordt uitgevoerd. Deze schillen in de DaemonSet bekijken voor sprake is van de */var/run/reboot-required* -bestand en start vervolgens een proces voor het opnieuw opstarten van de knooppunten.

### <a name="node-upgrades"></a>Knooppunt bijgewerkt

Er is een extra proces in AKS waarmee u *upgrade* een cluster. Een upgrade is doorgaans te verplaatsen naar een nieuwere versie van Kubernetes, beveiligingsupdates knooppunt niet alleen van toepassing. Een AKS-upgrade voert de volgende handelingen uit:

* Een nieuw knooppunt wordt geïmplementeerd met de meest recente beveiligingsupdates en Kubernetes-versie die zijn toegepast.
* Een oude knooppunt wordt afgebakend en geleegd.
* Schillen zijn gepland op het nieuwe knooppunt.
* De oude knooppunt is verwijderd.

U kan niet op dezelfde Kubernetes-versie blijven tijdens een upgrade-gebeurtenis. U moet een nieuwere versie van Kubernetes. Als u wilt bijwerken naar de nieuwste versie van Kubernetes, kunt u [uw AKS-cluster upgraden][aks-upgrade].

## <a name="deploy-kured-in-an-aks-cluster"></a>Kured in een AKS-cluster implementeren

Implementatie van de `kured` DaemonSet, het volgende voorbeeld YAML manifest van de GitHub-projectpagina passen. Dit manifest maakt u een rol en clusterfunctie, bindingen en een service-account en vervolgens implementeert u de DaemonSet met behulp van `kured` versie 1.1.0 die ondersteuning biedt voor AKS clusters 1.9 of hoger.

```console
kubectl apply -f https://github.com/weaveworks/kured/releases/download/1.1.0/kured-1.1.0.yaml
```

U kunt ook extra parameters voor `kured`, zoals integratie met Prometheus of Slack. Zie voor meer informatie over aanvullende configuratieparameters, de [kured installatie docs][kured-install].

## <a name="update-cluster-nodes"></a>Update-clusterknooppunten

Standaard controleren AKS-knooppunten op updates elke buiten kantooruren. Als u niet wilt wachten, kunt u handmatig een update om te controleren of uitvoeren `kured` correct wordt uitgevoerd. Voer de stappen voor het eerst [SSH naar een van uw AKS-knooppunten][aks-ssh]. Zodra u een SSH-verbinding naar het knooppunt hebt, controleren op updates en deze als volgt toepassen:

```console
sudo apt-get update && sudo apt-get upgrade -y
```

Als er updates zijn toegepast waarvoor opnieuw opstarten van een knooppunt, een bestand wordt geschreven naar */var/run/reboot-required*. `Kured` controleert op knooppunten die worden opgestart om de 60 minuten standaard moeten.

## <a name="monitor-and-review-reboot-process"></a>Volg en controleer opnieuw opstarten-proces

Wanneer een van de replica's in de DaemonSet heeft gedetecteerd dat een knooppunt opnieuw opstarten vereist is, wordt een vergrendeling op het knooppunt via de Kubernetes API geplaatst. Deze vergrendeling voorkomt u dat de extra schillen die op het knooppunt is gepland. De vergrendeling wordt ook aangegeven dat slechts één knooppunt moet opnieuw worden opgestart op een tijdstip. Met het knooppunt uitschakelen afgebakend, actieve schillen zijn geleegd vanaf het knooppunt en het knooppunt opnieuw wordt opgestart.

U kunt de status van de knooppunten met behulp van bewaken de [kubectl ophalen knooppunten] [ kubectl-get-nodes] opdracht. De volgende voorbeelduitvoer ziet u een knooppunt met de status van *SchedulingDisabled* als het knooppunt wordt voorbereid voor het proces opnieuw opstarten:

```
NAME                       STATUS                     ROLES     AGE       VERSION
aks-nodepool1-28993262-0   Ready,SchedulingDisabled   agent     1h        v1.11.7
```

Nadat de updateproces voltooid is, vindt u de status van de knooppunten met behulp van de [kubectl ophalen knooppunten] [ kubectl-get-nodes] opdracht met de `--output wide` parameter. Deze extra uitvoer ziet u een verschil in *KERNEL-versie* van de onderliggende knooppunten, zoals in de volgende voorbeelduitvoer wordt weergegeven. De *aks-nodepool1-28993262-0* is bijgewerkt in een vorige stap en toont de kernelversie *4.15.0-1039-azure*. Het knooppunt *aks-nodepool1-28993262-1* die niet is bijgewerkt toont de kernelversie *4.15.0-1037-azure*.

```
NAME                       STATUS    ROLES     AGE       VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-28993262-0   Ready     agent     1h        v1.11.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS   4.15.0-1039-azure   docker://3.0.4
aks-nodepool1-28993262-1   Ready     agent     1h        v1.11.7   10.240.0.5    <none>        Ubuntu 16.04.6 LTS   4.15.0-1037-azure   docker://3.0.4
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel beschreven hoe u `kured` naar knooppunten automatisch opnieuw opgestart als onderdeel van het updateproces voor de beveiliging. Als u wilt bijwerken naar de nieuwste versie van Kubernetes, kunt u [uw AKS-cluster upgraden][aks-upgrade].

<!-- LINKS - external -->
[kured]: https://github.com/weaveworks/kured
[kured-install]: https://github.com/weaveworks/kured#installation
[kubectl-get-nodes]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[DaemonSet]: concepts-clusters-workloads.md#statefulsets-and-daemonsets
[aks-ssh]: ssh.md
[aks-upgrade]: upgrade-cluster.md

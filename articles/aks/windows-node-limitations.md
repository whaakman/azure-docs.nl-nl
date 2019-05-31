---
title: Beperkingen voor Windows Server-knooppuntgroepen in Azure Kubernetes Service (AKS)
description: Meer informatie over bekende beperkingen tijdens het uitvoeren van Windows Server-knooppuntgroepen en werkbelastingen van toepassingen in Azure Kubernetes Service (AKS)
services: container-service
author: tylermsft
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: twhitney
ms.openlocfilehash: 34ece6e49332f781f688a8741db3514faf8c9a25
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304398"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Huidige beperkingen voor Windows Server-knooppuntgroepen en werkbelastingen van toepassingen in Azure Kubernetes Service (AKS)

In Azure Kubernetes Service (AKS), kunt u een knooppuntgroep die Windows Server wordt uitgevoerd als het gastbesturingssysteem te installeren op de knooppunten maken. Deze knooppunten kunnen de systeemeigen Windows-containertoepassingen, zoals die zijn gebaseerd op .NET Framework uitvoeren. Als er belangrijke verschillen in hoe de Linux- en Windows-besturingssysteem biedt ondersteuning voor containers, zijn sommige algemene Kubernetes en pod-gerelateerde functies momenteel niet beschikbaar voor Windows-knooppuntgroepen.

In dit artikel bevat een overzicht van enkele van de beperkingen en OS-concepten voor Windows Server-knooppunten in AKS. Knooppuntgroepen voor Windows Server zijn momenteel in preview.

> [!IMPORTANT]
> AKS-preview-functies zijn selfservice, aanmelden. Ze zijn bedoeld om het verzamelen van fouten en feedback van onze community. Preview-versie, worden deze functies zijn niet bedoeld voor gebruik in productieomgevingen. Functies in public preview vallen onder 'best effort'-ondersteuning. Hulp van de AKS-teams voor technische ondersteuning is beschikbaar tijdens kantooruren Pacific tijdzone (PST) alleen. Zie de volgende artikelen ondersteuning voor aanvullende informatie:
>
> * [Ondersteuningsbeleid voor AKS][aks-support-policies]
> * [Veelgestelde vragen over ondersteuning van Azure][aks-faq]

## <a name="limitations-for-windows-server-in-kubernetes"></a>Beperkingen voor WindowsServer in Kubernetes

Windows Server-containers moeten worden uitgevoerd op een containerhost op basis van een Windows. Voor Windows Server-containers in AKS uitvoert, kunt u [maken van een knooppunt van toepassingen die Windows Server wordt uitgevoerd] [ windows-node-cli] als het gastbesturingssysteem te installeren. Ondersteuning voor Windows Server-knooppunt groep bevat enkele beperkingen die deel van de upstream-Windows-Server in een Kubernetes-project uitmaken. Deze beperkingen zijn niet specifiek zijn voor AKS. Zie voor meer informatie over deze upstream ondersteuning voor Windows Server in Kubernetes [Windows Server-containers in Kubernetes beperkingen][upstream-limitations].

De volgende upstream beperkingen voor Windows Server-containers in Kubernetes zijn relevant voor AKS:

- Windows Server-containers kunnen alleen gebruiken voor Windows Server 2019, die overeenkomt met het onderliggende besturingssysteem van de Windows Server-knooppunt.
    - Containerinstallatiekopieën gebouwd met behulp van Windows Server 2016, zoals het Basisbesturingssysteem worden niet ondersteund.
- Bevoegde containers kunnen niet worden gebruikt.
- Linux-specifieke functies zoals uitvoerenals, SELinux, AppArmor of POSIX-mogelijkheden zijn niet beschikbaar in Windows Server-containers.
    - Bestand systeembeperkingen die Linux-specifieke zoals UUI/GUID, per gebruikersmachtigingen gelden ook zijn niet beschikbaar in Windows Server-containers.
- Azure-schijven en Azure Files zijn de ondersteunde volumetypen geopend als NTFS-volumes in de Windows Server-container.
    - NFS-opslag / volumes worden niet ondersteund.

## <a name="aks-limitations-for-windows-server-node-pools"></a>AKS-beperkingen voor Windows Server-knooppuntgroepen

De volgende beperkingen zijn van toepassing op Windows Server-knooppunt groep ondersteuning in AKS:

- Een AKS-cluster bevat altijd een knooppuntgroep Linux als het eerste knooppunt van toepassingen. Deze eerste op basis van Linux-knooppuntgroep kan niet worden verwijderd tenzij het AKS-cluster zelf wordt verwijderd.
- AKS-clusters, moeten het model Azure CNI (Geavanceerd) voor netwerken gebruiken.
    - Kubenet (basis)-netwerken wordt niet ondersteund. U kunt een AKS-cluster dat gebruik maakt van kubenet niet maken. Zie voor meer informatie over de verschillen in de netwerk-modellen [netwerk concepten voor toepassingen in AKS][azure-network-models].
    - Het model van Azure CNI netwerk vergt extra planning en overwegingen voor het beheer van IP-adres. Zie voor meer informatie over het plannen en implementeren van Azure CNI [netwerken van Azure CNI configureren in AKS][configure-azure-cni].
- Windows Server-knooppunten in AKS moet *bijgewerkt* bugfixes naar een meest recente versie van Windows Server 2019 naar de nieuwste patch onderhouden en bijgewerkt. Windows-Updates zijn niet ingeschakeld in de afbeelding basisknooppunt in AKS. Een regelmatige rond de releasecyclus van Windows Update en uw eigen validatieproces, moet u een upgrade op de Windows Server-knooppunt pool(s) uitvoeren in uw AKS-cluster. Zie voor meer informatie over het bijwerken van een Windows Server-knooppuntgroep [een knooppuntgroep in AKS Upgrade][nodepool-upgrade].
    - Deze upgrades van Windows Server-knooppunt gebruiken tijdelijk extra IP-adressen in het subnet van het virtuele netwerk als een nieuw knooppunt wordt geïmplementeerd, voordat de oude knooppunt wordt verwijderd.
    - vCPU-quota ook tijdelijk in het abonnement worden gebruikt als een nieuw knooppunt wordt geïmplementeerd, wordt het oude knooppunt verwijderd.
    - U kunt geen automatisch bijwerken en beheren van opnieuw opstarten met behulp van `kured` net als bij Linux-knooppunten in AKS.
- Het AKS-cluster kan maximaal acht knooppuntgroepen hebben.
    - U kunt maximaal 400 knooppunten hebben in die pools acht knooppunten.
- De pool-naam van het Windows Server-knooppunt heeft een limiet van 6 tekens.
- Preview-functies in AKS zoals Network Policy and cluster automatisch schalen, niet zijn goedgekeurd voor Windows Server-knooppunten.
- Inkomend verkeer controllers moeten alleen worden gepland op Linux-knooppunten met behulp van een NodeSelector.
- Azure Dev opslagruimten is momenteel alleen beschikbaar voor knooppunt op basis van Linux-pools.
- Groep beheerde serviceaccounts (gMSA) ondersteuning bij de Windows Server-knooppunten niet zijn gekoppeld aan een Active Directory-domein is momenteel niet beschikbaar in AKS.
    - De open-source, upstream [aks-engine] [ aks-engine] project biedt momenteel ondersteuning voor gMSA als u nodig hebt om deze functie te gebruiken.

## <a name="os-concepts-that-are-different"></a>OS-concepten die verschillen

Kubernetes is in het verleden op Linux gerichte. Veel voorbeelden die worden gebruikt in de upstream [Kubernetes.io] [ kubernetes] website zijn bedoeld voor gebruik op Linux-knooppunten. Wanneer u implementaties die gebruikmaken van Windows Server-containers, de volgende overwegingen bij het toepassen van de OS-niveau maakt:

- **Identiteit** -Linux maakt gebruik van gebruikers-id (UID) en groeps-id (groeps-id), weergegeven als geheel getal typen. Namen van gebruikers en groepen niet zijn canonieke: ze zijn een alias in */etc/groepen* of */etc/passwd* terug aan de UID + groeps-id.
    - Windows Server maakt gebruik van een grotere binaire SID (security identifier) die is opgeslagen in de database van Windows Security Access Manager (SAM). Deze database wordt niet gedeeld tussen de host en containers, of tussen containers.
- **Bestandsmachtigingen** -Windows-Server maakt gebruik van een lijst voor toegangsbeheer op basis van de SID, in plaats van een bitmasker van machtigingen en UID + groeps-id
- **Bestandspaden** -overeenkomst op Windows Server is het gebruik van \ in plaats van /.
    - Geef het pad correct voor Windows Server-containers in pod-specificaties die volumes koppelen. Bijvoorbeeld, in plaats van een koppelpunt verwijzen van */mnt/volume* in een Linux-container, geeft u een stationsletter en een locatie zoals */K/Volume* koppelen als de *K:* station.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met Windows Server-containers in AKS, [maken van een knooppunt van toepassingen die Windows Server wordt uitgevoerd in AKS][windows-node-cli].

<!-- LINKS - external -->
[upstream-limitations]: https://kubernetes.io/docs/setup/windows/#limitations
[kubernetes]: https://kubernetes.io
[aks-engine]: https://github.com/azure/aks-engine

<!-- LINKS - internal -->
[azure-network-models]: concepts-network.md#azure-virtual-networks
[configure-azure-cni]: configure-azure-cni.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[windows-node-cli]: windows-container-cli.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md

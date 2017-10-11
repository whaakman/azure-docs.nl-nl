---
title: Hoe werkt de replicatie van on-premises virtuele machines naar een secundaire on-premises site in Azure Site Recovery? | Microsoft Docs
description: Dit artikel biedt een overzicht van de onderdelen en architectuur die worden gebruikt bij het repliceren van on-premises virtuele machines en fysieke servers naar een secundaire site met de Azure Site Recovery-service.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: raynew
ms.openlocfilehash: fca95c63964b955db7ddfbe53250702cc8af122e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-does-on-premises-machine-replication-to-a-secondary-site-work-in-site-recovery"></a>Hoe werkt de replicatie van on-premises virtuele machines naar een secundaire site in Site Recovery?

In dit artikel worden de onderdelen en processen beschreven die betrokken zijn bij het repliceren van on-premises virtuele machines en fysieke servers naar Azure, met behulp van de [Azure Site Recovery](site-recovery-overview.md)-service.

U kunt de volgende onderdelen repliceren naar een secundaire on-premises site:
- On-premises virtuele machines van Hyper-V, virtuele machines van Hyper-V in Hyper-V-clusters en zelfstandige hosts die worden beheerd in VMM-clouds (Virtual Machine Manager) van System Center.
- On-premises virtuele machines van VMware, en fysieke Windows- en Linux-servers. In dit scenario wordt de replicatie beheerd door Scout.

U kunt onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen plaatsen.

## <a name="replicate-hyper-v-vms-to-a-secondary-on-premises-site"></a>Virtuele machines van Hyper-V repliceren naar een secundaire on-premises site


### <a name="architectural-components"></a>Architectuuronderdelen

Dit is wat u nodig hebt om virtuele Hyper V-machines te repliceren naar een secundaire site.

**Onderdeel** | **Locatie** | **Details**
--- | --- | ---
**Azure** | U hebt een account in Microsoft Azure nodig. |
**VMM-server** | Het wordt aanbevolen om één VMM-beheerserver te gebruiken op de primaire site en één op de secundaire site | Elke VMM-server moet verbinding met internet hebben.<br/><br/> Elke server moet ten minste één VMM-privécloud hebben met de Hyper-V-profielset.<br/><br/> U installeert de Azure Site Recovery-provider op de VMM-server. De Provider coördineert en stuurt replicatie met de Site Recovery-service via internet. De communicatie tussen de Provider en Azure is beveiligd en versleuteld.
**Hyper-V-server** |  U hebt een of meer Hyper-V-hostservers nodig die zich bevinden in de primaire en secundaire VMM-clouds.<br/><br/> Servers moeten verbinding met het internet hebben.<br/><br/> Gegevens worden tussen de primaire en secundaire Hyper-V-hostservers via het LAN of VPN gerepliceerd met behulp van Kerberos of verificatie via certificaat.  
**Virtuele Hyper-V-machines** | Bevindt zich op de Hyper-V-bronhostserver. | De bronhostserver moet ten minste één VM hebben die u wilt repliceren.

### <a name="replication-process"></a>Replicatieproces

1. U stelt het Azure-account in.
2. U maakt een Replication Services-kluis voor Site Recovery en configureert de kluisinstellingen, waaronder:

    - De replicatiebron en het replicatiedoel (primaire en secundaire sites).
    - Installatie van de Azure Site Recovery-provider en de Microsoft Azure Recovery Services-agent. De provider wordt geïnstalleerd op de VMM-servers en de agent op elke Hyper-V-host.
    - U maakt een replicatiebeleid voor de bron-VMM-cloud. Het beleid wordt toegepast op alle VM’s die zich op hosts in de cloud bevinden.
    - U schakelt replicatie voor virtuele Hyper-V-machines in. De initiële replicatie wordt uitgevoerd in overeenstemming met de instellingen van het replicatiebeleid.
4. Verschillen in de gegevens worden bijgehouden en de replicatie van deltaverschillen begint nadat de initiële replicatie is voltooid. Bijgehouden wijzigingen voor een item worden opgeslagen in een .hrl-bestand.
5. U voert een testfailover uit om te controleren of alles werkt.

**Afbeelding 1: replicatie van VMM naar VMM**

![On-premises naar on-premises](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Failover- en failbackproces

1. U kunt een geplande of niet-geplande [failover](site-recovery-failover.md) uitvoeren tussen on-premises sites. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
2. U kunt een failover van één machine uitvoeren of [herstelplannen](site-recovery-create-recovery-plans.md) maken om de failover van meerdere virtuele machines te coördineren.
4. Als u een niet-geplande failover naar een secundaire site uitvoert wanneer de failovermachines in de secundaire locatie niet zijn ingeschakeld voor beveiliging of replicatie. Als u een geplande failover hebt uitgevoerd, zijn de machines op de secundaire locatie na de failover beveiligd.
5. U geeft de failover vervolgens toegang tot de werkbelasting via de replica-VM.
6. Als uw primaire site weer beschikbaar is, start u de omgekeerde replicatie van de secundaire site naar de primaire site. Omgekeerde replicatie geeft de virtuele machines een beveiligde status, maar het secundaire datacenter is nog steeds de actieve locatie.
7. Als u de primaire site weer de actieve locatie wilt maken, start u een geplande failover van de secundaire site naar de primaire site, gevolgd door nog een omgekeerde replicatie.




## <a name="replicate-vmware-vmsphysical-servers-to-a-secondary-site"></a>Virtuele VMware-machines/fysieke servers repliceren naar een secundaire site

U repliceert virtuele machines van VMware of fysieke servers naar een secundaire site met InMage Scout, en met behulp van deze architectuuronderdelen:


### <a name="architectural-components"></a>Architectuuronderdelen

**Onderdeel** | **Locatie** | **Details**
--- | --- | ---
**Azure** | InMage Scout. | Voor het gebruik van InMage Scout hebt u een Azure-abonnement nodig.<br/><br/> Nadat u een Recovery Services-kluis hebt gemaakt, downloadt u InMage Scout en installeert u de nieuwste updates om de implementatie in te stellen.
**Processerver** | Bevindt zich op de primaire site | U implementeert de processerver voor het afhandelen van caching, compressie en gegevensoptimalisatie.<br/><br/> Hier wordt ook de push-installatie van de Unified Agent afgehandeld naar computers die u wilt beveiligen.
**Configuratieserver** | Bevindt zich op de secundaire site | De configuratieserver dient om uw implementatie te beheren, te configureren en te bewaken met behulp van de beheerwebsite of de vContinuum-console.
**vContinuum-server** | Optioneel. Deze wordt geïnstalleerd op dezelfde locatie als de configuratieserver. | De vContinuum-server biedt een console voor het beheren en controleren van de beveiligde omgeving.
**Hoofddoelserver** | Bevindt zich op de secundaire site | De hoofddoelserver bevat gerepliceerde gegevens. De hoofddoelserver ontvangt gegevens van de processerver, maakt een replicamachine op de secundaire site en bevat de gegevensretentiepunten.<br/><br/> Het aantal hoofddoelservers dat u nodig hebt, is afhankelijk van het aantal machines dat u wilt beveiligen.<br/><br/> Als u failback wilt uitvoeren naar de primaire site, hebt u daar ook een hoofddoelserver nodig. De Unified Agent is geïnstalleerd op deze server.
**VMware ESX/ESXi- en vCenter-server** |  Virtuele machines worden op ESX-/ESXi-hosts gehost. Hosts worden beheerd via een vCenter-server | U hebt een VMware-infrastructuur nodig om virtuele VMware-machines te repliceren.
**VM's/fysieke servers** |  De Unified Agent wordt geïnstalleerd op virtuele VMware-machines of fysieke servers die u wilt repliceren. | De agent fungeert als communicatieprovider tussen alle onderdelen.


### <a name="replication-process"></a>Replicatieproces

1. U stelt de onderdeelservers in op elke site (configuratie, proces, hoofddoel) en installeert de Unified Agent op de machines die u wilt repliceren.
2. Na de initiële replicatie verzendt de agent op elke machine deltareplicatiewijzigingen naar de processerver.
3. De processerver optimaliseert de gegevens en brengt deze over naar de hoofddoelserver op de secundaire site. De configuratieserver beheert het replicatieproces.

**Afbeelding 2: replicatie van VMware naar VMware**

![VMware naar VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="next-steps"></a>Volgende stappen

De [ondersteuningsmatrix](site-recovery-support-matrix-to-sec-site.md) controleren

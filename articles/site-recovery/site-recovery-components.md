---
title: Hoe werkt Site Recovery? | Microsoft Docs
description: In dit artikel vindt u een overzicht van de architectuur van Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 4674985363bc1267449e018ab15a53757a8fd32d
ms.lasthandoff: 03/15/2017


---
# <a name="how-does-azure-site-recovery-work"></a>Hoe werkt Azure Site Recovery?

Dit artikel beschrijft de onderliggende architectuur van de [Azure Site Recovery](site-recovery-overview.md)-service en de onderdelen daarvan.

U kunt onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen plaatsen.


## <a name="replicate-to-azure"></a>Repliceren naar Azure

U kunt het volgende repliceren naar Azure:

- **VMware**: on-premises virtuele VMware-machines die worden uitgevoerd op een [ondersteunde host](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). U kunt virtuele VMware-machines met een [ondersteund besturingssysteem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) repliceren.
- **Hyper-V**: on-premises virtuele Hyper-V-machines die worden uitgevoerd op [ondersteunde hosts](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- **Fysieke machines**: on-premises fysieke servers waarop Windows of Linux wordt uitgevoerd, op [ondersteunde besturingssystemen](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions). U kunt virtuele Hyper-V-machines repliceren waarop gastbesturingssystemen worden uitgevoerd die [worden ondersteund door Hyper-V en Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).

## <a name="vmware-to-azure"></a>VMware naar Azure

Dit is wat u nodig hebt om virtuele VMware-machines te repliceren naar Azure.

Onderwerp | Onderdeel | Details
--- | --- | ---
**Azure** | U hebt in Azure een Azure-account, een Azure-opslagaccount en een Azure-netwerk nodig. | Opslag en netwerk kunnen Resource Manager-accounts of klassieke accounts zijn.<br/><br/>  Gerepliceerde gegevens worden opgeslagen in het opslagaccount. Azure-VM’s worden gemaakt met de gerepliceerde gegevens bij failover van uw on-premises site. De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Configuratieserver** | Een enkele beheerserver (virtuele VMware-machine) voert alle on-premises onderdelen uit (de configuratieserver, de processerver en de hoofddoelserver) | De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
 **Processerver**:  | standaard geïnstalleerd op de configuratieserver. | Fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag.<br/><br/> De processerver verwerkt ook de push-installatie van de Mobility-service naar beveiligde computers en voert automatische detectie van virtuele VMware-machines uit.<br/><br/> Naarmate uw implementatie groeit, kunt u extra, afzonderlijk toegewezen processervers toevoegen om het toenemende replicatieverkeer af te handelen.
 **Hoofddoelserver** | Standaard geïnstalleerd op de on-premises configuratieserver. | Hier worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld.<br/><br/> Als de hoeveelheid failbackverkeer hoog is, kunt u een afzonderlijke hoofddoelserver voor failback implementeren.
**VMware-servers** | Virtuele VMware-machines worden gehost op vSphere ESXi-servers. Het wordt aangeraden om een vCenter-server te gebruiken om de hosts te beheren. | U voegt VMware-servers toe aan uw Recovery Services-kluis.<br/><br/> I
**Gerepliceerde machines** | De Mobility-service wordt geïnstalleerd op elke virtuele VMware-machine die u wilt repliceren. Deze kan handmatig worden geïnstalleerd op elke computer, of met een push-installatie van de processerver.

**Afbeelding 1: onderdelen van VMware naar Azure**

![Onderdelen](./media/site-recovery-components/arch-enhanced.png)

### <a name="replication-process"></a>Replicatieproces

1. U stelt de implementatie, met inbegrip van Azure-onderdelen, en een Recovery Services-kluis in. In de kluis geeft u de replicatiebron en het replicatiedoel op, stelt u de configuratieserver in, voegt u VMware-servers toe, maakt u een replicatiebeleid, implementeert u de Mobility-service, schakelt u de replicatie in en voert u een failovertest uit.
2.  Machines beginnen met de replicatie volgens het replicatiebeleid. Een initiële kopie van de gegevens wordt gerepliceerd naar Azure Storage.
4. De replicatie van deltaverschillen naar Azure begint nadat de initiële replicatie is voltooid. Bijgehouden wijzigingen voor een machine worden opgeslagen in een .hrl-bestand.
    - Replicerende machines communiceren met de configuratieserver op inkomende HTTPS-poort 443 voor replicatie.
    - Replicerende machines verzenden replicatiegegevens naar de processerver op inkomende HTTPS-poort 9443 (kan worden geconfigureerd).
    - De configuratieserver coördineert het replicatiebeheer met Azure via de uitgaande HTTPS-poort 443.
    - De processerver ontvangt gegevens van de bronmachines, optimaliseert en versleutelt deze gegevens en verzendt ze naar Azure Storage via de uitgaande poort 443.
    - Als u consistentie tussen meerdere VM's inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004. Meerdere VM’s worden gebruikt als u meerdere machines groepeert in de replicatiegroepen die crashconsistent en app-consistent herstelpunten delen bij failover. Dit is handig als machines dezelfde workload gebruiken en consistent moeten zijn.
5. Verkeer wordt via internet gerepliceerd naar de openbare eindpunten van Azure Storage. U kunt ook [openbare peering](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-circuit-peerings#public-peering) van Azure ExpressRoute gebruiken. Het repliceren van verkeer via een site-naar-site-VPN van een on-premises site naar Azure wordt niet ondersteund.

**Afbeelding 2: replicatie van VMware naar Azure**

![Verbeterd](./media/site-recovery-components/v2a-architecture-henry.png)

### <a name="failover-and-failback"></a>Failover en failback

1. Wanneer u hebt gecontroleerd of de testfailover is geslaagd, kunt u naar behoefte niet-geplande failovers naar Azure uitvoeren. Geplande failover wordt niet ondersteund.
2. U kunt een failover van één machine uitvoeren of [herstelplannen](site-recovery-create-recovery-plans.md) maken om failovers van meerdere virtuele machines uit te voeren.
3. Wanneer u een failover uitvoert, worden er replica-VM's gemaakt in Azure. U geeft een failover toegang tot de workload via de replica van de Azure-VM.
4. Als uw primaire on-premises site weer beschikbaar is, kunt u een failback uitvoeren. U stelt een infrastructuur voor failback in, start de replicatie van de machine vanaf de secundaire site naar de primaire site en voert een niet-geplande failover uit vanaf de secundaire site. Nadat u deze failover doorvoert, worden de gegevens teruggeplaatst op de on-premises site en moet u de replicatie naar Azure opnieuw inschakelen. [Meer informatie](site-recovery-failback-azure-to-vmware.md)

Er zijn enkele vereisten voor een failback:


- **Tijdelijke processerver in Azure**: als u na een failover een failback vanuit Azure wilt uitvoeren, moet u een virtuele Azure-machine hebben geconfigureerd als processerver, om de replicatie vanuit Azure af te handelen. U kunt deze virtuele machine verwijderen wanneer de failback is voltooid.
- **VPN-verbinding**: voor de failback moet u een VPN-verbinding (of Azure ExpressRoute) hebben ingesteld van het Azure-netwerk naar de on-premises site.
- **Afzonderlijke on-premises hoofddoelserver**: op de on-premises hoofddoelserver wordt de failback afgehandeld. De hoofddoelserver wordt standaard op de beheerserver geïnstalleerd, maar als u een failback van grotere hoeveelheden verkeer uitvoert, kunt u voor dit doel beter een afzonderlijke on-premises hoofddoelserver instellen.
- **Failbackbeleid**: als u wilt repliceren naar de on-premises site, hebt u een failbackbeleid nodig. Dit wordt automatisch gemaakt wanneer u uw replicatiebeleid maakt.

**Afbeelding 3: failback van VMware/fysieke failback**

![Failback](./media/site-recovery-components/enhanced-failback.png)

## <a name="physical-to-azure"></a>Fysiek naar Azure

Wanneer u fysieke on-premises servers naar Azure wilt repliceren, gebruikt u dezelfde onderdelen en processen als bij een replicatie van [VMware naar Azure](#vmware-replication-to-azure). Houd echter de volgende verschillen in gedachten:

- U kunt als configuratieserver een fysieke server gebruiken in plaats van een virtuele VMware-machine
- U hebt een on-premises VMware-infrastructuur nodig voor failback. U kunt geen failback uitvoeren naar een fysieke computer.

## <a name="hyper-v-to-azure"></a>Hyper-V naar Azure

Dit is wat u nodig hebt om virtuele Hyper V-machines te repliceren naar Azure.

**Onderwerp** | **Onderdeel** | **Details**
--- | --- | ---
**Azure** | U hebt in Azure een Microsoft Azure-account, een Azure-opslagaccount en een Azure-netwerk nodig. | Opslag en netwerk kunnen Resource Manager-gebaseerd of klassieke accounts zijn.<br/><br/> Gerepliceerde gegevens worden opgeslagen in het opslagaccount. Azure-VM’s worden gemaakt met de gerepliceerde gegevens bij failover van uw on-premises site.<br/><br/> De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**VMM-server** | Hyper-V-hosts die zich in VMM-clouds bevinden | Als Hyper-V-hosts in VMM-clouds worden beheerd, kunt u de VMM-server registreren in de Recovery Services-kluis.<br/><br/> U installeert de Site Recovery-provider op de VMM-server om replicatie te beheren met Azure.<br/><br/> U hebt ingestelde logische en VM-netwerken nodig om netwerktoewijzing te configureren. Een netwerk met virtuele machines moet zijn gekoppeld aan een logisch netwerk dat verbinding heeft met de cloud.
**Hyper-V-host** | Hyper-V-servers kunnen worden geïmplementeerd met of zonder de VMM-server. | Als er geen VMM-server is, wordt de Site Recovery-provider geïnstalleerd op de host, zodat replicatie via internet kan worden beheerd met Site Recovery. Als er een VMM-server is, wordt de provider daarop geïnstalleerd en niet op de host.<br/><br/> De Recovery Services-agent wordt geïnstalleerd op de host om de gegevensreplicatie af te handelen.<br/><br/> De communicatie vanuit zowel de Provider als de agent is beveiligd en versleuteld. De gerepliceerde gegevens in de Azure-opslag zijn eveneens versleuteld.
**Virtuele Hyper-V-machines** | U hebt één of meer virtuele machines nodig op de Hyper-V-hostserver. | Er hoeft niets verplicht op virtuele machines te worden geïnstalleerd


### <a name="replication-process"></a>Replicatieproces

1. U stelt de Azure-onderdelen in. Wij raden u aan om opslag- en netwerkaccounts in te stellen voordat u begint met de Site Recovery-implementatie.
2. U maakt een Replication Services-kluis voor Site Recovery en configureert de kluisinstellingen, waaronder:
    - Bron- en doelinstellingen. Als u geen Hyper-V-hosts in een VMM-cloud beheert, moet u voor het doel een Hyper-V-sitecontainer maken en hier Hyper-V-hosts aan toevoegen. Als uw Hyper-V-hosts in VMM worden beheerd, is de bron de VMM-cloud. Het doel is Azure.
    - Installatie van de Azure Site Recovery-provider en de Microsoft Azure Recovery Services-agent. Als u VMM hebt, wordt de Provider hierop geïnstalleerd en de agent op elke Hyper-V-host. Als u geen VMM hebt, worden de provider en de agent geïnstalleerd op elke host.
    - U maakt een replicatiebeleid voor de Hyper-V-site of de VMM-cloud. Het beleid wordt toegepast op alle VM’s die zich op hosts in de site of de cloud bevinden.
    - U schakelt replicatie voor virtuele Hyper-V-machines in. De initiële replicatie wordt uitgevoerd in overeenstemming met de instellingen van het replicatiebeleid.
4. Verschillen in de gegevens worden bijgehouden en de replicatie van deltaverschillen naar Azure begint nadat de initiële replicatie is voltooid. Bijgehouden wijzigingen voor een item worden opgeslagen in een .hrl-bestand.
5. U voert een testfailover uit om te controleren of alles werkt.

### <a name="failover-and-failback-process"></a>Failover- en failbackproces

1. U kunt een geplande of niet-geplande [failover](site-recovery-failover.md) uitvoeren van on-premises virtuele Hyper-V-machines naar Azure. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
2. U kunt een failover van één machine uitvoeren of [herstelplannen](site-recovery-create-recovery-plans.md) maken om de failover van meerdere virtuele machines te coördineren.
4. Nadat u de failover hebt uitgevoerd, worden de gemaakte replica-VM’s in Azure weergegeven. U kunt een openbaar IP-adres toewijzen aan de VM, indien nodig.
5. U geeft de failover vervolgens toegang tot de workload via de replica van de Azure-VM.
6. Als uw primaire on-premises site weer beschikbaar is, kunt u een [failback](site-recovery-failback-from-azure-to-hyper-v.md) uitvoeren. U start een geplande failover van Azure naar de primaire site. Voor een geplande failover kunt u een failback naar de dezelfde VM of naar een alternatieve locatie selecteren en wijzigingen synchroniseren tussen Azure en on-premises, zodat er geen gegevens verloren gaan. Wanneer VM on-premises worden gemaakt, kunt u de failover doorvoeren.

**Afbeelding 4: replicatie van Hyper-V-site naar Azure**

![Onderdelen](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Afbeelding 5: replicatie van Hyper-V in VMM-clouds naar Azure**

![Onderdelen](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replicate-to-a-secondary-site"></a>Repliceren naar een secundaire site

U kunt de volgende zaken repliceren naar uw secundaire site:

- **VMware**: on-premises virtuele VMware-machines die worden uitgevoerd op een [ondersteunde host](site-recovery-support-matrix-to-sec-site.md#on-premises-servers). U kunt virtuele VMware-machines met een [ondersteund besturingssysteem](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions) repliceren.
- **Fysieke machines**: on-premises fysieke servers waarop Windows of Linux wordt uitgevoerd, op [ondersteunde besturingssystemen](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).
- **Hyper-V**: on-premises virtuele Hyper-V-machines die worden uitgevoerd op [ondersteunde Hyper-V-hosts](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) die worden beheerd in VMM-clouds. [Ondersteunde hosts](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). U kunt virtuele Hyper-V-machines repliceren waarop gastbesturingssystemen worden uitgevoerd die [worden ondersteund door Hyper-V en Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).


## <a name="vmwarephysical-to-a-secondary-site"></a>Van VMware/fysieke server naar een secundaire site

U repliceert virtuele VMware-machines of fysieke servers naar een secundaire site met InMage Scout.

### <a name="components"></a>Onderdelen

**Onderwerp** | **Onderdeel** | **Details**
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

**Afbeelding 6: replicatie van VMware naar VMware**

![VMware naar VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="hyper-v-to-a-secondary-site"></a>Van Hyper-V naar een secundaire site

Dit is wat u nodig hebt om virtuele Hyper V-machines te repliceren naar een secundaire site.


**Onderwerp** | **Onderdeel** | **Details**
--- | --- | ---
**Azure** | U hebt een Microsoft Azure-account nodig. |
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

**Afbeelding 7: replicatie van VMM naar VMM**

![On-premises naar on-premises](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback"></a>Failover en failback

1. U kunt een geplande of niet-geplande [failover](site-recovery-failover.md) uitvoeren tussen on-premises sites. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
2. U kunt een failover van één machine uitvoeren of [herstelplannen](site-recovery-create-recovery-plans.md) maken om de failover van meerdere virtuele machines te coördineren.
4. Als u een niet-geplande failover naar een secundaire site uitvoert wanneer de failovermachines in de secundaire locatie niet zijn ingeschakeld voor beveiliging of replicatie. Als u een geplande failover hebt uitgevoerd, zijn de machines op de secundaire locatie na de failover beveiligd.
5. U geeft de failover vervolgens toegang tot de werkbelasting via de replica-VM.
6. Als uw primaire site weer beschikbaar is, start u de omgekeerde replicatie van de secundaire site naar de primaire site. Omgekeerde replicatie geeft de virtuele machines een beveiligde status, maar het secundaire datacenter is nog steeds de actieve locatie.
7. Als u de primaire site weer de actieve locatie wilt maken, start u een geplande failover van de secundaire site naar de primaire site, gevolgd door nog een omgekeerde replicatie.


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](site-recovery-hyper-v-azure-architecture.md) over de replicatiewerkstroom in Hyper-V.
- [Vereisten controleren](site-recovery-prereq.md)


---
title: Aanbevolen procedures voor Azure Site Recovery | Microsoft Docs
description: In dit artikel worden de aanbevolen procedures beschreven voor het implementeren van Azure Site Recovery
services: site-recovery
documentationCenter: 
author: rayne-wiselman"
manager: cfreeman
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-support-matrix-to-azure
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: a53e2a09fb2aabe96dd5347ea7509815d92bfdb8
ms.lasthandoff: 03/15/2017

---

# <a name="get-ready-to-deploy-azure-site-recovery"></a>Voorbereiden op het implementeren van Azure Site Recovery

In dit artikel wordt beschreven hoe u zich voorbereidt op het implementeren van Azure Site Recovery.

## <a name="protecting-hyper-v-virtual-machines"></a>Virtuele Hyper-V-machines beveiligen

U hebt een aantal implementatieopties voor het beveiligen van virtuele Hyper-V-machines. U kunt virtuele Hyper-V-machines on-premises repliceren naar Azure of naar een secundair datacenter. Er zijn verschillende vereisten voor elke implementatie.

**Vereiste** | **Repliceren naar Azure (met VMM)** | **Virtuele Hyper-V-machines repliceren naar Azure (zonder VMM)** | **Virtuele Hyper-V-machines repliceren naar een secundaire site (met VMM)** | **Details**
---|---|---|---|---
**VMM** | VMM die wordt uitgevoerd op System Center 2012 R2 <br/><br/>Ten minste één VMM-cloud met één of meer VMM-hostgroepen. | N.v.t. | VMM-servers op de primaire en secundaire sites met ten minste System Center 2012 SP1 met de meest recente updates. <br/><br/> Ten minste één cloud op elke VMM-server. Voor clouds moet het Hyper-V-capaciteitsprofiel zijn ingesteld.<br/><br/> De broncloud moet over ten minste één VMM-hostgroep beschikken. | Optioneel. System Center VMM hoeft niet te zijn geïmplementeerd om virtuele Hyper-V-machines te kunnen repliceren naar Azure, maar als u wilt repliceren, moet u er wel voor zorgen dat de VMM-server correct is ingesteld. Dit betekent ook dat u de juiste VMM-versie moet gebruiken en dat de clouds moeten zijn ingesteld.
**Hyper-V** | Ten minste één Hyper-V-hostserver op de on-premises site, met Windows Server 2012 R2 of hoger | Ten minste één Hyper-V-server op de bron- en doelsite, met minimaal Windows Server 2012 met de nieuwste updates geïnstalleerd. Er moet verbinding zijn met internet.<br/><br/> De Hyper-V-servers moeten zich in een hostgroep bevinden in een VMM-cloud. | Ten minste één Hyper-V-server op de bron- en doelsite, met minimaal Windows Server 2012 met de nieuwste updates geïnstalleerd. Er moet verbinding zijn met internet.<br/><br/> De Hyper-V-servers moeten zich in een hostgroep bevinden in een VMM-cloud. |
**Virtuele machines** | Ten minste één virtuele machine op de Hyper-V-bronhostserver | Ten minste één virtuele machine op de Hyper-V-hostserver in de VMM-broncloud | Ten minste één virtuele machine op de Hyper-V-hostserver in de VMM-broncloud. |  Virtuele machines die worden gerepliceerd naar Azure, moeten voldoen aan de vereisten voor virtuele Azure-machines
**Azure-account** | U hebt een Azure-account en een abonnement op de Site Recovery-service nodig. | U hebt een Azure-account en een abonnement op de Site Recovery-service nodig. | N.v.t. | Als u geen account hebt, gaat u aan de slag met een gratis proefversie.
**Azure Storage** | U hebt een abonnement nodig op een Azure Storage-account waarvoor geo-replicatie is ingeschakeld. | U hebt een abonnement nodig op een Azure Storage-account waarvoor geo-replicatie is ingeschakeld. | N.v.t. | Het account moet zich in dezelfde regio bevinden als de Azure Site Recovery-kluis en moet zijn gekoppeld aan hetzelfde abonnement.
**Netwerken** | Stel netwerktoewijzing in om ervoor te zorgen dat alle machines waarvoor een failover wordt uitgevoerd, in hetzelfde Azure-netwerk met elkaar kunnen worden verbonden, ongeacht het herstelplan waarvan ze deel uitmaken. Als er daarnaast in het Azure-doelnetwerk een netwerkgateway is geconfigureerd, kunnen virtuele machines worden verbonden met andere on-premises virtuele machines. Als u geen netwerktoewijzing instelt, kunnen alleen apparaten waarvoor binnen hetzelfde herstelplan een failover wordt uitgevoerd, worden verbonden. | N.v.t. |  <br/><br/>Stel netwerktoewijzing in om ervoor te zorgen dat virtuele machines na het uitvoeren van een failover worden verbonden met de juiste netwerken, en om ervoor te zorgen dat de gerepliceerde virtuele machines optimaal worden geplaatst op de Hyper-V-hostservers. Als u geen netwerktoewijzing configureert, worden gerepliceerde machines na het uitvoeren van een failover niet verbonden met een VM-netwerk. |  Als u netwerktoewijzing wilt instellen met VMM, moet u ervoor zorgen dat de logische VMM-netwerken en de VM-netwerken juist zijn geconfigureerd.
**Providers en agents** | Tijdens de implementatie installeert u de Azure Site Recovery-provider op de VMM-servers. U installeert de Azure Recovery Services-agent op de Hyper-V-servers in VMM-clouds. | Tijdens de implementatie installeert u zowel de Azure Site Recovery-provider als de Azure Recovery Services-agent op de Hyper-V-hostserver of in het cluster| Tijdens de implementatie installeert u de Azure Site Recovery-provider op de VMM-servers. U installeert de Azure Recovery Services-agent op de Hyper-V-servers in VMM-clouds. | Providers en agents worden via internet verbonden met Site Recovery middels een versleutelde HTTPS-verbinding. U hoeft geen firewalluitzonderingen toe te voegen en u hoeft geen specifieke proxy te maken voor de providerverbinding.
**Verbinding met internet** | Alleen voor de VMM-servers is een internetverbinding nodig | Alleen voor de Hyper-V-hostservers is een internetverbinding nodig | Alleen voor VMM-servers is een internetverbinding nodig | Er hoeft verder niets op virtuele machines te worden geïnstalleerd en deze hoeven niet rechtstreeks met internet te worden verbonden.



## <a name="protect-vmware-virtual-machines-or-physical-servers"></a>Virtuele VMware-machines of fysieke servers beveiligen

U hebt een aantal implementatieopties voor het beveiligen van virtuele VMware-machines of fysieke Windows-/Linux-servers. U kunt ze repliceren naar Azure of naar een secundair datacenter. Er zijn verschillende vereisten voor elke implementatie.

**Vereiste** | **Virtuele VMware-machines/fysieke servers repliceren naar Azure** | * **Virtuele VMware-machines/fysieke servers repliceren naar een secundaire site**  
---|---|---
**Primaire site** | **Processerver**: een speciale Windows-server (fysiek of virtueel) | **Processerver**: een speciale Windows-server (fysiek of een virtuele VMware-machine)<br/><br/>  
**Secundaire on-premises site** | N.v.t. | **Configuratieserver**: een speciale Windows-server (fysiek of virtueel) <br/><br/> **Hoofddoelserver**: een speciale server (fysiek of virtueel). Voer een configuratie met Windows uit om Windows-machines te beveiligen en gebruik Linux om Linux-machines te beveiligen.
**Azure** | **Abonnement**: u hebt een abonnement nodig voor de Site Recovery-service. <br/><br/> **Opslagaccount**: u hebt een opslagaccount nodig waarvoor geo-replicatie is ingeschakeld. Het account moet zich in dezelfde regio bevinden als de Site Recovery-kluis en moet zijn gekoppeld aan hetzelfde abonnement. <br/><br/> **Configuratieserver**: u moet de configuratieserver instellen als virtuele Azure-machine <br/><br/> **Hoofddoelserver**: u moet de hoofddoelserver instellen als virtuele Azure-machine <br/><br/> Voer een configuratie met Windows uit om Windows-machines te beveiligen en gebruik Linux om Linux-machines te beveiligen.<br/><br/> **Virtueel Azure-netwerk**: u hebt een virtueel Azure-netwerk nodig om de configuratieserver en de hoofddoelserver op te implementeren. Dit netwerk moet onderdeel zijn van hetzelfde abonnement als de Azure Site Recovery-kluis en moet zich in dezelfde regio bevinden | N.v.t.  
**Virtuele machines/fysieke servers** | Ten minste één virtuele VMware-machine of fysieke Windows-/Linux-server.<br/><br/>Tijdens de implementatie wordt de Mobility-service geïnstalleerd op elke machine| Ten minste één virtuele VMware-machine of fysieke Windows-/Linux-server.<br/><br/> Tijdens de implementatie wordt de Unified-agent geïnstalleerd op elke machine.




## <a name="azure-virtual-machine-requirements"></a>Vereisten voor virtuele Azure-machines

U kunt Site Recovery implementeren voor het repliceren van virtuele machines en fysieke servers met een besturingssysteem dat wordt ondersteund door Azure. Dit omvat de meeste versies van Windows en Linux. U moet ervoor zorgen dat de on-premises virtuele machines die u wilt beveiligen, voldoen aan de vereisten van Azure.


## <a name="optimizing-your-deployment"></a>Uw implementatie optimaliseren

Gebruik de volgende tips om uw implementatie te optimaliseren en te schalen.

- **Grootte van het besturingssysteemvolume**: wanneer u een virtuele machine repliceert naar Azure, moet het besturingsysteemvolume kleiner zijn dan 1 TB. Als u meer volumes hebt, kunt u ze handmatig verplaatsen naar een andere schijf voordat u aan de slag gaat met het implementeren.
- **Groote van de gegevensschijf**: als u repliceert naar Azure, kunt u maximaal 32 gegevensschijven gebruiken voor één virtuele machine, elk met een maximale grootte van 1 TB. U kunt virtuele machines tot ~32 TB effectief repliceren en er effectief failovers voor uitvoeren.
- **Herstelplanlimieten**: met Site Recovery kunnen duizenden virtuele machines worden geschaald. Herstelplannen worden ontwikkeld als model voor toepassingen waarvoor samen een failover moet worden uitgevoerd. In een herstelplan mogen daarom maximaal 50 machines worden vermeld.
- **Azure-servicelimieten**: elk Azure-abonnement wordt geleverd met een reeks standaardlimieten voor kerngeheugens, cloudservices enz. Het wordt aanbevolen om een testfailover uit te voeren om de beschikbaarheid van resources in uw abonnement te valideren. U kunt de limieten wijzigen via de ondersteuning van Azure.
- **Capaciteitsplanning**: maak een planning voor het schalen en de prestaties.
- **Replicatiebandbreedte**: als u te weinig replicatiebandbreedte hebt, let u hierop:
    - **ExpressRoute**: Site Recovery werkt met Azure ExpressRoute en WAN-optimalisatie, bijvoorbeeld middels Riverbed.
    - **Replicatieverkeer**: Site Recovery voert een slimme eerste replicatie uit met alleen gegevensblokken in plaats van de gehele VHD. Bij doorlopende replicatie worden alleen wijzigingen gerepliceerd.
    - **Netwerkverkeer**: u kunt het netwerkverkeer dat wordt gebruikt voor replicatie, beheren door Windows QoS in te stellen met een beleid dat is gebaseerd op het doel-IP-adres en de doelpoort.  Er is een extra optie als u naar Azure Site Recovery repliceert met de Azure Backup-agent. U kunt voor die agent beperkingen configureren.
- **RTO**: als u de beoogde hersteltijd (RTO) wilt berekenen die u kunt verwachten met Site Recovery, wordt aangeraden om een testfailover uit te voeren en de Site Recovery-taken te bekijken om te analyseren hoelang het duurt om de bewerkingen te voltooien. Als u failovers uitvoert naar Azure, wordt voor de beste RTO aangeraden om alle handmatige acties te automatiseren. Voer hiervoor een integratie uit met Azure Automation en gebruik herstelplannen.
- **RPO**: Site Recovery biedt ondersteuning voor een bijna synchroon Recovery Point Objective (RPO) wanneer u naar Azure repliceert. Hiervoor wordt aangenomen dat er voldoende bandbreedte beschikbaar is tussen uw datacenter en Azure.


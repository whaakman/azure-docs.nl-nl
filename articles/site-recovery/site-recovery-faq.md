---
title: 'Azure Site Recovery: Veelgestelde vragen | Microsoft Docs'
description: Dit artikel wordt populaire vragen over Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5cdc4bcd-b4fe-48c7-8be1-1db39bd9c078
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: raynew
ms.openlocfilehash: 82cec6df5d5d6ecf1147cac29b8fc46966ea57de
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: veelgestelde vragen
Dit artikel bevat veelgestelde vragen over Azure Site Recovery. Als u vragen hebt na het lezen van dit artikel, plaatst u deze op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).

## <a name="general"></a>Algemeen
### <a name="what-does-site-recovery-do"></a>Wat doet Site Recovery?
Site Recovery draagt bij aan uw zakelijke continuïteit en noodherstelplan (BCDR), door te organiseren en het automatiseren van replicatie van Azure VM's tussen regio's, on-premises virtuele machines en fysieke servers naar Azure en on-premises machines naar een secundaire datacenter. [Meer informatie](site-recovery-overview.md).

### <a name="what-can-site-recovery-protect"></a>Wat kan Site Recovery beveiligen?
* **Virtuele machines in Azure**: Site Recovery kan elke workload uitgevoerd op een ondersteunde Azure-virtuele machine gerepliceerd
* **Hyper-V virtuele machines**: Site Recovery kan elke workload uitgevoerd op een virtuele machine van Hyper-V beveiligen.
* **Fysieke servers**: Site Recovery kunt beveiligen fysieke servers met Windows of Linux.
* **Virtuele VMware-machines**: Site Recovery kan elke workload uitgevoerd in een VMware-VM beveiligen.



### <a name="can-i-replicate-azure-vms"></a>Kan ik virtuele Azure-machines repliceren?
Ja, kunt u de ondersteunde Azure-machines repliceren tussen Azure-regio's. [Meer informatie](site-recovery-azure-to-azure.md).

### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Wat moet ik in Hyper-V aan de organisatie van replicatie met Site Recovery?
Wat u voor de Hyper-V-hostserver nodig hebt, is afhankelijk van het implementatiescenario. Controleer de lijst met vereisten voor Hyper-V in:

* [Hyper-V-machines (zonder VMM) repliceren naar Azure](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V-machines (met VMM) repliceren naar Azure](site-recovery-vmm-to-azure.md)
* [Hyper-V-machines repliceren naar een secundair datacenter](site-recovery-vmm-to-vmm.md)
* Als u repliceert naar een secundair datacenter meer informatie over [ondersteunde gastbesturingssystemen voor Hyper-V-machines](https://technet.microsoft.com/library/mt126277.aspx).
* Als u naar Azure repliceert, alle gastbesturingssystemen die ondersteuning biedt voor siteherstel [ondersteund door Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Kan ik virtuele machines beveiligen als Hyper-V wordt uitgevoerd op een clientbesturingssysteem?
Nee, virtuele machines moeten zich bevinden op een Hyper-V-hostserver die wordt uitgevoerd op een ondersteunde Windows-servercomputer. Als u wilt beveiligen van een clientcomputer kan u deze als een fysieke machine repliceert [Azure](site-recovery-vmware-to-azure.md) of een [secundair datacenter](site-recovery-vmware-to-vmware.md).

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Welke workloads kan ik met Site Recovery beveiligen?
U kunt Site Recovery gebruiken om de meeste workloads die worden uitgevoerd op een ondersteunde virtuele machine of fysieke server te beveiligen. Site Recovery biedt ondersteuning voor toepassingsgevoelige replicatie, zodat apps kunnen worden hersteld naar een intelligente status. Het kan worden geïntegreerd met Microsoft-toepassingen zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory en werkt uitstekend samen met toonaangevende leveranciers, waaronder Oracle, SAP, IBM en Red Hat. Lees [hier](site-recovery-workload.md) meer informatie over workloadbeveiliging.

### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Moeten Hyper-V-hosts in VMM-clouds worden?
Als u repliceren naar een secundair datacenter wilt moet Hyper-V-machines op host Hyper-V van servers die zich bevinden in een VMM-cloud. Als u repliceren naar Azure wilt, kunt u virtuele machines repliceren met of zonder VMM-clouds. [Lees meer](tutorial-hyper-v-to-azure.md) over Hyper-V-replicatie naar Azure.

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Kan ik Site Recovery met VMM implementeren als ik maar één VMM-server heb?

Ja. Kunt u virtuele machines in Hyper-V-servers in de VMM-cloud naar Azure repliceren of u kunt repliceren tussen VMM-clouds op dezelfde server. Voor on-premises lokale replicatie, we raden u aan een VMM-server in de primaire en secundaire sites.  

### <a name="what-physical-servers-can-i-protect"></a>Welke fysieke servers kan ik beveiligen?
U kunt fysieke servers met Windows en Linux naar Azure of een secundaire site repliceren. [Meer informatie over](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) besturingssysteemvereisten.  Dezelfde vereisten toepassen of u fysieke servers naar Azure of naar een secundaire site repliceert.


Houd er rekening mee dat er fysieke servers als virtuele machines in Azure wordt uitgevoerd als de lokale server uitvalt. Failback naar een on-premises fysieke server wordt momenteel niet ondersteund. U kunt alleen een failback naar een virtuele machine van VMware beveiligd als fysieke machine.

### <a name="what-vmware-vms-can-i-protect"></a>Welke virtuele VMware-machines kan ik beveiligen?

Als u virtuele VMware-machines wilt beveiligen, hebt u een vSphere-hypervisor nodig, evenals virtuele machines waarop VMware-hulpprogramma's worden uitgevoerd. We raden ook het gebruik aan van een VMware vCenter-server om de hypervisors te beheren. [Meer informatie](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) over de juiste vereisten voor het repliceren van VMware-servers en virtuele machines naar Azure of naar een secundaire site.


### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Kan ik met Site Recovery herstel na noodgeval voor mijn filialen beheren?
Ja. Wanneer u Site Recovery gebruiken om replicatie en failover te organiseren in uw filialen, krijgt u een uniforme orchestration en de weergave van alle werkbelastingen van uw branch office op een centrale locatie. Vanuit het hoofdkantoor kunt u eenvoudig failovers uitvoeren en herstel na noodgeval beheren voor alle filialen. Het is niet nodig de afzonderlijke filialen te bezoeken.

## <a name="pricing"></a>Prijzen
Voor vragen over prijzen, raadpleegt u de veelgestelde vragen op [prijzen voor Azure Site Recovery](https://azure.microsoft.com/en-in/pricing/details/site-recovery/).

## <a name="security"></a>Beveiliging
### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Worden er replicatiegegevens verzonden naar de Site Recovery-service?
Nee, Site Recovery gerepliceerde gegevens niet onderscheppen en heeft geen informatie over wat er op uw virtuele machines of fysieke servers wordt uitgevoerd.
Er worden alleen replicatiegegevens uitgewisseld tussen uw on-premises Hyper-V-hosts, VMware-hypervisors of fysieke servers en de Azure-opslag of uw secundaire site. Met Site Recovery kunnen deze gegevens niet worden onderschept. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  

Site Recovery is ISO 27001: 2013, 27018, HIPAA-, DPA gecertificeerd en is bezig te SOC2 en FedRAMP JAB-beoordelingen.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Zelfs de metagegevens van onze lokale moet om wettelijke redenen binnen dezelfde geografische regio blijven. Site Recovery kan helpen ons?
Ja. Wanneer u een Site Recovery-kluis in een regio maakt, zorgt u ervoor dat alle metagegevens die we moet inschakelen en organisatie van replicatie en failover blijft binnen die regio's geografische grens.

### <a name="does-site-recovery-encrypt-replication"></a>Wordt replicatie met Site Recovery versleuteld?
Voor virtuele machines en fysieke servers wordt repliceren tussen lokale sites van versleuteling in doorvoer ondersteund. Voor virtuele machines en fysieke servers repliceren naar Azure, zowel versleuteling in de doorvoer en [versleuteling in rust (in Azure)](https://docs.microsoft.com/azure/storage/storage-service-encryption) worden ondersteund.

## <a name="replication"></a>Replicatie

### <a name="can-i-replicate-over-a-site-to-site-vpn-to-azure"></a>Kan ik repliceren via een VPN site-naar-site naar Azure?
Azure Site Recovery repliceert gegevens naar een Azure storage-account via een openbaar eindpunt. Replicatie is niet via een VPN site-naar-site. U kunt een VPN site-naar-site maken met een Azure-netwerk. Dit verstoren replicatie van Site Recovery niet.

### <a name="can-i-use-expressroute-to-replicate-virtual-machines-to-azure"></a>Kan ik virtuele machines repliceren naar Azure ExpressRoute gebruiken?
Ja, ExpressRoute kan worden gebruikt voor het repliceren van virtuele machines naar Azure. Azure Site Recovery repliceert gegevens naar een Azure Storage-Account via een openbaar eindpunt. U moet instellen [openbare peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) met ExpressRoute voor replicatie van Site Recovery. Nadat een virtuele Azure-netwerk is niet via de virtuele machines kunt u deze kunt openen met behulp van de [privépeering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering) ingesteld voor de virtuele Azure-netwerk.

### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Zijn er vereisten voor het repliceren van virtuele machines naar Azure?
Virtuele machines die u wilt repliceren naar Azure moet voldoen aan [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

Uw Azure gebruikersaccount moet zijn bepaalde [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) replicatie van een nieuwe virtuele machine naar Azure in te schakelen.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Kan ik virtuele machines van de tweede generatie Hyper-V repliceren naar Azure?
Ja. Site Recovery converteert van generatie 2 naar generatie 1 tijdens failover. Bij de failback wordt de machine weer teruggezet naar generatie 2 geconverteerd. [Meer informatie](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Hoe betaal ik voor virtuele Azure-machines als ik naar Azure repliceer?
Gegevens worden gerepliceerd naar geo-redundante Azure-opslag en u hoeft niet te betalen eventuele kosten van de virtuele machine Azure IaaS bieden een groot voordeel tijdens een normale replicatie. Wanneer u een failover naar Azure uitvoert, maakt Site Recovery automatisch virtuele Azure IaaS-machines. Vervolgens worden er kosten in rekening gebracht voor de rekenresources die u in Azure verbruikt.

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Kan ik Site Recovery-scenario's met een SDK automatiseren?
Ja. U kunt Site Recovery-werkstromen automatiseren met de Rest API-, PowerShell- of Azure-SDK. Ondersteunde scenario's voor de implementatie van Site Recovery met behulp van PowerShell:

* [Hyper-V-machines in VMMs-clouds repliceren naar Azure PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
* [Hyper-V-machines zonder VMM repliceren naar Azure PowerShell-Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)

### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Als ik repliceer naar Azure, wat voor opslagaccount heb ik dan nodig?
U moet een LRS of GRS-opslagaccount. GRS wordt aanbevolen, omdat de gegevens dan flexibel zijn te gebruiken als er sprake is van regionale uitval of als de primaire regio niet kan worden hersteld. Het account moet zich in dezelfde regio bevinden als de Recovery Services-kluis. Premium-opslag wordt ondersteund voor VMware VM, Hyper-V-machine en replicatie van de fysieke server, wanneer u Site Recovery in Azure portal implementeert.

### <a name="how-often-can-i-replicate-data"></a>Hoe vaak kan ik gegevens repliceren?
* **Hyper-V:** Hyper-V-machines kunnen elke 30 seconden (met uitzondering van premium-opslag), 5 minuten of 15 minuten worden gerepliceerd. Als u de SAN-replicatie hebt ingesteld zijn replicatie is synchroon.
* **VMware en fysieke servers:** hier is de replicatiefrequentie niet relevant. Replicatie is continue.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Kan ik de replicatie van bestaande herstelsite naar een andere tertiaire site uitbreiden?
Uitgebreide of gekoppelde replicatie wordt niet ondersteund. Deze functie in aanvragen [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Kan ik de eerste keer dat ik naar Azure repliceer, offline replicatie uitvoeren?
Nee, dit wordt niet ondersteund. Aanvragen van deze functie in de [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>Kan ik bepaalde schijven uitsluiten van replicatie?
Dit wordt ondersteund wanneer u klaar [repliceren van virtuele VMware-machines en Hyper-V-machines](site-recovery-exclude-disk.md) naar Azure met behulp van de Azure-portal.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Kan ik virtuele machines met dynamische schijven repliceren?
Dynamische schijven worden ondersteund bij het repliceren van virtuele Hyper-V-machines. Ze worden ook ondersteund als virtuele VMware-machines en fysieke machines repliceren naar Azure. De besturingssysteemschijf moet een standaardschijf zijn.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Kan ik een nieuwe machine toevoegen aan een bestaande replicatiegroep
Nieuwe machines toevoegen aan bestaande replicatiegroepen wordt ondersteund. Selecteer de replicatiegroep (van 'Gerepliceerde items' blade) en klik met de rechtermuisknop/Selecteer contextmenu in de replicatiegroep en selecteer de relevante optie om dit te doen.

![Toevoegen aan replicatiegroep](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Kan ik de bandbreedte beperken die is toegewezen voor Hyper-V-replicatieverkeer?
Ja. Meer informatie over het beperken van de bandbreedte in de artikelen over implementatie:

* [Capaciteitsplanning voor het repliceren van virtuele VMware-machines en fysieke servers](site-recovery-plan-capacity-vmware.md)
* [Capaciteitsplanning voor Hyper-V-machines repliceren naar Azure](site-recovery-capacity-planning-for-hyper-v-replication.md)

## <a name="failover"></a>Failover
### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Als ik ben failover wordt uitgevoerd naar Azure, hoe krijg ik toegang tot de virtuele Azure-machines na een failover?
U kunt de virtuele Azure-machines benaderen via een beveiligde internetverbinding, via een VPN tussen sites of via Azure ExpressRoute. U moet een aantal zaken om verbinding te kunnen voorbereiden. [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Als ik naar Azure hoe Azure ervoor zorgen dat een failover is mijn gegevens tegen?
Azure is ontworpen voor herstelbaarheid. Site Recovery voert automatisch een failover naar een secundair Azure datacenter, in overeenstemming met de Azure SLA indien nodig. Als dit gebeurt, we zorgen ervoor dat uw metagegevens en kluizen binnen dezelfde geografische regio die u hebt gekozen voor uw kluis blijven.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Als ik repliceer tussen twee datacentra wat gebeurt er als mijn primaire datacenter een stroomstoring optreedt?
U kunt een ongeplande failover vanuit de secundaire site activeren. Site Recovery heeft geen connectiviteit vanuit de primaire site nodig om de failover uit te voeren.

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?
Failover wordt niet automatisch uitgevoerd. U start een failover met één klik in de portal of u kunt [Site Recovery PowerShell](/powershell/module/azurerm.siterecovery) voor het activeren van een failover. Terug mislukt, is een eenvoudige actie in de Site Recovery-portal.

Als u wilt automatiseren u kunnen lokale Orchestrator of Operations Manager gebruiken voor het detecteren van de virtuele machine is vastgelopen en vervolgens de failover activeren met de SDK.

* [Lees meer](site-recovery-create-recovery-plans.md) over plannen voor herstel.
* [Lees meer](site-recovery-failover.md) over failover.
* [Lees meer](site-recovery-failback-azure-to-vmware.md) over mislukte back-VMware-machines en fysieke servers

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-failover-back-to-a-different-host"></a>Als mijn lokale host niet reageert of vastgelopen, kan ik failover naar een andere host?
Ja, u kunt het herstel van alternatieve locatie failback naar een andere host van Azure. Meer informatie over de opties in de onderstaande koppelingen voor VMware en Hyper-v virtuele machines.

* [Voor virtuele VMware-machines](site-recovery-how-to-failback-azure-to-vmware.md#fail-back-to-the-original-or-alternate-location)
* [Voor Hyper-v virtuele machines](site-recovery-failback-from-azure-to-hyper-v.md#failback-to-an-alternate-location)

## <a name="service-providers"></a>Serviceproviders
### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Ik ben een serviceprovider. Werkt Site Recovery voor toegewezen als gedeelde infrastructuurmodellen?
Ja, Site Recovery werkt voor zowel toegewezen als gedeelde infrastructuurmodellen.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Voor een serviceprovider, wordt de identiteit van mijn tenants gedeeld met de Site Recovery-service?
Nee. Tenant-identiteit blijft anonieme. Uw tenants hebben geen toegang nodig tot de Site Recovery-portal. Alleen de beheerder van de serviceprovider communiceert met de portal.

### <a name="will-tenant-application-data-ever-go-to-azure"></a>Gaat tenant toepassingsgegevens ooit naar Azure?
Bij replicatie tussen sites van serviceproviders worden er nooit toepassingsgegevens naar Azure overgezet. Gegevens worden versleuteld in transit en rechtstreeks gerepliceerd tussen de sites van serviceproviders.

Als u naar Azure repliceert, worden er toepassingsgegevens verzonden naar Azure-opslag, maar niet naar de Site Recovery-service. Gegevens in transit is versleuteld en blijven versleuteld in Azure.

### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Ontvangen mijn tenants een factuur voor Azure-services?
Nee. Azure-services worden rechtstreeks aan de serviceprovider gefactureerd. Serviceproviders zijn zelf verantwoordelijk voor het genereren van facturen voor hun tenants.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Als ik naar Azure repliceer, moet ik dan altijd een virtuele machine in Azure uitvoeren?
Nee, gegevens worden gerepliceerd naar Azure storage-account in uw abonnement. Wanneer u een failovertest (details voor DR) of een werkelijke failover uitvoert, maakt Site Recovery automatisch virtuele machines in uw abonnement.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Wordt er gezorgd voor isolatie op tenantniveau wanneer ik naar Azure repliceer?
Ja.

### <a name="what-platforms-do-you-currently-support"></a>Welke platforms worden er momenteel ondersteund?
We bieden ondersteuning voor Azure Pack, Cloud Platform System en System Center gebaseerde (2012 en hoger)-implementaties. [Meer informatie](https://technet.microsoft.com/library/dn850370.aspx) over de integratie van Azure Pack en de Site Recovery.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Worden implementaties van één Azure Pack en één VMM-server ondersteund?
Ja, kunt u Hyper-V virtuele machines repliceren naar Azure of tussen sites van serviceproviders.  Houd er rekening mee dat als u tussen sites van serviceproviders repliceert, Azure-runbookintegratie niet beschikbaar.

## <a name="next-steps"></a>Volgende stappen
* Lees het [Site Recovery-overzicht](site-recovery-overview.md)
* Informatie over de [Site Recovery-architectuur](site-recovery-components.md)  

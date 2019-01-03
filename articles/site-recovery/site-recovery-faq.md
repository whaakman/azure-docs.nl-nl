---
title: 'Azure Site Recovery: Veelgestelde vragen | Microsoft Docs'
description: Dit artikel worden besproken populaire vragen over Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: raynew
ms.openlocfilehash: 0c70d3b5166b0e3719aa621091920d2c91696bf1
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53973533"
---
# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: veelgestelde vragen (FAQ)
Dit artikel bevat veelgestelde vragen over Azure Site Recovery. Als u vragen hebt na het lezen van dit artikel, plaatst u deze op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).

## <a name="general"></a>Algemeen
### <a name="what-does-site-recovery-do"></a>Wat doet Site Recovery?
Site Recovery draagt bij aan uw zakelijke continuïteit en noodherstelplan (BCDR), door indelen en automatiseren van de replicatie van Azure-VM's tussen regio's, on-premises virtuele machines en fysieke servers naar Azure, en on-premises machines naar een secundair datacenter. [Meer informatie](site-recovery-overview.md).

### <a name="what-can-site-recovery-protect"></a>Wat kunt u met Site Recovery beveiligen?
* **Azure-VM's**: Site Recovery kan elke workload die worden uitgevoerd op een ondersteunde Azure-machine gerepliceerd
* **Hyper-V virtuele machines**: Site Recovery kan elke werkbelasting die wordt uitgevoerd op een Hyper-V-virtuele machine te beschermen.
* **Fysieke servers**: Site Recovery kan fysieke servers met Windows of Linux beschermen.
* **Virtuele VMware-machines**: Site Recovery kan elke workload die worden uitgevoerd in een VMware-VM te beveiligen.



### <a name="can-i-replicate-azure-vms"></a>Kan ik virtuele Azure-machines repliceren?
Ja, kunt u ondersteunde Azure-VM's repliceren tussen Azure-regio's. [Meer informatie](site-recovery-azure-to-azure.md).

### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Wat moet ik in Hyper-V om replicatie met Site Recovery?
Wat u voor de Hyper-V-hostserver nodig hebt, is afhankelijk van het implementatiescenario. Controleer de lijst met vereisten voor Hyper-V in:

* [Hyper-V-machines (zonder VMM) repliceren naar Azure](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V-machines (met VMM) repliceren naar Azure](site-recovery-vmm-to-azure.md)
* [Hyper-V-machines repliceren naar een secundair datacenter](site-recovery-vmm-to-vmm.md)
* Als u repliceert naar een secundair datacenter meer informatie over [ondersteunde gastbesturingssystemen voor Hyper-V-machines](https://technet.microsoft.com/library/mt126277.aspx).
* Als u naar Azure repliceert, Site Recovery biedt ondersteuning voor alle gastbesturingssystemen die worden [ondersteund door Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Kan ik virtuele machines beveiligen als Hyper-V wordt uitgevoerd op een clientbesturingssysteem?
Nee, virtuele machines moeten zich bevinden op een Hyper-V-hostserver die wordt uitgevoerd op een ondersteunde Windows-servercomputer. Als u wilt beveiligen van een clientcomputer kan u deze als een fysieke machine repliceert [Azure](site-recovery-vmware-to-azure.md) of een [secundair datacenter](site-recovery-vmware-to-vmware.md).

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Welke workloads kan ik met Site Recovery beveiligen?
U kunt Site Recovery gebruiken om de meeste workloads die worden uitgevoerd op een ondersteunde virtuele machine of fysieke server te beschermen. Site Recovery biedt ondersteuning voor toepassingsgevoelige replicatie, zodat apps kunnen worden hersteld naar een intelligente status. Het kan worden geïntegreerd met Microsoft-toepassingen zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory en werkt nauw samen met toonaangevende leveranciers zoals Oracle, SAP, IBM en Red Hat. Lees [hier](site-recovery-workload.md) meer informatie over workloadbeveiliging.

### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Moeten Hyper-V-hosts zich in de VMM-clouds?
Als u repliceren naar een secundair datacenter, wilt en vervolgens de Hyper-V-machines moeten zich op Hyper-V-hosts servers die zich bevinden in een VMM-cloud. Als u repliceren naar Azure wilt, kunt u virtuele machines repliceren met of zonder VMM-clouds. [Lees meer](tutorial-hyper-v-to-azure.md) over Hyper-V-replicatie naar Azure.

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Kan ik Site Recovery met VMM implementeren als ik maar één VMM-server heb?

Ja. Kunt u virtuele machines in Hyper-V-servers in de VMM-cloud naar Azure repliceren, of u kunt repliceren tussen VMM-clouds op dezelfde server. On-premises naar on-premises replicatie, wordt u aangeraden dat er een VMM-server in de primaire en secundaire sites.  

### <a name="what-physical-servers-can-i-protect"></a>Welke fysieke servers kan ik beveiligen?
U kunt fysieke servers met Windows en Linux naar Azure of naar een secundaire site repliceren. Meer informatie over vereisten voor [replicatie naar Azure](vmware-physical-azure-support-matrix.md#replicated-machines), en [replicatie naar een secundaire site](vmware-physical-secondary-support-matrix.md#replicated-vm-support).


Houd er rekening mee dat de fysieke servers wordt uitgevoerd als virtuele machines in Azure als uw on-premises server uitvalt. Failback uitvoeren naar een on-premises fysieke server wordt momenteel niet ondersteund. U kunt alleen een failback naar een virtuele VMware-machine voor een beveiligd als fysieke machine.

### <a name="what-vmware-vms-can-i-protect"></a>Welke virtuele VMware-machines kan ik beveiligen?

Als u virtuele VMware-machines wilt beveiligen, hebt u een vSphere-hypervisor nodig, evenals virtuele machines waarop VMware-hulpprogramma's worden uitgevoerd. We raden ook het gebruik aan van een VMware vCenter-server om de hypervisors te beheren. Meer informatie over de vereisten voor [replicatie naar Azure](vmware-physical-azure-support-matrix.md#replicated-machines), of [replicatie naar een secundaire site](vmware-physical-secondary-support-matrix.md#replicated-vm-support).


### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Kan ik met Site Recovery herstel na noodgeval voor mijn filialen beheren?
Ja. Wanneer u Site Recovery gebruiken voor het indelen van replicatie en failover in uw filialen, krijgt u een uniforme indeling en de weergave van alle werkbelastingen van uw vertakking office op een centrale locatie. Vanuit het hoofdkantoor kunt u eenvoudig failovers uitvoeren en herstel na noodgeval beheren voor alle filialen. Het is niet nodig de afzonderlijke filialen te bezoeken.

## <a name="pricing"></a>Prijzen
Voor vragen over prijzen, raadpleegt u de veelgestelde vragen op [prijzen voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).

## <a name="security"></a>Beveiliging
### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Worden er replicatiegegevens verzonden naar de Site Recovery-service?
Nee, Site Recovery geen gerepliceerde gegevens worden onderschept en heeft geen informatie over wat wordt uitgevoerd op uw virtuele machines of fysieke servers.
Er worden alleen replicatiegegevens uitgewisseld tussen uw on-premises Hyper-V-hosts, VMware-hypervisors of fysieke servers en de Azure-opslag of uw secundaire site. Met Site Recovery kunnen deze gegevens niet worden onderschept. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  

Site Recovery is ISO 27001: 2013, 27018, HIPAA, DPA gecertificeerd en wordt momenteel SOC2 en FedRAMP JAB-beoordelingen.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Verband met nalevingsvereisten moet ook de metagegevens van onze on-premises binnen dezelfde geografische regio blijven. Site Recovery kan helpen ons?
Ja. Wanneer u een Site Recovery-kluis in een regio maken, we ervoor zorgen dat alle metagegevens die we moet inschakelen en replicatie en failover blijft binnen deze regio's geografische grens.

### <a name="does-site-recovery-encrypt-replication"></a>Wordt replicatie met Site Recovery versleuteld?
Voor virtuele machines en fysieke servers wordt repliceren tussen on-premises sites versleuteling-in-transit ondersteund. Voor virtuele machines en fysieke servers repliceren naar Azure, zowel versleuteling-in-transit en [versleuteling-at-rest (in Azure)](https://docs.microsoft.com/azure/storage/storage-service-encryption) worden ondersteund.

## <a name="replication"></a>Replicatie

### <a name="can-i-replicate-over-a-site-to-site-vpn-to-azure"></a>Kan ik repliceren via een VPN-site-naar-site naar Azure?
Azure Site Recovery repliceert gegevens naar een Azure storage-account via een openbaar eindpunt. Replicatie is niet via een site-naar-site-VPN. U kunt een VPN-site-naar-site maken met een Azure-netwerk. Dit niet leiden tot problemen met Site Recovery-replicatie.

### <a name="can-i-use-expressroute-to-replicate-virtual-machines-to-azure"></a>Kan ik ExpressRoute gebruiken voor het repliceren van virtuele machines naar Azure?
Ja, [ExpressRoute kan worden gebruikt](concepts-expressroute-with-site-recovery.md) voor het repliceren van on-premises virtuele machines naar Azure. Azure Site Recovery repliceert gegevens naar een Azure Storage-Account via een openbaar eindpunt. U moet instellen [openbare peering](../expressroute/expressroute-circuit-peerings.md#publicpeering) of [Microsoft-peering](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) ExpressRoute gebruiken voor Site Recovery-replicatie. Microsoft-peering is de aanbevolen routeringsdomein voor replicatie. Zorg ervoor dat de [vereisten voor netwerken](vmware-azure-configuration-server-requirements.md#network-requirements) ook voor replicatie wordt voldaan. Nadat de virtuele machines een failover zijn naar een Azure virtual network kunt u ze openen met behulp van de [privépeering](../expressroute/expressroute-circuit-peerings.md#privatepeering) installatie met behulp van Azure virtual network. Replicatie wordt niet ondersteund via persoonlijke peering.

### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Zijn er vereisten voor het repliceren van virtuele machines naar Azure?
[Virtuele VMware-machines](vmware-physical-azure-support-matrix.md#replicated-machines) en [Hyper-V-machines](hyper-v-azure-support-matrix.md#replicated-vms) u wilt repliceren naar Azure, moet voldoen aan de vereisten voor Azure.

Uw Azure-gebruikersaccount moet bepaalde [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) replicatie van een nieuwe virtuele machine naar Azure in te schakelen.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Kan ik virtuele machines van de tweede generatie Hyper-V repliceren naar Azure?
Ja. Site Recovery converteert van generatie 2 naar generatie 1 tijdens de failover. Bij de failback wordt de machine geconverteerd naar generatie 2. [Meer informatie](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Hoe betaal ik voor virtuele Azure-machines als ik naar Azure repliceer?
Tijdens een normale replicatie, gegevens worden gerepliceerd naar geografisch redundante opslag met Azure en u hoeft te betalen Azure IaaS VM kosten, bieden een belangrijk voordeel. Wanneer u een failover naar Azure uitvoert, maakt Site Recovery automatisch virtuele Azure IaaS-machines. Vervolgens worden er kosten in rekening gebracht voor de rekenresources die u in Azure verbruikt.

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Kan ik Site Recovery-scenario's met een SDK automatiseren?
Ja. U kunt Site Recovery-werkstromen automatiseren met de Rest API-, PowerShell- of Azure-SDK. Momenteel ondersteunde scenario's voor het implementeren van Site Recovery met behulp van PowerShell:

* [Hyper-V-machines in VMMs-clouds repliceren naar Azure PowerShell Resource Manager](hyper-v-vmm-powershell-resource-manager.md)
* [Virtuele Hyper-V-machines zonder VMM repliceren naar Azure PowerShell Resource Manager](hyper-v-azure-powershell-resource-manager.md)
* [Het repliceren van VMware naar Azure met PowerShell Resource Manager](vmware-azure-disaster-recovery-powershell.md)

### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Als ik repliceer naar Azure, wat voor opslagaccount heb ik dan nodig?
U moet een LRS of GRS-opslagaccount. GRS wordt aanbevolen, omdat de gegevens dan flexibel zijn te gebruiken als er sprake is van regionale uitval of als de primaire regio niet kan worden hersteld. Het account moet zich in dezelfde regio bevinden als de Recovery Services-kluis. Premium storage wordt ondersteund voor VMware-VM, Hyper-V-VM en fysieke servers repliceren, bij het implementeren van Site Recovery in Azure portal.

### <a name="how-often-can-i-replicate-data"></a>Hoe vaak kan ik gegevens repliceren?
* **Hyper-V:** Hyper-V-machines kunnen elke 30 seconden (met uitzondering van premium storage), 5 minuten of 15 minuten worden gerepliceerd. Als u de SAN-replicatie hebt ingesteld zijn replicatie is synchroon.
* **Azure-VM's, VMware en fysieke servers:** Een replicatiefrequentie is hier niet relevant. Replicatie is continue.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Kan ik replicatie van bestaande herstelsite naar een andere tertiaire site verlengen?
Uitgebreide of gekoppelde replicatie wordt niet ondersteund. Deze functie in aanvragen [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Kan ik de eerste keer dat ik naar Azure repliceer, offline replicatie uitvoeren?
Nee, dit wordt niet ondersteund. Aanvragen van deze functie in de [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>Kan ik bepaalde schijven uitsluiten van replicatie?
Dit wordt ondersteund als u repliceert virtuele VMware-machines en Hyper-V-machines naar Azure met behulp van de Azure-portal.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Kan ik virtuele machines met dynamische schijven repliceren?
Dynamische schijven worden ondersteund bij het repliceren van virtuele Hyper-V-machines. Ze worden ook ondersteund bij het repliceren van virtuele VMware-machines en fysieke machines naar Azure. De besturingssysteemschijf moet een standaardschijf.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Kan ik een nieuwe virtuele machine toevoegen aan een bestaande replicatiegroep?
Nieuwe machines toevoegen aan bestaande replicatiegroepen wordt ondersteund. Om dit te doen, selecteert u de replicatiegroep (van de blade 'Gerepliceerde items') en het contextmenu of te selecteren Klik met de rechtermuisknop op de replicatiegroep en selecteer de gewenste optie.

![Toevoegen aan de replicatiegroep](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Kan ik de bandbreedte beperken die is toegewezen voor Hyper-V-replicatieverkeer?
Ja. U kunt meer lezen over het beperken van de bandbreedte in de artikelen over implementatie:

* [Capaciteitsplanning voor het repliceren van virtuele VMware-machines en fysieke servers](site-recovery-plan-capacity-vmware.md)
* [Capaciteitsplanning voor Hyper-V-machines repliceren naar Azure](site-recovery-capacity-planning-for-hyper-v-replication.md)

## <a name="failover"></a>Failover
### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Als ik ben Failover-overschakeling uitvoeren naar Azure, hoe krijg ik toegang tot de virtuele Azure-machines na een failover?
U kunt de virtuele Azure-machines benaderen via een beveiligde internetverbinding, via een VPN tussen sites of via Azure ExpressRoute. U moet een aantal dingen om verbinding te kunnen voorbereiden. [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Als ik een failover naar Azure, hoe Azure ervoor zorgen dat is mijn gegevens tegen?
Azure is ontworpen voor herstelbaarheid. Site Recovery is voert automatisch een failover naar een secundaire Azure-datacenter, in overeenstemming met de Azure SLA als de noodzaak daartoe zich voordoet. Als dit gebeurt, zorgen wij ervoor dat uw metagegevens en kluizen binnen dezelfde geografische regio die u hebt gekozen voor uw kluis blijven.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Als ik repliceer tussen twee datacenters wat gebeurt er als mijn primaire datacenter een stroomstoring optreedt?
U kunt een ongeplande failover vanuit de secundaire site activeren. Site Recovery heeft geen connectiviteit vanuit de primaire site nodig om de failover uit te voeren.

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?
Failover wordt niet automatisch uitgevoerd. Initiëren van failover met één klik in de portal of kunt u [Site Recovery PowerShell](/powershell/module/azurerm.siterecovery) dat een failover wordt geactiveerd. Failback is een eenvoudige actie in de Site Recovery-portal.

Voor het automatiseren van u kunnen on-premises Orchestrator of Operations Manager gebruiken voor het detecteren van een storing van de virtuele machine en vervolgens de failover activeren met de SDK.

* [Lees meer](site-recovery-create-recovery-plans.md) over plannen voor herstel.
* [Lees meer](site-recovery-failover.md) over failover.
* [Lees meer](site-recovery-failback-azure-to-vmware.md) over mislukte back-VMware-machines en fysieke servers

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-failover-back-to-a-different-host"></a>Als mijn on-premises-host niet reageert of vastgelopen, kan ik failover naar een andere host?
Ja, kunt u de alternatieve locatie herstellen naar een andere host failback van Azure. Meer informatie over de opties in de onderstaande koppelingen voor VMware en Hyper-V virtuele machines.

* [Voor virtuele VMware-machines](concepts-types-of-failback.md#alternate-location-recovery-alr)
* [Voor Hyper-V virtuele machines](hyper-v-azure-failback.md#perform-failback)

## <a name="service-providers"></a>Serviceproviders
### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Ik ben een serviceprovider. Werkt Site Recovery voor toegewezen als gedeelde infrastructuurmodellen?
Ja, Site Recovery werkt voor zowel toegewezen als gedeelde infrastructuurmodellen.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Voor een serviceprovider, is de identiteit van mijn tenants gedeeld met de Site Recovery-service?
Nee. Tenant-identiteit blijft anonieme. Uw tenants hebben geen toegang nodig tot de Site Recovery-portal. Alleen de beheerder van de serviceprovider communiceert met de portal.

### <a name="will-tenant-application-data-ever-go-to-azure"></a>Gaat de gegevens van de tenant ooit naar Azure?
Bij replicatie tussen sites van serviceproviders worden er nooit toepassingsgegevens naar Azure overgezet. Gegevens worden versleuteld in-transit en rechtstreeks gerepliceerd tussen sites van de serviceprovider.

Als u naar Azure repliceert, worden er toepassingsgegevens verzonden naar Azure-opslag, maar niet naar de Site Recovery-service. Gegevens in transit versleuteld en blijven versleuteld in Azure.

### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Ontvangen mijn tenants een factuur voor Azure-services?
Nee. Azure-services worden rechtstreeks aan de serviceprovider gefactureerd. Serviceproviders zijn zelf verantwoordelijk voor het genereren van facturen voor hun tenants.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Als ik naar Azure repliceer, moet ik dan altijd een virtuele machine in Azure uitvoeren?
Nee, de gegevens worden gerepliceerd naar een Azure storage-account in uw abonnement. Wanneer u een failovertest (details voor DR) of een werkelijke failover uitvoert, maakt Site Recovery automatisch virtuele machines in uw abonnement.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Wordt er gezorgd voor isolatie op tenantniveau wanneer ik naar Azure repliceer?
Ja.

### <a name="what-platforms-do-you-currently-support"></a>Welke platforms worden er momenteel ondersteund?
We bieden ondersteuning voor Azure Pack, Cloud Platform System en System Center op basis van implementaties met (2012 en hoger). [Meer informatie](https://technet.microsoft.com/library/dn850370.aspx) over de integratie van Azure Pack en Site Recovery.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Worden implementaties van één Azure Pack en één VMM-server ondersteund?
Ja, kunt u Hyper-V-machines repliceren naar Azure, of tussen sites van serviceproviders.  Houd er rekening mee dat als u tussen sites van serviceproviders repliceert, Azure-runbookintegratie niet beschikbaar is.

## <a name="next-steps"></a>Volgende stappen
* Lees het [Site Recovery-overzicht](site-recovery-overview.md)
* Informatie over de [Site Recovery-architectuur](site-recovery-components.md)  

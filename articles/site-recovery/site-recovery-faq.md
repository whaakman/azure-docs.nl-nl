---
title: 'Azure Site Recovery: Veelgestelde vragen | Microsoft Docs'
description: Dit artikel worden besproken populaire vragen over Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 4/08/2019
ms.author: raynew
ms.openlocfilehash: 74ccc76ff139cae21e3583b0fea11596f5fd6b62
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413926"
---
# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: veelgestelde vragen (FAQ)
In dit artikel bevat een overzicht van de veelgestelde vragen over Azure Site Recovery.</br>
Voor bepaalde query's op verschillende ASR scenario verwijzen we u scenario Veelgestelde vragen over de specifieke.<br>

- [Herstel van virtuele Azure-machine na noodgevallen naar Azure](azure-to-azure-common-questions.md)
- [Herstel van virtuele VMware-machine na noodgevallen naar Azure](vmware-azure-common-questions.md)
- [Noodherstel voor Hyper-V-VM naar Azure](hyper-v-azure-common-questions.md)
 
## <a name="general"></a>Algemeen

### <a name="what-does-site-recovery-do"></a>Wat doet Site Recovery?
Site Recovery draagt bij aan uw zakelijke continuïteit en noodherstelplan (BCDR), door indelen en automatiseren van de replicatie van Azure-VM's tussen regio's, on-premises virtuele machines en fysieke servers naar Azure, en on-premises machines naar een secundair datacenter. [Meer informatie](site-recovery-overview.md).

### <a name="can-i-protect-a-virtual-machine-that-has-a-docker-disk"></a>Kan ik een virtuele machine waarvoor een Docker-schijf beveiligen?

Nee, dit is een niet-ondersteund scenario.

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
Nee, de gegevens worden gerepliceerd naar Azure-opslag in uw abonnement. Wanneer u een failovertest (details voor DR) of een werkelijke failover uitvoert, maakt Site Recovery automatisch virtuele machines in uw abonnement.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Wordt er gezorgd voor isolatie op tenantniveau wanneer ik naar Azure repliceer?
Ja.

### <a name="what-platforms-do-you-currently-support"></a>Welke platforms worden er momenteel ondersteund?
We bieden ondersteuning voor Azure Pack, Cloud Platform System en System Center op basis van implementaties met (2012 en hoger). [Meer informatie](https://technet.microsoft.com/library/dn850370.aspx) over de integratie van Azure Pack en Site Recovery.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Worden implementaties van één Azure Pack en één VMM-server ondersteund?
Ja, kunt u Hyper-V-machines repliceren naar Azure, of tussen sites van serviceproviders.  Houd er rekening mee dat als u tussen sites van serviceproviders repliceert, Azure-runbookintegratie niet beschikbaar is.

## <a name="pricing"></a>Prijs

### <a name="where-can-i-find-pricing-information"></a>Waar vind ik informatie over prijzen?
Beoordeling [prijzen voor Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) details.


### <a name="how-can-i-calculate-approximate-charges-during-the-use-of-site-recovery"></a>Hoe kan ik de geschatte kosten in rekening gebracht tijdens het gebruik van Site Recovery berekenen?

U kunt de [prijscalculator](https://aka.ms/asr_pricing_calculator) kosten wilt ramen tijdens het gebruik van Site Recovery.

Voor gedetailleerde raming van kosten, voer de tool voor implementatieplanning voor [VMware](https://aka.ms/siterecovery_deployment_planner) of [Hyper-V](https://aka.ms/asr-deployment-planner), en gebruik de [kosten van het rapport met kostenramingen](https://aka.ms/asr_DP_costreport).


### <a name="managed-disks-are-now-used-to-replicate-vmware-vms-and-physical-servers-do-i-incur-additional-charges-for-the-cache-storage-account-with-managed-disks"></a>Beheerde schijven zijn nu gebruikt voor het repliceren van virtuele VMware-machines en fysieke servers. Moet ik rekenen extra kosten in rekening gebracht voor de cache-opslagaccount met beheerde schijven?

Nee, er zijn geen extra kosten berekend voor de cache. Als u naar standard storage-account repliceert, is deze cacheopslag deel uitmaken van hetzelfde doel-opslagaccount.

### <a name="i-have-been-an-azure-site-recovery-user-for-over-a-month-do-i-still-get-the-first-31-days-free-for-every-protected-instance"></a>Ik ben al meer dan een maand gebruiker van Azure Site Recovery. Krijg ik nog steeds de eerste 31 dagen gratis voor elke beschermde instantie?

Ja. Voor elk beschermd exemplaar worden geen Azure Site Recovery-kosten in rekening gebracht tijdens de eerste 31 dagen. Bijvoorbeeld, als u hebt 10 exemplaren voor de laatste zes maanden en u een exemplaar van 11 verbinden met Azure Site Recovery, zijn er geen kosten in rekening gebracht voor het 11e exemplaar voor de eerste 31 dagen. De eerste 10 exemplaren steeds Azure Site Recovery kosten in rekening gebracht omdat ze hebt beschermd gedurende meer dan 31 dagen.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>Worden er gedurende de eerste 31 dagen andere Azure-kosten in rekening gebracht?

Ja, hoewel de Site Recovery gratis is gedurende de eerste 31 dagen van een beschermd exemplaar, worden er mogelijk kosten in rekening gebracht voor Azure Storage, opslagtransacties en gegevensoverdracht. Voor een herstelde virtuele machine worden mogelijk ook Azure-rekenkosten in rekening gebracht.


### <a name="is-there-a-cost-associated-to-perform-disaster-recovery-drillstest-failover"></a>Is er een kosten die gepaard gaan om uit te voeren disaster recovery oefeningen/test-failover?

Er is geen afzonderlijke kosten voor noodherstelanalyse. Er zijn rekenkosten in rekening gebracht nadat de virtuele machine is gemaakt na de testfailover.



## <a name="security"></a>Beveiliging

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Worden er replicatiegegevens verzonden naar de Site Recovery-service?
Nee, Site Recovery geen gerepliceerde gegevens worden onderschept en heeft geen informatie over wat wordt uitgevoerd op uw virtuele machines of fysieke servers.
Er worden alleen replicatiegegevens uitgewisseld tussen uw on-premises Hyper-V-hosts, VMware-hypervisors of fysieke servers en de Azure-opslag of uw secundaire site. Met Site Recovery kunnen deze gegevens niet worden onderschept. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  

Site Recovery is ISO 27001: 2013, 27018, HIPAA, DPA gecertificeerd en wordt momenteel SOC2 en FedRAMP JAB-beoordelingen.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Verband met nalevingsvereisten moet ook de metagegevens van onze on-premises binnen dezelfde geografische regio blijven. Site Recovery kan helpen ons?
Ja. Wanneer u een Site Recovery-kluis in een regio maken, we ervoor zorgen dat alle metagegevens die we moet inschakelen en replicatie en failover blijft binnen deze regio's geografische grens.

### <a name="does-site-recovery-encrypt-replication"></a>Wordt replicatie met Site Recovery versleuteld?
Voor virtuele machines en fysieke servers wordt repliceren tussen on-premises sites versleuteling-in-transit ondersteund. Voor virtuele machines en fysieke servers repliceren naar Azure, zowel versleuteling-in-transit en [versleuteling-at-rest (in Azure)](https://docs.microsoft.com/azure/storage/storage-service-encryption) worden ondersteund.




## <a name="disaster-recovery"></a>Herstel na noodgeval

### <a name="what-can-site-recovery-protect"></a>Wat kunt u met Site Recovery beveiligen?
* **Azure-VM's**: Site Recovery kan elke workload die worden uitgevoerd op een ondersteunde Azure-machine gerepliceerd
* **Hyper-V virtuele machines**: Site Recovery kan elke werkbelasting die wordt uitgevoerd op een Hyper-V-virtuele machine te beschermen.
* **Fysieke servers**: Site Recovery kan fysieke servers met Windows of Linux beschermen.
* **Virtuele VMware-machines**: Site Recovery kan elke workload die worden uitgevoerd in een VMware-VM te beveiligen.

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Welke workloads kan ik met Site Recovery beveiligen?
U kunt Site Recovery gebruiken om de meeste workloads die worden uitgevoerd op een ondersteunde virtuele machine of fysieke server te beschermen. Site Recovery biedt ondersteuning voor toepassingsgevoelige replicatie, zodat apps kunnen worden hersteld naar een intelligente status. Het kan worden geïntegreerd met Microsoft-toepassingen zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory en werkt nauw samen met toonaangevende leveranciers zoals Oracle, SAP, IBM en Red Hat. Lees [hier](site-recovery-workload.md) meer informatie over workloadbeveiliging.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Kan ik met Site Recovery herstel na noodgeval voor mijn filialen beheren?
Ja. Wanneer u Site Recovery gebruiken voor het indelen van replicatie en failover in uw filialen, krijgt u een uniforme indeling en de weergave van alle werkbelastingen van uw vertakking office op een centrale locatie. Vanuit het hoofdkantoor kunt u eenvoudig failovers uitvoeren en herstel na noodgeval beheren voor alle filialen. Het is niet nodig de afzonderlijke filialen te bezoeken.


### <a name="is-disaster-recovery-supported-for-azure-vms"></a>Wordt herstel na noodgevallen voor Azure VM's ondersteund?

Ja, ondersteunt Site Recovery na noodgevallen voor Azure-VM's tussen Azure-regio's. [Lees de veelgestelde vragen over](azure-to-azure-common-questions.md) over noodherstel van de virtuele machine van Azure.

### <a name="is-disaster-recovery-supported-for-vmware-vms"></a>Wordt herstel na noodgevallen voor VMware-VM's ondersteund?

Ja, Site Recovery biedt ondersteuning voor herstel na noodgevallen van on-premises VMware-VM's. [Lees de veelgestelde vragen over](vmware-azure-common-questions.md) voor herstel na noodgevallen van virtuele VMware-machines.

### <a name="is-disaster-recovery-supported-for-hyper-v-vms"></a>Wordt herstel na noodgevallen voor Hyper-V virtuele machines ondersteund?
Ja, Site Recovery biedt ondersteuning voor herstel na noodgevallen van on-premises Hyper-V-machines. [Lees de veelgestelde vragen over](hyper-v-azure-common-questions.md) voor herstel na noodgevallen van Hyper-V-machines.

## <a name="is-disaster-recovery-supported-for-physical-servers"></a>Wordt herstel na noodgevallen voor fysieke servers ondersteund?
Ja, Site Recovery biedt ondersteuning voor herstel na noodgevallen van on-premises fysieke servers met Windows en Linux naar Azure of naar een secundaire site. Meer informatie over vereisten voor herstel na noodgevallen naar [Azure](vmware-physical-azure-support-matrix.md#replicated-machines), en zo de[een secundaire site](vmware-physical-secondary-support-matrix.md#replicated-vm-support).
Houd er rekening mee dat fysieke servers wordt uitgevoerd als virtuele machines in Azure na een failover. Failback van Azure naar een on-premises fysieke server wordt momenteel niet ondersteund. U kunt alleen een failover naar een virtuele machine van VMware.





## <a name="replication"></a>Replicatie

### <a name="can-i-replicate-over-a-site-to-site-vpn-to-azure"></a>Kan ik repliceren via een VPN-site-naar-site naar Azure?
Azure Site Recovery repliceert gegevens naar Azure storage-account of beheerde schijven, via een openbaar eindpunt. Replicatie is niet via een site-naar-site-VPN. 

### <a name="why-cant-i-replicate-over-vpn"></a>Waarom kan ik niet repliceren via VPN?

Wanneer u naar Azure repliceren, bereikt replicatieverkeer in de openbare eindpunten van een Azure Storage. Zo kunt u alleen repliceren via het openbare internet met ExpressRoute (openbare peering) en VPN werkt niet.

### <a name="can-i-use-riverbed-steelheads-for-replication"></a>Kan ik Riverbed SteelHeads voor replicatie gebruiken?

Onze partner, Riverbed, bevat gedetailleerde richtlijnen over het werken met Azure Site Recovery. Lees hun [handleiding](https://community.riverbed.com/s/article/DOC-4627).

### <a name="can-i-use-expressroute-to-replicate-virtual-machines-to-azure"></a>Kan ik ExpressRoute gebruiken voor het repliceren van virtuele machines naar Azure?
Ja, [ExpressRoute kan worden gebruikt](concepts-expressroute-with-site-recovery.md) voor het repliceren van on-premises virtuele machines naar Azure.

- Azure Site Recovery repliceert gegevens naar een Azure Storage via een openbaar eindpunt. U moet instellen [openbare peering](../expressroute/expressroute-circuit-peerings.md#publicpeering) of [Microsoft-peering](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) ExpressRoute gebruiken voor Site Recovery-replicatie.
- Microsoft-peering is de aanbevolen routeringsdomein voor replicatie.
- Nadat de virtuele machines een failover zijn naar een Azure virtual network kunt u ze openen met behulp van de [privépeering](../expressroute/expressroute-circuit-peerings.md#privatepeering) installatie met behulp van Azure virtual network.
- Replicatie wordt niet ondersteund via persoonlijke peering.
- Als u VMware-machines of fysieke machines beschermen wilt, zorgt u ervoor dat de configuratieserver voldoet aan [netwerkvereisten](vmware-azure-configuration-server-requirements.md#network-requirements) voor replicatie. 



### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-or-managed-disk-do-i-need"></a>Als ik naar Azure repliceer, wat voor soort storage-account of een beheerde schijf heb ik nodig?

U moet een LRS of GRS-opslag. GRS wordt aanbevolen, omdat de gegevens dan flexibel zijn te gebruiken als er sprake is van regionale uitval of als de primaire regio niet kan worden hersteld. Het account moet zich in dezelfde regio bevinden als de Recovery Services-kluis. Premium storage wordt ondersteund voor VMware-VM, Hyper-V-VM en fysieke servers repliceren, bij het implementeren van Site Recovery in Azure portal. Beheerde schijven alleen ondersteunen LRS.

### <a name="how-often-can-i-replicate-data"></a>Hoe vaak kan ik gegevens repliceren?
* **Hyper-V:** Hyper-V-machines kunnen worden gerepliceerd om de vijf minuten, o 30 seconden (met uitzondering van premium storage)
* **Azure VM's, VMware-VM's, fysieke servers:** Een replicatiefrequentie is hier niet relevant. Replicatie is continue.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Kan ik replicatie van bestaande herstelsite naar een andere tertiaire site verlengen?
Uitgebreide of gekoppelde replicatie wordt niet ondersteund. Deze functie in aanvragen [Feedbackforum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Kan ik de eerste keer dat ik naar Azure repliceer, offline replicatie uitvoeren?
Nee, dit wordt niet ondersteund. Aanvragen van deze functie in de [Feedbackforum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>Kan ik bepaalde schijven uitsluiten van replicatie?
Dit wordt ondersteund als u repliceert virtuele VMware-machines en Hyper-V-machines naar Azure met behulp van de Azure-portal.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Kan ik virtuele machines met dynamische schijven repliceren?
Dynamische schijven worden ondersteund bij het repliceren van Hyper-V virtuele machines, en bij het repliceren van virtuele VMware-machines en fysieke machines naar Azure. De besturingssysteemschijf moet een standaardschijf.


### <a name="can-i-throttle-bandwidth-allotted-for-replication-traffic"></a>Kan ik de bandbreedte die is toegewezen voor replicatieverkeer beperken?
Ja. U kunt meer lezen over het beperken van de bandbreedte in deze artikelen:

* [Capaciteitsplanning voor het repliceren van virtuele VMware-machines en fysieke servers](site-recovery-plan-capacity-vmware.md)
* [Capaciteitsplanning voor Hyper-V-machines repliceren naar Azure](site-recovery-capacity-planning-for-hyper-v-replication.md)



## <a name="failover"></a>Failover
### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-vms-after-failover"></a>Als ik ben Failover-overschakeling uitvoeren naar Azure, hoe krijg ik toegang tot de Azure VM's na een failover?

U kunt de virtuele Azure-machines benaderen via een beveiligde internetverbinding, via een VPN tussen sites of via Azure ExpressRoute. U moet een aantal dingen om verbinding te kunnen voorbereiden. [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Als ik een failover naar Azure, hoe Azure ervoor zorgen dat is mijn gegevens tegen?
Azure is ontworpen voor herstelbaarheid. Site Recovery is al voert automatisch een failover naar een secundaire Azure-datacenter, in overeenstemming met de Azure SLA. Als dit gebeurt, zorgen wij ervoor dat uw metagegevens en kluizen binnen dezelfde geografische regio die u hebt gekozen voor uw kluis blijven.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Als ik repliceer tussen twee datacenters wat gebeurt er als mijn primaire datacenter een stroomstoring optreedt?
U kunt een ongeplande failover vanuit de secundaire site activeren. Site Recovery heeft geen connectiviteit vanuit de primaire site nodig om de failover uit te voeren.

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?
Failover wordt niet automatisch uitgevoerd. Initiëren van failover met één klik in de portal of kunt u [Site Recovery PowerShell](/powershell/module/az.recoveryservices) dat een failover wordt geactiveerd. Failback is een eenvoudige actie in de Site Recovery-portal.

Voor het automatiseren van u kunnen on-premises Orchestrator of Operations Manager gebruiken voor het detecteren van een storing van de virtuele machine en vervolgens de failover activeren met de SDK.

* [Lees meer](site-recovery-create-recovery-plans.md) over plannen voor herstel.
* [Lees meer](site-recovery-failover.md) over failover.
* [Lees meer](site-recovery-failback-azure-to-vmware.md) over mislukte back-VMware-machines en fysieke servers

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-fail-back-to-a-different-host"></a>Als mijn on-premises-host niet reageert of is vastgelopen, ik een failback naar een andere host uitvoeren kan?
Ja, kunt u de alternatieve locatie herstellen naar een andere host failback van Azure.

* [Voor virtuele VMware-machines](concepts-types-of-failback.md#alternate-location-recovery-alr)
* [Voor Hyper-V virtuele machines](hyper-v-azure-failback.md#perform-failback)

## <a name="automation"></a>Automation

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Kan ik Site Recovery-scenario's met een SDK automatiseren?
Ja. U kunt Site Recovery-werkstromen automatiseren met de Rest API-, PowerShell- of Azure-SDK. Momenteel ondersteunde scenario's voor het implementeren van Site Recovery met behulp van PowerShell:

* [Hyper-V-machines in VMMs-clouds repliceren naar Azure PowerShell Resource Manager](hyper-v-vmm-powershell-resource-manager.md)
* [Virtuele Hyper-V-machines zonder VMM repliceren naar Azure PowerShell Resource Manager](hyper-v-azure-powershell-resource-manager.md)
* [Het repliceren van VMware naar Azure met PowerShell Resource Manager](vmware-azure-disaster-recovery-powershell.md)

## <a name="componentprovider-upgrade"></a>Component/provider upgrade

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-site-recovery-upgrades"></a>Waar vind ik de release-opmerkingen/updatepakketten van Site Recovery-upgrades

[Informatie over](site-recovery-whats-new.md) over nieuwe updates en [rollup-informatie ophalen](service-updates-how-to.md).

## <a name="next-steps"></a>Volgende stappen
* Lees het [Site Recovery-overzicht](site-recovery-overview.md)


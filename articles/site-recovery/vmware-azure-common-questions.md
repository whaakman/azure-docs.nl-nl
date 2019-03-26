---
title: Veelgestelde vragen - VMware naar Azure-noodherstel met Azure Site Recovery | Microsoft Docs
description: In dit artikel bevat een overzicht van veelgestelde vragen bij het instellen van herstel na noodgevallen van on-premises VMware-machines naar Azure met Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 03/21/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: cdb8fe5deb71c014f7e0af01d070e5004d8c9994
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418785"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Veelgestelde vragen - VMware naar Azure-replicatie

In dit artikel vindt u antwoorden op veelgestelde vragen, we zien bij het implementeren van herstel na noodgevallen van on-premises VMware-machines naar Azure. Als u vragen hebt na het lezen van dit artikel, plaatst u deze op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Algemeen
### <a name="how-is-site-recovery-priced"></a>Hoe wordt de Site Recovery geprijsd?
Beoordeling [prijzen voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) details.

### <a name="how-do-i-pay-for-azure-vms"></a>Hoe moet ik betalen voor Azure VM's?
Tijdens de replicatie, gegevens worden gerepliceerd naar Azure storage en betaalt u geen wijzigingen in de virtuele machine. Wanneer u een failover naar Azure uitvoert, maakt Site Recovery automatisch virtuele machines van Azure IaaS. Hierna wordt u gefactureerd voor de rekenresources die u in Azure gebruikt.

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>Wat kan ik doen met VMware naar Azure-replicatie?
- **Herstel na noodgevallen**: U kunt volledige noodherstel instellen. In dit scenario kunt u on-premises VMware-machines repliceren naar Azure storage. Klik, als uw on-premises infrastructuur niet beschikbaar is, kunt u een failover naar Azure. Wanneer u een failover uitvoert, kan Azure-VM's worden gemaakt met behulp van de gerepliceerde gegevens. U kunt toegang tot apps en workloads op de Azure VM's, totdat uw on-premises datacenter weer beschikbaar is. U kunt vervolgens failback van Azure naar uw on-premises site.
- **Migratie**: U kunt Site Recovery gebruiken voor het migreren van on-premises VMware-machines naar Azure. In dit scenario kunt u on-premises VMware-machines repliceren naar Azure storage. Vervolgens kunt u een failover van on-premises naar Azure. Na een failover zijn uw toepassingen en workloads beschikbaar en worden uitgevoerd op Azure Virtual machines.

## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>Wat moet ik in Azure?
U moet een Azure-abonnement, een Recovery Services-kluis, een cache-opslagaccount, beheerde schijven en een virtueel netwerk. De kluis, cache-opslagaccount, beheerde schijven en netwerk moet zich in dezelfde regio bevinden.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Mijn Azure-account moet machtigingen voor het maken van virtuele machines?
Als u een abonnementsbeheerder bent, hebt u de replicatiemachtigingen die u nodig hebt. Als u niet bent, moet u machtigingen voor het maken van een Azure-VM in de resourcegroep en het virtuele netwerk dat u opgeeft wanneer u Site Recovery- en schrijfmachtigingen voor het geselecteerde opslagaccount configureren of een beheerde schijf op basis van uw configuratie. [Meer informatie](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).

### <a name="can-i-use-guest-os-server-license-on-azure"></a>Kan ik Gastbesturingssysteem server-licentie gebruiken op Azure?
Ja, Microsoft Software Assurance-klanten kunnen gebruikmaken van [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) om op te slaan op de licentiekosten voor **Windows Server-machines** die zijn gemigreerd naar Azure of naar het gebruik van Azure voor herstel na noodgevallen.

## <a name="pricing"></a>Prijzen

### <a name="how-are-licensing-charges-handled-during-replication-after-failover"></a>Hoe worden verwerkt licentieverlening kosten in rekening gebracht tijdens de replicatie na een failover?

Raadpleeg onze Veelgestelde vragen over licenties [hier](https://aka.ms/asr_pricing_FAQ) voor meer informatie.

### <a name="how-can-i-calculate-approximate-charges-during-the-use-of-site-recovery"></a>Hoe kan ik de geschatte kosten in rekening gebracht tijdens het gebruik van Site Recovery berekenen?

U kunt [prijscalculator](https://aka.ms/asr_pricing_calculator) kosten wilt ramen tijdens het gebruik van Azure Site Recovery. De tool voor implementatieplanning voor gedetailleerde raming van kosten worden uitgevoerd (https://aka.ms/siterecovery_deployment_planner) en analyseren van de [kosten van het rapport met kostenramingen](https://aka.ms/asr_DP_costreport).

### <a name="is-there-any-difference-in-cost-when-i-replicate-directly-to-managed-disk"></a>Is er een verschil in kosten als ik repliceer rechtstreeks op een beheerde schijf?

Beheerde schijven worden in rekening gebracht iets anders dan de storage-accounts. Zie het voorbeeld hieronder voor een schijf die als bron van de grootte van 100 GiB. Het voorbeeld is specifiek voor differentiële kosten voor opslag. Deze kosten omvat de kosten voor momentopnamen, cacheopslag en transacties.

* Standard storage-account Visual Studio. Standard HDD Managed Disk

    - **Ingerichte opslagschijf door Azure Site Recovery**: S10
    - **Standard-opslagaccount in rekening gebracht op volume verbruikt**: $5 per maand
    - **Standard-beheerde schijven in rekening gebracht op ingerichte volume**: $5.89 per maand

* Premium storage-account Visual Studio. Premium SSD Managed Disk 
    - **Ingerichte opslagschijf door Azure Site Recovery**: P10
    - **Premium storage-account in rekening gebracht op ingerichte volume**: $17.92 per maand
    - **Premium beheerde schijven in rekening gebracht op ingerichte volume**: $17.92 per maand

Meer informatie over [gedetailleerde prijsinformatie van beheerde schijven](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="do-i-incur-additional-charges-for-cache-storage-account-with-managed-disks"></a>Ik extra kosten voor Cache-Opslagaccount met beheerde schijven?

Nee, u doet geen extra kosten voor cache. Cache is altijd onderdeel van VMware naar Azure-architectuur. Als u naar standard storage-account repliceert, is deze cacheopslag deel uitmaken van hetzelfde doel-opslagaccount.

### <a name="i-have-been-an-azure-site-recovery-user-for-over-a-month-do-i-still-get-the-first-31-days-free-for-every-protected-instance"></a>Ik ben al meer dan een maand gebruiker van Azure Site Recovery. Krijg ik nog steeds de eerste 31 dagen gratis voor elke beschermde instantie?

Ja, het maakt niet uit hoe lang u Azure Site Recovery al gebruikt. Voor elk beschermd exemplaar worden geen Azure Site Recovery-kosten in rekening gebracht tijdens de eerste 31 dagen. Als u de laatste zes maanden bijvoorbeeld tien exemplaren hebt beschermd en een elfde exemplaar toevoegt aan Azure Site Recovery, worden er gedurende de eerste 31 dagen geen Azure Site Recovery-kosten in rekening gebracht voor het elfde exemplaar. Voor de eerste tien exemplaren worden er nog steeds Azure Site Recovery-kosten in rekening gebracht omdat ze al meer dan 31 dagen worden beschermd.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>Worden er gedurende de eerste 31 dagen andere Azure-kosten in rekening gebracht?

Ja, hoewel Azure Site Recovery gratis is gedurende de eerste 31 dagen van een beschermd exemplaar, worden er mogelijk kosten in rekening gebracht voor Azure Storage, opslagtransacties en gegevensoverdracht. Voor een herstelde virtuele machine worden mogelijk ook Azure-rekenkosten in rekening gebracht.

### <a name="what-charges-do-i-incur-while-using-azure-site-recovery"></a>Op welke kosten moet ik rekenen wanneer ik Azure Site Recovery gebruik?

Raadpleeg onze [Veelgestelde vragen over de kosten in rekening gebracht](https://aka.ms/asr_pricing_FAQ) voor gedetailleerde informatie.

### <a name="is-there-a-cost-associated-to-perform-dr-drillstest-failover"></a>Zijn er kosten gekoppeld aan de DR-oefeningen/test-failover uitvoeren?

Er is geen afzonderlijke kosten voor noodherstelanalyse. Er zijn rekenkosten in rekening gebracht nadat de virtuele machine is gemaakt na test-failover.

## <a name="azure-site-recovery-components-upgrade"></a>Azure Site Recovery-onderdelen bijwerken

### <a name="my-mobility-agentconfiguration-serverprocess-server-version-is-very-old-and-my-upgrade-has-failed-how-should-i-upgrade-to-latest-version"></a>De Mobility-agent/Configuration Server/versie van de processerver is zeer verouderd en wordt mijn upgrade is mislukt. Hoe moet ik een upgrade uitvoeren naar de nieuwste versie?

Azure Site Recovery volgt N-4 ondersteuningsmodel. Raadpleeg onze [ondersteuningsverklaring](https://aka.ms/asr_support_statement) om te begrijpen van de informatie over hoe u een upgrade van zeer oude versies.

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-azure-site-recovery"></a>Waar vind ik de release-opmerkingen/updatepakketten van Azure Site Recovery?

Raadpleeg de [document](https://aka.ms/asr_update_rollups) voor release-opmerkingen bij de informatie. Koppelingen van de installatie van de betreffende onderdelen vindt u in elke update totaliseren.

### <a name="how-should-i-upgrade-site-recovery-components-for-on-premises-vmware-or-physical-site-to-azure"></a>Hoe moet ik een upgrade Site Recovery-onderdelen voor on-premises VMware of fysieke-site naar Azure?

Raadpleeg onze richtlijnen voor het opgegeven [hier](https://aka.ms/asr_vmware_upgrades) uw onderdelen te upgraden.

## <a name="is-reboot-of-source-machine-mandatory-for-each-upgrade"></a>Opnieuw opstarten van de bronmachine verplicht is voor elke upgrade?

Hoewel aanbevolen, is dit het niet verplicht voor elke upgrade. Raadpleeg [hier](https://aka.ms/asr_vmware_upgrades) wissen voor richtlijnen.

## <a name="on-premises"></a>On-premises

### <a name="what-do-i-need-on-premises"></a>Wat kan ik on-premises nodig?

In on-premises hebt u het volgende nodig:
- Site Recovery-onderdelen geïnstalleerd op een enkele VMware-VM.
- Een VMware-infrastructuur, met ten minste één ESXi-host, en we raden u aan een vCenter-server.
- Een of meer virtuele VMware-machines te repliceren.

[Meer informatie](vmware-azure-architecture.md) over VMware naar Azure-architectuur.

De on-premises configuratieserver kan als volgt worden geïmplementeerd:

- We adviseren dat u de configuratieserver implementeren als een VMware-VM gebruiken een OVA-sjabloon met de configuratieserver vooraf zijn geïnstalleerd.
- Als u niet een sjabloon om een bepaalde reden gebruiken, kunt u de configuratieserver handmatig instellen. [Meer informatie](physical-azure-disaster-recovery.md#set-up-the-source-environment).



### <a name="where-do-on-premises-vms-replicate-to"></a>Waar repliceer on-premises machines naar?
Gegevens worden gerepliceerd naar Azure storage. Wanneer u een failover uitvoert, Site Recovery automatisch virtuele Azure-machines maakt van het storage-account of een beheerde schijf op basis van uw configuratie.

## <a name="replication"></a>Replicatie

### <a name="what-applications-can-i-replicate"></a>Welke toepassingen kan ik repliceren?
U kunt elke app of de werkbelasting wordt uitgevoerd op een VMware-VM die aan de voldoet repliceren [replicatievereisten](vmware-physical-azure-support-matrix.md##replicated-machines). Site Recovery biedt ondersteuning voor toepassingsgevoelige replicatie, zodat apps kunnen worden failover is uitgevoerd en kan niet naar een intelligente status. Site Recovery kan worden geïntegreerd met Microsoft-toepassingen zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory, en werkt nauw samen met toonaangevende leveranciers zoals Oracle, SAP, IBM en Red Hat. Lees [hier](site-recovery-workload.md) meer informatie over workloadbeveiliging.

### <a name="can-i-protect-a-virtual-machine-that-has-docker-disk-configuration"></a>Kan ik een virtuele machine waarvoor Docker-schijfconfiguratie beveiligen?

Nee, dit is een niet-ondersteund scenario.

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Kan ik repliceren naar Azure met een site-naar-site-VPN?
Site Recovery repliceert gegevens van on-premises naar Azure storage via een openbaar eindpunt of met behulp van openbare ExpressRoute-peering. Replicatie via een site-naar-site VPN-netwerk wordt niet ondersteund.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>Kan ik repliceren naar Azure met ExpressRoute?
Ja, ExpressRoute kan worden gebruikt voor het repliceren van virtuele machines naar Azure. Site Recovery repliceert gegevens naar Azure Storage via een openbaar eindpunt. U moet instellen [openbare peering](../expressroute/expressroute-circuit-peerings.md#publicpeering) of [Microsoft-peering](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) ExpressRoute gebruiken voor Site Recovery-replicatie. Microsoft-peering is de aanbevolen routeringsdomein voor replicatie. Zorg ervoor dat de [vereisten voor netwerken](vmware-azure-configuration-server-requirements.md#network-requirements) ook voor replicatie wordt voldaan. Nadat de virtuele machines een failover uitvoeren naar een Azure-netwerk, kunt u ze openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#privatepeering).

### <a name="how-can-i-change-storage-account-after-machine-is-protected"></a>Hoe kan ik storage-account wijzigen nadat de machine is beveiligd?

U moet uitschakelen en inschakelen van replicatie om te upgraden of downgraden van het opslagaccounttype.

### <a name="can-i-replicate-to-storage-accounts-for-new-machine"></a>Kan ik repliceren naar storage-accounts voor nieuwe machine?

Nee, vanaf maart-19, kunt u repliceren naar managed disks op Azure vanuit de portal. Replicatie naar storage-accounts voor een nieuwe virtuele machine is alleen beschikbaar via REST-API en Powershell. API-versie 2016-08-10- of 2018-01-10 gebruiken voor het repliceren naar storage-accounts.

### <a name="what-are-the-benefits-in-replicating-to-managed-disks"></a>Wat zijn de voordelen bij het repliceren naar managed disks?

Lees het artikel over hoe u [Azure Site Recovery herstel na noodgeval met beheerde schijven vereenvoudigt](https://azure.microsoft.com/blog/simplify-disaster-recovery-with-managed-disks-for-vmware-and-physical-servers/).

### <a name="how-can-i-change-managed-disk-type-after-machine-is-protected"></a>Hoe kan ik Managed Disk-type wijzigen nadat de machine is beveiligd?

Ja, kunt u eenvoudig het type beheerde schijf wijzigen. [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage). Echter wanneer u het type beheerde schijf wijzigt, zorg ervoor dat u wacht op de nieuwe herstelpunten moeten worden gegenereerd als u wilt testfailover of failover plaatsen deze activiteit.

### <a name="can-i-switch-the-replication-from-managed-disks-to-unmanaged-disks"></a>Kan ik de replicatie van beheerde schijven naar niet-beheerde schijven wisselen?

Nee, overstappen van beheerd naar niet-beheerde wordt niet ondersteund.

### <a name="why-cant-i-replicate-over-vpn"></a>Waarom kan ik niet repliceren via VPN?

Wanneer u naar Azure repliceren, replicatieverkeer bereikt de openbare eindpunten van een Azure Storage, dus u kunt alleen repliceren via het openbare internet met ExpressRoute (openbare peering) en VPN werkt niet.

### <a name="can-i-use-riverbed-steelheads-for-replication"></a>Kan ik Riverbed SteelHeads voor replicatie gebruiken?

Onze partner, Riverbed, bevat een gedetailleerde richtlijnen over het werken met Azure Site Recovery. Raadpleeg de [handleiding](https://community.riverbed.com/s/article/DOC-4627).

### <a name="what-are-the-replicated-vm-requirements"></a>Wat zijn de vereisten van de gerepliceerde VM's?

Voor replicatie, moet een ondersteund besturingssysteem op een VMware-VM worden uitgevoerd. Bovendien hebben de virtuele machine moet voldoen aan de vereisten voor Azure-VM's. [Meer informatie](vmware-physical-azure-support-matrix.md##replicated-machines) in de ondersteuningsmatrix.

### <a name="how-often-can-i-replicate-to-azure"></a>Hoe vaak kan ik repliceren naar Azure?
Replicatie is continue bij het repliceren van virtuele VMware-machines naar Azure.

### <a name="can-i-retain-the-ip-address-on-failover"></a>Kan ik het IP-adres bij failover bewaren?
Ja, kunt u het IP-adres bij failover behouden. Zorg ervoor dat u het doel-IP-adres op 'berekening en netwerk-blade voordat de failover wordt vermeld. Zorg er ook voor de machines afsluiten op het moment van failover naar het IP-conflicten te voorkomen dat op het moment van failback.

### <a name="can-i-extend-replication"></a>Kan ik replicatie verlengen?
Uitgebreide of gekoppelde replicatie wordt niet ondersteund. Deze functie in aanvragen [Feedbackforum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-initial-replication"></a>Kan ik een offline initiële replicatie?
Nee, dit wordt niet ondersteund. Aanvragen van deze functie in de [Feedbackforum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Kan ik schijven uitsluiten?
Ja, kunt u schijven uitsluiten van replicatie.

### <a name="can-i-change-the-target-vm-size-or-vm-type-before-failover"></a>Kan ik de doel-VM-grootte of het type van de virtuele machine voordat de failover wijzigen?
Ja, kunt u het type of de grootte van de virtuele machine elk gewenst moment voordat de failover door te gaan naar berekening en netwerk-instellingen van het item replicatie vanaf de portal.

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Kan ik virtuele machines met dynamische schijven repliceren?
Dynamische schijven kunnen worden gerepliceerd. De besturingssysteemschijf moet een standaardschijf.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Als ik gebruik replicatiegroepen voor multi-VM-consistentie, kan ik toevoegen een nieuwe virtuele machine aan een bestaande replicatiegroep?
Ja, kunt u nieuwe virtuele machines toevoegen aan een bestaande replicatiegroep wanneer u replicatie voor deze inschakelt. U kunt een virtuele machine niet toevoegen aan een bestaande replicatiegroep nadat replicatie is gestart, en u een replicatiegroep voor bestaande VM's maken kunt.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Kan ik virtuele machines die worden gerepliceerd door toe te voegen of het formaat van schijven wijzigen?

U kunt de schijfgrootte wijzigen voor VMware-replicatie naar Azure. Als u wilt toevoegen van nieuwe schijven moet u de schijf toevoegen en weer inschakelen van beveiliging voor de virtuele machine.

### <a name="can-i-migrate-on-premises-machines-to-a-new-vcenter-without-impacting-ongoing-replication"></a>Kan ik migreren op lokale machines met een Vcenter-nieuwe zonder enige impact op de doorlopende replicatie?
Wijziging van de Vcenter- of de migratie is niet het geval is, van invloed op doorlopende replicatie. U moet Azure Site Recovery met de nieuwe Vcenter instellen en replicatie inschakelen voor machines.

### <a name="can-i-replicate-to-cachetarget-storage-account-which-has-a-vnet-with-azure-storage-firewalls-configured-on-it"></a>Kan ik repliceren naar cache/doel-opslagaccount met een Vnet (met Azure storage-firewalls) geconfigureerd voor?
Azure Site Recovery biedt Nee, geen ondersteuning voor replicatie naar de opslag op Vnet.

## <a name="configuration-server"></a>Configuratieserver

### <a name="what-does-the-configuration-server-do"></a>Wat doet de configuratieserver?
De configuratieserver wordt uitgevoerd de on-premises Site Recovery-onderdelen, met inbegrip van:
- De configuratieserver coördineert de communicatie tussen on-premises en Azure en beheert de gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens; Met caching, compressie en versleuteling, optimaliseert en verzendt die deze naar Azure storage-., de processerver installeert ook Mobility-Service op VM's die u wilt repliceren en detecteert automatisch on-premises virtuele VMware-machines.
- De hoofddoelserver die verantwoordelijk is voor replicatiegegevens tijdens de failback vanuit Azure.

[Meer informatie](vmware-azure-architecture.md) over de onderdelen van de configuratieserver en processen.

### <a name="where-do-i-set-up-the-configuration-server"></a>Waar kan ik de configuratieserver instellen?
In dat geval moet u een één maximaal beschikbare on-premises virtuele VMware-machine in voor de configuratieserver.

### <a name="what-are-the-requirements-for-the-configuration-server"></a>Wat zijn de vereisten voor de configuratieserver?

Controleer de [vereisten](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>Kan ik handmatig instellen de configuratieserver in plaats van een sjabloon?
Het is raadzaam dat u de meest recente versie van de OVF-sjabloon [maken van de configuratieserver VM](vmware-azure-deploy-configuration-server.md). Als voor een of andere reden is niet mogelijk, zoals u geen toegang tot de VMware-server, kunt u [de geïntegreerde Setup-bestand downloaden](physical-azure-set-up-source.md) vanuit de portal en uitvoeren op een virtuele machine.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Kan een configuratieserver in meer dan één regio repliceren?
Nee. Om dit te doen, moet u voor het instellen van een configuratieserver in elke regio.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Kan ik een configuratieserver in Azure hosten?
Tijdens het mogelijk is moet de virtuele Azure-machine waarop de configuratieserver wordt uitgevoerd om te communiceren met uw on-premises VMware-infrastructuur en virtuele machines. Dit kunt latenties toevoegen en voortdurende replicatie van invloed zijn op.

### <a name="how-do-i-update-the-configuration-server"></a>Hoe kan ik de configuratieserver bijwerken?
[Meer informatie over](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) bijwerken van de configuratieserver. U vindt de meest recente update-informatie in de [updates van de Azure-pagina](https://azure.microsoft.com/updates/?product=site-recovery). U kunt ook rechtstreeks downloaden de nieuwste versie van de configuratieserver in [Microsoft Download Center](https://aka.ms/asrconfigurationserver). Als uw versie ouder dan 4 versies van de huidige versie is, raadpleegt u onze [ondersteuningsverklaring](https://aka.ms/asr_support_statement) voor richtlijnen voor upgrades.

### <a name="should-i-backup-the-deployed-configuration-server"></a>Moet ik back-up van het geïmplementeerde configuratie-server?
We raden u aan om regelmatig geplande back-ups van de configuratieserver. Voor succesvolle failback wordt de virtuele machine wordt een failback moet aanwezig zijn in de database van de configuratieserver en de configuratieserver moet worden uitgevoerd en een verbonden status heeft. U kunt meer informatie over algemene beheertaken voor configuration server [hier](vmware-azure-manage-configuration-server.md).

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Als ik van de configuratieserver instelt ben, kan ik handmatig downloaden en installeren MySQL?

Ja. Downloaden van MySQL en plaats deze in de **C:\Temp\ASRSetup** map. Installeer deze vervolgens handmatig. Bij het instellen van de virtuele machine van de configuratieserver en de voorwaarden accepteren, MySQL wordt vermeld als **al geïnstalleerd** in **Download en installeer**.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>Kan ik te voorkomen dat het downloaden van MySQL, maar kunt Site Recovery installeren?

Ja. Download het installatieprogramma van MySQL en plaats deze in de **C:\Temp\ASRSetup** map.  Bij het instellen van de virtuele machine van de configuratieserver, de voorwaarden accepteren en klik op **Download en installeer**, de portal wordt het installatieprogramma dat u hebt toegevoegd voor het installeren van MySQL gebruikt.
 
### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>Kan ik de virtuele machine van de configuratieserver voor iets anders gebruiken?
Nee, moet u de virtuele machine alleen gebruiken voor de configuratieserver. 

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>Kan ik een configuratieserver klonen en voor het indelen van gebruiken?
Nee, moet u een nieuwe configuratieserver om te voorkomen dat problemen met registratie instellen.

### <a name="can-i-change-the-vault-registered-in-the-configuration-server"></a>Kan ik de kluis geregistreerd op de configuratieserver wijzigen?
Nee. Nadat een kluis is geregistreerd bij de configuratieserver, kan niet worden gewijzigd. Beoordeling [in dit artikel](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) voor herregistratie stappen.

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>Ik kan de dezelfde configuratieserver voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers gebruiken
Ja, maar houd er rekening mee dat de fysieke computer kan worden alleen mogelijk is naar een VMware-VM.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>Waar kan ik de wachtwoordzin voor de configuratieserver downloaden?
[Lees dit artikel](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) voor meer informatie over het downloaden van de wachtwoordzin.

### <a name="where-can-i-download-vault-registration-keys"></a>Waar kan ik registratiesleutels kluis downloaden?

In de **Recovery Services-kluis**, **beheren** > **infrastructuur voor Site Recovery** > **configuratieservers**. In **Servers**, selecteer **registratiesleutel downloaden** voor het downloaden van het bestand met kluisreferenties.



## <a name="mobility-service"></a>Mobility-service

### <a name="where-can-i-find-the-mobility-service-installers"></a>Waar vind ik de Mobility-service-installatieprogramma's?
Het installatieprogramma's zijn ondergebracht in de **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** map op de configuratieserver.

## <a name="how-do-i-install-the-mobility-service"></a>Hoe installeer ik de Mobility-service?
U installeert op elke virtuele machine die u repliceren wilt, met behulp van een [push-installatie](vmware-physical-mobility-service-overview.md#push-installation), of [handmatige installatie](vmware-physical-mobility-service-overview.md#install-mobility-agent-through-ui) vanuit de gebruikersinterface of Powershell. U kunt ook implementeren met behulp van een implementatieprogramma zoals [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md).



## <a name="security"></a>Beveiliging

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Welke toegang Site Recovery moet VMware-servers
Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Instellen van een VMware-VM met de configuratieserver en andere on-premises Site Recovery-onderdelen. [Meer informatie](vmware-azure-deploy-configuration-server.md)
- Virtuele machines automatisch detecteren voor replicatie. Meer informatie over een account voorbereiden voor automatische detectie. [Meer informatie](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Welke toegang Site Recovery moet VMware-VM's?

- Als u wilt repliceren, moet een VMware-VM hebben de Site Recovery Mobility service geïnstalleerd en wordt uitgevoerd. U kunt het hulpprogramma handmatig implementeren of u kunt opgeven dat Site Recovery een push-installatie van de dienst doen moet wanneer u replicatie voor een virtuele machine inschakelt. Site Recovery moet voor de push-installatie een account die kan worden gebruikt voor het installeren van de service zijn. [Meer informatie](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation). De processerver (standaard uitgevoerd op de configuratieserver) uitvoert deze installatie. [Meer informatie](vmware-azure-install-mobility-service.md) over de installatie van de Mobility-service.
- Tijdens de replicatie communiceren de virtuele machines als volgt met Site Recovery:
    - Virtuele machines communiceren met de configuratieserver op poort 443 voor HTTPS voor het replicatiebeheer van.
    - Virtuele machines verzenden replicatiegegevens naar de processerver op HTTPS-poort 9443 (kan worden gewijzigd).
    - Als u multi-VM-consistentie inschakelt, wordt de status van virtuele machines communiceren met elkaar via poort 20004.


### <a name="is-replication-data-sent-to-site-recovery"></a>Worden er replicatiegegevens verzonden met Site Recovery?
Nee, Site Recovery gerepliceerde gegevens niet worden onderschept, en heeft geen informatie over wat wordt uitgevoerd op uw virtuele machines. Replicatiegegevens worden uitgewisseld tussen VMware-hypervisors en Azure storage. Met Site Recovery kunnen deze gegevens niet worden onderschept. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  

Site Recovery is ISO 27001: 2013, 27018, HIPAA, DPA gecertificeerd en wordt momenteel SOC2 en FedRAMP JAB-beoordelingen.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>Kunnen we de metagegevens van on-premises bewaren binnen een geografische regio's?
Ja. Wanneer u een kluis in een regio maken, zorgen we dat alle metagegevens die wordt gebruikt door Site Recovery blijft binnen een geografische grens van die regio.

### <a name="does-site-recovery-encrypt-replication"></a>Wordt replicatie met Site Recovery versleuteld?
Ja, zowel versleuteling-in-transit en [versleuteling in Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) worden ondersteund.


## <a name="failover-and-failback"></a>Failover en failback
### <a name="can-i-use-the-process-server-at-on-premises-for-failback"></a>Kan ik de processerver op on-premises voor failback gebruiken?
Het is raadzaam een processerver maken in Azure voor failback doel om te voorkomen dat gegevensoverdracht latenties. Bovendien, als u het Bronnetwerk met virtuele machines met de Azure-gerichte netwerk op de configuratieserver gescheiden, is essentieel is voor het gebruik van de processerver voor failback gemaakt in Azure.

### <a name="how-far-back-can-i-recover"></a>Hoe ver terug kan ik gegevens herstellen?
Voor VMware naar Azure is het oudste herstelpunt dat u kunt gebruiken dit 72 uur.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Hoe krijg ik toegang tot Azure-VM's na een failover?
Na een failover, kunt u Azure-VM's openen via een beveiligde internetverbinding, via een site-naar-site-VPN of via Azure ExpressRoute. U moet een aantal dingen om verbinding te kunnen voorbereiden. [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Failover wordt uitgevoerd gegevens tegen?
Azure is ontworpen voor herstelbaarheid. Site Recovery is ontworpen voor failover naar een secundaire Azure-datacenter, in overeenstemming met de Azure SLA. Wanneer een failover optreedt, zorgen wij ervoor dat uw metagegevens en kluizen binnen dezelfde geografische regio die u hebt gekozen voor uw kluis blijven.

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?
[Failover](site-recovery-failover.md) wordt niet automatisch uitgevoerd. Initiëren van failover met één klik in de portal of kunt u [PowerShell](/powershell/module/azurerm.siterecovery) dat een failover wordt geactiveerd.

### <a name="can-i-fail-back-to-a-different-location"></a>Kan ik een failback uitvoeren naar een andere locatie?
Ja, als u een failover naar Azure, kunt u failover naar een andere locatie als de oorspronkelijke map is niet beschikbaar. [Meer informatie](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>Waarom heb ik een VPN of ExpressRoute voor failback nodig?
Wanneer u een failback van Azure, gegevens van Azure is gekopieerd naar uw on-premises virtuele machine en persoonlijke toegang is vereist.

### <a name="can-i-resize-the-azure-vm-after-failover"></a>Kan ik het formaat van de virtuele Azure-machine na een failover?
Nee, u kunt de grootte of het type van de doel-VM niet wijzigen na de failover.


## <a name="automation-and-scripting"></a>Automation en scripting

### <a name="can-i-set-up-replication-with-scripting"></a>Kan ik replicatie met het uitvoeren van scripts instellen?
Ja. U kunt met behulp van de Rest-API, PowerShell of de SDK van Azure Site Recovery-werkstromen automatiseren. [Meer](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Prestaties en capaciteit
### <a name="can-i-throttle-replication-bandwidth"></a>Kan ik de replicatiebandbreedte beperken?
Ja. [Meer informatie](site-recovery-plan-capacity-vmware.md).


## <a name="next-steps"></a>Volgende stappen
* [Beoordeling](vmware-physical-azure-support-matrix.md) vereisten ondersteunen.
* [Instellen van](vmware-azure-tutorial.md) VMware naar Azure-replicatie.

---
title: Veelgestelde vragen over VMware naar Azure-noodherstel met Azure Site Recovery | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over herstel na noodgevallen van on-premises VMware-machines naar Azure met behulp van Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 05/10/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: 4cc305e51113e67eb389bde8cb4cd28a3de8e3df
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969309"
---
# <a name="common-questions-about-vmware-to-azure-replication"></a>Veelgestelde vragen over VMware naar Azure-replicatie

In dit artikel vindt u antwoorden op veelgestelde vragen die voordoen zich wanneer u herstel na noodgevallen van on-premises virtuele VMware-machines (VM's) in Azure implementeert.

## <a name="general"></a>Algemeen

### <a name="what-do-i-need-for-vmware-vm-disaster-recovery"></a>Wat heb ik nodig voor noodherstel van VMware-VM?

[Meer informatie over de onderdelen betrokken](vmware-azure-architecture.md) in herstel na noodgevallen van virtuele VMware-machines.

### <a name="can-i-use-site-recovery-to-migrate-vmware-vms-to-azure"></a>Kan ik Site Recovery gebruiken voor het migreren van virtuele VMware-machines naar Azure?

Ja. Naast het gebruik van Site Recovery kunt u volledige noodherstel instellen voor VMware-VM's, kunt u Site Recovery ook gebruiken voor het migreren van on-premises VMware-machines naar Azure. In dit scenario kunt u on-premises VMware-machines repliceren naar Azure Storage. Vervolgens kunt u een failover van on-premises naar Azure. Na een failover zijn uw toepassingen en workloads beschikbaar en worden uitgevoerd op Azure Virtual machines. Het proces is, zoals het instellen van volledige noodherstel, behalve dat de migratie van een systeem u kunt geen failback van Azure.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Mijn Azure-account moet machtigingen voor het maken van virtuele machines?

Als u een abonnementsbeheerder bent, hebt u de replicatiemachtigingen die u nodig hebt. Als u niet een beheerder bent, moet u machtigingen voor de volgende acties uitvoeren:

- Een Azure-VM maken in de resource groep en het virtuele netwerk u dat u opgeeft wanneer u Site Recovery configureert.
- Schrijven naar het geselecteerde opslagaccount of een beheerde schijf op basis van uw configuratie.

[Meer informatie](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) over vereiste machtigingen.

### <a name="what-applications-can-i-replicate"></a>Welke toepassingen kan ik repliceren?

U kunt repliceren elke app of de werkbelasting wordt uitgevoerd op een VMware-VM die voldoet aan de [replicatievereisten](vmware-physical-azure-support-matrix.md#replicated-machines).

- Site Recovery biedt ondersteuning voor toepassingsgevoelige replicatie, zodat apps kunnen worden failover is uitgevoerd en kan niet naar een intelligente status.
- Site Recovery worden geïntegreerd met Microsoft-toepassingen zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory. Het werkt ook nauw samen met toonaangevende leveranciers zoals Oracle, SAP, IBM en Red Hat.

Lees [hier](site-recovery-workload.md) meer informatie over workloadbeveiliging.

### <a name="can-i-use-a-guest-os-server-license-on-azure"></a>Kan ik een gast-OS-server-licentie gebruiken op Azure?

Ja, Microsoft Software Assurance-klanten kunnen gebruikmaken van [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) op te slaan op de licentiekosten voor Windows Server-machines die worden gemigreerd naar Azure, of om Azure te gebruiken voor herstel na noodgevallen.

## <a name="security"></a>Beveiliging

### <a name="what-access-to-vmware-servers-does-site-recovery-need"></a>Welke toegang tot de VMware-servers is nodig voor Site Recovery?

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Instellen van een VMware-VM waarop de Site Recovery-configuratieserver wordt uitgevoerd.
- Virtuele machines automatisch detecteren voor replicatie.

### <a name="what-access-to-vmware-vms-does-site-recovery-need"></a>Welke toegang tot de VMware-VM's is nodig voor Site Recovery?

- Als u wilt repliceren, moet een VMware-VM hebben de Site Recovery Mobility service geïnstalleerd en wordt uitgevoerd. U kunt het hulpprogramma handmatig implementeren of u kunt opgeven dat Site Recovery een push-installatie van de service doen wanneer u replicatie voor een virtuele machine inschakelt.
- Tijdens de replicatie communiceren de virtuele machines als volgt met Site Recovery:
    - Virtuele machines communiceren met de configuratieserver op HTTPS-poort 443 voor replicatie.
    - Virtuele machines verzenden replicatiegegevens naar de processerver op HTTPS-poort 9443. (Deze instelling kan worden gewijzigd.)
    - Als u multi-VM-consistentie inschakelt, wordt de status van virtuele machines communiceren met elkaar via poort 20004.

### <a name="is-replication-data-sent-to-site-recovery"></a>Worden er replicatiegegevens verzonden met Site Recovery?

Nee, Site Recovery geen gerepliceerde gegevens worden onderschept en heeft geen informatie over wat wordt uitgevoerd op uw virtuele machines. Replicatiegegevens worden uitgewisseld tussen VMware-hypervisors en Azure Storage. Met Site Recovery kunnen deze gegevens niet worden onderschept. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  

Site Recovery is gecertificeerd voor ISO 27001: 2013 en 27018, HIPAA en DPA. Er wordt momenteel SOC2 en FedRAMP JAB-beoordelingen.

## <a name="pricing"></a>Prijs

### <a name="how-do-i-calculate-approximate-charges-for-vmware-disaster-recovery"></a>Hoe ik geschatte kosten in rekening gebracht voor VMware-noodherstel berekend?

Gebruik de [prijscalculator](https://aka.ms/asr_pricing_calculator) kosten wilt ramen tijdens het gebruik van Site Recovery.

Voor een gedetailleerde raming van kosten, voer het hulpprogramma deployment planner voor [VMware](https://aka.ms/siterecovery_deployment_planner) en gebruik de [kosten van het rapport met kostenramingen](https://aka.ms/asr_DP_costreport).

### <a name="is-there-any-difference-in-cost-between-replicating-to-storage-or-directly-to-managed-disks"></a>Is er een verschil in kosten tussen repliceren naar de opslag of rechtstreeks naar managed disks?

Beheerde schijven worden in rekening gebracht net iets anders uit storage-accounts. [Meer informatie](https://azure.microsoft.com/pricing/details/managed-disks/) over de prijzen voor beheerde schijf.

## <a name="mobility-service"></a>Mobility-service

### <a name="where-can-i-find-the-mobility-service-installers"></a>Waar vind ik de Mobility-service-installatieprogramma's?

Het installatieprogramma's zijn in de map %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository op de configuratieserver.

## <a name="how-do-i-install-the-mobility-service"></a>Hoe installeer ik de Mobility-service?

Op elke virtuele machine die u wilt repliceren, moet u de service installeren op een van verschillende manieren:

- [Push-installatie](vmware-physical-mobility-service-overview.md#push-installation)
- [Handmatige installatie](vmware-physical-mobility-service-overview.md#install-mobility-agent-through-ui) vanuit de gebruikersinterface of PowerShell
- Implementatie met behulp van een implementatieprogramma zoals [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md)

## <a name="managed-disks"></a>Managed Disks

### <a name="where-does-site-recovery-replicate-data-to"></a>Waar Site Recovery gegevens repliceren?

Site Recovery repliceert on-premises VMware-machines en fysieke servers naar beheerde schijven in Azure.

- De processerver van Site Recovery schrijft de replicatielogboeken naar een cache-opslagaccount in de doelregio.
- Deze logboeken worden gebruikt om herstelpunten te maken op Azure beheerde schijven die zijn voorzien van voorvoegsel van **asrseeddisk**.
- Wanneer een failover optreedt, wordt het herstelpunt dat u gebruikt om een nieuwe doel beheerde schijf te maken. Deze beheerde schijf is gekoppeld aan de virtuele machine in Azure.
- Virtuele machines die eerder zijn gerepliceerd naar een opslagaccount (vóór maart 2019) worden niet beïnvloed.

### <a name="can-i-replicate-new-machines-to-storage-accounts"></a>Kan ik nieuwe machines repliceren naar storage-accounts?

Nee. Vanaf maart 2019, in de Azure-portal kunt u repliceren naar Azure alleen beheerde schijven.

Replicatie van nieuwe virtuele machines naar een opslagaccount is alleen beschikbaar via PowerShell of de REST-API (versie 2018-01-10- of 2016-08-10).

### <a name="what-are-the-benefits-of-replicating-to-managed-disks"></a>Wat zijn de voordelen van het repliceren naar beheerde schijven?

[Informatie over hoe](https://azure.microsoft.com/blog/simplify-disaster-recovery-with-managed-disks-for-vmware-and-physical-servers/) Site Recovery vereenvoudigt het herstel na noodgeval met beheerde schijven.

### <a name="can-i-change-the-managed-disk-type-after-a-machine-is-protected"></a>Kan ik het type beheerde schijf wijzigen nadat een virtuele machine is beveiligd?

Ja, u kunt eenvoudig [wijzigen van het type beheerde schijf](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage) voor lopende replicaties. Voordat u het type wijzigt, zorg ervoor dat er geen handtekening voor gedeelde toegang die URL wordt gegenereerd op de beheerde schijf:

1. Ga naar de **Managed Disk** resource in Azure portal en controleert u of u een shared access signature-URL banner op de **overzicht** blade.
1. Als de banner aanwezig is, selecteert u deze naar de continue export annuleren.
1. Wijzig het type van de schijf binnen enkele minuten. Als u het type beheerde schijf wijzigt, wacht u tot de nieuwe herstelpunten moeten worden gegenereerd door Azure Site Recovery.
1. Gebruik de nieuwe herstelpunten voor een testfailover of failover in de toekomst.

### <a name="can-i-switch-replication-from-managed-disks-to-unmanaged-disks"></a>Kan ik replicatie van beheerde schijven naar niet-beheerde schijven wisselen?

Nee. Overschakelen van beheerd naar niet-beheerde wordt niet ondersteund.

## <a name="replication"></a>Replicatie

### <a name="what-are-the-replicated-vm-requirements"></a>Wat zijn de vereisten van de gerepliceerde VM's?

[Meer informatie](vmware-physical-azure-support-matrix.md#replicated-machines) over de ondersteuningsvereisten voor voor virtuele VMware-machines en fysieke servers.

### <a name="how-often-can-i-replicate-to-azure"></a>Hoe vaak kan ik repliceren naar Azure?

Replicatie is continue bij het repliceren van virtuele VMware-machines naar Azure.

### <a name="can-i-extend-replication"></a>Kan ik replicatie verlengen?

Uitgebreide of gekoppelde replicatie wordt niet ondersteund. Aanvragen van deze functie in de [Feedbackforum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-initial-replication"></a>Kan ik een offline initiële replicatie?

Offline replicatie wordt niet ondersteund. Aanvragen van deze functie in de [Feedbackforum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="what-is-asrseeddisk"></a>Wat is asrseeddisk?

Voor elke schijf die als bron, worden gegevens gerepliceerd naar een beheerde schijf in Azure. Deze schijf is voorzien van het voorvoegsel van **asrseeddisk**. De kopie van de schijf die als bron en alle momentopnamen voor herstelpunt worden opgeslagen.

### <a name="can-i-exclude-disks-from-replication"></a>Kan ik schijven uitsluiten van replicatie?

Ja, kunt u schijven uitsluiten.

### <a name="can-i-replicate-vms-that-have-dynamic-disks"></a>Kan ik virtuele machines waarvoor dynamische schijven repliceren?

Dynamische schijven kunnen worden gerepliceerd. De besturingssysteemschijf moet een standaardschijf.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Als ik gebruik replicatiegroepen voor multi-VM-consistentie, kan ik toevoegen een nieuwe virtuele machine aan een bestaande replicatiegroep?

Ja, kunt u nieuwe virtuele machines toevoegen aan een bestaande replicatiegroep wanneer u replicatie voor deze inschakelt. Echter:

- U kunt een virtuele machine niet toevoegen aan een bestaande replicatiegroep, nadat replicatie is gestart.
- U kunt geen een replicatiegroep maken voor bestaande VM's.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Kan ik virtuele machines die worden gerepliceerd door toe te voegen of het formaat van schijven wijzigen?

Voor VMware-replicatie naar Azure, kunt u de grootte van de schijf wijzigen. Als u toevoegen van nieuwe schijven wilt, moet u de schijf toevoegen en inschakelen van beveiliging voor de virtuele machine.

### <a name="can-i-migrate-on-premises-machines-to-a-new-vcenter-server-without-impacting-ongoing-replication"></a>Kan ik on-premises machines migreren naar een nieuwe vCenter-Server zonder enige impact op de doorlopende replicatie?

Nee. Een wijziging van de VMware Vcenter of de migratie is van invloed op doorlopende replicatie. Site Recovery instellen met de nieuwe vCenter-Server en replicatie inschakelen voor machines opnieuw.

### <a name="can-i-replicate-to-a-cache-or-target-storage-account-that-has-a-virtual-network-with-azure-firewalls-configured-on-it"></a>Kan ik repliceren naar een cache of doel-opslagaccount met een virtueel netwerk (met Azure-Firewalls) geconfigureerd voor?

Nee, Site Recovery biedt geen ondersteuning voor replicatie naar Azure Storage op virtuele netwerken.

## <a name="component-upgrade"></a>Upgrade van onderdeel

### <a name="my-version-of-the-mobility-services-agent-or-configuration-server-is-old-and-my-upgrade-failed-what-do-i-do"></a>Mijn versie van de Mobility-services-server-agent of de configuratie is verouderd en Mijn upgrade is mislukt. Wat moet ik doen?

Site Recovery volgt het ondersteuningsmodel N-4. [Meer informatie](https://aka.ms/asr_support_statement) over het bijwerken van oude versies.

### <a name="where-can-i-find-the-release-notes-and-update-rollups-for-azure-site-recovery"></a>Waar kan ik de opmerkingen bij de release vinden en updatepakketten voor Azure Site Recovery?

[Meer informatie over nieuwe updates](site-recovery-whats-new.md), en [rollup-informatie ophalen](service-updates-how-to.md).

### <a name="where-can-i-find-upgrade-information-for-disaster-recovery-to-azure"></a>Waar vind ik een upgrade-informatie voor herstel na noodgevallen naar Azure?

[Meer informatie over het upgraden van](https://aka.ms/asr_vmware_upgrades).

## <a name="do-i-need-to-reboot-source-machines-for-each-upgrade"></a>Moet ik bronmachines voor elke upgrade opnieuw opstarten?

Opnieuw opstarten wordt aanbevolen, maar niet verplicht voor elke upgrade. [Meer informatie](https://aka.ms/asr_vmware_upgrades).

## <a name="configuration-server"></a>Configuratieserver

### <a name="what-does-the-configuration-server-do"></a>Wat doet de configuratieserver?

De configuratieserver wordt uitgevoerd de on-premises Site Recovery-onderdelen, met inbegrip van:

- De configuratieserver zelf. De server coördineert de communicatie tussen on-premises onderdelen en Azure, en beheert de gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze server:
    1. Replicatiegegevens ontvangt.
    2. Optimaliseert de gegevens met caching, compressie en codering.
    3. Verzendt de gegevens naar Azure Storage.
  De processerver heeft een push-installatie van de Mobility-Service op virtuele machines ook en detecteert automatisch on-premises virtuele VMware-machines.
- De hoofddoelserver-server, die de replicatiegegevens tijdens de failback vanuit Azure afgehandeld.

[Meer informatie](vmware-azure-architecture.md) over de onderdelen van de configuratieserver en processen.

### <a name="where-do-i-set-up-the-configuration-server"></a>Waar kan ik de configuratieserver instellen?

U moet een enkele, maximaal beschikbare on-premises virtuele VMware-machine voor de configuratieserver. Voor herstel na noodgevallen fysieke server, installeert u de configuratieserver op een fysieke computer.

### <a name="what-do-i-need-for-the-configuration-server"></a>Wat heb ik nodig voor de configuratieserver

Controleer de [vereisten](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>Kan ik handmatig instellen de configuratieserver in plaats van een sjabloon?

We raden aan dat u [maken van de configuratieserver VM](vmware-azure-deploy-configuration-server.md) met behulp van de meest recente versie van de virtuele Machine indeling OVF sjabloon (Open). Als u de sjabloon niet kunt gebruiken (bijvoorbeeld als u geen toegang tot de VMware-server), [downloaden](physical-azure-set-up-source.md) het setup-bestand van de portal en het instellen van de configuratieserver.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Kan een configuratieserver in meer dan één regio repliceren?

Nee. Als u wilt repliceren naar meer dan één regio, moet u een configuratieserver in elke regio.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Kan ik een configuratieserver in Azure hosten?

Hoewel het mogelijk is, moet de virtuele Azure-machine waarop de configuratieserver wordt uitgevoerd om te communiceren met uw on-premises VMware-infrastructuur en virtuele machines. Deze communicatie wordt toegevoegd latentie en heeft gevolgen voor doorlopende replicatie.

### <a name="how-do-i-update-the-configuration-server"></a>Hoe kan ik de configuratieserver bijwerken?

[Meer](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) het bijwerken van de configuratieserver.

- U kunt de meest recente update informatie vinden op de [updates van de Azure-pagina](https://azure.microsoft.com/updates/?product=site-recovery).
- U kunt de meest recente versie downloaden via de portal. Of u kunt downloaden de nieuwste versie van de configuratieserver rechtstreeks vanuit de [Microsoft Download Center](https://aka.ms/asrconfigurationserver).
- Als uw versie meer dan vier versies ouder dan de huidige versie is, raadpleegt u de [ondersteuningsverklaring](https://aka.ms/asr_support_statement) voor richtlijnen voor upgrades.

### <a name="should-i-back-up-the-configuration-server"></a>Moet ik back-up van de configuratieserver?

We raden u aan om regelmatig geplande back-ups van de configuratieserver.

- De virtuele machine wordt een failback moet voor geslaagde failback bestaat in de database van de configuratieserver.
- De configuratieserver moet worden uitgevoerd en een verbonden status heeft.
- [Meer informatie](vmware-azure-manage-configuration-server.md) over algemene beheertaken voor configuratie-server.

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Als ik van de configuratieserver instelt ben, kan ik handmatig downloaden en installeren MySQL?

Ja. Downloaden van MySQL en plaats deze in de map C:\Temp\ASRSetup. Vervolgens installeert u deze handmatig. Bij het instellen van de virtuele machine van de configuratieserver en de voorwaarden accepteren, MySQL wordt vermeld als **al geïnstalleerd** in **Download en installeer**.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>Kan ik te voorkomen dat het downloaden van MySQL, maar kunt Site Recovery installeren?

Ja. Download het installatieprogramma van MySQL en plaats deze in de map C:\Temp\ASRSetup. Bij het instellen van de configuratieserver VM, accepteer de voorwaarden en selecteer **Download en installeer**. De portal wordt het installatieprogramma dat u hebt toegevoegd voor het installeren van MySQL gebruikt.

### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>Kan ik de virtuele machine van de configuratieserver voor iets anders gebruiken?

Nee. Gebruik de virtuele machine alleen voor de configuratieserver.

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>Kan ik een configuratieserver klonen en voor het indelen van gebruiken?

Nee. Een nieuwe configuratieserver instellen om inschrijving problemen te vermijden.

### <a name="can-i-change-the-vault-in-which-the-configuration-server-is-registered"></a>Kan ik de kluis waarin de configuratieserver is geregistreerd wijzigen?

Nee. Nadat een kluis gekoppeld aan de configuratieserver is, kan niet worden gewijzigd. [Informatie over](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) over het registreren van een configuratieserver met een andere kluis.

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>Kan ik de dezelfde configuratieserver voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers gebruiken?

Ja, maar houd er rekening mee dat de fysieke computer kunt kan worden niet terug alleen bij een VMware-VM.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>Waar kan ik de wachtwoordzin voor de configuratieserver downloaden?

[Meer](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) het downloaden van de wachtwoordzin.

### <a name="where-can-i-download-vault-registration-keys"></a>Waar kan ik registratiesleutels kluis downloaden?

Selecteer in de Recovery Services-kluis **configuratieservers** in **Site Recovery-infrastructuur** > **beheren**. Klik op **Servers**, selecteer **registratiesleutel downloaden** voor het downloaden van het bestand met kluisreferenties.

## <a name="process-server"></a>Processerver

### <a name="why-am-i-unable-to-select-the-process-server-when-i-enable-replication"></a>Waarom kan ik de processerver selecteren wanneer ik replicatie inschakelen?

Updates in versies 9,24 en hoger nu weer de [status van de processerver wanneer u replicatie inschakelt](vmware-azure-enable-replication.md#enable-replication). Deze functie helpt om te voorkomen dat de processerver beperking en om het gebruik van processervers die niet in orde te minimaliseren.

### <a name="how-do-i-update-the-process-server-to-version-924-or-later-for-accurate-health-information"></a>Hoe kan ik de processerver bijwerken naar versie 9.24 of hoger voor nauwkeurige gezondheidsinformatie?

Beginnen met [versie 9,24](service-updates-how-to.md#links-to-currently-supported-update-rollups), meer waarschuwingen zijn toegevoegd om aan te geven van de status van de processerver. [Uw Site Recovery-onderdelen bijwerken naar versie 9.24 of hoger] (service-updates-how-to.md#links-to-currently-supported-update-rollups) zodat alle waarschuwingen worden gegenereerd.

## <a name="failover-and-failback"></a>Failover en failback

### <a name="can-i-use-the-on-premises-process-server-for-failback"></a>Kan ik de on-premises processerver voor failback gebruiken?

Het is raadzaam het maken van een processerver in Azure voor failback doeleinden, om te voorkomen dat gegevensoverdracht latenties. Bovendien in het geval u het Bronnetwerk met virtuele machines met de Azure-gerichte netwerk op de configuratieserver gescheiden, is het essentieel is voor het gebruik van de processerver voor failback gemaakt in Azure.

### <a name="can-i-keep-the-ip-address-on-failover"></a>Kan ik het IP-adres behouden bij failover?

Ja, kunt u het IP-adres behouden bij failover. Zorg ervoor dat u opgeeft de doel-IP-adres in de **berekening en netwerk** instellingen voor de virtuele machine voordat de failover. Bovendien machines afsluiten op het moment van failover IP-adres om conflicten te voorkomen tijdens de failback.

### <a name="can-i-change-the-target-vm-size-or-vm-type-before-failover"></a>Kan ik de doel-VM-grootte of het type van de virtuele machine voordat de failover wijzigen?

Ja, u kunt het type of de grootte van de virtuele machine wijzigen op elk gewenst moment voordat de failover. In de portal, gebruikt u de **berekening en netwerk** instellingen voor de gerepliceerde virtuele machine.

### <a name="how-far-back-can-i-recover"></a>Hoe ver terug kan ik gegevens herstellen?

Voor VMware naar Azure is het oudste herstelpunt dat u kunt gebruiken dit 72 uur.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Hoe krijg ik toegang tot Azure-VM's na een failover?

Na een failover, kunt u Azure-VM's openen via een beveiligde internetverbinding, via een site-naar-site-VPN of via Azure ExpressRoute. Als u wilt verbinden, moet u op verschillende manieren voorbereiden. [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).

### <a name="is-failed-over-data-resilient"></a>Is failover gegevens tegen?

Azure is ontworpen voor herstelbaarheid. Site Recovery is ontworpen voor failover naar een secundaire Azure-datacenter, zoals vereist door de Azure service level agreement (SLA). Wanneer een failover optreedt, zorgen wij ervoor dat uw metagegevens en kluizen blijven in dezelfde geografische regio die u hebt gekozen voor uw kluis.

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?

[Failover](site-recovery-failover.md) wordt niet automatisch uitgevoerd. U start een failover door te maken van een enkelvoudige selectie in de portal of kunt u [PowerShell](/powershell/module/az.recoveryservices) dat een failover wordt geactiveerd.

### <a name="can-i-fail-back-to-a-different-location"></a>Kan ik een failback uitvoeren naar een andere locatie?

Ja. Als u een failover naar Azure, kunt u een failback naar een andere locatie als de oorspronkelijke map is niet beschikbaar. [Meer informatie](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-with-private-peering-to-fail-back"></a>Waarom moet ik een VPN of ExpressRoute met privé-peering voor het uitvoeren van een failback?

Wanneer u een failback van Azure, gegevens van Azure is gekopieerd naar uw on-premises virtuele machine en persoonlijke toegang is vereist.

### <a name="can-i-resize-the-azure-vm-after-failover"></a>Kan ik het formaat van de virtuele Azure-machine na een failover?

Nee, u kunt de grootte of het type van de doel-VM niet wijzigen na de failover.

## <a name="automation-and-scripting"></a>Automation en scripting

### <a name="can-i-set-up-replication-with-scripting"></a>Kan ik replicatie met het uitvoeren van scripts instellen?

Ja. U kunt Site Recovery-werkstromen automatiseren met behulp van de Rest-API, PowerShell of de Azure SDK. [Meer informatie](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Prestaties en capaciteit

### <a name="can-i-throttle-replication-bandwidth"></a>Kan ik de replicatiebandbreedte beperken?

Ja. [Meer informatie](site-recovery-plan-capacity-vmware.md).

## <a name="next-steps"></a>Volgende stappen

- [Beoordeling](vmware-physical-azure-support-matrix.md) vereisten ondersteunen.
- [Instellen van](vmware-azure-tutorial.md) VMware naar Azure-replicatie.

---
title: Veelgestelde vragen - VMware naar Azure-noodherstel met Azure Site Recovery | Microsoft Docs
description: In dit artikel bevat een overzicht van algemene vragen bestandsnaamgedeelte herstel na noodgevallen van on-premises VMware-machines naar Azure met behulp van Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 04/23/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: dffbb2c52b4e43eefe6b4f377bd7af529bae8cc5
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125556"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Veelgestelde vragen - VMware naar Azure-replicatie

In dit artikel vindt u antwoorden op veelgestelde vragen wanneer u herstel na noodgevallen van on-premises VMware-VM's naar Azure implementeert. 

## <a name="general"></a>Algemeen 
### <a name="what-do-i-need-for-vmware-vm-disaster-recovery"></a>Wat heb ik nodig voor noodherstel van VMware-VM?

[Meer informatie over](vmware-azure-architecture.md) de onderdelen die betrokken zijn bij herstel na noodgevallen van virtuele VMware-machines. 

### <a name="can-i-use-site-recovery-to-migrate-vmware-vms-to-azure"></a>Kan ik Site Recovery gebruiken voor het migreren van virtuele VMware-machines naar Azure?

Ja, naast het gebruik van Site Recovery kunt u volledige noodherstel instellen voor VMware-VM's, u kunt ook Site Recovery gebruiken voor het migreren van on-premises VMware-machines naar Azure. In dit scenario kunt u on-premises VMware-machines repliceren naar Azure storage. Vervolgens kunt u een failover van on-premises naar Azure. Na een failover zijn uw toepassingen en workloads beschikbaar en worden uitgevoerd op Azure Virtual machines. Het proces is vergelijkbaar met het instellen van volledige noodherstel, behalve dat de migratie van een systeem u kunt geen failback van Azure.


### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Mijn Azure-account moet machtigingen voor het maken van virtuele machines?
Als u een abonnementsbeheerder bent, hebt u de replicatiemachtigingen die u nodig hebt. Als u niet bent, moet u machtigingen voor het maken van een Azure-VM in de resourcegroep en het virtuele netwerk dat u opgeeft wanneer u Site Recovery- en schrijfmachtigingen voor het geselecteerde opslagaccount configureren of een beheerde schijf op basis van uw configuratie. [Meer informatie](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).

### <a name="what-applications-can-i-replicate"></a>Welke toepassingen kan ik repliceren?
U kunt elke app of de werkbelasting wordt uitgevoerd op een VMware-VM die aan de voldoet repliceren [replicatievereisten](vmware-physical-azure-support-matrix.md##replicated-machines).
- Site Recovery biedt ondersteuning voor toepassingsgevoelige replicatie, zodat apps kunnen worden failover is uitgevoerd en kan niet naar een intelligente status.
- Site Recovery kan worden geïntegreerd met Microsoft-toepassingen zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory, en werkt nauw samen met toonaangevende leveranciers zoals Oracle, SAP, IBM en Red Hat.
- Lees [hier](site-recovery-workload.md) meer informatie over workloadbeveiliging.

### <a name="can-i-use-a-guest-os-server-license-on-azure"></a>Kan ik een gast-OS-server-licentie gebruiken op Azure?
Ja, Microsoft Software Assurance-klanten kunnen gebruikmaken van [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) om op te slaan op de licentiekosten voor **Windows Server-machines** die zijn gemigreerd naar Azure of naar het gebruik van Azure voor herstel na noodgevallen.

## <a name="security"></a>Beveiliging

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Welke toegang Site Recovery moet VMware-servers
Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Een VMware-VM met de Site Recovery-configuratieserver instellen
- Virtuele machines automatisch detecteren voor replicatie. 


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Welke toegang Site Recovery moet VMware-VM's?

- Als u wilt repliceren, moet een VMware-VM hebben de Site Recovery Mobility service geïnstalleerd en wordt uitgevoerd. U kunt het hulpprogramma handmatig implementeren of u kunt opgeven dat Site Recovery een push-installatie van de dienst doen moet wanneer u replicatie voor een virtuele machine inschakelt. 
- Tijdens de replicatie communiceren de virtuele machines als volgt met Site Recovery:
    - Virtuele machines communiceren met de configuratieserver op poort 443 voor HTTPS voor het replicatiebeheer van.
    - Virtuele machines verzenden replicatiegegevens naar de processerver op HTTPS-poort 9443 (kan worden gewijzigd).
    - Als u multi-VM-consistentie inschakelt, wordt de status van virtuele machines communiceren met elkaar via poort 20004.


### <a name="is-replication-data-sent-to-site-recovery"></a>Worden er replicatiegegevens verzonden met Site Recovery?
Nee, Site Recovery gerepliceerde gegevens niet worden onderschept, en heeft geen informatie over wat wordt uitgevoerd op uw virtuele machines. Replicatiegegevens worden uitgewisseld tussen VMware-hypervisors en Azure storage. Met Site Recovery kunnen deze gegevens niet worden onderschept. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  

Site Recovery is ISO 27001: 2013, 27018, HIPAA, DPA gecertificeerd en wordt momenteel SOC2 en FedRAMP JAB-beoordelingen.


## <a name="pricing"></a>Prijzen
### <a name="how-can-i-calculate-approximate-charges-for-vmware-disaster-recovery"></a>Hoe kan ik de geschatte kosten in rekening gebracht voor VMware-noodherstel berekenen?

U kunt de [prijscalculator](https://aka.ms/asr_pricing_calculator) kosten wilt ramen tijdens het gebruik van Site Recovery.

Voor gedetailleerde raming van kosten, voer de tool voor implementatieplanning voor [VMware](https://aka.ms/siterecovery_deployment_planner), en gebruik de [kosten van het rapport met kostenramingen](https://aka.ms/asr_DP_costreport).

### <a name="is-there-any-difference-in-cost-between-replicating-to-storage-or-directly-to-managed-disks"></a>Is er een verschil in kosten tussen repliceren naar de opslag of rechtstreeks naar managed disks?

Beheerde schijven worden in rekening gebracht iets anders dan de storage-accounts. [Meer informatie](https://azure.microsoft.com/pricing/details/managed-disks/) over de prijs van beheerde schijven.

## <a name="mobility-service"></a>Mobility-service

### <a name="where-can-i-find-the-mobility-service-installers"></a>Waar vind ik de Mobility-service-installatieprogramma's?
Het installatieprogramma's zijn de **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** map op de configuratieserver.

## <a name="how-do-i-install-the-mobility-service"></a>Hoe installeer ik de Mobility-service?
U installeert op elke virtuele machine die u repliceren wilt, met behulp van een aantal methoden:
- [Push-installatie](vmware-physical-mobility-service-overview.md#push-installation)
- [Handmatige installatie](vmware-physical-mobility-service-overview.md#install-mobility-agent-through-ui) vanuit de gebruikersinterface of Powershell.
- Implementatie met behulp van een implementatieprogramma zoals [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md).

## <a name="managed-disks"></a>Managed Disks

### <a name="where-does-site-recovery-replicate-data-to"></a>Waar Site Recovery gegevens repliceren?

Site Recovery repliceert on-premises VMware-machines en fysieke servers naar beheerde schijven in Azure.
- De processerver van Site Recovery schrijft de replicatielogboeken naar een cache-opslagaccount in de doelregio.
- Deze logboeken worden gebruikt voor het maken van herstelpunten op de beheerde schijven.
- Wanneer een failover optreedt, wordt het herstelpunt dat u selecteert om te maken van de beheerde schijf gebruikt.
- Virtuele machines die eerder zijn gerepliceerd naar een opslagaccount (vóór maart 2019) worden niet beïnvloed.


### <a name="can-i-replicate-new-machines-to-storage-accounts"></a>Kan ik nieuwe machines repliceren naar storage-accounts?

Nee, vanaf maart 2019, in de portal, kunt u repliceren naar Azure alleen beheerde schijven. 

Replicatie van nieuwe virtuele machines naar een opslagaccount is alleen beschikbaar via PowerShell of de REST-API (versie 2018-01-10- of 2016-08-10).

### <a name="what-are-the-benefits-in-replicating-to-managed-disks"></a>Wat zijn de voordelen bij het repliceren naar managed disks?

[Informatie over hoe](https://azure.microsoft.com/blog/simplify-disaster-recovery-with-managed-disks-for-vmware-and-physical-servers/) Site Recovery vereenvoudigt het herstel na noodgeval met beheerde schijven.


### <a name="can-i-change-the-managed-disk-type-after-machine-is-protected"></a>Kan ik het type beheerde schijf wijzigen nadat de machine is beveiligd?

Ja, u kunt eenvoudig [wijzigen van het type beheerde schijf](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage). Voordat u het type wijzigt, zorg ervoor dat u de SAS-URL voor de schijf intrekken door te gaan naar de resource beheerde schijf op de Azure-portal. Annuleren van de overzichtsblade een continue export. Zodra de SAS-URL is ingetrokken, moet u het type van de schijf wijzigen binnen enkele minuten. Echter, als u het type beheerde schijf wijzigt, wachten op nieuwe herstelpunten moeten worden gegenereerd door Azure Site Recovery. Gebruik de nieuwe herstelpunten voor testfailover of failover voortaan.

### <a name="can-i-switch-replication-from-managed-disks-to-unmanaged-disks"></a>Kan ik replicatie van beheerde schijven naar niet-beheerde schijven wisselen?

Nee, overstappen van beheerd naar niet-beheerde wordt niet ondersteund.

## <a name="replication"></a>Replicatie


### <a name="what-are-the-replicated-vm-requirements"></a>Wat zijn de vereisten van de gerepliceerde VM's?

[Meer informatie](vmware-physical-azure-support-matrix.md##replicated-machines) over de vereisten voor virtuele VMware-machine/fysieke server en ondersteuning.

### <a name="how-often-can-i-replicate-to-azure"></a>Hoe vaak kan ik repliceren naar Azure?
Replicatie is continue bij het repliceren van virtuele VMware-machines naar Azure.


### <a name="can-i-extend-replication"></a>Kan ik replicatie verlengen?
Uitgebreide of gekoppelde replicatie wordt niet ondersteund. Deze functie in aanvragen [Feedbackforum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-initial-replication"></a>Kan ik een offline initiële replicatie?
Nee, dit wordt niet ondersteund. Aanvragen van deze functie in de [Feedbackforum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks-from-replication"></a>Kan ik schijven uitsluiten van replicatie?
Ja, kunt u schijven uitsluiten.

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Kan ik virtuele machines met dynamische schijven repliceren?
Dynamische schijven kunnen worden gerepliceerd. De besturingssysteemschijf moet een standaardschijf.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Als ik gebruik replicatiegroepen voor multi-VM-consistentie, kan ik toevoegen een nieuwe virtuele machine aan een bestaande replicatiegroep?
Ja, kunt u nieuwe virtuele machines toevoegen aan een bestaande replicatiegroep wanneer u replicatie voor deze inschakelt.
- U kunt een virtuele machine niet toevoegen aan een bestaande replicatiegroep, nadat replicatie is gestart.
- U kunt geen een replicatiegroep maken voor bestaande VM's.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Kan ik virtuele machines die worden gerepliceerd door toe te voegen of het formaat van schijven wijzigen?

U kunt de schijfgrootte wijzigen voor VMware-replicatie naar Azure. Als u wilt toevoegen van nieuwe schijven moet u de schijf toevoegen en weer inschakelen van beveiliging voor de virtuele machine.

### <a name="can-i-migrate-on-premises-machines-to-a-new-vcenter-server-without-impacting-ongoing-replication"></a>Kan ik on-premises machines migreren naar een nieuwe vCenter-Server zonder enige impact op de doorlopende replicatie?
Wijziging van de Vcenter- of de migratie is niet het geval is, van invloed op doorlopende replicatie. U moet de Site Recovery instellen met de nieuwe vCenter-Server, en replicatie inschakelen voor machines opnieuw.

### <a name="can-i-replicate-to-a-cachetarget-storage-account-which-has-a-vnet-with-azure-storage-firewalls-configured-on-it"></a>Kan ik repliceren naar een cache-/ doelopslagaccount met een VNet (met Azure storage-firewalls) geconfigureerd voor?
Nee, Site Recovery biedt geen ondersteuning voor replicatie naar de opslag op Vnet.





## <a name="component-upgrade"></a>Upgrade van onderdeel

### <a name="my-version-of-the-mobility-services-agent-or-configuration-server-is-old-and-my-upgrade-failed-what-do-i-do"></a>Mijn versie van de Mobility-services-server-agent of de configuratie is de oude als de upgrade is mislukt. Wat moet ik doen?  

Site Recovery volgt het ondersteuningsmodel N-4. [Meer informatie](https://aka.ms/asr_support_statement) over het bijwerken van oude versies.

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-azure-site-recovery"></a>Waar vind ik de release-opmerkingen/updatepakketten van Azure Site Recovery?

[Informatie over](site-recovery-whats-new.md) over nieuwe updates en [rollup-informatie ophalen](service-updates-how-to.md).

### <a name="where-can-i-find-upgrade-information-for-disaster-recovery-to-azure"></a>Waar vind ik een upgrade-informatie voor herstel na noodgevallen naar Azure?

[Meer informatie over](https://aka.ms/asr_vmware_upgrades) upgraden.

## <a name="do-i-need-to-reboot-source-machines-for-each-upgrade"></a>Moet ik bronmachines voor elke upgrade opnieuw opstarten?

Hoewel aanbevolen, is dit het niet verplicht voor elke upgrade. [Meer informatie](https://aka.ms/asr_vmware_upgrades).


## <a name="configuration-server"></a>Configuratieserver

### <a name="what-does-the-configuration-server-do"></a>Wat doet de configuratieserver?

De configuratieserver wordt uitgevoerd de on-premises Site Recovery-onderdelen, met inbegrip van:
- De configuratieserver zelf die coördineert de communicatie tussen on-premises en Azure en beheert de gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag. De processerver ook is een push-installatie van de Mobility-Service op virtuele machines, en detecteert automatisch on-premises virtuele VMware-machines.
- De hoofddoelserver die verantwoordelijk is voor replicatiegegevens tijdens de failback vanuit Azure.

[Meer informatie](vmware-azure-architecture.md) over de onderdelen van de configuratieserver en processen.

### <a name="where-do-i-set-up-the-configuration-server"></a>Waar kan ik de configuratieserver instellen?
In dat geval moet u een één maximaal beschikbare on-premises virtuele VMware-machine in voor de configuratieserver. Voor herstel na noodgevallen fysieke server, kunt u de configuratieserver installeren op een fysieke computer.

### <a name="what-do-i-need-for-the-configuration-server"></a>Wat heb ik nodig voor de configuratieserver

Controleer de [vereisten](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>Kan ik handmatig instellen de configuratieserver in plaats van een sjabloon?
We raden aan dat u [maken van de configuratieserver VM](vmware-azure-deploy-configuration-server.md) met de meest recente versie van het OVF-sjabloon. Als voor een of andere reden is niet mogelijk, zoals u geen toegang tot de VMware-server, kunt u [downloaden](physical-azure-set-up-source.md) het setup-bestand van de portal en het instellen van de configuratieserver.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Kan een configuratieserver in meer dan één regio repliceren?
Nee. Om dit te doen, moet u een configuratieserver in elke regio.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Kan ik een configuratieserver in Azure hosten?
Tijdens het mogelijk is moet de virtuele Azure-machine waarop de configuratieserver wordt uitgevoerd om te communiceren met uw on-premises VMware-infrastructuur en virtuele machines. Dit wordt toegevoegd latentie en heeft gevolgen voor doorlopende replicatie.

### <a name="how-do-i-update-the-configuration-server"></a>Hoe kan ik de configuratieserver bijwerken?

[Informatie over](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) configuratieserver bijwerken.
- U kunt de meest recente update informatie vinden op de [updates van de Azure-pagina](https://azure.microsoft.com/updates/?product=site-recovery).
- U kunt de meest recente versie downloaden via de portal. U kunt ook rechtstreeks downloaden de nieuwste versie van de configuratieserver in de [Microsoft Download Center](https://aka.ms/asrconfigurationserver).
- Als uw versie meer dan vier versies ouder dan de huidige versie is, raadpleegt u onze [ondersteuningsverklaring](https://aka.ms/asr_support_statement) voor richtlijnen voor upgrades.

### <a name="should-i-back-up-the-configuration-server"></a>Moet ik back-up van de configuratieserver?
We raden u aan om regelmatig geplande back-ups van de configuratieserver.
- De virtuele machine wordt een failback moet voor geslaagde failback bestaat in de database van de configuratieserver.
- De configuratieserver moet worden uitgevoerd en een verbonden status heeft.
- [Meer informatie](vmware-azure-manage-configuration-server.md) over algemene beheertaken voor configuratie-server.

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Als ik van de configuratieserver instelt ben, kan ik handmatig downloaden en installeren MySQL?

Ja. Downloaden van MySQL en plaats deze in de **C:\Temp\ASRSetup** map. Installeer deze vervolgens handmatig. Bij het instellen van de virtuele machine van de configuratieserver en de voorwaarden accepteren, MySQL wordt vermeld als **al geïnstalleerd** in **Download en installeer**.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>Kan ik te voorkomen dat het downloaden van MySQL, maar kunt Site Recovery installeren?

Ja. Download het installatieprogramma van MySQL en plaats deze in de **C:\Temp\ASRSetup** map.  Bij het instellen van de virtuele machine van de configuratieserver, de voorwaarden accepteren en klik op **Download en installeer**. De portal wordt het installatieprogramma dat u hebt toegevoegd voor het installeren van MySQL gebruikt.
 
### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>Kan ik de virtuele machine van de configuratieserver voor iets anders gebruiken?
Nee, moet u de virtuele machine alleen gebruiken voor de configuratieserver. 

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>Kan ik een configuratieserver klonen en voor het indelen van gebruiken?
Nee, moet u instellen een nieuwe configuratieserver om te voorkomen dat problemen met registratie.

### <a name="can-i-change-the-vault-in-which-the-configuration-server-is-registered"></a>Kan ik de kluis waarin de configuratieserver is geregistreerd wijzigen?
Nee. Nadat een kluis gekoppeld aan de configuratieserver is, kan niet worden gewijzigd. Beoordeling [in dit artikel](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) voor meer informatie over het opnieuw te registreren.

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>Ik kan de dezelfde configuratieserver voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers gebruiken
Ja, maar houd er rekening mee dat de fysieke computer kan alleen worden mislukte terug naar een VMware-VM.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>Waar kan ik de wachtwoordzin voor de configuratieserver downloaden?
[Meer informatie over het](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) downloaden van de wachtwoordzin.

### <a name="where-can-i-download-vault-registration-keys"></a>Waar kan ik registratiesleutels kluis downloaden?

Klik in de Recovery Services-kluis op **configuratieservers** in **Site Recovery-infrastructuur** > **beheren**. Klik vervolgens in **Servers**, selecteer **registratiesleutel downloaden** voor het downloaden van het bestand met kluisreferenties.

## <a name="process-server"></a>Processerver

### <a name="unable-to-select-process-server-during-enable-replication"></a>Kan geen processerver selecteren tijdens replicatie inschakelen

Van 9.24 versie verbeteringen zijn aangebracht voor [binnen het product richtlijnen](vmware-azure-manage-process-server.md#process-server-selection-guidance) op bij het instellen van een uitbreidbare processerver. Dit is om te voorkomen dat processen server beperken en te voorkomen dat het gebruik van de processerver niet in orde.

### <a name="what-should-i-do-to-obtain-accurate-health-status-of-process-server"></a>Wat moet ik doen om te verkrijgen van nauwkeurige status van de processerver

Upgraden van Site Recovery-onderdelen op de [meest recente versies](service-updates-how-to.md#links-to-currently-supported-update-rollups) (ten minste 9.24 of hoger).

## <a name="failover-and-failback"></a>Failover en failback
### <a name="can-i-use-the-process-server-at-on-premises-for-failback"></a>Kan ik de processerver op on-premises voor failback gebruiken?
Sterk aanbevolen het maken van een processerver in Azure voor failback doeleinden, om te voorkomen dat gegevensoverdracht latenties. Bovendien in het geval u het Bronnetwerk met virtuele machines met de Azure-gerichte netwerk op de configuratieserver gescheiden, is het essentieel is voor het gebruik van de processerver voor failback gemaakt in Azure.

### <a name="can-i-retain-the-ip-address-on-failover"></a>Kan ik het IP-adres bij failover bewaren?
Ja, kunt u het IP-adres bij failover behouden. Zorg ervoor dat u opgeeft de doel-IP-adres in 'berekening en netwerk-instellingen voor de virtuele machine voordat de failover. Bovendien machines afsluiten op het moment van failover IP-adres om conflicten te voorkomen tijdens de failback.

### <a name="can-i-change-the-target-vm-size-or-vm-type-before-failover"></a>Kan ik de doel-VM-grootte of het type van de virtuele machine voordat de failover wijzigen?
Ja, u kunt het type of de grootte van de virtuele machine op elk gewenst moment voordat de failover van de instellingen berekening en netwerk van de gerepliceerde virtuele machine, in de portal wijzigen.

### <a name="how-far-back-can-i-recover"></a>Hoe ver terug kan ik gegevens herstellen?
Voor VMware naar Azure is het oudste herstelpunt dat u kunt gebruiken dit 72 uur.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Hoe krijg ik toegang tot Azure-VM's na een failover?
Na een failover, kunt u Azure-VM's openen via een beveiligde internetverbinding, via een site-naar-site-VPN of via Azure ExpressRoute. U moet een aantal dingen om verbinding te kunnen voorbereiden. [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Failover wordt uitgevoerd gegevens tegen?

Azure is ontworpen voor herstelbaarheid. Site Recovery is ontworpen voor failover naar een secundaire Azure-datacenter, in overeenstemming met de Azure SLA. Wanneer een failover optreedt, zorgen wij ervoor dat uw metagegevens en kluizen binnen dezelfde geografische regio die u hebt gekozen voor uw kluis blijven.

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?
[Failover](site-recovery-failover.md) wordt niet automatisch uitgevoerd. Initiëren van failover met één klik in de portal of kunt u [PowerShell](/powershell/module/az.recoveryservices) dat een failover wordt geactiveerd.

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

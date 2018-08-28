---
title: SAP HANA installeren op SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Over het installeren van SAP HANA op een SAP HANA op Azure (grote instantie).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/27/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d335e135551b7b6faed8ee566acb14b46fd6c81
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43107508"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Installeren en configureren van SAP HANA (grote instanties) op Azure

Hier volgen enkele belangrijke definities moet weten voordat u deze handleiding leest. In [SAP HANA (grote instanties) overzicht en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) hebben we twee verschillende soorten HANA grote instantie eenheden met geïntroduceerd:

- S72, S72m, S144, S144m, S192, S192m en S192xm, dit is het 'ik klasse type' van SKU's.
- S384, S384m S384xm, S384xxm, S576m, S576xm, S768m, S768xm en S960m, dit is de 'Type II-klasse' van SKU's.

De klasse-id gaat moet worden gebruikt in de documentatie van HANA grote instantie uiteindelijk verwijzen naar verschillende mogelijkheden en vereisten op basis van HANA grote instantie SKU's.

Er zijn andere definities die we vaak gebruiken:
- **Grote instantiestempel:** een hardware-infrastructuur-stack die SAP HANA TDI is gecertificeerd en toegewezen voor het uitvoeren van SAP HANA-instanties in Azure.
- **SAP HANA op Azure (grote instanties):** officiële naam voor de aanbieding in Azure om uit te voeren van HANA-exemplaren in op SAP HANA TDI-gecertificeerde hardware die is geïmplementeerd in grote instantie stempels in verschillende Azure-regio's. De gerelateerde term **HANA grote instantie** kort voor SAP HANA op Azure (grote instanties) en is veel gebruikt in deze handleiding technische implementatie.


De installatie van SAP HANA is uw verantwoordelijkheid en kunt u de activiteit starten na de levering van een nieuwe SAP HANA op Azure (grote instanties)-server. En nadat de verbinding tussen uw Azure vnet (s) en de HANA grote instantie eenheid/eenheden tot stand is gebracht. 

> [!Note]
> Per SAP-beleid, moet de installatie van SAP HANA worden uitgevoerd door een persoon die is gecertificeerd voor SAP HANA-installaties uitvoeren. Een persoon, die is doorgegeven het koppelen van technologie met SAP gecertificeerde examen, certificeringsexamen SAP HANA-installatie, of door een SAP-gecertificeerde systeemintegrator (SI).

Controleer opnieuw, met name bij het plannen van HANA-2.0 installeren [SAP Support Opmerking #2235581 - SAP HANA: ondersteunde besturingssystemen](https://launchpad.support.sap.com/#/notes/2235581/E) om ervoor te zorgen dat het besturingssysteem wordt ondersteund met de SAP HANA release u besluiten om te installeren. U ontdekt dat de ondersteund besturingssysteem voor HANA 2.0 beperkter dan het besturingssysteem dat wordt ondersteund voor HANA 1.0 is. 

> [!IMPORTANT] 
> Voor Type II eenheden alleen de SLES 12 SP2 OS-versie op dit moment ondersteund. 

## <a name="first-steps-after-receiving-the-hana-large-instance-units"></a>Eerste stappen na de ontvangst van de HANA grote instantie eenheid/eenheden

**Stap 1** na ontvangst van de HANA grote instantie en toegang tot stand gebrachte en in te stellen verbinding met de exemplaren, bestaat uit het registreren van het besturingssysteem van het exemplaar bij uw besturingssysteem-provider. Deze stap omvat het SUSE Linux-besturingssysteem wordt geregistreerd bij een exemplaar van SUSE SMT die u nodig hebt geïmplementeerd in een virtuele machine in Azure. De eenheid HANA grote instantie kan verbinding maken met dit exemplaar SMT (Zie verderop in deze documentatie). Of uw Red Hat-besturingssysteem moet worden geregistreerd met de Red Hat Doelabonnementbeheerder u verbinding wilt maken. Zie ook opmerkingen in deze [document](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Deze stap is ook nodig zijn om te kunnen zijn voor het vullen van het besturingssysteem. Een taak die in de verantwoordelijkheid van de klant. Voor SUSE, vindt u documentatie om te installeren en configureren van SMT [hier](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

**Tweede stap** is om te controleren op nieuwe patches en verbeteringen van de specifieke versie/versie van het besturingssysteem. Controleer of de patch-niveau van de HANA grote instantie op de meest recente status is. Op basis van de timing van OS patch/releases en wijzigingen naar de installatiekopie van die Microsoft kunt implementeren, kunnen er gevallen waarbij de meest recente patches niet opgenomen worden mag. Het is daarom een verplichte stap na over een eenheid HANA grote instantie, om te controleren of patches die relevant zijn voor beveiliging, functies, beschikbaarheid en prestaties in de tussentijd zorgen zijn vrijgegeven door de specifieke Linux-leverancier en moeten worden toegepast.

**Het derde stap** bestaat uit het controleren van de relevante SAP-opmerkingen voor het installeren en configureren van SAP HANA op specifieke release/versie van het besturingssysteem. Vanwege het wijzigen van aanbevelingen of wijzigingen aan SAP-opmerkingen of configuraties die afhankelijk van de installatie van de afzonderlijke scenario's zijn, Microsoft niet altijd mogelijk om een HANA grote instantie eenheid perfect geconfigureerd. Het is daarom verplicht voor u als een klant, om te lezen van dat de SAP-opmerkingen met betrekking tot SAP HANA op uw exacte Linux-versie. Ook Controleer de configuraties van release/versie van het besturingssysteem die nodig zijn en de configuratie-instellingen toepassen wanneer nog niet gedaan.

Controleer de volgende parameters in specifieke, en uiteindelijk wordt aangepast aan:

- net.core.rmem_max = 16777216
- net.core.wmem_max = 16777216
- NET.Core.rmem_default 16777216 =
- NET.Core.wmem_default 16777216 =
- net.core.optmem_max = 16777216
- net.ipv4.tcp_rmem = 65536 16777216 16777216
- net.ipv4.tcp_wmem = 65536 16777216 16777216

Beginnen met SP1 SLES12 en RHEL 7.2, moeten deze parameters worden ingesteld in een configuratiebestand in de map /etc/sysctl.d. Bijvoorbeeld, moet een configuratiebestand met de naam 91-NetApp-HANA.conf worden gemaakt. Voor oudere versies van SLES en RHEL moet deze parameters set in/etc/sysctl.conf.

Voor alle RHEL vrijgegeven en beginnen met SLES12, de 
- sunrpc.tcp_slot_table_entries = 128

parameter moet worden ingesteld als in/etc/modprobe.d/sunrpc-local.conf. Als het bestand niet bestaat, moet deze eerst worden gemaakt door de volgende vermelding toe te voegen: 
- Opties voor sunrpc tcp_max_slot_table_entries = 128

**Vierde stap** is om te controleren of de systeemtijd van de HANA grote instantie eenheid. De exemplaren zijn geïmplementeerd met de tijdzone van een systeem waarbij de locatie van de Azure-regio die de HANA grote Instantiestempel bevindt zich in. U staat op vrije de systeemtijd of de tijdzone van de exemplaren die u wilt wijzigen. In dat geval en volgorde meer instanties in uw tenant, moet u aan te passen van de tijdzone van de zojuist geleverde exemplaren worden voorbereid. Microsoft-bewerkingen hebben geen inzicht in de tijdzone van het systeem dat u instellen met de exemplaren na de overdracht. Geïmplementeerde exemplaren kunnen daarom niet worden ingesteld in dezelfde tijdzone als het account dat u hebt gewijzigd. Als gevolg hiervan is het uw verantwoordelijkheid als klant om te controleren en indien nodig aan te passen de tijdzone van de instantie (s) afgegeven. 

**Vijfde stap** is om te controleren op etc/hosts. Als de blades afgegeven ophalen, hebben ze verschillende IP-adressen die zijn toegewezen voor verschillende doeleinden (Zie volgende sectie). Controleer het bestand etc/hosts. In gevallen waar de eenheden worden toegevoegd aan een bestaande tenant geen verwachte etc/hosts van de zojuist geïmplementeerde systemen correct onderhouden met de IP-adressen van eerder geleverde systemen. Het is daarom op u als klant om te controleren of de juiste instellingen zo, dat een zojuist geïmplementeerde exemplaar kan communiceren en de namen van de eerder geïmplementeerde eenheden in uw tenant omzetten. 

## <a name="networking"></a>Netwerken
Gaan we ervan uit dat u de aanbevelingen in het ontwerpen van uw Azure vnet's en de VNets verbinden met de HANA grote instanties zoals beschreven in deze documenten hebt gevolgd:

- [Overzicht van de SAP HANA (grote instantie) en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Er zijn enkele details waard is om te vermelden over de netwerken van de afzonderlijke eenheden. Elke eenheid HANA grote instantie wordt geleverd met twee of drie IP-adressen die zijn toegewezen aan twee of drie NIC-poorten van de eenheid. Drie IP-adressen worden gebruikt in HANA uitschaalconfiguraties en de HANA-Systeemreplicatie-scenario. Een van de IP-adressen die zijn toegewezen aan de NIC van de eenheid valt buiten de Server IP-adresgroep die is beschreven in de [SAP HANA (grote instantie) overzicht en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

Raadpleeg [HLI scenario's ondersteund](hana-supported-scenario.md) voor meer informatie over Ethernet-details voor uw architectuur.

## <a name="storage"></a>Storage

De opslagindeling voor SAP HANA op Azure (grote instanties) is geconfigureerd door SAP HANA op Azure Service Management via SAP hulplijnen aanbevolen zoals beschreven in [opslagvereisten voor SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) technisch document. De ruwe grootte van de verschillende volumes met de verschillende HANA grote instanties SKU's is beschreven in [SAP HANA (grote instantie) overzicht en architectuur op Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

De naamgeving van de opslagvolumes worden vermeld in de volgende tabel:

| Opslaggebruik | De naam van koppelpunt | Volumenaam | 
| --- | --- | ---|
| HANA-gegevens | /Hana/Data/SID/mnt0000<m> | Storage IP:/hana_data_SID_mnt00001_tenant_vol |
| HANA-logboek | /Hana/log/SID/mnt0000<m> | Storage IP:/hana_log_SID_mnt00001_tenant_vol |
| HANA logboekback-up | /Hana/log/backups | Storage IP:/hana_log_backups_SID_mnt00001_tenant_vol |
| HANA gedeeld | /Hana/Shared/SID | Storage IP:/hana_shared_SID_mnt00001_tenant_vol/shared |
| usr/sap | /usr/SAP/SID | Storage IP:/hana_shared_SID_mnt00001_tenant_vol/usr_sap |

Waarbij SID de HANA-instantie systeem-ID = 

Als de tenant een interne opsomming van bewerkingen = bij het implementeren van een tenant.

Zoals u ziet, HANA, gedeeld en usr/sap delen hetzelfde volume. De naamgeving van de quorumbron: heeft betrekking op de systeem-ID van de HANA-instanties, evenals het aantal koppelen. In implementaties omhoog is er alleen een koppelpunt, zoals mnt00001. Terwijl in scale-out-implementatie u zoveel koppelingen als ziet hebben u model en worker-knooppunten. Voor de scale-out-omgeving, gegevens, log, back-up logboekvolumes gedeeld en die is gekoppeld aan elk knooppunt in de configuratie van de scale-out. Voor configuraties met meerdere instanties van SAP is een andere set volumes gemaakt en gekoppeld aan de eenheid HAN grote instantie. Raadpleeg [HLI scenario's ondersteund](hana-supported-scenario.md) voor lay-out opslaggegevens voor uw scenario.

Lees het document en een eenheid HANA grote instantie zoeken, ontdekt u dat de eenheden voor HANA/gegevens afkomstig met in plaats daarvan royale schijfvolume zijn en dat we beschikken over een volume HANA/log/back-up. De reden waarom we de HANA/gegevens zo groot grootte is dat de storage-momentopnamen we voor u als een klant bieden van hetzelfde schijfvolume gebruikmaakt. Dit betekent dat de meer opslag momentopnamen die u uitvoert, hoe meer opslagruimte wordt gebruikt door momentopnamen in uw opslagvolumes die zijn toegewezen. Het volume HANA/log/back-up wordt niet beschouwd moet het volume op databaseback-ups worden geplaatst. Het formaat wordt aangepast om te worden gebruikt als back-upvolume voor de HANA transactielogboekback-ups. In toekomstige snapshot versies van de opslag self service, dat wordt dit bepaald volume om vaker momentopnamen heeft betrekking op. En met die frequentere replicaties naar de site voor noodherstel als u om de optie-in voor het herstel na noodgevallen geleverd door de infrastructuur van HANA grote instantie. Zie voor meer informatie [SAP HANA (grote instanties) met hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

Naast de opslag die is geleverd, kunt u extra opslagcapaciteit in stappen van 1 TB aanschaffen. Deze extra opslag kan worden toegevoegd als nieuwe volumes aan een HANA grote instanties.

Tijdens de onboarding met SAP HANA op Azure Service Management, de klant Hiermee geeft u een gebruikers-ID (UID) en groep-ID (groeps-id) voor de groep van gebruikers- en sapsys sidadm (ex: 1000,500) is het nodig dat tijdens de installatie van de SAP HANA-systeem, deze dezelfde waarden worden gebruikt. Als u implementeren meerdere HANA-exemplaren op een eenheid wilt, krijgt u meerdere sets van volumes (één set voor elk exemplaar). Tijdens de implementatie moet u als gevolg hiervan definiëren:

- De SID van de verschillende HANA-instanties (sidadm is buiten deze afgeleid).
- De grootte van het geheugen van de verschillende HANA-instanties. Omdat de grootte van het geheugen per instantie de grootte van de volumes in elke volumeset afzonderlijke definieert.

Op basis van aanbevelingen voor opslag-provider de volgende koppelingsopties zijn geconfigureerd voor alle gekoppelde volumes (met uitzondering van opstarten LUN):

- NFS-rw bergen = 4, hard, timeo = 600, rsize = 1048576, wsize = 1048576, Intro, noatime, vergrendelen 0 0

Deze punten zijn geconfigureerd in/etc/fstab, zoals wordt weergegeven in de volgende afbeeldingen koppelen:

![fstab van gekoppelde volumes in HANA grote instantie eenheid](./media/hana-installation/image1_fstab.PNG)

De uitvoer van de opdracht df -h op een eenheid S72m HANA grote instantie eruit als:

![fstab van gekoppelde volumes in HANA grote instantie eenheid](./media/hana-installation/image2_df_output.PNG)


De controller voor opslag en de knooppunten in de stempels grote instantie worden gesynchroniseerd met de NTP-servers. Met u synchronisatie van de SAP HANA op Azure (grote instanties)-eenheden en Azure-VM's op basis van een NTP-server, mogen er geen aanzienlijke tijd drift gebeurt tussen de infrastructuur en de rekeneenheden in Azure of grote instantie stempels.

Als u wilt optimaliseren SAP HANA naar de opslag die onder gebruikt, moet u ook de volgende configuratieparameters voor SAP HANA instellen:

- max_parallel_io_requests 128
- async_read_submit op
- async_write_submit_active op
- alle async_write_submit_blocks
 
Voor SAP HANA 1.0 versies tot SPS12, deze parameters worden ingesteld tijdens de installatie van de SAP HANA-database, zoals beschreven in [SAP Opmerking #2267798 - configuratie van de SAP HANA-Database](https://launchpad.support.sap.com/#/notes/2267798)

U kunt ook de parameters na de installatie van de SAP HANA-database configureren met behulp van de hdbparam-framework. 

Met SAP HANA 2.0, is het framework hdbparam afgeschaft. De parameters moeten als gevolg hiervan worden ingesteld met behulp van SQL-opdrachten. Zie voor meer informatie, [SAP Opmerking #2399079: opheffing van hdbparam in HANA 2](https://launchpad.support.sap.com/#/notes/2399079).

Raadpleeg [HLI scenario's ondersteund](hana-supported-scenario.md) voor meer informatie over de indeling voor uw architectuur.

## <a name="operating-system"></a>Besturingssysteem

> [!IMPORTANT] 
> Voor Type II eenheden alleen de SLES 12 SP2 OS-versie op dit moment ondersteund. 

Wisselruimte van het geleverde installatiekopie van het besturingssysteem is ingesteld op 2 GB volgens de [SAP Support Opmerking #1999997 - Veelgestelde vragen over: SAP HANA geheugen](https://launchpad.support.sap.com/#/notes/1999997/E). Verschillende instellingen gewenste moet worden ingesteld door u als een klant.

[SUSE Linux Enterprise Server 12 SP1 voor SAP-toepassingen](https://www.suse.com/products/sles-for-sap/hana) is de verdeling van Linux voor SAP HANA op Azure (grote instanties) is geïnstalleerd. Deze specifieke distributie biedt mogelijkheden voor SAP-specifieke &quot;gebruiksklaar&quot; (met inbegrip van de vooraf ingestelde parameters voor het uitvoeren van SAP op SLES effectief).

Zie [Resource bibliotheek/White Papers](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) op de website van SUSE en [SAP op SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) op de SAP Community netwerk (SCN) voor verschillende nuttige bronnen die betrekking hebben op de implementatie van SAP HANA op SLES (met inbegrip van de configuratie van hoge Beschikbaarheid, beveiliging die specifiek zijn voor SAP-bewerkingen en meer).

Aanvullende en nuttige SAP op SUSE-gerelateerde koppelingen:

- [SAP HANA op SUSE Linux-Site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [Best Practice voor SAP: in de wachtrij plaatsen replicatie: SAP NetWeaver op SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113).
- [ClamSAP – virusbeveiliging SLES voor SAP](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (met inbegrip van SLES 12 voor SAP-toepassingen).

SAP-ondersteuning-opmerkingen van toepassing op het implementeren van SAP HANA op SLES 12:

- [SAP Support Opmerking #1944799: SAP HANA richtlijnen voor de installatie van het besturingssysteem SLES](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html).
- [SAP Support Opmerking #2205917: SAP HANA DB aanbevolen besturingssysteeminstellingen voor het voor SLES 12 voor SAP-toepassingen](https://launchpad.support.sap.com/#/notes/2205917/E).
- [SAP-ondersteuning Opmerking #1984787 – SUSE Linux Enterprise Server 12: Opmerkingen bij de installatie van de](https://launchpad.support.sap.com/#/notes/1984787).
- [SAP Support Opmerking #171356 – SAP-Software op Linux: algemene informatie](https://launchpad.support.sap.com/#/notes/1984787).
- [SAP Support Opmerking #1391070 – oplossingen voor Linux-UUID](https://launchpad.support.sap.com/#/notes/1391070).

[Red Hat Enterprise Linux voor SAP HANA](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) is een andere aanbieding voor het uitvoeren van SAP HANA op grote HANA-instanties. Versies van RHEL 6.7 en 7.2 zijn beschikbaar. Neem notitie in systeemeigen Azure-VM's waarbij alleen RHEL 7.2 en meer recente versies worden ondersteund, HANA grote instanties bieden ondersteuning voor RHEL 6.7 ook. Maar we een RHEL 7.x versie gebruiken adviseren.

Aanvullende en nuttige SAP op Red Hat verwante koppelingen:
- [SAP HANA op Red Hat Linux Site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

SAP-ondersteuning-opmerkingen van toepassing op SAP HANA implementeren op Red Hat:

- [SAP Support Opmerking #2009879 - SAP HANA-richtlijnen voor het Red Hat Enterprise Linux (RHEL)-besturingssysteem](https://launchpad.support.sap.com/#/notes/2009879/E).
- [SAP Support Opmerking #2292690 - SAP HANA DB: Besturingssysteem aanbevolen instellingen voor RHEL 7](https://launchpad.support.sap.com/#/notes/2292690).
- [SAP Support Opmerking #2247020 - SAP HANA DB: Besturingssysteem aanbevolen instellingen voor RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020).
- [SAP Support Opmerking #1391070 – oplossingen voor Linux-UUID](https://launchpad.support.sap.com/#/notes/1391070).
- [SAP Support Opmerking #2228351 - Linux: SAP HANA-Database SP's 11 revisie 110 (of hoger) in 6 RHEL of SLES 11](https://launchpad.support.sap.com/#/notes/2228351).
- [SAP Support Opmerking #2397039 - Veelgestelde vragen over: SAP op RHEL](https://launchpad.support.sap.com/#/notes/2397039).
- [SAP Support Opmerking #1496410 - Red Hat Enterprise Linux 6.x: installatie en Upgrade](https://launchpad.support.sap.com/#/notes/1496410).
- [SAP Support Opmerking #2002167 - Red Hat Enterprise Linux 7.x: installatie en Upgrade](https://launchpad.support.sap.com/#/notes/2002167).

## <a name="time-synchronization"></a>Tijdsynchronisatie

SAP-toepassingen die zijn gebouwd op de architectuur van SAP NetWeaver zijn gevoelige op tijd verschillen voor de verschillende onderdelen die deel uitmaken van de SAP-systeem. SAP ABAP korte geheugendumps met de titel van Downloadfout van ZDATE\_grote\_tijd\_DIFF zijn waarschijnlijk vertrouwd, zoals deze korte dumpbestanden voor foutopsporing worden weergegeven wanneer de systeemtijd van de andere servers of virtuele machines te ver uit elkaar is verhuizen.

Voor SAP HANA op Azure (grote instanties), tijd synchronisatie uitgevoerd in Azure heeft&#39;t zijn van toepassing op de compute-eenheden in de stempels grote instantie. Deze synchronisatie is niet van toepassing voor het uitvoeren van SAP-toepassingen in systeemeigen Azure-VM's, terwijl Azure zorgt voor een systeem&#39;s tijd juist is gesynchroniseerd. Als gevolg hiervan een ander tijdstip server moet worden ingesteld die kan worden gebruikt door SAP-toepassingsservers die worden uitgevoerd op Azure VM's en de SAP HANA-database exemplaren die worden uitgevoerd op HANA grote instanties. De opslaginfrastructuur in grote instantie stempels is het tijd gesynchroniseerd met de NTP-servers.

## <a name="setting-up-smt-server-for-suse-linux"></a>SMT-server instellen voor SUSE Linux
SAP HANA grote instanties hebt geen directe verbinding met Internet. Het is daarom niet een eenvoudig proces aan zoals eenheden registreren bij de OS-provider te downloaden en toepassen van patches. In het geval van SUSE Linux mogelijk een oplossing voor het instellen van een SMT-server in een Azure-VM. Terwijl de virtuele Azure-machine moet worden gehost in een Azure-VNet is verbonden met de HANA grote instantie. Met dergelijke een SMT-server, kan de eenheid HANA grote instantie registreren en downloaden van patches. 

SUSE biedt een grotere handleiding op hun [abonnement beheerprogramma voor SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

Als voorwaarde voor de installatie van een SMT-server die voldoet aan vereisten voor de taak voor HANA grote instantie moet:

- Een Azure-VNet die is verbonden met het circuit HANA grote instantie ER.
- Een SUSE-account dat is gekoppeld aan een organisatie. Terwijl de organisatie een geldige SUSE-abonnement hebt moet.

### <a name="installation-of-smt-server-on-azure-vm"></a>Installatie van SMT-server op Azure VM

In deze stap installeert u het SMT-server in een Azure-VM. De eerste meting is voor aanmelding bij de [SUSE klant Center](https://scc.suse.com/)

Als u bent aangemeld, geeft gaat u naar de organisatie--> referenties van de organisatie. In deze sectie vindt u de referenties die nodig zijn voor het instellen van de SMT-server.

De derde stap is het installeren van een SUSE Linux-VM in de Azure-VNet. Voor het implementeren van de virtuele machine, neemt u een installatiekopie van de galerie SLES 12 SP2 van Azure. Een DNS-naam niet definiëren in het implementatieproces en gebruik geen statische IP-adressen zoals te zien is in deze schermopname

![VM-implementatie voor SMT-server](./media/hana-installation/image3_vm_deployment.png)

De geïmplementeerde virtuele machine een kleinere virtuele machine is en het interne IP-adres in het Azure-VNet van 10.34.1.4 kreeg. De naam van de virtuele machine is smtserver. Na de installatie, is de verbinding met de HANA grote instantie eenheid/eenheden gecontroleerd. Afhankelijk van hoe u naamomzetting georganiseerd mogelijk moet u de resolutie van de eenheden HANA grote instantie in etc/hosts van de Azure-VM configureren. Een extra schijf toevoegen aan de virtuele machine die u moet worden gebruikt voor het opslaan van de patches. De opstartschijf zelf kan worden te klein. In het geval is aangetoond dat is de schijf gekoppeld aan /srv/www/htdocs zoals wordt weergegeven in de volgende schermafbeelding. Er moet een schijf van 100 GB voldoende.

![VM-implementatie voor SMT-server](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Meld u aan bij de HANA grote instantie eenheid/eenheden, te onderhouden/etc/hosts en controleer of u de Azure-VM die moet worden uitgevoerd, het SMT-server via het netwerk kunt bereiken.

Nadat deze controle is voltooid, moet u zich aanmeldt bij de Azure-VM die het SMT-server moet worden uitgevoerd. Als u putty gebruikt voor aanmelding bij de virtuele machine, moet u deze reeks opdrachten uitvoeren in de bash-venster:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Na deze opdrachten wordt uitgevoerd, opnieuw opstarten van uw bash voor het activeren van de instellingen. Start vervolgens YAST.

In YAST, gaat u naar het onderhoud van Software en zoek naar smt. Selecteer smt, die automatisch wordt overgeschakeld naar yast2 smt zoals hieronder wordt weergegeven

![SMT in yast](./media/hana-installation/image5_smt_in_yast.PNG)


De selectie voor installatie op de smtserver accepteren. Wanneer geïnstalleerd, gaat u naar de configuratie van de SMT-server en voer de referenties van de organisatie van de SUSE klant Center die u eerder hebt opgehaald. De hostnaam van uw virtuele Azure-machine ook opgeven als de URL van de SMT-Server. In deze demonstratie is https://smtserver zoals weergegeven in de volgende afbeeldingen.

![Configuratie van SMT-server](./media/hana-installation/image6_configuration_of_smtserver1.png)

Als volgende stap moet u testen of de verbinding met het midden van de klant SUSE werkt. Zoals u in de volgende afbeeldingen, in het geval demonstratie ziet, dit werkt.

![Test verbinding maken met SUSE klant Center](./media/hana-installation/image7_test_connect.png)

Eenmaal de SMT setup wordt gestart, moet u een databasewachtwoord op te geven. Omdat dit een nieuwe installatie, moet u dit wachtwoord definiëren, zoals wordt weergegeven in de volgende afbeeldingen.

![Wachtwoord voor de database definiëren](./media/hana-installation/image8_define_db_passwd.PNG)

De volgende interactie hebt is wanneer een certificaat wordt gemaakt. Ga in het dialoogvenster zoals hierna ziet en de stap te gaan.

![Certificaat voor SMT-server maken](./media/hana-installation/image9_certificate_creation.PNG)

Er is mogelijk enkele minuten in de stap van "Controle uitvoeren synchronisatie" besteed aan het einde van de configuratie. Nadat de installatie en configuratie van de server SMT, vindt u de opslagplaats van de directory onder het koppelpunt punt /srv/www/htdocs/plus enkele submappen onder de opslagplaats. 

Start opnieuw op de server SMT en gerelateerde services met de volgende opdrachten.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

### <a name="download-of-packages-onto-smt-server"></a>Het downloaden van pakketten naar SMT-server

Nadat alle services opnieuw gestart zijn, selecteert u de juiste pakketten in SMT-beheer via het Yast. De selectie van het pakket is afhankelijk van de installatiekopie van het besturingssysteem van de server HANA grote instantie en niet op de SLES-versie of de versie van de virtuele machine waarop het SMT-server wordt uitgevoerd. Hieronder ziet u een voorbeeld van het scherm selecteren.

![Pakketten selecteren](./media/hana-installation/image10_select_packages.PNG)

Zodra u klaar met de selectie van het pakket bent, moet u de eerste kopie van de select-pakketten naar de SMT-server die u instelt starten. Dit exemplaar is geactiveerd in de shell met behulp van de opdracht smt-mirror zoals hieronder wordt weergegeven


![Pakketten naar SMT-server downloaden](./media/hana-installation/image11_download_packages.PNG)

Zoals u hierboven ziet, moeten de pakketten gekopieerd naar de mappen die zijn gemaakt op basis van het koppelpunt punt /srv/www/htdocs. Dit proces kan even duren. Afhankelijk van hoeveel pakketten die u selecteert, het kan duren een uur of langer.
Als dit proces is voltooid, moet u verplaatsen naar het SMT-clientinstallatie. 

### <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>Instellen van de client SMT op HANA grote instantie eenheden

De client (s) zijn in dit geval de eenheden HANA grote instantie. Het script clientSetup4SMT.sh de installatie van de SMT gekopieerd naar de Azure-VM. Kopie die via script zijn aan de eenheid HANA grote instantie dat u wilt verbinding maken met uw SMT-server. Start het script met de optie -h en geef deze als parameter de naam van uw SMT-server. In dit voorbeeld smtserver.

![SMT-client configureren](./media/hana-installation/image12_configure_client.PNG)

Er is mogelijk een scenario waarbij de belasting van het certificaat van de server door de client is voltooid, maar de registratie is mislukt, zoals hieronder weergegeven.

![Registratie van de client is mislukt](./media/hana-installation/image13_registration_failed.PNG)

Als de registratie is mislukt, Lees dit [SUSE ondersteuning biedt voor document](https://www.suse.com/de-de/support/kb/doc/?id=7006024) en bevat de stappen uit te voeren.

> [!IMPORTANT] 
> Als de servernaam van de moet u de naam van de virtuele machine, in dit geval smtserver, zonder de volledig gekwalificeerde domeinnaam opgeven. Alleen de virtuele machine de naam werkt. 

Nadat deze stappen zijn uitgevoerd, moet u de volgende opdracht uitvoeren op de eenheid HANA grote instantie

```
SUSEConnect –cleanup
```

> [!Note] 
> In onze tests hebben we altijd moeten over een paar minuten na deze stap. De clientSetup4SMT.sh direct na de corrigerende maatregelen die worden beschreven in het artikel SUSE is beëindigd met berichten die het certificaat niet gebruikt nog worden. Doorgaans 5-10 minuten wachten en het uitvoeren van clientSetup4SMT.sh is in configuratie van een geslaagde client beëindigd.

Als u in het probleem dat u nodig hebt om op te lossen op basis van de stappen van het SUSE-artikel hebt uitgevoerd, moet u opnieuw clientSetup4SMT.sh op de eenheid HANA grote instantie. Nu het moet worden voltooid, zoals hieronder weergegeven.

![Registratie is voltooid](./media/hana-installation/image14_finish_client_config.PNG)

Met deze stap maakt u het SMT-client van de eenheid HANA grote instantie verbinding maken op basis van de SMT-server die u hebt geïnstalleerd in de Azure-VM geconfigureerd. U kunt nu 'zypper van' of 'zypper in' patches voor het besturingssysteem installeren op HANA grote instanties of installeren van extra pakketten nemen. Het is duidelijk dat alleen u patches die u hebt gedownload krijgt voordat u op de SMT-server.


## <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Voorbeeld van een installatie van de SAP HANA op grote HANA-instanties
In deze sectie wordt beschreven hoe SAP HANA installeren op een eenheid HANA grote instantie. De status gestart die we hebben er als volgt uitzien:

- U hebt opgegeven Microsoft de gegevens voor het implementeren van een SAP HANA grote instantie.
- U hebt de SAP HANA grote instantie van Microsoft hebt ontvangen.
- U hebt een Azure-VNet die is verbonden met uw on-premises netwerk hebt gemaakt.
- U hebt verbinding het circuit ExpressRotue voor HANA grote instanties met de dezelfde Azure-VNet.
- U hebt een Azure-VM die u als een jump-box voor HANA grote instanties gebruikt geïnstalleerd.
- U hebt ervoor gezorgd dat u verbinding vanuit de jump-box op uw machine HANA grote instantie en vice versa maken kunt.
- U hebt gecontroleerd of alle vereiste pakketten en patches worden geïnstalleerd.
- U leest de SAP-opmerkingen en documentatie met betrekking tot HANA-installatie op het besturingssysteem u gebruikt en u hebt ervoor gezorgd dat de HANA-release van keuze wordt ondersteund voor het besturingssysteem release.

Wat wordt weergegeven in de volgende reeksen is het downloaden van de HANA-installatiepakketten voor de VM, in dit geval wordt uitgevoerd op een Windows-besturingssysteem, het exemplaar van de pakketten aan de eenheid HANA grote instantie en de volgorde van de installatie van de jump-box.

### <a name="download-of-the-sap-hana-installation-bits"></a>Het downloaden van de SAP HANA-installatie-bits
Aangezien de eenheden HANA grote instantie geen directe verbinding met internet hebben, niet kan u de installatiepakketten rechtstreeks downloaden vanaf SAP voor HANA grote instantie VM. Om te strijden tegen de ontbrekende directe verbinding met internet, moet u de jump-box. U downloaden de pakketten naar de jump-box VM.

De installatiepakketten HANA downloaden, moet u een SAP-S-gebruiker of een andere gebruiker, waardoor u toegang krijgt tot de SAP-Marketplace. Met deze reeks schermen volgen na het aanmelden:

Ga naar [SAP Service Marketplace](https://support.sap.com/en/index.html) > Klik op downloaden van Software >-installaties en -Upgrade > door alfabetische > onder H: SAP HANA-Platform Edition > SAP HANA-Platform Edition 2.0 > installatie > downloaden de volgende bestanden

![HANA-installatie downloaden](./media/hana-installation/image16_download_hana.PNG)

In het geval demonstratie hebben we SAP HANA 2.0 installatiepakketten gedownload. In de Azure jump-box VM uitbreiden u de zelfuitpakkend archieven naar de map zoals hieronder wordt weergegeven.

![Uitpakken van HANA-installatie](./media/hana-installation/image17_extract_hana.PNG)

Als het archief zijn uitgepakt, Kopieer de map die zijn gemaakt door de extractie, in het geval hierboven 51052030, aan de HANA grote instantie-eenheid in het volume /hana/shared in een map die u hebt gemaakt.

> [!Important]
> Kopieer de installatiepakketten niet in de hoofdmap of opstartinstallatiekopieën LUN, omdat er ruimte is beperkt en moet worden gebruikt door andere processen ook.


### <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>SAP HANA installeren op de eenheid HANA grote instantie
Als u wilt installeren SAP HANA, moet u aanmelden als gebruiker hoofdmap. Alleen basis heeft onvoldoende machtigingen voor het installeren van SAP HANA.
Het eerste wat dat u moet doen is het instellen van machtigingen op de map die u in/hana/gedeelde hebt gekopieerd. De machtigingen nodig om in te stellen, zoals

```
chmod –R 744 <Installation bits folder>
```

Als u wilt voor het installeren van SAP HANA met behulp van de grafische installatie, moet het pakket gtk2 worden geïnstalleerd op de HANA grote instanties. Controleer of deze is geïnstalleerd met de opdracht

```
rpm –qa | grep gtk2
```

In de verdere stappen, zijn we aan te tonen de SAP HANA-installatie met behulp van de grafische gebruikersinterface. Als volgende stap gaat u naar de installatiemap en navigeer naar de map sub HDB_LCM_LINUX_X86_64. Starten

```
./hdblcmgui 
```
uit die map. Nu ophalen u geleid via een reeks schermen waar u nodig hebt om te zorgen dat de gegevens voor de installatie. In het geval is aangetoond dat installeert we de SAP HANA-database-server en de SAP HANA client-onderdelen. Onze selectie is daarom 'SAP HANA-Database, zoals hieronder wordt weergegeven

![HANA in installatie selecteren](./media/hana-installation/image18_hana_selection.PNG)

In het volgende scherm kiest u de optie Nieuw systeem installeren

![Selecteer de nieuwe installatie HANA](./media/hana-installation/image19_select_new.PNG)

Na deze stap moet u kiezen tussen verschillende extra onderdelen die verder kunnen worden geïnstalleerd met de SAP HANA-database-server.

![Aanvullende HANA onderdelen selecteren](./media/hana-installation/image20_select_components.PNG)

We hebben gekozen ten behoeve van deze documentatie wordt de SAP HANA-Client en de SAP HANA Studio. We ook een exemplaar omhoog geïnstalleerd. daarom in het volgende scherm moet u kiezen één Host-systeem 

![Selecteer de installatie van de scale-up](./media/hana-installation/image21_single_host.PNG)

In het volgende scherm moet u enkele gegevens opgeven

![Bieden van SAP HANA-SID](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Als HANA systeem-ID (SID), moet u voor de dezelfde SID, als u Microsoft verstrekt wanneer u de implementatie van HANA grote instantie besteld. Een andere SID is de installatie mislukken vanwege problemen met toegang tot de machtiging op de verschillende volumes

Als de installatie van gedeelde map die u gebruikt de/hana/directory. In de volgende stap moet u de locaties opgeven voor de HANA-gegevensbestanden en de HANA-logboekbestanden


![Geef HANA Logboeklocatie](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> U moet definiëren als gegevens en logboekbestanden de volumes die al bij de koppelpunten met de SID die u hebt gekozen in de selectie van het scherm voordat u dit scherm is geleverd. Als de beveiligings-id niet met de naam die u hebt getypt overeen in, in het scherm voordat, gaat u terug en aanpassen van de SID op de waarde die u op de koppelpunten hebt.

De hostnaam van de beoordeling en uiteindelijk pas dit in de volgende stap. 

![Naam van de beoordeling-host](./media/hana-installation/image24_review_host_name.PNG)

In de volgende stap moet u ook gegevens die u naar Microsoft is opgegeven toen u de implementatie van HANA grote instantie besteld op te halen. 

![Geef systeemgebruiker UID en groeps-id](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> U moet dezelfde gebruikers-ID van systeem en ID van de groep van de gebruiker opgeven als u Microsoft verstrekt als u de implementatie van de eenheid order. Als u niet de dezelfde id's geven, mislukt de installatie van SAP HANA op de eenheid HANA grote instantie.

In de volgende twee schermen die we niet zichtbaar zijn in deze documentatie, moet u het wachtwoord opgeven voor de gebruiker van de SAP HANA-database en het wachtwoord voor de gebruiker sapadm, die wordt gebruikt voor de SAP-Hostagent die wordt geïnstalleerd als onderdeel van de SAP HANA-datab exemplaar van de as-omgeving.

Na het definiëren van het wachtwoord is een bevestigingsvenster weergegeven. Controleer alle gegevens die worden vermeld en doorgaan met de installatie. U bereiken een scherm met de installatievoortgang waarin de voortgang van de installatie, zoals de onderstaande

![Voortgang van de installatie controleren](./media/hana-installation/image27_show_progress.PNG)

Als de installatie is voltooid, moet u een afbeelding, zoals de volgende uitvoer

![Installatie is voltooid](./media/hana-installation/image28_install_finished.PNG)

Op dit moment moet de SAP HANA-instantie actief en werkend en klaar voor gebruik. U moet geen verbinding maken met het vanuit SAP HANA Studio. Controleer ook of u controleren of de meest recente patches van SAP HANA en toepassen van patches.
























































 







 





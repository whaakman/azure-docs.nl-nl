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
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dad088138fea2dd4fadc0cc9eed71245c32a8e0b
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162653"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Installeren en configureren van SAP HANA (grote instanties) op Azure

Voordat u dit artikel leest, raken met [HANA grote instanties algemene termen](hana-know-terms.md) en de [HANA grote instanties SKU's](hana-available-skus.md).

De installatie van SAP HANA is uw verantwoordelijkheid en kunt u de activiteit starten na de levering van een nieuwe SAP HANA op Azure (grote instanties)-server. En nadat de verbinding tussen uw Azure vnet (s) en de HANA grote instantie eenheid/eenheden tot stand is gebracht. 

> [!Note]
> Per SAP-beleid, moet de installatie van SAP HANA worden uitgevoerd door een persoon die is gecertificeerd voor SAP HANA-installaties uitvoeren. Een persoon, die is doorgegeven het koppelen van technologie met SAP gecertificeerde examen, certificeringsexamen SAP HANA-installatie, of door een SAP-gecertificeerde systeemintegrator (SI).

Controleer opnieuw, met name bij het plannen van HANA-2.0 installeren [SAP Support Opmerking #2235581 - SAP HANA: ondersteunde besturingssystemen](https://launchpad.support.sap.com/#/notes/2235581/E) om ervoor te zorgen dat het besturingssysteem wordt ondersteund met de SAP HANA release u besluiten om te installeren. U ontdekt dat de ondersteund besturingssysteem voor HANA 2.0 beperkter dan het besturingssysteem dat wordt ondersteund voor HANA 1.0 is. 

> [!IMPORTANT] 
> Voor Type II eenheden alleen de SLES 12 SP2 OS-versie op dit moment ondersteund. 

U moet het volgende controleren voordat u begint met de HANA-installatie:
- [Valideren van de HLI eenheid/eenheden](#validate-the-hana-large-instance-units)
- [Configuratie van besturingssysteem](#operating-system)
- [Netwerkconfiguratie](#networking)
- [Opslagconfiguratie](#storage)


## <a name="validate-the-hana-large-instance-units"></a>Valideren van de HANA grote instantie eenheid/eenheden

Nadat u de eenheid HANA grote instantie van Microsoft ontvangt het valideren van de volgende instellingen en pas dit nodig is.

**Stap 1** na ontvangst van de HANA grote instantie en toegang tot stand gebrachte en in te stellen verbinding met de exemplaren, bestaat uit het registreren van het besturingssysteem van het exemplaar bij uw besturingssysteem-provider. Deze stap omvat het SUSE Linux-besturingssysteem wordt geregistreerd bij een exemplaar van SUSE SMT die u nodig hebt geïmplementeerd in een virtuele machine in Azure. De eenheid HANA grote instantie kan verbinding maken met dit exemplaar SMT (Zie [hoe u setup SMT-server voor SUSE Linux](hana-setup-smt.md)). Of uw Red Hat-besturingssysteem moet worden geregistreerd met de Red Hat Doelabonnementbeheerder u verbinding wilt maken. Zie ook opmerkingen in deze [document](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Deze stap is ook nodig zijn om te kunnen zijn voor het vullen van het besturingssysteem. Een taak die in de verantwoordelijkheid van de klant. Voor SUSE, vindt u documentatie om te installeren en configureren van SMT [hier](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

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

### <a name="time-synchronization"></a>Tijdsynchronisatie

SAP-toepassingen die zijn gebouwd op de architectuur van SAP NetWeaver zijn gevoelige op tijd verschillen voor de verschillende onderdelen die deel uitmaken van de SAP-systeem. SAP ABAP korte geheugendumps met de titel van Downloadfout van ZDATE\_grote\_tijd\_DIFF zijn waarschijnlijk vertrouwd, zoals deze korte dumpbestanden voor foutopsporing worden weergegeven wanneer de systeemtijd van de andere servers of virtuele machines te ver uit elkaar is verhuizen.

Voor SAP HANA op Azure (grote instanties), tijd synchronisatie uitgevoerd in Azure heeft&#39;t zijn van toepassing op de compute-eenheden in de stempels grote instantie. Deze synchronisatie is niet van toepassing voor het uitvoeren van SAP-toepassingen in systeemeigen Azure-VM's, terwijl Azure zorgt voor een systeem&#39;s tijd juist is gesynchroniseerd. Als gevolg hiervan een ander tijdstip server moet worden ingesteld die kan worden gebruikt door SAP-toepassingsservers die worden uitgevoerd op Azure VM's en de SAP HANA-database exemplaren die worden uitgevoerd op HANA grote instanties. De opslaginfrastructuur in grote instantie stempels is het tijd gesynchroniseerd met de NTP-servers.


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

Lees het document en een eenheid HANA grote instantie zoeken, ontdekt u dat de eenheden voor HANA/gegevens afkomstig met in plaats daarvan royale schijfvolume zijn en dat we beschikken over een volume HANA/log/back-up. De reden waarom we de HANA/gegevens zo groot grootte is dat de storage-momentopnamen we voor u als een klant bieden van hetzelfde schijfvolume gebruikmaakt. Dit betekent dat de meer opslag momentopnamen die u uitvoert, hoe meer opslagruimte wordt gebruikt door momentopnamen in uw opslagvolumes die zijn toegewezen. Het volume HANA/log/back-up wordt niet beschouwd moet het volume op databaseback-ups worden geplaatst. Het formaat wordt aangepast om te worden gebruikt als back-upvolume voor de HANA transactielogboekback-ups. Zie voor meer informatie [SAP HANA (grote instanties) met hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

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


**Volgende stappen**

- Raadpleeg [HANA-installatie op HLI](hana-example-installation.md)










































 







 





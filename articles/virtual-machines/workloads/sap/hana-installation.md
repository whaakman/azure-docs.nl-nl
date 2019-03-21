---
title: SAP HANA installeren op SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Over het installeren van SAP HANA op een SAP HANA op Azure (grote instanties).
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
ms.date: 03/05/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5b6c636366d494901a34078100290084298de686
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57999840"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Installeren en configureren van SAP HANA (grote instanties) op Azure

Voordat u dit artikel leest, raken met [HANA grote instanties algemene termen](hana-know-terms.md) en de [HANA grote instanties SKU's](hana-available-skus.md).

De installatie van SAP HANA is uw verantwoordelijkheid. U kunt beginnen met het installeren van een nieuwe SAP HANA op Azure (grote instanties)-server nadat u de verbinding tot stand tussen uw Azure-netwerken en de HANA grote instantie eenheid/eenheden brengen. 

> [!Note]
> Per SAP-beleid, moet de installatie van SAP HANA worden uitgevoerd door een persoon die het examen Certified SAP-technologie koppelen, SAP HANA-installatie-certificeringsexamen is verstreken, of is een SAP-gecertificeerde systeemintegrator (SI).

Wanneer u van plan bent HANA 2.0 installeren, Zie [SAP support Opmerking #2235581 - SAP HANA: Ondersteunde besturingssystemen](https://launchpad.support.sap.com/#/notes/2235581/E) om ervoor te zorgen dat het besturingssysteem wordt ondersteund met SAP HANA release die u installeert. De ondersteund besturingssysteem voor HANA 2.0 is beperkter dan het ondersteunde besturingssysteem voor HANA 1.0. 

> [!IMPORTANT] 
> Voor Type II-eenheden, momenteel die alleen de versie van het besturingssysteem van SLES 12 SP2 wordt ondersteund. 

U moet het volgende controleren voordat u begint met de HANA-installatie:
- [HLI eenheid/eenheden](#validate-the-hana-large-instance-units)
- [Configuratie van besturingssysteem](#operating-system)
- [Netwerkconfiguratie](#networking)
- [Opslagconfiguratie](#storage)


## <a name="validate-the-hana-large-instance-units"></a>Valideren van de HANA grote instantie eenheid/eenheden

Nadat u de eenheid HANA grote instantie van Microsoft hebt ontvangen, het valideren van de volgende instellingen en pas dit nodig is.

De **eerste stap** nadat u de HANA grote instantie ontvangen en tot stand brengen van toegang en connectiviteit met de exemplaren, bestaat uit het registreren van het besturingssysteem van het exemplaar met de provider van uw besturingssysteem. Deze stap omvat het registreren van uw SUSE Linux-besturingssysteem in een exemplaar van SUSE SMT die geïmplementeerd in een virtuele machine in Azure. 

De eenheid HANA grote instantie kan verbinding maken met dit SMT-exemplaar. (Zie voor meer informatie, [SMT-server instellen voor SUSE Linux](hana-setup-smt.md)). U kunt ook moet uw Red Hat-besturingssysteem worden geregistreerd met de Red Hat Doelabonnementbeheerder die u verbinding maken moet met. Voor meer informatie, Zie de opmerkingen in [wat SAP HANA op Azure (grote instanties) is?](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Deze stap is nodig voor het patchen van het besturingssysteem, de verantwoordelijkheid van de klant is. Voor SUSE, de documentatie te vinden voor het installeren en configureren van SMT op deze pagina over [SMT installatie](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

De **tweede stap** is om te controleren op nieuwe patches en verbeteringen van de specifieke versie/versie van het besturingssysteem. Controleer of de patch-niveau van de HANA grote instantie is de meest recente status. Mogelijk zijn er gevallen waarbij de meest recente patches worden niet opgenomen. Na een HANA grote instantie-eenheid overname, is het verplicht om te controleren of patches moeten worden toegepast.

De **derde stap** bestaat uit het controleren van de relevante SAP-opmerkingen voor het installeren en configureren van SAP HANA op specifieke release/versie van het besturingssysteem. Vanwege het wijzigen van aanbevelingen of wijzigingen voor de SAP-opmerkingen bij de of configuraties die afhankelijk van de installatie van de afzonderlijke scenario's zijn, Microsoft niet altijd mogelijk een eenheid HANA grote instantie perfect configureren. 

Daarom is het verplicht is voor u als een klant om te lezen van de SAP-opmerkingen met betrekking tot SAP HANA voor de exacte versie van Linux. Controleer de configuraties van het besturingssysteem/versie ook en de configuratie-instellingen van toepassing als u dat nog niet gedaan hebt.

Specifiek, Controleer de volgende parameters en uiteindelijk aanpassen aan:

- net.core.rmem_max = 16777216
- net.core.wmem_max = 16777216
- net.core.rmem_default = 16777216
- net.core.wmem_default = 16777216
- net.core.optmem_max = 16777216
- net.ipv4.tcp_rmem = 65536 16777216 16777216
- net.ipv4.tcp_wmem = 65536 16777216 16777216

Beginnen met SP1 SLES12 en RHEL 7.2, moeten deze parameters worden ingesteld in een configuratiebestand in de map /etc/sysctl.d. Bijvoorbeeld, moet een configuratiebestand met de naam 91-NetApp-HANA.conf worden gemaakt. Voor oudere versies van SLES en RHEL moet deze parameters set in/etc/sysctl.conf.

Voor alle versies van de RHEL beginnen met RHEL 6.3, houd rekening met het volgende: 
- De sunrpc.tcp_slot_table_entries = 128 parameter in/etc/modprobe.d/sunrpc-local.conf moet worden ingesteld. Als het bestand niet bestaat, moet u eerst maken door de volgende vermelding toe te voegen: 
    - options sunrpc tcp_max_slot_table_entries=128

De **vierde stap** is om te controleren of de systeemtijd van de eenheid HANA grote instantie. De exemplaren zijn geïmplementeerd met de tijdzone van een systeem. Deze tijdzone vertegenwoordigt de locatie van de Azure-regio waarin de stempel HANA grote instantie zich bevindt. U kunt de systeemtijd of de tijdzone van de exemplaren die van jou wijzigen. 

Als u meer exemplaren in uw tenant rangschikken, moet u aan te passen van de tijdzone van de zojuist geleverde exemplaren. Microsoft heeft geen inzicht in de tijdzone van het systeem dat u instellen met de exemplaren na de overdracht. Geïmplementeerde exemplaren kunnen dus niet worden ingesteld in dezelfde tijdzone als het account dat u hebt gewijzigd. Er is uw verantwoordelijkheid als klant aan te passen van de tijdzone van de instantie (s) die zijn overgedragen, indien nodig. 

De **vijfde stap** is om te controleren op etc/hosts. Als de blades afgegeven ophalen, hebben ze verschillende IP-adressen die zijn toegewezen voor verschillende doeleinden. Controleer het bestand etc/hosts. Wanneer de eenheden worden toegevoegd aan een bestaande tenant, etc/hosts van de zojuist geïmplementeerde systemen correct onderhouden met de IP-adressen van systemen die eerder werden geleverd niet verwacht. Het is uw verantwoordelijkheid als de klant zorgt ervoor dat een zojuist geïmplementeerde exemplaar kan communiceren en omzetten van de namen van de eenheden op die u eerder hebt in uw tenant hebt geïmplementeerd. 


## <a name="operating-system"></a>Besturingssysteem

> [!IMPORTANT] 
> Alleen de versie van het besturingssysteem van SLES 12 SP2 wordt momenteel ondersteund voor Type II-eenheden. 

De wisselruimte van het geleverde installatiekopie van het besturingssysteem is ingesteld op 2 GB volgens de [SAP support Opmerking #1999997 - Veelgestelde vragen over: SAP HANA-geheugen](https://launchpad.support.sap.com/#/notes/1999997/E). Als een klant, indien een andere instelling gewenst, moet u deze zelf instellen.

[SUSE Linux Enterprise Server 12 SP1 voor SAP-toepassingen](https://www.suse.com/products/sles-for-sap/download/) is de verdeling van Linux voor SAP HANA op Azure (grote instanties) geïnstalleerd. Deze specifieke distributie biedt mogelijkheden voor SAP-specifieke 'buiten het vak' (met inbegrip van de vooraf ingestelde parameters voor het uitvoeren van SAP op SLES effectief).

Zie [bibliotheek-/ whitepapers Resource](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) op de website van SUSE en [SAP op SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) op de SAP Community netwerk (SCN) voor verschillende nuttige bronnen die betrekking hebben op de implementatie van SAP HANA op SLES (met inbegrip van de configuratie van hoge beschikbaarheid, beveiliging die specifiek is voor de SAP-bewerkingen, en meer).

Hieronder vindt u aanvullende en nuttige SAP op SUSE-gerelateerde koppelingen:

- [SAP HANA op SUSE Linux-site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [Aanbevolen procedures voor SAP: Enqueue replication – SAP NetWeaver on SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113)
- [ClamSAP – virusbeveiliging SLES voor SAP](https://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (met inbegrip van SLES 12 voor SAP-toepassingen)

Hier volgen de SAP-opmerkingen voor ondersteuning die gelden voor het implementeren van SAP HANA op SLES 12:

- [SAP-ondersteuning Opmerking #1944799: SAP HANA-richtlijnen voor de installatie van de SLES-besturingssysteem](https://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
- [SAP support Opmerking #2205917: SAP HANA DB aanbevolen instellingen voor het besturingssysteem voor SLES 12 voor SAP-toepassingen](https://launchpad.support.sap.com/#/notes/2205917/E)
- [SAP support Opmerking #1984787 – SUSE Linux Enterprise Server 12: opmerkingen bij de installatie](https://launchpad.support.sap.com/#/notes/1984787)
- [SAP-ondersteuning Opmerking #171356 – SAP-software op Linux:  Algemene informatie](https://launchpad.support.sap.com/#/notes/1984787)
- [SAP support Opmerking #1391070 – Linux UUID-oplossingen](https://launchpad.support.sap.com/#/notes/1391070)

[Red Hat Enterprise Linux voor SAP HANA](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) is een andere aanbieding voor het uitvoeren van SAP HANA op grote HANA-instanties. Versies van RHEL 6.7 en 7.2 zijn beschikbaar. Houd er rekening mee in systeemeigen Azure-VM's, waarbij alleen RHEL 7.2 en meer recente versies worden ondersteund, dat HANA grote instanties bieden ondersteuning voor RHEL 6.7 ook. We raden echter aan met behulp van een versie van RHEL 7.x.

Hieronder vindt u meer nuttige SAP op Red Hat verwante koppelingen:
- [SAP HANA op Red Hat Linux site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

Hieronder vindt u SAP support opmerkingen in die van toepassing zijn op SAP HANA implementeren op Red Hat:

- [SAP-ondersteuning Opmerking #2009879 - SAP HANA-richtlijnen voor het besturingssysteem van Red Hat Enterprise Linux (RHEL)](https://launchpad.support.sap.com/#/notes/2009879/E)
- [SAP support Opmerking #2292690 - SAP HANA DB: Aanbevolen besturingssysteeminstellingen voor RHEL 7](https://launchpad.support.sap.com/#/notes/2292690)
- [SAP Support Opmerking #2247020 - SAP HANA DB: Aanbevolen besturingssysteeminstellingen voor RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020)
- [SAP support Opmerking #1391070 – Linux UUID-oplossingen](https://launchpad.support.sap.com/#/notes/1391070)
- [SAP support Opmerking #2228351 - Linux: SAP HANA-Database SP's 11 revisie 110 (of hoger) in 6 RHEL of SLES 11](https://launchpad.support.sap.com/#/notes/2228351)
- [SAP support Opmerking #2397039 - Veelgestelde vragen over: SAP op RHEL](https://launchpad.support.sap.com/#/notes/2397039)
- [SAP support Opmerking #1496410 - Red Hat Enterprise Linux 6.x: Installatie en upgrade](https://launchpad.support.sap.com/#/notes/1496410)
- [SAP support Opmerking #2002167 - Red Hat Enterprise Linux 7.x: Installatie en upgrade](https://launchpad.support.sap.com/#/notes/2002167)

### <a name="time-synchronization"></a>Tijdsynchronisatie

SAP-toepassingen die zijn gebouwd op de architectuur van SAP NetWeaver zijn gevoelig zijn voor verschillen van de tijd voor de verschillende onderdelen die deel uitmaken van de SAP-systeem. SAP ABAP korte geheugendumps met de titel van Downloadfout van ZDATE\_grote\_tijd\_DIFF bent waarschijnlijk bekend. Dat komt doordat deze korte dumpbestanden voor foutopsporing worden weergegeven wanneer de systeemtijd van de andere servers of virtuele machines te ver uit elkaar is verhuizen.

Voor SAP HANA op Azure (grote instanties), tijdsynchronisatie die is uitgevoerd in Azure is niet van toepassing op de compute-eenheden in de stempels grote instantie. Deze synchronisatie is niet van toepassing voor het uitvoeren van SAP-toepassingen in systeemeigen Azure-VM's, omdat Azure zorgt ervoor dat de tijd van een systeem goed wordt gesynchroniseerd. 

Als gevolg hiervan moet u een ander tijdstip-server die kan worden gebruikt door de SAP-toepassingsservers die worden uitgevoerd op virtuele Azure-machines en de SAP HANA-database-exemplaren die worden uitgevoerd op HANA grote instanties instellen. De opslaginfrastructuur in grote instantie stempels is tijd gesynchroniseerd met de NTP-servers.


## <a name="networking"></a>Netwerken
Gaan we ervan uit dat u de aanbevelingen in het ontwerpen van uw Azure virtual networks en deze virtuele netwerken verbinden met de HANA grote instanties gevolgd, zoals beschreven in de volgende documenten:

- [Overzicht van de SAP HANA (grote instantie) en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Er zijn enkele details over de netwerken van de afzonderlijke eenheden opgemerkt. Elke eenheid HANA grote instantie wordt geleverd met twee of drie IP-adressen die zijn toegewezen aan twee of drie NIC-poorten. Drie IP-adressen worden gebruikt in HANA scale-out configuraties en het scenario met HANA system replication. Een van de IP-adressen die is toegewezen aan de NIC van de eenheid is buiten de server IP-adresgroep die wordt beschreven in [SAP HANA (grote instanties) overzicht en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

Zie voor meer informatie over Ethernet-details voor uw architectuur, de [HLI scenario's ondersteund](hana-supported-scenario.md).

## <a name="storage"></a>Opslag

De opslagindeling voor SAP HANA op Azure (grote instanties) is geconfigureerd door SAP HANA op Azure-service-beheer via SAP aanbevolen richtlijnen. Deze richtlijnen worden beschreven in de [opslagvereisten voor SAP HANA](https://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) technisch document. 

De ruwe grootte van de verschillende volumes met de verschillende HANA grote instanties SKU's wordt gedocumenteerd in [SAP HANA (grote instanties) overzicht en architectuur op Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

De naamgeving van de opslagvolumes worden vermeld in de volgende tabel:

| Opslaggebruik | Naam koppelen | Volumenaam | 
| --- | --- | ---|
| HANA-gegevens | /hana/data/SID/mnt0000<m> | Storage IP:/hana_data_SID_mnt00001_tenant_vol |
| HANA-logboek | /hana/log/SID/mnt0000<m> | Storage IP:/hana_log_SID_mnt00001_tenant_vol |
| HANA logboekback-up | /Hana/log/backups | Storage IP:/hana_log_backups_SID_mnt00001_tenant_vol |
| HANA gedeeld | /Hana/Shared/SID | Storage IP:/hana_shared_SID_mnt00001_tenant_vol/shared |
| usr/sap | /usr/SAP/SID | Storage IP:/hana_shared_SID_mnt00001_tenant_vol/usr_sap |

*SID* is de HANA-instantie systeem-ID. 

*Tenant* is een interne opsomming van bewerkingen bij het implementeren van een tenant.

HANA usr/sap delen hetzelfde volume. De naamgeving van de quorumbron: bevat de systeem-ID van de HANA-instanties, evenals het aantal koppelen. In implementaties van scale-up is er slechts één koppeling zoals mnt00001. In scale-out implementaties, daarentegen, ziet u zoveel koppelingen als u het model en worker-knooppunten hebben. 

Back-volumes zijn gedeeld en die is gekoppeld aan elk knooppunt in de configuratie van de scale-out voor scale-out-omgevingen, gegevens, log, en het logboek. Voor configuraties die meerdere exemplaren van SAP, is een andere set volumes gemaakt en gekoppeld aan de eenheid HANA grote instantie. Zie voor lay-out opslaggegevens voor uw scenario, [HLI scenario's ondersteund](hana-supported-scenario.md).

Wanneer u een eenheid HANA grote instantie bekijkt, ontdekt u dat de eenheden voor HANA/gegevens afkomstig zijn met vriendelijke schijfvolume en er is een volume HANA/log/back-up. De reden dat we de HANA/gegevens zo groot is gemaakt, is de storage-momentopnamen bieden we dat u als een klant zijn met behulp van dezelfde schijfvolume. Meer opslagmomentopnamen die u uitvoert, hoe meer opslagruimte wordt gebruikt door momentopnamen in uw opslagvolumes die zijn toegewezen. 

Het volume HANA/log/back-up mag niet het volume voor een databaseback-ups. Het wordt aangepast als de back-upvolume worden gebruikt voor de HANA transactielogboekback-ups. Zie voor meer informatie, [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Naast de opslag die wordt geleverd, kunt u extra opslagcapaciteit in stappen van 1 TB aanschaffen. Deze extra opslag kan worden toegevoegd als nieuwe volumes aan een HANA grote instantie.

Tijdens de onboarding met SAP HANA op Azure service management, de klant Hiermee geeft u een gebruiker-ID (UID) en de groep groeps-ID (id) voor de groep van gebruikers- en sapsys sidadm (bijvoorbeeld: 1000,500). Tijdens de installatie van de SAP HANA-systeem, moet u deze dezelfde waarden gebruiken. Omdat u implementeren meerdere HANA-exemplaren op een eenheid wilt, krijgt u meerdere sets van volumes (één set voor elk exemplaar). Tijdens de implementatie moet u als gevolg hiervan het volgende wordt gedefinieerd:

- De SID van de verschillende HANA-instanties (sidadm is afgeleid van het).
- De grootte van het geheugen van de verschillende HANA-instanties. De grootte van het geheugen per exemplaar definieert de grootte van de volumes in elke volumeset afzonderlijke.

Op basis van aanbevelingen voor opslag-provider, de volgende koppelingsopties zijn geconfigureerd voor alle gekoppelde volumes (met uitzondering van opstarten LUN):

- NFS-rw bergen = 4, hard, timeo = 600, rsize = 1048576, wsize = 1048576, Intro, noatime, vergrendelen 0 0

Deze punten zijn geconfigureerd in/etc/fstab zoals wordt weergegeven in de volgende afbeeldingen koppelen:

![fstab van gekoppelde volumes in HANA grote instantie eenheid](./media/hana-installation/image1_fstab.PNG)

De uitvoer van de opdracht df -h op een eenheid S72m HANA grote instantie lijkt:

![fstab van gekoppelde volumes in HANA grote instantie eenheid](./media/hana-installation/image2_df_output.PNG)


De controller voor opslag en de knooppunten in de stempels grote instantie worden gesynchroniseerd met de NTP-servers. Als u de SAP HANA op Azure (grote instanties)-eenheden en Azure-VM's op basis van een NTP-server synchroniseert, moet er geen afwijking veel tijd tussen de infrastructuur en de rekeneenheden in Azure of grote instantie stempels.

Voor het optimaliseren van SAP HANA naar de opslag die onder gebruikt, moet u de volgende parameters van de SAP HANA-configuratie instellen:

- max_parallel_io_requests 128
- async_read_submit op
- async_write_submit_active op
- async_write_submit_blocks all
 
Voor SAP HANA 1.0 versies tot SPS12, deze parameters worden ingesteld tijdens de installatie van de SAP HANA-database, zoals beschreven in [SAP-notitie #2267798 - configuratie van de SAP HANA-database](https://launchpad.support.sap.com/#/notes/2267798).

U kunt ook de parameters na de installatie van de SAP HANA-database configureren met behulp van de hdbparam-framework. 

De opslag die wordt gebruikt in HANA grote instanties heeft een maximale bestandsgrootte. De [formaat beperking is 16 TB](https://docs.netapp.com/ontap-9/index.jsp?topic=%2Fcom.netapp.doc.dot-cm-vsmg%2FGUID-AA1419CF-50AB-41FF-A73C-C401741C847C.html) per bestand. In tegenstelling tot in de beperkingen van de bestandsgrootte in de bestandssystemen EXT3 is HANA niet op de hoogte van de opslag-beperking afgedwongen door de opslag HANA grote instanties impliciet. Als gevolg hiervan wordt HANA niet automatisch gemaakt een nieuw bestand na het verstrijken van de maximale bestandsgrootte van 16TB. Als HANA probeert te uitbreiding van het bestand dan 16 TB, rapporteert HANA fouten en de indexserver loopt vast aan het einde.

> [!IMPORTANT]
> Om te voorkomen dat een poging om te groeien gegevensbestanden voorbij de maximale bestandsgrootte 16 TB aan opslag van HANA grote instantie HANA, moet u de volgende parameters instellen in het configuratiebestand van de SAP HANA-global.ini
> 
> - datavolume_striping=true
> - datavolume_striping_size_gb = 15000
> - Zie ook SAP Opmerking [#2400005](https://launchpad.support.sap.com/#/notes/2400005)
> - Houd rekening met SAP-notitie [#2631285](https://launchpad.support.sap.com/#/notes/2631285)


Met SAP HANA 2.0, is het framework hdbparam afgeschaft. Als gevolg hiervan moeten de parameters worden ingesteld met behulp van SQL-opdrachten. Zie voor meer informatie, [SAP-notitie #2399079: Afschaffing van hdbparam in HANA 2](https://launchpad.support.sap.com/#/notes/2399079).

Raadpleeg [HLI scenario's ondersteund](hana-supported-scenario.md) voor meer informatie over de opslagindeling voor uw architectuur.


**Volgende stappen**

- Raadpleeg [HANA-installatie op HLI](hana-example-installation.md)










































 







 





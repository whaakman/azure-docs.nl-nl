---
title: SAP HANA installeren op SAP HANA in Azure (grote exemplaren) | Microsoft Docs
description: Het SAP HANA installeren op een SAP HANA in Azure (grote exemplaar).
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ef85c098058c97e5ec6d758fcf1dab5b1a87786
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Het installeren en configureren van SAP HANA (grote exemplaren) in Azure

Hier volgen enkele belangrijke definities moet weten voordat u deze handleiding leest. In [SAP HANA (grote exemplaren) overzicht en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) er zijn twee verschillende soorten HANA grote exemplaar eenheden met geïntroduceerd:

- S72, S72m S144, S144m, S192 en S192m die naar verwezen als 'Type ik klasse' van SKU's.
- S384, S384m S384xm, S576, S768 en S960 die naar als de 'Type II class-instructie van SKU's verwezen.

De klasse-aanduiding gaat moet worden gebruikt in de documentatie van de grote exemplaar HANA uiteindelijk verwijzen naar verschillende mogelijkheden en vereisten op basis van HANA grote exemplaar SKU's.

Er zijn andere definities die wordt vaak gebruikt:
- **Grote exemplaar stempel:** een hardware-infrastructuur stack die SAP HANA TDI gecertificeerd en toegewezen voor SAP HANA-exemplaren in Azure uitvoeren.
- **SAP HANA in Azure (grote exemplaren):** officiële taal voor de aanbieding in Azure uitvoeren HANA-exemplaren op SAP HANA TDI gecertificeerde hardware die wordt geïmplementeerd in grote exemplaar stempels in verschillende Azure-regio's. De verwante term **HANA grote exemplaar** kort voor SAP HANA in Azure (grote exemplaren) en veel is deze technische handleiding gebruikt.


De installatie van de SAP HANA is uw verantwoordelijkheid en kunt u de activiteit starten na de levering van een nieuwe SAP HANA op Azure (grote exemplaren)-server. En na de connectiviteit tussen uw Azure-VNet(s) en de HANA grote exemplaar eenheid tot stand is gebracht. 

> [!Note]
> Per SAP-beleid, moet de installatie van de SAP HANA worden uitgevoerd door een persoon die is gecertificeerd voor SAP HANA-installaties uitvoeren. Een persoon, die is doorgegeven de examen gecertificeerd SAP-technologie koppelen, certificeringsexamen SAP HANA-installatie, of door een SAP-certificering system integrator (SI).

Controle op opnieuw, vooral bij het plannen voor het installeren van HANA 2.0, [SAP ondersteuning Opmerking #2235581 - SAP HANA: ondersteunde besturingssystemen](https://launchpad.support.sap.com/#/notes/2235581/E) om ervoor te zorgen dat het besturingssysteem wordt ondersteund met de SAP HANA release u besluiten om te installeren. Zult u ontdekken dat de ondersteund besturingssysteem voor HANA 2.0 beperkter dan het besturingssysteem dat wordt ondersteund voor HANA 1.0 is. 

## <a name="first-steps-after-receiving-the-hana-large-instance-units"></a>Eerste stappen na de ontvangst van het exemplaar van HANA grote eenheid

**Stap 1** na ontvangst van het grote HANA-exemplaar en hebben toegang tot stand gebracht en connectiviteit met de exemplaren, is het besturingssysteem van het exemplaar te registreren bij uw OS-provider. Deze stap omvat het registreren van uw SUSE Linux-besturingssysteem in een exemplaar van SUSE SMT die u nodig hebt geïmplementeerd in een virtuele machine in Azure. De eenheid HANA grote exemplaar kan verbinding maken met dit exemplaar SMT (Zie verderop in deze documentatie). Of uw besturingssysteem RedHat moet worden geregistreerd met Red Hat abonnement Manager u verbinding moet maken. Zie ook opmerkingen in deze [document](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Deze stap is ook nodig zijn om te kunnen patch van het besturingssysteem. Een taak die is in de verantwoordelijkheid van de klant. Voor SUSE, documentatie installeren en configureren van SMT vinden [hier](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

**Tweede stap** is om te controleren op nieuwe patches en verbeteringen van specifieke release/versie van het besturingssysteem. Controleer of de patch-niveau van het grote HANA-exemplaar op de meest recente toestand. Op basis van de timing van OS patch/releases en wijzigingen in de installatiekopie van het die Microsoft kunt implementeren, kunnen er gevallen waarbij de meest recente patches niet opgenomen worden kunnen. Daarom is een verplichte stap nadat u via een eenheid HANA grote exemplaar om te controleren of patches die relevant zijn voor beveiliging, functionaliteit, beschikbaarheid en prestaties door de leverancier van de bepaalde Linux ondertussen zijn uitgebracht en moeten worden toegepast.

**Het derde stap** uitchecken van de relevante SAP-opmerkingen voor het installeren en configureren van SAP HANA op specifieke release/versie van het besturingssysteem is. Vanwege het wijzigen van aanbevelingen of wijzigingen in de opmerkingen bij de SAP, of-configuraties die afhankelijk van afzonderlijke installatiescenario's zijn Microsoft altijd kan niet worden naar een grote exemplaar HANA Unit perfect geconfigureerd. Daarom is het noodzakelijk dat u als klant lezen dat opmerkingen bij de SAP gerelateerd aan SAP HANA op de exacte versie van Linux. Controleer de configuraties van release/versie van het besturingssysteem nodig ook en de configuratie-instellingen toepassen waarbij niet is gebeurd.

In specifieke, Controleer de volgende parameters en uiteindelijk wordt aangepast aan:

- NET.Core.rmem_max 16777216 =
- NET.Core.wmem_max 16777216 =
- NET.Core.rmem_default 16777216 =
- NET.Core.wmem_default 16777216 =
- NET.Core.optmem_max 16777216 =
- NET.IPv4.tcp_rmem 65536 16777216 16777216 =
- NET.IPv4.tcp_wmem 65536 16777216 16777216 =

Beginnen met SLES12 SP1 en RHEL 7.2, moeten deze parameters worden ingesteld in een configuratiebestand in de map /etc/sysctl.d. Bijvoorbeeld, moet een configuratiebestand met de naam 91-NetApp-HANA.conf worden gemaakt. Voor oudere versies van SLES en RHEL moet deze parameters set in/etc/sysctl.conf.

Voor alle RHEL vrijgegeven en beginnen met SLES12, de 
- sunrpc.tcp_slot_table_entries = 128

parameter moet worden ingesteld als in/etc/modprobe.d/sunrpc-local.conf. Als het bestand niet bestaat, moet deze eerst worden gemaakt door de volgende vermelding toe te voegen: 
- Opties voor sunrpc tcp_max_slot_table_entries = 128

**Vierde stap** is om te controleren van de systeemtijd van uw exemplaar van HANA grote eenheid. De exemplaren worden geïmplementeerd met de tijdzone van een systeem met daarin de locatie van de Azure-regio die de HANA grote exemplaar stempel bevindt zich in. U bent kunt wijzigen van de systeemtijd of de tijdzone van de exemplaren die u eigenaar. Dit leidt en ordening meer exemplaren in uw tenant, moet u aan te passen aan de tijdzone van de nieuwe geleverde exemplaren worden voorbereid. Microsoft-bewerkingen hebben geen inzicht in de tijdzone die u instellen met de exemplaren na de overdracht. Geïmplementeerde exemplaren kunnen daarom niet worden ingesteld in dezelfde tijdzone als het account dat u hebt gewijzigd. Als gevolg hiervan, is het uw verantwoordelijkheid als klant om te controleren en indien nodig de tijdzone van de exemplaren afgegeven aanpassen. 

**Vijfde stap** is om te controleren, etc/hosts. Als de blades afgegeven ophalen, hebben ze verschillende IP-adressen toegewezen voor verschillende doeleinden (Zie volgende sectie). Controleer het etc/hosts-bestand. In gevallen waar eenheden zijn toegevoegd aan een bestaande tenant is niet van plan bent etc/hosts van de geïmplementeerde systemen correct worden beheerd met de IP-adressen van eerder geleverde systemen. Daarom is u als klant om te controleren van de juiste instellingen, dat een zojuist geïmplementeerde exemplaar kan communiceren en de namen van de eerder geïmplementeerde eenheden in uw tenant omzetten. 

## <a name="networking"></a>Netwerken
We gaan ervan uit dat u de aanbevelingen bij het ontwerpen van uw Azure VNets en deze VNets verbinden met de grote exemplaren HANA zoals beschreven in deze documenten gevolgd:

- [Overzicht van de SAP HANA (grote exemplaar) en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Infrastructuur voor SAP HANA (grote exemplaren) en de verbindingen van Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Er zijn een aantal details waard te vermelden over de netwerken van de afzonderlijke eenheden. Elke eenheid HANA grote exemplaar wordt geleverd met twee of drie IP-adressen die zijn toegewezen aan twee of drie NIC-poorten van de eenheid. Drie IP-adressen worden gebruikt in HANA scale-out configuraties en het scenario HANA System Replication. Een van de IP-adressen toegewezen aan de Netwerkinterfacekaart van de eenheid valt buiten de Server IP-adresgroep die is beschreven in de [SAP HANA (grote exemplaar) overzicht en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

De verdeling van eenheden met twee IP-adressen toegewezen moet eruitzien als:

- eth0.xx moet een IP-adres toegewezen valt buiten het bereik van groep met IP-adres dat u naar Microsoft verzonden. Dit IP-adres moet worden gebruikt voor het onderhouden van in/etc/hosts van het besturingssysteem.
- eth1.xx moet een IP-adres toegewezen dat wordt gebruikt voor communicatie met NFS hebben. Daarom deze adressen komen **niet** moet worden behouden in etc/hosts om verkeer van de exemplaar-exemplaar binnen de tenant.

Een bladeconfiguratie met twee IP-adressen toegewezen is niet geschikt is voor implementatie gevallen van HANA System Replication of HANA scale-out. Als twee IP-adressen toegewezen alleen gelet en implementeren van een dergelijke configuratie, neem contact op met de SAP HANA op Azure Service Management ophalen van een derde IP-adres in een derde willen VLAN toegewezen. Voor grote exemplaar HANA eenheden met drie IP-adressen toegewezen op drie NIC-poorten, gelden de volgende informatie over het gebruiksregels:

- eth0.xx moet een IP-adres toegewezen valt buiten het bereik van groep met IP-adres dat u naar Microsoft verzonden. Dit IP-adres wordt daarom niet worden gebruikt voor het onderhouden van in/etc/hosts van het besturingssysteem.
- eth1.xx moet een IP-adres toegewezen dat wordt gebruikt voor communicatie met NFS opslag hebben. Dit type adressen moet daarom niet worden beheerd in etc/hosts.
- eth2.xx mag uitsluitend worden gebruikt in etc/hosts voor de communicatie tussen de verschillende exemplaren worden bijgehouden. Deze adressen is ook de IP-adressen die moeten worden behouden in scale-out HANA configuraties als IP-adressen die HANA wordt gebruikt voor de configuratie tussen knooppunten.



## <a name="storage"></a>Storage

De opslagindeling voor SAP HANA in Azure (grote exemplaren) wordt geconfigureerd via de SAP HANA op Azure Service Management via SAP hulplijnen aanbevolen zoals beschreven in [opslagvereisten voor SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) witboek. De ruwe grootte van de verschillende volumes met de verschillende HANA grote exemplaren SKU's is beschreven in [SAP HANA (grote exemplaar) overzicht en architectuur op Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

De naamgeving van de opslagvolumes worden vermeld in de volgende tabel:

| Opslaggebruik | De naam van koppelpunt | Volumenaam | 
| --- | --- | ---|
| HANA gegevens | /Hana/Data/SID/mnt0000<m> | Opslag IP: / hana_data_SID_mnt00001_tenant_vol |
| HANA-logboek | /Hana/log/SID/mnt0000<m> | Opslag IP: / hana_log_SID_mnt00001_tenant_vol |
| HANA logboekback-up | /Hana/log/backups | Opslag IP: / hana_log_backups_SID_mnt00001_tenant_vol |
| HANA gedeeld | /Hana/Shared/SID | Opslag IP: / hana_shared_SID_mnt00001_tenant_vol/gedeeld |
| usr/sap | /usr/SAP/SID | Opslag IP: / hana_shared_SID_mnt00001_tenant_vol/usr_sap |

Waar SID het exemplaar HANA systeem-ID = 

- En tenantverkeer = van een interne opsomming van bewerkingen bij het implementeren van een tenant.

Zoals u ziet, HANA gedeeld en usr/sap delen hetzelfde volume. De naamgeving volgt de namen van de quorumbron: bevat de systeem-ID van de exemplaren HANA, evenals het aantal koppelen. In implementaties van scale-up alleen is één koppelpunt, zoals mnt00001. Terwijl in scale-out-implementatie u zoveel koppelingen als ziet hebben u worker en master knooppunten. Voor scale-out-omgeving, gegevens, logboek, back-logboekvolumes gedeeld en gekoppeld aan elk knooppunt in de configuratie van de scale-out. Voor configuraties met meerdere exemplaren van het SAP, een andere set volumes gemaakt en gekoppeld aan de eenheid HAN grote exemplaar.

Als u het artikel lezen en zoeken van een grote exemplaar HANA-eenheid, zult u ontdekken dat de eenheden worden geleverd met in plaats daarvan vriendelijke schijfvolume HANA/gegevens en dat we een volume HANA/log/back-up hebben. De reden waarom we de HANA/gegevens zo groot formaat is dat de opslag-momentopnamen we voor u als klant bieden het dezelfde schijfvolume wordt gebruikt. Dit betekent dat de meer opslagruimte momentopnamen die u uitvoert, des te meer ruimte is verbruikt door momentopnamen in de opslagvolumes toegewezen. Het volume HANA/log/back-up wordt niet beschouwd moet het volume de databaseback-ups in plaatsen. Het wordt aangepast als back-volume voor de HANA transactielogboekback-ups worden gebruikt. In toekomstige snapshot versies van de opslag-self service dat we heeft betrekking op dit specifieke volume om frequentere momentopnamen. En met die frequentere replicaties naar de site van de herstel na noodgevallen als u om een optie in voor de disaster recovery functionaliteit van de infrastructuur HANA grote exemplaar. Zie voor meer informatie [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

Naast de opslag die is opgegeven, kunt u extra opslagcapaciteit in stappen van 1 TB aanschaffen. Deze extra opslagruimte kan worden toegevoegd als nieuwe volumes op een grote HANA-exemplaren.

Tijdens de voorbereiding met SAP HANA op Azure-servicebeheer, de klant Hiermee geeft u een gebruikers-ID (UID) en groep-ID (GID) voor de groep van gebruikers- en sapsys sidadm (ex: 1000,500) is het nodig dat tijdens de installatie van het systeem SAP HANA deze dezelfde waarden worden gebruikt. Als u implementeren meerdere HANA-exemplaren op een eenheid wilt, kunt u meerdere sets van volumes (één ingesteld voor elk exemplaar) ophalen. Tijdens de implementatie moet u als gevolg hiervan definiëren:

- De SID van de verschillende HANA-exemplaren (sidadm is buiten deze afgeleid).
- De grootte van het geheugen van de verschillende exemplaren van HANA. Omdat de grootte van het geheugen per exemplaren de grootte van de volumes in elke set afzonderlijk volume bepaalt.

Op basis van opslagaanbevelingen voor provider de volgende mount-opties zijn geconfigureerd voor alle gekoppelde volumes (met uitzondering van opstarten LUN):

- NFS-rw bergen = 4, hard, timeo = 600, rsize = 1048576, wsize = 1048576, Intro, noatime, vergrendelen 0 0

Deze punten zijn geconfigureerd in/etc/fstab zoals weergegeven in de onderstaande afbeeldingen koppelen:

![fstab van gekoppelde volumes in grote exemplaar HANA-eenheid](./media/hana-installation/image1_fstab.PNG)

De uitvoer van de opdracht df -h op een eenheid S72m HANA grote exemplaar eruit als:

![fstab van gekoppelde volumes in grote exemplaar HANA-eenheid](./media/hana-installation/image2_df_output.PNG)


De controller voor opslag en de knooppunten in de stempels grote exemplaar worden gesynchroniseerd met NTP-servers. Met u synchronisatie van de SAP HANA op Azure (grote exemplaren) eenheden en virtuele Azure-machines op basis van een NTP-server, moet er geen aanzienlijke tijd afwijking gebeurt tussen de infrastructuur en de compute-eenheden in Azure of grote exemplaar stempels.

Om te optimaliseren SAP HANA naar de opslag die onder gebruikt, moet u ook de volgende SAP HANA-configuratieparameters instellen:

- max_parallel_io_requests 128
- async_read_submit op
- async_write_submit_active op
- alle async_write_submit_blocks
 
Voor SAP HANA 1.0 versies tot SPS12, deze parameters worden ingesteld tijdens de installatie van de SAP HANA-database, zoals beschreven in [SAP Opmerking #2267798 - configuratie van de SAP HANA-Database](https://launchpad.support.sap.com/#/notes/2267798)

U kunt ook de parameters na de installatie van de SAP HANA-database configureren met behulp van het framework hdbparam. 

Met SAP HANA 2.0, is het framework hdbparam afgeschaft. De parameters moeten als gevolg hiervan worden ingesteld met behulp van SQL-opdrachten. Zie voor meer informatie [SAP-notitie #2399079: eliminatie van hdbparam in HANA 2](https://launchpad.support.sap.com/#/notes/2399079).


## <a name="operating-system"></a>Besturingssysteem

Wisselruimte van het geleverde installatiekopie van het besturingssysteem is ingesteld op 2 GB volgens de [SAP ondersteuning Opmerking #1999997 - Veelgestelde vragen over: SAP HANA geheugen](https://launchpad.support.sap.com/#/notes/1999997/E). Een andere instelling hebben gewenste moet worden ingesteld door u als een klant.

[SUSE Linux Enterprise Server 12 SP1 voor SAP-toepassingen](https://www.suse.com/products/sles-for-sap/hana) is de verdeling van Linux is geïnstalleerd voor SAP HANA in Azure (grote exemplaren). Deze bepaalde distributie biedt mogelijkheden voor SAP-specifieke &quot;gebruiksklaar&quot; (inclusief vooraf ingestelde parameters voor het uitvoeren van de SAP effectief op SLES).

Zie [Resource bibliotheek/White Papers](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) op de website SUSE en [SAP op SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) op SAP Community netwerk (SCN) voor verschillende nuttige informatiebronnen met betrekking tot het implementeren van SAP HANA op SLES (inclusief de opzet van hoge Beschikbaarheid, beveiliging hardening die specifiek zijn voor SAP-bewerkingen en meer).

Aanvullende en nuttige SAP op SUSE-gerelateerde koppelingen:

- [SAP HANA op SUSE Linux Site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [Best Practice voor SAP: in de wachtrij plaatsen replicatie – SAP NetWeaver op SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113).
- [ClamSAP – SLES virusbeveiliging voor SAP](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (inclusief SLES 12 voor SAP-toepassingen).

SAP ondersteuning opmerkingen van toepassing op het implementeren van SAP HANA op SLES 12:

- [SAP ondersteuning Opmerking #1944799 – SAP HANA richtlijnen voor de installatie van het besturingssysteem SLES](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html).
- [SAP ondersteuning Opmerking #2205917 – SAP HANA DB aanbevolen OS-instellingen voor SLES 12 voor SAP-toepassingen](https://launchpad.support.sap.com/#/notes/2205917/E).
- [SAP ondersteuning Opmerking #1984787 – SUSE Linux Enterprise Server 12: Opmerkingen bij de installatie van de](https://launchpad.support.sap.com/#/notes/1984787).
- [SAP ondersteuning Opmerking #171356 – SAP-Software op Linux: algemene informatie](https://launchpad.support.sap.com/#/notes/1984787).
- [SAP ondersteuning Opmerking #1391070 – Linux UUID oplossingen](https://launchpad.support.sap.com/#/notes/1391070).

[Red Hat Enterprise Linux voor SAP HANA](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) is een andere aanbieding voor SAP HANA uitgevoerd op grote HANA-exemplaren. Releases van RHEL 6.7 en 7.2 zijn beschikbaar. Neem notitie in in de systeemeigen virtuele Azure-machines waar alleen RHEL 7.2 en meer recente versies worden ondersteund, HANA grote exemplaren bieden ondersteuning voor RHEL 6.7 ook. Maar wordt u aangeraden een RHEL 7.x release.

Aanvullende en nuttige SAP op Red Hat verwante koppelingen:
- [SAP HANA op Red Hat Linux Site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

SAP ondersteuning opmerkingen bij de implementatie van SAP HANA op Red Hat die van toepassing zijn:

- [SAP ondersteuning Opmerking #2009879 - SAP HANA richtlijnen voor het Red Hat Enterprise Linux (RHEL)-besturingssysteem](https://launchpad.support.sap.com/#/notes/2009879/E).
- [SAP ondersteuning Opmerking #2292690 - SAP HANA-database: OS aanbevolen instellingen voor RHEL 7](https://launchpad.support.sap.com/#/notes/2292690).
- [SAP ondersteuning Opmerking #2247020 - SAP HANA-database: OS aanbevolen instellingen voor RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020).
- [SAP ondersteuning Opmerking #1391070 – Linux UUID oplossingen](https://launchpad.support.sap.com/#/notes/1391070).
- [SAP ondersteuning Opmerking #2228351 - Linux: SAP HANA-Database SP's 11 revisie 110 (of hoger) op de RHEL 6 of SLES 11](https://launchpad.support.sap.com/#/notes/2228351).
- [#2397039 - Veelgestelde vragen over SAP ondersteuning Opmerking: SAP-op de RHEL](https://launchpad.support.sap.com/#/notes/2397039).
- [SAP ondersteuning Opmerking #1496410 - Red Hat Enterprise Linux 6.x: installatie en Upgrade](https://launchpad.support.sap.com/#/notes/1496410).
- [SAP ondersteuning Opmerking #2002167 - Red Hat Enterprise Linux 7.x: installatie en Upgrade](https://launchpad.support.sap.com/#/notes/2002167).

## <a name="time-synchronization"></a>Tijdsynchronisatie

SAP-toepassingen die zijn gebaseerd op de architectuur SAP NetWeaver zijn gevoelig voor tijdsverschil voor de verschillende onderdelen waaruit het SAP-systeem. SAP ABAP korte dumpen met de fouttitel van ZDATE\_grote\_tijd\_DIFF zijn waarschijnlijk vertrouwd, zoals deze korte dumpbestanden weergegeven wanneer de systeemtijd van de andere servers of virtuele machines te ver uit elkaar is verhuizen.

Voor SAP HANA in Azure (grote exemplaren) uitgevoerd in Azure heeft &#39; tijdsynchronisatie t toepassen op de compute-eenheden in de grote exemplaar stempels. Deze synchronisatie is niet van toepassing voor het uitvoeren van SAP-toepassingen in systeemeigen Azure VM's, zoals Azure zorgt ervoor een systeem &#39;dat; s tijd juist is gesynchroniseerd. Als gevolg hiervan een afzonderlijke tijd server moet worden ingesteld die kan worden gebruikt door SAP toepassingsservers uitgevoerd op Azure VM's en de SAP HANA-database instanties die worden uitgevoerd op grote HANA-exemplaren. De infrastructuur voor opslag in grote exemplaar stempels is tijd die zijn gesynchroniseerd met NTP-servers.

## <a name="setting-up-smt-server-for-suse-linux"></a>SMT-server instellen voor SUSE Linux
SAP HANA grote exemplaren hebben geen directe verbinding met Internet. Het is daarom niet een eenvoudig proces aan dergelijke eenheid registreren bij de OS-provider te downloaden en patches toepassen. In het geval van SUSE Linux mogelijk een oplossing voor het instellen van een SMT-server in een Azure VM. Terwijl de virtuele machine in Azure moet worden gehost in een Azure-VNet is verbonden met het grote HANA-exemplaar. Met dergelijke een SMT-server, kan de eenheid HANA grote exemplaar registreren en patches te downloaden. 

SUSE biedt een uitgebreidere handleiding op hun [abonnement beheerprogramma voor SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

Als de voorwaarde voor de installatie van een SMT-server die voldoet aan de taak voor grote HANA-exemplaar moet:

- Een Azure-VNet dat is verbonden met het circuit HANA grote exemplaar ER.
- Een SUSE-account dat is gekoppeld aan een organisatie. Terwijl de organisatie een geldig SUSE-abonnement hebt moet.

### <a name="installation-of-smt-server-on-azure-vm"></a>Installatie van SMT-server op Azure VM

In deze stap installeert u het SMT-server in een Azure VM. De eerste eenheid is aan te melden bij de [SUSE Customer Center](https://scc.suse.com/)

Als u bent aangemeld,--gaat u naar de organisatie > referenties van de organisatie. In deze sectie moet u de referenties die nodig zijn voor het instellen van de SMT-server vinden.

De derde stap is het installeren van een SUSE Linux virtuele machine in Azure VNet. Duren voor het implementeren van de virtuele machine, een installatiekopie SLES 12 SP2-galerie van Azure. Een DNS-naam niet definiëren in het implementatieproces en gebruik niet statische IP-adressen zoals in deze schermafbeelding te maken

![VM-implementatie voor SMT-server](./media/hana-installation/image3_vm_deployment.png)

De geïmplementeerde virtuele machine een kleinere VM is en het interne IP-adres in het Azure VNet van 10.34.1.4 kreeg. Naam van de virtuele machine is smtserver. De verbinding met de grote HANA exemplaar/eenheden is na de installatie gecontroleerd. Afhankelijk van hoe u naamomzetting geordend mogelijk moet u resolutie van de eenheden HANA grote exemplaar in etc/hosts van de Azure VM configureren. Een extra schijf toevoegen aan de virtuele machine die u wilt worden gebruikt voor het opslaan van de patches. De opstartschijf zelf mogelijk te klein. In het geval gedemonstreerd, is de schijf is gekoppeld aan /srv/www/htdocs zoals weergegeven in de volgende schermafbeelding. Er moet een schijf van 100 GB voldoende.

![VM-implementatie voor SMT-server](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Aanmelden bij de eenheid HANA grote exemplaar, / etc/hosts onderhouden en controleren of u de Azure-virtuele machine die moet worden uitgevoerd, de SMT-server via het netwerk kunt bereiken.

Nadat deze controle is voltooid, moet u zich aanmelden bij de virtuele machine van Azure die de SMT-server moet worden uitgevoerd. Als u zich aanmelden bij de virtuele machine putty gebruikt, moet u deze reeks opdrachten uitvoeren in uw bash-venster:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Na het uitvoeren van deze opdrachten opnieuw opstarten van uw bash voor het activeren van de instellingen. Start vervolgens YAST.

Ga naar softwareonderhoud en zoek naar smt in YAST. Selecteer smt die automatisch wordt overgeschakeld naar yast2 smt zoals hieronder wordt weergegeven

![SMT in yast](./media/hana-installation/image5_smt_in_yast.PNG)


De selectie voor installatie op de smtserver accepteren. Eenmaal geïnstalleerd, gaat u naar de configuratie van de SMT-server en voert u de organisatie-referenties van het SUSE klant Center die u eerder hebt opgehaald. Voer uw Azure VM-hostnaam ook als de URL van de SMT-Server. In deze demonstratie was https://smtserver zoals weergegeven in de volgende afbeeldingen.

![Configuratie van SMT-server](./media/hana-installation/image6_configuration_of_smtserver1.png)

Volgende stap moet u testen of de verbinding met het midden van de klant SUSE werkt. Zoals u in de onderstaande afbeeldingen, in het geval demonstratie ziet het werkt.

![Test verbinding maken met SUSE Customer Center](./media/hana-installation/image7_test_connect.png)

Eenmaal de SMT setup wordt gestart, moet u het databasewachtwoord van een op te geven. Omdat het een nieuwe installatie, moet u dit wachtwoord definiëren, zoals wordt weergegeven in de volgende afbeeldingen.

![Wachtwoord voor de database definiëren](./media/hana-installation/image8_define_db_passwd.PNG)

De volgende interactie die u hebt is wanneer een certificaat wordt gemaakt. Ga in het dialoogvenster zoals volgende en de stap moet worden voortgezet.

![Certificaat voor SMT-server maken](./media/hana-installation/image9_certificate_creation.PNG)

Er zijn mogelijk enkele minuten in de stap van "Controle uitvoeren synchronisatie" besteed aan het einde van de configuratie. Nadat de installatie en configuratie van de server SMT, zult u de opslagplaats directory onder het koppelpunt punt /srv/www/htdocs/plus een aantal submappen onder de opslagplaats. 

De SMT-server en de verwante services met de volgende opdrachten opnieuw gestart.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

### <a name="download-of-packages-onto-smt-server"></a>Tijdens het downloaden van pakketten naar SMT-server

Nadat alle services opnieuw gestart zijn, selecteert u de juiste pakketten in SMT-beheer met behulp van Yast. De selectie van het pakket is afhankelijk van de installatiekopie van het besturingssysteem van de server HANA grote exemplaar en niet de SLES release of de versie van de virtuele machine waarop het SMT-server wordt uitgevoerd. Een voorbeeld van het selectiescherm worden hieronder weergegeven.

![Pakketten selecteren](./media/hana-installation/image10_select_packages.PNG)

Als u klaar met de selectie van het pakket bent, moet u start de initiële kopie van de select-pakketten naar de SMT-server die u instelt. Dit exemplaar wordt gegeven in de shell met behulp van de opdracht smt-mirror zoals hieronder wordt weergegeven


![Pakketten naar SMT-server downloaden](./media/hana-installation/image11_download_packages.PNG)

Als u hierboven ziet, moeten de pakketten naar de mappen gemaakt onder het koppelpunt punt /srv/www/htdocs ophalen gekopieerd. Dit kan even duren. Afhankelijk van hoeveel pakketten die u selecteert, het kan duren tot één uur of meer.
Als dit proces is voltooid, moet u verplaatsen naar het SMT-clientinstallatie. 

### <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>De client SMT op grote exemplaar HANA eenheden ingesteld

De client (s) zijn in dit geval de eenheden HANA grote exemplaar. Het script clientSetup4SMT.sh de installatie van de SMT gekopieerd naar de Azure VM. Kopie die script via de eenheid HANA grote exemplaar u wilt verbinding maken met uw SMT-server. Het script met de optie -h Start en de naam van uw server SMT geven als parameter. In dit voorbeeld smtserver.

![SMT-client configureren](./media/hana-installation/image12_configure_client.PNG)

Er is mogelijk een scenario waarbij de belasting van het certificaat van de server door de client is voltooid, maar de registratie is mislukt, zoals hieronder wordt weergegeven.

![De registratie van de client is mislukt](./media/hana-installation/image13_registration_failed.PNG)

Als de registratie is mislukt, Lees dit [SUSE ondersteunen document](https://www.suse.com/de-de/support/kb/doc/?id=7006024) en er de stappen uit te voeren.

> [!IMPORTANT] 
> Als de servernaam van de moet u de naam van de virtuele machine, in dit geval smtserver, zonder de volledig gekwalificeerde domeinnaam opgeven. Alleen de virtuele machine naam werkt. 

Nadat u deze stappen zijn uitgevoerd, moet u de volgende opdracht uitvoeren op de eenheid HANA grote exemplaar

```
SUSEConnect –cleanup
```

> [!Note] 
> In onze tests hebben we altijd moeten wacht een paar minuten na deze stap. De clientSetup4SMT.sh onmiddellijk na de corrigerende maatregelen beschreven in het artikel SUSE beëindigd met dat het certificaat niet geldig nog berichten. Meestal 5-10 minuten wachten en het uitvoeren van clientSetup4SMT.sh beëindigd in een geslaagde clientconfiguratie.

Als u het probleem dat u nodig hebt om op te lossen op basis van de stappen van het artikel SUSE is gedetecteerd, moet u clientSetup4SMT.sh op de eenheid HANA grote exemplaar opnieuw starten. Nu het moet worden voltooid zoals hieronder wordt weergegeven.

![Clientregistratie is geslaagd](./media/hana-installation/image14_finish_client_config.PNG)

Met deze stap maakt u het SMT-client van de eenheid HANA grote exemplaar verbinding maken met de SMT-server die u hebt geïnstalleerd in de Azure VM geconfigureerd. U kunt nu 'zypper up' of 'zypper in' OS patches voor grote exemplaren HANA installeren of extra pakketten installeren nemen. Ervan wordt uitgegaan dat u alleen patches die u hebt gedownload krijgen kunt voordat op de SMT-server.


## <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Voorbeeld van een SAP HANA-installatie op grote HANA-exemplaren
Deze sectie ziet u hoe SAP HANA installeren op een grote exemplaar HANA-eenheid. De startstatus van de hebben we eruitzien als:

- U hebt Microsoft de gegevens voor het implementeren van een grote SAP HANA-exemplaar opgegeven.
- U ontvangen het grote SAP HANA-exemplaar van Microsoft.
- U hebt een Azure VNet dat is verbonden met uw on-premises netwerk gemaakt.
- U verbinding het circuit ExpressRotue voor grote exemplaren HANA naar het hetzelfde Azure-VNet.
- U hebt een Azure VM u als een korte inleiding voor grote exemplaren HANA gebruiken geïnstalleerd.
- U hebt aangebracht of dat u verbinding in het vak korte inleiding op uw machine HANA grote exemplaar en vice versa maken kunt.
- U hebt gecontroleerd of alle vereiste pakketten en -patches worden geïnstalleerd.
- U leest de SAP-opmerkingen en documentatie over HANA installatie van het besturingssysteem u gebruikt en ervoor hebt gezorgd dat de HANA-release van de keuze van het besturingssysteem wordt ondersteund release.

Wat wordt weergegeven in de volgende reeksen is het downloaden van de installatiepakketten HANA aan het vak jump VM, in dit geval wordt uitgevoerd op een Windows-besturingssysteem, het exemplaar van de pakketten op de eenheid HANA grote exemplaar en de volgorde van de installatie.

### <a name="download-of-the-sap-hana-installation-bits"></a>Tijdens het downloaden van de SAP HANA-bits installatie
Aangezien de eenheden HANA grote exemplaar geen directe verbinding met internet hebben, downloaden u niet rechtstreeks de installatiepakketten uit SAP voor HANA grote exemplaar VM. Als u wilt opheffen ontbrekende directe verbinding met internet, moet u het selectievakje korte inleiding in. U kunt de pakketten downloaden aan het vak jump VM.

Als u wilt de installatiepakketten HANA downloaden, moet u een SAP-S-gebruiker of een andere gebruiker, waarmee u toegang tot de SAP-Marketplace. Deze reeks schermen doorlopen na het aanmelden:

Ga naar [SAP Service Marketplace](https://support.sap.com/en/index.html) > Software downloaden klikt u op >-installaties en -Upgrade > alfabetische Index > onder H – SAP HANA Platform editie > SAP HANA Platform Edition 2.0 > installatie > de volgende bestanden downloaden

![HANA installatie downloaden](./media/hana-installation/image16_download_hana.PNG)

In het geval demonstratie hebben we installatiepakketten voor SAP HANA 2.0 gedownload. Klik in het Azure jump VM uitbreiden u zelfuitpakkende archiefbestanden naar de map zoals hieronder wordt weergegeven.

![Pak HANA installatie](./media/hana-installation/image17_extract_hana.PNG)

Wanneer het archief zijn uitgepakt, kopieert u de map die is gemaakt door de extractie in het geval hierboven 51052030, de HANA grote exemplaar eenheid in het volume /hana/shared in een map die u hebt gemaakt.

> [!Important]
> Kopieer de installatiepakketten niet in de hoofdmap of opstartinstallatiekopieën LUN, aangezien er ruimte is beperkt en moet worden gebruikt door andere processen ook.


### <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>SAP HANA installeren op de eenheid HANA grote exemplaar
Om te installeren SAP HANA, moet u zich aanmelden als gebruiker hoofdmap. Alleen hoofdmap heeft onvoldoende machtigingen voor het installeren van SAP HANA.
Het eerste wat dat u moet doen is machtigingen worden ingesteld voor de map die u gekopieerd naar/hana/gedeeld. De machtigingen moeten worden ingesteld zoals

```
chmod –R 744 <Installation bits folder>
```

Als u wilt een SAP HANA grafische Setup installeert, moet het pakket gtk2 worden geïnstalleerd op de grote HANA-exemplaren. Controleer of deze is geïnstalleerd met de opdracht

```
rpm –qa | grep gtk2
```

Verdere stappen zijn we demonstreren van de SAP HANA-instellingen met de grafische gebruikersinterface. Volgende stap gaat u naar de installatiemap en navigeer naar de map sub HDB_LCM_LINUX_X86_64. Starten

```
./hdblcmgui 
```
buiten-map. Nu ophalen u door geleid een reeks schermen waarin u wilt de gegevens leveren voor de installatie. In het geval is aangetoond dat installeert we de SAP HANA-database-server en de SAP HANA-clientonderdelen. Onze selectie is daarom 'SAP HANA-Database, zoals hieronder wordt weergegeven

![Selecteer HANA in installatie](./media/hana-installation/image18_hana_selection.PNG)

In het volgende scherm moet optie u de nieuw systeem installeren

![Selecteer de nieuwe installatie HANA](./media/hana-installation/image19_select_new.PNG)

Na deze stap moet u kiezen tussen verschillende extra onderdelen die verder kunnen worden geïnstalleerd met de SAP HANA-databaseserver.

![Aanvullende HANA onderdelen selecteren](./media/hana-installation/image20_select_components.PNG)

Omwille van de in deze documentatie wordt gekozen we de SAP HANA-Client en de SAP HANA-Studio. We ook een scale-up-exemplaar geïnstalleerd. daarom in het volgende scherm moet u kiezen één Host-systeem 

![Selecteer de installatie van de scale-up](./media/hana-installation/image21_single_host.PNG)

In het volgende scherm moet u enkele gegevens opgeven

![Geef SAP HANA-SID](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Als HANA systeem-ID (SID), moet u dezelfde SID, als u Microsoft opgegeven toen u de implementatie van grote exemplaar HANA besteld bieden. Een andere SID is de installatie mislukken vanwege problemen met machtiging toegang op verschillende volumes

Als installatie gedeelde map die u gebruikt de/hana/directory. In de volgende stap moet u de locaties opgeven voor de gegevensbestanden HANA en de logboekbestanden HANA


![Geef de locatie van de HANA-logboek](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> U moet definiëren als gegevens en logboekbestanden de volumes die al bij de koppelpunten die de SID die u hebt gekozen in de selectie van het scherm voordat dit scherm bevatten. Als de beveiligings-id niet met de naam die u hebt getypt overeen in, in het scherm voordat Ga terug en aanpassen van de SID aan de waarde die u op de koppelpunten hebt.

Bekijk de hostnaam en uiteindelijk corrigeren in de volgende stap. 

![De hostnaam controleren](./media/hana-installation/image24_review_host_name.PNG)

In de volgende stap moet u ook gegevens die u naar Microsoft is opgegeven toen u de implementatie van grote exemplaar HANA besteld op te halen. 

![Geef systeemgebruiker UID en GID](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> U moet dezelfde gebruikers-ID van systeem en ID van de groep van de gebruiker opgeven als u Microsoft als u de implementatie eenheid volgorde opgegeven. Als u niet erg dezelfde id's geven, mislukt de installatie van de SAP HANA op de eenheid HANA grote exemplaar.

In de volgende twee schermen we niet zichtbaar zijn in deze documentatie, moet u het wachtwoord opgeven voor de systeemgebruiker van de SAP HANA-database en het wachtwoord voor de gebruiker sapadm, die wordt gebruikt voor de Agent van de SAP-Host die wordt geïnstalleerd als onderdeel van de SAP HANA-datab exemplaar van de as-omgeving.

Na het definiëren van het wachtwoord, wordt een bevestigingsvenster weergegeven. Controleer alle gegevens die worden vermeld en doorgaan met de installatie. U hebt een voortgangsscherm waarin de voortgang van de installatie, zoals in het onderstaande bereikt

![Controleer de voortgang van installatie](./media/hana-installation/image27_show_progress.PNG)

Als de installatie is voltooid, moet u een afbeelding zoals de volgende

![De installatie is voltooid](./media/hana-installation/image28_install_finished.PNG)

Op dit moment moet de SAP HANA-exemplaar actief en werkend en klaar voor gebruik. U moet mogelijk zijn tot stand te brengen vanuit SAP HANA Studio. Controleer ook of u voor de meest recente patches van SAP HANA controleren en deze patches toepassen.
























































 







 





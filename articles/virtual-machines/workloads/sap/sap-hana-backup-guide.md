---
title: Back-handleiding voor SAP HANA op Azure Virtual Machines | Microsoft Docs
description: Back-handleiding voor SAP HANA bevat twee belangrijke back-mogelijkheden voor SAP HANA voor virtuele machines in Azure
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: 9e5b124643b753f404ba6012d3df998f567be59a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="backup-guide-for-sap-hana-on-azure-virtual-machines"></a>Back-uphandleiding voor SAP HANA op Azure Virtual Machines

## <a name="getting-started"></a>Aan de slag

De back-handleiding voor SAP HANA uitgevoerd op virtuele Machines in Azure wordt alleen Azure-specifieke onderwerpen beschreven. Raadpleeg voor algemene SAP HANA back-up verwante items, de SAP HANA-documentatie (Zie _back-documentatie voor SAP HANA_ verderop in dit artikel).

De focus van dit artikel is op twee belangrijke back-mogelijkheden voor SAP HANA op Azure virtuele machines:

- HANA back-up naar het bestandssysteem in een Azure virtuele Linux-Machine (Zie [SAP HANA Azure Backup op bestandsniveau](sap-hana-backup-file-level.md))
- HANA back-up op basis van opslag-momentopnamen met behulp van de functie Azure-opslag-blob momentopname handmatig of Azure Backup-Service (Zie [SAP HANA back-up op basis van opslag-momentopnamen](sap-hana-backup-storage-snapshots.md))

SAP HANA biedt een back-up van de API waarmee back-uphulpprogramma's van derden integreren rechtstreeks met SAP HANA. (Dat is niet binnen het bereik van deze handleiding.) Er is geen directe integratie van SAP HANA met Azure Backup-service beschikbaar is genomen op basis van deze API.

SAP HANA officieel op Azure VM-type GS5 wordt ondersteund als één exemplaar met een extra beperking OLAP-werkbelastingen (Zie [vinden gecertificeerd IaaS Platforms](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html) op de website SAP). In dit artikel wordt bijgewerkt als nieuwe aanbiedingen voor SAP HANA op Azure beschikbaar.

Er is ook een SAP HANA hybride oplossing beschikbaar in Azure, waarbij SAP HANA waarop niet-gevirtualiseerde fysieke servers. Deze back-handleiding voor SAP HANA Azure bestrijkt echter een pure Azure-omgeving waarin SAP HANA wordt uitgevoerd in een virtuele machine van Azure niet SAP HANA waarop &quot;grote exemplaren.&quot; Zie [SAP HANA (grote exemplaren) overzicht en architectuur op Azure](hana-overview-architecture.md) voor meer informatie over deze back-upoplossing op &quot;grote exemplaren&quot; op basis van opslag-momentopnamen.

Algemene informatie over SAP producten die worden ondersteund in Azure vindt u in [SAP-notitie 1928533](https://launchpad.support.sap.com/#/notes/1928533).

De volgende drie cijfers biedt een overzicht van de SAP HANA back-upopties systeemeigen mogelijkheden van Azure op dit moment gebruikt en drie mogelijke toekomstige back-scenario's worden ook weergegeven. Verwante artikelen [SAP HANA Azure Backup op bestandsniveau](sap-hana-backup-file-level.md) en [SAP HANA back-up op basis van opslag-momentopnamen](sap-hana-backup-storage-snapshots.md) worden deze opties in meer detail, met inbegrip van grootte- en Prestatieoverwegingen voor SAP HANA back-ups die multi-terabyte groot zijn beschreven.

![Deze afbeelding ziet u twee mogelijkheden voor het opslaan van de huidige status van de virtuele machine](media/sap-hana-backup-guide/image001.png)

Deze afbeelding ziet u de mogelijkheid om de huidige status van virtuele machine, hetzij via Azure Backup-service of handmatige momentopname van de VM-schijven op te slaan. Met deze benadering, één &#39; t hebben voor het beheren van back-ups SAP HANA. De uitdaging van het scenario voor schijven momentopname is consistentie van het bestandssysteem en de status van een toepassingsconsistente schijf. De consistentiecontrole onderwerp wordt besproken in de sectie _SAP HANA gegevensconsistentie bij het maken van opslag-momentopnamen_ verderop in dit artikel. Mogelijkheden en beperkingen van Azure Backup-service die betrekking hebben op SAP HANA back-ups worden ook besproken verderop in dit artikel.

![Deze afbeelding ziet u opties voor het maken van een SAP HANA bestandsback-up in de virtuele machine](media/sap-hana-backup-guide/image002.png)

In deze afbeelding ziet u opties voor het nemen van een SAP HANA bestandsback-up in de virtuele machine en vervolgens opslaan van het back-upbestanden HANA ergens anders met verschillende hulpprogramma's. Een HANA back-up duurt langer dan een back-oplossing op basis van een momentopname vereist, maar heeft voordelen ten met betrekking tot de integriteit en consistentie. Verderop in dit artikel vindt u meer informatie.

![De volgende afbeelding toont een mogelijke toekomstige SAP HANA back-scenario](media/sap-hana-backup-guide/image003.png)

De volgende afbeelding toont een mogelijke toekomstige SAP HANA back-scenario. Als SAP HANA toegestaan voor het maken van back-ups van een secundaire replicatie, zou deze extra opties voor back-strategieën toevoegen. Het is momenteel niet mogelijk volgens een bericht in de SAP HANA-Wiki:

_&quot;Is het mogelijk is het nemen van back-ups op de secundaire kant?_

_Nee, momenteel kunnen alleen gegevens en logboekback-ups op de primaire kant. Als automatische back-up is ingeschakeld, na overname secundaire naast de logboekback-ups automatisch er geschreven.&quot;_

## <a name="sap-resources-for-hana-backup"></a>SAP-resources voor HANA back-up

### <a name="sap-hana-backup-documentation"></a>Back-documentatie voor SAP HANA

- [Inleiding tot SAP HANA-beheer](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)
- [Uw back-up en herstelstrategie plannen](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)
- [HANA back-up met behulp van ABAP DBACOCKPIT plannen](http://www.hanatutorials.com/p/schedule-hana-backup-using-abap.html)
- [Planning gegevensback-ups (SAP HANA Cockpit)](http://help.sap.com/saphelp_hanaplatform/helpdata/en/6d/385fa14ef64a6bab2c97a3d3e40292/frameset.htm)
- Veelgestelde vragen over SAP HANA back-up [SAP-notitie 1642148](https://launchpad.support.sap.com/#/notes/1642148)
- Veelgestelde vragen over SAP HANA-database en -opslag-momentopnamen in [SAP-notitie 2039883](https://launchpad.support.sap.com/#/notes/2039883)
- Bestandssystemen ongeschikt netwerk voor back-up en herstel in [SAP-notitie 1820529](https://launchpad.support.sap.com/#/notes/1820529)

### <a name="why-sap-hana-backup"></a>Waarom SAP HANA back-up?

Azure storage biedt beschikbaarheid en betrouwbaarheid gebruiksklaar (Zie [Inleiding tot Microsoft Azure Storage](../../../storage/common/storage-introduction.md) voor meer informatie over Azure storage).

Het minimum voor &quot;back-&quot; te vertrouwen op de Azure Sla's, de bestanden voor SAP HANA gegevens en logboekbestanden op Azure virtuele harde schijven die zijn gekoppeld aan de server voor SAP HANA VM is. Deze aanpak bevat informatie over de mislukte VM, maar niet mogelijke beschadiging van de SAP HANA-gegevens en logboekbestanden of logische fouten, zoals het verwijderen van gegevens of bestanden per ongeluk. Back-ups zijn ook vereist voor naleving of wettelijke redenen. Kortom, is altijd een nodig voor SAP HANA back-ups.

### <a name="how-to-verify-correctness-of-sap-hana-backup"></a>Controleren of de juistheid van SAP HANA back-up
Wanneer u opslag-momentopnamen, wordt een terugzetbewerking test uitgevoerd op een andere site aanbevolen. Deze aanpak biedt een manier om ervoor te zorgen dat een back-up correct en interne processen voor back-up en herstel van werken zoals verwacht. Hoewel dit een aanzienlijke drempel on-premises is het veel eenvoudiger om uit te voeren in de cloud door te geven van de benodigde resources tijdelijk voor dit doel.

Houd er rekening mee die tijdens het doorzoeken van een eenvoudig terugzetten en controle of HANA actief is en wordt uitgevoerd, is niet voldoende. In het ideale geval moet een een consistentiecontrole voor de tabel om er zeker van te zijn dat de herstelde database goed is uitgevoerd. SAP HANA biedt verschillende soorten consistentiecontroles beschreven in [SAP-notitie 1977584](https://launchpad.support.sap.com/#/notes/1977584).

Informatie over de consistentiecontrole van de tabel kan worden gevonden op de SAP-website op [tabel en consistentiecontroles catalogus](http://help.sap.com/saphelp_hanaplatform/helpdata/en/25/84ec2e324d44529edc8221956359ea/content.htm#loio9357bf52c7324bee9567dca417ad9f8b).

Voor standaardbestandback-ups is een terugzetbewerking test niet nodig. Er zijn twee SAP HANA-hulpprogramma's die u helpen te controleren welke back-up kan worden gebruikt om te herstellen: hdbbackupdiag en hdbbackupcheck. Zie [handmatig controleren of een herstel is mogelijk](https://help.sap.com/saphelp_hanaplatform/helpdata/en/77/522ef1e3cb4d799bab33e0aeb9c93b/content.htm) voor meer informatie over deze hulpprogramma's.

### <a name="pros-and-cons-of-hana-backup-versus-storage-snapshot"></a>Voordelen en nadelen van HANA back-up versus opslag momentopname

SAP bestaat &#39; t geven de voorkeur aan beide HANA back-up en opslag-momentopnamen. Geeft de voor- en nadelen, zodat een kan bepalen die wordt gebruikt, afhankelijk van de situatie en de beschikbare opslagruimte-technologie (Zie [plannen van uw back-up- en herstelstrategie](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)).

Op Azure, houd rekening met het feit dat de blob van Azure-functie &#39;Snapshot; t garanderen van de consistentie van het bestandssysteem (Zie [Using blob momentopnamen met PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)). De volgende sectie _SAP HANA gegevensconsistentie bij het maken van opslag-momentopnamen_, enkele overwegingen met betrekking tot deze functie wordt besproken.

Bovendien een heeft de facturering implicaties begrijpt vaak werken met blobs momentopnamen, zoals beschreven in dit artikel: [begrijpen hoe momentopnamen doorlopen kosten](/rest/api/storageservices/understanding-how-snapshots-accrue-charges): het &#39; zo duidelijk als het gebruik van Azure virtuele t schijven.

### <a name="sap-hana-data-consistency-when-taking-storage-snapshots"></a>SAP HANA gegevensconsistentie bij het maken van opslag-momentopnamen

Systeem- en consistentie van bestand is een complex probleem bij het maken van opslag-momentopnamen. De eenvoudigste manier om problemen te voorkomen dat kan worden afgesloten SAP HANA en mogelijk zelfs de hele virtuele machine. Afsluiten mogelijk goed met een demo of prototype of zelfs een ontwikkelsysteem, maar is geen optie voor een productiesysteem.

In Azure, een heeft Houd er rekening mee dat de blob van Azure-functie &#39;Snapshot; t garantie consistentie van het bestandssysteem. Dit werkt prima echter met behulp van de SAP HANA-functie momentopname, zolang er slechts één virtuele schijf die betrokken zijn is. Maar zelfs met een enkele schijf extra items moeten worden gecontroleerd. [SAP-notitie 2039883](https://launchpad.support.sap.com/#/notes/2039883) belangrijke informatie over SAP HANA back-ups via opslag-momentopnamen heeft. Bijvoorbeeld, noemt dat met het bestandssysteem XFS uitgevoerd is om **xfs\_blokkeren** voordat u een momentopname van de opslag om te consistentie te garanderen (Zie [xfs\_freeze(8) - Linux man pagina ](https://linux.die.net/man/8/xfs_freeze) voor meer informatie over **xfs\_blokkeren**).

Het onderwerp van de consistentie wordt nog meer lastig in geval waarbij een enkel bestandssysteem verspreid over meerdere schijven/volumes. Bijvoorbeeld, met behulp mdadm of LVM en te verwijderen. De SAP-notitie bovengenoemde statussen:

_&quot;Maar houd er rekening mee dat het opslagsysteem heeft met het i/o-consistentie te garanderen tijdens het maken van een momentopname van de opslag per SAP HANA-gegevensvolume, dat wil zeggen vastleggen van een SAP HANA servicespecifieke gegevensvolume moet een atomic-bewerking.&quot;_

Ervan uitgaande dat er een XFS bestandssysteem spanning vier Azure virtuele schijven, bevatten de volgende stappen een consistente momentopname te maken die het gegevensgebied HANA vertegenwoordigt:

- HANA momentopname voorbereiden
- Het bestandssysteem blokkeren (bijvoorbeeld gebruiken **xfs\_blokkeren**)
- Alle momentopnamen van de benodigde blob in Azure maken
- Blokkering opheffen, zodat het bestandssysteem
- De momentopname HANA bevestigen

U wordt aangeraden de bovenstaande procedure gebruiken in alle gevallen moet voor de zekerheid, ongeacht welk bestandssysteem. Of als één schijf of striping via mdadm of LVM over meerdere schijven.

Het is belangrijk om te bevestigen dat de momentopname HANA. Vanwege de &quot;kopiëren bij schrijven&quot; SAP HANA mogelijk geen extra schijfruimte, terwijl in deze modus momentopname voorbereiden. &#39; ook niet mogelijk is worden nieuwe back-ups gestart totdat de SAP HANA-momentopname is bevestigd.

Azure Backup-service gebruikt Azure VM-extensies voor het behandelen van de consistentie van het bestandssysteem. Deze VM-uitbreidingen zijn niet beschikbaar voor zelfstandig gebruik. Een heeft nog steeds SAP HANA consistentie te beheren. Zie het artikel gerelateerde [SAP HANA Azure back-up op bestandsniveau](sap-hana-backup-file-level.md) voor meer informatie.

### <a name="sap-hana-backup-scheduling-strategy"></a>SAP HANA-strategie voor back-up plannen

Het artikel SAP HANA [plannen van uw back-up- en herstelstrategie](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) statussen van een basisniveau om back-ups:

- Opslag-momentopnamen (dagelijks)
- Voltooid de back-up met behulp van bestands- of bacint-indeling (één keer per week)
- Automatische logboekback-ups

U kunt desgewenst kan een volledig zonder opslag-momentopnamen; gaan ze kunnen worden vervangen door HANA delta back-ups, zoals incrementele of differentiële back-ups (Zie [Delta back-ups](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm)).

De beheerdershandleiding voor HANA biedt een lijst met voorbeelden. Dit wijst erop dat een SAP HANA herstellen naar een bepaald punt in tijd met behulp van de volgende volgorde van de back-ups:

1. Volledige back-up
2. Differentiële back-up
3. Incrementele back-up 1
4. Incrementele back-up 2
5. Logboekback-ups

Met betrekking tot een exacte planning wanneer en hoe vaak een specifieke back-uptype moet gebeuren, het is niet mogelijk om te geven van een algemene richtlijn: het bestand is te klantspecifieke en is afhankelijk van hoeveel gegevenswijzigingen in het systeem optreden. Een basic aanbeveling SAP-kant kunnen worden gezien als algemene richtlijnen, is het maken een volledige HANA back-up eenmaal per week.
Met betrekking tot logboekback-ups, Zie de documentatie van de SAP HANA [logboekback-ups](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm).

SAP wordt ook aangeraden tijdens het doorzoeken van sommige housekeeping van de back-upcatalogus om te voorkomen dat groeiende oneindig (Zie [huishoudelijke voor back-upcatalogus en back-upopslag](http://help.sap.com/saphelp_hanaplatform/helpdata/en/ca/c903c28b0e4301b39814ef41dbf568/content.htm)).

### <a name="sap-hana-configuration-files"></a>SAP HANA-configuratiebestanden

Zoals vermeld in de veelgestelde vragen in [SAP-notitie 1642148](https://launchpad.support.sap.com/#/notes/1642148), de configuratiebestanden voor SAP HANA maken geen deel uit van een standaard HANA back-up. Ze zijn niet essentieel is voor het herstellen van een systeem. De configuratie van de HANA kan handmatig worden gewijzigd na het terugzetten. Als een wil ontvangen tijdens het herstelproces dezelfde aangepaste configuratie, is het nodig om back-up van de configuratiebestanden HANA afzonderlijk.

Als standaard HANA back-ups eigen HANA back-upbestand systeem gaat zijn, kan een ook de configuratiebestanden kopiëren naar het bestandssysteem voor dezelfde back-up en vervolgens kopieert u alles samen naar de uiteindelijke opslaglocatie zoals cool storage-blob.

### <a name="sap-hana-cockpit"></a>SAP HANA Cockpit

SAP HANA Cockpit biedt de mogelijkheid van controleren en beheren van SAP HANA via een browser. Het kan ook de verwerking van SAP HANA back-ups en daarom kan worden gebruikt als een alternatief voor SAP HANA Studio en ABAP DBACOCKPIT (Zie [SAP HANA Cockpit](https://help.sap.com/saphelp_hanaplatform/helpdata/en/73/c37822444344f3973e0e976b77958e/content.htm) voor meer informatie).

![Deze afbeelding ziet u het scherm SAP HANA Cockpit Database beheer](media/sap-hana-backup-guide/image004.png)

Deze afbeelding ziet u het scherm SAP HANA Cockpit Database beheer en de back-tegel aan de linkerkant. De back-tegel ziet juiste gebruikersmachtigingen nodig voor aanmeldingsaccount.

![Back-ups kunnen worden bewaakt in SAP HANA Cockpit zolang deze lopende](media/sap-hana-backup-guide/image005.png)

Back-ups kunnen worden bewaakt in SAP HANA Cockpit terwijl ze voortdurend zijn en zodra dit is voltooid, de back-details beschikbaar zijn.

![Een voorbeeld van Firefox op een Azure-VM SLES 12 met Gnome bureaublad](media/sap-hana-backup-guide/image006.png)

De vorige schermafbeeldingen zijn uitgevoerd vanaf een Windows Azure VM. Dit is een voorbeeld Firefox gebruiken op een Azure-VM SLES 12 met Gnome bureaublad. De optie voor het definiëren van de back-upschema's SAP HANA in SAP HANA Cockpit worden weergegeven. Zoals ook zien kunnen, maar er wordt verwezen datum/tijd als een voorvoegsel voor de back-upbestanden. SAP HANA Studio de standaardvoorvoegsel is &quot;COMPLETE\_gegevens\_back-&quot; bij het uitvoeren van een volledige back-up. Gebruik een uniek voorvoegsel wordt aanbevolen.

### <a name="sap-hana-backup-encryption"></a>Back-up SAP HANA-versleuteling

SAP HANA biedt versleuteling van gegevens en logboekbestanden. Als voor SAP HANA-gegevens en logboekbestanden worden niet versleuteld, zijn klikt u vervolgens de back-ups ook niet versleuteld. Het is tot de klant een vorm van de oplossing van derden gebruiken voor het versleutelen van de SAP HANA back-ups. Zie [gegevens en versleuteling voor volumes van logboek](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/01f36fbb5710148b668201a6e95cf2/content.htm) voor meer informatie over SAP HANA-codering.

In Microsoft Azure kan een klant gebruiken IaaS VM versleuteling te versleutelen. Zo kan een speciale gegevensschijven gekoppeld aan de virtuele machine, die worden gebruikt voor SAP HANA back-ups opslaan en vervolgens kopieën te maken van deze schijven gebruiken.

Azure Backup-service kan versleutelde VM's / schijven verwerken (Zie [How to back up en herstel van virtuele machines met Azure Backup versleuteld](../../../backup/backup-azure-vms-encryption.md)).

Een andere optie is de SAP HANA-virtuele machine en de schijven zonder versleuteling onderhouden, en de back-upbestanden voor SAP HANA opslaan in een opslagaccount waarvoor versleuteling is ingeschakeld (Zie [Azure Storage Service: versleuteling van gegevens in rust](../../../storage/common/storage-service-encryption.md)).

## <a name="test-setup"></a>Test setup

### <a name="test-virtual-machine-on-azure"></a>Test-virtuele Machine in Azure

Een SAP HANA-installatie in een virtuele machine van Azure GS5 is gebruikt voor de volgende back-up/herstel testen.

![Deze afbeelding ziet u deel van het overzicht van Azure portal voor de test HANA VM](media/sap-hana-backup-guide/image007.png)

In deze afbeelding ziet deel van het overzicht van Azure portal voor de test HANA VM.

### <a name="test-backup-size"></a>Back-grootte testen

![In deze afbeelding is overgenomen van de back-console in HANA Studio en de grootte van de back-upbestand van 229 GB voor de server van de index HANA toont](media/sap-hana-backup-guide/image008.png)

Een dummy-tabel is gevuld met gegevens een back-grootte van de totale gegevens van meer dan 200 GB worden afgeleid van realistische prestatiegegevens ophalen. De afbeelding is overgenomen van de back-console in Studio HANA en geeft de omvang van de back-upbestand van 229 GB voor de server van de index HANA. Bij de tests voor is de back-up standaardvoorvoegsel 'COMPLETE_DATA_BACKUP' in SAP HANA Studio gebruikt. In de echte productiesystemen, moet een nuttiger voorvoegsel worden gedefinieerd. SAP HANA Cockpit stelt datum/tijd.

### <a name="test-tool-to-copy-files-directly-to-azure-storage"></a>Testen van hulpprogramma voor kopiëren van bestanden rechtstreeks naar de Azure-opslag

Om te zetten back-upbestanden voor SAP HANA rechtstreeks naar Azure blob-opslag of Azure-bestandsshares, is het hulpprogramma blobxfer omdat deze ondersteuning biedt voor beide doelen en kunnen eenvoudig worden geïntegreerd in automatiseringsscripts vanwege de opdrachtregelinterface gebruikt. Het hulpprogramma blobxfer is beschikbaar op [GitHub](https://github.com/Azure/blobxfer).

### <a name="test-backup-size-estimation"></a>De grootte van de back-schatting testen

Het is belangrijk om te schatten van de back-grootte van SAP HANA. Deze schatting helpt de prestaties verbeteren door het definiëren van de back-maximale bestandsgrootte voor een aantal back-upbestanden of als gevolg van parallelle uitvoering tijdens het kopiëren van een bestand. (Deze informatie worden verderop in dit artikel uitgelegd.) Een moet ook bepalen of een volledige back-up of een delta-back-up (incrementele of differentiële).

Gelukkig is er een eenvoudige SQL-instructie die u maakt een schatting van de grootte van de back-upbestanden: **Selecteer \* van M\_back-\_grootte\_SCHATTINGEN** (Zie [schatting de benodigde ruimte in het bestandssysteem voor een gegevensback-up](https://help.sap.com/saphelp_hanaplatform/helpdata/en/7d/46337b7a9c4c708d965b65bc0f343c/content.htm)).

![De uitvoer van deze SQL-instructie komt overeen met bijna hetzelfde de werkelijke grootte van de volledige gegevens back-up op schijf](media/sap-hana-backup-guide/image009.png)

De uitvoer van deze SQL-instructie voor het testsysteem komt overeen met bijna hetzelfde de werkelijke grootte van de volledige gegevens back-up op schijf.

### <a name="test-hana-backup-file-size"></a>Grootte van back-upbestand HANA testen

![De back-HANA Studio-console kan één tot de maximale bestandsgrootte van de back-upbestanden HANA beperken](media/sap-hana-backup-guide/image010.png)

De back-console HANA Studio kunt een voor de maximale bestandsgrootte van de back-upbestanden HANA beperken. In de voorbeeldomgeving maakt die functie het mogelijk dat meerdere kleinere back-bestanden in plaats van een back-upbestand van 230 GB. Kleinere bestanden heeft een aanzienlijke invloed op prestaties (Zie het artikel gerelateerde [SAP HANA Azure back-up op bestandsniveau](sap-hana-backup-file-level.md)).

## <a name="summary"></a>Samenvatting

Op basis van de resultaten van die de volgende tabellen tonen de voordelen en nadelen van oplossingen voor back-up van een SAP HANA-database op Azure virtuele machines.

**Back-up SAP HANA naar het bestandssysteem en daarna back-upbestanden naar de laatste back-updoelmap kopiëren**

|Oplossing                                           |Professionals                                 |Nadelen                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Behoudt HANA back-ups op VM-schijven                      |Er zijn geen extra risicobeheerinspanningen     |Lokale VM schijfruimte vrij eats           |
|Blobxfer hulpprogramma Back-upbestanden naar blob storage kopiëren |Parallelle uitvoering om meerdere bestanden te kopiëren keuze cool blob storage gebruiken | Extra hulpprogramma onderhoud en aangepaste scripts | 
|BLOB kopiëren via Powershell of CLI                    |Geen aanvullende hulpprogramma nodig kan worden geconfigureerd via Azure Powershell of CLI |handmatig proces klant heeft tot het zorgt voor het uitvoeren van scripts en het beheer van de gekopieerde blobs voor terugzetten|
|Kopiëren naar de NFS-share                                  |Na de verwerking van back-upbestanden op andere virtuele machine zonder gevolgen voor de server HANA|Trage kopieerproces|
|Blobxfer kopiëren naar Azure File-Service                |Geen eat ruimte op de lokale VM-schijven|Er zijn geen directe ondersteuning door back-up, de grootte van beperking van de bestandsshare dat zich momenteel in 5 TB HANA schrijven|
|Azure Backup-Agent                                 | Zou worden aanbevolen oplossing         | Momenteel niet beschikbaar op Linux    |



**Back-up SAP HANA op basis van opslag-momentopnamen**

|Oplossing                                           |Professionals                                 |Nadelen                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Azure Backup-Service                               | Kan back-up van de virtuele machine op basis van de blob-momentopnamen | Wanneer u geen herstelbewerkingen op bestand, het maken van een nieuwe virtuele machine is vereist voor het herstelproces, die vervolgens de noodzaak van een nieuwe sleutel voor SAP HANA-licentie impliceert|
|Handmatige blob momentopnamen                              | Flexibiliteit voor het maken en herstellen van specifieke VM-schijven zonder de unieke ID van de virtuele machine wijzigen|Alle handmatig werk die moet worden uitgevoerd door de klant|

## <a name="next-steps"></a>Volgende stappen
* [SAP HANA Azure Backup op bestandsniveau](sap-hana-backup-file-level.md) wordt de back-optie op basis van bestanden beschreven.
* [SAP HANA back-up op basis van opslag-momentopnamen](sap-hana-backup-storage-snapshots.md) opslag op basis van een momentopname back-up wordt de optie beschreven.
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen in Azure (grote exemplaren), [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md).

---
title: Back-uphandleiding voor SAP HANA op Azure Virtual Machines | Microsoft Docs
description: Back-uphandleiding voor SAP HANA biedt twee belangrijke back-mogelijkheden voor SAP HANA op Azure virtual machines
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: rclaus
ms.openlocfilehash: 89896fab7b1c359007ed23d4f9d9771e366ca68a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58013343"
---
# <a name="backup-guide-for-sap-hana-on-azure-virtual-machines"></a>Back-uphandleiding voor SAP HANA op Azure Virtual Machines

## <a name="getting-started"></a>Aan de slag

De back-uphandleiding voor SAP HANA op Azure virtuele Machines worden alleen Azure-specifieke onderwerpen beschreven. Raadpleeg voor algemene SAP HANA back-up verwante items, de SAP HANA-documentatie (Zie _documentatie over backup van SAP HANA_ verderop in dit artikel).

De focus van dit artikel is op twee belangrijke back-mogelijkheden voor SAP HANA op Azure virtual machines:

- HANA back-up naar het bestandssysteem in een Azure Linux-Machine (Zie [SAP HANA Azure back-up op bestandsniveau](sap-hana-backup-file-level.md))
- HANA back-up op basis van opslagmomentopnamen handmatig met behulp van de functie van Azure storage blob snapshot of Azure Backup-Service (Zie [back-up van SAP HANA op basis van opslagmomentopnamen](sap-hana-backup-storage-snapshots.md))

SAP HANA biedt een back-up van de API waarmee back-uphulpprogramma's van derden integreren rechtstreeks met SAP HANA. (Dat is niet binnen het bereik van deze handleiding.) Er is geen rechtstreekse integratie van SAP HANA met Azure Backup-service beschikbaar is nu op basis van deze API.

SAP HANA wordt officieel ondersteund op verschillende typen Azure VM's, zoals Azure M-serie. Voor een volledige lijst van SAP HANA Azure Virtual machines gecertificeerde, Bekijk [gecertificeerde IaaS-platformen vinden](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). In dit artikel wordt bijgewerkt als nieuwe aanbiedingen voor SAP HANA op Azure beschikbaar.

Er is ook een hybride oplossing voor SAP HANA beschikbaar op Azure, waarbij SAP HANA op wordt uitgevoerd niet-gevirtualiseerde fysieke servers. Deze back-handleiding voor SAP HANA Azure bestrijkt echter een zuivere Azure-omgeving waarin de SAP HANA wordt uitgevoerd in een Azure-VM niet SAP HANA op &quot;grote instanties.&quot; Zie [SAP HANA (grote instanties) overzicht en architectuur op Azure](hana-overview-architecture.md) voor meer informatie over deze back-upoplossing op &quot;grote instanties&quot; op basis van opslagmomentopnamen.

Algemene informatie over SAP-producten ondersteund op Azure kan worden gevonden [SAP-notitie 1928533](https://launchpad.support.sap.com/#/notes/1928533).

De volgende drie cijfers biedt een overzicht van de back-upopties van SAP HANA met behulp van systeemeigen Azure-mogelijkheden op dit moment, en ook drie mogelijke toekomstige back-scenario's worden weergegeven. De gerelateerde artikelen [SAP HANA Azure back-up op bestandsniveau](sap-hana-backup-file-level.md) en [back-up van SAP HANA op basis van opslagmomentopnamen](sap-hana-backup-storage-snapshots.md) beschrijven deze opties in meer detail, met inbegrip van grootte- en Prestatieoverwegingen voor SAP HANA back-ups die multi-terabytes groot zijn.

![In deze afbeelding ziet u twee mogelijkheden voor het opslaan van de huidige status van de virtuele machine](media/sap-hana-backup-guide/image001.png)

In deze afbeelding ziet u de mogelijkheid om de huidige status van virtuele machine, hetzij via Azure Backup-service of handmatige momentopname van VM-schijven op te slaan. Met deze methode heeft één&#39;t gemachtigd voor het beheren van back-ups van SAP HANA. De uitdaging van het scenario van de momentopname schijf is consistentie van bestandssysteem en de status van een App-consistente schijf. De consistentie-onderwerp wordt beschreven in de sectie _SAP HANA-gegevensconsistentie bij het nemen van opslagmomentopnamen_ verderop in dit artikel. Mogelijkheden en beperkingen van Azure Backup-service met betrekking tot back-ups van SAP HANA worden ook verderop in dit artikel besproken.

![In deze afbeelding ziet u opties voor het maken van een SAP HANA bestandsback-up binnen de virtuele machine](media/sap-hana-backup-guide/image002.png)

In deze afbeelding ziet u opties voor het nemen van een SAP HANA-back-up binnen de virtuele machine en vervolgens opslaan van het back-upbestanden HANA ergens anders met verschillende hulpprogramma's. Een HANA back-up duurt langer dan een momentopname op basis van back-upoplossing vereist, maar het heeft namelijk voordelen met betrekking tot de integriteit en consistentie. Meer informatie vindt u verderop in dit artikel.

![In deze afbeelding toont een mogelijke toekomstige SAP HANA back-scenario](media/sap-hana-backup-guide/image003.png)

In deze afbeelding toont een mogelijke toekomstige SAP HANA back-scenario. Als de SAP HANA toegestaan nemen van back-ups van een secundaire replicatie, zou deze extra opties voor back-upstrategieën toevoegen. Het is momenteel niet mogelijk op basis van een bericht in de SAP HANA-Wiki:

_&quot;Is het mogelijk om back-ups uitvoeren op de secundaire kant?_

_Nee, momenteel kunt u alleen ondernemen gegevens en logboekback-ups op de primaire zijde. Als na overname naar de secundaire zijde, automatische back-up is ingeschakeld, wordt de logboekback-ups automatisch er worden geschreven.&quot;_

## <a name="sap-resources-for-hana-backup"></a>SAP-resources voor HANA back-up

### <a name="sap-hana-backup-documentation"></a>Documentatie over backup van SAP HANA

- [Inleiding tot beheer van SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)
- [Plannen van de back-up en de strategie voor herstel](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)
- [HANA-back-up met behulp van ABAP DBACOCKPIT plannen](https://www.hanatutorials.com/p/schedule-hana-backup-using-abap.html)
- [Planning gegevensback-ups (SAP HANA Cockpit)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/6d/385fa14ef64a6bab2c97a3d3e40292/frameset.htm)
- Veelgestelde vragen over SAP HANA back-up [1642148 voor SAP-notitie](https://launchpad.support.sap.com/#/notes/1642148)
- Veelgestelde vragen over SAP HANA-database en storage-momentopnamen in [2039883 voor SAP-notitie](https://launchpad.support.sap.com/#/notes/2039883)
- Ongeschikte netwerk bestandssystemen voor back-up en herstel in [1820529 voor SAP-notitie](https://launchpad.support.sap.com/#/notes/1820529)

### <a name="why-sap-hana-backup"></a>Waarom SAP HANA back-up?

Azure storage biedt beschikbaarheid en betrouwbaarheid gebruiksklaar (Zie [Inleiding tot Microsoft Azure Storage](../../../storage/common/storage-introduction.md) voor meer informatie over Azure storage).

Het minimale aantal voor &quot;back-up&quot; te vertrouwen op de SLA's voor Azure, de SAP HANA-gegevens en logboekbestanden bestanden op Azure-VHD's die zijn gekoppeld aan de virtuele machine van de SAP HANA-server is. Deze methode heeft betrekking op VM-fouten, maar niet mogelijke beschadiging van de SAP HANA-gegevens en logboekbestanden of logische fouten, zoals gegevens of bestanden per ongeluk verwijderen. Back-ups zijn ook vereist voor naleving of om wettelijke redenen. Kort gezegd, is het altijd een nodig voor back-ups van SAP HANA.

### <a name="how-to-verify-correctness-of-sap-hana-backup"></a>Juistheid van SAP HANA back-up controleren
Wanneer u opslagmomentopnamen, wordt die wordt uitgevoerd een test voor het terugzetten op een ander systeem aanbevolen. Deze benadering biedt een manier om te zorgen dat een back-up correct en interne processen voor back-up en herstellen werken zoals verwacht. Dit is een aanzienlijke drempel on-premises is het veel eenvoudiger om uit te voeren in de cloud door te geven van de benodigde resources tijdelijk voor dit doel.

Houden er rekening mee dat doen een eenvoudig herstellen en controleren of HANA actief is en het uitvoeren is niet voldoende. In het ideale geval moet een een consistentiecontrole voor de tabel om er zeker van te zijn dat de herstelde database goed is uitgevoerd. SAP HANA biedt verschillende soorten consistentiecontroles wordt beschreven in [SAP-notitie 1977584](https://launchpad.support.sap.com/#/notes/1977584).

Informatie over de consistentiecontrole van de tabel kan ook worden gevonden op de website van de SAP op [tabel en consistentiecontroles catalogus](https://help.sap.com/saphelp_hanaplatform/helpdata/en/25/84ec2e324d44529edc8221956359ea/content.htm#loio9357bf52c7324bee9567dca417ad9f8b).

Voor back-ups van standard-is test terugzetten niet nodig. Er zijn twee SAP HANA-hulpprogramma's die helpen om te controleren welke back-up kan worden gebruikt voor herstel: hdbbackupdiag en hdbbackupcheck. Zie [handmatig controleren of een herstel is mogelijk](https://help.sap.com/saphelp_hanaplatform/helpdata/en/77/522ef1e3cb4d799bab33e0aeb9c93b/content.htm) voor meer informatie over deze hulpprogramma's.

### <a name="pros-and-cons-of-hana-backup-versus-storage-snapshot"></a>Voordelen en nadelen van HANA back-up ten opzichte van de storage-momentopname

SAP heeft&#39;t de voorkeur geeft aan een van beide HANA back-up ten opzichte van de momentopname van de opslag. Geeft de voor- en nadelen, zodat een welke bepalen kunt gebruiken afhankelijk van de situatie en de beschikbare opslag-technologie (Zie [plannen van uw back-up en strategie voor herstel](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)).

In Azure, worden op de hoogte van het feit dat de Azure-blob-functie Snapshot&#39;t garantie consistentie van bestandssysteem (Zie [met blob-momentopnamen met PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)). De volgende sectie, _SAP HANA-gegevensconsistentie bij het nemen van opslagmomentopnamen_, enkele overwegingen met betrekking tot deze functie wordt besproken.

Bovendien heeft een inzicht in de facturering gevolgen wanneer vaak werken met blob-momentopnamen zoals beschreven in dit artikel: [Inzicht in hoe momentopnamen toerekenen kosten](/rest/api/storageservices/understanding-how-snapshots-accrue-charges): het is&#39;t als voor de hand liggende als Azure virtuele schijven.

### <a name="sap-hana-data-consistency-when-taking-storage-snapshots"></a>SAP HANA-gegevensconsistentie bij het nemen van opslagmomentopnamen

Systeem- en bestandsconsistentie is een complex probleem bij het nemen van storage-momentopnamen. De eenvoudigste manier om problemen te vermijden zou afsluiten SAP HANA, of misschien zelfs de hele virtuele machine zijn. Het is mogelijk dat een afsluiten goed met een demo of prototype of zelfs een systeem voor de ontwikkeling, maar deze kan niet worden gebruikt voor een productiesysteem.

In Azure, een Houd er rekening mee dat de Azure-blob-functie Snapshot heeft&#39;t garantie consistentie van bestandssysteem. Werkt prima echter met behulp van de SAP HANA-functie, snapshot, zolang er slechts één virtuele schijf die betrokken zijn is. Maar zelfs met een enkele schijf extra items moeten worden gecontroleerd. [SAP-notitie 2039883](https://launchpad.support.sap.com/#/notes/2039883) bevat belangrijke informatie over SAP HANA back-ups via opslagmomentopnamen. Bijvoorbeeld, genoemd, met het bestandssysteem XFS is nodig om uit te voeren **xfs\_blokkeren** voordat u begint met een momentopname van een opslagruimte om te consistentie te garanderen (Zie [xfs\_freeze(8) - pagina voor Linux-man ](https://linux.die.net/man/8/xfs_freeze) voor meer informatie over **xfs\_blokkeren**).

Het onderwerp van de consistentie wordt het nog moeilijker in een aanvraag waarin een systeem met één bestand meerdere schijven/volumes omvat. Bijvoorbeeld, met behulp van mdadm of LVM en striping. De SAP-notitie bovengenoemde statussen:

_&quot;Maar houd er rekening mee dat het opslagsysteem heeft om de i/o-consistentie te garanderen tijdens het maken van een momentopname van een opslag per gegevensvolume van SAP HANA, dat wil zeggen vastleggen van een volume met SAP HANA servicespecifieke gegevens moet een atomische bewerking.&quot;_

Ervan uitgaande dat er is een XFS bestandssysteem spanning vier Azure virtuele schijven, bevatten de volgende stappen een consistente momentopname te maken die staat voor het gegevensgebied HANA:

- HANA-momentopname voorbereiden
- Het bestandssysteem stilzetten (Gebruik bijvoorbeeld **xfs\_blokkeren**)
- Alle benodigde blob-momentopnamen maken op Azure
- Blokkering van het bestandssysteem
- Controleer of de HANA-momentopname

De aanbeveling is het gebruik van de bovenstaande procedure in alle gevallen moet aan de veilige, ongeacht welke bestandssysteem. Of als het één schijf of striping via mdadm of LVM over meerdere schijven.

Het is belangrijk om te bevestigen van de HANA-momentopname. Vanwege de &quot;kopiëren bij schrijven&quot; SAP HANA mogelijk niet meer schijfruimte vrij terwijl in deze modus momentopname voorbereiden. Deze&#39;ook niet mogelijk is om te starten van nieuwe back-ups totdat de SAP HANA-momentopname is bevestigd.

Azure Backup-service maakt gebruik van Azure VM-extensies voorzichtig zijn van de consistentie van bestandssysteem. Deze VM-extensies zijn niet beschikbaar voor zelfstandig gebruik. Een heeft nog steeds voor het beheren van SAP HANA-consistentie. Zie het bijbehorende artikel [SAP HANA Azure back-up op bestandsniveau](sap-hana-backup-file-level.md) voor meer informatie.

### <a name="sap-hana-backup-scheduling-strategy"></a>SAP HANA-strategie voor back-up plannen

De SAP HANA-artikel [plannen van uw back-up en strategie voor herstel](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) statussen van een basic-abonnement om back-ups:

- Storage-momentopnamen (dagelijks)
- Volledige back-up met behulp van bestands- of bacint indeling (één keer per week)
- Automatische logboekback-ups

(Optioneel) kan een volledig zonder opslagmomentopnamen; gaan ze kunnen worden vervangen door HANA delta-back-ups, zoals incrementele of differentiële back-ups (Zie [Delta-back-ups](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm)).

Gebruikershandleiding voor de HANA biedt een lijst met voorbeelden. Dit kan erop wijzen dat een SAP HANA naar een bepaald punt in tijd met behulp van de volgende volgorde van de back-ups herstellen:

1. Volledige back-up
2. Differentiële back-up
3. Incrementele back-up 1
4. Incrementele back-up 2
5. Logboekback-ups

Met betrekking tot een exacte plannen wanneer en hoe vaak een bepaald type back-up moet gebeuren, is het niet mogelijk om te geven van een algemene richtlijn — te klantspecifieke is en is afhankelijk van hoeveel gegevenswijzigingen in het systeem optreden. Een algemene aanbeveling van SAP-zijde, die kan worden gezien als algemene richtlijnen, is het maken een volledige HANA back-up eenmaal per week.
Met betrekking tot logboekback-ups, Zie de documentatie van SAP HANA [logboekback-ups](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm).

SAP wordt ook aangeraden voeren sommige housekeeping van de back-catalogus om te voorkomen dat deze groeiende oneindig (Zie [huishoudelijke voor back-catalogus en back-upopslag](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ca/c903c28b0e4301b39814ef41dbf568/content.htm)).

### <a name="sap-hana-configuration-files"></a>SAP HANA-configuratiebestanden

Zoals vermeld in de veelgestelde vragen in [SAP-notitie 1642148](https://launchpad.support.sap.com/#/notes/1642148), de SAP HANA-configuratiebestanden maken geen deel uit van een standard HANA back-up. Ze zijn niet essentieel is voor het herstellen van een systeem. De configuratie van de HANA kan handmatig worden gewijzigd na het herstel. In het geval één ophalen van dezelfde aangepaste configuratie tijdens het herstelproces wilt, is het nodig om back-up van de configuratiebestanden HANA afzonderlijk.

Als een systeem voor back-upbestand van toegewezen HANA standaard HANA back-ups gaat, kan een ook Kopieer de configuratiebestanden voor de dezelfde back-bestandssysteem en Kopieer alles samen naar de bestemming van de laatste opslag, zoals cool blob-opslag.

### <a name="sap-hana-cockpit"></a>SAP HANA Cockpit

SAP HANA Cockpit biedt de mogelijkheid om controleren en beheren van SAP HANA via een browser. Dit kunt u ook de verwerking van back-ups van SAP HANA en daardoor kan worden gebruikt als alternatief voor SAP HANA Studio en ABAP DBACOCKPIT (Zie [SAP HANA Cockpit](https://help.sap.com/saphelp_hanaplatform/helpdata/en/73/c37822444344f3973e0e976b77958e/content.htm) voor meer informatie).

![In deze afbeelding ziet u het scherm van SAP HANA Cockpit Database Administration](media/sap-hana-backup-guide/image004.png)

In deze afbeelding ziet u het scherm van het beheer van Cockpit-Database van SAP HANA en de back-tegel aan de linkerkant. De back-tegel ziet relevante machtigingen nodig voor aanmeldingsaccount.

![Back-ups kunnen worden bewaakt in SAP HANA Cockpit terwijl ze voortdurend worden](media/sap-hana-backup-guide/image005.png)

Back-ups kunnen worden bewaakt in SAP HANA Cockpit terwijl ze voortdurend en, als deze is voltooid, alle details van de back-up beschikbaar zijn.

![Een voorbeeld met behulp van Firefox op een Azure-VM SLES 12 met Gnome desktop](media/sap-hana-backup-guide/image006.png)

De vorige schermafbeeldingen zijn uitgevoerd vanaf een Windows VM in Azure. Dit is een voorbeeld met behulp van Firefox op een Azure-VM SLES 12 met Gnome desktop. Hier ziet u de optie voor het definiëren van SAP HANA-back-upschema's in SAP HANA Cockpit. Zoals ook zien kunnen, stelt deze datum/tijd als een voorvoegsel voor de back-upbestanden. In SAP HANA Studio, het standaardvoorvoegsel is &quot;voltooid\_gegevens\_back-up&quot; bij het uitvoeren van een volledige bestandsback-up. Gebruik een unieke voorvoegsel wordt aanbevolen.

### <a name="sap-hana-backup-encryption"></a>SAP HANA-versleuteling van back-up

SAP HANA biedt versleuteling van gegevens en logboekbestanden. Als voor SAP HANA-gegevens en logboekbestanden zijn niet versleuteld, zijn klikt u vervolgens de back-ups ook niet versleuteld. Het is aan de klant kan een vorm van de oplossing van derden gebruiken voor het versleutelen van de back-ups van SAP HANA. Zie [gegevens en logboekbestanden Volumeversleuteling](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/01f36fbb5710148b668201a6e95cf2/content.htm) voor meer informatie over SAP HANA-versleuteling.

Op Microsoft Azure, kan een klant de functie voor IaaS-VM-versleuteling gebruiken om te versleutelen. Bijvoorbeeld, kan een speciale gegevensschijven die zijn gekoppeld aan de virtuele machine, en die worden gebruikt voor het opslaan van back-ups van SAP HANA en kopieën maken van deze schijven gebruiken.

Versleutelde virtuele machines /-schijven kan worden verwerkt door Azure Backup-service (Zie [back-up en herstellen versleutelde virtuele machines met Azure Backup](../../../backup/backup-azure-vms-encryption.md)).

Een andere optie is de SAP HANA-virtuele machine en de schijven zonder versleuteling beheren en opslaan van de back-upbestanden van de SAP HANA in een opslagaccount waarvoor versleuteling is ingeschakeld (Zie [Azure Storage-Serviceversleuteling voor Data-at-Rest](../../../storage/common/storage-service-encryption.md)).

## <a name="test-setup"></a>Test-installatie

### <a name="test-virtual-machine-on-azure"></a>Test-virtuele Machine op Azure

Om uit te voeren op onze tests, is de installatie van een SAP HANA in een Azure-GS5-VM voor de volgende back-up/herstel testen gebruikt. De beginselen zijn dezelfde als de M-serie VM's.

![In deze afbeelding ziet u onderdeel van het overzicht van Azure portal voor de test-HANA VM](media/sap-hana-backup-guide/image007.png)

In deze afbeelding ziet deel van het overzicht van Azure portal voor de test-HANA VM.

### <a name="test-backup-size"></a>Testen van back-upgrootte

![In deze afbeelding is afkomstig uit de back-upconsole in HANA Studio en ziet u de grootte van de back-upbestand van 229 GB voor de HANA-server voor index](media/sap-hana-backup-guide/image008.png)

Een tijdelijke tabel is ingevuld met gegevens om op te halen van een back-grootte van de totale hoeveelheid gegevens van meer dan 200 GB voor het afleiden van realistische prestatiegegevens. De afbeelding is afkomstig uit de back-upconsole in HANA Studio en ziet u de grootte van de back-upbestand van 229 GB voor de HANA-server voor index. Bij de tests voor is de back-up standaardvoorvoegsel "COMPLETE_DATA_BACKUP" in SAP HANA Studio gebruikt. In de echte productiesystemen, moet een nuttiger voorvoegsel worden gedefinieerd. SAP HANA Cockpit stelt voor datum/tijd.

### <a name="test-tool-to-copy-files-directly-to-azure-storage"></a>Testen van hulpprogramma bestanden rechtstreeks naar Azure storage te kopiëren

Om over te dragen SAP HANA-back-upbestanden rechtstreeks naar Azure blob storage of Azure-bestandsshares, is het hulpprogramma blobxfer omdat het ondersteunt zowel doelen en deze eenvoudig kan worden geïntegreerd in automatiseringsscripts vanwege de opdrachtregelinterface gebruikt. Het hulpprogramma blobxfer is beschikbaar op [GitHub](https://github.com/Azure/blobxfer).

### <a name="test-backup-size-estimation"></a>Schatting van de back-upgrootte testen

Het is belangrijk om te schatten van de grootte van de back-up van SAP HANA. Deze schatting kunt u de prestaties verbeteren door het definiëren van de grootte van de maximale back-upbestand voor een aantal back-upbestanden of vanwege de parallelle uitvoering tijdens het kopiëren van bestanden. (Deze gegevens worden verderop in dit artikel beschreven.) Een moet ook beslissen of u een volledige back-up of een delta-back-up (incrementele of differentiële).

Gelukkig is er een eenvoudige SQL-instructie maakt een schatting van de grootte van de back-upbestanden: **Selecteer \* van M\_back-up\_grootte\_SCHATTINGEN** (Zie [schatting de ruimte die nodig zijn in het bestandssysteem voor een gegevensback-up](https://help.sap.com/saphelp_hanaplatform/helpdata/en/7d/46337b7a9c4c708d965b65bc0f343c/content.htm)).

![De uitvoer van deze SQL-instructie overeenkomt met bijna exact de werkelijke grootte van de volledige gegevens back-up op schijf](media/sap-hana-backup-guide/image009.png)

De uitvoer van deze SQL-instructie voor het testsysteem, komt overeen met bijna exact de werkelijke grootte van de volledige gegevens back-up op schijf.

### <a name="test-hana-backup-file-size"></a>Grootte van back-upbestand HANA testen

![De back-upconsole HANA Studio kan beperken van de maximale bestandsgrootte van de back-upbestanden HANA](media/sap-hana-backup-guide/image010.png)

De back-upconsole HANA Studio kunt een om te beperken van de maximale bestandsgrootte van HANA back-upbestanden. In de voorbeeldomgeving maakt die functie het mogelijk om op te halen van meerdere kleinere back-upbestanden in plaats van één 230 GB back-upbestand. Kleinere bestanden een aanzienlijke invloed heeft op de prestaties (Zie het bijbehorende artikel [SAP HANA Azure back-up op bestandsniveau](sap-hana-backup-file-level.md)).

## <a name="summary"></a>Samenvatting

Op basis van de resultaten van die de volgende tabellen tonen de voor- en nadelen van oplossingen voor back-up van een SAP HANA-database die wordt uitgevoerd op Azure virtual machines.

**Back-up van SAP HANA naar het bestandssysteem en daarna back-upbestanden naar de laatste back-updoelmap kopiëren**

|Oplossing                                           |Professionals                                 |Nadelen                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Behoudt HANA back-ups op VM-schijven                      |Er zijn geen extra risicobeheerinspanningen     |Eats lokale VM-schijfruimte vrij           |
|Blobxfer-hulpprogramma voor het back-upbestanden naar blob storage kopiëren |Parallelle uitvoering om meerdere bestanden te kopiëren keuze cool blob storage gebruiken | Onderhoud van extra hulpmiddelen en aangepaste scripts | 
|BLOB kopiëren via Powershell of CLI                    |Er zijn geen extra hulpmiddelen die nodig zijn, kan worden geconfigureerd via Azure Powershell of CLI |handmatig proces-klant beschikt om te zorgen voor het uitvoeren van scripts en het beheer van de gekopieerde blobs voor terugzetten|
|Kopiëren naar NFS-share                                  |Na verwerking van back-upbestanden op andere virtuele machine zonder gevolgen voor de HANA-server|Trage kopieerproces|
|Blobxfer kopiëren naar Azure File-Service                |Geen eten ruimte vrij op de lokale VM-schijven|Er is geen directe ondersteuning door HANA back-up, formaat beperking van de bestandsshare die momenteel op 5 TB schrijven|
|Azure Backup Agent                                 | Zou worden aanbevolen oplossing         | Op dit moment niet beschikbaar op Linux    |



**Back-SAP HANA op basis van opslagmomentopnamen**

|Oplossing                                           |Professionals                                 |Nadelen                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Azure Backup Service                               | Hiermee kunnen back-up van virtuele machine op basis van de blob-momentopnamen | Als u niet op herstel, het maken van een nieuwe virtuele machine is vereist voor het herstelproces, wat vervolgens betekent de noodzaak van een nieuwe sleutel van de SAP HANA-licentie dat|
|Handmatige blob-momentopnamen                              | Flexibiliteit om te maken en herstellen van specifieke VM-schijven zonder te hoeven wijzigen van de unieke ID van de virtuele machine|Alle handmatig werk dat moet worden uitgevoerd door de klant|

## <a name="next-steps"></a>Volgende stappen
* [SAP HANA Azure back-up op bestandsniveau](sap-hana-backup-file-level.md) beschrijving van de bestanden zijn gebaseerd back-upoptie.
* [Back-up van SAP HANA op basis van opslagmomentopnamen](sap-hana-backup-storage-snapshots.md) beschrijving van de opslag op basis van een momentopname van back-upoptie.
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure (grote instanties), [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md).

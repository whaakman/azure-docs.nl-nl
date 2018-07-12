---
title: StorSimple Adapter voor SharePoint installeren | Microsoft Docs
description: Beschrijft het installeren en configureren of verwijderen van de StorSimple-Adapter voor SharePoint in een SharePoint-serverfarm.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 36c20b75-f2e5-4184-a6b5-9c5e618f79b2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/06/2017
ms.author: v-sharos
ms.openlocfilehash: 2e1b231a5cf13d2655ff66c7e48752729c580f48
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38232864"
---
# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>Installeren en configureren van de StorSimple-Adapter voor SharePoint
## <a name="overview"></a>Overzicht
De StorSimple-Adapter voor SharePoint is een onderdeel dat beschikt u over flexibele Microsoft Azure StorSimple-opslag en betere gegevensbescherming naar SharePoint-serverfarms. Binaire grote Object (BLOB)-inhoud van de SQL Server-inhoudsdatabases naar het apparaat met Microsoft Azure StorSimple hybrid cloud-opslag kunt u de adapter.

De StorSimple-Adapter voor SharePoint fungeert als een externe BLOB Storage planning-provider en de functie voor SQL Server extern BLOB-opslag gebruikt voor het opslaan van ongestructureerde SharePoint-inhoud (in de vorm van BLOBs) op een bestandsserver die wordt ondersteund door een StorSimple-apparaat.

> [!NOTE]
> De StorSimple-Adapter voor SharePoint biedt ondersteuning voor SharePoint Server 2010 extern BLOB Storage planning. Het biedt geen ondersteuning voor SharePoint Server 2010 externe BLOB Storage (EBS).


* Voor het downloaden van de StorSimple-Adapter voor SharePoint, gaat u naar [StorSimple Adapter voor SharePoint] [ 1] in het Microsoft Download Center.
* Voor informatie over het plannen van Resourcestructuur en Resourcestructuur beperkingen, gaat u naar [besluit te gebruikt dit veld in SharePoint 2013] [ 2] of [plannen voor Resourcestructuur (SharePoint Server 2010)] [ 3].

De rest van dit overzicht wordt een korte beschrijving van de rol van de StorSimple-Adapter voor SharePoint en de SharePoint-capaciteit en prestaties van limieten die u moet rekening houden met voordat u installeert en configureert de adapter. Nadat u deze gegevens bekijken, gaat u naar [StorSimple-Adapter voor SharePoint-installatie](#storsimple-adapter-for-sharepoint-installation) om te beginnen met het instellen van de adapter.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple Adapter voor SharePoint-voordelen
In een SharePoint-site wordt inhoud opgeslagen als niet-gestructureerde blobgegevens in een of meer inhoudsdatabases. Standaard worden deze databases worden gehost op computers waarop SQL Server wordt uitgevoerd en bevinden zich in de SharePoint-serverfarm. BLOBs toenemen snel in omvang, verbruiken grote hoeveelheden on-premises opslag. Daarom is het raadzaam om een andere, minder dure opslagoplossing te zoeken. SQL Server biedt een technologie met de naam van de externe Blob Storage planning waarmee u kunt de inhoud van BLOB opslaan in het bestandssysteem buiten de SQL Server-database. BLOBs kunnen zich bevinden in het bestandssysteem op de computer waarop SQL Server wordt uitgevoerd met Resourcestructuur, of ze kunnen worden opgeslagen in het bestandssysteem op een andere servercomputer.

Resourcestructuur is vereist dat u een RBS-provider, zoals de StorSimple-Adapter voor SharePoint gebruiken om in te schakelen Resourcestructuur in SharePoint. De StorSimple-Adapter voor SharePoint werkt met Resourcestructuur, zodat u BLOBs verplaatsen naar een server die back-up gemaakt door de Microsoft Azure StorSimple-systeem. Microsoft Azure StorSimple slaat vervolgens de BLOB-gegevens lokaal of in de cloud, op basis van gebruik. BLOBs die zeer actief zijn (gewoonlijk aangeduid als laag 1 of gegevens) lokaal zich bevinden. Minder actieve gegevens en archivering van gegevens zich bevinden in de cloud. Nadat u op een inhoudsdatabase Resourcestructuur hebt ingeschakeld, wordt een nieuwe blobinhoud in SharePoint gemaakt op het StorSimple-apparaat en niet in de inhoud van de database opgeslagen.

De Microsoft Azure StorSimple-implementatie van Resourcestructuur biedt de volgende voordelen:

* BLOB-inhoud naar een aparte server verplaatsen, kunt u beperken de querybelasting op SQL Server, die de SQL Server-reactietijd kunt verbeteren. 
* Azure StorSimple maakt gebruik van gegevensontdubbeling en compressie om gegevensgrootte te beperken.
* Azure StorSimple biedt bescherming van gegevens in de vorm van lokale en cloudmomentopnamen. Ook als u de database zelf op het StorSimple-apparaat, kunt u back-up de inhoud van de database en de BLOBs samen in een crash-consistente manier. (De inhoud van de database te verplaatsen naar het apparaat wordt alleen ondersteund voor de StorSimple 8000-apparaat. Deze functie wordt niet ondersteund voor de 5000 of 7000-serie.)
* Azure StorSimple bevat disaster recovery functies zoals failover, bestands- en volume recovery (inclusief test recovery) en snel herstel van gegevens.
* Data recovery-software, zoals Kroll Ontrack PowerControls, kunt u met StorSimple momentopnamen van BLOB-gegevens om uit te voeren herstel op itemniveau van SharePoint-inhoud. (Deze gegevens recovery-software is een afzonderlijke aankoop.)
* De StorSimple-Adapter voor SharePoint kunt aansluiten op de centrale beheersite van SharePoint-portal, zodat u kunt voor het beheren van uw gehele SharePoint-oplossing van een centrale locatie.

BLOB-inhoud verplaatsen naar het bestandssysteem, kan andere kosten te besparen en de voordelen bieden. Bijvoorbeeld, met behulp van Resourcestructuur kunt verminderen de noodzaak van dure laag 1-opslag en omdat Hiermee verkleint u de inhoud van de database, Resourcestructuur het aantal databases dat is vereist in de SharePoint-serverfarm kunt beperken. Kan zijn dat de andere factoren, zoals de maximale grootte database en de hoeveelheid niet-Resourcestructuur inhoud, ook opslagvereisten beïnvloeden. Zie voor meer informatie over de kosten en voordelen van het gebruik van Resourcestructuur [plannen voor Resourcestructuur (SharePoint Foundation 2010)] [ 4] en [besluit te gebruikt dit veld in SharePoint 2013] [5].

### <a name="capacity-and-performance-limits"></a>Limieten voor capaciteit en prestaties
Voordat u kunt u Resourcestructuur in uw SharePoint-oplossing overwegen, moet u rekening houden met de geteste prestaties en de beperkingen van de capaciteit van SharePoint Server 2010 en SharePoint Server 2013 en hoe deze limieten zich verhouden tot aanvaardbare prestaties. Zie voor meer informatie, [Software grenzen en beperkingen voor SharePoint 2013](https://technet.microsoft.com/library/cc262787.aspx).

Lees het volgende voordat u Resourcestructuur configureren:

* Zorg ervoor dat de totale grootte van de inhoud (de grootte van een inhoudsdatabase) plus de grootte van alle bijbehorende externalized BLOBs niet de maximale grootte voor Resourcestructuur dat wordt ondersteund door SharePoint overschrijdt. Deze limiet is 200 GB. 
  
    **Meting inhoud van de database en de grootte van de BLOB**
  
  1. Deze query uitvoeren op de centrale beheersite WFE. Start de SharePoint-beheershell en voer de volgende Windows PowerShell-opdracht om op te halen van de grootte van de inhoudsdatabases:
     
     `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`
     
      Deze stap wordt de grootte van de inhoud van de database op de schijf.
  2. Voer een van de volgende SQL-query's in SQL Management Studio op het SQL server-vak in elke inhoudsdatabase en het resultaat toevoegen aan het aantal verkregen in stap 1.
     
     Voer op de inhoudsdatabases van SharePoint 2013:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`
     
     Voer op de inhoudsdatabases van SharePoint 2010:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`
     
     Deze stap wordt de grootte van de BLOBs die zijn is externalized opgehaald.
* Het is raadzaam dat u alle inhoud voor BLOB STORAGE en database lokaal opslaan op het StorSimple-apparaat. Het StorSimple-apparaat is een cluster met twee knooppunten voor hoge beschikbaarheid. Hoge beschikbaarheid biedt het plaatsen van de inhoudsdatabases en BLOBs op het StorSimple-apparaat.
  
    Traditionele SQL Server-migratie aanbevolen werkwijzen gebruikt om de inhoud van de database verplaatsen naar het StorSimple-apparaat. Verplaats de database pas nadat alle BLOB-inhoud uit de database is verplaatst naar de bestandsshare via de Resourcestructuur. Als u ervoor kiest om te verplaatsen van de inhoud van de database met de StorSimple-apparaat, raden wij u aan de inhoud van de database-opslag te configureren op het apparaat als een primaire volume.
* In Microsoft Azure StorSimple, als gelaagde volumes, is er geen manier om te waarborgen dat inhoud lokaal opgeslagen op de StorSimple device gelaagd naar Microsoft Azure-cloudopslag. Daarom is het raadzaam StorSimple lokaal vastgemaakt volumes gebruiken in combinatie met SharePoint RBS. Dit zorgt ervoor dat alle BLOB-inhoud lokaal op het StorSimple-apparaat blijft, en niet wordt verplaatst naar Microsoft Azure.
* Als u de inhoudsdatabases niet bij de StorSimple-apparaat bewaren, gebruikt u traditionele hoge beschikbaarheid aanbevolen procedures van SQL Server die ondersteuning bieden voor Resourcestructuur. SQL Server clustering ondersteunt Resourcestructuur, terwijl SQL Server spiegelen niet. 

> [!WARNING]
> Als u Resourcestructuur niet hebt ingeschakeld, niet aangeraden de inhoud van de database verplaatsen naar het StorSimple-apparaat. Dit is een niet-geteste configuratie.

## <a name="storsimple-adapter-for-sharepoint-installation"></a>StorSimple Adapter voor SharePoint-installatie
Voordat u de StorSimple-Adapter voor SharePoint installeert kunt, moet u het StorSimple-apparaat configureren en ervoor zorgen dat de SharePoint-serverfarm en de SQL Server-instantiëring aan alle vereisten. Deze zelfstudie beschrijft configuratievereisten, evenals de procedures voor het installeren en upgraden van de StorSimple-Adapter voor SharePoint.

## <a name="configure-prerequisites"></a>Vereisten configureren
Voordat u de StorSimple-Adapter voor SharePoint installeert kunt, controleert u de StorSimple-apparaat, SharePoint server-farm en SQL Server-instantiëring voldoen aan de volgende vereisten.

### <a name="system-requirements"></a>Systeemvereisten
De StorSimple-Adapter voor SharePoint werkt met de volgende hardware en software:

* Ondersteund besturingssysteem: Windows Server 2008 R2 SP1, Windows Server 2012 of Windows Server 2012 R2
* Ondersteunde SharePoint-versies: SharePoint Server 2010 of SharePoint Server 2013
* Ondersteunde versies van SQL Server: SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition of SQL Server 2012 Enterprise Edition
* StorSimple-apparaten ondersteund StorSimple 8000-serie, StorSimple 7000-serie en StorSimple 5000-reeks.

### <a name="storsimple-device-configuration-prerequisites"></a>Configuratievereisten voor StorSimple-apparaat
Het StorSimple-apparaat een apparaat is geblokkeerd en als zodanig vereist een bestandsserver waarop de gegevens kunnen worden gehost. U wordt aangeraden dat u een afzonderlijke server in plaats van een bestaande server van de SharePoint-farm gebruiken. Deze bestandsserver moet op het dezelfde local area network (LAN) als de SQL Server-computer die als host fungeert voor de inhoudsdatabases.

> [!TIP]
> * Als u uw SharePoint-farm voor maximale beschikbaarheid configureert, moet u ook de bestandsserver voor hoge beschikbaarheid implementeren.
> * Als u de inhoud van de database niet bij de StorSimple-apparaat bewaren, gebruikt u traditionele, maximaal beschikbare aanbevolen procedures die ondersteuning bieden voor Resourcestructuur. SQL Server clustering ondersteunt Resourcestructuur, terwijl SQL Server spiegelen niet. 


Zorg ervoor dat uw StorSimple-apparaat correct is geconfigureerd en dat de juiste volumes voor de ondersteuning van uw SharePoint-implementatie zijn geconfigureerd en toegankelijk is vanaf de SQL Server-computer. Ga naar [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md) als u nog niet hebt geïmplementeerd en geconfigureerd van uw StorSimple-apparaat. Houd er rekening mee de IP-adres van het StorSimple-apparaat. u moet deze tijdens de StorSimple-Adapter voor SharePoint-installatie.

Bovendien moet u ervoor zorgen dat het volume moet worden gebruikt voor BLOB bedrijfsprocessen voldoet aan de volgende vereisten:

* Het volume moet zijn geformatteerd met een clustergrootte van 64 KB.
* Uw web-front-end (WFE) en de toepassingsservers moet toegang hebben tot het volume via een pad (Universal Naming Convention) bevinden.
* De SharePoint-serverfarm moet worden geconfigureerd voor het schrijven naar het volume.

> [!NOTE]
> Na het installeren en configureren van de adapter, alle BLOB bedrijfsprocessen moet gaan via het StorSimple-apparaat (het apparaat zal dienen als de volumes met SQL Server en de opslaglagen beheren). U kunt andere doelen niet gebruiken voor BLOB-bedrijfsprocessen.


Als u van plan bent te StorSimple Snapshot Manager om momentopnamen van de gegevens voor BLOB STORAGE en database te gebruiken, moet u StorSimple Snapshot Manager installeren op de database-server zodat deze de SQL Writer-Service gebruiken kunt voor het implementeren van de Windows Volume Shadow Copy Service (VSS).

> [!IMPORTANT]
> StorSimple Snapshot Manager biedt geen ondersteuning voor de SharePoint VSS Writer en toepassingsconsistente momentopnamen van SharePoint-gegevens kan niet worden gemaakt. StorSimple Snapshot Manager voorziet in een SharePoint-scenario, alleen crash-consistente back-ups.


## <a name="sharepoint-farm-configuration-prerequisites"></a>Configuratievereisten voor SharePoint-farm
Zorg ervoor dat uw SharePoint-serverfarm correct is geconfigureerd, als volgt:

* Controleer of uw SharePoint-serverfarm in orde is, en controleert u het volgende:
* Alle SharePoint WFE en toepassingsservers die zijn geregistreerd in de farm worden uitgevoerd en van de server waarop u de StorSimple-Adapter voor SharePoint installeert kunnen worden gepingd.
* De SharePoint Timer-service (SPTimerV3 of SPTimerV4) wordt uitgevoerd op de WFE-server en de toepassingsserver.
* Zowel de IIS-toepassingsgroep waarmee de centraalbeheersite van SharePoint wordt uitgevoerd als de SharePoint Timer-service hebt beheerdersbevoegdheden nodig.
* Zorg ervoor dat Internet Explorer Verbeterde beveiligingscontext (IE ESC) is uitgeschakeld. Volg deze stappen voor IE ESC uitschakelen:
  
  1. Sluit alle exemplaren van Internet Explorer.
  2. Start de Server Manager.
  3. Klik in het linkerdeelvenster op **lokale Server**.
  4. In het rechterdeelvenster naast **verbeterde beveiliging van Internet Explorer**, klikt u op **op**.
  5. Onder **beheerders**, klikt u op **uit**.
  6. Klik op **OK**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Externe vereisten voor BLOB Storage planning
Zorg ervoor dat u van een ondersteunde versie van SQL Server gebruikmaakt. Alleen de volgende versies worden ondersteund en kan gebruikt dit veld:

* SQL Server 2008 Enterprise Edition
* SQL Server 2008 R2 Enterprise Edition
* SQL Server 2012 Enterprise Edition

BLOBs kunnen worden externalized voor alleen volumes die het StorSimple-apparaat aan SQL Server biedt. Er zijn geen andere doelen voor BLOB bedrijfsprocessen worden ondersteund.

Als u alle vereiste configuratiestappen hebt voltooid, gaat u naar [de StorSimple-Adapter voor SharePoint installeren](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>De StorSimple-Adapter voor SharePoint installeren
Gebruik de volgende stappen voor het installeren van de StorSimple-Adapter voor SharePoint. Als u de software opnieuw installeert, Zie [bijwerken of opnieuw installeren van de StorSimple-Adapter voor SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). De tijd die nodig is voor de installatie is afhankelijk van het totale aantal SharePoint-databases in uw SharePoint-serverfarm.

[!INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]

## <a name="configure-rbs"></a>Resourcestructuur configureren
Nadat u de StorSimple-Adapter voor SharePoint installeert, configureert u Resourcestructuur zoals beschreven in de volgende procedure.

> [!TIP]
> De StorSimple-Adapter voor SharePoint kunt aansluiten op de pagina Centraal beheer van SharePoint zodat Resourcestructuur moet worden ingeschakeld of uitgeschakeld in elke inhoudsdatabase in de SharePoint-farm. In- of uitschakelen van Resourcestructuur op de inhoud van de database wordt echter een IIS opnieuw instellen, die, afhankelijk van de configuratie van uw bedrijf tijdelijk de beschikbaarheid van de SharePoint web-front-end (WFE onderbroken kan). (Factoren, zoals het gebruik van een front-end-load balancer, de huidige serverwerkbelasting, enzovoort, kunnen beperken of deze onderbreking te elimineren.) Als u wilt voorkomen dat gebruikers een onderbreking, wordt aangeraden dat u in- of uitschakelen van Resourcestructuur alleen tijdens een geplande onderhoudsvenster.


[!INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]

## <a name="configure-garbage-collection"></a>Garbagecollection configureren
Wanneer u objecten uit een SharePoint-site worden verwijderd, zijn ze niet automatisch verwijderd uit de store Resourcestructuur volume. In plaats daarvan een asynchrone, achtergrond onderhoudsprogramma verwijdert zwevende BLOBs uit de store van het bestand. Systeembeheerders kunnen plannen dat dit proces periodiek wordt uitgevoerd of ze deze altijd indien nodig kunnen starten.

Dit onderhoudsprogramma (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) wordt automatisch geïnstalleerd op alle SharePoint WFE-servers en toepassingsservers, wanneer u Resourcestructuur inschakelt. Het programma is geïnstalleerd op de volgende locatie: *opstartschijf*: \Program Files\Microsoft externe SQL-blobopslag 10.50\Maintainer\

Zie voor meer informatie over het configureren en gebruiken van de onderhoudsprogramma [Resourcestructuur onderhouden in SharePoint Server 2013][8].

> [!IMPORTANT]
> Het Resourcestructuur maintainer programma is resource-intensief. U moet plannen voor uitvoering alleen tijdens perioden van weinig activiteit op de SharePoint-farm.


### <a name="delete-orphaned-blobs-immediately"></a>Zwevende BLOBs onmiddellijk verwijderen
Als u verwijderen van zwevende BLOBs onmiddellijk wilt, kunt u de volgende instructies gebruiken. Houd er rekening mee dat deze instructies een voorbeeld zijn van hoe dit kan worden gedaan in een SharePoint 2013-omgeving met de volgende onderdelen:

* De naam van de inhoud van de database is WSS_Content.
* De naam van de SQL Server is SHRPT13 SQL12\SHRPT13.
* Naam van de webtoepassing is SharePoint – 80.

[!INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]

## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>Bijwerken of opnieuw installeren van de StorSimple-Adapter voor SharePoint
Gebruik de volgende procedure SharePoint-server bijwerken en opnieuw StorSimple-Adapter voor SharePoint installeren of te eenvoudig upgraden of opnieuw installeren van de adapter in een bestaande SharePoint-serverfarm.

> [!IMPORTANT]
> Lees de volgende informatie voordat u probeert te upgraden van uw SharePoint-software en/of de upgrade of de StorSimple-Adapter voor SharePoint installeren:
> 
> * Alle bestanden die eerder zijn verplaatst naar de externe opslag, via de Resourcestructuur zijn is niet beschikbaar totdat de installatie is voltooid en de functie Resourcestructuur opnieuw is ingeschakeld. Als u wilt beperken impact op gebruikers, uitvoeren een upgrade of installatie tijdens gepland onderhoud.
> * De tijd die nodig is voor de upgrade/opnieuw installeren kan variëren, afhankelijk van het totale aantal SharePoint-databases in de SharePoint-serverfarm.
> * Nadat de upgrade/installatie voltooid is, moet u Resourcestructuur inschakelen voor de inhoudsdatabases. Zie [configureren Resourcestructuur](#configure-rbs) voor meer informatie.
> * Als u Resourcestructuur configureert voor een SharePoint-farm met een zeer groot aantal databases (meer dan 200), de **Centraal beheer van SharePoint** pagina mogelijk time-out. Als dat gebeurt, vernieuw de pagina. Dit heeft geen invloed op het configuratieproces.


[!INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]

## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple Adapter voor SharePoint verwijderen
De volgende procedures wordt beschreven hoe u de BLOBs terug verplaatsen naar de SQL Server-inhoudsdatabases en verwijder de StorSimple-Adapter voor SharePoint. 

> [!IMPORTANT]
> U moet de BLOBs terug verplaatsen naar de inhoudsdatabases voordat u de adaptersoftware verwijderen.


### <a name="before-you-begin"></a>Voordat u begint
Verzamelt de volgende informatie voordat u de gegevens terug naar de SQL Server-inhoudsdatabases verplaatsen en beginnen met het verwijderingsproces adapter:

* De namen van alle databases waarvoor Resourcestructuur is ingeschakeld
* Het UNC-pad van de geconfigureerde BLOB-opslag

### <a name="move-the-blobs-back-to-the-content-databases"></a>De BLOBs terug verplaatsen naar de inhoudsdatabases
Voordat u de StorSimple-Adapter voor SharePoint-software verwijdert, moet u alle BLOBs die zijn externalized terug naar de SQL Server-inhoudsdatabases migreren. Als u probeert te verwijderen van de StorSimple-Adapter voor SharePoint voordat u alle BLOBs terug naar de inhoudsdatabases verplaatsen, ziet u de volgende waarschuwing weergegeven.

![Waarschuwingsbericht](./media/storsimple-adapter-for-sharepoint/sasp1.png)

#### <a name="to-move-the-blobs-back-to-the-content-databases"></a>De BLOBs terug verplaatsen naar de inhoudsdatabases
1. Download de externalized op hun beurt.
2. Open de **Centraal beheer van SharePoint** pagina en blader naar **systeeminstellingen**.
3. Onder **Azure StorSimple**, klikt u op **StorSimple-Adapter configureren**.
4. Op de **StorSimple-Adapter configureren** pagina, klikt u op de **uitschakelen** knop onder elk van de inhoudsdatabases die u wilt verwijderen van externe BLOB-opslag. 
5. De objecten uit SharePoint verwijderen en vervolgens opnieuw uploaden.

U kunt ook kunt u de Microsoft` RBS Migrate()` PowerShell-cmdlet die deel uitmaakt van SharePoint. Zie voor meer informatie, [inhoud migreren naar of uit Resourcestructuur](https://technet.microsoft.com/library/ff628255.aspx).

Nadat u de BLOBs terug naar de inhoud van de database verplaatsen, gaat u naar de volgende stap: [verwijderen van de adapter](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>De adapter verwijderen
Nadat u de BLOBs terug naar de SQL Server-inhoudsdatabases verplaatsen, kunt u een van de volgende opties voor het verwijderen van de StorSimple-Adapter voor SharePoint gebruiken.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Het installatieprogramma voor het verwijderen van de adapter te gebruiken
1. Gebruik een account met administrator-bevoegdheden voor aanmelding bij de web-front-end (WFE)-server.
2. Dubbelklik op de StorSimple-Adapter voor SharePoint-installatieprogramma. De Wizard Setup wordt gestart.
   
    ![Setup-wizard](./media/storsimple-adapter-for-sharepoint/sasp2.png)
3. Klik op **Volgende**. De volgende pagina wordt weergegeven.
   
    ![Pagina voor Setup wizard verwijderen](./media/storsimple-adapter-for-sharepoint/sasp3.png)
4. Klik op **verwijderen** het verwijderingsproces te selecteren. De volgende pagina wordt weergegeven.
   
    ![Bevestigingspagina voor Setup-wizard](./media/storsimple-adapter-for-sharepoint/sasp4.png)
5. Klik op **verwijderen** de verwijdering te bevestigen. De volgende pagina van de voortgang wordt weergegeven.
   
    ![Instellen van de voortgangspagina van wizard](./media/storsimple-adapter-for-sharepoint/sasp5.png)
6. Wanneer het verwijderen voltooid is, verschijnt de voltooiingspagina. Klik op **voltooien** aan de Wizard Setup afsluit.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Het Configuratiescherm gebruiken om te verwijderen van de adapter
1. Open het Configuratiescherm en klik vervolgens op **programma's en onderdelen**.
2. Selecteer **StorSimple Adapter voor SharePoint**, en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/library/ff943565.aspx

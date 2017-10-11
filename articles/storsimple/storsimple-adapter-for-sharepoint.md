---
title: StorSimple-Adapter installeren voor SharePoint | Microsoft Docs
description: Beschrijft het installeren en configureren of verwijder de StorSimple-Adapter voor SharePoint in een SharePoint-serverfarm.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 36c20b75-f2e5-4184-a6b5-9c5e618f79b2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/06/2017
ms.author: v-sharos
ms.openlocfilehash: 8910471e09b9ecc797005818538ccfc6a91c68a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>Installeren en configureren van de StorSimple-Adapter voor SharePoint
## <a name="overview"></a>Overzicht
De StorSimple-Adapter voor SharePoint is een onderdeel waarmee u Microsoft Azure StorSimple flexibele opslag en gegevensbeveiliging voor SharePoint server-farms. Binaire grote Object (BLOB) om inhoud te verplaatsen van de SQL Server-inhoudsdatabases op het apparaat met Microsoft Azure StorSimple hybride cloud-opslag kunt u de adapter.

De StorSimple-Adapter voor SharePoint fungeert als een externe BLOB Storage planning provider en de functie SQL Server externe BLOB Storage gebruikt voor het opslaan van ongestructureerde SharePoint-inhoud (in de vorm van BLOB's) op een bestandsserver die wordt ondersteund door een StorSimple-apparaat.

> [!NOTE]
> De StorSimple-Adapter voor SharePoint biedt ondersteuning voor SharePoint Server 2010 afstand BLOB Storage planning. Geen biedt ondersteuning voor SharePoint Server 2010 externe BLOB Storage (EBS).


* Ga naar de StorSimple-Adapter voor SharePoint downloaden, [StorSimple-Adapter voor SharePoint] [ 1] in het Microsoft Download Center.
* Voor informatie over de planning voor Resourcestructuur en Resourcestructuur beperkingen gaat u naar [beslist naar gebruikt dit veld in SharePoint 2013] [ 2] of [plannen voor Resourcestructuur (SharePoint Server 2010)] [ 3].

De rest van dit overzicht wordt een korte beschrijving van de rol van de StorSimple-Adapter voor SharePoint en de SharePoint-capaciteit en prestaties limieten die u kennen moet voordat u installeren en configureren van de adapter. Nadat u deze gegevens bekijken, gaat u naar [StorSimple-Adapter voor SharePoint-installatie](#storsimple-adapter-for-sharepoint-installation) om te beginnen met instellen van de adapter.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple-Adapter voor SharePoint voordelen
Inhoud wordt opgeslagen in een SharePoint-site als niet-gestructureerde BLOB-gegevens in een of meer inhoudsdatabases. Standaard worden deze databases worden gehost op computers waarop SQL Server uitvoert en bevinden zich in de SharePoint-serverfarm. BLOBs kunnen snel worden verhoogd met grootte, verbruiken grote hoeveelheden lokale opslag. Daarom is het raadzaam om een andere, minder dure opslagoplossing te vinden. SQL Server biedt een technologie voor externe Blob Storage planning waarmee u het opslaan van BLOB-inhoud in het bestandssysteem buiten de SQL Server-database. BLOBs kunnen zich bevinden in het bestandssysteem op de computer waarop SQL Server wordt uitgevoerd met Resourcestructuur, of ze kunnen worden opgeslagen in het bestandssysteem op een andere servercomputer.

Resourcestructuur moet u een RBS-provider, zoals de StorSimple-Adapter voor SharePoint, gebruiken om in te schakelen Resourcestructuur in SharePoint. De StorSimple-Adapter voor SharePoint werkt met Resourcestructuur, zodat u BLOBs verplaatsen naar een server die back-up gemaakt door de Microsoft Azure StorSimple-systeem. Microsoft Azure StorSimple slaat vervolgens de BLOB-gegevens lokaal of in de cloud, op basis van gebruik. BLOBs die zeer actief zijn (gewoonlijk aangeduid als het niveau van Tier 1 of gegevens hot) staan lokaal. Minder actieve gegevens en archivering van gegevens zich bevinden in de cloud. Nadat u op een inhoudsdatabase Resourcestructuur hebt ingeschakeld, wordt alle nieuwe BLOB-inhoud in SharePoint gemaakt op het StorSimple-apparaat en niet in de inhoud van de database opgeslagen.

De implementatie van Microsoft Azure StorSimple van Resourcestructuur biedt de volgende voordelen:

* BLOB-inhoud verplaatsen naar een aparte server, kunt u de querybelasting van SQL Server, SQL Server-reactiesnelheid betere beperken. 
* Azure StorSimple maakt gebruik van gegevensontdubbeling en compressie om gegevensgrootte te beperken.
* Azure StorSimple biedt bescherming van gegevens in de vorm van lokale en cloudmomentopnamen. Ook als u de database zelf plaats op het StorSimple-apparaat, kunt u back-up de inhoud van de database en de BLOBs samen in een crashconsistent manier. (De inhoud van de database te verplaatsen naar het apparaat wordt alleen ondersteund voor de StorSimple 8000 serie-apparaat. Deze functie wordt niet ondersteund voor de 5000 of 7000-serie.)
* Azure StorSimple bevat disaster recovery functies zoals failover, bestands- en volume herstel (inclusief test recovery) en snel herstel van gegevens.
* U kunt data recovery software, zoals Kroll Ontrack PowerControls, met StorSimple momentopnamen van blobgegevens herstel op itemniveau van SharePoint-inhoud uit te voeren. (Deze software voor het herstel van gegevens is een afzonderlijke aankoop.)
* De StorSimple-Adapter voor SharePoint kunt aansluiten op de portal SharePoint Centraal beheer, zodat u voor het beheren van uw hele SharePoint-oplossing van een centrale locatie.

BLOB-inhoud verplaatsen naar het bestandssysteem bieden andere kostenbesparingen en voordelen. Bijvoorbeeld de behoefte aan dure Tier 1-opslag met behulp van Resourcestructuur inkorten en omdat deze de inhoudsdatabase kleiner, Resourcestructuur het aantal databases dat is vereist in de SharePoint-serverfarm kan verminderen. Kan zijn dat de andere factoren, zoals de maximale grootte database en de hoeveelheid inhoud niet Resourcestructuur, ook opslagvereisten beïnvloeden. Zie voor meer informatie over de kosten en voordelen van het gebruik van Resourcestructuur [plannen voor Resourcestructuur (SharePoint Foundation 2010)] [ 4] en [beslist naar gebruikt dit veld in SharePoint 2013] [5].

### <a name="capacity-and-performance-limits"></a>Limieten voor capaciteit en prestaties
Voordat u kunt u Resourcestructuur in de SharePoint-oplossing overwegen, moet u zich bewust zijn van de geteste prestaties en beperkingen van de capaciteit van SharePoint Server 2010 en SharePoint Server 2013 en hoe deze beperkingen hebben betrekking op aanvaardbare prestaties. Zie voor meer informatie [Software grenzen en beperkingen voor SharePoint 2013](https://technet.microsoft.com/library/cc262787.aspx).

Controleer het volgende voordat u Resourcestructuur configureren:

* Zorg ervoor dat de totale grootte van de inhoud (de grootte van een inhoudsdatabase) plus de grootte van alle gekoppelde externalized BLOBs niet de maximale grootte voor Resourcestructuur dat wordt ondersteund door SharePoint overschrijdt. Deze limiet is 200 GB. 
  
    **Meting inhoud van de database en de grootte van de BLOB**
  
  1. Deze query uitvoeren op de centrale beheersite WFE. Start de SharePoint-beheershell en voer vervolgens de volgende Windows PowerShell-opdracht voor het ophalen van de grootte van de inhoudsdatabases:
     
     `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`
     
      Deze stap wordt de grootte van de inhoud van de database op de schijf opgehaald.
  2. Voer een van de volgende SQL-query's in SQL Management Studio op de SQL server-box op elke inhoudsdatabase en het resultaat toevoegen aan het aantal verkregen in stap 1.
     
     Voer op de inhoudsdatabases van SharePoint 2013:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`
     
     Voer op de inhoudsdatabases van SharePoint 2010:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`
     
     Deze stap wordt de grootte van de BLOBs die hebben is externalized.
* U wordt aangeraden dat u alle BLOBS en database inhoud lokaal opslaan op het StorSimple-apparaat. Het StorSimple-apparaat is een cluster met twee knooppunten voor hoge beschikbaarheid. De inhoudsdatabases en BLOBs plaatsen op het StorSimple-apparaat biedt hoge beschikbaarheid.
  
    Gebruik traditionele SQL Server migratie best practices in de database met inhoud verplaatsen naar het StorSimple-apparaat. Verplaats de database pas nadat alle BLOB-inhoud uit de database is verplaatst naar de bestandsshare via de Resourcestructuur. Als u ervoor kiest de inhoud van de database verplaatsen naar het StorSimple-apparaat, wordt u aangeraden om de inhoud van de database-opslag te op het apparaat als primaire volume configureren.
* In Microsoft Azure StorSimple, als gelaagde volumes gebruikt, is er geen manier om bescherming te bieden die inhoud op het apparaat wordt niet in tiers worden verdeeld naar Microsoft Azure-cloudopslag van StorSimple lokaal opgeslagen. Daarom wordt u aangeraden lokaal vastgemaakt StorSimple-volumes in combinatie met SharePoint RBS. Dit zorgt ervoor dat alle BLOB-inhoud lokaal op de StorSimple-apparaat blijft, en niet naar de Microsoft Azure verplaatst is.
* Als u niet de inhoudsdatabases op het StorSimple-apparaat opslaat, gebruik traditionele SQL Server hoge beschikbaarheid best practices die ondersteuning bieden voor Resourcestructuur. SQL Server clustering ondersteunt Resourcestructuur, terwijl SQL Server spiegelen niet. 

> [!WARNING]
> Als u Resourcestructuur niet hebt ingeschakeld, niet aangeraden de inhoudsdatabase te verplaatsen naar de StorSimple-apparaat. Dit is een niet-geteste configuratie.

## <a name="storsimple-adapter-for-sharepoint-installation"></a>StorSimple-Adapter voor SharePoint-installatie
Voordat u de StorSimple-Adapter voor SharePoint installeren kunt, moet u het StorSimple-apparaat configureren en zorg ervoor dat de SharePoint-serverfarm en SQL Server-exemplaar aan alle vereisten voldoen. Deze zelfstudie wordt beschreven configuratievereisten, evenals de procedures voor het installeren en upgraden van de StorSimple-Adapter voor SharePoint.

## <a name="configure-prerequisites"></a>Vereisten configureren
Voordat u de StorSimple-Adapter voor SharePoint installeren kunt, zorg ervoor dat de StorSimple-apparaat, SharePoint-serverfarm en instantiëring van SQL Server voldoen aan de volgende vereisten.

### <a name="system-requirements"></a>Systeemvereisten
De StorSimple-Adapter voor SharePoint werkt met de volgende hardware en software:

* Ondersteund besturingssysteem: Windows Server 2008 R2 SP1, Windows Server 2012 of Windows Server 2012 R2
* Ondersteunde SharePoint versies – SharePoint Server 2010 of SharePoint Server 2013
* Ondersteunde versies van SQL Server: SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition of SQL Server 2012 Enterprise Edition
* StorSimple-apparaten ondersteund StorSimple 8000-serie, StorSimple 7000-serie of StorSimple 5000-reeks.

### <a name="storsimple-device-configuration-prerequisites"></a>Configuratievereisten voor StorSimple-apparaat
Het StorSimple-apparaat is een block-apparaat, en als zodanig vereist een bestandsserver waarop de gegevens kan worden gehost. Het is raadzaam dat u een afzonderlijke server in plaats van een bestaande server van de SharePoint-farm. Deze bestandsserver moet op dezelfde local area network (LAN) als de SQL Server-computer die als host fungeert voor de inhoudsdatabases.

> [!TIP]
> * Als u uw SharePoint-farm voor maximale beschikbaarheid configureert, moet u ook de bestandsserver voor hoge beschikbaarheid implementeren.
> * Als u niet de inhoud van de database op het StorSimple-apparaat opslaat, gebruikt u traditionele hoge beschikbaarheid aanbevolen procedures die ondersteuning bieden voor Resourcestructuur. SQL Server clustering ondersteunt Resourcestructuur, terwijl SQL Server spiegelen niet. 


Zorg ervoor dat uw StorSimple-apparaat correct is geconfigureerd en dat de relevante volumes voor de ondersteuning van uw SharePoint-implementatie zijn geconfigureerd en toegankelijk is vanaf de SQL Server-computer. Ga naar [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md) als u nog niet hebt geïmplementeerd en geconfigureerd uw StorSimple-apparaat. Noteer de IP-adres van de StorSimple-apparaat; u hebt deze tijdens de StorSimple-Adapter voor SharePoint-installatie.

Bovendien moet u ervoor dat het volume moet worden gebruikt voor BLOB externalization aan de volgende vereisten voldoet:

* Het volume moet zijn geformatteerd met een clustergrootte van 64 KB.
* Uw web-front-end (WFE) en toepassingsservers moet toegang kunnen krijgen tot het volume via een pad (Universal Naming Convention) bevinden.
* De SharePoint-serverfarm moet worden geconfigureerd voor het schrijven naar het volume.

> [!NOTE]
> Na het installeren en configureren van de adapter alle BLOB externalization moet gaan via het StorSimple-apparaat (het apparaat wordt de volumes voor SQL Server en de opslaglagen beheren). U kunt andere doelen niet gebruiken voor BLOB externalization.


Als u van plan bent StorSimple Snapshot Manager om momentopnamen van de BLOB- en gegevens te gebruiken, moet u StorSimple Snapshot Manager installeren op de databaseserver, zodat deze de SQL Writer Service gebruiken kunt voor het implementeren van Windows Volume Shadow Copy Service (VSS).

> [!IMPORTANT]
> StorSimple Snapshot Manager biedt geen ondersteuning voor de SharePoint VSS Writer en toepassingsconsistente momentopnamen kan niet van de SharePoint-gegevens. In een SharePoint-scenario biedt StorSimple Snapshot Manager alleen crashconsistente back-ups.


## <a name="sharepoint-farm-configuration-prerequisites"></a>Configuratievereisten voor SharePoint-farm
Zorg ervoor dat uw SharePoint-serverfarm correct is geconfigureerd, als volgt:

* Controleer of uw SharePoint-serverfarm is in orde en controleert u het volgende:
* Alle SharePoint WFE en toepassingsservers geregistreerd in de farm worden uitgevoerd en van de server waarop u de StorSimple-Adapter voor SharePoint installeert kunnen worden gepingd.
* De SharePoint Timer-service (SPTimerV3 of SPTimerV4) wordt uitgevoerd op elke WFE-server en de toepassingsserver.
* Zowel de IIS-toepassingsgroep waaronder de centrale beheersite van SharePoint wordt uitgevoerd als de SharePoint Timer-service hebt beheerdersbevoegdheden nodig.
* Zorg ervoor dat Internet Explorer Verbeterde beveiligingscontext (IE ESC) is uitgeschakeld. Volg deze stappen voor IE ESC uitschakelen:
  
  1. Sluit alle exemplaren van Internet Explorer.
  2. Start de Server Manager.
  3. Klik in het linkerdeelvenster op **lokale Server**.
  4. Klik in het rechterdeelvenster, naast **verbeterde beveiliging van Internet Explorer**, klikt u op **op**.
  5. Onder **beheerders**, klikt u op **uit**.
  6. Klik op **OK**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Externe vereisten voor BLOB Storage planning
Zorg ervoor dat u van een ondersteunde versie van SQL Server gebruikmaakt. Alleen de volgende versies worden ondersteund en kunnen gebruikt dit veld:

* SQL Server 2008 Enterprise Edition
* SQL Server 2008 R2 Enterprise Edition
* SQL Server 2012 Enterprise Edition

BLOBs kunnen worden externalized voor alleen volumes die het StorSimple-apparaat aan SQL Server biedt. Er zijn geen andere doelen voor BLOB externalization worden ondersteund.

Als u alle vereiste configuratiestappen hebt voltooid, gaat u naar [de StorSimple-Adapter installeren voor SharePoint](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>De StorSimple-Adapter installeren voor SharePoint
Gebruik de volgende stappen uit de StorSimple-Adapter installeren voor SharePoint. Als u de software opnieuw installeert, raadpleegt u [bijwerken of opnieuw installeren van de StorSimple-Adapter voor SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). De tijd die nodig zijn voor de installatie, is afhankelijk van het totale aantal SharePoint-databases in de SharePoint-serverfarm.

[!INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]

## <a name="configure-rbs"></a>Resourcestructuur configureren
Nadat u het StorSimple-Adapter voor SharePoint installeert, configureert u Resourcestructuur zoals beschreven in de volgende procedure.

> [!TIP]
> De StorSimple-Adapter voor SharePoint kunt aansluiten op de pagina SharePoint Centraal beheer zodat Resourcestructuur moet worden ingeschakeld of uitgeschakeld op elke inhoudsdatabase in de SharePoint-farm. Echter, in- of uitschakelen van Resourcestructuur op de inhoud van de database wordt een IIS opnieuw instellen, die, afhankelijk van uw farmconfiguratie kan tijdelijk worden verstoren de beschikbaarheid van de SharePoint-webfront-end (WFE). (Factoren zoals het gebruik van een front-end load balancer, de huidige serverwerkbelasting, enzovoort, kunnen beperken of deze onderbreking elimineren.) Als u wilt voorkomen dat gebruikers een onderbreking van, het is raadzaam dat u in- of uitschakelen van Resourcestructuur alleen tijdens een geplande onderhoudsvenster.


[!INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]

## <a name="configure-garbage-collection"></a>Garbagecollection configureren
Wanneer u objecten uit een SharePoint-site worden verwijderd, worden ze niet automatisch uit de store Resourcestructuur volume verwijderd. In plaats daarvan een asynchrone, onderhoudsprogramma achtergrond verwijderen van zwevende BLOBs uit het archief. Dit proces periodiek wordt uitgevoerd door systeembeheerders kunnen plannen of ze deze indien nodig kunnen starten.

Dit onderhoudsprogramma (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) wordt automatisch geïnstalleerd op alle SharePoint WFE-servers en toepassingsservers, wanneer u Resourcestructuur inschakelt. Het programma is geïnstalleerd op de volgende locatie: *opstartschijf*: \Program Files\Microsoft externe SQL-blobopslag 10.50\Maintainer\

Zie voor meer informatie over het configureren en gebruiken van het onderhoudsprogramma [Resourcestructuur onderhouden in SharePoint Server 2013][8].

> [!IMPORTANT]
> Het programma van de maintainer Resourcestructuur is veel bronnen. U moet plannen om alleen tijdens perioden met een lichte activiteit wordt uitgevoerd op de SharePoint-farm.


### <a name="delete-orphaned-blobs-immediately"></a>Zwevende BLOBs onmiddellijk verwijderen
Als u zwevende BLOBs onmiddellijk te verwijderen moet, kunt u de volgende instructies. Houd er rekening mee dat deze instructies een voorbeeld zijn van hoe u dit in een SharePoint 2013-omgeving met de volgende onderdelen doen kunt:

* De naam van de inhoud van de database is WSS_Content.
* De naam van de SQL Server is SHRPT13 SQL12\SHRPT13.
* Naam van de webtoepassing is SharePoint: 80.

[!INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]

## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>Bijwerken of opnieuw installeren van de StorSimple-Adapter voor SharePoint
Gebruik de volgende procedure voor SharePoint-server bijwerken en opnieuw StorSimple-Adapter installeren voor SharePoint of voor gewoon upgraden of opnieuw installeren van de adapter in een bestaande SharePoint-serverfarm.

> [!IMPORTANT]
> Controleer de volgende informatie voordat u probeert bij te werken van uw SharePoint-software en/of de upgrade of de StorSimple-Adapter installeren voor SharePoint:
> 
> * Bestanden die eerder zijn verplaatst naar een externe opslag via de Resourcestructuur zijn is niet beschikbaar totdat de installatie is voltooid en de functie Resourcestructuur weer is ingeschakeld. Uitvoeren als u wilt beperken impact van de gebruiker, een upgrade of herinstallatie tijdens een geplande onderhoudsvenster.
> * De tijd die nodig zijn voor de upgrade/opnieuw installeren kan variëren, afhankelijk van het totale aantal SharePoint-databases in de SharePoint-serverfarm.
> * Nadat de upgrade/installatie voltooid is, moet u Resourcestructuur inschakelen voor de inhoudsdatabases. Zie [Resourcestructuur configureren](#configure-rbs) voor meer informatie.
> * Als u Resourcestructuur configureert voor een SharePoint-farm met een zeer groot aantal databases (groter dan 200), de **SharePoint Centraal beheer** pagina mogelijk time-out. Als dit het geval is, wordt de pagina vernieuwen. Dit heeft geen invloed op het configuratieproces.


[!INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]

## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple-Adapter voor SharePoint verwijderen
De volgende procedures wordt beschreven hoe de BLOBs verplaatst naar de SQL Server-inhoudsdatabases en verwijder de StorSimple-Adapter voor SharePoint. 

> [!IMPORTANT]
> U moet de BLOBs terug verplaatsen naar de inhoudsdatabases voordat u de adaptersoftware verwijderen.


### <a name="before-you-begin"></a>Voordat u begint
De volgende informatie verzamelen voordat u de gegevens naar de SQL Server-inhoudsdatabases verplaatst en beginnen met het verwijderingsproces adapter:

* De namen van alle databases waarvoor Resourcestructuur is ingeschakeld
* Het UNC-pad van de geconfigureerde BLOB-opslag

### <a name="move-the-blobs-back-to-the-content-databases"></a>De BLOBs verplaatst naar de inhoudsdatabases
Voordat u de StorSimple-Adapter voor SharePoint-software verwijdert, moet u alle BLOBs die zijn externalized terug naar de SQL Server-inhoudsdatabases migreren. Als u probeert te verwijderen van de StorSimple-Adapter voor SharePoint voordat u alle BLOBs terug naar de inhoudsdatabases verplaatsen, ziet u de volgende waarschuwing weergegeven.

![Waarschuwingsbericht](./media/storsimple-adapter-for-sharepoint/sasp1.png)

#### <a name="to-move-the-blobs-back-to-the-content-databases"></a>De BLOBs terug verplaatsen naar de inhoudsdatabases
1. Elk van de externalized objecten downloaden.
2. Open de **SharePoint Centraal beheer** pagina en blader naar **systeeminstellingen**.
3. Onder **Azure StorSimple**, klikt u op **StorSimple-Adapter configureren**.
4. Op de **StorSimple-Adapter configureren** pagina, klikt u op de **uitschakelen** knop onder elk van de inhoudsdatabases die u wilt verwijderen uit externe BLOB-opslag. 
5. Verwijder de objecten van SharePoint en vervolgens opnieuw uploaden.

U kunt ook de Microsoft gebruiken` RBS Migrate()` PowerShell-cmdlet die deel uitmaakt van SharePoint. Zie voor meer informatie [inhoud migreert van of naar Resourcestructuur](https://technet.microsoft.com/library/ff628255.aspx).

Nadat u de BLOBs terug naar de inhoud van de database verplaatst, gaat u naar de volgende stap: [verwijderen van de adapter](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>De adapter verwijderen
Nadat u de BLOBs terug naar de SQL Server-inhoudsdatabases verplaatsen, gebruikt u een van de volgende opties voor het verwijderen van de StorSimple-Adapter voor SharePoint.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Het installatieprogramma gebruiken om te verwijderen van de adapter
1. Gebruik een account met administrator-bevoegdheden voor aanmelding bij de webserver voor de front-(WFE).
2. Dubbelklik op de StorSimple-Adapter voor SharePoint-installatieprogramma. De Wizard Setup wordt gestart.
   
    ![Wizard Setup](./media/storsimple-adapter-for-sharepoint/sasp2.png)
3. Klik op **Volgende**. De volgende pagina wordt weergegeven.
   
    ![Setup-wizardpagina verwijderen](./media/storsimple-adapter-for-sharepoint/sasp3.png)
4. Klik op **verwijderen** het verwijderingsproces te selecteren. De volgende pagina wordt weergegeven.
   
    ![Bevestigingspagina voor Setup-wizard](./media/storsimple-adapter-for-sharepoint/sasp4.png)
5. Klik op **verwijderen** de verwijdering te bevestigen. De volgende pagina van de voortgang wordt weergegeven.
   
    ![Voortgangspagina van Setup-wizard](./media/storsimple-adapter-for-sharepoint/sasp5.png)
6. Wanneer het verwijderen voltooid is, verschijnt de voltooiingspagina. Klik op **voltooien** naar de Wizard Setup afsluit.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Het Configuratiescherm gebruiken om te verwijderen van de adapter
1. Open het Configuratiescherm en klik vervolgens op **programma's en onderdelen**.
2. Selecteer **StorSimple-Adapter voor SharePoint**, en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx

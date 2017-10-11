<!--author=SharS last changed: 9/17/15-->

### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>Upgrade van SharePoint 2010 naar SharePoint 2013 en vervolgens de StorSomple-Adapter installeren voor SharePoint
> [!IMPORTANT]
> Bestanden die eerder zijn verplaatst naar een externe opslag via de Resourcestructuur zijn is niet beschikbaar totdat de upgrade is voltooid en de functie Resourcestructuur weer is ingeschakeld. Uitvoeren als u wilt beperken impact van de gebruiker, een upgrade of herinstallatie tijdens een geplande onderhoudsvenster.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>SharePoint 2010 upgraden naar SharePoint 2013 en installeer vervolgens de adapter
1. Noteer het pad van de store BLOB voor de externalized BLOBs en de inhoudsdatabases waarvoor Resourcestructuur is ingeschakeld in de farm SharePoint 2010. 
2. Installeer en configureer het nieuwe SharePoint 2013-farm. 
3. Databases, toepassingen en siteverzamelingen verplaatsen van de SharePoint 2010-farm aan de nieuwe SharePoint 2013-farm. Voor instructies, gaat u naar [overzicht van het upgradeproces voor SharePoint 2013](https://technet.microsoft.com/library/cc262483.aspx).
4. De StorSimple-Adapter installeren voor SharePoint op de nieuwe farm. Ga naar [de StorSimple-Adapter installeren voor SharePoint](#install-the-storsimple-adapter-for-sharepoint) voor procedures.
5. Gebruik de informatie die u in stap 1 hebt genoteerd en Resourcestructuur inschakelen voor dezelfde reeks inhoudsdatabases en geef dezelfde BLOB store pad dat is gebruikt in de SharePoint 2010-installatie. Ga naar [Resourcestructuur configureren](#configure-rbs) voor procedures. Nadat u deze stap hebt voltooid, is eerder externalized bestanden moet toegankelijk is vanaf de nieuwe farm. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>Upgrade de StorSimple-Adapter voor SharePoint
> [!IMPORTANT]
> U moet plannen dat deze upgrade voordoen tijdens een geplande onderhoudsvenster voor de volgende redenen:
> 
> * Eerder zich externalized inhoud niet beschikbaar totdat de adapter wordt opnieuw geïnstalleerd.
> * Alle inhoud die is geüpload naar de site wanneer u de vorige versie van de StorSimple-Adapter voor SharePoint hebt verwijderd, maar voordat u de nieuwe versie installeert worden opgeslagen in de inhoudsdatabase. U moet deze inhoud verplaatsen naar het StorSimple-apparaat nadat u de nieuwe adapter. U kunt de Microsoft` RBS Migrate()` PowerShell-cmdlet die deel uitmaakt van SharePoint voor het migreren van de inhoud. Zie voor meer informatie [inhoud migreert van of naar Resourcestructuur](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>De StorSimple-Adapter voor SharePoint bijwerken
1. Verwijder de vorige versie van StorSimple-Adapter voor SharePoint.
   
   > [!NOTE]
   > Hiermee worden automatisch Resourcestructuur uitgeschakeld op de inhoudsdatabases. Bestaande BLOBs blijft echter op het StorSimple-apparaat. Omdat Resourcestructuur is uitgeschakeld en de BLOBs niet zijn gemigreerd naar de inhoudsdatabases, mislukken alle aanvragen voor deze BLOBs. 
   > 
   > 
2. De nieuwe StorSimple-Adapter installeren voor SharePoint. De nieuwe adapter automatisch de inhoudsdatabases die eerder zijn ingeschakeld of uitgeschakeld voor Resourcestructuur herkent en gebruikt de eerdere instellingen.


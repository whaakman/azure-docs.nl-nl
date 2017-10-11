<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> Als u wijzigingen aanbrengt aan de StorSimple-Adapter voor de configuratie van SharePoint RBS, moet u zijn aangemeld met een gebruikersaccount die deel uitmaakt van de groep Domeinadministrators. Bovendien moet u toegang tot de configuratiepagina van een browser op dezelfde host als een Centraalbeheersite.
> 
> 

#### <a name="to-configure-rbs"></a>Resourcestructuur configureren
1. Open de pagina Centraal beheer van SharePoint en blader naar **systeeminstellingen**. 
2. In de **Azure StorSimple** sectie, klikt u op **StorSimple-Adapter configureren**.
   
    ![Configureer de StorSimple-Adapter](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. Op de **StorSimple-Adapter configureren** pagina:
   
   1. Zorg ervoor dat de **inschakelen bewerken pad** selectievakje is ingeschakeld.
   2. Typ het pad Universal Naming Convention (UNC) van de BLOB-opslag in het tekstvak.
      
      > [!NOTE]
      > De BLOB-store-volume moet worden gehost op een iSCSI-volume dat is geconfigureerd op het StorSimple-apparaat.

   3. Klik op de **inschakelen** knop onder elk van de inhoudsdatabases die u wilt configureren voor externe opslag.
      
      > [!NOTE]
      > De BLOB-opslag moet worden gedeeld en bereikbaar door alle web-front-(WFE)-servers en de gebruikersaccount die is geconfigureerd voor de SharePoint-serverfarm moet toegang hebben tot de share.
      
      ![De provider Resourcestructuur inschakelen](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      Wanneer u in- of uitschakelen van Resourcestructuur, ziet u ook het volgende bericht.
      
      ![Uitschakelen van StorSimple Adapter configureren](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. Klik op de **Update** om de configuratie toepassen. Wanneer u klikt op de **Update** knop klikt, wordt de status van de configuratie Resourcestructuur bijgewerkt op alle WFE-servers en de gehele farm worden Resourcestructuur ingeschakeld. Het volgende bericht wordt weergegeven.
      
      ![Adapter configuratiebericht](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > Als u Resourcestructuur configureert voor een SharePoint-farm met een zeer groot aantal databases (groter dan 200), kan de Centraal beheer van SharePoint-webpagina time-out. Als dit het geval is, wordt de pagina vernieuwen. Dit heeft geen invloed op het configuratieproces.

4. Controleer of de configuratie:
   
   1. Meld u aan bij de website Centraal beheer van SharePoint en blader naar de **StorSimple-Adapter configureren** pagina.
   2. Controleer de configuratiedetails om ervoor te zorgen dat ze overeenkomen met de instellingen die u hebt ingevoerd. 
5. Controleer of Resourcestructuur correct werkt:
   
   1. Een document uploaden naar SharePoint. 
   2. Blader naar het UNC-pad dat u hebt geconfigureerd. Zorg ervoor dat de mapstructuur Resourcestructuur is gemaakt en dat deze het geüploade object bevat.
6. (Optioneel) U kunt de Microsoft-RBS `Migrate()` PowerShell-cmdlet die deel uitmaakt van SharePoint voor het migreren van bestaande BLOB-inhoud naar het StorSimple-apparaat. Zie voor meer informatie [inhoud migreert van of naar Resourcestructuur in SharePoint 2013] [ 6] of [inhoud migreert van of naar Resourcestructuur (SharePoint Foundation 2010)] [7].
7. (Optioneel) Bij installaties van de test, kunt u controleren dat de BLOBs als volgt buiten de inhoud van de database zijn verplaatst: 
   
   1. Start SQL Management Studio.
   2. De query ListBlobsInDB_2010.sql of ListBlobsInDB_2013.sql als volgt uitvoeren.
      
      ```
      **ListBlobsInDB_2013.sql**
      
        USE WSS_Content
        GO
      
        SELECT DocStreams.DocId,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               DocStreams.RbsId,
               TimeLastModified
      
        FROM DocStreams
      
             INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      
      **ListBlobsInDB_2010.sql**
      
        USE WSS_Content
        GO
      
        SELECT AllDocStreams.Id,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               RbsId,
               TimeLastModified
        FROM AllDocStreams
      
             INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      ```
      
      Als Resourcestructuur correct is geconfigureerd, kan een NULL-waarde moet worden weergegeven in de kolom SizeOfContentInDB voor elk object dat is geüpload en met succes externalized met Resourcestructuur.
8. (Optioneel) Nadat u Resourcestructuur configureert en alle BLOB-inhoud naar het StorSimple-apparaat verplaatsen, kunt u de inhoud van de database kunt verplaatsen naar het apparaat. Als u kiest voor het verplaatsen van de inhoud van de database, raden wij u aan de inhoud van de database-opslag te configureren op het apparaat als primaire volume. Gebruik vervolgens tot stand gebracht aanbevolen procedures voor SQL Server voor het migreren van de inhoud van de database op het StorSimple-apparaat. 
   
   > [!NOTE]
   > De inhoud van de database te verplaatsen naar het apparaat wordt alleen ondersteund voor de StorSimple 8000 serie (dit wordt niet ondersteund voor de 5000 of 7000-serie).
   
   Als u BLOBs en de inhoud van de database in afzonderlijke volumes op het StorSimple-apparaat opslaat, raden wij u ze configureren in dezelfde volumecontainer. Dit zorgt ervoor dat er wordt een back-samen.
   
   > [!WARNING]
   > Als u Resourcestructuur niet hebt ingeschakeld, niet aangeraden de inhoudsdatabase te verplaatsen naar de StorSimple-apparaat. Dit is een niet-geteste configuratie.
   
9. Ga naar de volgende stap: [garbagecollection configureren](#configure-garbage-collection).

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx

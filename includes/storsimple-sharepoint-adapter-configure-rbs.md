---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 3888242f0379cc97bbe511e49a31a0f7eb8c5372
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166050"
---
<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> Als u wijzigingen aanbrengt aan de StorSimple-Adapter voor SharePoint RBS configuratie, moet u zijn aangemeld met een gebruikersaccount dat deel uitmaakt van de groep Domeinadministrators. Bovendien moet u toegang tot de configuratiepagina vanuit een browser die wordt uitgevoerd op dezelfde host als de centrale beheersite.
> 
> 

#### <a name="to-configure-rbs"></a>Resourcestructuur configureren
1. Open de pagina Centraal beheer van SharePoint en blader naar **systeeminstellingen**. 
2. In de **Azure StorSimple** sectie, klikt u op **StorSimple-Adapter configureren**.
   
    ![De StorSimple-Adapter configureren](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. Op de **StorSimple-Adapter configureren** pagina:
   
   1. Zorg ervoor dat de **inschakelen bewerken pad** selectievakje is ingeschakeld.
   2. Typ in het tekstvak de Universal Naming Convention (UNC)-pad van de BLOB-archief.
      
      > [!NOTE]
      > De BLOB store-volume moet worden gehost op een iSCSI-volume dat is geconfigureerd op het StorSimple-apparaat.

   3. Klik op de **inschakelen** knop onder elk van de inhoudsdatabases die u wilt configureren voor externe opslag.
      
      > [!NOTE]
      > De BLOB-archief moet worden gedeeld en bereikbaar is door alle web-front-end (WFE)-servers en het gebruikersaccount dat is geconfigureerd voor de SharePoint-serverfarm moet toegang hebben tot de share.
      
      ![De provider Resourcestructuur inschakelen](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      Wanneer u in- of uitschakelen van Resourcestructuur, ziet u ook het volgende bericht weergegeven.
      
      ![Uitschakelen van StorSimple-Adapter configureren](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. Klik op de **Update** knop om toe te passen van de configuratie. Wanneer u klikt op de **Update** knop klikt, wordt de status van de configuratie Resourcestructuur bijgewerkt op alle WFE-servers en de hele farm worden Resourcestructuur ingeschakeld. Het volgende bericht wordt weergegeven.
      
      ![Adapter configuratiebericht](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > Als u Resourcestructuur configureert voor een SharePoint-farm met een zeer groot aantal databases (meer dan 200), kan de Centraal beheer van SharePoint-webpagina time-out. Als dat gebeurt, vernieuw de pagina. Dit heeft geen invloed op het configuratieproces.

4. Controleer of de configuratie:
   
   1. Meld u aan bij de website Centraal beheer van SharePoint en blader naar de **StorSimple-Adapter configureren** pagina.
   2. Controleer de informatie over de configuratie om ervoor te zorgen dat ze overeenkomen met de instellingen die u hebt ingevoerd. 
5. Controleer of Resourcestructuur correct werkt:
   
   1. Een document uploaden naar SharePoint. 
   2. Blader naar het UNC-pad dat u hebt geconfigureerd. Zorg ervoor dat de mapstructuur Resourcestructuur is gemaakt en dat deze het geüploade object bevat.
6. (Optioneel) U kunt de Microsoft-RBS `Migrate()` PowerShell-cmdlet die deel uitmaakt van SharePoint om te migreren van bestaande BLOB-inhoud naar het StorSimple-apparaat. Zie voor meer informatie, [inhoud migreren naar of uit Resourcestructuur in SharePoint 2013] [ 6] of [inhoud migreren naar of uit Resourcestructuur (SharePoint Foundation 2010)] [7].
7. (Optioneel) Op de test-installaties, kunt u controleren dat de BLOBs zijn verplaatst uit de inhoud van de database als volgt: 
   
   1. Start SQL Management Studio.
   2. Voer de query ListBlobsInDB_2010.sql of ListBlobsInDB_2013.sql als volgt uit.
      
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
      
      Als Resourcestructuur correct is geconfigureerd, kan een NULL-waarde moet worden weergegeven in de kolom SizeOfContentInDB voor elk object dat is geüpload en is externalized met Resourcestructuur.
8. (Optioneel) Nadat u Resourcestructuur configureert en alle BLOB-inhoud naar het StorSimple-apparaat verplaatsen, kunt u de inhoud van de database verplaatsen naar het apparaat. Als u ervoor kiest om te verplaatsen van de inhoud van de database, raden wij u aan de inhoud van de database-opslag te configureren op het apparaat als een primaire volume. Gebruik vervolgens tot stand gebracht aanbevolen procedures voor SQL Server voor het migreren van de inhoud van de database met de StorSimple-apparaat. 
   
   > [!NOTE]
   > De inhoud van de database te verplaatsen naar het apparaat wordt alleen ondersteund voor de StorSimple 8000-serie (dit wordt niet ondersteund voor de 5000 of 7000-serie).
   
   Als u BLOBs en de inhoud van de database in afzonderlijke volumes op het StorSimple-apparaat opgeslagen, wordt u aangeraden dat u ze in dezelfde volumecontainer configureren. Dit zorgt ervoor dat ze worden back-ups samen.
   
   > [!WARNING]
   > Als u Resourcestructuur niet hebt ingeschakeld, niet aangeraden de inhoud van de database verplaatsen naar het StorSimple-apparaat. Dit is een niet-geteste configuratie.
   
9. Ga naar de volgende stap: [garbagecollection configureren](#configure-garbage-collection).

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx

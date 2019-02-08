---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 0b5d9deacdd4266da30f17c95b6e575a652d2f76
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55889419"
---
In deze procedure maakt u het volgende doen:

1. [Voorbereiden om uit te voeren van het uitvoerbare bestand van de Maintainer](#to-prepare-to-run-the-maintainer) .
2. [De inhoud van de database en de Prullenbak voorbereiden voor onmiddellijke verwijdering van zwevende BLOBs](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Voer Maintainer.exe](#to-run-the-maintainer).
4. [Herstellen van de inhoud van de database en de instellingen voor Prullenbak](#to-revert-the-content-database-and-recycle-bin-settings).

#### <a name="to-prepare-to-run-the-maintainer"></a>Om voor te bereiden om uit te voeren van de Maintainer
1. Open de SharePoint 2013-beheershell als beheerder op de front-endwebserver.
2. Navigeer naar de map *opstartschijf*: \Program Files\Microsoft 10.50\Maintainer externe SQL-Blob-opslag\.
3. Wijzig de naam van **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** naar **web.config**.
4. Gebruik `aspnet_regiis -pdf connectionStrings` voor het ontsleutelen van het bestand web.config.
5. In het ontsleutelde web.config-bestand, onder de `connectionStrings` knooppunt, de verbindingsreeks voor uw SQL server-exemplaar en de naam van de inhoud van de database toevoegen. Zie het volgende voorbeeld
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Gebruik `aspnet_regiis –pef connectionStrings` voor het opnieuw versleutelen van het bestand web.config. 
7. De naam van web.config Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config. 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a>Als u wilt voorbereiden van de inhoud zwevende-database en de Prullenbak onmiddellijk verwijderen van BLOBs
1. Op de SQL-Server in SQL Management Studio de volgende update query's uitvoeren voor de doeldatabase voor inhoud: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. Op de front-endwebserver onder **centrale beheersite**, bewerken de **algemene instellingen van webtoepassing** voor de gewenste inhoud van de database tijdelijk uit te schakelen de Prullenbak. Deze actie wordt ook de Prullenbak voor alle siteverzamelingen verwante. Om dit te doen, klikt u op **centrale beheersite** -> **Toepassingsbeheer** -> **Web-Apps (web-apps beheren)**  ->  **SharePoint - 80** -> **algemene toepassingsinstellingen**. Stel de **Status van de Prullenbak** naar **OFF**.
   
    ![Algemene instellingen van webtoepassing](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a>Om uit te voeren van de Maintainer
* Op de front-end-webserver, in de SharePoint 2013-beheershell, voer de Maintainer als volgt uit:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Alleen de `GarbageCollection` bewerking wordt ondersteund voor StorSimple op dit moment. Houd er ook rekening mee dat de parameters die zijn uitgegeven voor Microsoft.Data.SqlRemoteBlobs.Maintainer.exe hoofdlettergevoelig zijn. 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a>Om terug te zetten van de inhoud van de database en de instellingen voor Prullenbak
1. Op de SQL-Server in SQL Management Studio de volgende update query's uitvoeren voor de doeldatabase voor inhoud:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Op de front-endwebserver in **centrale beheersite**, bewerken de **algemene instellingen van webtoepassing** voor de gewenste inhoud van de database opnieuw inschakelen van de Prullenbak. Om dit te doen, klikt u op **centrale beheersite** -> **Toepassingsbeheer** -> **Web-Apps (web-apps beheren)**  ->  **SharePoint - 80** -> **algemene toepassingsinstellingen**. Zet de Status van Recycle Bin op **ON**.


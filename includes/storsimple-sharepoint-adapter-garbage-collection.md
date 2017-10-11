<!--author=SharS last changed: 9/17/15-->

U wordt in deze procedure:

1. [Voorbereiden om uit te voeren uitvoerbare bestand van de Maintainer](#to-prepare-to-run-the-maintainer) .
2. [De inhoud van de database en de Prullenbak voorbereiden voor onmiddellijke verwijdering van zwevende BLOBs](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Voer Maintainer.exe](#to-run-the-maintainer).
4. [Herstellen van de inhoud van de database en de instellingen van de Prullenbak](#to-revert-the-content-database-and-recycle-bin-settings).

#### <a name="to-prepare-to-run-the-maintainer"></a>Voorbereiden om uit te voeren de Maintainer
1. Open de SharePoint 2013-beheershell als beheerder op de front-end-webserver.
2. Navigeer naar de map *opstartschijf*: \Program Files\Microsoft 10.50\Maintainer SQL externe Blob-opslag\.
3. Wijzig de naam van **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** naar **web.config**.
4. Gebruik `aspnet_regiis -pdf connectionStrings` voor het ontsleutelen van het bestand web.config.
5. In het bestand web.config te ontsleutelen onder de `connectionStrings` knooppunt toevoegen van de verbindingsreeks voor de SQL server-exemplaar en de naam van de inhoud van de database. Zie het volgende voorbeeld
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Gebruik `aspnet_regiis –pef connectionStrings` opnieuw versleutelen van het bestand web.config. 
7. De naam van web.config Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config. 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a>Als u wilt de inhoud voorbereiden zwevende database en de Prullenbak worden onmiddellijk verwijderd BLOBs
1. Voer op de SQL-Server in SQL Management Studio de volgende update-query's voor de doel-inhoudsdatabase: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. Op de front-endwebserver onder **Centraal beheer**, bewerk de **algemene instellingen van webtoepassing** voor de gewenste inhoudsdatabase tijdelijk uitschakelen van de Prullenbak. Deze actie wordt ook de Prullenbak voor alle siteverzamelingen verwante. Om dit te doen, klikt u op **Centraal beheer** -> **Toepassingsbeheer** -> **webtoepassingen (web-apps beheren)**  ->  **SharePoint - 80** -> **algemene toepassingsinstellingen**. Stel de **Status van de Prullenbak** naar **OFF**.
   
    ![Algemene instellingen van webtoepassing](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a>De Maintainer uitvoeren
* Op de front-endwebserver in SharePoint 2013-beheershell de Maintainer als volgt uitvoeren:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Alleen de `GarbageCollection` bewerking voor StorSimple wordt ondersteund op dit moment. Let ook op dat de parameters die zijn uitgegeven voor Microsoft.Data.SqlRemoteBlobs.Maintainer.exe hoofdlettergevoelig zijn. 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a>De inhoud van de database en de Prullenbak-instellingen herstellen
1. Voer op de SQL-Server in SQL Management Studio de volgende update-query's voor de doel-inhoudsdatabase:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Op de front-endwebserver in **Centraal beheer**, bewerk de **algemene instellingen van webtoepassing** voor de gewenste inhoud van de database opnieuw inschakelen van de Prullenbak. Om dit te doen, klikt u op **Centraal beheer** -> **Toepassingsbeheer** -> **webtoepassingen (web-apps beheren)**  ->  **SharePoint - 80** -> **algemene toepassingsinstellingen**. De Status van Recycle Bin instellen op **ON**.


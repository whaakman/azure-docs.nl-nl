---
title: Een service gesplitste samenvoegen implementeren | Microsoft Docs
description: Splitsen en samenvoegen met hulpprogramma's voor elastische database
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 9a993c0f-7052-46cd-aa59-073bea8d535a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 6e2fea882c248fa095a9d450ed54a7b4e64b45e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-split-merge-service"></a>Een service voor splitsen en samenvoegen implementeren
De splitsing merge-hulpprogramma kunt u gegevens verplaatsen tussen shard-databases. Zie [verplaatsen van gegevens tussen cloud uitgebreide databases](sql-database-elastic-scale-overview-split-and-merge.md)

## <a name="download-the-split-merge-packages"></a>Downloaden van de pakketten gesplitste samenvoegen
1. Download de nieuwste versie van de NuGet van [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).
2. Open een opdrachtprompt en navigeer naar de map waar u nuget.exe gedownload. De download bevat PowerShell commmands.
3. Download het pakket van de meest recente gesplitste samenvoegen in de huidige map met de onderstaande opdracht:
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

De bestanden worden geplaatst in een map met de naam **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** waar *x.x.xxx.x* weerspiegelt het versienummer. Vinden van de bestanden van de Service gesplitste samenvoegen in de **content\splitmerge\service** onderliggende map en de splitsing samenvoegen PowerShell-scripts (en vereiste client-dll's) in de **content\splitmerge\powershell** onderliggende map.

## <a name="prerequisites"></a>Vereisten
1. Maak een Azure SQL DB-database die wordt gebruikt als de status van de gesplitste samenvoegen-database. Ga naar de [Azure Portal](https://portal.azure.com). Maak een nieuwe **SQL-Database**. Geef een naam op voor de database en maak een nieuwe beheerder en het wachtwoord. Zorg ervoor dat voor het vastleggen van de naam en het wachtwoord voor later gebruik.
2. Zorg ervoor dat uw Azure SQL DB-server kan Azure-Services tot stand te brengen. In de portal in de **firewallinstellingen**, zorg ervoor dat de **toegang tot Azure-Services toestaan** is ingesteld op **op**. Klik op het pictogram 'opslaan'.
   
   ![Toegestane services][1]
3. Maak een Azure Storage-account dat wordt gebruikt voor diagnostische uitvoer. Ga naar de Azure-Portal. Klik in de linkerbalk op **nieuw**, klikt u op **gegevens en opslag**, klikt u vervolgens **opslag**.
4. Maak een Azure Cloud Service die uw service gesplitste Merge bevat.  Ga naar de Azure-Portal. Klik in de linkerbalk op **nieuw**, klikt u vervolgens **Compute**, **Cloudservice**, en **maken**. 

## <a name="configure-your-split-merge-service"></a>Configureer uw service gesplitste samenvoegen
### <a name="split-merge-service-configuration"></a>Configuratie gesplitste Merge-service
1. In de map waar u de splitsing Merge-assembly's hebt gedownload, maakt u een kopie van de **ServiceConfiguration.Template.cscfg** -bestand dat naast verzonden **SplitMergeService.cspkg** en wijzig deze  **ServiceConfiguration.cscfg**.
2. Open **ServiceConfiguration.cscfg** in een teksteditor zoals Visual Studio die invoer zoals de indeling van certificaatvingerafdrukken valideert.
3. Een nieuwe database maken of kies een bestaande database om te fungeren als de database van de status voor gesplitste samenvoegbewerkingen en de verbindingsreeks van de database ophalen. 
   
   > [!IMPORTANT]
   > Gebruik de Latijnse sortering op dit moment de status van database (SQL\_Latin1\_algemene\_CP1\_CI\_AS). Zie voor meer informatie [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).
   >

   Met Azure SQL DB is de verbindingsreeks meestal van het formulier:
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. Deze verbindingsreeks invoeren in het cscfg-bestand in zowel de **SplitMergeWeb** en **SplitMergeWorker** secties van de rol in de instelling ElasticScaleMetadata.
5. Voor de **SplitMergeWorker** rol, voer een geldige verbindingsreeks naar Azure-opslag voor de **WorkerRoleSynchronizationStorageAccountConnectionString** instelling.

### <a name="configure-security"></a>Beveiliging configureren
Raadpleeg voor gedetailleerde instructies voor het configureren van de beveiliging van de service de [gesplitste samenvoegen Beveiligingsconfiguratie](sql-database-elastic-scale-split-merge-security-configuration.md).

Voor de doeleinden van een eenvoudige test-implementatie voor deze zelfstudie wordt een minimaal aantal configuratiestappen worden uitgevoerd voor de service en wordt uitgevoerd. Deze stappen alleen het één machine/account inschakelen om te communiceren met de service wordt uitgevoerd.

### <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken
Een nieuwe map maken en uitvoeren vanuit deze map de volgende opdracht uit via een [opdrachtprompt voor ontwikkelaars voor Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) venster:

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

U wordt gevraagd om een wachtwoord voor het beveiligen van de persoonlijke sleutel. Voer een sterk wachtwoord in en Bevestig het. U wordt gevraagd om het wachtwoord voor zodra er meer na die worden gebruikt. Klik op **Ja** aan het einde om het te importeren in de vertrouwde certificeringsinstantie instanties basisarchief.

### <a name="create-a-pfx-file"></a>Een PFX-bestand maken
Voer de volgende opdracht uit hetzelfde venster waar makecert is uitgevoerd. Gebruik hetzelfde wachtwoord dat u gebruikt voor het maken van het certificaat:

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-the-client-certificate-into-the-personal-store"></a>Importeer het clientcertificaat in het persoonlijke archief
1. Dubbelklik in Windows Verkenner op **MyCert.pfx**.
2. In de **Wizard Certificaat importeren** Selecteer **huidige gebruiker** en klik op **volgende**.
3. Bevestig het bestandspad en klik op **volgende**.
4. Typ het wachtwoord, laat u **alle uitgebreide eigenschappen bevatten** gecontroleerd en klikt u op **volgende**.
5. Laat **automatisch het certificaatarchief [...] selecteren**  gecontroleerd en klikt u op **volgende**.
6. Klik op **voltooien** en **OK**.

### <a name="upload-the-pfx-file-to-the-cloud-service"></a>Het PFX-bestand uploaden naar de cloudservice
1. Ga naar de [Azure-portal](https://portal.azure.com).
2. Selecteer **Cloudservices**.
3. Selecteer de cloudservice die u hierboven hebt gemaakt voor de splitsing/Merge-service.
4. Klik op **certificaten** in het bovenste menu.
5. Klik op **uploaden** in de balk onderaan.
6. Selecteer het PFX-bestand en voer hetzelfde wachtwoord als hierboven.
7. Als voltooid, kopieert u de vingerafdruk van het certificaat van de nieuwe vermelding in de lijst.

### <a name="update-the-service-configuration-file"></a>Het configuratiebestand van de service bijwerken
Plak de vingerafdruk van het certificaat dat hierboven wordt gekopieerd naar de vingerafdruk kenmerkwaarde van deze instellingen.
Voor de werkrol:
   ```
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Voor de Webrol:

   ```
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Dat de implementaties voor productie certificaten scheiden moet worden gebruikt voor de CA, voor versleuteling, het servercertificaat en clientcertificaten. Zie voor gedetailleerde instructies voor dit [Beveiligingsconfiguratie](sql-database-elastic-scale-split-merge-security-configuration.md).

## <a name="deploy-your-service"></a>Uw service implementeren
1. Ga naar de [Azure Portal](https://manage.windowsazure.com).
2. Klik op de **Cloudservices** tabblad aan de linkerkant en selecteer de cloudservice die u eerder hebt gemaakt.
3. Klik op **Dashboard**.
4. Kies de faseringsomgeving en klik vervolgens op **uploaden van een nieuwe implementatie van de staging**.
   
   ![Fasering][3]
5. Voer een implementatielabel in het dialoogvenster. Klik voor 'Pakket' en 'Configuration', 'Van Local' en kies de **SplitMergeService.cspkg** bestands- en uw cscfg-bestand dat u eerder hebt geconfigureerd.
6. Zorg ervoor dat het selectievakje **toch implementeren als een of meer rollen met één exemplaar** is ingeschakeld.
7. Druk op de knop maatstreepjes in de rechterbenedenhoek om te beginnen met de implementatie. Verwacht een paar minuten duren.

   ![Uploaden][4]

## <a name="troubleshoot-the-deployment"></a>Problemen met de implementatie oplossen
Als uw Webrol niet online komen, is het waarschijnlijk een probleem met de beveiligingsconfiguratie. Controleer of de SSL is geconfigureerd zoals hierboven is beschreven.

Als uw werkrol mislukt online worden gezet, maar de Webrol is geslaagd, is het zeer waarschijnlijk een probleem opgetreden bij het verbinden met de status-database die u eerder hebt gemaakt.

* Zorg ervoor dat de verbindingsreeks in uw cscfg-bestand juist is.
* Controleer of de server en database bestaat en of de gebruikers-id en het wachtwoord juist zijn.
* Voor Azure SQL DB moet de verbindingsreeks van het formulier:

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* Zorg ervoor dat de naam van de server begint niet met **https://**.
* Zorg ervoor dat uw Azure SQL DB-server kan Azure-Services tot stand te brengen. U doet dit door https://manage.windowsazure.com openen, klikt u op 'SQL Databases' aan de linkerkant, klikt u boven 'Servers' en selecteer uw server. Klik op **configureren** aan de bovenkant en zorg ervoor dat de **Azure Services** is ingesteld op "Ja". (Zie de sectie vereisten aan het begin van dit artikel).

## <a name="test-the-service-deployment"></a>De service-implementatie testen
### <a name="connect-with-a-web-browser"></a>Verbinding maken met een webbrowser
Bepaal de web-eindpunt van uw service gesplitste samenvoegen. U kunt dit vinden in de klassieke Azure Portal door te gaan naar de **Dashboard** van uw cloudservice en zoeken onder **Site-URL** aan de rechterkant. Vervang **http://** met **https://** omdat de standaardbeveiligingsinstellingen het HTTP-eindpunt uitschakelen. Laad de pagina voor deze URL in uw browser.

### <a name="test-with-powershell-scripts"></a>Testen met PowerShell-scripts
De implementatie en uw omgeving kunnen worden getest door het uitvoeren van de opgenomen voorbeeld PowerShell-scripts.

De scriptbestanden opgenomen zijn:

1. **SetupSampleSplitMergeEnvironment.ps1** -stelt u een test-gegevenslaag voor gesplitste/Merge (Zie onderstaande tabel voor een gedetailleerde beschrijving)
2. **ExecuteSampleSplitMerge.ps1** -test-bewerkingen op de test wordt uitgevoerd gegevens servicetier (Zie onderstaande tabel voor een gedetailleerde beschrijving)
3. **GetMappings.ps1** - site op het hoogste voorbeeldscript die de huidige status van de shard-toewijzingen wordt afgedrukt.
4. **ShardManagement.psm1** -helper-script waarmee de API ShardManagement verpakt
5. **SqlDatabaseHelpers.psm1** -helper-script voor het maken en beheren van de SQL-databases
   
   <table style="width:100%">
     <tr>
       <th>PowerShell-bestand</th>
       <th>Stappen</th>
     </tr>
     <tr>
       <th rowspan="5">SetupSampleSplitMergeEnvironment.ps1</th>
       <td>1.    Hiermee maakt u een manager-database van de shard-kaart</td>
     </tr>
     <tr>
       <td>2.    2 shard-databases maakt.
     </tr>
     <tr>
       <td>3.    Maakt een shard-toewijzing voor deze database (verwijdert alle bestaande shard op die databases toegewezen). </td>
     </tr>
     <tr>
       <td>4.    Hiermee maakt u een kleine voorbeeldtabel in zowel de shards en vult de tabel in een van de shards.</td>
     </tr>
     <tr>
       <td>5.    Verklaart de SchemaInfo voor de shard-tabel.</td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th>PowerShell-bestand</th>
       <th>Stappen</th>
     </tr>
   <tr>
       <th rowspan="4">ExecuteSampleSplitMerge.ps1 </th>
       <td>1.    Een split-aanvraag verzendt naar de web-frontend gesplitste Merge-Service, die half gegevens van de eerste shard naar de tweede shard splitst.</td>
     </tr>
     <tr>
       <td>2.    De web-frontend voor de status van de gesplitste worden opgevraagd en wacht totdat de aanvraag is voltooid.</td>
     </tr>
     <tr>
       <td>3.    Een merge-aanvraag verzendt naar de frontend web gesplitste Merge-Service, die de gegevens van de tweede shard terug naar de eerste shard.</td>
     </tr>
     <tr>
       <td>4.    De web-frontend voor de status van de aanvraag merge worden opgevraagd en wacht totdat de aanvraag is voltooid.</td>
     </tr>
   </table>
   
## <a name="use-powershell-to-verify-your-deployment"></a>PowerShell gebruiken om uw implementatie te controleren
1. Open een nieuw PowerShell-venster en Ga naar de map waar u het pakket gesplitste samenvoegen hebt gedownload en navigeer vervolgens naar de map 'powershell'.
2. Maak een Azure SQL database-server (of kies een bestaande server) waar de shard-toewijzing manager en shards wordt gemaakt.
   
   > [!NOTE]
   > Het script SetupSampleSplitMergeEnvironment.ps1 maakt alle deze databases op dezelfde server standaard aan het script eenvoudig te houden. Dit is geen beperking van de gesplitste Merge-Service zelf.
   >
   
   Een SQL-aanmelding voor verificatie met lees-/ schrijftoegang tot de databases vereist voor de service gesplitste samenvoegen gegevens verplaatsen en het bijwerken van de shard-toewijzing. Aangezien de splitsing Merge-Service wordt uitgevoerd in de cloud, ondersteunt het momenteel geen geïntegreerde verificatie.
   
   Zorg ervoor dat de Azure SQL-server is geconfigureerd voor toegang van de IP-adres van de machine uitvoeren van deze scripts. U vindt deze instelling onder de Azure SQL-server / configuration / IP-adressen toegestaan.
3. Voer het script SetupSampleSplitMergeEnvironment.ps1 voor het maken van de voorbeeldomgeving.
   
   Dit script uitvoert wordt wissen uit een bestaande shard management kaartgegevens structuren op de shard kaart manager-database en de shards. Kan het zijn handig voor het script opnieuw uitgevoerd als u wilt de shard-kaart of shards opnieuw te initialiseren.
   
   Voorbeeld-opdrachtregel:

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. Voer het script Getmappings.ps1 om weer te geven van de toewijzingen die momenteel aanwezig zijn in de voorbeeldomgeving.
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. Voer het script ExecuteSampleSplitMerge.ps1 voor het uitvoeren van een splitsbewerking (de helft van de gegevens op de eerste shard verplaatst naar de tweede shard) en vervolgens een merge-bewerking (de gegevens terug verplaatsen naar de eerste shard). Als u SSL geconfigureerd en links van het http-eindpunt dat is uitgeschakeld, zorg ervoor dat u gebruikt het eindpunt https://.
   
   Voorbeeld-opdrachtregel:

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   Als u krijgt de onderstaande fout, is het meest waarschijnlijk een probleem met het certificaat van uw Web-eindpunt. Probeer verbinding te maken met de webservice-eindpunt met uw favoriete webbrowser en controleer of er een certificaatfout.
   
     ```
     Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLSsecure channel.
     ```
   
   Als deze is voltooid, de uitvoer moet eruitzien als in het onderstaande:
   
   ```
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C to end
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C to end
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```
6. Experimenteer met andere gegevenstypen! Alle deze scripts duren voordat een optionele - ShardKeyType parameter waarmee u het sleuteltype opgeven. De standaardwaarde is Int32, maar u kunt ook opgeven Int64, Guid of binair.

## <a name="create-requests"></a>Aanvragen maken
De service kan worden gebruikt met behulp van de webgebruikersinterface of te importeren en gebruiken van de SplitMerge.psm1 PowerShell-module die wordt uw aanvragen via de Webrol indienen.

De service kan de gegevens in zowel de shard-tabellen en de verwijzingsdimensies verplaatsen. Een shard-tabel heeft een sharding-sleutelkolom en andere rijgegevens in elke shard heeft. Een tabel is niet shard zodat deze dezelfde rijgegevens in elke shard bevat. Verwijzingsdimensies zijn handig voor gegevens die niet vaak wijzigen en join wordt gebruikt met shard tabellen in query's.

Om een splitsing merge-bewerking uitvoeren, moet u de shard-tabellen en tabellen die u wilt hebt verplaatst declareren. Dit wordt bewerkstelligd met de **SchemaInfo** API. Deze API is in de **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** naamruimte.

1. Voor elke shard-tabel maken van een **ShardedTableInfo** object met een beschrijving van de tabel bovenliggende schemanaam (optioneel, wordt standaard ingesteld op 'dbo'), de tabelnaam en de naam van de kolom in de tabel die de sharding-sleutel bevat.
2. Maak voor elke tabel verwijzing naar een **ReferenceTableInfo** object met een beschrijving van de tabel bovenliggende schemanaam (optioneel, wordt standaard ingesteld op 'dbo') en de naam van de tabel.
3. De bovenstaande TableInfo objecten toevoegen aan een nieuwe **SchemaInfo** object.
4. Geen verwijzing ophalen naar een **ShardMapManager** object en de aanroep **GetSchemaInfoCollection**.
5. Toevoegen de **SchemaInfo** naar de **SchemaInfoCollection**, geven de naam van de shard-toewijzing.

Een voorbeeld hiervan kan worden weergegeven in het script SetupSampleSplitMergeEnvironment.ps1.

De splitsing Merge-service maakt geen de doeldatabase (of het schema voor alle tabellen in de database) voor u. Ze moeten zijn vooraf gemaakte voordat een aanvraag verzonden naar de service.

## <a name="troubleshooting"></a>Problemen oplossen
Mogelijk ziet u de onderstaande bericht bij het uitvoeren van de powershell-voorbeeldscripts:

   ```
   Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.
   ```

Deze fout betekent dat uw SSL-certificaat is niet juist geconfigureerd. Volg de instructies in de sectie 'Verbinding maken met een webbrowser'.

Als u geen aanvragen indienen mogelijk ziet u dit:

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

In dit geval uw configuratiebestand controleren, met name de instelling voor **WorkerRoleSynchronizationStorageAccountConnectionString**. Deze fout geeft meestal aan dat de werkrol niet met succes metagegevensdatabase bij het eerste gebruik initialiseren kan. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png


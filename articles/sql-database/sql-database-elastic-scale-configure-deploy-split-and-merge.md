---
title: Een service voor splitsen en samenvoegen implementeren | Microsoft Docs
description: De splitsen en samenvoegen ook gebruiken om gegevens te verplaatsen tussen de shard-databases.
services: sql-database
ms.service: sql-database
subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 10/05/2018
ms.openlocfilehash: 6fb001f3df78e5f85d8f89e3a45df59c0aec98a1
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48869208"
---
# <a name="deploy-a-split-merge-service-to-move-data-between-sharded-databases"></a>Een service voor het verplaatsen van gegevens tussen de shard-databases voor splitsen en samenvoegen implementeren

Het hulpprogramma voor splitsen en samenvoegen kunt u gegevens verplaatsen tussen shard-databases. Zie [om gegevens te verplaatsen tussen uitgeschaalde clouddatabases](sql-database-elastic-scale-overview-split-and-merge.md)

## <a name="download-the-split-merge-packages"></a>Downloaden van de pakketten splitsen en samenvoegen
1. Download de nieuwste versie van NuGet van [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).
2. Open een opdrachtprompt en navigeer naar de map waar u nuget.exe gedownload. De download bevat onder andere PowerShell-opdrachten.
3. Download de meest recente pakket met splitsen en samenvoegen in de huidige map met de onderstaande opdracht:
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

De bestanden worden geplaatst in een map met de naam **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** waar *x.x.xxx.x* geeft het versienummer. Zoeken naar de bestanden van de Service splitsen en samenvoegen in de **content\splitmerge\service** onderliggende map en de splitsen en samenvoegen PowerShell-scripts (en vereiste client-dll's) in de **content\splitmerge\powershell** onderliggende map.

## <a name="prerequisites"></a>Vereisten
1. Maak een Azure SQL DB-database die wordt gebruikt als de status van splitsen en samenvoegen-database. Ga naar de [Azure Portal](https://portal.azure.com). Maak een nieuwe **SQL-Database**. Geef een naam op voor de database en maak een nieuwe beheerder en het wachtwoord. Zorg ervoor dat de naam en wachtwoord op voor later gebruik.
2. Zorg ervoor dat uw Azure SQL-database-server kan Azure-Services tot stand te brengen. In de portal in de **Firewall-instellingen**, zorg ervoor dat de **toegang tot Azure-Services toestaan** is ingesteld op **op**. Klik op het pictogram 'opslaan'.
3. Maak een Azure-opslagaccount voor diagnostische uitvoer.
4. Maak een Azure-Cloudservice voor uw service voor splitsen en samenvoegen.

## <a name="configure-your-split-merge-service"></a>Configureer uw service voor splitsen en samenvoegen
### <a name="split-merge-service-configuration"></a>Configuratie van de service voor splitsen en samenvoegen
1. In de map waarin u de splitsen en samenvoegen-assembly's gedownload, maakt u een kopie van de **ServiceConfiguration.Template.cscfg** -bestand dat naast verzonden **SplitMergeService.cspkg** en wijzig de naam **ServiceConfiguration.cscfg**.
2. Open **ServiceConfiguration.cscfg** in een teksteditor, zoals Visual Studio die invoer zoals de indeling van de vingerafdrukken van het certificaat worden gevalideerd.
3. Een nieuwe database maken of kies een bestaande database om te fungeren als de database van de status voor splitsen en samenvoegen bewerkingen en de verbindingsreeks van de database op te halen. 
   
   > [!IMPORTANT]
   > Gebruik de Latijnse sortering op dit moment de status van database (SQL\_Latin1\_algemene\_CP1\_CI\_AS). Zie voor meer informatie, [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).
   >

   Met Azure SQL DB is de verbindingsreeks doorgaans van het formulier:
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. Deze verbindingsreeks invoeren in het cscfg-bestand in zowel de **SplitMergeWeb** en **SplitMergeWorker** secties van de rol in de instelling ElasticScaleMetadata.
5. Voor de **SplitMergeWorker** rol, voert u een geldige verbindingsreeks naar Azure storage voor de **WorkerRoleSynchronizationStorageAccountConnectionString** instelling.

### <a name="configure-security"></a>Beveiliging configureren
Raadpleeg voor gedetailleerde instructies voor het configureren van de beveiliging van de service de [beveiligingsconfiguratie splitsen en samenvoegen](sql-database-elastic-scale-split-merge-security-configuration.md).

Voor de toepassing van een eenvoudig test-implementatie voor deze zelfstudie wordt een minimaal aantal configuratiestappen worden uitgevoerd aan de slag van de service en wordt uitgevoerd. Deze stappen schakelt u alleen het één machine /-account om te communiceren met de service wordt uitgevoerd.

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

U wordt gevraagd om een wachtwoord voor het beveiligen van de persoonlijke sleutel. Voer een sterk wachtwoord in en Bevestig het. U wordt vervolgens gevraagd om het wachtwoord moet worden gebruikt zodra er meer hierna. Klik op **Ja** aan het einde te importeren naar het archief Vertrouwde Certification Authorities basis.

### <a name="create-a-pfx-file"></a>Een PFX-bestand maken
De volgende opdracht uitvoeren vanaf hetzelfde venster waar makecert is uitgevoerd. Gebruik hetzelfde wachtwoord dat u gebruikt voor het maken van het certificaat:

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-the-client-certificate-into-the-personal-store"></a>Importeer het clientcertificaat in het persoonlijke archief
1. Dubbelklik in Windows Verkenner op **MyCert.pfx**.
2. In de **Wizard Certificaat importeren** Selecteer **huidige gebruiker** en klikt u op **volgende**.
3. Controleer of het bestandspad en op **volgende**.
4. Typ het wachtwoord, laat u **alle uitgebreide eigenschappen omvatten** ingeschakeld en klik op **volgende**.
5. Laat **selecteert automatisch het certificaatarchief [...]**  ingeschakeld en klik op **volgende**.
6. Klik op **voltooien** en **OK**.

### <a name="upload-the-pfx-file-to-the-cloud-service"></a>Het PFX-bestand uploaden naar de cloudservice
1. Ga naar de [Azure Portal](https://portal.azure.com).
2. Selecteer **Cloudservices**.
3. Selecteer de cloudservice die u hierboven hebt gemaakt voor de service voor splitsen/samenvoegen.
4. Klik op **certificaten** in het bovenste menu.
5. Klik op **uploaden** in de onderste balk.
6. Selecteer het PFX-bestand en het wachtwoord invoeren als hierboven.
7. Als voltooid, kopieert u de certificaatvingerafdruk van het van het nieuwe item in de lijst.

### <a name="update-the-service-configuration-file"></a>Het configuratiebestand van de service bijwerken
Plak de vingerafdruk van het certificaat dat hierboven wordt gekopieerd naar het kenmerk/vingerafdrukwaarde van deze instellingen.
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

. Houd er rekening mee dat de implementaties voor productie certificaten scheiden moet worden gebruikt voor de CA voor versleuteling, het servercertificaat en clientcertificaten. Zie voor gedetailleerde instructies op deze [Beveiligingsconfiguratie](sql-database-elastic-scale-split-merge-security-configuration.md).

## <a name="deploy-your-service"></a>De service implementeert
1. Ga naar [Azure Portal](https://portal.azure.com)
2. Selecteer de cloudservice die u eerder hebt gemaakt.
3. Klik op **Overzicht**.
4. Kies de faseringsomgeving en klik vervolgens op **uploaden**.
5. Voer een implementatielabel in het dialoogvenster. Klik op 'Uit lokale' voor 'Package' en 'Configuration', en kies de **SplitMergeService.cspkg** bestands- en uw cscfg-bestand dat u eerder hebt geconfigureerd.
6. Zorg ervoor dat het selectievakje **toch implementeren als een of meer rollen met één exemplaar** is ingeschakeld.
7. Klik op de knop tikken in de rechterbenedenhoek om te beginnen met de implementatie. Verwacht dat het een paar minuten duren.


## <a name="troubleshoot-the-deployment"></a>De implementatie oplossen
Als uw Webrol niet online is gekomen, is het waarschijnlijk een probleem met de beveiligingsconfiguratie. Controleer of de SSL is geconfigureerd, zoals hierboven beschreven.

Als de werkrol niet online is gekomen, maar uw Webrol is geslaagd, is het zeer waarschijnlijk een probleem opgetreden bij het verbinden met de status-database die u eerder hebt gemaakt.

* Zorg ervoor dat de verbindingsreeks in uw cscfg juist is.
* Controleer of de server en database bestaat, en dat de gebruikers-id en het wachtwoord correct zijn.
* Voor Azure SQL DB moet de verbindingsreeks van het formulier:

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* Zorg ervoor dat de servernaam begint niet met **https://**.
* Zorg ervoor dat uw Azure SQL-database-server kan Azure-Services tot stand te brengen. Om dit te doen, opent u uw database in de portal en zorg ervoor dat de **toegang tot Azure-Services toestaan** is ingesteld op ** op ***.

## <a name="test-the-service-deployment"></a>De service-implementatie testen
### <a name="connect-with-a-web-browser"></a>Verbinding maken met een webbrowser
Bepaal het eindpunt van uw service voor splitsen en samenvoegen op het web. U kunt dit vinden in de portal door te gaan naar de **overzicht** van uw cloudservice en Ga naar **Site-URL** aan de rechterkant. Vervang **http://** met **https://** omdat de standaardinstellingen van de beveiliging het HTTP-eindpunt uitschakelen. Laad de pagina voor deze URL in uw browser.

### <a name="test-with-powershell-scripts"></a>Testen met PowerShell-scripts
De implementatie en uw omgeving kunnen worden getest door het uitvoeren van de opgenomen voorbeeld PowerShell-scripts.

De scriptbestanden zijn:

1. **SetupSampleSplitMergeEnvironment.ps1** -de gegevenslaag van een test voor splitsen en samenvoegen/ingesteld (Zie onderstaande tabel voor een gedetailleerde beschrijving)
2. **ExecuteSampleSplitMerge.ps1** -testbewerkingen op de test wordt uitgevoerd gegevens in lagen (Zie onderstaande tabel voor een gedetailleerde beschrijving)
3. **GetMappings.ps1** : op het hoogste niveau voorbeeldscript die de huidige status van de shard-toewijzingen afgedrukt.
4. **ShardManagement.psm1** -helper-script dat de ShardManagement-API loopt
5. **SqlDatabaseHelpers.psm1** -helper-script voor het maken en beheren van de SQL-databases
   
   <table style="width:100%">
     <tr>
       <th>PowerShell-bestand</th>
       <th>Stappen</th>
     </tr>
     <tr>
       <th rowspan="5">SetupSampleSplitMergeEnvironment.ps1</th>
       <td>1.    Hiermee maakt u een database voor shard-Toewijzingsbeheer</td>
     </tr>
     <tr>
       <td>2.    Hiermee maakt u 2 shard-databases.
     </tr>
     <tr>
       <td>3.    Hiermee maakt u een shard-toewijzing voor deze databases (verwijdert alle bestaande shard-voor deze databases kaarten). </td>
     </tr>
     <tr>
       <td>4.    Hiermee maakt u een tabel van klein in zowel de shards en vult de tabel in een van de shards.</td>
     </tr>
     <tr>
       <td>5.    Verklaart de waarde voor SchemaInfo voor de shard-tabel.</td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th>PowerShell-bestand</th>
       <th>Stappen</th>
     </tr>
   <tr>
       <th rowspan="4">ExecuteSampleSplitMerge.ps1 </th>
       <td>1.    Verzendt een split-aanvraag naar de front-end van de web Service voor splitsen en samenvoegen, die wordt gesplitst halve gegevens van de eerste shard naar de tweede shard.</td>
     </tr>
     <tr>
       <td>2.    De webfrontend voor de status van de gesplitste worden opgevraagd en wacht totdat de aanvraag is voltooid.</td>
     </tr>
     <tr>
       <td>3.    Verzendt een samenvoegaanvraag aan de front-end van de web Service voor splitsen en samenvoegen, die de gegevens van de tweede shard terug naar de eerste shard verplaatst.</td>
     </tr>
     <tr>
       <td>4.    De frontend van de website van de status van de aanvraag samenvoegen worden opgevraagd en wacht totdat de aanvraag is voltooid.</td>
     </tr>
   </table>
   
## <a name="use-powershell-to-verify-your-deployment"></a>PowerShell gebruiken om te controleren of uw implementatie
1. Open een nieuwe PowerShell-venster en navigeer naar de map waar u de splitsen en samenvoegen-pakket hebt gedownload en navigeer vervolgens naar de map 'powershell'.
2. Een Azure SQL database-server (of kies een bestaande server) waar de shard-Toewijzingsbeheer en de shards worden gemaakt.
   
   > [!NOTE]
   > Het script SetupSampleSplitMergeEnvironment.ps1 maakt alle deze databases op dezelfde server standaard aan het script eenvoudig te houden. Dit is geen beperking van de splitsen en samenvoegen-Service zelf.
   >
   
   De aanmelding voor een SQL-verificatie met lees-/ schrijftoegang tot de databases nodig voor de service voor splitsen en samenvoegen voor het verplaatsen van gegevens en de shard-toewijzing bijwerken. Omdat de Service voor splitsen en samenvoegen wordt uitgevoerd in de cloud, ondersteunt het momenteel geen geïntegreerde verificatie.
   
   Zorg ervoor dat de Azure SQL-server is geconfigureerd, zodat toegang vanaf het IP-adres van de machine uitvoeren van deze scripts. U vindt deze instelling onder de Azure SQL-server / configuration / IP-adressen toegestaan.
3. Voer het script SetupSampleSplitMergeEnvironment.ps1 voor het maken van de voorbeeldomgeving.
   
   Uitvoeren van dit script worden gewist van alle bestaande shard map management gegevens structuren op de database voor shard-manager en de shards. Het is mogelijk handig om het script opnieuw uitvoeren als u wilt opnieuw initialiseren van de shard-toewijzing of shards.
   
   Voorbeeld van opdrachtregel:

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. Voer het script Getmappings.ps1 om de toewijzingen die momenteel aanwezig zijn in de voorbeeldomgeving weer te geven.
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. Voer het script ExecuteSampleSplitMerge.ps1 voor het uitvoeren van een splitsbewerking (de helft van de gegevens op de eerste shard verplaatst naar de tweede shard) en vervolgens een samenvoeging (de gegevens terug verplaatsen naar de eerste shard). Als u SSL geconfigureerd en links van het http-eindpunt dat is uitgeschakeld, zorg ervoor dat u het eindpunt https:// in plaats daarvan.
   
   Voorbeeld van opdrachtregel:

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   Als u de onderstaande fout, is het meest waarschijnlijk een probleem met het certificaat van uw Web-eindpunt. Probeer verbinding te maken met het eindpunt op het Web met uw favoriete webbrowser en controleer of er een certificaatfout.
   
     ```
     Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLSsecure channel.
     ```
   
   Als deze is geslaagd, de uitvoer moet eruitzien als de onderstaande:
   
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
6. Experimenteer met andere gegevenstypen. Al deze scripts duren voordat een optionele - ShardKeyType parameter waarmee u het type sleutel opgeven. De standaardwaarde is Int32, maar u kunt ook opgeven Int64, Guid of binair.

## <a name="create-requests"></a>Aanvragen maken
De service kan worden gebruikt met behulp van de web-UI of door het importeren en het gebruik van de SplitMerge.psm1 PowerShell-module die wordt uw aanvragen via de Webrol indienen.

De service kunt verplaatsen van gegevens in shard tabellen en tabellen. Een shard-tabel heeft een sharding-sleutelkolom en andere rijgegevens in elke shard heeft. Een tabel is niet shard, zodat deze dezelfde rijgegevens in elke shard bevat. Tabellen zijn handig voor gegevens die niet vaak veranderen en wordt gebruikt om lid te worden met shard tabellen in query's.

Als u wilt splitsen en samenvoegen-bewerking uitvoeren, moet u de shard tabellen en tabellen die u wilt hebt verplaatst declareren. Dit is mogelijk dankzij de **waarde voor SchemaInfo** API. Deze API is in de **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** naamruimte.

1. Voor elke shard-tabel, maakt u een **ShardedTableInfo** object met een beschrijving van de tabel bovenliggende schemanaam (optioneel, standaard ingesteld op 'dbo'), naam van de tabel en de naam van de kolom in de tabel die de sharding-sleutel bevat.
2. Voor elke tabel, maakt u een **ReferenceTableInfo** object met een beschrijving van de tabel bovenliggende schemanaam (optioneel, standaard ingesteld op 'dbo') en de naam van de tabel.
3. De bovenstaande TableInfo objecten toevoegen aan een nieuwe **waarde voor SchemaInfo** object.
4. Geen verwijzing ophalen naar een **ShardMapManager** object en roep **GetSchemaInfoCollection**.
5. Toevoegen de **waarde voor SchemaInfo** naar de **SchemaInfoCollection**, geven de naam van de shard-toewijzing.

Een voorbeeld hiervan kan worden weergegeven in het script SetupSampleSplitMergeEnvironment.ps1.

De service voor splitsen en samenvoegen maakt geen de doeldatabase (of een schema voor alle tabellen in de database) voor u. Ze moeten zijn vooraf gemaakte voordat een aanvraag wordt verzonden naar de service.

## <a name="troubleshooting"></a>Problemen oplossen
Mogelijk ziet u de onderstaande bericht bij het uitvoeren van de powershell-voorbeeldscripts:

   ```
   Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.
   ```

Deze fout betekent dat uw SSL-certificaat niet correct is geconfigureerd. Volg de instructies in de sectie 'Verbinding maken met een webbrowser'.

Als u geen aanvragen indienen ziet u mogelijk deze:

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

In dit geval Controleer uw configuratiebestand, met name de instelling voor **WorkerRoleSynchronizationStorageAccountConnectionString**. Deze fout geeft meestal aan dat de werkrol niet met succes de metagegevensdatabase voor eerste keer wordt gebruikt initialiseren kan. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png


---
title: Maken en beheren van elastische taken met behulp van PowerShell | Microsoft Docs
description: PowerShell gebruikt voor het beheren van Azure SQL Database-groepen
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 737d8d13-5632-4e18-9cb0-4d3b8a19e495
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 357937aad5eb13ca87267629eb542cc43119dc0a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a>Maken en beheren van de SQL-Database van de elastische taken met behulp van PowerShell (preview)

De PowerShell-APIs voor **elastische Database taken** (in preview) kunt u definiëren van een groep met databases op basis waarvan de scripts worden uitgevoerd. In dit artikel laat zien hoe maken en beheren van **elastische Database taken** met PowerShell-cmdlets. Zie [elastische taken overzicht](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Zie voor een gratis proefversie [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Een set databases die zijn gemaakt met de hulpprogramma's van elastische Database. Zie [aan de slag met hulpprogramma's voor elastische Database](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
* **Elastische Database taken** PowerShell pakket: Zie [elastische Database installeren taken](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Selecteer uw Azure-abonnement
Om het abonnement te selecteren moet u uw abonnements-Id (**- SubscriptionId**) of de naam van abonnement (**- SubscriptionName**). U kunt uitvoeren als u meerdere abonnementen hebt de **Get-AzureRmSubscription** cmdlet en kopieert u de informatie over het gewenste abonnement uit de resultatenset is ingesteld. Zodra u de abonnementsgegevens van uw hebt, voer de volgende cmdlet als u dit abonnement de standaard, namelijk het doel voor het maken en beheren van taken:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

De [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) wordt aanbevolen voor gebruik ontwikkelen en uitvoeren van PowerShell-scripts uitvoeren op de elastische Database-taken.

## <a name="elastic-database-jobs-objects"></a>Elastische taken databaseobjecten
De volgende tabel geeft een lijst van alle objecttypen van **elastische Database taken** samen met de beschrijving en de relevante PowerShell APIs.

<table style="width:100%">
  <tr>
    <th>Objecttype</th>
    <th>Beschrijving</th>
    <th>Gerelateerde PowerShell API 's</th>
  </tr>
  <tr>
    <td>Referentie</td>
    <td>Gebruikersnaam en het wachtwoord moet worden gebruikt wanneer verbinding maken met databases voor uitvoering van scripts of de toepassing van DACPACs. <p>Het wachtwoord wordt versleuteld vóór het verzenden naar en opslaan in de database van de elastische Database-taken.  Het wachtwoord worden ontsleuteld door de service elastische Database-taken via de referentie is gemaakt en dat is geüpload van het script voor installatie.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Nieuwe AzureSqlJobCredential</p><p>Set-AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Script</td>
    <td>Transact-SQL-script moet worden gebruikt voor uitvoering tussen databases.  Het script moet worden gemaakt voor de idempotent worden omdat de service wordt opnieuw geprobeerd de uitvoering van het script op fouten.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Nieuwe AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">-Gegevenslaagtoepassing </a> pakket moet worden toegepast voor databases.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Nieuwe AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Database-doel</td>
    <td>Database- en de naam die verwijst naar een Azure SQL Database.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nieuwe AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Doel van shard-kaart</td>
    <td>De combinatie van een doel van de database en een referentie om te worden gebruikt om informatie opgeslagen in een elastische Database shard-toewijzing te bepalen.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nieuwe AzureSqlJobTarget</p>
    <p>Set-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Doel van de aangepaste verzameling</td>
    <td>Gedefinieerde groep met databases gezamenlijk gebruiken voor uitvoering.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nieuwe AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Aangepaste verzameling onderliggende doel</td>
    <td>Database-doel waarnaar wordt verwezen in een aangepaste verzameling.</td>
    <td>
    <p>Voeg AzureSqlJobChildTarget</p>
    <p>Verwijder AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Job</td>
    <td>
    <p>Definitie van de parameters voor een taak die kan worden gebruikt voor het activeren van uitvoering of om te voldoen aan een schema.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Nieuwe AzureSqlJob</p>
    <p>Set-AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Uitvoeren van taak</td>
    <td>
    <p>Container van taken die nodig zijn om te voldoen aan het uitvoeren van een script of een DACPAC toepassen op een doel met referenties voor databaseverbindingen met fouten verwerkt overeenkomstig een beleid kan worden uitgevoerd.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start AzureSqlJobExecution</p>
    <p>Stop AzureSqlJobExecution</p>
    <p>Wacht AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Uitvoering van de taak taak</td>
    <td>
    <p>De eenheid van werk om te voldoen aan een taak.</p>
    <p>Als een taak kan niet met succes uitvoeren, wordt de resulterende uitzonderingsbericht vastgelegd en een nieuwe, overeenkomende projecttaak worden gemaakt en uitgevoerd volgens het opgegeven uitvoeringsbeleid.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start AzureSqlJobExecution</p>
    <p>Stop AzureSqlJobExecution</p>
    <p>Wacht AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Taak uitvoeringsbeleid</td>
    <td>
    <p>Besturingselementen taak uitvoering time-outs, limieten voor nieuwe pogingen en interval tussen nieuwe pogingen.</p>
    <p>Elastische taken van de Database bevat een standaarduitvoeringsbeleid taak waardoor in feite oneindige pogingen van de mislukte taak met exponentieel uitstel intervallen tussen nieuwe pogingen.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Nieuwe AzureSqlJobExecutionPolicy</p>
    <p>Set-AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Planning</td>
    <td>
    <p>Specificatie voor het uitvoeren van plaatsvinden in een interval van terugkerende of één keer op basis van tijd.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Nieuwe AzureSqlJobSchedule</p>
    <p>Set-AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Taak wordt geactiveerd</td>
    <td>
    <p>Een toewijzing tussen een taak en een schema wilt uitvoeren volgens de planning van de taak.</p>
    </td>
    <td>
    <p>Nieuwe AzureSqlJobTrigger</p>
    <p>Verwijder AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Ondersteunde elastische Database taken groeperen typen
De taak uitgevoerd (T-SQL) Transact-SQL-scripts of de toepassing van DACPACs in een groep van databases. Wanneer een taak wordt verzonden om te worden uitgevoerd in een groep met databases, de taak 'wordt uitgebreid' de in onderliggende taken waarbij elk de aangevraagde uitgevoerd op een individuele database in de groep uitvoert. 

Er zijn twee typen groepen die u kunt maken: 

* [Shard-toewijzing](sql-database-elastic-scale-shard-map-management.md) groep: als een taak wordt verzonden naar een shard-toewijzing met als doel, de taak query de shard-kaart om te bepalen van de huidige set shards en maakt vervolgens onderliggende taken voor elke shard in de shard-toewijzing.
* Verzamelgroep voor de aangepaste: een aangepaste set van databases gedefinieerd. Wanneer een taak is bedoeld voor een aangepaste verzameling, wordt gemaakt onderliggende taken voor elke database die momenteel in de verzameling aangepaste.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Taken voor het instellen van de elastische Database verbinding
Een verbinding moet worden ingesteld op de taken *beheer database* voorafgaand aan de taken API's gebruikt. Deze cmdlet uitvoert, activeert een referentie-venster moet worden aanvragen van de gebruikersnaam en wachtwoord die zijn gemaakt tijdens de installatie van de elastische Database taken weergegeven. Alle voorbeelden in dit onderwerp wordt ervan uitgegaan dat al deze eerste stap is uitgevoerd.

Open een verbinding met de elastische Database taken:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Versleutelde referenties binnen de taken voor elastische Database
Databasereferenties kunnen worden ingevoegd in de taken *beheer database* met het wachtwoord dat is versleuteld. Het is nodig voor het opslaan van referenties in voor het inschakelen van de taken worden uitgevoerd op een later tijdstip (met behulp van de taakschema's).

Versleuteling werkt via een certificaat gemaakt als onderdeel van het script voor installatie. Het installatiescript maakt en uploadt het certificaat in de Azure-Cloudservice voor het ontsleutelen van de opgeslagen gecodeerde wachtwoorden. De Azure Cloud Service later slaat de openbare sleutel in de taken *beheer database* waarmee de API met PowerShell of Azure portal-interface voor het versleutelen van een opgegeven wachtwoord zonder het certificaat moet lokaal zijn geïnstalleerd .

De referentie-wachtwoorden zijn versleuteld en beveiligd van gebruikers met alleen-lezentoegang voor elastische Database taken objecten. Maar het is mogelijk dat een kwaadwillende gebruiker met lees-/ schrijftoegang tot elastische Database taken objecten om op te halen van een wachtwoord. Referenties zijn ontworpen om opnieuw worden gebruikt voor uitvoering. Referenties worden doorgegeven aan de doeldatabases bij het tot stand brengen van verbindingen. Er zijn momenteel geen beperkingen op de doeldatabases die worden gebruikt voor elke referentie, kwaadwillende gebruiker kan een database-doel voor een database onder het beheer van de kwaadwillende gebruiker toevoegen. De gebruiker kan vervolgens een taak die gericht is op deze database voor het verkrijgen van de referentie-wachtwoord starten.

Aanbevolen beveiligingsprocedures voor elastische Database taken omvatten:

* Gebruik van de API's naar vertrouwde personen te beperken.
* Referenties hoeven de minimaal benodigde bevoegdheden nodig zijn voor de taak met taak uitvoeren.  Meer informatie ziet u binnen dit [autorisatie en machtigingen](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-artikel.

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Een versleutelde referentie voor de taakuitvoering van de voor databases maken
Een nieuwe versleutelde referentie maken de [ **cmdlet Get-Credential** ](https://technet.microsoft.com/library/hh849815.aspx) vraagt om een gebruikersnaam en wachtwoord die kan worden doorgegeven aan de [ **cmdlet New-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Referenties bijwerken
Wanneer wachtwoorden wijzigen, gebruikt de [ **cmdlet Set-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) en stel de **CredentialName** parameter.

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Een elastische Database shard-toewijzing doel definiëren
Uitvoeren van een taak op alle databases in een set shard (gemaakt met behulp van [clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md)), een shard-toewijzing gebruiken als het doel van de database. Dit voorbeeld vereist een shard-toepassing gemaakt met behulp van de clientbibliotheek voor elastische Database. Zie [aan de slag met elastische Database extra voorbeeld](sql-database-elastic-scale-get-started.md).

De shard kaart manager-database moet worden ingesteld als het doel van een database en vervolgens de specifieke shard-toewijzing moet worden opgegeven als een doel.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Maken van een T-SQL-Script voor uitvoering tussen databases
Bij het maken van T-SQL-scripts voor de uitvoering van het is raadzaam om te bouwen om te worden [idempotent](https://en.wikipedia.org/wiki/Idempotence) en robuuste tegen fouten. Elastische Database-taken opnieuw uitvoeren van het script als een fout optreedt, ongeacht de indeling van de fout wordt aangetroffen die worden uitgevoerd.

Gebruik de [ **cmdlet New-AzureSqlJobContent** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) maken en opslaan van een script voor uitvoering en stel de **- ContentName** en **- CommandText** parameters.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Een nieuw script maken vanuit een bestand
Als de T-SQL-script is gedefinieerd in een bestand, gebruikt u dit voor het importeren van het script:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Bijwerken van een T-SQL-script voor uitvoering tussen databases
Deze PowerShell-script de T-SQL-opdrachttekst voor een bestaand script-updates.

Stel de volgende variabelen in overeenstemming met de definitie van de gewenste script moet worden ingesteld:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Bijwerken van de definitie voor een bestaand script
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Een taak voor het uitvoeren van een script in een shard-toewijzing te maken
Deze PowerShell-script start een taak voor uitvoering van een script dat op elke shard in een elastische Schaalfunctionaliteit van shard-toewijzing.

Stel de volgende variabelen in overeenstemming met de gewenste script en de doelserver:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Als een taak wilt uitvoeren
Deze PowerShell-script wordt een bestaande taak uitgevoerd:

Update de volgende variabele in overeenstemming met de naam van de gewenste taak hebt uitgevoerd:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>De status van de taakuitvoering van een enkele ophalen
Gebruik de [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) en stel de **JobExecutionId** parameter om de status van de taakuitvoering weer te geven.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Gebruik van dezelfde **Get-AzureSqlJobExecution** cmdlet uit met de **voor IncludeChildren** -parameter voor de status van onderliggende taak uitvoeringen, namelijk de specifieke status voor elke taak uitgevoerd op elke database weergeven gericht door de taak.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>De status tussen uitvoeringen van meerdere taak weergeven
De [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) heeft meerdere optionele parameters die kunnen worden gebruikt om meerdere taak uitvoeringen, gefilterd via de opgegeven parameters weer te geven. Het volgende voorbeeld toont een aantal mogelijke manieren waarop Get-AzureSqlJobExecution gebruiken:

Alle actieve taken voor de bovenste niveau uitvoeringen ophalen:

    Get-AzureSqlJobExecution

Alle bovenste niveau taak uitvoeringen, met inbegrip van niet-actieve taak uitvoeringen ophalen:

    Get-AzureSqlJobExecution -IncludeInactive

Alle onderliggende taak uitvoeringen van een opgegeven taak uitvoerings-ID, met inbegrip van niet-actieve taak uitvoeringen ophalen:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

Ophalen van alle taak uitvoeringen gemaakt met behulp van een planning / taak combinatie, met inbegrip van niet-actieve taken:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Alle taken die gericht is op een opgegeven shard-kaart, inclusief niet-actieve taken worden opgehaald:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Alle taken die gericht is op een opgegeven aangepaste verzameling, met inbegrip van niet-actieve taken worden opgehaald:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Ophalen van de lijst van de taak taak uitvoeringen binnen een specifieke taak kan worden uitgevoerd:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Taak uitvoeren taakdetails ophalen:

De volgende PowerShell-script kan worden gebruikt om de details van een taak uitvoering van taken, die bijzonder nuttig is bij het opsporen van fouten in uitvoering weer te geven.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>Storingen in uitvoering taak ophalen
De **JobTaskExecution object** bevat een eigenschap voor de levenscyclus van de taak samen met een berichteigenschap. Als de uitvoering van de taak een taak is mislukt, de levenscyclus van de eigenschap wordt ingesteld op *mislukt* en de berichteigenschap zal worden ingesteld op de resulterende uitzonderingsbericht en de stack. Als een taak is mislukt, is het belangrijk om de details van taken waarvoor is mislukt voor een bepaalde taak weer te geven.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Wachttijd voor het uitvoeren van een taak te voltooien
De volgende PowerShell-script kan worden gebruikt om te wachten op een taak te voltooien:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Maken van een aangepaste uitvoeringsbeleid
Elastische Database taken ondersteunt het maken van aangepaste uitvoeringsbeleidsregels die kunnen worden toegepast wanneer u taken start.

Uitvoeringsbeleidsregels toestaan op dit moment voor het definiëren van:

* Naam: Id voor het uitvoeringsbeleid.
* De time-out van de taak: Totale tijd voordat een taak met elastische taken van de Database, worden geannuleerd.
* Eerste Interval voor opnieuw proberen: Interval moet worden gewacht voordat de eerste poging.
* Maximale Interval voor opnieuw proberen: Cap intervallen voor opnieuw proberen te gebruiken.
* Opnieuw proberen Interval Backoff Coefficient: Coefficient die wordt gebruikt voor het berekenen van de volgende interval tussen nieuwe pogingen.  De volgende formule wordt gebruikt: (eerste Interval voor opnieuw proberen) * Math.pow ((Interval Backoff Coefficient) (nummer van pogingen) - 2). 
* Maximum aantal pogingen: Het maximum aantal probeer het opnieuw probeert uit te voeren in een job.

Het standaarduitvoeringsbeleid maakt gebruik van de volgende waarden:

* Naam: Standaarduitvoeringsbeleid
* De time-out van de taak: 1 week
* Eerste Interval voor opnieuw proberen: 100 milliseconden
* Maximale Interval voor opnieuw proberen: 30 minuten
* Coefficient Interval opnieuw proberen: 2
* Maximum aantal pogingen: 2.147.483.647

Maak de gewenste uitvoeringsbeleid:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Een aangepaste uitvoeringsbeleid bijwerken
Werk het gewenste uitvoeringsbeleid om bij te werken:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a>Een taak annuleren
Elastische Database taken ondersteunt aanvragen van de annulering van taken.  Als taken voor elastische Database een annuleringsverzoek voor een taak die momenteel wordt uitgevoerd detecteert, wordt geprobeerd de taak te stoppen.

Er zijn twee verschillende manieren dat elastische Database taken een annulering kan uitvoeren:

1. Annuleren taken momenteel worden uitgevoerd: als een annulering wordt gedetecteerd, terwijl een taak die momenteel wordt uitgevoerd, een annulering wordt geprobeerd binnen het momenteel wordt uitgevoerd aspect van de taak.  Bijvoorbeeld: als er een langdurige query die momenteel wordt uitgevoerd wanneer een annulering wordt uitgevoerd, zal er een poging om de query te annuleren.
2. Nieuwe pogingen taak annuleren: als een annulering wordt gedetecteerd door de besturingselement-thread voordat een taak voor uitvoering wordt gestart, de besturingselement-thread wordt voorkomen dat de taak starten en declareren van de aanvraag als geannuleerd.

Als een taak annulering is aangevraagd voor een bovenliggende taak, wordt de annuleringsaanvraag gehonoreerd voor de bovenliggende taak en alle bijbehorende onderliggende taken.

Gebruik hiervoor een annuleringsaanvraag indienen, de [ **cmdlet Stop AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) en stel de **JobExecutionId** parameter.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Verwijderen van een taak en Taakgeschiedenis asynchroon
Elastische taken van de Database biedt ondersteuning voor asynchrone verwijderen van jobs. Een taak kan worden gemarkeerd voor verwijdering en het systeem de taak en de taakgeschiedenis wordt verwijderd nadat alle uitvoeringen van de taak voor de taak hebt voltooid. Het systeem wordt niet automatisch uitvoeringen van de actieve taak geannuleerd.  

Aanroepen [ **Stop AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) uitvoeringen van de actieve taak annuleren.

Gebruiken om te verwijderen van een taak activeren, de [ **cmdlet Remove-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) en stel de **JobName** parameter.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="to-create-a-custom-database-target"></a>Om het doel van een aangepaste database te maken
U kunt aangepaste database doelen voor directe uitvoering of voor insluiting in een aangepaste database wilt definiëren. Bijvoorbeeld, omdat **elastische pools** zijn nog niet rechtstreeks ondersteund met behulp van PowerShell APIs, kunt u een aangepaste database doel en de aangepaste database verzameling target dit alle databases in de pool omvat.

De volgende variabelen in overeenstemming met de gewenste databasegegevens instellen:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Om een doel van de verzameling aangepaste database te maken
Gebruik de [ **nieuw AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet voor het definiëren van een doel van de verzameling aangepaste database zodat kan worden uitgevoerd via meerdere gedefinieerde database doelen. Na het maken van een databasegroep, kunnen de databases worden gekoppeld aan het doel van de aangepaste verzameling.

Stel de volgende variabelen in overeenstemming met de configuratie van de gewenste aangepaste verzameling doel:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Databases toevoegen aan een doel van de verzameling aangepaste database
Een database toevoegen aan een specifieke aangepaste verzameling gebruik de [ **toevoegen AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Bekijk de databases binnen een verzameling aangepaste database doel
Gebruik de [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet voor het ophalen van de onderliggende databases binnen een doel van de verzameling aangepaste database. 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Een taak voor het uitvoeren van een script voor een doel van de verzameling aangepaste database maken
Gebruik de [ **nieuw AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) cmdlet een taak op basis van een groep met databases die zijn gedefinieerd door een doel van de verzameling aangepaste database te maken. Elastische taken van de Database wordt de taak uitbreiden naar meerdere onderliggende taken elke overeenkomt met een database die is gekoppeld aan het doel van de verzameling aangepaste database en zorg ervoor dat het script wordt uitgevoerd tegen elke database. Ook is het belangrijk dat scripts idempotent zijn flexibel omgaan met nieuwe pogingen worden.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Gegevens verzamelen over databases
Een taak kunt u een query uitvoeren in een groep van databases en de resultaten naar een specifieke tabel verzenden. De tabel kan worden opgevraagd achteraf om te zien van het queryresultaat van elke database. Dit biedt een asynchrone methode voor het uitvoeren van een query tussen meerdere databases. Mislukte pogingen worden automatisch verwerkt via de nieuwe pogingen.

De opgegeven doeltabel wordt automatisch gemaakt als deze nog niet bestaat. De nieuwe tabel komt overeen met het schema van de geretourneerde resultatenset. Als meerdere resultatensets worden geretourneerd door een script, wordt taken voor elastische Database alleen het eerste verzonden naar de doeltabel.

De volgende PowerShell-script een script wordt uitgevoerd en de resultaten in een opgegeven tabel verzamelt. Dit script wordt ervan uitgegaan dat een T-SQL-script is gemaakt waarmee een enkelvoudig resultaat wordt verkregen set levert en dat er een doel van de verzameling aangepaste database is gemaakt.

Dit script maakt gebruik van de [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet. Stel de parameters voor het script, referenties en uitvoering van doel:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Maken en start een taak voor scenario's voor het verzamelen van gegevens
Dit script maakt gebruik van de [ **Start AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.

    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Een taak uitvoeren trigger plannen
De volgende PowerShell-script kan worden gebruikt voor het maken van een terugkerend schema. Dit script maakt gebruik van een minuut interval maar [ **nieuw AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) biedt ook ondersteuning voor parameters - DayInterval, - HourInterval, - MonthInterval, en -WeekInterval. Schema's die slechts één keer worden uitgevoerd, kunnen worden gemaakt door doorgeven - eenmalige.

Maak een nieuwe planning:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Om een taak uitgevoerd op een tijdschema te activeren
Een trigger voor de taak kan worden gedefinieerd als een taak uitgevoerd volgens een tijdschema hebben. De volgende PowerShell-script kan worden gebruikt voor het maken van een trigger taak.

Gebruik [nieuw AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) en stel de volgende variabelen in overeenstemming met de gewenste taak en planning:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Verwijderen van een associatie met geplande taak wordt uitgevoerd volgens schema stoppen
Als u wilt stoppen met het uitvoeren van terugkerende taak via een trigger taak, kan de trigger taak worden verwijderd. Verwijderen van een trigger taak om te stoppen van een taak uit te voeren volgens een schema met de [ **cmdlet Remove-AzureSqlJobTrigger**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Taak triggers die zijn gebonden aan een tijdschema ophalen
De volgende PowerShell-script kan worden gebruikt om te verkrijgen en weergeven van de taak triggers geregistreerd voor een bepaald tijdschema.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Voor het ophalen van de taak triggers die is gebonden aan een taak
Gebruik [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) verkrijgen en schema's met een geregistreerde taak weergegeven.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Maken van een toepassing-gegevenslaagtoepassingen (DACPAC) voor uitvoering voor databases
Zie het maken van een DACPAC [Data-Tier applications](https://msdn.microsoft.com/library/ee210546.aspx). Gebruik voor het implementeren van een DACPAC de [cmdlet New-AzureSqlJobContent](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent). De DACPAC moet toegankelijk zijn voor de service. Het beste een gemaakte DACPAC uploaden naar Azure Storage en maak een [Shared Access Signature](../storage/common/storage-dotnet-shared-access-signature-part-1.md) voor de DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Een toepassing-gegevenslaagtoepassingen (DACPAC) voor uitvoering tussen databases bijwerken
Bestaande DACPACs geregistreerd binnen een elastische Database taken kunnen worden bijgewerkt om te verwijzen naar een nieuwe URI's. Gebruik de [ **cmdlet Set-AzureSqlJobContentDefinition** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) geregistreerd DACPAC de DACPAC-URI op een bestaande bijwerken:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Een taak voor het toepassen van een toepassing-gegevenslaagtoepassingen (DACPAC) voor databases te maken
Nadat een DACPAC binnen elastische taken van de Database is gemaakt, kunt u een taak gemaakt voor het toepassen van de DACPAC voor een groep met databases. De volgende PowerShell-script kan worden gebruikt voor het maken van een taak DACPAC via een aangepaste verzameling van databases:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->

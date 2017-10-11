---
title: Aan de slag met elastische database taken | Microsoft Docs
description: het gebruik van taken voor elastische database
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 2540de0e-2235-4cdd-9b6a-b841adba00e5
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: 05c20e880d4eb1eacdecc0c4c7e7491dfe1e6a89
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-elastic-database-jobs"></a>Aan de slag met taken voor elastische Database
Elastische Database-taken (preview) voor Azure SQL Database kunt u betrouwbaarheid T-SQL-scripts die meerdere databases tijdens het automatisch opnieuw uit te voeren en het geven van de uiteindelijke voltooiing garanties omvatten uitvoeren. Zie voor meer informatie over de functie van de taak elastische Database, de [overzicht functiepagina](sql-database-elastic-jobs-overview.md).

In dit onderwerp wordt uitgebreid voor het voorbeeld dat is gevonden in [aan de slag met hulpprogramma's voor elastische Database](sql-database-elastic-scale-get-started.md). Wanneer voltooid, wordt u: informatie over het maken en beheren van taken die een groep verwante databases beheren. Het is niet vereist voor het gebruik van de elastische Schaalfunctionaliteit van hulpprogramma's om te profiteren van de voordelen van elastische taken.

## <a name="prerequisites"></a>Vereisten
Downloaden en uitvoeren van de [aan de slag met elastische Database extra voorbeeld](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Een shard-toewijzing manager met behulp van de voorbeeld-app maken
Hier maakt u een shard-toewijzing manager samen met enkele shards, gevolgd door het invoegen van gegevens in de shards. Als u al shards instellen met gedeelde gegevens bevat, kunt u de volgende stappen overslaan en verplaatsen naar de volgende sectie.

1. Bouwen en uitvoeren van de **aan de slag met hulpprogramma's voor elastische Database** voorbeeldtoepassing. Volg de stappen totdat stap 7 in de sectie [downloaden en uitvoeren van de voorbeeld-app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Aan het einde van stap 7 ziet u de volgende opdrachtprompt:

   ![opdrachtprompt](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. Typ in het opdrachtvenster '1' en druk op **Enter**. Dit maakt de shard kaart manager en voegt twee shards aan de server. Vervolgens typt u "3" en druk op **Enter**; deze actie vier keer herhalen. Dit invoegen voorbeeld gegevensrijen in uw shards.
3. De [Azure Portal](https://portal.azure.com) drie nieuwe databases moeten worden weergegeven:

   ![Visual Studio-bevestiging](./media/sql-database-elastic-query-getting-started/portal.png)

   We gaan nu een verzameling aangepaste database die overeenkomt met de databases in de shard-toewijzing maken. Dit kunnen we maken en uitvoeren van een taak die een nieuwe tabel toevoegen via shards.

Hier wordt meestal maken we een shard-toewijzing met als doel, met behulp van de **nieuw AzureSqlJobTarget** cmdlet. De shard kaart manager-database moet worden ingesteld als het doel van een database en vervolgens de specifieke shard-toewijzing is opgegeven als een doel. We gaan in plaats daarvan inventariseren van alle databases in de server en de databases toevoegen aan de nieuwe aangepaste verzameling met uitzondering van de database master.

## <a name="creates-a-custom-collection-and-add-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Een aangepaste verzameling maakt en alle databases op de server toevoegen aan het doel van de aangepaste verzameling met uitzondering van de master.
   ```
    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName
    $dbsinserver | %{
    $currentdb = $_.DatabaseName
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target."
        }

        else
        {
            throw $_
        }

    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
   }
   ```
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Maken van een T-SQL-Script voor uitvoering tussen databases
   ```
    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script
   ```

## <a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>De taak voor het uitvoeren van een script via de aangepaste groep van databases maken

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-the-job"></a>De taak uitvoeren
De volgende PowerShell-script kan worden gebruikt als een bestaande taak wilt uitvoeren:

Update de volgende variabele in overeenstemming met de naam van de gewenste taak hebt uitgevoerd:

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-the-state-of-a-single-job-execution"></a>De status van het uitvoeren van een enkele taak ophalen
Gebruik van dezelfde **Get-AzureSqlJobExecution** cmdlet uit met de **voor IncludeChildren** -parameter voor de status van onderliggende taak uitvoeringen, namelijk de specifieke status voor elke taak uitgevoerd op elke database weergeven gericht door de taak.

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-the-state-across-multiple-job-executions"></a>De status tussen uitvoeringen van meerdere taak weergeven
De **Get-AzureSqlJobExecution** cmdlet heeft meerdere optionele parameters die kunnen worden gebruikt om meerdere taak uitvoeringen, gefilterd via de opgegeven parameters weer te geven. Het volgende voorbeeld toont een aantal mogelijke manieren waarop Get-AzureSqlJobExecution gebruiken:

Alle actieve taken voor de bovenste niveau uitvoeringen ophalen:

   ```
    Get-AzureSqlJobExecution
   ```

Alle bovenste niveau taak uitvoeringen, met inbegrip van niet-actieve taak uitvoeringen ophalen:

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

Alle onderliggende taak uitvoeringen van een opgegeven taak uitvoerings-ID, met inbegrip van niet-actieve taak uitvoeringen ophalen:

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

Ophalen van alle taak uitvoeringen gemaakt met behulp van een planning / taak combinatie, met inbegrip van niet-actieve taken:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

Alle taken die gericht is op een opgegeven shard-kaart, inclusief niet-actieve taken worden opgehaald:

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Alle taken die gericht is op een opgegeven aangepaste verzameling, met inbegrip van niet-actieve taken worden opgehaald:

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Ophalen van de lijst van de taak taak uitvoeringen binnen een specifieke taak kan worden uitgevoerd:

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

Taak uitvoeren taakdetails ophalen:

De volgende PowerShell-script kan worden gebruikt om de details van een taak uitvoering van taken, die bijzonder nuttig is bij het opsporen van fouten in uitvoering weer te geven.
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a>Storingen in uitvoering taak ophalen
Het object JobTaskExecution bevat een eigenschap voor de levenscyclus van de taak samen met een berichteigenschap. Als de uitvoering van de taak een taak is mislukt, de levenscyclus van de eigenschap wordt ingesteld op *mislukt* en de berichteigenschap zal worden ingesteld op de resulterende uitzonderingsbericht en de stack. Als een taak is mislukt, is het belangrijk om de details van taken waarvoor is mislukt voor een bepaalde taak weer te geven.

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions)
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }
   ```

## <a name="waiting-for-a-job-execution-to-complete"></a>Wacht op de taakuitvoering van een te voltooien
De volgende PowerShell-script kan worden gebruikt om te wachten op een taak te voltooien:

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

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

   ```
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy
   ```

### <a name="update-a-custom-execution-policy"></a>Een aangepaste uitvoeringsbeleid bijwerken
Werk het gewenste uitvoeringsbeleid om bij te werken:

   ```
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
   ```

## <a name="cancel-a-job"></a>Een taak annuleren
Elastische Database taken ondersteunt taken annulering aanvragen.  Als taken voor elastische Database een annuleringsverzoek voor een taak die momenteel wordt uitgevoerd detecteert, wordt geprobeerd de taak te stoppen.

Er zijn twee verschillende manieren dat elastische Database taken een annulering kan uitvoeren:

1. Momenteel worden uitgevoerd annuleren taken: als een annulering wordt gedetecteerd, terwijl een taak die momenteel wordt uitgevoerd, een annulering wordt geprobeerd binnen het momenteel wordt uitgevoerd aspect van de taak.  Bijvoorbeeld: als er een langdurige query die momenteel wordt uitgevoerd wanneer een annulering wordt uitgevoerd, zal er een poging om de query te annuleren.
2. Annuleren pogingen: Als een annulering wordt gedetecteerd door de besturingselement-thread voordat een taak voor uitvoering wordt gestart, de besturingselement-thread wordt voorkomen dat de taak starten en declareren van de aanvraag als geannuleerd.

Als een taak annulering is aangevraagd voor een bovenliggende taak, wordt de annuleringsaanvraag gehonoreerd voor de bovenliggende taak en alle bijbehorende onderliggende taken.

Gebruik hiervoor een annuleringsaanvraag indienen, de **Stop AzureSqlJobExecution** cmdlet en stel de **JobExecutionId** parameter.

   ```
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Een taak door de naam van de taakgeschiedenis verwijderen
Elastische taken van de Database biedt ondersteuning voor asynchrone verwijderen van jobs. Een taak kan worden gemarkeerd voor verwijdering en het systeem de taak en de taakgeschiedenis wordt verwijderd nadat alle uitvoeringen van de taak voor de taak hebt voltooid. Het systeem wordt niet automatisch uitvoeringen van de actieve taak geannuleerd.  

Stop AzureSqlJobExecution moet in plaats daarvan worden aangeroepen om te annuleren uitvoeringen van actieve taken.

Gebruiken om te verwijderen van een taak activeren, de **verwijderen AzureSqlJob** cmdlet en stel de **JobName** parameter.

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a>Het doel van een aangepaste database maken
Aangepaste database doelen kunnen worden gedefinieerd in elastische Database-taken die kunnen worden gebruikt voor uitvoering rechtstreeks of voor insluiting in de groep van een aangepaste database. Aangezien **elastische pools** niet nog rechtstreeks worden ondersteund via de APIs PowerShell u gewoon maakt een aangepaste database doel en de aangepaste database verzameling target dit alle databases in de pool omvat.

De volgende variabelen in overeenstemming met de gewenste databasegegevens instellen:

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a>Een doel van de verzameling aangepaste database maken
Een verzameling aangepaste database doel kan worden gedefinieerd als de uitvoering inschakelen tussen meerdere gedefinieerde database doelen. Nadat de groep van een database is gemaakt, is databases kunnen worden gekoppeld aan het doel van de aangepaste verzameling.

Stel de volgende variabelen in overeenstemming met de configuratie van de gewenste aangepaste verzameling doel:

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-to-a-custom-database-collection-target"></a>Databases toevoegen aan een doel van de verzameling aangepaste database
Database-doelen kunnen worden gekoppeld aan de aangepaste database verzameling doelen voor het maken van een groep met databases. Wanneer een taak is gemaakt die een aangepaste database verzameling doel, zal het worden uitgebreid om het doel van de databases die zijn gekoppeld aan de groep op het moment van de uitvoering.

De gewenste database toevoegen aan een specifieke aangepaste verzameling:

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Bekijk de databases binnen een verzameling aangepaste database doel
Gebruik de **Get-AzureSqlJobTarget** cmdlet voor het ophalen van de onderliggende databases binnen een doel van de verzameling aangepaste database.

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Een taak voor het uitvoeren van een script voor een doel van de verzameling aangepaste database maken
Gebruik de **nieuw AzureSqlJob** cmdlet een taak op basis van een groep met databases die zijn gedefinieerd door een doel van de verzameling aangepaste database te maken. Elastische taken van de Database wordt de taak uitbreiden naar meerdere onderliggende taken elke overeenkomt met een database die is gekoppeld aan het doel van de verzameling aangepaste database en zorg ervoor dat het script wordt uitgevoerd tegen elke database. Ook is het belangrijk dat scripts idempotent zijn flexibel omgaan met nieuwe pogingen worden.

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a>Gegevens verzamelen over databases
**Elastische Database taken** ondersteunt een query uit te voeren in een groep van databases en verzendt de resultaten in een opgegeven databasetabel. De tabel kan worden opgevraagd achteraf om te zien van het queryresultaat van elke database. Dit is een asynchrone mechanisme voor het uitvoeren van een query tussen meerdere databases. Fout gevallen, zoals een van de databases die tijdelijk niet beschikbaar worden automatisch verwerkt via de nieuwe pogingen.

De opgegeven doeltabel wordt automatisch gemaakt als deze nog niet bestaat, die overeenkomt met het schema van de geretourneerde resultatenset. Als de uitvoering van een script meerdere resultatensets worden geretourneerd, verzendt elastische Database taken alleen de eerste die aan de tabel van de opgegeven bestemming.

Het volgende PowerShell-script kan worden gebruikt voor het uitvoeren van een script voor het verzamelen van de resultaten in een opgegeven tabel. Dit script wordt ervan uitgegaan dat een T-SQL-script is gemaakt waarmee een enkelvoudig resultaat wordt verkregen set levert en een doel van de verzameling aangepaste database is gemaakt.

Stel het volgende in overeenstemming met de gewenste script, referenties en uitvoering van doel:

   ```
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
   ```

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Maak en start een taak voor scenario's voor het verzamelen van gegevens
   ```
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Een planning voor het uitvoeren van de taak met behulp van een trigger taak maken
De volgende PowerShell-script kan worden gebruikt om een terugkerende planning te maken. Dit script maakt gebruik van een interval van één minuut, maar nieuwe AzureSqlJobSchedule biedt ook ondersteuning voor parameters - DayInterval, - HourInterval, - MonthInterval, en -WeekInterval. Schema's die slechts één keer worden uitgevoerd, kunnen worden gemaakt door doorgeven - eenmalige.

Maak een nieuwe planning:
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Als u wilt dat een taak uitgevoerd op een tijdschema trigger taak maken
Een trigger voor de taak kan worden gedefinieerd als een taak uitgevoerd volgens een tijdschema hebben. De volgende PowerShell-script kan worden gebruikt voor het maken van een trigger taak.

Stel de volgende variabelen in overeenstemming met de gewenste taak en planning:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Verwijderen van een associatie met geplande taak wordt uitgevoerd volgens schema stoppen
Als u wilt stoppen met het uitvoeren van terugkerende taak via een trigger taak, kan de trigger taak worden verwijderd.
Verwijderen van een trigger taak om te stoppen van een taak uit te voeren volgens een schema met de **verwijderen AzureSqlJobTrigger** cmdlet.

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-to-excel"></a>De queryresultaten elastische database importeren in Excel
 U kunt de resultaten van van een query met een Excel-bestand importeren.

1. Start Excel 2013.
2. Navigeer naar de **gegevens** lint.
3. Klik op **van andere bronnen** en klik op **van SQL Server**.

   ![Excel importeren uit andere bronnen](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. In de **Wizard Gegevensverbinding** Typ de servernaam en aanmeldingsreferenties van de server. Klik op **Volgende**.
5. In het dialoogvenster **selecteert u de database met de gegevens die u wilt dat**, selecteer de **ElasticDBQuery** database.
6. Selecteer de **klanten** tabel in de lijstweergave en klikt u op **volgende**. Klik vervolgens op **voltooien**.
7. In de **importgegevens** formulier onder **selecteren hoe u wilt weergeven van deze gegevens in uw werkmap**, selecteer **tabel** en klik op **OK**.

Alle rijen uit **klanten** tabel, opgeslagen in verschillende shards vullen de Excel-werkblad.

## <a name="next-steps"></a>Volgende stappen
Nu kunt u functies van de gegevens van Excel. De verbindingsreeks gebruiken met de servernaam, databasenaam en referenties verbinding maken met uw integratiefuncties BI en gegevens van de elastische database. Zorg ervoor dat SQL Server wordt ondersteund als een gegevensbron voor het hulpprogramma. Raadpleeg de elastische query uitvoeren op database en de externe tabellen net als elke andere SQL Server-database en SQL Server-tabellen die u met het hulpprogramma verbinding wilt maken.

### <a name="cost"></a>Kosten
Er zijn geen extra kosten voor het gebruik van de functie van de query elastische Database. Echter op dit moment deze functie is alleen beschikbaar op premium-databases als een eindpunt, maar de shards kunnen zijn van een servicelaag.

Zie voor informatie over prijzen [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->

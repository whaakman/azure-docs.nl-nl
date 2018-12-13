---
title: Aan de slag met taken voor elastic database | Microsoft Docs
description: Taken voor elastic database gebruiken voor het uitvoeren van T-SQL-scripts die meerdere databases omvatten.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 07/16/2018
ms.openlocfilehash: 0269a8ea460667d44b6173e4504a9ccb5695d722
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863530"
---
# <a name="getting-started-with-elastic-database-jobs"></a>Aan de slag met taken voor Elastic Database

[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]

Taken voor elastic Database (preview) voor Azure SQL Database kunt u T-SQL-scripts die meerdere databases omvatten tijdens het automatisch opnieuw te proberen en die uiteindelijke voltooiing waarborgen biedt betrouwbaar uit te voeren. Zie voor meer informatie over de functie Elastic Database taak [elastische taken](sql-database-elastic-jobs-overview.md).

In dit artikel wordt het voorbeeld dat is gevonden in uitgebreid [aan de slag met elastische Databasehulpprogramma's](sql-database-elastic-scale-get-started.md). Wanneer dit is voltooid, leert u hoe u taken die voor het beheren van een groep gerelateerde databases maken en beheren. Het is niet vereist voor het gebruik van de hulpprogramma's voor elastisch schalen om te profiteren van de voordelen van elastische taken.

## <a name="prerequisites"></a>Vereisten

Downloaden en uitvoeren van de [aan de slag met hulpprogramma's elastische Database voor klantorders](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Een shard-Toewijzingsbeheer met behulp van de voorbeeld-app maken

Hier maakt u een shard-toewijzing manager samen met verschillende shards, gevolgd door het invoegen van gegevens in de shards. Als u al ingesteld met shard-gegevens in deze shards, kunt u de volgende stappen overslaan en verplaatsen naar de volgende sectie.

1. Bouwen en uit de **aan de slag met elastische Databasehulpprogramma's** voorbeeldtoepassing. Volg de stappen tot stap 7 in de sectie [downloaden en uitvoeren van de voorbeeld-app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Aan het einde van stap 7 ziet u de volgende opdrachtprompt:

   ![opdrachtprompt](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. Typ in het opdrachtvenster "1" en druk op **Enter**. Dit maakt de shard-Toewijzingsbeheer en twee shards toevoegen aan de server. Vervolgens typt u '3' en druk op **Enter**; deze actie vier keer herhalen. Dit voorbeeldrijen met gegevens in uw shards ingevoegd.
3. De [Azure-portal](https://portal.azure.com) drie nieuwe databases moeten worden weergegeven:

   ![Visual Studio-bevestiging](./media/sql-database-elastic-query-getting-started/portal.png)

   Op dit moment maken we een aangepaste database-verzameling die overeenkomt met alle databases in de shard-toewijzing. Dit kan we maken en uitvoeren van een taak die wordt een nieuwe tabel toegevoegd in verschillende shards.

Hier wordt meestal maken we een shard-toewijzing als doel, met behulp van de **New-AzureSqlJobTarget** cmdlet. De database voor shard-Toewijzingsbeheer moet worden ingesteld als het doel van een database en vervolgens de specifieke shard-toewijzing is opgegeven als een doel. We gaan in plaats daarvan het inventariseren van alle databases in de server en de databases toevoegen aan de nieuwe aangepaste verzameling met uitzondering van de database master.

## <a name="creates-a-custom-collection-and-add-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Hiermee maakt u een aangepaste verzameling en alle databases op de server toevoegen aan het doel van de aangepaste verzameling met uitzondering van master

   ```Powershell
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
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Maken van een T-SQL-Script voor de uitvoering voor databases
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

## <a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>De taak voor het uitvoeren van een script voor de aangepaste groep databases maken

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-the-job"></a>Het uitvoeren van taak
De volgende PowerShell-script kan worden gebruikt voor het uitvoeren van een bestaande taak:

Update voor de volgende variabele overeenstemming met de naam van de gewenste taak hebt uitgevoerd:

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-the-state-of-a-single-job-execution"></a>De status van het uitvoeren van een enkele taak ophalen
Gebruik dezelfde **Get-AzureSqlJobExecution** cmdlet met de **voor IncludeChildren** parameter om de status van onderliggende taakuitvoeringen, namelijk de specifieke status voor het uitvoeren van elke taak voor elke database weer te geven het doel van de taak.

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-the-state-across-multiple-job-executions"></a>De status over meerdere taakuitvoeringen weergeven
De **Get-AzureSqlJobExecution** cmdlet heeft meerdere optionele parameters die kunnen worden gebruikt om meerdere taakuitvoeringen, gefilterd door de opgegeven parameters weer te geven. Het volgende voorbeeld toont enkele van de mogelijke manieren waarop u met Get-AzureSqlJobExecution:

Alle actieve op het hoogste niveau taakuitvoeringen ophalen:

   ```
    Get-AzureSqlJobExecution
   ```

Alle taakuitvoeringen op het hoogste niveau, met inbegrip van niet-actieve taakuitvoeringen ophalen:

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

Alle onderliggende taakuitvoeringen van een opgegeven taak uitvoerings-ID, met inbegrip van niet-actieve taakuitvoeringen ophalen:

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

Ophalen van alle taakuitvoeringen gemaakt met behulp van een planning / taak combinatie, met inbegrip van niet-actieve taken:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

Alle taken die zijn gericht op een opgegeven shard-toewijzing, met inbegrip van niet-actieve taken ophalen:

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Alle taken die zijn gericht op een opgegeven aangepaste verzameling, met inbegrip van niet-actieve taken ophalen:

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Ophalen van de lijst van de taak taakuitvoeringen binnen een specifieke taak kan worden uitgevoerd:

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

Ophalen van gegevens van taakuitvoering taak:

De volgende PowerShell-script kan worden gebruikt om de details van een taak uitvoeren van taak, die vooral handig is bij het opsporen van fouten van de uitvoering weer te geven.
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a>Fouten in taakuitvoeringen voor de taak ophalen
Het object JobTaskExecution bevat een eigenschap voor de levenscyclus van de taak, samen met de berichteigenschap van een. Als de uitvoering van de taak een taak is mislukt, de levenscyclus van de eigenschap is ingesteld op *mislukt* en de bericht-eigenschap is ingesteld op de resulterende uitzonderingsbericht en de stack. Als een taak is mislukt, is het belangrijk om de details van de taken die niet is gelukt voor een bepaalde taak weer te geven.

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

## <a name="waiting-for-a-job-execution-to-complete"></a>Wachten op het uitvoeren van een taak te voltooien
De volgende PowerShell-script kan worden gebruikt om te wachten op een taak te voltooien:

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a>Een van aangepaste uitvoeringsbeleid maken
Taken voor elastic Database biedt ondersteuning voor het maken van aangepaste uitvoeringsbeleidsregels die kunnen worden toegepast bij het starten van taken.

Uitvoeringsbeleidsregels toestaan op dit moment voor het definiëren van:

* Naam: De id voor het uitvoeringsbeleid.
* De time-out van de taak: Totale tijd voordat een taak is geannuleerd door de taken voor Elastic Database.
* Eerste Interval voor opnieuw proberen: Interval moet worden gewacht voordat de eerste poging.
* Maximaal Interval voor opnieuw proberen: De limiet van interval tussen nieuwe pogingen te gebruiken.
* Interval voor uitstel coëfficiënt voor opnieuw proberen: Coëfficiënt die wordt gebruikt voor het berekenen van de volgende interval tussen nieuwe pogingen.  De volgende formule wordt gebruikt: (eerste Interval voor opnieuw proberen) * Math.pow ((Interval uitstel coëfficiënt) (nummer van de nieuwe pogingen) - 2).
* Maximum aantal pogingen: Het maximale aantal nieuwe pogingen om uit te voeren in een job.

Het standaard-uitvoeringsbeleid maakt gebruik van de volgende waarden:

* Naam: Standaard-uitvoeringsbeleid
* De time-out van de taak: 1 week
* Eerste Interval voor opnieuw proberen: 100 milliseconden
* Maximaal Interval voor opnieuw proberen: 30 minuten
* De coëfficiënt Interval opnieuw proberen: 2
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
Update voor de gewenste uitvoeringsbeleid om bij te werken:

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

Taken voor elastic Database ondersteunt jobs annulering aanvragen.  Als taken voor Elastic Database een aanvraag voor annulering voor een taak die momenteel wordt uitgevoerd detecteert, wordt geprobeerd om de taak te stoppen.

Er zijn twee verschillende manieren dat taken voor Elastic Database een annulering kan uitvoeren:

1. Annuleren momenteel actieve taken: als een annulering wordt gedetecteerd, terwijl een taak wordt momenteel uitgevoerd, een annulering wordt uitgevoerd binnen het momenteel wordt uitgevoerd aspect van de taak.  Bijvoorbeeld: als er een langlopende query's die momenteel wordt uitgevoerd wanneer een annulering wordt uitgevoerd, er is een poging tot annuleren van de query.
2. Annuleren nieuwe pogingen: Als een annulering wordt gedetecteerd door de besturingselement-thread voordat een taak wordt gestart voor de uitvoering, de besturingselement-thread vermijdt u de taak starten en de aanvraag worden gedeclareerd als geannuleerd.

Als een geannuleerde taken voor een bovenliggende taak is aangevraagd, wordt de aanvraag voor annulering herkend voor de bovenliggende taak en alle bijbehorende onderliggende taken.

Om in te dienen een aanvraag voor annulering, gebruikt u de **Stop-AzureSqlJobExecution** cmdlet en stel de **JobExecutionId** parameter.

   ```Powershell
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Een taak door de naam en de geschiedenis van de taak verwijderen

Taken voor elastic Database biedt ondersteuning voor asynchrone verwijderen van taken. Een taak kan worden gemarkeerd voor verwijdering en het systeem worden verwijderd en de taakgeschiedenis van de taak nadat alle taakuitvoeringen voor de taak hebt voltooid. Het systeem wordt niet automatisch geannuleerd voor actieve taakuitvoeringen.  

Stop-AzureSqlJobExecution moet in plaats daarvan worden aangeroepen als u wilt annuleren active taakuitvoeringen.

Voor het activeren van taak verwijderen, gebruikt u de **Remove-AzureSqlJob** cmdlet en stel de **JobName** parameter.

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a>Maak een aangepaste database-doel
Aangepaste database doelen kunnen worden gedefinieerd in de taken voor Elastic Database die kunnen worden gebruikt voor de uitvoering van rechtstreeks of voor opname in de groep van een aangepaste database. Aangezien **elastische pools** niet nog rechtstreeks worden ondersteund via de PowerShell-APIs u Maak eenvoudig een aangepaste database doel en de aangepaste database verzameling target dit alle databases in de groep omvat.

Stel de volgende variabelen in overeenstemming met de gewenste database-informatie:

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a>Maak een verzameling aangepaste database doel
Een doel van de verzameling aangepaste database kan worden gedefinieerd om in te schakelen worden uitgevoerd in meerdere database gedefinieerde doelen. Nadat de groep van een database is gemaakt, is databases kunnen worden gekoppeld aan het doel van de aangepaste verzameling.

Stel de volgende variabelen in overeenstemming met de configuratie van de gewenste aangepaste verzameling doel:

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-to-a-custom-database-collection-target"></a>Databases toevoegen aan een verzameling aangepaste database doel
Doelen van de database kunnen worden gekoppeld aan de doelen van verzameling aangepaste database te maken van een groep databases. Wanneer een taak wordt gemaakt die gericht is op een verzameling aangepaste database doel, wordt het uitgevouwen wilt richten op de databases die zijn gekoppeld aan de groep op het moment van uitvoering.

De gewenste database toevoegen aan een specifieke aangepaste verzameling:

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>De databases binnen een doel van de verzameling aangepaste database bekijken
Gebruik de **Get-AzureSqlJobTarget** cmdlet voor het ophalen van de onderliggende databases binnen een verzameling aangepaste database doel.

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Een taak voor het uitvoeren van een script voor een doel van de verzameling aangepaste database maken
Gebruik de **New-AzureSqlJob** cmdlet om een taak op basis van een groep databases die zijn gedefinieerd door een doel van de verzameling aangepaste database te maken. Taken voor elastic Database breidt de taak naar meerdere onderliggende taken elke overeenkomt met een database die is gekoppeld aan het doel van de verzameling aangepaste database en zorg ervoor dat het script is uitgevoerd voor elke database. Nogmaals, is het belangrijk dat scripts idempotent zijn voor nieuwe pogingen tolerantie.

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a>Het verzamelen van gegevens tussen databases
**Taken voor elastic Database** biedt ondersteuning voor een query uitvoeren voor een groep databases en de resultaten verzenden naar een opgegeven databasetabel. De tabel kan worden opgevraagd achteraf om te zien van de query-resultaten van elke database. Dit is een asynchrone mechanisme voor het uitvoeren van een query voor verschillende databases. Mislukte aanvragen, zoals een van de databases worden tijdelijk niet beschikbaar worden automatisch verwerkt via nieuwe pogingen.

De opgegeven doeltabel wordt automatisch gemaakt als deze nog niet bestaat, die overeenkomt met het schema van de geretourneerde resultaten. Als de uitvoering van een script meerdere resultatensets worden geretourneerd, verzendt taken voor Elastic Database alleen het eerste item aan de tabel van de opgegeven bestemming.

De volgende PowerShell-script kan worden gebruikt voor het uitvoeren van een script voor het verzamelen van de resultaten in een opgegeven tabel. Met dit script wordt ervan uitgegaan dat een T-SQL-script is gemaakt die een set één resultaat weergeeft en een doel van de verzameling aangepaste database is gemaakt.

Stel het volgende in overeenstemming met de gewenste script, referenties en doel voor uitvoering:

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

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Een schema voor het uitvoeren van de taak met behulp van de trigger van een taak maken
De volgende PowerShell-script kan worden gebruikt om een terugkerende planning te maken. Met dit script maakt gebruik van een interval van één minuut, maar nieuwe AzureSqlJobSchedule biedt ook ondersteuning voor de parameters - DayInterval, - HourInterval, - MonthInterval, en -WeekInterval. Schema's die slechts één keer uitvoeren kunnen worden gemaakt door te geven - eenmalig.

Een nieuw schema maken:
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Een taak trigger hebt een taak uitgevoerd op een tijdschema
Een trigger voor de taak kan worden gedefinieerd als een taak uitgevoerd volgens een schema. De volgende PowerShell-script kan worden gebruikt om te maken van een trigger voor de taak.

De volgende variabelen overeen te komen met de gewenste taak en de planning instellen:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Verwijderen van een geplande is gekoppeld aan de taak wordt uitgevoerd op de planning stoppen
Als u wilt stoppen met het uitvoeren van terugkerende taak via een trigger taak, kan de trigger van de taak worden verwijderd.
Verwijderen van een trigger taak voor het stoppen van een taak uit te voeren op basis van een planning met de **Remove-AzureSqlJobTrigger** cmdlet.

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-to-excel"></a>Queryresultaten voor elastische database importeren in Excel
 U kunt de resultaten van een query naar een Excel-bestand importeren.

1. Start Excel 2013.
2. Navigeer naar de **gegevens** lint.
3. Klik op **van andere bronnen** en klikt u op **van SQL Server**.

   ![Excel importeren uit andere bronnen](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. In de **Wizard Gegevensverbinding** typt u de referenties voor de servernaam en aanmeldingsgegevens. Klik op **Volgende**.
5. In het dialoogvenster **selecteert u de database met de gegevens die u wilt dat**, selecteer de **ElasticDBQuery** database.
6. Selecteer de **klanten** tabel in de lijstweergave en klikt u op **volgende**. Klik vervolgens op **voltooien**.
7. In de **importgegevens** formulier, onder **selecteert u hoe u wilt weergeven van deze gegevens in uw werkmap**, selecteer **tabel** en klikt u op **OK**.

Alle rijen uit **klanten** tabel, die zijn opgeslagen in verschillende shards de Excel-werkblad vullen.

## <a name="next-steps"></a>Volgende stappen
Nu kunt u functies van Excel-gegevens. Gebruik de verbindingsreeks met de servernaam, databasenaam en referenties verbinding maken met uw gegevens en BI-integratie-hulpprogramma's van de query's in elastic database. Zorg ervoor dat SQL Server wordt ondersteund als een gegevensbron voor het hulpprogramma. Raadpleeg de query's in elastic database en de externe tabellen net als elke andere SQL Server-database en SQL Server-tabellen waarmee u verbinding met uw hulpprogramma maken wilt.

### <a name="cost"></a>Kosten
Er is geen extra kosten in rekening gebracht voor het gebruik van de functie Elastic Database-query. Hoewel op dit moment deze functie is alleen beschikbaar op Premium en bedrijfskritiek databases en elastische pools als een eindpunt, de shards kunnen zijn van een servicelaag.

Zie voor informatie over de prijzen [prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->

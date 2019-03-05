---
title: Maken en beheren van elastische taken met behulp van PowerShell | Microsoft Docs
description: PowerShell gebruikt voor het beheren van pools voor Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: powershell
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: dc9b58402ed0fcc48d60b51d30f891ffcadddbe0
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57315619"
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a>SQL Database elastische taken met behulp van PowerShell (preview) maken en beheren

De PowerShell-APIs voor **taken voor Elastic Database** (in preview), kunt u bij het definiëren van een groep databases op basis waarvan de scripts worden uitgevoerd. Dit artikel wordt beschreven hoe u kunt maken en beheren van **taken voor Elastic Database** met behulp van PowerShell-cmdlets. Zie [overzicht van elastische taken](sql-database-elastic-jobs-overview.md). 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Zie voor een gratis proefversie [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Een set van databases die zijn gemaakt met de hulpprogramma's voor elastische databases. Zie [aan de slag met elastische Databasehulpprogramma's](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
* **Taken voor elastic Database** PowerShell pakket: Zie [taken voor Elastic Database installeren](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Selecteer uw Azure-abonnement
Selecteer het abonnement dat u uw abonnements-Id moet (**- SubscriptionId**) of de naam van abonnement (**- SubscriptionName**). Als u meerdere abonnementen hebt kunt u uitvoeren de **Get-AzSubscription** cmdlet en kopieert u de informatie over het gewenste abonnement van het resultaat is ingesteld. Wanneer u gegevens van uw abonnement hebt, kunt u de volgende cmdlet om in te stellen van dit abonnement als de standaard, namelijk het doel voor het maken en beheren van taken uitvoeren:

    Select-AzSubscription -SubscriptionId {SubscriptionID}

De [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) wordt aanbevolen voor gebruik voor ontwikkelen en uitvoeren van PowerShell-scripts op basis van de taken voor Elastic Database.

## <a name="elastic-database-jobs-objects"></a>Elastische taken databaseobjecten
De volgende tabel geeft een lijst van alle objecttypen van **taken voor Elastic Database** samen met de beschrijving en de relevante PowerShell APIs.

<table style="width:100%">
  <tr>
    <th>Objecttype</th>
    <th>Description</th>
    <th>Gerelateerde PowerShell-API 's</th>
  </tr>
  <tr>
    <td>Referentie</td>
    <td>Gebruikersnaam en wachtwoord om verbinding te maken met databases voor uitvoering van scripts of toepassingen van DACPACs. <p>Het wachtwoord wordt versleuteld vóór het verzenden naar en opgeslagen in de taken voor Elastic Database-database.  Het wachtwoord worden ontsleuteld door de taken voor Elastic Database-service via de referenties die zijn gemaakt en geüpload van het script voor installatie.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>New-AzureSqlJobCredential</p><p>Set-AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Script</td>
    <td>Transact-SQL-script moet worden gebruikt voor de uitvoering van databases.  Het script moet worden gemaakt voor idempotent zijn, omdat de service wordt opnieuw geprobeerd de uitvoering van het script op fouten.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>New-AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Data-tier-toepassing </a> pakket moet worden toegepast voor alle databases.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>New-AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Database Target</td>
    <td>Naam van database en server die verwijst naar een Azure SQL Database.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>New-AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Doel voor shard-kaart</td>
    <td>De combinatie van een doel voor de database en een referentie op die moet worden gebruikt om te bepalen die zijn opgeslagen in een elastische Database-shard-toewijzing.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>New-AzureSqlJobTarget</p>
    <p>Set-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Doel van de aangepaste verzameling</td>
    <td>Gedefinieerde groep met databases die gezamenlijk gebruiken voor de uitvoering.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>New-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Aangepaste verzameling onderliggende doel</td>
    <td>Database-doel waarnaar wordt verwezen vanuit een aangepaste verzameling.</td>
    <td>
    <p>Add-AzureSqlJobChildTarget</p>
    <p>Remove-AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Taak</td>
    <td>
    <p>Definitie van de parameters voor een taak die kan worden gebruikt voor het activeren van de uitvoering of om te voldoen aan een schema.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>New-AzureSqlJob</p>
    <p>Set-AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Uitvoeren van taak</td>
    <td>
    <p>Container van taken die nodig zijn om te voldoen aan het uitvoeren van een script of een DACPAC toepassen op een doel met referenties voor databaseverbindingen met fouten in overeenstemming met een uitvoeringsbeleid behandeld.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Stop-AzureSqlJobExecution</p>
    <p>Wait-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Taak uitvoeren van taak</td>
    <td>
    <p>Één eenheid van het werk om te voldoen aan een taak.</p>
    <p>Als een taak is niet kan worden uitgevoerd, de resulterende uitzonderingsbericht worden geregistreerd en een nieuwe taak overeenkomende worden gemaakt en uitgevoerd in overeenstemming met het opgegeven RemoteSigned.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Stop-AzureSqlJobExecution</p>
    <p>Wait-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Taak-uitvoeringsbeleid</td>
    <td>
    <p>Besturingselementen-taak uitvoeren-outs, limieten voor opnieuw proberen en intervallen tussen nieuwe pogingen.</p>
    <p>Taken voor elastic Database bevat een standaard taak uitvoeringsbeleid waardoor in feite oneindige nieuwe pogingen van de mislukte taak met exponentieel uitstel van intervallen tussen nieuwe pogingen.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>New-AzureSqlJobExecutionPolicy</p>
    <p>Set-AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Planning</td>
    <td>
    <p>Specificatie voor de uitvoering om te worden toegepast op een terugkerende interval of in één keer op basis van tijd.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>New-AzureSqlJobSchedule</p>
    <p>Set-AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Taak wordt geactiveerd</td>
    <td>
    <p>Een toewijzing tussen een taak en een schema uitvoeren volgens de planning van de taak.</p>
    </td>
    <td>
    <p>New-AzureSqlJobTrigger</p>
    <p>Remove-AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Ondersteunde taken voor Elastic Database groep typen
De taak wordt uitgevoerd van Transact-SQL (T-SQL)-scripts of de toepassing van DACPACs voor een groep databases. Wanneer een job wordt verzonden om te worden uitgevoerd voor een groep databases, de taak 'uitgebreid"de in onderliggende taken waarbij elk het aangevraagde uitvoeren op een afzonderlijke database in de groep uitvoert. 

Er zijn twee typen groepen die u kunt maken: 

* [Shard-toewijzing](sql-database-elastic-scale-shard-map-management.md) groep: Als een taak wordt verzonden naar het doel van een shard-toewijzing, wordt de taak query's de shard-toewijzing om te bepalen van de huidige set met shards en maakt vervolgens onderliggende taken voor elke shard in de shard-toewijzing.
* Aangepaste verzameling groep: Een aangepaste set met databases. Wanneer een taak is gericht op een aangepaste verzameling, maakt het onderliggende taken voor elke database die momenteel in de volgende zelfgemaakte collectie.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Taken voor het instellen van de elastische Database verbinding
Een verbinding moet worden ingesteld voor de taken *control database* voordat u met behulp van de taken API's. Deze cmdlet uitvoert, wordt een venster van de referentie naar het pop-aanvragen van de gebruikersnaam en wachtwoord die zijn gemaakt bij het installeren van de taken voor Elastic Database geactiveerd. Alle voorbeelden in dit onderwerp wordt ervan uitgegaan dat al deze eerste stap is uitgevoerd.

Open een verbinding met de taken voor Elastic Database:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Versleutelde referenties binnen de taken voor Elastic Database
Databasereferenties kunnen worden ingevoegd in de taken *control database* met het wachtwoord dat is versleuteld. Het is nodig voor het opslaan van referenties zodat taken worden uitgevoerd op een later tijdstip (met behulp van de taakschema's).

Versleuteling werkt via een certificaat dat is gemaakt als onderdeel van het script voor installatie. Het installatiescript maakt en uploadt het certificaat in de Azure-Cloudservice voor het decoderen van de opgeslagen gecodeerde wachtwoorden. De openbare sleutel in de taken in de Azure-Cloudservice later worden opgeslagen *control database* waarmee de PowerShell-API of Azure portal-interface voor het versleutelen van een opgegeven wachtwoord zonder het certificaat moet lokaal worden geïnstalleerd .

De referentie-wachtwoorden zijn versleuteld en beveiligd tegen gebruikers met alleen-lezen toegang tot Elastic Database jobs objecten. Maar het is mogelijk dat een kwaadwillende gebruiker met lees-/ schrijftoegang tot taken voor Elastic Database-objecten om op te halen van een wachtwoord. Referenties zijn ontworpen om te worden hergebruikt voor taakuitvoeringen. Referenties worden doorgegeven aan doeldatabases bij het maken van verbindingen. Er zijn momenteel geen beperkingen voor de doeldatabases die worden gebruikt voor elke referentie, kwaadwillende gebruiker kan een database-doel voor een database onder beheer van de kwaadwillende gebruiker toevoegen. De gebruiker kan vervolgens een taak die gericht is op deze database te krijgen van de referentie wachtwoord starten.

Aanbevolen beveiligingsprocedures voor taken voor Elastic Database zijn onder andere:

* Beperk het gebruik van de API's tot vertrouwde personen.
* Referenties zijn de minimaal benodigde bevoegdheden die nodig zijn om uit te voeren van de taak.  Meer informatie kan worden weergegeven in deze [autorisatie en machtigingen](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server-MSDN-artikel.

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Een versleutelde referentie voor de taakuitvoering van de voor databases maken
Een nieuwe versleutelde referentie maken de [ **cmdlet Get-Credential** ](/powershell/module/microsoft.powershell.security/get-credential) gevraagd een gebruikersnaam en wachtwoord die kan worden doorgegeven aan de [ **cmdlet New-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Referenties bijwerken
Wanneer wachtwoorden wijzigen, gebruikt u de [ **cmdlet Set-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) en stel de **CredentialName** parameter.

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Voor het definiëren van een elastische Database shard map doel
Voor het uitvoeren van een taak voor alle databases in een shard-verzameling (gemaakt met behulp van [Elastic Database-clientbibliotheek](sql-database-elastic-database-client-library.md)), een shard-toewijzing te gebruiken als het doel van de database. In dit voorbeeld is een shard-toepassing gemaakt met behulp van de clientbibliotheek voor Elastic Database vereist. Zie [aan de slag met hulpprogramma's elastische Database voor klantorders](sql-database-elastic-scale-get-started.md).

De database voor shard-Toewijzingsbeheer moet worden ingesteld als het doel van een database en vervolgens de specifieke shard-toewijzing moet worden opgegeven als een doel.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Maken van een T-SQL-Script voor de uitvoering voor databases
Bij het maken van T-SQL-scripts voor de uitvoering, is het raadzaam om te bouwen om te worden [idempotent](https://en.wikipedia.org/wiki/Idempotence) en flexibele tegen fouten. Taken voor elastic Database wordt opnieuw geprobeerd van uitvoering van een script als een storing optreedt, ongeacht de indeling van de fout wordt aangetroffen die worden uitgevoerd.

Gebruik de [ **cmdlet New-AzureSqlJobContent** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) maken en opslaan van een script voor uitvoering en stel de **- ContentName** en **- CommandText** de parameters.

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

### <a name="create-a-new-script-from-a-file"></a>Een nieuw script maken van een bestand
Als de T-SQL-script is gedefinieerd in een bestand, gebruikt u dit voor het importeren van het script:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Bijwerken van een T-SQL-script voor uitvoering tussen databases
Dit PowerShell-script werkt de T-SQL-opdrachttekst voor een bestaand script.

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

### <a name="to-update-the-definition-to-an-existing-script"></a>De definitie bijwerken naar een bestaand script
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Om een taak voor het uitvoeren van een script in een shard-toewijzing te maken
Dit PowerShell-script start een taak voor uitvoering van een script dat in elke shard in een shard-toewijzing voor elastisch schalen.

Stel de volgende variabelen in overeenstemming met de gewenste script en het doel:

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
Dit PowerShell-script wordt een bestaande taak uitgevoerd:

Update voor de volgende variabele overeenstemming met de naam van de gewenste taak hebt uitgevoerd:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>De status van het uitvoeren van een enkele taak ophalen
Gebruik de [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) en stel de **JobExecutionId** parameter om de status van het uitvoeren van taak weer te geven.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Gebruik dezelfde **Get-AzureSqlJobExecution** cmdlet met de **voor IncludeChildren** parameter om de status van onderliggende taakuitvoeringen, namelijk de specifieke status voor het uitvoeren van elke taak voor elke database weer te geven het doel van de taak.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>De status over meerdere taakuitvoeringen weergeven
De [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) meerdere optionele parameters die kunnen worden gebruikt om meerdere taakuitvoeringen, gefilterd door de opgegeven parameters weer te geven. Het volgende voorbeeld toont enkele van de mogelijke manieren waarop u met Get-AzureSqlJobExecution:

Alle actieve bovenste niveau taakuitvoeringen ophalen:

    Get-AzureSqlJobExecution

Alle taakuitvoeringen van de bovenste niveau, met inbegrip van niet-actieve taakuitvoeringen ophalen:

    Get-AzureSqlJobExecution -IncludeInactive

Alle onderliggende taakuitvoeringen van een opgegeven taak uitvoerings-ID, met inbegrip van niet-actieve taakuitvoeringen ophalen:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

Ophalen van alle taakuitvoeringen gemaakt met behulp van een planning / taak combinatie, met inbegrip van niet-actieve taken:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Alle taken die zijn gericht op een opgegeven shard-toewijzing, met inbegrip van niet-actieve taken ophalen:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Alle taken die zijn gericht op een opgegeven aangepaste verzameling, met inbegrip van niet-actieve taken ophalen:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Ophalen van de lijst van de taak taakuitvoeringen binnen een specifieke taak kan worden uitgevoerd:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Ophalen van gegevens van taakuitvoering taak:

De volgende PowerShell-script kan worden gebruikt om de details van een taak uitvoeren van taak, die vooral handig is bij het opsporen van fouten van de uitvoering weer te geven.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>Om op te halen in taak taakuitvoeringen toepassingsfouten
De **JobTaskExecution object** bevat een eigenschap voor de levenscyclus van de taak, samen met de berichteigenschap van een. Als de uitvoering van de taak een taak is mislukt, de levenscyclus van de eigenschap wordt ingesteld op *mislukt* en de berichteigenschap zal worden ingesteld op de resulterende uitzonderingsbericht en de stack. Als een taak is mislukt, is het belangrijk om de details van de taken die niet is gelukt voor een bepaalde taak weer te geven.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Na afloop van het uitvoeren van een taak te voltooien
De volgende PowerShell-script kan worden gebruikt om te wachten op een taak te voltooien:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Een van aangepaste uitvoeringsbeleid maken
Taken voor elastic Database biedt ondersteuning voor het maken van aangepaste uitvoeringsbeleidsregels die kunnen worden toegepast bij het starten van taken.

Uitvoeringsbeleidsregels toestaan op dit moment voor het definiëren van:

* Naam: De id voor het uitvoeringsbeleid.
* De time-out van de taak: Totale tijd voordat een taak wordt geannuleerd door de taken voor Elastic Database.
* Interval voor eerste opnieuw proberen: Het interval moet worden gewacht voordat de eerste poging.
* Interval voor maximale aantal nieuwe pogingen: De limiet van interval tussen nieuwe pogingen te gebruiken.
* Interval voor uitstel coëfficiënt voor opnieuw proberen: De coëfficiënt gebruikt voor het berekenen van de volgende interval tussen nieuwe pogingen.  De volgende formule wordt gebruikt: (De eerste Interval voor opnieuw proberen) * Math.pow ((Interval uitstel coëfficiënt) (aantal nieuwe pogingen) - 2). 
* Maximum aantal pogingen: Het maximale aantal nieuwe pogingen om uit te voeren in een job.

Het standaard-uitvoeringsbeleid maakt gebruik van de volgende waarden:

* Naam: Standaard-uitvoeringsbeleid
* De time-out van de taak: 1 week
* Interval voor eerste opnieuw proberen:  100 milliseconden
* Interval voor maximale aantal nieuwe pogingen: 30 minuten
* De coëfficiënt Interval voor opnieuw proberen: 2
* Maximum aantal pogingen: 2,147,483,647

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
Update voor de gewenste uitvoeringsbeleid om bij te werken:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a>Een taak annuleren
Taken voor elastic Database biedt ondersteuning voor annulering aanvragen van taken.  Als taken voor Elastic Database een aanvraag voor annulering voor een taak die momenteel wordt uitgevoerd detecteert, zal worden geprobeerd om de taak te stoppen.

Er zijn twee verschillende manieren dat taken voor Elastic Database een annulering kan uitvoeren:

1. Annuleren momenteel actieve taken: Als een annulering wordt gedetecteerd, terwijl een taak wordt momenteel uitgevoerd, wordt bij een annulering worden uitgevoerd binnen het momenteel wordt uitgevoerd aspect van de taak.  Bijvoorbeeld: Als er een langlopende query's die momenteel wordt uitgevoerd wanneer een annulering wordt uitgevoerd, wordt er een poging tot annuleren van de query.
2. Annuleren van nieuwe pogingen: Als bij een annulering wordt gedetecteerd door de besturingselement-thread voordat een taak voor uitvoering is gestart, wordt de besturingselement-thread te voorkomen dat de taak starten en Declareer van de aanvraag, zoals geannuleerd.

Als een geannuleerde taken voor een bovenliggende taak is aangevraagd, wordt de aanvraag voor annulering voor de bovenliggende taak en alle bijbehorende onderliggende taken worden uitgevoerd.

Om in te dienen een aanvraag voor annulering, gebruikt u de [ **cmdlet Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) en stel de **JobExecutionId** parameter.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Een taak verwijderen en asynchroon Taakgeschiedenis
Taken voor elastic Database biedt ondersteuning voor asynchrone verwijderen van taken. Een taak kan worden gemarkeerd voor verwijdering en het systeem en de taakgeschiedenis van de taak wordt verwijderd nadat alle taakuitvoeringen voor de taak hebt voltooid. Het systeem wordt niet automatisch geannuleerd voor actieve taakuitvoeringen.  

Aanroepen [ **Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) uitvoeringen door een actieve taak annuleren.

Voor het activeren van taak verwijderen, gebruikt u de [ **cmdlet Remove-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) en stel de **JobName** parameter.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="to-create-a-custom-database-target"></a>Om het doel van een aangepaste database te maken
U kunt aangepaste database doelen voor de uitvoering van directe of voor opname in de groep van een aangepaste database definiëren. Bijvoorbeeld, omdat **elastische pools** zijn nog niet rechtstreeks ondersteund met behulp van PowerShell-APIs, kunt u een aangepaste database doel en de aangepaste database verzameling target dit alle databases in de groep omvat.

Stel de volgende variabelen in overeenstemming met de gewenste database-informatie:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Om een doel van de verzameling aangepaste database te maken
Gebruik de [ **New-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet voor het definiëren van een doel van de verzameling aangepaste database zodat kan worden uitgevoerd in meerdere database gedefinieerde doelen. Na het maken van een databasegroep, kunnen de databases worden gekoppeld aan het doel van de aangepaste verzameling.

Stel de volgende variabelen in overeenstemming met de configuratie van de gewenste aangepaste verzameling doel:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Databases toevoegen aan een verzameling aangepaste database doel
Een database toevoegen aan een specifieke aangepaste verzameling gebruikt de [ **toevoegen AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>De databases binnen een doel van de verzameling aangepaste database bekijken
Gebruik de [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet voor het ophalen van de onderliggende databases binnen een verzameling aangepaste database doel. 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Een taak voor het uitvoeren van een script voor een doel van de verzameling aangepaste database maken
Gebruik de [ **New-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) cmdlet om een taak op basis van een groep databases die zijn gedefinieerd door een doel van de verzameling aangepaste database te maken. Taken voor elastic Database wordt de taak naar meerdere onderliggende taken elke overeenkomt met een database die is gekoppeld aan het doel van de verzameling aangepaste database wilt uitbreiden en zorg ervoor dat het script is uitgevoerd voor elke database. Nogmaals, is het belangrijk dat scripts idempotent zijn voor nieuwe pogingen tolerantie.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Het verzamelen van gegevens tussen databases
Een taak kunt u een query uitvoeren voor een groep databases en de resultaten verzenden naar een specifieke tabel. De tabel kan worden opgevraagd achteraf om te zien van de query-resultaten van elke database. Dit is een asynchrone methode voor het uitvoeren van een query voor verschillende databases. Mislukte pogingen worden automatisch verwerkt via nieuwe pogingen.

De opgegeven doeltabel wordt automatisch gemaakt als deze nog niet bestaat. De nieuwe tabel komt overeen met het schema van de geretourneerde resultaten. Als meerdere resultatensets worden geretourneerd door een script, wordt taken voor Elastic Database alleen de eerste verzenden naar de doeltabel.

De volgende PowerShell-script een script wordt uitgevoerd en de resultaten in een opgegeven tabel verzamelt. Met dit script wordt ervan uitgegaan dat er een T-SQL-script is gemaakt die een set één resultaat weergeeft en dat er een doel van de verzameling aangepaste database is gemaakt.

Dit script gebruikt de [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet. Stel de parameters voor het script, referenties en doel voor uitvoering:

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Om te maken en starten van een taak voor scenario's voor het verzamelen van gegevens
Dit script gebruikt de [ **Start AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.

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

## <a name="to-schedule-a-job-execution-trigger"></a>Voor het plannen van een taak uitvoeren-trigger
De volgende PowerShell-script kan worden gebruikt om een terugkerende planning te maken. Met dit script maakt gebruik van een interval van minuten, maar [ **New-AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) biedt ook ondersteuning voor de parameters - DayInterval, - HourInterval, - MonthInterval, en -WeekInterval. Schema's die slechts één keer uitvoeren kunnen worden gemaakt door te geven - eenmalig.

Een nieuw schema maken:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Voor het activeren van een taak uitgevoerd op een tijdschema
Een trigger voor de taak kan worden gedefinieerd als een taak uitgevoerd volgens een schema. De volgende PowerShell-script kan worden gebruikt om te maken van een trigger voor de taak.

Gebruik [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) en stelt u de volgende variabelen overeen te komen met de gewenste taak en de planning:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Voor het verwijderen van een geplande is gekoppeld aan de taak wordt uitgevoerd op de planning stoppen
Als u wilt stoppen met het uitvoeren van terugkerende taak via een trigger taak, kan de trigger van de taak worden verwijderd. Verwijderen van een trigger taak voor het stoppen van een taak uit te voeren op basis van een planning met de [ **cmdlet Remove-AzureSqlJobTrigger**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Taak triggers die zijn gebonden aan een tijdschema ophalen
De volgende PowerShell-script kan worden gebruikt om toegang te verkrijgen en weergeven van de taak triggers geregistreerd voor een bepaald tijdschema.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Als u wilt ophalen van de taak triggers afhankelijk is van een taak
Gebruik [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) te verkrijgen en schema's met een geregistreerde taak weer te geven.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Een data-tier-toepassing (DACPAC) voor de uitvoering voor databases maken
Zie voor het maken van een DACPAC [Data-Tier applications](https://msdn.microsoft.com/library/ee210546.aspx). Voor het implementeren van een DACPAC, gebruikt u de [cmdlet New-AzureSqlJobContent](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent). De DACPAC moet toegankelijk zijn voor de service. Het verdient aanbeveling een gemaakte DACPAC uploaden naar Azure Storage en maken een [Shared Access Signature](../storage/common/storage-dotnet-shared-access-signature-part-1.md) voor de DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Bijwerken van een data-tier-toepassing (DACPAC) voor de uitvoering voor databases
Bestaande DACPACs geregistreerd binnen taken voor Elastic Database kunnen worden bijgewerkt om te verwijzen naar een nieuwe URI's. Gebruik de [ **cmdlet Set-AzureSqlJobContentDefinition** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) geregistreerd om bij te werken van de URI DACPAC op een bestaande DACPAC:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Een taak voor het toepassen van een data-tier-toepassing (DACPAC) voor databases maken
Nadat een DACPAC binnen taken voor Elastic Database is gemaakt, kunt u een taak gemaakt om toe te passen de DACPAC voor een groep databases. De volgende PowerShell-script kan worden gebruikt voor een DACPAC-taak maken via een aangepaste verzameling van databases:

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

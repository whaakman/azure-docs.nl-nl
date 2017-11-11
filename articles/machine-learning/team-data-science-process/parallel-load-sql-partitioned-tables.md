---
title: Bouwen en tabellen voor het snel parallelle importeren van gegevens in een SQL-Server op een Azure VM optimaliseren | Microsoft Docs
description: Parallel bulkimporteren van gegevens met SQL-partitietabellen
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: ff90fdb0-5bc7-49e8-aee7-678b54f901c8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: bradsev
ms.openlocfilehash: 77638ff52edbc2b782b21a4ca1c727a2b46f22f3
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Parallel bulkimporteren van gegevens met SQL-partitietabellen
Dit document wordt beschreven hoe u gepartitioneerde tabellen voor het snel parallelle bulkbewerkingen voor importeren van gegevens naar een SQL Server-database. Voor grote laden/overdracht van gegevens naar een SQL-database, het importeren van gegevens naar de SQL-database en de volgende query's kan worden verbeterd via *gepartitioneerde tabellen en weergaven*. 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Maak een nieuwe database en een set bestandsgroepen
* [Maak een nieuwe database](https://technet.microsoft.com/library/ms176061.aspx), als deze niet al bestaat.
* Database-bestandsgroepen toevoegen aan de database, die de gepartitioneerde fysieke bestanden bevat. 
* Dit kan worden gedaan met [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) als de nieuwe of [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) als de database al bestaat.
* Een of meer bestanden (indien nodig) toevoegen aan de bestandsgroep van elke database.
  
  > [!NOTE]
  > Geef de doel-bestandsgroep, die gegevens voor deze partitie bevat en waar de gegevens van de bestandsgroep worden opgeslagen of hostnamen de fysieke database.
  > 
  > 

Het volgende voorbeeld maakt een nieuwe database met drie bestandsgroepen naast de primaire en logboekregistratie groepen, met een fysiek bestand in elk. De databasebestanden zijn gemaakt in de standaardmap SQL Server-gegevens, zoals geconfigureerd in de SQL Server-exemplaar. Zie voor meer informatie over de standaardbestandslocaties [bestandslocaties voor standaard- en benoemde exemplaren van SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

    EXECUTE ('
        CREATE DATABASE <database_name>
         ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
         LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')

## <a name="create-a-partitioned-table"></a>Een gepartitioneerde tabel maken
Voor het maken van gepartitioneerde tabellen volgens het gegevensschema, toegewezen aan de database-bestandsgroepen gemaakt in de vorige stap, moet u eerst een partitiefunctie en schema maken. Als gegevens bulksgewijs naar de gepartitioneerde tabel(len) geïmporteerd, worden de records verdeeld over de bestandsgroepen volgens een partitieschema, zoals hieronder wordt beschreven.

### <a name="1-create-a-partition-function"></a>1. De partitiefunctie van een maken
[Maken van een partitiefunctie](https://msdn.microsoft.com/library/ms187802.aspx) deze functie definieert het bereik van waarden/grenzen worden opgenomen in elke afzonderlijke partitietabel, bijvoorbeeld, om te beperken van partities per maand (sommige\_datetime\_veld) in het jaar 2013:
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

### <a name="2-create-a-partition-scheme"></a>2. Maken van een partitieschema opgeven
[Maken van een partitieschema](https://msdn.microsoft.com/library/ms179854.aspx). Dit schema wordt bijvoorbeeld elk partitiebereik in de partitiefunctie aan een fysieke bestandsgroep toegewezen:
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  Om te controleren of de bereiken van kracht in elke partitie volgens de functie /-schema, voer de volgende query:
  
        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

### <a name="3-create-a-partition-table"></a>3. Een partitietabel maken
[Gepartitioneerde tabel maken](https://msdn.microsoft.com/library/ms174979.aspx)(s) op basis van uw gegevensschema en geeft u de partitie schema en beperking veld dat wordt gebruikt voor het partitioneren van de tabel, bijvoorbeeld:
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Zie voor meer informatie [gepartitioneerde tabellen maken en indexen](https://msdn.microsoft.com/library/ms188730.aspx).

## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>De gegevens voor elke afzonderlijke partitietabel bulkimport

* U kunt BCP BULK INSERT of andere methoden, zoals [SQL Server Migration Wizard](http://sqlazuremw.codeplex.com/). Het opgegeven voorbeeld maakt gebruik van de BCP-methode.
* [De database wijzigen](https://msdn.microsoft.com/library/bb522682.aspx) transactie logboekregistratie schema wijzigen in BULK_LOGGED minimaliseren overhead van het aan te melden, bijvoorbeeld:
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* Het laden van gegevens sneller, start u de bulkbewerkingen voor importeren parallel. Zie voor tips over bespoedigen bulksgewijs importeren van big data in SQL Server-databases [laden van 1TB in minder dan 1 uur](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

De volgende PowerShell-script is een voorbeeld van een parallelle gegevens laden met BCP.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0

    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>

    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"

    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir

    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }

    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }

    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }

    Get-Job

    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Maken van indexen voor het optimaliseren van joins en prestaties van query 's
* Als u gegevens voor modellering uit meerdere tabellen extraheren, maakt u indexen voor de join-sleutels om de join-prestaties te verbeteren.
* [Maken van indexen](https://technet.microsoft.com/library/ms188783.aspx) (geclusterde of niet-geclusterde) die gericht is op de dezelfde bestandsgroep voor elke partitie voor bijvoorbeeld:
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  Of,
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > U kunt ervoor kiezen de indexen voordat bulksgewijs importeren van gegevens. Maken van een index voor het bulksgewijs importeren vertraagt de het laden van gegevens.
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Geavanceerde analyses proces en de technologie in actie voorbeeld
Zie voor een overzicht van de end-to-end voorbeeld met het Team gegevens wetenschap proces met een openbare gegevensset, [Team gegevens wetenschappelijke processen in actie: met behulp van SQL Server](sql-walkthrough.md).


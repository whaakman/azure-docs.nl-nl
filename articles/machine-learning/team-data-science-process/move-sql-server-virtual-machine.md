---
title: Gegevens verplaatsen naar SQL Server op een virtuele machine in Azure | Microsoft Docs
description: Gegevens uit platte bestanden of verplaatsen van een lokale SQL Server met SQL Server op Azure VM.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 2c9ef1d3-4f5c-4b1f-bf06-223646c8af06
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: b8c936163e8e0880d3518f44dba107a0393fd11f
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Gegevens verplaatsen naar SQL Server op een virtuele Azure-machine
In dit onderwerp bevat een overzicht van de opties voor het verplaatsen van gegevens uit platte bestanden (TSV- of CSV-indeling) of van een lokale SQL Server naar SQL Server op Azure een virtuele machine. Deze taken voor het verplaatsen van gegevens naar de cloud maken deel uit van het Team gegevens wetenschap proces.

Zie voor in dit onderwerp bevat een overzicht van de opties voor het verplaatsen van gegevens naar een Azure SQL Database voor Machine Learning, [gegevens verplaatsen naar een Azure SQL Database voor Azure Machine Learning](move-sql-azure.md).

De **menu** onderstaande koppelingen naar onderwerpen waarin wordt beschreven hoe u opnemen van gegevens in andere omgevingen doel waar de gegevens kunnen worden opgeslagen en verwerkt tijdens het Team gegevens wetenschap proces (TDSP).

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

De volgende tabel geeft een overzicht van de opties voor het verplaatsen van gegevens naar SQL Server op Azure een virtuele machine.

| <b>BRON</b> | <b>BESTEMMING: SQL Server op Azure VM</b> |
| --- | --- |
| <b>Plat bestand</b> |1. <a href="#insert-tables-bcp">Hulpprogramma voor opdrachtregelprogramma voor het bulksgewijs kopiëren (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">Bulksgewijs invoegen SQL-Query</a><br> 3. <a href="#sql-builtin-utilities">Grafische ingebouwde hulpprogramma's in SQL Server</a> |
| <b>On-Premises SQL Server</b> |1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Implementeren van een SQL Server-Database naar een Microsoft Azure VM-wizard</a><br> 2. <a href="#export-flat-file">Exporteren naar een plat bestand</a><br> 3. <a href="#sql-migration">Wizard voor migratie van SQL-Database</a> <br> 4. <a href="#sql-backup">Back-up van database maken en terugzetten</a><br> |

Let op: dit document wordt ervan uitgegaan dat de SQL-opdrachten worden uitgevoerd vanuit SQL Server Management Studio of Visual Studio Database Explorer.

> [!TIP]
> Als alternatief kunt u [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) maken en plannen van een pijplijn die de gegevens naar een virtuele machine van SQL Server op Azure verplaatsen. Zie voor meer informatie [kopiëren van gegevens met Azure Data Factory (Kopieeractiviteit)](../../data-factory/v1/data-factory-data-movement-activities.md).
>
>

## <a name="prereqs"></a>Vereisten
Deze zelfstudie wordt ervan uitgegaan dat u hebt:

* Een **Azure-abonnement**. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Een **Azure storage-account**. U wordt een Azure storage-account gebruiken voor het opslaan van de gegevens in deze zelfstudie. Zie het artikel [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen account Azure-opslagaccount hebt. Nadat u het opslagaccount hebt gemaakt, moet u de accountsleutel gebruikt voor toegang tot de opslag verkrijgen. Zie [beheren van uw toegangssleutels voor opslag](../../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Ingericht **SQL Server op een virtuele machine van Azure**. Zie voor instructies [instellen van een virtuele machine van Azure SQL-Server als een server IPython Notebook voor geavanceerde analyses](../data-science-virtual-machine/setup-sql-server-virtual-machine.md).
* Geïnstalleerd en geconfigureerd **Azure PowerShell** lokaal. Zie voor instructies [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

## <a name="filesource_to_sqlonazurevm"></a>Verplaatsen van gegevens van een plat bestand-bron met SQL Server op een Azure VM
Als uw gegevens zich in een plat bestand (gerangschikt in een indeling rij/kolom), kan het worden verplaatst naar SQL Server VM op Azure via de volgende methoden:

1. [Hulpprogramma voor opdrachtregelprogramma voor het bulksgewijs kopiëren (BCP)](#insert-tables-bcp)
2. [Bulksgewijs invoegen SQL-Query](#insert-tables-bulkquery)
3. [Grafische ingebouwde hulpprogramma's in SQL Server (voor importeren/exporteren, SSIS)](#sql-builtin-utilities)

### <a name="insert-tables-bcp"></a>Hulpprogramma voor opdrachtregelprogramma voor het bulksgewijs kopiëren (BCP)
BCP is een opdrachtregelprogramma dat is geïnstalleerd met SQL Server en is een van de snelste manieren om gegevens te verplaatsen. Voor alle drie SQL Server varianten (On-premises SQL Server, SQL Azure en SQL Server VM op Azure) werkt.

> [!NOTE]
> **Waar moet mijn gegevens voor BCP**  
> Hoewel niet vereist, kunt dat bestanden met brongegevens zich op dezelfde computer als de doel-SQL-Server u sneller overdrachten (netwerk snelheid tegenover lokale schijf i/o-snelheid). Kunt u de platte bestanden met gegevens met de computer waarop SQL Server is geïnstalleerd met behulp van verschillende kopiëren hulpprogramma's zoals [AZCopy](../../storage/common/storage-use-azcopy.md), [Azure Opslagverkenner](http://storageexplorer.com/) of windows kopiëren en plakken via Extern bureaublad Protocol (RDP).
>
>

1. Zorg ervoor dat de database en de tabellen op de doel-SQL Server-database worden gemaakt. Hier volgt een voorbeeld van hoe u doet dit met behulp van de `Create Database` en `Create Table` opdrachten:

        CREATE DATABASE <database_name>

        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        )
2. Genereren van het indelingsbestand dat het schema voor de tabel beschrijft de volgende opdracht vanaf de opdrachtregel van de machine waarop bcp is geïnstalleerd.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
3. De gegevens invoegen in de database met behulp van de bcp-opdracht als volgt. Dit moet werken vanaf de opdrachtregel ervan uitgaande dat de SQL-Server is geïnstalleerd op dezelfde machine:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **BCP wordt ingevoegd optimaliseren** raadpleegt u het volgende artikel ['Richtlijnen voor het optimaliseren van bulkimport'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) dergelijke voegt optimaliseren.
>
>

### <a name="insert-tables-bulkquery-parallel"></a>Voegt parallelizing voor snellere verplaatsing van gegevens
Als de gegevens die u verplaatst groot is, kunt u dingen sneller maken door het gelijktijdig uitvoeren van meerdere BCP opdrachten parallel in een PowerShell-Script.

> [!NOTE]
> **BIG data opname** om te optimaliseren voor grote en zeer grote gegevenssets is laden van gegevens, partitie-uw logische en fysieke databasetabellen met meerdere tabellen voor bestandsgroepen en partitie. Zie voor meer informatie over het maken en laden van gegevens naar partitietabellen [parallelle Load SQL-partitietabellen](parallel-load-sql-partitioned-tables.md).
>
>

De PowerShell-voorbeeldscript hieronder laten zien parallelle voegt met behulp van bcp:

    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n
      }


    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }


    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }

    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Bulksgewijs invoegen SQL-Query
[Bulksgewijs invoegen SQL-Query](https://msdn.microsoft.com/library/ms188365) kan worden gebruikt om gegevens te importeren vanuit de rij/kolom op basis van bestanden in de database (de ondersteunde typen worden behandeld in de[voorbereiden-gegevens voor bulksgewijs wilt exporteren naar of importeren (SQL Server)](https://msdn.microsoft.com/library/ms188609)) onderwerp.

Hier volgen enkele Voorbeeldopdrachten voor Bulk Insert zijn zoals hieronder:  

1. Uw gegevens analyseren en eventuele aangepaste opties instellen voordat u importeert om ervoor te zorgen dat de SQL Server-database wordt ervan uitgegaan dezelfde indeling voor de speciale velden zoals datums dat. Hier volgt een voorbeeld van hoe de datumnotatie instellen als jaar, maand, dag (als uw gegevens de datum in de notatie voor jaar, maand, dag bevat):

        SET DATEFORMAT ymd;    
2. Gegevens importeren met bulksgewijs importinstructies:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )

### <a name="sql-builtin-utilities"></a>Ingebouwde hulpprogramma's in SQL Server
SQL Server integraties Services (SSIS) kunt u gegevens in SQL Server VM op Azure uit een plat bestand importeren.
SSIS is beschikbaar in twee studio omgevingen. Zie voor meer informatie [Integration Services (SSIS) en Studio omgevingen](https://technet.microsoft.com/library/ms140028.aspx):

* Zie voor meer informatie over SQL Server Data Tools [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
* Zie voor meer informatie over de Wizard importeren/exporteren [Wizard SQL Server importeren en exporteren](https://msdn.microsoft.com/library/ms141209.aspx)

## <a name="sqlonprem_to_sqlonazurevm"></a>Verplaatsen van gegevens uit on-premises SQL Server met SQL Server op een Azure VM
U kunt ook de volgende migratiestrategieën gebruiken:

1. [Implementeren van een SQL Server-Database naar een Microsoft Azure VM-wizard](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Exporteren naar platte bestanden](#export-flat-file)
3. [Wizard voor migratie van SQL-Database](#sql-migration)
4. [Back-up van database maken en terugzetten](#sql-backup)

We beschrijven elk van deze hieronder:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Implementeren van een SQL Server-Database naar een Microsoft Azure VM-wizard
De **implementeren van een SQL Server-Database naar een Microsoft Azure VM-wizard** is een eenvoudige en de aanbevolen manier om gegevens te verplaatsen van een lokale SQL Server-exemplaar naar SQL Server op een Azure VM. Zie voor gedetailleerde stappen evenals een bespreking van de alternatieven, [migreren van een database met SQL Server op een virtuele machine van Azure](../../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Exporteren naar platte bestanden
Verschillende methoden kunnen worden gebruikt voor het bulksgewijs gegevens uit een On-Premises SQL Server exporteren, zoals beschreven in de [bulksgewijs importeren en exporteren van gegevens (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) onderwerp. Dit document wordt uitgelegd hoe de bulksgewijs kopiëren programma (BCP) als voorbeeld. Wanneer gegevens worden geëxporteerd naar een plat bestand, kan deze worden geïmporteerd in een andere SQL-server via bulkimport.

1. De gegevens uit on-premises SQL-Server te exporteren naar een bestand met het hulpprogramma bcp als volgt

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. De database en de tabel op SQL Server VM maken over het gebruik van Azure de `create database` en `create table` voor het tabelschema geëxporteerd in stap 1.
3. Maak een indelingsbestand voor het beschrijven van het tabelschema van de gegevens worden geëxporteerd/geïmporteerd. Details van het indelingsbestand worden beschreven in [maken van een indelingsbestand (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).

    Indeling van Bestandsgeneratie BCP bij het uitvoeren van de SQL Server-machine

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    Indeling van bestand genereren met het wanneer BCP op afstand uitvoeren in een SQL-Server

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. Gebruik een van de methoden die worden beschreven in de sectie [verplaatsen van gegevens van bestandsbron](#filesource_to_sqlonazurevm) gegevens in platte bestanden verplaatsen naar een SQL Server.

### <a name="sql-migration"></a>Wizard voor migratie van SQL-Database
[SQL Server-Database Migration Wizard](http://sqlazuremw.codeplex.com/) biedt een gebruiksvriendelijke manier om gegevens te verplaatsen tussen twee exemplaren van SQL server. Dit kan de gebruiker toewijzen van het gegevensschema tussen de bronnen en doeltabellen, kiest u kolomtypen en verschillende andere functionaliteiten. Bulksgewijs kopiëren (BCP) onder de dekt wordt gebruikt. Een schermopname van het welkomstscherm voor de migratie van de SQL-Database-wizard worden hieronder weergegeven.  

![Wizard SQL Server-migratie][2]

### <a name="sql-backup"></a>Back-up van database maken en terugzetten
SQL Server ondersteunt:

1. [Back-up van database en de functionaliteit herstellen](https://msdn.microsoft.com/library/ms187048.aspx) (zowel voor een lokaal bestand of Bacpac-exporteren naar blob) en [laag gegevenstoepassingen](https://msdn.microsoft.com/library/ee210546.aspx) (met behulp van bacpac).
2. Mogelijkheid om SQL Server-VM's rechtstreeks in Azure maken met een gekopieerde database of kopiëren naar een bestaande Azure SQL-database. Zie voor meer informatie [de Database-Wizard kopiëren gebruiken](https://msdn.microsoft.com/library/ms188664.aspx).

Een schermopname van de Database-back-up/terugzetten opties van SQL Server Management Studio wordt hieronder weergegeven.

![Hulpprogramma voor het SQL Server importeren][1]

## <a name="resources"></a>Resources
[Een Database migreren naar SQL Server op een virtuele machine in Azure](../../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[Overzicht van SQL Server op virtuele machines in Azure](../../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/move-sql-server-virtual-machine/database_migration_wizard.png

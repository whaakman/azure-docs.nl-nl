---
title: Azure Data Lake Storage Gen1 en Azure SQL-database met behulp van Sqoop kopiëren | Microsoft Docs
description: Sqoop gebruiken om te kopiëren van gegevens tussen Azure SQL Database en Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3f914b2a-83cc-4950-b3f7-69c921851683
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 958171a8d1091254588aef250406b968009eb968
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391739"
---
# <a name="copy-data-between-azure-data-lake-storage-gen1-and-azure-sql-database-using-sqoop"></a>Azure Data Lake Storage Gen1 en Sqoop met Azure SQL-database kopiëren
Leer hoe u Apache Sqoop gebruiken om te importeren en exporteren van gegevens tussen Azure SQL Database en Azure Data Lake Storage Gen1.

## <a name="what-is-sqoop"></a>Wat is Sqoop?
BIG data-toepassingen zijn een logische keuze voor de verwerking van ongestructureerde en semi-gestructureerde gegevens, zoals Logboeken en -bestanden. Er kan ook wel nodig voor het verwerken van gestructureerde gegevens die zijn opgeslagen in relationele databases.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) is een hulpprogramma dat is ontworpen voor het overbrengen van gegevens tussen relationele databases en een big data-opslagplaats, zoals Data Lake Storage Gen1. U kunt deze gebruiken om gegevens te importeren uit een relationeel databasebeheersysteem (RDBMS), zoals Azure SQL Database in Data Lake Storage Gen1. U kunt vervolgens transformeren en analyseren van de gegevens met behulp van big data-workloads en vervolgens de gegevens weer geëxporteerd naar een RDBMS. In deze zelfstudie kunt u een Azure SQL Database als uw relationele database importeren/exporteren uit.

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een account met Azure Data Lake Storage Gen1**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)
* **Azure HDInsight-cluster** met toegang tot een Gen1 van Data Lake Storage-account. Zie [een HDInsight-cluster maken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md). In dit artikel wordt ervan uitgegaan dat u een HDInsight Linux-cluster met Data Lake Storage Gen1 toegang hebt.
* **Azure SQL-database**. Zie voor instructies over het maken van een [maken van een Azure SQL database](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Leert u snel met video's?
[Bekijk deze video](https://mix.office.com/watch/1butcdjxmu114) over het kopiëren van gegevens tussen Azure Storage-Blobs en Data Lake Storage Gen1 DistCp gebruiken.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Van voorbeeldtabellen in de Azure SQL-Database maken
1. Te beginnen met twee tabellen maken in de Azure SQL-Database. Gebruik [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) of Visual Studio dat verbinding maken met de Azure SQL Database en voer de volgende query's.

    **Tabel1 maken**

        CREATE TABLE [dbo].[Table1](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_1] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO

    **Table2 maken**

        CREATE TABLE [dbo].[Table2](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_2] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO
2. In **Tabel1**, enkele voorbeeldgegevens toevoegen. Laat **Table2** leeg zijn. We het importeren van gegevens van **Tabel1** in Data Lake Storage Gen1. Er wordt vervolgens gegevens exporteren uit Data Lake Storage Gen1 in **Table2**. Voer het volgende codefragment.

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-storage-gen1"></a>Sqoop gebruiken vanuit een HDInsight-cluster met toegang op Data Lake Storage Gen1
Een HDInsight-cluster heeft al de Sqoop-pakketten beschikbaar. Als u het HDInsight-cluster voor het gebruik van Data Lake Storage Gen1 als extra opslag hebt geconfigureerd, kunt u Sqoop gebruiken (zonder eventuele configuratiewijzigingen) te importeren/exporteren van gegevens tussen een relationele database (in dit voorbeeld wordt een Azure SQL Database) en een Data Lake Gen1 opslagaccount.

1. Voor deze zelfstudie veronderstellen we dat u een Linux-cluster hebt gemaakt, zodat u SSH verbinding maken met het cluster moet gebruiken. Zie [verbinding maken met een Linux gebaseerde HDInsight-cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
2. Controleer of u toegang het Data Lake Storage Gen1 account uit het cluster tot kunt. Voer de volgende opdracht uit vanaf de SSH-prompt:

        hdfs dfs -ls adl://<data_lake_storage_gen1_account>.azuredatalakestore.net/

    Dit moet een lijst van bestanden/mappen in het Data Lake Storage Gen1-account opgeven.

### <a name="import-data-from-azure-sql-database-into-data-lake-storage-gen1"></a>Gegevens importeren uit een Azure SQL Database in Data Lake Storage Gen1
1. Ga naar de map waar Sqoop pakketten beschikbaar zijn. Meestal dit is op `/usr/hdp/<version>/sqoop/bin`.
2. Importeer de gegevens van **Tabel1** bij het Gen1 van Data Lake Storage-account. Gebruik de volgende syntaxis:

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-storage-gen1-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Houd er rekening mee dat **sql-database-server-name** tijdelijke aanduiding vertegenwoordigt de naam van de server waarop de Azure SQL-database wordt uitgevoerd. **naam van een SQL-database** tijdelijke aanduiding voor de naam van de werkelijke vertegenwoordigt.

    Bijvoorbeeld:


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. Controleer of dat de gegevens is overgebracht naar het Data Lake Storage Gen1-account. Voer de volgende opdracht uit:

        hdfs dfs -ls adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Hier ziet u de volgende uitvoer.


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Elke **onderdeel-m -*** bestand komt overeen met een rij in de brontabel **Tabel1**. U kunt de inhoud van het onderdeel - m - weergeven * bestanden om te controleren.


### <a name="export-data-from-data-lake-storage-gen1-into-azure-sql-database"></a>Gegevens exporteren uit Data Lake Storage Gen1 naar Azure SQL Database
1. De gegevens exporteren uit het Gen1 van Data Lake Storage-account naar de lege tabel **Table2**, in de Azure SQL-Database. Gebruik de volgende syntaxis.

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-storage-gen1-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Bijvoorbeeld:


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. Controleer of de gegevens daadwerkelijk is geüpload naar de SQL-databasetabel. Gebruik [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) of Visual Studio dat verbinding maken met de Azure SQL Database en voer de volgende query uit.

        SELECT * FROM TABLE2

    Dit moet de volgende uitvoer hebben.

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a>Prestatie-overwegingen bij het Sqoop gebruiken

Zie voor uw taak Sqoop om gegevens te kopiëren naar Data Lake Storage Gen1 afstemmen van prestaties, [Sqoop prestaties document](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).

## <a name="see-also"></a>Zie ook
* [Gegevens kopiëren van Azure Storage-Blobs naar Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md)
* [Gegevens beveiligen in Data Lake Storage Gen1](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)

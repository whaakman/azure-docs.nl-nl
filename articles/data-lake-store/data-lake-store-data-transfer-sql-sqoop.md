---
title: "Kopiëren van gegevens tussen Data Lake Store en Azure SQL database met Sqoop | Microsoft Docs"
description: "Sqoop gebruiken om gegevens tussen Azure SQL Database- en Data Lake Store te kopiëren"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3f914b2a-83cc-4950-b3f7-69c921851683
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 2611b50114a9fae003fe6e77300780a9af0698cc
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Kopiëren van gegevens tussen Data Lake Store en Azure SQL database met Sqoop
Informatie over het Apache Sqoop gebruiken om te importeren en exporteren van gegevens tussen Azure SQL Database- en Data Lake Store.

## <a name="what-is-sqoop"></a>Wat is Sqoop?
BIG data-toepassingen zijn een natuurlijke keuze voor het verwerken van ongestructureerde en semi-gestructureerde gegevens, zoals Logboeken en -bestanden. Er kan ook wel nodig om gestructureerde gegevens die zijn opgeslagen in de relationele databases te verwerken.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) is een hulpprogramma waarmee gegevens worden overgebracht tussen relationele databases en een big data-opslagplaats, zoals Data Lake Store. U kunt deze gebruiken om gegevens te importeren uit een relationele databasebeheersysteem (RDBMS) zoals Azure SQL Database in Data Lake Store. U kunt vervolgens transformeren en analyseren van de gegevens met behulp van big data-workloads en de gegevens vervolgens exporteren naar een RDBMS. In deze zelfstudie kunt u een Azure SQL Database als uw relationele database importeren/exporteren uit.

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Data Lake Store-account**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight-cluster** met toegang tot een Data Lake Store-account. Zie [een HDInsight-cluster maken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). In dit artikel wordt ervan uitgegaan dat u een HDInsight Linux-cluster met Data Lake Store toegang hebt.
* **Azure SQL-database**. Zie voor instructies over het maken van een [maken van een Azure SQL database](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Leert u snel met video's?
[Bekijk deze video](https://mix.office.com/watch/1butcdjxmu114) voor het kopiëren van gegevens tussen Azure Storage-Blobs en Data Lake Store met DistCp.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Maken van tabellen in de Azure SQL Database
1. Worden gestart met twee tabellen in de Azure SQL Database maken. Gebruik [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) of Visual Studio verbinding maken met de Azure SQL Database en voer de volgende query's.

    **Table1 maken**

        CREATE TABLE [dbo].[Table1](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO

    **Tabel2 maken**

        CREATE TABLE [dbo].[Table2](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO
2. In **Table1**, voorbeeldgegevens toevoegen. Laat **Table2** leeg. We het importeren van gegevens van **Table1** in Data Lake Store. Vervolgens we gegevens worden geëxporteerd van Data Lake Store in **Table2**. Voer het volgende fragment.

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Sqoop gebruiken van een HDInsight-cluster met toegang naar Data Lake Store
Een HDInsight-cluster heeft al het Sqoop pakketten beschikbaar. Als u het HDInsight-cluster voor het gebruik van Data Lake Store als extra opslag hebt geconfigureerd, kunt u Sqoop gebruiken (zonder configuratiewijzigingen) te importeren/exporteren van gegevens tussen een relationele database (in dit voorbeeld wordt een Azure SQL Database) en een Data Lake Store-account .

1. Voor deze zelfstudie we wordt ervan uitgegaan dat u een Linux-cluster hebt gemaakt, dus u SSH gebruiken moet om verbinding met het cluster. Zie [verbinding maken met een Linux gebaseerde HDInsight-cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
2. Controleer of u toegang hebt tot het Data Lake Store-account van het cluster. Voer de volgende opdracht achter de SSH-opdrachtprompt:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    Dit moet een lijst met bestanden/mappen in het Data Lake Store-account op te geven.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Gegevens importeren uit Azure SQL Database in Data Lake Store
1. Ga naar de map waar Sqoop pakketten beschikbaar zijn. Dit wordt normaal gesproken worden op `/usr/hdp/<version>/sqoop/bin`.
2. De gegevens importeren uit **Table1** bij de Data Lake Store-account. Gebruik de volgende syntaxis:

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Houd er rekening mee dat **sql-database-server-name** tijdelijke aanduiding vertegenwoordigt de naam van de server waarop de Azure SQL database wordt uitgevoerd. **naam van een SQL-database** tijdelijke aanduiding voor de werkelijke databasenaam vertegenwoordigt.

    Bijvoorbeeld:


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. Controleer of dat de gegevens is overgebracht naar het Data Lake Store-account. Voer de volgende opdracht uit:

        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Hier ziet u de volgende uitvoer.


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Elke **onderdeel-m -*** bestand komt overeen met een rij in de brontabel **Table1**. U kunt de inhoud van het onderdeel - m - weergeven * bestanden om te controleren.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Gegevens uit Data Lake Store exporteren naar Azure SQL Database
1. De gegevens van Data Lake Store-account exporteren naar de tabel leeg **Table2**, in de Azure SQL-Database. Gebruik de volgende syntaxis.

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Bijvoorbeeld:


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. Controleer of dat de gegevens is geüpload naar de SQL-databasetabel. Gebruik [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) of Visual Studio verbinding maken met de Azure SQL Database en voer vervolgens de volgende query.

        SELECT * FROM TABLE2

    Dit moet de volgende uitvoer hebben.

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a>Prestatie-overwegingen bij het gebruik van Sqoop

Zie voor prestaties afstemmen van de taak Sqoop om gegevens te kopiëren naar Data Lake Store, [Sqoop prestaties document](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).

## <a name="see-also"></a>Zie ook
* [Gegevens kopiëren van Azure Storage-Blobs naar Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

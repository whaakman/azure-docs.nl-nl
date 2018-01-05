---
title: Apache Sqoop met Hadoop - Azure HDInsight | Microsoft Docs
description: Informatie over het Apache Sqoop gebruiken om te importeren en exporteren tussen Hadoop in HDInsight en een Azure SQL Database.
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
keywords: hadoop sqoop, sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.author: larryfr
ms.openlocfilehash: a0a63c414bc68f5125b65e288d78fb546c376c04
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a>Apache Sqoop gebruiken om te importeren en exporteren van gegevens tussen Hadoop in HDInsight en SQL-Database

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het Apache Sqoop gebruiken om te importeren en exporteren tussen een Hadoop-cluster in Azure HDInsight en Azure SQL Database of Microsoft SQL Server-database. De stappen in dit document gebruikt de `sqoop` opdracht rechtstreeks vanuit de headnode van het Hadoop-cluster. U kunt SSH verbinding maken met het hoofdknooppunt en voer de opdrachten in dit document.

> [!IMPORTANT]
> De stappen in dit document wordt alleen werken met HDInsight-clusters die gebruikmaken van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

> [!WARNING]
> De stappen in dit document wordt ervan uitgegaan dat u een Azure SQL Database met de naam al hebt gemaakt `sqooptest`.
>
> Dit document bevat een T-SQL-instructies die worden gebruikt voor het maken en een tabel in SQL-Database. Er zijn veel clients dat u deze instructies met SQL Database kunt gebruiken. U wordt aangeraden de volgende clients:
>
> * [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md)
> * [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md)
> * De [sqlcmd](https://docs.microsoft.com/sql/tools/sqlcmd-utility) hulpprogramma

## <a name="create-the-table-in-sql-database"></a>De tabel in SQL-Database maken

> [!IMPORTANT]
> Als u van het HDInsight-cluster gebruikmaakt en SQL-Database gemaakt [cluster en SQL-database maken](hdinsight-use-sqoop.md), de stappen in deze sectie overslaan. De database en tabel zijn gemaakt als onderdeel van de stappen in de [cluster en SQL-database maken](hdinsight-use-sqoop.md) document.

Een SQL-client gebruiken om verbinding maken met de `sqooptest` database in SQL-Database. Gebruik vervolgens de volgende T-SQL te maken van een tabel met de naam `mobiledata`:

```sql
CREATE TABLE [dbo].[mobiledata](
[clientid] [nvarchar](50),
[querytime] [nvarchar](50),
[market] [nvarchar](50),
[deviceplatform] [nvarchar](50),
[devicemake] [nvarchar](50),
[devicemodel] [nvarchar](50),
[state] [nvarchar](50),
[country] [nvarchar](50),
[querydwelltime] [float],
[sessionid] [bigint],
[sessionpagevieworder] [bigint])
GO
CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
GO
```

## <a name="sqoop-export"></a>Sqoop exporteren

1. SSH gebruiken voor verbinding met het HDInsight-cluster. Bijvoorbeeld de volgende opdracht maakt verbinding met de primaire headnode van een cluster met de naam `mycluster`:

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Om te controleren of de SQL-Database door Sqoop kan zien, moet u de volgende opdracht gebruiken:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    Voer desgevraagd het wachtwoord voor aanmelding bij de SQL-Database.

    Met deze opdracht retourneert een lijst met databases, met inbegrip van de **sqooptest** database die eerder is gebruikt.

3. Exporteren van gegevens van de component **hivesampletable** tabel naar de **mobiledata** tabel in SQL-Database, gebruikt u de volgende opdracht:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P -table 'mobiledata' --hcatalog-table hivesampletable
    ```

4. Om te bevestigen dat de gegevens zijn geëxporteerd, gebruikt u de volgende query van de SQL-client om de geëxporteerde gegevens weer te geven:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata;
    ```

    Met deze opdracht worden 50 rijen die zijn geïmporteerd in de tabel.

## <a name="sqoop-import"></a>Sqoop importeren

1. Gebruik de volgende opdracht voor het importeren van gegevens uit de **mobiledata** tabel in SQL-Database naar de **wasb: / / / zelfstudies/usesqoop/importeddata** directory op HDInsight:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    De velden in de gegevens worden gescheiden door een tab-teken en de regels worden beëindigd door een nieuwe-regelteken.

    > [!IMPORTANT]
    > De `wasb:///` pad werkt met clusters die gebruikmaken van Azure Storage als de standaard clusteropslag. Gebruik voor clusters die gebruikmaken van Azure Data Lake Store, `adl:///` in plaats daarvan.

2. Zodra het importeren is voltooid, gebruikt u de volgende opdracht om een lijst van de gegevens in de nieuwe map:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>Met behulp van SQL Server

U kunt ook Sqoop gebruiken om te importeren en exporteren van gegevens uit SQL Server. De verschillen tussen het gebruik van SQL-Database en SQL Server zijn:

* HDInsight en SQL Server moeten allebei op hetzelfde Azure Virtual Network.

    Zie voor een voorbeeld de [HDInsight verbinding maken met uw lokale netwerk](./../connect-on-premises-network.md) document.

    Zie voor meer informatie over het gebruik van HDInsight met een Azure-netwerk de [HDInsight uitbreiden met Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md) document. Zie voor meer informatie over Azure Virtual Network de [Virtual Network-overzicht](../../virtual-network/virtual-networks-overview.md) document.

* SQL Server moet worden geconfigureerd voor het toestaan van SQL-verificatie. Zie voor meer informatie de [een verificatiemodus kiezen](https://msdn.microsoft.com/ms144284.aspx) document.

* U moet SQL Server configureren voor externe verbindingen kan accepteren. Zie voor meer informatie de [oplossen verbinding te maken met de SQL Server database engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) document.

* Gebruik de volgende Transact-SQL-instructies om te maken de **mobiledata** tabel:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    ```

* Wanneer u verbinding maakt met de SQL Server uit HDInsight, moet u wellicht de IP-adres van de SQL Server gebruiken. Bijvoorbeeld:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> -P -table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Beperkingen

* Bulksgewijs export - met Linux gebaseerde HDInsight, de Sqoop-connector gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt geen ondersteuning voor bulksgewijs invoegen.

* Batchverwerking - met HDInsight op basis van Linux, wanneer u de `-batch` bij het uitvoeren van invoeg-switch, Sqoop meerdere invoegen in plaats van de bewerkingen insert batchverwerking maakt.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe Sqoop gebruiken. Voor meer informatie zie:

* [Oozie gebruiken met HDInsight](../hdinsight-use-oozie.md): Sqoop gebruiken in een werkstroom Oozie in te grijpen.
* [Vertraging vluchtgegevens met HDInsight analyseren](../hdinsight-analyze-flight-delay-data.md): Hive gebruiken voor het analyseren van vlucht gegevens uit te stellen en vervolgens Sqoop gebruiken om gegevens te exporteren naar een Azure SQL database.
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md): vinden van andere methoden voor het uploaden van gegevens naar HDInsight/Azure Blob-opslag.

[hdinsight-versions]:  ../hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

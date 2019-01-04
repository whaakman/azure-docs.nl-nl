---
title: Apache Sqoop met Apache Hadoop - Azure HDInsight
description: Leer hoe u Apache Sqoop gebruiken om te importeren en exporteren tussen een Azure SQL Database en de Apache Hadoop op HDInsight.
keywords: hadoop sqoop, sqoop
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 03/26/2018
ms.openlocfilehash: d7df1c65b8588b97a6beb0a4c2428b3c6430c3b2
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53635694"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-sql-database"></a>Apache Sqoop gebruiken om te importeren en exporteren van gegevens tussen Apache Hadoop op HDInsight en SQL-Database

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Leer hoe u Apache Sqoop gebruiken om te importeren en exporteren tussen een Apache Hadoop-cluster in Azure HDInsight en Azure SQL Database- of Microsoft SQL Server-database. De stappen in dit document gebruiken de `sqoop` opdracht rechtstreeks vanuit het hoofdknooppunt van het Hadoop-cluster. U kunt SSH verbinding met het hoofdknooppunt en voer de opdrachten in dit document.

> [!IMPORTANT]  
> De stappen in dit document werken alleen met HDInsight-clusters die gebruikmaken van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

> [!WARNING]  
> De stappen in dit document wordt ervan uitgegaan dat u een Azure SQL Database met de naam al hebt gemaakt `sqooptest`.
>
> Dit document bevat een T-SQL-instructies die worden gebruikt voor het maken en query uitvoeren op een tabel in SQL-Database. Er zijn veel clients dat u deze instructies met SQL-Database gebruiken kunt. U wordt aangeraden de volgende clients:
>
> * [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md)
> * [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md)
> * De [sqlcmd](https://docs.microsoft.com/sql/tools/sqlcmd-utility) hulpprogramma

## <a name="create-the-table-in-sql-database"></a>De tabel in SQL Database maken

> [!IMPORTANT]  
> Als u van het HDInsight-cluster gebruikmaakt en SQL-Database gemaakt [cluster en SQL-database maken](hdinsight-use-sqoop.md), kunt u de stappen in deze sectie overslaan. De database en tabel zijn gemaakt als onderdeel van de stappen in de [cluster en SQL-database maken](hdinsight-use-sqoop.md) document.

Een SQL-client gebruiken voor verbinding met de `sqooptest` database in uw SQL-Database. Vervolgens de volgende T-SQL gebruiken om een tabel met de naam `mobiledata`:

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

## <a name="sqoop-export"></a>Exporteren met Sqoop

1. Gebruik SSH verbinding maken met de HDInsight-cluster. Bijvoorbeeld de volgende opdracht maakt verbinding met het primaire hoofdknooppunt van een cluster met de naam `mycluster`:

    ```bash
    ssh mycluster-ssh.azurehdinsight.net
    ```

    Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Om te controleren of Sqoop uw SQL-Database kunt zien, gebruikt u de volgende opdracht uit:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    Voer desgevraagd het wachtwoord voor aanmelding bij de SQL-Database.

    Met deze opdracht retourneert een lijst van databases, met inbegrip van de **sqooptest** database die u eerder hebt gebruikt.

3. Voor het exporteren van gegevens uit de component **hivesampletable** tabel naar de **mobiledata** tabel in SQL-Database, gebruikt u de volgende opdracht uit:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P -table 'mobiledata' --hcatalog-table hivesampletable
    ```

4. Om te controleren dat de gegevens zijn geëxporteerd, gebruikt u de volgende query uit uw SQL-client om de geëxporteerde gegevens weer te geven:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata;
    ```

    Met deze opdracht worden 50 rijen die zijn geïmporteerd in de tabel.

## <a name="sqoop-import"></a>Sqoop-Importeer

1. Gebruik de volgende opdracht voor het importeren van gegevens uit de **mobiledata** tabel in SQL-Database naar de **wasb: / / / zelfstudies/usesqoop/importeddata** map op HDInsight:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    De velden in de gegevens worden gescheiden door een teken tabblad en de regels worden beëindigd door een nieuwe-regelteken.

    > [!IMPORTANT]  
    > De `wasb:///` pad werkt met clusters waarvoor wordt gebruikgemaakt van Azure Storage als de standaardopslag voor het cluster. Gebruik voor clusters die gebruikmaken van Azure Data Lake Storage `adl:///` in plaats daarvan.

2. Zodra het importeren is voltooid, gebruikt u de volgende opdracht uit om de gegevens in de nieuwe map:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>Met behulp van SQL Server

U kunt ook Sqoop gebruiken om te importeren en exporteren van gegevens uit SQL Server. Zijn de verschillen tussen het gebruik van SQL Database en SQL Server:

* Zowel HDInsight en SQL Server moeten zich op dezelfde Azure Virtual Network.

    Zie voor een voorbeeld: de [HDInsight verbinden met uw on-premises netwerk](./../connect-on-premises-network.md) document.

    Zie voor meer informatie over het gebruik van HDInsight met een Azure-netwerk de [HDInsight uitbreiden met Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md) document. Zie voor meer informatie over Azure Virtual Network, de [overzicht van Virtual Network](../../virtual-network/virtual-networks-overview.md) document.

* SQL Server moet worden geconfigureerd dat SQL-verificatie. Zie voor meer informatie de [een verificatiemodus kiezen](https://msdn.microsoft.com/ms144284.aspx) document.

* U moet SQL Server configureren voor externe verbindingen kan accepteren. Zie voor meer informatie de [problemen oplossen met verbinding te maken met de SQL Server database engine](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) document.

* Gebruik de volgende Transact-SQL-instructies gebruikt om te maken de **mobiledata** tabel:

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

* Wanneer u verbinding maakt met de SQL Server in HDInsight, mogelijk hebt u het IP-adres van de SQL Server. Bijvoorbeeld:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> -P -table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Beperkingen

* Bulksgewijs exporteren - met Linux gebaseerde HDInsight, de Sqoop-connector die wordt gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt geen ondersteuning voor bulksgewijs invoegen.

* Batchverwerking - met HDInsight op basis van Linux bij het gebruik van de `-batch` overschakelen tijdens het uitvoeren van ingevoegd, Sqoop kunt u meerdere invoegingen in plaats van de batchverwerking van de insert-bewerkingen.

## <a name="next-steps"></a>Volgende stappen

U hebt nu geleerd hoe u Sqoop gebruiken. Voor meer informatie zie:

* [Apache Oozie gebruiken met HDInsight](../hdinsight-use-oozie.md): Sqoop actie gebruiken in een Oozie-workflow.
* [Analyseren van gegevens van vertragingen van vluchten met behulp van HDInsight](../hdinsight-analyze-flight-delay-data.md): Apache Hive gebruiken voor het analyseren van gegevens van vertragingen van vluchten en Sqoop kunt gebruiken om gegevens te exporteren naar een Azure SQL-database.
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md): Andere methoden voor het uploaden van gegevens naar HDInsight/Azure-blobopslag te vinden.

[hdinsight-versions]:  ../hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: https://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

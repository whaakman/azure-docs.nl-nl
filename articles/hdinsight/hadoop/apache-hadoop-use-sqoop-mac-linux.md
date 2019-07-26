---
title: Apache Sqoop met Apache Hadoop-Azure HDInsight
description: Meer informatie over het gebruik van Apache Sqoop voor het importeren en exporteren tussen Apache Hadoop op HDInsight en een Azure SQL Database.
keywords: Hadoop-sqoop, sqoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: c839aeae77d7e75fb30d82c410c331d21f5868ae
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68406038"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-sql-database"></a>Apache Sqoop gebruiken voor het importeren en exporteren van gegevens tussen Apache Hadoop op HDInsight en SQL Database

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Meer informatie over het gebruik van Apache Sqoop voor het importeren en exporteren van een Apache Hadoop cluster in azure HDInsight en Azure SQL Database of Microsoft SQL Server Data Base. De stappen in dit document gebruiken de `sqoop` opdracht rechtstreeks vanuit het hoofd knooppunt van het Hadoop-cluster. U gebruikt SSH om verbinding te maken met het hoofd knooppunt en de opdrachten uit te voeren in dit document. Dit artikel is een voortzetting van het [gebruik van Apache Sqoop met Hadoop in HDInsight](./hdinsight-use-sqoop.md).

## <a name="prerequisites"></a>Vereisten

* Volt ooien van de [instelling test omgeving](./hdinsight-use-sqoop.md#create-cluster-and-sql-database) [voor het gebruik van Apache Sqoop met Hadoop in HDInsight](./hdinsight-use-sqoop.md).

* Een client voor het opvragen van de Azure SQL database. Overweeg het gebruik van [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md) of [Visual Studio code](../../sql-database/sql-database-connect-query-vscode.md).

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="sqoop-export"></a>Sqoop exporteren

Van Hive naar SQL Server.

1. Gebruik SSH om verbinding te maken met het HDInsight-cluster. Vervang `CLUSTERNAME` door de naam van uw cluster en voer de volgende opdracht in:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Vervang `MYSQLSERVER` door de naam van uw SQL Server. Als u wilt controleren of Sqoop uw SQL Database kan zien, voert u de onderstaande opdracht in voor de open SSH-verbinding. Geef het wacht woord voor de SQL Server aanmelding op wanneer u hierom wordt gevraagd. Met deze opdracht wordt een lijst met data bases geretourneerd.

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433 --username sqluser -P
    ```

3. Vervang `MYSQLSERVER` door de naam van uw SQL Server en `MYDATABASE` met de naam van uw SQL database. Als u gegevens uit de Hive `hivesampletable` -tabel naar `mobiledata` de tabel in SQL database wilt exporteren, voert u de onderstaande opdracht in voor de open SSH-verbinding. Voer het wacht woord voor de SQL Server aanmelding in wanneer u hierom wordt gevraagd

    ```bash
    sqoop export --connect 'jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433;database=MYDATABASE' --username sqluser -P -table 'mobiledata' --hcatalog-table hivesampletable
    ```

4. Als u wilt controleren of de gegevens zijn geëxporteerd, gebruikt u de volgende query's van uw SQL-client om de geëxporteerde gegevens weer te geven:

    ```sql
    SELECT COUNT(*) FROM [dbo].[mobiledata] WITH (NOLOCK);
    SELECT TOP(25) * FROM [dbo].[mobiledata] WITH (NOLOCK);
    ```

## <a name="sqoop-import"></a>Sqoop importeren

Van SQL Server naar Azure Storage.

1. Vervang `MYSQLSERVER` door de naam van uw SQL Server en `MYDATABASE` met de naam van uw SQL database. Voer de onderstaande opdracht in uw open SSH-verbinding in om gegevens uit `mobiledata` de tabel in SQL database te importeren `wasb:///tutorials/usesqoop/importeddata` in de map op HDInsight. Geef het wacht woord voor de SQL Server aanmelding op wanneer u hierom wordt gevraagd. De velden in de gegevens worden gescheiden door een tab en de lijnen worden afgesloten met een nieuw regel teken.

    ```bash
    sqoop import --connect 'jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433;database=MYDATABASE' --username sqluser -P --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

2. Zodra het importeren is voltooid, voert u de volgende opdracht in uw open SSH-verbinding in om de gegevens in de nieuwe map weer te geven:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="limitations"></a>Beperkingen

* Bulk export-met HDInsight op basis van Linux, de Sqoop-connector die wordt gebruikt om gegevens te exporteren naar Microsoft SQL Server of Azure SQL Database biedt geen ondersteuning voor bulksgewijs invoegen.

* Batch verwerking: met HDInsight op basis van Linux kunt u met `-batch` behulp van de switch tijdens het invoegen meerdere invoegen gebruiken in plaats van een batch verwerking van de Insert-bewerkingen uit te voeren.

## <a name="important-considerations"></a>Belang rijke overwegingen

* Zowel HDInsight als SQL Server moeten zich op dezelfde Azure Virtual Network.

    Zie voor een voor beeld het [Connect HDInsight to your on-premises netwerk](./../connect-on-premises-network.md) document.

    Zie het document [Hdinsight uitbreiden met azure Virtual Network](../hdinsight-plan-virtual-network-deployment.md) voor meer informatie over het gebruik van hdinsight met een Azure Virtual Network. Zie het document [Virtual Network Overview](../../virtual-network/virtual-networks-overview.md) voor meer informatie over Azure Virtual Network.

* SQL Server moet worden geconfigureerd om SQL-verificatie toe te staan. Zie het document [een verificatie modus kiezen](https://msdn.microsoft.com/ms144284.aspx) voor meer informatie.

* Mogelijk moet u SQL Server configureren om externe verbindingen te accepteren. Zie [problemen met het maken van verbinding met het document van de SQL Server Data Base-Engine oplossen](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

U hebt nu geleerd hoe u Sqoop kunt gebruiken. Voor meer informatie zie:

* [Apache Oozie gebruiken met HDInsight](../hdinsight-use-oozie-linux-mac.md): Gebruik de actie Sqoop in een Oozie-werk stroom.
* [Analyseer gegevens van de vlucht vertraging met HDInsight](../interactive-query/interactive-query-tutorial-analyze-flight-data.md): Gebruik interactieve Query's om vlucht vertragings gegevens te analyseren en gebruik vervolgens Sqoop om gegevens te exporteren naar een Azure-SQL database.
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md): Andere methoden vinden voor het uploaden van gegevens naar HDInsight/Azure Blob-opslag.

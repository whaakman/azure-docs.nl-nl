---
title: Apache Sqoop met Apache Hadoop - Azure HDInsight
description: Leer hoe u Apache Sqoop gebruiken om te importeren en exporteren tussen een Azure SQL Database en de Apache Hadoop op HDInsight.
keywords: hadoop sqoop,sqoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 02b4eb6c367510e8994aa7723fe3fdd3e43af0e6
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462181"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-sql-database"></a>Apache Sqoop gebruiken om te importeren en exporteren van gegevens tussen Apache Hadoop op HDInsight en SQL-Database

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Leer hoe u Apache Sqoop gebruiken om te importeren en exporteren tussen een Apache Hadoop-cluster in Azure HDInsight en Azure SQL Database- of Microsoft SQL Server-database. De stappen in dit document gebruiken de `sqoop` opdracht rechtstreeks vanuit het hoofdknooppunt van het Hadoop-cluster. U kunt SSH verbinding met het hoofdknooppunt en voer de opdrachten in dit document. In dit artikel is een vervolg van [Apache Sqoop gebruiken met Hadoop in HDInsight](./hdinsight-use-sqoop.md).

## <a name="prerequisites"></a>Vereisten

* Voltooiing van [testomgeving instellen](./hdinsight-use-sqoop.md#create-cluster-and-sql-database) van [Apache Sqoop gebruiken met Hadoop in HDInsight](./hdinsight-use-sqoop.md).

* Een client om te zoeken in de Azure SQL-database. Overweeg het gebruik van [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md) of [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md).

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="sqoop-export"></a>Exporteren met Sqoop

Van Hive met SQL Server.

1. Gebruik SSH verbinding maken met de HDInsight-cluster. Vervang `CLUSTERNAME` met de naam van uw cluster, voert u de opdracht:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Vervang `MYSQLSERVER` met de naam van uw SQL-Server. Als u wilt controleren of Sqoop uw SQL-Database kunt zien, voert u de onderstaande opdracht in uw SSH-verbinding openen. Voer het wachtwoord voor de SQL Server-aanmelding wanneer hierom wordt gevraagd. Met deze opdracht retourneert een lijst met databases.

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433 --username sqluser -P
    ```

3. Vervang `MYSQLSERVER` met de naam van uw SQL-Server en `MYDATABASE` met de naam van uw SQL-database. Voor het exporteren van gegevens uit de component `hivesampletable` tabel naar de `mobiledata` tabel in SQL-Database, voert u de onderstaande opdracht in uw SSH-verbinding openen. Voer het wachtwoord voor de SQL Server-aanmelding wanneer hierom wordt gevraagd

    ```bash
    sqoop export --connect 'jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433;database=MYDATABASE' --username sqluser -P -table 'mobiledata' --hcatalog-table hivesampletable
    ```

4. Om te controleren dat de gegevens zijn geëxporteerd, gebruikt u de volgende query's van uw SQL-client om de geëxporteerde gegevens weer te geven:

    ```sql
    SELECT COUNT(*) FROM [dbo].[mobiledata] WITH (NOLOCK);
    SELECT TOP(25) * FROM [dbo].[mobiledata] WITH (NOLOCK);
    ```

## <a name="sqoop-import"></a>Sqoop-Importeer

Uit SQL Server naar Azure storage.

1. Vervang `MYSQLSERVER` met de naam van uw SQL-Server en `MYDATABASE` met de naam van uw SQL-database. Voer de onderstaande opdracht in uw open SSH-verbinding met het importeren van gegevens van de `mobiledata` tabel in SQL-Database naar de `wasb:///tutorials/usesqoop/importeddata` map op HDInsight. Voer het wachtwoord voor de SQL Server-aanmelding wanneer hierom wordt gevraagd. De velden in de gegevens worden gescheiden door een teken tabblad en de regels worden beëindigd door een nieuwe-regelteken.

    ```bash
    sqoop import --connect 'jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433;database=MYDATABASE' --username sqluser -P --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

2. Zodra het importeren is voltooid, voert u de volgende opdracht in uw SSH-verbinding openen om de gegevens in de nieuwe map:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="limitations"></a>Beperkingen

* Bulksgewijs exporteren - met Linux gebaseerde HDInsight, de Sqoop-connector die wordt gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt geen ondersteuning voor bulksgewijs invoegen.

* Batchverwerking - met HDInsight op basis van Linux bij het gebruik van de `-batch` overschakelen tijdens het uitvoeren van ingevoegd, Sqoop kunt u meerdere invoegingen in plaats van de batchverwerking van de insert-bewerkingen.

## <a name="important-considerations"></a>Belangrijke overwegingen

* Zowel HDInsight en SQL Server moeten zich op dezelfde Azure Virtual Network.

    Zie voor een voorbeeld: de [HDInsight verbinden met uw on-premises netwerk](./../connect-on-premises-network.md) document.

    Zie voor meer informatie over het gebruik van HDInsight met een Azure-netwerk de [HDInsight uitbreiden met Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md) document. Zie voor meer informatie over Azure Virtual Network, de [overzicht van Virtual Network](../../virtual-network/virtual-networks-overview.md) document.

* SQL Server moet worden geconfigureerd dat SQL-verificatie. Zie voor meer informatie de [een verificatiemodus kiezen](https://msdn.microsoft.com/ms144284.aspx) document.

* U moet SQL Server configureren voor externe verbindingen kan accepteren. Zie voor meer informatie de [problemen oplossen met verbinding te maken met de SQL Server database engine](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) document.

## <a name="next-steps"></a>Volgende stappen

U hebt nu geleerd hoe u Sqoop gebruiken. Voor meer informatie zie:

* [Apache Oozie gebruiken met HDInsight](../hdinsight-use-oozie-linux-mac.md): Sqoop actie gebruiken in een Oozie-workflow.
* [Analyseren van gegevens van vertragingen van vluchten met behulp van HDInsight](../interactive-query/interactive-query-tutorial-analyze-flight-data.md): Interactive Query gebruiken voor het analyseren van gegevens van vertragingen van vluchten en Sqoop kunt gebruiken om gegevens te exporteren naar een Azure SQL-database.
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md): Andere methoden voor het uploaden van gegevens naar HDInsight/Azure-blobopslag te vinden.

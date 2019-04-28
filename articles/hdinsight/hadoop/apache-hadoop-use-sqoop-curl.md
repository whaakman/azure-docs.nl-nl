---
title: Curl gebruiken voor het exporteren van gegevens met Apache Sqoop in Azure HDInsight
description: Leer hoe u op afstand Apache Sqoop taken indienen bij HDInsight met Curl.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 345f492c5b2c754cbbcfa150561ee06b5a4154a5
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126950"
---
# <a name="run-apache-sqoop-jobs-in-hdinsight-with-curl"></a>Apache Sqoop-opdrachten uitvoeren in HDInsight met Curl
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van Curl Apache Sqoop uitvoeren op een Apache Hadoop-cluster in HDInsight. In dit artikel ziet u hoe u gegevens uit Azure storage exporteren en importeren in een SQL Server-database met Curl. In dit artikel is een vervolg van [Apache Sqoop gebruiken met Hadoop in HDInsight](./hdinsight-use-sqoop.md).

CURL is gebruikt om te demonstreren hoe u met HDInsight kunt werken met behulp van onbewerkte HTTP-aanvragen uitvoeren, controleren en de resultaten van Sqoop taken ophalen. Dit werkt met behulp van de WebHCat REST-API (voorheen bekend als Templeton) geleverd door uw HDInsight-cluster.

## <a name="prerequisites"></a>Vereisten

* Voltooiing van [testomgeving instellen](./hdinsight-use-sqoop.md#create-cluster-and-sql-database) van [Apache Sqoop gebruiken met Hadoop in HDInsight](./hdinsight-use-sqoop.md).

* Een client om te zoeken in de Azure SQL-database. Overweeg het gebruik van [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md) of [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md).

* [cURL](https://curl.haxx.se/). CURL is een hulpprogramma voor het overbrengen van gegevens van of naar een HDInsight-cluster.

* [jq](https://stedolan.github.io/jq/). Het hulpprogramma jq wordt gebruikt om de JSON-gegevens geretourneerd door de REST-aanvragen te verwerken.

## <a name="submit-apache-sqoop-jobs-by-using-curl"></a>Apache Sqoop taken verzenden met behulp van Curl

Curl gebruiken voor het exporteren van gegevens met behulp van Apache Sqoop taken uit Azure storage naar SQL Server.

> [!NOTE]  
> Wanneer u Curl of een andere REST-communicatie gebruikt met WebHCat, moet u de aanvragen verifiëren door de gebruikersnaam en het wachtwoord voor de beheerder van het HDInsight-cluster op te geven. U moet ook de clusternaam gebruiken als onderdeel van de URI (Uniform Resource Identifier) die wordt gebruikt om de aanvragen naar de server te verzenden.

Voor de opdrachten in deze sectie vervangt u `USERNAME` met de gebruiker om te verifiëren met het cluster en vervang `PASSWORD` met het wachtwoord voor het gebruikersaccount. Vervang `CLUSTERNAME` door de naam van uw cluster.
 
De REST API is beveiligd via [basisverificatie](https://en.wikipedia.org/wiki/Basic_access_authentication). U moet aanvragen altijd uitvoeren via een beveiligde HTTP-verbinding (HTTPS). Zo zorgt u ervoor dat uw referenties veilig worden verzonden naar de server.

1. Gebruik een opdrachtregel met de volgende opdracht om te controleren of u verbinding met uw HDInsight-cluster kunt maken:

    ```cmd
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Het antwoord dat u ontvangt, is vergelijkbaar met het volgende antwoord:

    ```json
    {"status":"ok","version":"v1"}
    ```

2. Vervang `SQLDATABASESERVERNAME`, `USERNAME@SQLDATABASESERVERNAME`, `PASSWORD`, `SQLDATABASENAME` met de juiste waarden uit de vereisten. Gebruik de volgende om een taak sqoop te verzenden:

    ```cmd
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /example/data/sample.log --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/data/sqoop/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop
    ```

    In deze opdracht worden de volgende parameters gebruikt:

   * **-d** - sinds `-G` wordt niet gebruikt, de aanvraag standaard ingesteld op de POST-methode. `-d` Hiermee geeft u de waarden die worden verzonden met de aanvraag.

       * **User.name** -de gebruiker die de opdracht wordt uitgevoerd.

       * **opdracht** -Sqoop van de opdracht moet worden uitgevoerd.

       * **statusdir** -de map die de status voor deze taak om te worden geschreven.

     Met deze opdracht wordt een taak-ID die kan worden gebruikt om te controleren of de status van de taak retourneren.

       ```json
       {"id":"job_1415651640909_0026"}
       ```

3. Gebruik de volgende opdracht om te controleren of de status van de taak. Vervang `JOBID` met de waarde die wordt geretourneerd in de vorige stap. Bijvoorbeeld, als de geretourneerde waarde is `{"id":"job_1415651640909_0026"}`, klikt u vervolgens `JOBID` zou `job_1415651640909_0026`.

    ```cmd
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Als de taak is voltooid, worden de status **geslaagd**.
   
   > [!NOTE]  
   > Deze aanvraag Curl retourneert een JavaScript Object Notation (JSON)-document met informatie over de taak. jq wordt gebruikt om op te halen, alleen de statuswaarde.

4. Zodra de status van de taak is gewijzigd in **geslaagd**, kunt u de resultaten van de taak ophalen uit Azure Blob-opslag. De `statusdir` parameter die is doorgegeven met de query bevat de locatie van het uitvoerbestand; in dit geval `wasb:///example/data/sqoop/curl`. Dit adres slaat de uitvoer van de taak in de `example/data/sqoop/curl` map op de standaard-storage-container die wordt gebruikt door uw HDInsight-cluster.

    U kunt de Azure-portal gebruiken voor toegang tot de stdout- en stderr-blobs.

5. Om te controleren dat de gegevens zijn geëxporteerd, gebruikt u de volgende query's van uw SQL-client om de geëxporteerde gegevens weer te geven:

    ```sql
    SELECT COUNT(*) FROM [dbo].[log4jlogs] WITH (NOLOCK);
    SELECT TOP(25) * FROM [dbo].[log4jlogs] WITH (NOLOCK);
    ```

## <a name="limitations"></a>Beperkingen
* Bulksgewijs exporteren - met Linux gebaseerde HDInsight, de Sqoop-connector die wordt gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt momenteel geen ondersteuning voor bulksgewijs invoegen.
* Batchverwerking - met HDInsight op basis van Linux bij het gebruik van de `-batch` overschakelen tijdens het uitvoeren van ingevoegd, Sqoop meerdere invoegingen in plaats van de bewerkingen insert batchverwerking wordt uitgevoerd.

## <a name="summary"></a>Samenvatting
Zoals in dit document wordt gedemonstreerd, kunt u een onbewerkte HTTP-aanvraag uitvoeren, controleren en de resultaten van Sqoop taken weergeven op uw HDInsight-cluster.

Zie voor meer informatie over de REST-interface gebruikt in dit artikel de <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Apache Sqoop REST-API-handleiding</a>.

## <a name="next-steps"></a>Volgende stappen
[Apache Sqoop gebruiken met Apache Hadoop op HDInsight](hdinsight-use-sqoop.md)

Voor andere HDInsight curl artikelen met betrekking tot:
 
* [Apache Hadoop-clusters met behulp van de Azure REST API maken](../hdinsight-hadoop-create-linux-clusters-curl-rest.md)
* [Apache Hive-query's uitvoeren met Apache Hadoop in HDInsight met behulp van REST](apache-hadoop-use-hive-curl.md)
* [MapReduce-taken uitvoeren met Apache Hadoop op HDInsight met behulp van REST](apache-hadoop-use-mapreduce-curl.md)
* [Apache Pig-taken uitvoeren met Apache Hadoop op HDInsight met cURL](apache-hadoop-use-pig-curl.md)

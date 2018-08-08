---
title: Hadoop Sqoop gebruiken met Curl in HDInsight - Azure
description: Leer hoe u op afstand Sqoop taken indienen bij HDInsight met Curl.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: 2c6376772aedbe097d737d97c673447adb12bed3
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39598983"
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Sqoop taken uitvoeren met Hadoop in HDInsight met Curl
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van Curl Sqoop uitvoeren op een Hadoop-cluster in HDInsight.

CURL is gebruikt om te demonstreren hoe u met HDInsight kunt werken met behulp van onbewerkte HTTP-aanvragen uitvoeren, controleren en de resultaten van Sqoop taken ophalen. Dit werkt met behulp van de WebHCat REST-API (voorheen bekend als Templeton) geleverd door uw HDInsight-cluster.

## <a name="prerequisites"></a>Vereisten
Als u wilt de stappen in dit artikel hebt voltooid, moet u het volgende:

* Volledige [Sqoop gebruiken met Hadoop in HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database) het configureren van een omgeving met een HDInsight-cluster en een Azure SQL-database.
* [cURL](http://curl.haxx.se/). CURL is een hulpprogramma voor het overbrengen van gegevens van of naar een HDInsight-cluster.
* [jq](http://stedolan.github.io/jq/). Het hulpprogramma jq wordt gebruikt om de JSON-gegevens geretourneerd door de REST-aanvragen te verwerken.

## <a name="submit-sqoop-jobs-by-using-curl"></a>Sqoop taken verzenden met behulp van Curl
> [!NOTE]
> Wanneer u Curl of een andere REST-communicatie gebruikt met WebHCat, moet u de aanvragen verifiëren door de gebruikersnaam en het wachtwoord voor de beheerder van het HDInsight-cluster op te geven. U moet ook de clusternaam gebruiken als onderdeel van de URI (Uniform Resource Identifier) die wordt gebruikt om de aanvragen naar de server te verzenden.
> 
> Voor de opdrachten in deze sectie vervangt u **USERNAME** door de gebruiker die moet worden geverifieerd bij het cluster en vervangt u **PASSWORD** door het wachtwoord voor het gebruikersaccount. Vervang **CLUSTERNAME** door de naam van uw cluster.
> 
> De REST API is beveiligd via [basisverificatie](http://en.wikipedia.org/wiki/Basic_access_authentication). U moet aanvragen altijd uitvoeren via een beveiligde HTTP-verbinding (HTTPS). Zo zorgt u ervoor dat uw referenties veilig worden verzonden naar de server.
> 
> 

1. Gebruik een opdrachtregel met de volgende opdracht om te controleren of u verbinding met uw HDInsight-cluster kunt maken:

    ```bash   
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Het antwoord dat u ontvangt, is vergelijkbaar met het volgende antwoord:

    ```json   
    {"status":"ok","version":"v1"}
    ```
   
    In deze opdracht worden de volgende parameters gebruikt:
   
   * **-u**: de gebruikersnaam en het wachtwoord voor het verifiëren van de aanvraag.
   * **-G**: hiermee wordt aangegeven dat dit een GET-aanvraag is.
     
     Het begin van de URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen. Het pad **status**, geeft aan dat de aanvraag is de status van WebHCat (ook wel bekend als Templeton) voor de server. 
2. Gebruik de volgende om een taak sqoop te verzenden:

    ```bash
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

3. Gebruik de volgende opdracht om te controleren of de status van de taak. Vervang **JOBID** met de waarde die wordt geretourneerd in de vorige stap. Bijvoorbeeld, als de geretourneerde waarde is `{"id":"job_1415651640909_0026"}`, klikt u vervolgens **JOBID** zou `job_1415651640909_0026`.

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Als de taak is voltooid, worden de status **geslaagd**.
   
   > [!NOTE]
   > Deze aanvraag Curl retourneert een JavaScript Object Notation (JSON)-document met informatie over de taak. jq wordt gebruikt om op te halen, alleen de statuswaarde.
   > 
   > 
4. Zodra de status van de taak is gewijzigd in **geslaagd**, kunt u de resultaten van de taak ophalen uit Azure Blob-opslag. De `statusdir` parameter die is doorgegeven met de query bevat de locatie van het uitvoerbestand; in dit geval **wasb: / / / voorbeeld/data/sqoop/curl**. Dit adres slaat de uitvoer van de taak in de **voorbeeld/data/sqoop/curl** map op de standaard-storage-container die wordt gebruikt door uw HDInsight-cluster.
   
    U kunt de Azure-portal gebruiken voor toegang tot de stdout- en stderr-blobs.  U kunt ook Microsoft SQL Server Management Studio gebruiken om te controleren of de gegevens die wordt geüpload naar de tabel log4jlogs.

## <a name="limitations"></a>Beperkingen
* Bulksgewijs exporteren - met Linux gebaseerde HDInsight, de Sqoop-connector die wordt gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt momenteel geen ondersteuning voor bulksgewijs invoegen.
* Batchverwerking - met HDInsight op basis van Linux bij het gebruik van de `-batch` overschakelen tijdens het uitvoeren van ingevoegd, Sqoop meerdere invoegingen in plaats van de bewerkingen insert batchverwerking wordt uitgevoerd.

## <a name="summary"></a>Samenvatting
Zoals in dit document wordt gedemonstreerd, kunt u een onbewerkte HTTP-aanvraag uitvoeren, controleren en de resultaten van Sqoop taken weergeven op uw HDInsight-cluster.

Zie voor meer informatie over de REST-interface gebruikt in dit artikel de <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST-API-handleiding</a>.

## <a name="next-steps"></a>Volgende stappen
Voor algemene informatie over Hive met HDInsight:

* [Sqoop gebruiken met Hadoop op HDInsight](hdinsight-use-sqoop.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor andere HDInsight curl artikelen met betrekking tot:
 
* [Hadoop-clusters met behulp van de Azure REST API maken](../hdinsight-hadoop-create-linux-clusters-curl-rest.md)
* [Hive-query's uitvoeren met Hadoop in HDInsight met behulp van REST](apache-hadoop-use-hive-curl.md)
* [MapReduce-taken uitvoeren met Hadoop op HDInsight met behulp van REST](apache-hadoop-use-mapreduce-curl.md)
* [Pig-taken uitvoeren met Hadoop op HDInsight met cURL](apache-hadoop-use-pig-curl.md)




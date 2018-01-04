---
title: Hadoop Sqoop gebruiken met Curl in HDInsight - Azure | Microsoft Docs
description: Informatie over het op afstand Sqoop om taken te verzenden naar HDInsight met Curl.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 39798321-78ca-428c-bcfe-322e49af4059
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/03/2018
ms.author: jgao
ms.openlocfilehash: 08e7288d4d9d06f3a74464817892c2b03ae8a86c
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Sqoop taken uitvoeren met Hadoop in HDInsight met Curl
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van Curl Sqoop taken uitvoeren op een Hadoop-cluster in HDInsight.

CURL wordt gebruikt om u te laten zien hoe u kunt werken met HDInsight met behulp van onbewerkte HTTP-aanvragen worden uitgevoerd, controleren en ophalen van de resultaten van Sqoop taken. Dit werkt met behulp van de WebHCat REST-API (voorheen bekend als Templeton) geleverd door uw HDInsight-cluster.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van de stappen in dit artikel, moet u het volgende:

* Volledige [Sqoop gebruiken met Hadoop in HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database) voor het configureren van een omgeving met een HDInsight-cluster en een Azure SQL-database.
* [CURL](http://curl.haxx.se/). CURL is een hulpprogramma voor gegevensoverdracht van of naar een HDInsight-cluster.
* [jq](http://stedolan.github.io/jq/). Het hulpprogramma jq wordt gebruikt voor het verwerken van de JSON-gegevens geretourneerd van de REST-aanvragen.

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
     
     Het begin van de URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen. Het pad **/status**, geeft aan dat de aanvraag is de status van WebHCat (ook wel bekend als Templeton) voor de server. 
2. Gebruik de volgende voor het verzenden van een taak sqoop:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /example/data/sample.log --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/data/sqoop/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop
    ```

    In deze opdracht worden de volgende parameters gebruikt:

    * **-d** - sinds `-G` niet wordt gebruikt, de aanvraag wordt standaard ingesteld op de POST-methode. `-d`Hiermee geeft u de waarden die worden verzonden met de aanvraag.

        * **User.name** -de gebruiker die de opdracht wordt uitgevoerd.

        * **opdracht** -Sqoop van de opdracht wordt uitgevoerd.

        * **statusdir** -de map die de status voor deze taak om te worden geschreven.

    Met deze opdracht wordt een taak-ID die kan worden gebruikt om te controleren van de status van de taak retourneren.

        ```json
        {"id":"job_1415651640909_0026"}
        ```

3. Gebruik de volgende opdracht om te controleren van de status van de taak. Vervang **JOBID** met de waarde die wordt geretourneerd in de vorige stap. Bijvoorbeeld, als de retourwaarde is `{"id":"job_1415651640909_0026"}`, klikt u vervolgens **JOBID** zou `job_1415651640909_0026`.

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Als de taak is voltooid, worden de status **geslaagd**.
   
   > [!NOTE]
   > Deze aanvraag Curl retourneert een notatie JSON (JavaScript Object)-document met informatie over de taak. jq wordt gebruikt voor het ophalen van de waarde van de status.
   > 
   > 
4. Zodra de status van de taak is gewijzigd in **geslaagd**, kunt u de resultaten van de taak ophalen uit Azure Blob-opslag. De `statusdir` parameter doorgegeven aan de query bevat de locatie van het uitvoerbestand; in dit geval **wasb: / / / voorbeeld/data/sqoop/curl**. Dit adres slaat de uitvoer van de taak in de **voorbeeld/data/sqoop/curl** directory op de standaard storage-container die wordt gebruikt door uw HDInsight-cluster.
   
    U kunt de Azure portal gebruiken voor toegang tot blobs stderr en stdout.  U kunt ook Microsoft SQL Server Management Studio gebruiken om de gegevens dat is geüpload naar de tabel log4jlogs te controleren.

## <a name="limitations"></a>Beperkingen
* Bulksgewijs export - met Linux gebaseerde HDInsight, de Sqoop-connector gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt momenteel geen ondersteuning voor bulksgewijs invoegen.
* Batchverwerking - met HDInsight op basis van Linux, wanneer u de `-batch` bij het uitvoeren van invoeg-switch, Sqoop meerdere invoegen in plaats van de bewerkingen insert batchverwerking wordt uitgevoerd.

## <a name="summary"></a>Samenvatting
Zoals wordt beschreven in dit document, kunt u een onbewerkte HTTP-aanvraag uitvoeren, bewaken en bekijk de resultaten van Sqoop taken op uw HDInsight-cluster.

Zie voor meer informatie over de REST-interface in dit artikel gebruikt de <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST-API-handleiding</a>.

## <a name="next-steps"></a>Volgende stappen
Voor algemene informatie over Hive met HDInsight:

* [Sqoop gebruiken met Hadoop in HDInsight](hdinsight-use-sqoop.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor andere HDInsight curl artikelen met betrekking tot:
 
* [Hadoop-clusters met de REST-API van Azure maken](../hdinsight-hadoop-create-linux-clusters-curl-rest.md)
* [Hive-query's uitvoeren met Hadoop in HDInsight met behulp van REST](apache-hadoop-use-hive-curl.md)
* [MapReduce-taken uitvoeren met Hadoop op HDInsight met behulp van REST](apache-hadoop-use-mapreduce-curl.md)
* [Pig-taken uitvoeren met Hadoop in HDInsight met cURL](apache-hadoop-use-pig-curl.md)




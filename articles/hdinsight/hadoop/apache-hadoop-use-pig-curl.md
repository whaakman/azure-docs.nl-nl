---
title: Hadoop Pig gebruiken met HDInsight - Azure RUST | Microsoft Docs
description: Informatie over het gebruik van REST Pig Latin taken uitvoeren op een Hadoop-cluster in Azure HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ed5e10d1-4f47-459c-a0d6-7ff967b468c4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2018
ms.author: larryfr
ms.openlocfilehash: c2c441d92607a017b4451cd8a06fb78a5e8e7135
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a>Pig-taken uitvoeren met Hadoop in HDInsight met behulp van REST

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Informatie over het Pig Latin taken worden uitgevoerd door het maken van de REST-aanvragen met een Azure HDInsight-cluster. CURL wordt gebruikt om u te laten zien hoe u kunt werken met HDInsight met behulp van de WebHCat REST-API.

> [!NOTE]
> Als u al bekend bent met Hadoop op basis van Linux-servers gebruiken, maar niet bekend met HDInsight bent, raadpleegt u [Linux gebaseerde HDInsight Tips](../hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Vereisten

* Een Azure HDInsight (Hadoop in HDInsight)-cluster (op basis van Linux of op basis van Windows)

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Pig-taken uitvoeren met behulp van Curl

> [!NOTE]
> De REST-API is beveiligd via [basistoegang verificatie](http://en.wikipedia.org/wiki/Basic_access_authentication). Altijd aanvragen indienen via HTTP Secure (HTTPS) om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server.
>
> Wanneer u de opdrachten in deze sectie, Vervang `USERNAME` met de gebruiker om te verifiëren met het cluster en vervang `PASSWORD` met het wachtwoord voor het gebruikersaccount. Vervang `CLUSTERNAME` door de naam van uw cluster.
>


1. Gebruik een opdrachtregel met de volgende opdracht om te controleren of u verbinding met uw HDInsight-cluster kunt maken:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    U ontvangt het volgende JSON-antwoord:

        {"status":"ok","version":"v1"}

    In deze opdracht worden de volgende parameters gebruikt:

    * **-u**: de gebruikersnaam en wachtwoord voor het verifiëren van de aanvraag
    * **-G**: geeft aan dat deze aanvraag een aanvraag voor ophalen

     Het begin van de URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen. Het pad **/status**, geeft aan dat de aanvraag om te retourneren van de status van WebHCat (ook wel bekend als Templeton) voor de server.

2. De volgende code gebruikt voor het verzenden van een taak Pig Latin aan het cluster:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    In deze opdracht worden de volgende parameters gebruikt:

    * **-d**: omdat `-G` niet wordt gebruikt, de aanvraag wordt standaard ingesteld op de POST-methode. `-d`Hiermee geeft u de waarden die worden verzonden met de aanvraag.

    * **User.name**: de gebruiker die de opdracht wordt uitgevoerd
    * **uitvoeren van**: de Pig Latin-instructies uit te voeren
    * **statusdir**: de map die de status voor deze taak wordt geschreven naar

    > [!NOTE]
    > U ziet dat de spaties in Pig Latin-instructies zijn vervangen door de `+` gebruikt in combinatie met Curl teken.

    Met deze opdracht als resultaat moet een taak-ID die kan worden gebruikt om te controleren van de status van de taak, bijvoorbeeld:

        {"id":"job_1415651640909_0026"}

3. Gebruik de volgende opdracht om te controleren van de status van de taak,

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     Vervang `JOBID` met de waarde die wordt geretourneerd in de vorige stap. Bijvoorbeeld, als de retourwaarde is `{"id":"job_1415651640909_0026"}`, klikt u vervolgens `JOBID` is `job_1415651640909_0026`.

    Als de taak is voltooid, wordt de status is **geslaagd**.

    > [!NOTE]
    > Deze aanvraag Curl retourneert een notatie JSON (JavaScript Object)-document met informatie over de taak en jq wordt gebruikt voor het ophalen van de waarde van de status.

## <a id="results"></a>Resultaten weergeven

Wanneer de status van de taak is gewijzigd in **geslaagd**, kunt u de resultaten van de taak ophalen. De `statusdir` parameter doorgegeven aan de query bevat de locatie van het uitvoerbestand; in dit geval `/example/pigcurl`.

HDInsight kunt gebruiken Azure Storage of Azure Data Lake Store als het standaardarchief van gegevens. Er zijn verschillende manieren om de gegevens afhankelijk van welke u gebruiken. Zie voor meer informatie de sectie van de opslag van de [Linux gebaseerde HDInsight-informatie](../hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) document.

## <a id="summary"></a>Samenvatting

Zoals wordt beschreven in dit document, kunt u een onbewerkte HTTP-aanvraag uitvoeren, bewaken en bekijk de resultaten van Pig-taken op uw HDInsight-cluster.

Zie voor meer informatie over de REST-interface in dit artikel gebruikt de [WebHCat verwijzing](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over Pig op HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

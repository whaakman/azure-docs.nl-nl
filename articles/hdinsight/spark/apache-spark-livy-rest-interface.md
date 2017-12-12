---
title: Livy Spark gebruiken voor het verzenden van taken met Spark op Azure HDInsight-cluster | Microsoft Docs
description: Informatie over het Apache Spark REST API gebruiken om Spark-taken op afstand met een Azure HDInsight-cluster te verzenden.
keywords: Apache spark rest-api, spark livy
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2817b779-1594-486b-8759-489379ca907d
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: nitinme
ms.openlocfilehash: 05a50488793482ef761f34f4729c52181bc3eaf4
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="use-apache-spark-rest-api-to-submit-remote-jobs-to-an-hdinsight-spark-cluster"></a>Apache Spark REST API gebruiken om externe taken met een HDInsight Spark-cluster te verzenden

Informatie over het gebruik van Livy, Apache Spark REST API, die wordt gebruikt voor het verzenden van externe taken naar een Azure HDInsight Spark-cluster. Zie voor gedetailleerde documentatie [http://livy.incubator.apache.org/](http://livy.incubator.apache.org/).

Hier kunt u interactieve Spark houders uitvoeren of verzenden van batchtaken worden uitgevoerd op Spark. In dit artikel wordt gesproken over met behulp van Livy om batchtaken te verzenden. De codefragmenten in dit artikel wordt cURL gebruiken om REST-API-aanroepen naar de Livy Spark-eindpunt.

**Vereisten:**

* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

* [cURL](http://curl.haxx.se/). In dit artikel wordt cURL gebruikt om te laten zien hoe u REST-API-aanroepen op basis van een HDInsight Spark-cluster.

## <a name="submit-a-livy-spark-batch-job"></a>Een batchtaak Livy Spark verzenden
Voordat u een batch-job indienen, kunt u de toepassing jar in de clusteropslag die is gekoppeld aan het cluster moet uploaden. U kunt [ **AzCopy**](../../storage/common/storage-use-azcopy.md), een opdrachtregelprogramma, om dit te doen. Er zijn verschillende andere clients die u gebruiken kunt om gegevens te uploaden. U vindt meer informatie hierover op [gegevens voor Hadoop-taken in HDInsight uploaden](../hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Voorbeelden**:

* Als het jar-bestand in de clusteropslag (WASB)
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* Als u wilt de jar-bestandsnaam en de classname doorgeven als onderdeel van een bestand voor invoer (in dit voorbeeld input.txt)
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-livy-spark-batches-running-on-the-cluster"></a>Informatie over Livy Spark batches uitgevoerd op het cluster
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Voorbeelden**:

* Als u ophalen alle de Livy Spark batches uitgevoerd op het cluster wilt:
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* Als u wilt ophalen van een specifieke batch met een bepaalde batchId
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a>Een batch-job Livy Spark verwijderen
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Voorbeeld**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a>Livy Spark en hoge beschikbaarheid
Livy biedt hoge beschikbaarheid voor Spark taken uitgevoerd op het cluster. Hier volgt een aantal voorbeelden.

* Als de service Livy uitvalt nadat u een taak op afstand voor een Spark-cluster hebt verzonden, blijft de taak op de achtergrond uitgevoerd. Hier wordt een back-up, wordt de status van de taak en de rapporten weer hersteld.
* Jupyter-notebooks voor HDInsight worden door Livy van stroom voorzien in de back-end. Als een laptop een Spark-taak wordt uitgevoerd en de Livy-service opnieuw wordt gestart, blijft de notebook uitvoeren van de cellen van de code. 

## <a name="show-me-an-example"></a>Een voorbeeld weergeven
In dit gedeelte kijken we voorbeelden Livy Spark batchverwerking indienen, de voortgang van de taak en verwijder deze vervolgens gebruiken. De toepassing in dit voorbeeld we gebruiken is ontwikkeld in het artikel [maken van een zelfstandige toepassing Scala en uit te voeren op HDInsight Spark-cluster](apache-spark-create-standalone-application.md). De stappen die hier wordt ervan uitgegaan dat:

* U hebt al de jar toepassing gekopieerd naar het opslagaccount die is gekoppeld aan het cluster.
* U hebt geïnstalleerd op de computer waarop u deze stappen probeert CuRL.

De volgende stappen uitvoeren:

1. Laten we eerst controleren of Livy Spark wordt uitgevoerd op het cluster. We kunnen dit doen door het ophalen van een lijst van het uitvoeren van batches. Als u een taak met behulp van Livy voor de eerste keer uitvoert, moet de uitvoer nul retourneren.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    U moet uitvoer vergelijkbaar met het volgende fragment krijgen:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    U ziet hoe de laatste regel in de uitvoer zegt **totaal: 0**, die geen actieve batches wordt voorgesteld.

2. Laten we nu een batch-job indienen. Het volgende fragment maakt gebruik van een bestand voor invoer (input.txt) om door te geven van de jar-naam en de naam van de klasse als parameters. Als u deze stappen vanuit een Windows-computer uitvoert, is met behulp van een bestand voor invoer de aanbevolen aanpak.
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    De parameters in het bestand **input.txt** als volgt gedefinieerd:
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    Als het goed is, wordt ongeveer het volgende codefragment weergegeven:
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    U ziet hoe de laatste regel van de uitvoer zegt **status: vanaf**. Ook wordt, **-id: 0**. Hier **0** is van de batch-ID.

3. U kunt nu de status van deze specifieke batch met behulp van de batch-id ophalen
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Als het goed is, wordt ongeveer het volgende codefragment weergegeven:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    De uitvoer ziet u nu **status: geslaagd**, die wordt voorgesteld dat de taak is voltooid.

4. Als u wilt, kunt u nu de batch verwijderen.
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Als het goed is, wordt ongeveer het volgende codefragment weergegeven:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    De laatste regel van de uitvoer bevat de batch is verwijderd. Verwijderen van een taak, terwijl deze wordt uitgevoerd, de taak ook is funest. Als u een taak die is voltooid, is of anderszins verwijdert worden verwijderd de taakgegevens volledig.

## <a name="using-livy-spark-on-hdinsight-35-clusters"></a>Gebruik van Livy Spark in HDInsight 3.5 clusters

3.5 HDInsight-cluster, schakelt het gebruik van lokale paden naar bestanden met voorbeeldgegevens toegang of potten standaard. We raden u aan het gebruik van de `wasb://` pad in plaats daarvan potten openen of voorbeeldgegevens bestanden van het cluster. Als u gebruiken, lokaal pad wilt, moet u de configuratie van de Ambari dienovereenkomstig bijwerken. Dit doet u als volgt:

1. Ga naar de Ambari-portal voor het cluster. De Ambari-Webgebruikersinterface is beschikbaar op uw HDInsight-cluster op https://**CLUSTERNAME**. azurehdidnsight.net, waarbij CLUSTERNAME de naam van het cluster is.

2. Klik in het linkernavigatievenster op **Livy**, en klik vervolgens op **Configs**.

3. Onder **livy-standaard** toevoegen de naam van de eigenschap `livy.file.local-dir-whitelist` en stel de waarde op **'/'** als u wilt toestaan dat volledige toegang tot bestandssysteem. Als u toestaan dat alleen toegang tot een specifieke map wilt, geeft u het pad naar map als de waarde.

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a>Verzenden van Livy taken voor een cluster binnen een virtuele Azure-netwerk

Als u verbinding met een HDInsight Spark-cluster uit een Azure-netwerk maakt, kunt u rechtstreeks verbinding met Livy op het cluster. In dat geval moet de URL voor het eindpunt van Livy is `http://<IP address of the headnode>:8998/batches`. Hier **8998** is de poort waarop Livy wordt uitgevoerd op de cluster-headnode. Zie voor meer informatie over de toegang tot services op niet-openbaar poorten [poorten die worden gebruikt door de services van Hadoop op HDInsight](../hdinsight-hadoop-port-settings-for-services.md).

## <a name="troubleshooting"></a>Problemen oplossen

Hier volgen enkele problemen beschreven die u tegenkomen kunt tijdens het gebruik van Livy voor verzenden van externe taak Spark-clusters.

### <a name="using-an-external-jar-from-the-additional-storage-is-not-supported"></a>Gebruik een externe jar uit de extra opslag wordt niet ondersteund

**Probleem:** als uw job Livy Spark verwijst naar een externe jar vanuit het extra opslagaccount die is gekoppeld aan het cluster, de taak is mislukt.

**Oplossing:** Zorg ervoor dat de jar die u wilt gebruiken beschikbaar in de standaard-opslag die is gekoppeld aan het HDInsight-cluster is.





## <a name="next-step"></a>Volgende stap

* [Livy REST API-documentatie](http://livy.incubator.apache.org/docs/latest/rest-api.html)
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)


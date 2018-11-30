---
title: Livy Spark gebruiken voor het verzenden van taken naar Spark-cluster in Azure HDInsight
description: Leer hoe u Apache Spark REST-API gebruiken voor het indienen van Spark-taken op afstand met een Azure HDInsight-cluster.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 6a0a2dec6beeecca3779f4b047d3b5fe6295a1e6
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495305"
---
# <a name="use-apache-spark-rest-api-to-submit-remote-jobs-to-an-hdinsight-spark-cluster"></a>Apache Spark REST API gebruiken voor het verzenden van externe taken naar een HDInsight Spark-cluster

Meer informatie over het gebruik van [Apache Livy](https://livy.incubator.apache.org/), wordt de [Apache Spark](https://spark.apache.org/) REST-API, die wordt gebruikt voor het verzenden van externe taken naar een Azure HDInsight Spark-cluster. Zie voor gedetailleerde documentatie [ http://livy.incubator.apache.org/ ](http://livy.incubator.apache.org/).

Hier kunt u uitvoeren van interactieve Spark shells of verzenden van batchtaken kunnen worden uitgevoerd op Spark. In dit artikel wordt besproken met behulp van Livy batchtaken indienen. De codefragmenten in dit artikel gebruiken we cURL voor REST-API-aanroepen naar het eindpunt Livy Spark.

**Vereisten:**

* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.

* [cURL](http://curl.haxx.se/). In dit artikel wordt cURL gebruikt om u te laten zien hoe u REST API-aanroepen op basis van een HDInsight Spark-cluster.

## <a name="submit-an-apache-livy-spark-batch-job"></a>Een Apache Livy Spark batch-taak verzenden
Voordat u een batch-taak verzendt, moet u de toepassing jar in de clusteropslag die is gekoppeld aan het cluster te uploaden. Dit kan met [**AzCopy**](../../storage/common/storage-use-azcopy.md), een opdrachtregelprogramma. Er zijn verschillende andere clients die u gebruiken kunt om gegevens te uploaden. U vindt meer informatie over deze [gegevens uploaden voor Apache Hadoop-taken in HDInsight](../hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches' -H "X-Requested-By: admin"

**Voorbeelden**:

* Als het jar-bestand zich op de cluster-storage (WASB)
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"
* Als u wilt de jar-bestandsnaam en de classname door te geven als onderdeel van een bestand voor invoer (in dit voorbeeld input.txt)
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"

## <a name="get-information-on-livy-spark-batches-running-on-the-cluster"></a>Informatie over Spark Livy batches uitgevoerd op het cluster
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Voorbeelden**:

* Als u ophalen van alle batches Livy Spark uitgevoerd op het cluster wilt:
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches" 
* Als u wilt ophalen van een specifieke batch met een bepaalde batchId
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a>Een Livy Spark batch-taak verwijderen
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Voorbeeld**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a>Livy Spark en hoge beschikbaarheid
Livy biedt hoge beschikbaarheid voor Spark taken die worden uitgevoerd op het cluster. Hier volgt een aantal voorbeelden.

* Als de Livy-service wordt uitgeschakeld nadat u een taak op afstand voor een Spark-cluster hebt verzonden, blijft de taak op de achtergrond worden uitgevoerd. Hier wordt een back-up, wordt de status van de taak en de rapporten weer hersteld.
* Jupyter-notebooks voor HDInsight worden aangestuurd door Livy in de back-end. Als een laptop een Spark-taak wordt uitgevoerd en de Livy-service opnieuw wordt gestart, blijft het notitieblok om uit te voeren van de cellen met code. 

## <a name="show-me-an-example"></a>Een voorbeeld weergeven
In deze sectie kijken we voorbeelden Livy Spark gebruiken voor het indienen van batch-taak, de voortgang van de taak en deze te verwijderen. De toepassing we in dit voorbeeld gebruiken is ontwikkeld in het artikel [maken van een zelfstandige Scala-toepassing en worden uitgevoerd op een HDInsight Spark-cluster](apache-spark-create-standalone-application.md). De stappen die hier wordt ervan uitgegaan dat:

* U hebt al de jar toepassing gekopieerd naar het opslagaccount dat is gekoppeld aan het cluster.
* U hebt geïnstalleerd op de computer waarop u deze stappen probeert CuRL.

Voer de volgende stappen uit:

1. Laat het ons eerst controleren of Livy Spark wordt uitgevoerd op het cluster. We kunnen dit doen door het ophalen van een lijst met batches. Als u een taak met behulp van Livy voor het eerst uitvoert, moet de uitvoer nul geretourneerd.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    Deze krijgt u uitvoer die vergelijkbaar is met het volgende codefragment:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    U ziet hoe de laatste regel in de uitvoer zegt **totaal: 0**, wat erop duidt geen actieve batches.

2. Laat het ons nu een batchtaak verzenden. Het volgende fragment wordt een bestand voor invoer (input.txt) om door te geven van de jar-naam en de naam van de klasse als parameters. Als u deze stappen vanaf een Windows-computer uitvoert, is met behulp van een bestand voor invoer de aanbevolen methode.
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"
   
    De parameters in het bestand **input.txt** zijn als volgt gedefinieerd:
   
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
   
    U ziet hoe de laatste regel van de uitvoer zegt **status: vanaf**. Ook de status, **-id: 0**. Hier **0** is van de batch-ID.

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
   
    De uitvoer nu bevat **status: geslaagd**, wat erop duidt dat de taak is voltooid.

4. Als u wilt, kunt u de batch nu verwijderen.
   
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
   
    De laatste regel van de uitvoer laat zien dat de batch is verwijderd. Verwijderen van een taak, terwijl deze wordt uitgevoerd, ook beëindigt de taak. Als u een taak die is voltooid, of anders is, verwijdert deze Hiermee verwijdert u de informatie over de taak volledig.

## <a name="updates-to-livy-configuration-starting-with-hdinsight-35-version"></a>Updates voor de configuratie van Livy vanaf versie 3.5 voor HDInsight

HDInsight 3.5-clusters en uitschakelen hierboven standaard gebruik van lokale paden naar bestanden met voorbeeldgegevens toegang of JAR-bestanden. We raden u aan het gebruik van de `wasb://` pad in plaats daarvan voor toegang tot JAR-bestanden of voorbeeldgegevens bestanden uit het cluster. Als u gebruiken, lokaal pad wilt, moet u de Ambari-configuratie dienovereenkomstig bijgewerkt. Dit doet u als volgt:

1. Ga naar de Ambari-portal voor het cluster. De Ambari-Webinterface is beschikbaar op uw HDInsight-cluster op https://**CLUSTERNAME**. azurehdidnsight.net, waarbij CLUSTERNAME de naam van uw cluster is.

2. Klik in de linkernavigatiebalk op **Livy**, en klik vervolgens op **Peeringconfiguraties**.

3. Onder **livy-standaard** toevoegen van de naam van de eigenschap `livy.file.local-dir-whitelist` en stel de waarde voor **'/'** als u wilt toestaan dat volledige toegang tot bestandssysteem. Als u toestaan dat alleen toegang tot een specifieke map wilt, geeft u het pad naar die map als de waarde.

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a>Indienen van Livy-taken voor een cluster in een Azure-netwerk

Als u verbinding met een HDInsight Spark-cluster op basis van binnen een virtueel Azure-netwerk maakt, kunt u rechtstreeks verbinding met Livy op het cluster. In dat geval moet de URL voor het eindpunt van Livy is `http://<IP address of the headnode>:8998/batches`. Hier **8998** is de poort waarop Livy op het hoofdknooppunt van het cluster wordt uitgevoerd. Zie voor meer informatie over het verkrijgen van toegang tot services op niet-openbare poorten [poorten die worden gebruikt door de services van Apache Hadoop op HDInsight](../hdinsight-hadoop-port-settings-for-services.md).

## <a name="troubleshooting"></a>Problemen oplossen

Hier volgen enkele problemen beschreven die u kunt tegenkomen tijdens het gebruik van Livy externe taak verzenden naar de Spark-clusters.

### <a name="using-an-external-jar-from-the-additional-storage-is-not-supported"></a>Gebruik een externe jar van de extra opslagruimte wordt niet ondersteund

**Probleem:** als uw Livy Spark-taak verwijst naar een externe jar van de extra opslagaccount dat is gekoppeld aan het cluster, de taak is mislukt.

**Oplossing:** Zorg ervoor dat de jar die u wilt gebruiken in de standaardopslag die is gekoppeld aan het HDInsight-cluster beschikbaar is.





## <a name="next-step"></a>Volgende stap

* [Apache Livy REST API-documentatie](http://livy.incubator.apache.org/docs/latest/rest-api.html)
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)


---
title: Hadoop Hive gebruiken met Curl in HDInsight - Azure | Microsoft Docs
description: Informatie over het op afstand verzenden van Pig-taken naar HDInsight met Curl.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: b451a80934a19f8a38ab9e8ace358674827aefa0
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>Hive-query's uitvoeren met Hadoop in HDInsight met behulp van REST

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Informatie over het gebruik van de REST-API WebHCat Hive-query's uitvoeren met Hadoop op Azure HDInsight-cluster.

[CURL](http://curl.haxx.se/) wordt gebruikt om u te laten zien hoe u kunt werken met HDInsight met behulp van onbewerkte HTTP-aanvragen. De [jq](http://stedolan.github.io/jq/) hulpprogramma wordt gebruikt voor het verwerken van de JSON-gegevens geretourneerd van de REST-aanvragen.

> [!NOTE]
> Als u al bekend bent met Hadoop op basis van Linux-servers gebruiken, maar niet bekend met HDInsight bent, raadpleegt u de [wat u moet weten over Hadoop op Linux gebaseerde HDInsight](../hdinsight-hadoop-linux-information.md) document.

## <a id="curl"></a>Hive-query's uitvoeren

> [!NOTE]
> Wanneer u cURL of andere REST-communicatie met WebHCat, moet u de aanvragen verifiëren door te geven van de gebruikersnaam en het wachtwoord voor de beheerder van de HDInsight-cluster.
>
> Voor de opdrachten in deze sectie vervangt u **admin** met de gebruiker om het cluster te verifiëren. Vervang **CLUSTERNAME** door de naam van uw cluster. Voer desgevraagd het wachtwoord voor het gebruikersaccount.
>
> De REST API is beveiligd via [basisverificatie](http://en.wikipedia.org/wiki/Basic_access_authentication). Om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server, moet u altijd aanvragen maken via HTTP Secure (HTTPS).

1. Gebruik een opdrachtregel met de volgende opdracht om te controleren of u verbinding met uw HDInsight-cluster kunt maken:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    U ontvangt een reactie vergelijkbaar met de volgende tekst:

    ```json
    {"status":"ok","version":"v1"}
    ```

    In deze opdracht worden de volgende parameters gebruikt:

    * **-u**: de gebruikersnaam en het wachtwoord voor het verifiëren van de aanvraag.
    * **-G** -geeft aan dat deze aanvraag een GET-bewerking.

   Het begin van de URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen. Het pad **/status**, geeft aan dat de aanvraag is de status van WebHCat (ook wel bekend als Templeton) voor de server. U kunt ook de versie van Hive aanvragen via de volgende opdracht:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

    Deze aanvraag retourneert een reactie vergelijkbaar met de volgende tekst:

    ```json
        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}
    ```

2. Gebruik de volgende voor het maken van een tabel met de naam **log4jLogs**:

    ```bash
    curl -u admin -d user.name=admin -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    De volgende parameters voor deze aanvraag:

   * **-d** - sinds `-G` niet wordt gebruikt, de aanvraag wordt standaard ingesteld op de POST-methode. `-d`Hiermee geeft u de waarden die worden verzonden met de aanvraag.

     * **User.name** -de gebruiker die de opdracht wordt uitgevoerd.
     * **uitvoeren van** -de HiveQL-instructies uit te voeren.
     * **statusdir** -de map die naar de status voor deze taak worden geschreven.

   Deze instructies uitvoeren de volgende acties:
   
   * **DROP TABLE** -als de tabel al bestaat, wordt deze verwijderd.
   * **EXTERNE tabel maken** -maakt een nieuwe 'extern' tabel in Hive. Externe tabellen worden de definitie van de tabel in Hive opslaan. De gegevens blijft op de oorspronkelijke locatie.

     > [!NOTE]
     > Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens wordt bijgewerkt door een externe bron verwacht. Bijvoorbeeld, een uploadproces geautomatiseerde gegevens of een andere MapReduce-bewerking.
     >
     > Verwijderen van een externe tabel komt **niet** de gegevens, de definitie van de tabel verwijderen.

   * **INDELING van de rij** : hoe de gegevens zijn ingedeeld. De velden in elk logboek worden gescheiden door een spatie.
   * **AS TEXTFILE locatie opgeslagen** - waar de gegevens worden opgeslagen (de map met de voorbeeldgegevens /) en dat deze is opgeslagen als tekst.
   * **Selecteer** -selecteert een telling van alle rijen waarin kolom **t4** bevat de waarde **[fout]**. Deze instructie retourneert een waarde van **3** omdat er zijn drie rijen met deze waarde.

     > [!NOTE]
     > U ziet dat de spaties tussen HiveQL-instructies zijn vervangen door de `+` gebruikt in combinatie met Curl teken. Tussen aanhalingstekens waarden met een spatie, zoals het scheidingsteken moeten niet worden vervangen door `+`.

   * **INPUT__FILE__NAME zoals '% 25.log'** -deze instructie wordt de zoekopdracht voor het gebruik van alleen bestanden die eindigen beperkt. log.

     > [!NOTE]
     > De `%25` is de URL gecodeerde vorm van %, zodat de werkelijke conditie `like '%.log'`. De % is URL zijn gecodeerd, zoals dit beschouwd als een speciaal teken in URL's.

   Met deze opdracht wordt een taak-ID die kan worden gebruikt om de status van de taak te controleren.

    ```json
       {"id":"job_1415651640909_0026"}
    ```

3. Als u wilt de status van de taak controleren, moet u de volgende opdracht gebruiken:

    ```bash
    curl -G -u admin -d user.name=admin https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Vervang **JOBID** met de waarde die wordt geretourneerd in de vorige stap. Bijvoorbeeld, als de retourwaarde is `{"id":"job_1415651640909_0026"}`, klikt u vervolgens **JOBID** zou `job_1415651640909_0026`.

    Als de taak is voltooid, wordt de status is **geslaagd**.

   > [!NOTE]
   > Deze aanvraag Curl retourneert een notatie JSON (JavaScript Object)-document met informatie over de taak. Jq wordt gebruikt voor het ophalen van de waarde van de status.

4. Zodra de status van de taak is gewijzigd in **geslaagd**, kunt u de resultaten van de taak ophalen uit Azure Blob-opslag. De `statusdir` parameter doorgegeven aan de query bevat de locatie van het uitvoerbestand; in dit geval **voorbeeld/curl**. Dit adres slaat de uitvoer in de **voorbeeld/curl** map in de opslag van de standaard clusters.

    U kunt de lijst en deze bestanden downloaden met behulp van de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Zie voor meer informatie over het gebruik van de Azure CLI met Azure Storage de [2.0 voor Azure CLI gebruiken met Azure Storage](https://docs.microsoft.com/azure/storage/storage-azure-cli#create-and-manage-blobs) document.

5. Gebruik de volgende instructies voor het maken van een nieuwe 'interne' tabel met de naam **foutenlogboeken**:

    ```bash
    curl -u admin -d user.name=admin -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Deze instructies uitvoeren de volgende acties:

   * **MAKEN van tabel als niet bestaat** -maakt een tabel, als deze niet al bestaat. Deze instructie maakt u een interne tabel die is opgeslagen in het datawarehouse Hive. Deze tabel wordt beheerd door Hive.

     > [!NOTE]
     > In tegenstelling tot externe tabellen voor het verwijderen van een interne tabel worden verwijderd en de onderliggende gegevens.

   * **OPGESLAGEN AS ORC** -slaat de gegevens geoptimaliseerd rij kolommen (ORC)-indeling. ORC is een maximaal geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens met Hive.
   * **INSERT OVERSCHRIJVEN... Selecteer** -selecteert rijen uit de **log4jLogs** tabel met **[fout]**, voegt u vervolgens de gegevens in de **foutenlogboeken** tabel.
   * **Selecteer** -selecteert alle rijen uit de nieuwe **foutenlogboeken** tabel.

6. Gebruik de taak-ID geretourneerd om de status van de taak te controleren. Zodra deze is geslaagd, de Azure CLI gebruiken zoals wordt beschreven eerder om te downloaden en de resultaten bekijken. De uitvoer moet drie regels, die allemaal bevatten bevatten **[fout]**.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over Hive met HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u van Tez met Hive gebruikmaakt, raadpleegt u de volgende documenten voor het opsporen van informatie:

* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](../hdinsight-debug-ambari-tez-view.md)

Zie voor meer informatie over de REST-API in dit document gebruikt de [WebHCat verwijzing](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) document.

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx



---
title: Hadoop Hive gebruiken met Curl in HDInsight - Azure | Microsoft Docs
description: Informatie over het op afstand verzenden van Pig-taken naar HDInsight met Curl.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/26/2018
ms.author: larryfr
ms.openlocfilehash: 8a7627e35a39dd4c9c94d273aa9f02c977dc88bf
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>Hive-query's uitvoeren met Hadoop in HDInsight met behulp van REST

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Informatie over het gebruik van de REST-API WebHCat Hive-query's uitvoeren met Hadoop op Azure HDInsight-cluster.

## <a name="prerequisites"></a>Vereisten

* Een op Linux gebaseerde Hadoop op HDInsight-cluster versie 3.4 of hoger.

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een REST-client. Dit document wordt Windows PowerShell en [Curl](http://curl.haxx.se/) voorbeelden.

    > [!NOTE]
    > Azure PowerShell biedt speciale cmdlets voor het werken met Hive in HDInsight. Zie voor meer informatie de [Hive gebruiken met Azure PowerShell](apache-hadoop-use-hive-powershell.md) document.

Dit document wordt ook gebruikgemaakt van Windows PowerShell en [Jq](http://stedolan.github.io/jq/) de JSON-gegevens geretourneerd van de REST-aanvragen verwerken.

## <a id="curl"></a>Een Hive-query uitvoeren

> [!NOTE]
> Wanneer u cURL of andere REST-communicatie met WebHCat, moet u de aanvragen verifiëren door te geven van de gebruikersnaam en het wachtwoord voor de beheerder van de HDInsight-cluster.
>
> De REST API is beveiligd via [basisverificatie](http://en.wikipedia.org/wiki/Basic_access_authentication). Om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server, moet u altijd aanvragen maken via HTTP Secure (HTTPS).

1. Als u wilt dat de cluster-aanmelding die wordt gebruikt door de scripts in dit document, moet u een van de volgende opdrachten gebruiken:

    ```bash
    read -p "Enter your cluster login account name: " LOGIN
    ```

    ```powershell
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login name and password"
    ```

2. Naam van het cluster stelt een van de volgende opdrachten te gebruiken:

    ```bash
    read -p "Enter the HDInsight cluster name: " CLUSTERNAME
    ```

    ```powershell
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    ```

3. Om te controleren of u verbinding met uw HDInsight-cluster maken kunt, moet u een van de volgende opdrachten gebruiken:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/status)
    ```
    
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/status" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    U ontvangt een reactie vergelijkbaar met de volgende tekst:

    ```json
    {"status":"ok","version":"v1"}
    ```

    In deze opdracht worden de volgende parameters gebruikt:

    * `-u` -De gebruikersnaam en wachtwoord voor het verifiëren van de aanvraag.
    * `-G` -Geeft aan dat deze aanvraag een GET-bewerking.

   Het begin van de URL `https://$CLUSTERNAME.azurehdinsight.net/templeton/v1`, is hetzelfde voor alle aanvragen. Het pad `/status`, geeft aan dat de aanvraag is de status van WebHCat (ook wel bekend als Templeton) voor de server. U kunt ook de versie van Hive aanvragen via de volgende opdracht:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/version/hive" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Deze aanvraag retourneert een reactie vergelijkbaar met de volgende tekst:

    ```json
        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}
    ```

4. Gebruik de volgende voor het maken van een tabel met de naam **log4jLogs**:

    ```bash
    JOBID=`curl -s -u $LOGIN -d user.name=$LOGIN -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/rest" https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/hive | jq .id`
    echo $JOBID
    ```

    ```powershell
    $reqParams = @{"user.name"="admin";"execute"="set hive.execution.engine=tez;DROP TABLE log4jLogs;CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED BY ' ' STORED AS TEXTFILE LOCATION '/example/data/;SELECT t4 AS sev,COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;";"statusdir"="/example/rest"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/hive" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    De POST-methode, die gegevens als onderdeel van de aanvraag naar de REST-API verzonden maakt gebruik van deze aanvraag. De volgende gegevenswaarden worden verzonden met de aanvraag:

     * `user.name` -De gebruiker die de opdracht wordt uitgevoerd.
     * `execute` -De HiveQL-instructies uit te voeren.
     * `statusdir` -De map die naar de status voor deze taak worden geschreven.

   Deze instructies uitvoeren de volgende acties:
   
   * `DROP TABLE` -Als de tabel al bestaat, wordt deze verwijderd.
   * `CREATE EXTERNAL TABLE` -Maakt een nieuwe tabel 'extern' in Hive. Externe tabellen worden de definitie van de tabel in Hive opslaan. De gegevens blijft op de oorspronkelijke locatie.

     > [!NOTE]
     > Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens wordt bijgewerkt door een externe bron verwacht. Bijvoorbeeld, een uploadproces geautomatiseerde gegevens of een andere MapReduce-bewerking.
     >
     > Verwijderen van een externe tabel komt **niet** de gegevens, de definitie van de tabel verwijderen.

   * `ROW FORMAT` -Hoe de gegevens zijn ingedeeld. De velden in elk logboek worden gescheiden door een spatie.
   * `STORED AS TEXTFILE LOCATION` -Waar de gegevens worden opgeslagen (de map met de voorbeeldgegevens /) en dat deze is opgeslagen als tekst.
   * `SELECT` : Hiermee kunt u een telling van alle rijen waarin kolom **t4** bevat de waarde **[fout]**. Deze instructie retourneert een waarde van **3** omdat er zijn drie rijen met deze waarde.

     > [!NOTE]
     > U ziet dat de spaties tussen HiveQL-instructies zijn vervangen door de `+` gebruikt in combinatie met Curl teken. Tussen aanhalingstekens waarden met een spatie, zoals het scheidingsteken moeten niet worden vervangen door `+`.

      Met deze opdracht wordt een taak-ID die kan worden gebruikt om de status van de taak te controleren.

5. Als u wilt de status van de taak controleren, moet u de volgende opdracht gebruiken:

    ```bash
    curl -G -u $LOGIN -d user.name=$LOGIN https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/$JOBID | jq .status.state
    ```

    ```powershell
    $reqParams=@{"user.name"="admin"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobID" `
       -Credential $creds `
       -Body $reqParams `
       -UseBasicParsing
    # ConvertFrom-JSON can't handle duplicate names with different case
    # So change one to prevent the error
    $fixDup=$resp.Content.Replace("jobID","job_ID")
    (ConvertFrom-Json $fixDup).status.state
    ```

    Als de taak is voltooid, wordt de status is **geslaagd**.

6. Zodra de status van de taak is gewijzigd in **geslaagd**, kunt u de resultaten van de taak ophalen uit Azure Blob-opslag. De `statusdir` parameter doorgegeven aan de query bevat de locatie van het uitvoerbestand; in dit geval `/example/rest`. Dit adres slaat de uitvoer in de `example/curl` map in de opslag van de standaard clusters.

    U kunt de lijst en deze bestanden downloaden met behulp van de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Zie voor meer informatie over het gebruik van de Azure CLI met Azure Storage de [2.0 voor Azure CLI gebruiken met Azure Storage](https://docs.microsoft.com/azure/storage/storage-azure-cli#create-and-manage-blobs) document.

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



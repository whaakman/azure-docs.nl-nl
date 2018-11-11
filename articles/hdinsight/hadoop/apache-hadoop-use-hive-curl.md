---
title: Hadoop Hive gebruiken met Curl in HDInsight - Azure
description: Informatie over het op afstand verzenden van Pig-taken naar HDInsight met Curl.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: ac17c299656cc8134141284e0b31bdf568c0e3e0
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016944"
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>Hive-query's uitvoeren met Hadoop in HDInsight met behulp van REST

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Leer hoe u de WebHCat REST-API gebruiken om het Hive-query's uitvoeren met Hadoop op Azure HDInsight-cluster.

## <a name="prerequisites"></a>Vereisten

* Een op Linux gebaseerde Hadoop op HDInsight-clusterversie 3.4 of hoger.

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een REST-client. Dit document maakt gebruik van Windows PowerShell en [Curl](http://curl.haxx.se/) voorbeelden.

    > [!NOTE]
    > Azure PowerShell biedt toegewezen cmdlets voor het werken met Hive in HDInsight. Zie voor meer informatie de [Hive gebruiken met Azure PowerShell](apache-hadoop-use-hive-powershell.md) document.

Dit document maakt ook gebruik van Windows PowerShell en [Jq](http://stedolan.github.io/jq/) de JSON-gegevens geretourneerd door de REST-aanvragen verwerken.

## <a id="curl"></a>Een Hive-query uitvoeren

> [!NOTE]
> Wanneer u cURL of een andere REST-communicatie met WebHCat, moet u de aanvragen verifiëren door te geven van de gebruikersnaam en het wachtwoord voor de beheerder van de HDInsight-cluster.
>
> De REST API is beveiligd via [basisverificatie](http://en.wikipedia.org/wiki/Basic_access_authentication). Om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server, moet u aanvragen altijd uitvoeren via HTTP Secure (HTTPS).

1. Om in te stellen de aanmelding bij cluster dat wordt gebruikt door de scripts in dit document, gebruikt u een van de volgende opdrachten:

    ```bash
    read -p "Enter your cluster login account name: " LOGIN
    ```

    ```powershell
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login name and password"
    ```

2. Om in te stellen de clusternaam, gebruikt u een van de volgende opdrachten:

    ```bash
    read -p "Enter the HDInsight cluster name: " CLUSTERNAME
    ```

    ```powershell
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    ```

3. Als u wilt controleren of u verbinding kunt maken met uw HDInsight-cluster, gebruikt u een van de volgende opdrachten:

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
    * `-G` -Geeft aan dat deze aanvraag een GET-bewerking is.

   Het begin van de URL, `https://$CLUSTERNAME.azurehdinsight.net/templeton/v1`, is hetzelfde voor alle aanvragen. Het pad `/status`, geeft aan dat de aanvraag is de status van WebHCat (ook wel bekend als Templeton) voor de server. U kunt ook de versie van Hive vragen met behulp van de volgende opdracht uit:

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

4. Het volgende gebruiken om te maken van een tabel met de naam **log4jLogs**:

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

    De POST-methode, waarmee gegevens als onderdeel van de aanvraag voor de REST-API verzonden maakt gebruik van deze aanvraag. De volgende gegevenswaarden worden verzonden met de aanvraag:

     * `user.name` -De gebruiker die de opdracht wordt uitgevoerd.
     * `execute` -De HiveQL-instructies uit te voeren.
     * `statusdir` -De map die de status voor deze taak worden geschreven naar.

   Deze instructies uit uitvoeren de volgende acties:
   
   * `DROP TABLE` -Als de tabel al bestaat, wordt deze verwijderd.
   * `CREATE EXTERNAL TABLE` -Maakt een nieuwe 'externe' tabel in Hive. Externe tabellen worden alleen de definitie van de tabel in Hive opslaan. De gegevens is achtergebleven in de oorspronkelijke locatie.

     > [!NOTE]
     > Externe tabellen moeten worden gebruikt wanneer u verwacht dat de onderliggende gegevens worden bijgewerkt door een externe bron. Bijvoorbeeld, een geautomatiseerd proces of een andere bewerking van MapReduce uploaden.
     >
     > Verwijderen van een externe tabel heeft **niet** de gegevens, alleen de definitie van de tabel verwijderen.

   * `ROW FORMAT` -Hoe de gegevens zijn opgemaakt. De velden in elk logboek worden gescheiden door een spatie.
   * `STORED AS TEXTFILE LOCATION` -Waar de gegevens worden opgeslagen (de voorbeeldgegevens/map) en dat deze wordt opgeslagen als tekst.
   * `SELECT` : Hiermee kunt u een aantal van alle rijen waar kolom **t4** bevat de waarde **[fout]**. Deze instructie retourneert een waarde van **3** als er drie rijen die deze waarde bevatten.

     > [!NOTE]
     > U ziet dat de adresruimten tussen HiveQL-instructies zijn vervangen door de `+` gebruikt in combinatie met Curl teken. Genoteerde waarden met een spatie, zoals het scheidingsteken, moeten niet worden vervangen door `+`.

      Met deze opdracht retourneert een taak-ID die kan worden gebruikt om de status van de taak te controleren.

5. Om te controleren of de status van de taak, gebruikt u de volgende opdracht:

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

6. Zodra de status van de taak is gewijzigd in **geslaagd**, kunt u de resultaten van de taak ophalen uit Azure Blob-opslag. De `statusdir` parameter die is doorgegeven met de query bevat de locatie van het uitvoerbestand; in dit geval `/example/rest`. Dit adres slaat de uitvoer in de `example/curl` map in de standaardopslag voor clusters.

    U kunt de lijst en deze bestanden downloaden met behulp van de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Zie voor meer informatie over het gebruik van de Azure CLI met Azure Storage de [Azure CLI gebruiken met Azure Storage](https://docs.microsoft.com/azure/storage/storage-azure-cli#create-and-manage-blobs) document.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over Hive met HDInsight:

* [Hive gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u met Hive Tez gebruikt, ziet u de volgende documenten voor foutopsporing van informatie:

* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](../hdinsight-debug-ambari-tez-view.md)

Zie voor meer informatie over de REST-API die wordt gebruikt in dit document, de [WebHCat verwijzing](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) document.

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



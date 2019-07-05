---
title: Apache Hadoop Hive gebruiken met Curl in HDInsight - Azure
description: Informatie over het op afstand Apache Pig-taken verzenden naar HDInsight met Curl.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: hrasheed
ms.openlocfilehash: 334d7b886aa4e2130a12f0c8a7919986fdac55d1
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508118"
---
# <a name="run-apache-hive-queries-with-apache-hadoop-in-hdinsight-using-rest"></a>Apache Hive-query's uitvoeren met Apache Hadoop in HDInsight met behulp van REST

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Informatie over het gebruik van de REST-API van WebHCat Apache Hive-query's uitvoeren met Apache Hadoop op Azure HDInsight-cluster.

## <a name="prerequisites"></a>Vereisten

* Een Apache Hadoop-cluster in HDInsight. Zie [aan de slag met HDInsight op Linux](./apache-hadoop-linux-tutorial-get-started.md).

* Een REST-client. Dit document wordt [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) op Windows PowerShell en [Curl](https://curl.haxx.se/) op [Bash](https://docs.microsoft.com/windows/wsl/install-win10).

* Als u Bash gebruikt, moet u ook jq, een opdrachtregelprogramma JSON-processor.  Zie [ https://stedolan.github.io/jq/ ](https://stedolan.github.io/jq/).

## <a name="base-uri-for-rest-api"></a>Basis-URI voor de Rest-API

De base id URI (Uniform Resource) voor de REST-API voor HDInsight is `https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME`, waarbij `CLUSTERNAME` is de naam van uw cluster.  Clusternamen in de URI's zijn **hoofdlettergevoelig**.  Terwijl de naam van het cluster in het gedeelte van de FQDN-naam (Fully Qualified Domain Name) van de URI (`CLUSTERNAME.azurehdinsight.net`) is niet hoofdlettergevoelig, andere exemplaren in de URI zijn hoofdlettergevoelig.

## <a name="authentication"></a>Verificatie

Wanneer u cURL of een andere REST-communicatie met WebHCat, moet u de aanvragen verifiëren door te geven van de gebruikersnaam en het wachtwoord voor de beheerder van de HDInsight-cluster. De REST API is beveiligd via [basisverificatie](https://en.wikipedia.org/wiki/Basic_access_authentication). Om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server, moet u aanvragen altijd uitvoeren via HTTP Secure (HTTPS).

### <a name="setup-preserve-credentials"></a>Setup (behouden referenties)
Uw referenties in om te voorkomen dat deze opnieuw invoeren voor elke voorbeeld behouden.  Naam van het cluster worden bewaard in een afzonderlijke stap.

**EEN. Bash**  
Het onderstaande script bewerken door te vervangen `PASSWORD` met uw huidige wachtwoord.  Voer de opdracht.

```bash
export password='PASSWORD'
```  

**B. PowerShell** uitvoeren van de onderstaande code en voer uw referenties in het pop-upvenster:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
```

### <a name="identify-correctly-cased-cluster-name"></a>De naam correct omhuld cluster identificeren
De werkelijke behuizing van de naam van het cluster is mogelijk anders dan u verwacht, afhankelijk van hoe het cluster is gemaakt.  De stappen die hier wordt het werkelijke hoofdlettergebruik weergeven en vervolgens opslaan in een variabele voor alle opeenvolgende voorbeelden.

De scripts hieronder om te vervangen bewerken `CLUSTERNAME` met de clusternaam van uw. Voer de opdracht. (De naam van het cluster voor de FQDN-naam is niet hoofdlettergevoelig).

```bash
export clusterName=$(curl -u admin:$password -sS -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
echo $clusterName
```  

```powershell
# Identify properly cased cluster name
$resp = Invoke-WebRequest -Uri "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" `
    -Credential $creds -UseBasicParsing
$clusterName = (ConvertFrom-Json $resp.Content).items.Clusters.cluster_name;

# Show cluster name
$clusterName
```

## <a id="curl"></a>Een Hive-query uitvoeren

1. Als u wilt controleren of u verbinding kunt maken met uw HDInsight-cluster, gebruikt u een van de volgende opdrachten:

    ```bash
    curl -u admin:$password -G https://$clusterName.azurehdinsight.net/templeton/v1/status
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

1. Het begin van de URL, `https://$CLUSTERNAME.azurehdinsight.net/templeton/v1`, is hetzelfde voor alle aanvragen. Het pad `/status`, geeft aan dat de aanvraag is de status van WebHCat (ook wel bekend als Templeton) voor de server. U kunt ook de versie van Hive vragen met behulp van de volgende opdracht uit:

    ```bash
    curl -u admin:$password -G https://$clusterName.azurehdinsight.net/templeton/v1/version/hive
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/version/hive" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Deze aanvraag retourneert een reactie vergelijkbaar met de volgende tekst:

    ```json
    {"module":"hive","version":"1.2.1000.2.6.5.3008-11"}
    ```

1. Het volgende gebruiken om te maken van een tabel met de naam **log4jLogs**:

    ```bash
    jobid=$(curl -s -u admin:$password -d user.name=admin -d execute="DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/rest" https://$clusterName.azurehdinsight.net/templeton/v1/hive | jq -r .id)
    echo $jobid
    ```

    ```powershell
    $reqParams = @{"user.name"="admin";"execute"="DROP TABLE log4jLogs;CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED BY ' ' STORED AS TEXTFILE LOCATION '/example/data/;SELECT t4 AS sev,COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;";"statusdir"="/example/rest"}
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
   * `SELECT` : Hiermee kunt u een aantal van alle rijen waar kolom **t4** bevat de waarde **[fout]** . Deze instructie retourneert een waarde van **3** als er drie rijen die deze waarde bevatten.

     > [!NOTE]  
     > U ziet dat de adresruimten tussen HiveQL-instructies zijn vervangen door de `+` gebruikt in combinatie met Curl teken. Genoteerde waarden met een spatie, zoals het scheidingsteken, moeten niet worden vervangen door `+`.

      Met deze opdracht retourneert een taak-ID die kan worden gebruikt om de status van de taak te controleren.

1. Om te controleren of de status van de taak, gebruikt u de volgende opdracht:

    ```bash
    curl -u admin:$password -d user.name=admin -G https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobid | jq .status.state
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

1. Zodra de status van de taak is gewijzigd in **geslaagd**, kunt u de resultaten van de taak ophalen uit Azure Blob-opslag. De `statusdir` parameter die is doorgegeven met de query bevat de locatie van het uitvoerbestand; in dit geval `/example/rest`. Dit adres slaat de uitvoer in de `example/curl` map in de standaardopslag voor clusters.

    U kunt de lijst en deze bestanden downloaden met behulp van de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Zie voor meer informatie over het gebruik van de Azure CLI met Azure Storage de [Azure CLI gebruiken met Azure Storage](https://docs.microsoft.com/azure/storage/storage-azure-cli#create-and-manage-blobs) document.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over Hive met HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Apache Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Zie voor meer informatie over de REST-API die wordt gebruikt in dit document, de [WebHCat verwijzing](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) document.
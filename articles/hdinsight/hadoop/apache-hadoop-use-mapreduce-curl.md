---
title: Gebruik MapReduce en Curl met Hadoop in HDInsight - Azure | Microsoft Docs
description: Informatie over het op afstand MapReduce-taken uitvoeren met Hadoop in HDInsight met Curl.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: dd3e5904ee21ee74da5adaa65abd7865a82c8b36
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>MapReduce-taken uitvoeren met Hadoop op HDInsight met behulp van REST

Informatie over het gebruik van de REST-API WebHCat MapReduce-taken uitvoeren op een Hadoop op HDInsight-cluster. CURL wordt gebruikt om u te laten zien hoe u kunt werken met HDInsight met behulp van onbewerkte HTTP-aanvragen voor MapReduce-taken uitvoeren.

> [!NOTE]
> Als u al bekend bent met Hadoop op basis van Linux-servers gebruiken, maar u niet bekend met HDInsight bent, raadpleegt u de [wat u moet weten over Hadoop op basis van Linux in HDInsight](../hdinsight-hadoop-linux-information.md) document.


## <a id="prereq"></a>Vereisten

* Een Hadoop op HDInsight-cluster
* [CURL](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>MapReduce-taken met Curl uitvoeren

> [!NOTE]
> Wanneer u Curl of andere REST-communicatie met WebHCat gebruikt, moet u de aanvragen verifiëren door te geven van de gebruikersnaam van de beheerder voor HDInsight-cluster en het wachtwoord. U moet de clusternaam gebruiken als onderdeel van de URI die wordt gebruikt voor het versturen van aanvragen naar de server.
>
> Voor de opdrachten in deze sectie vervangt u **admin** met de gebruiker om het cluster te verifiëren. Vervang **CLUSTERNAME** door de naam van uw cluster. Geef desgevraagd het wachtwoord voor het gebruikersaccount.
>
> De REST-API is beveiligd met behulp van [basistoegang verificatie](http://en.wikipedia.org/wiki/Basic_access_authentication). U moet aanvragen altijd maken met behulp van HTTPS om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server.


1. Gebruik een opdrachtregel met de volgende opdracht om te controleren of u verbinding met uw HDInsight-cluster kunt maken:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    U ontvangt een reactie vergelijkbaar met de volgende JSON:

        {"status":"ok","version":"v1"}

    In deze opdracht worden de volgende parameters gebruikt:

   * **-u**: geeft aan dat de gebruikersnaam en wachtwoord voor het verifiëren van de aanvraag
   * **-G**: geeft aan dat deze bewerking een GET-aanvraag

   Het begin van de URI **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen.

2. Als u een MapReduce-taak, moet u de volgende opdracht gebruiken:

    ```bash
    curl -u admin -d user.name=admin -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    Het einde van de URI (mapreduce/jar) ziet WebHCat of er een MapReduce-taak op deze aanvraag wordt gestart van een klasse in een jar-bestand. In deze opdracht worden de volgende parameters gebruikt:

   * **-d**: `-G` niet wordt gebruikt, zodat de aanvraag wordt standaard ingesteld op de POST-methode. `-d`Hiermee geeft u de waarden die worden verzonden met de aanvraag.
    * **User.name**: de gebruiker die de opdracht wordt uitgevoerd
    * **JAR**: de locatie van de jar-bestand met de klasse om te worden uitgevoerd
    * **klasse**: de klasse met de MapReduce-logica
    * **arg**: de argumenten worden doorgegeven aan de MapReduce-taak. In dit geval, het bestand invoertekst en de map die worden gebruikt voor de uitvoer

   Met deze opdracht als resultaat moet een taak-ID die kan worden gebruikt om de status van de taak te controleren:

       {"id":"job_1415651640909_0026"}

3. Als u wilt de status van de taak controleren, moet u de volgende opdracht gebruiken:

    ```bash
    curl -G -u admin -d user.name=admin https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Vervang de **JOBID** met de waarde die wordt geretourneerd in de vorige stap. Bijvoorbeeld, als de retourwaarde is `{"id":"job_1415651640909_0026"}`, en vervolgens de JOBID is `job_1415651640909_0026`.

    Als de taak voltooid is, wordt de status geretourneerd is `SUCCEEDED`.

   > [!NOTE]
   > Deze aanvraag Curl retourneert een JSON-document met informatie over de taak. Jq wordt gebruikt voor het ophalen van de waarde van de status.

4. Wanneer de status van de taak is gewijzigd in `SUCCEEDED`, kunt u de resultaten van de taak ophalen uit Azure Blob-opslag. De `statusdir` parameter die wordt doorgegeven aan de query bevat de locatie van het uitvoerbestand. In dit voorbeeld is de locatie `/example/curl`. Dit adres slaat de uitvoer van de taak in de opslag van de standaard clusters op `/example/curl`.

U kunt de lijst en deze bestanden downloaden met behulp van de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Zie voor meer informatie over het werken met blobs met Azure CLI het [met behulp van de Azure CLI 2.0 met Azure Storage](../../storage/common/storage-azure-cli.md#create-and-manage-blobs) document.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce-taken in HDInsight:

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)

Zie voor meer informatie over de REST-interface die wordt gebruikt in dit artikel, de [WebHCat verwijzing](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
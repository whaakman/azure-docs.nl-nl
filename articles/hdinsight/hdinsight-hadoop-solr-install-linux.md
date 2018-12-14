---
title: Gebruik scriptacties Solr installeren op Linux gebaseerde HDInsight - Azure
description: Leer hoe u Solr installeren op Linux gebaseerde HDInsight Hadoop-clusters met behulp van scriptacties.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 3500a29c1cdd8b1997f67a3cf1918090dc4ca812
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53383592"
---
# <a name="install-and-use-apache-solr-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van Apache Solr op HDInsight Hadoop-clusters

Leer hoe u Apache Solr installeren op Azure HDInsight met behulp van scriptacties. Solr is een krachtige zoek-platform en biedt zoekfuncties op bedrijfsniveau op gegevens die worden beheerd door Hadoop.

> [!IMPORTANT]  
> Voor de stappen in dit document hebt u een HDInsight-cluster nodig dat werkt met Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

> [!IMPORTANT]  
> Het voorbeeld van een script gebruikt in dit document wordt Solr 4.9 geïnstalleerd met een specifieke configuratie. Als u configureren van het cluster Solr met verschillende verzamelingen, shards, schema's, replica's, enzovoort wilt, moet u het script en Solr binaire bestanden wijzigen.

## <a name="whatis"></a>Wat is Solr

[Apache Solr](http://lucene.apache.org/solr/features.html) is een platform voor enterprise search waarmee krachtige zoeken in volledige tekst van gegevens. Hoewel Hadoop kunt opslaan en beheren van grote hoeveelheden gegevens, biedt Apache Solr de zoekfuncties voor het snel de gegevens worden opgehaald.

> [!WARNING]   
> Onderdelen van het HDInsight-cluster worden volledig ondersteund door Microsoft.
>
> Aangepaste onderdelen, zoals Solr, commercieel redelijke ondersteuning om het probleem verder oplossen met u te helpen te ontvangen. Microsoft-ondersteuning niet mogelijk problemen met aangepaste onderdelen op te lossen. Het is wellicht voor ondersteuning contact opnemen met de open-source-community's. Er zijn bijvoorbeeld veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Ook Apache-projecten project-sites hebben op [ http://apache.org ](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).

## <a name="what-the-script-does"></a>Wat het script doet

Met dit script maakt de volgende wijzigingen aan het HDInsight-cluster:

* Solr 4.9 in geïnstalleerd `/usr/hdp/current/solr`
* Hiermee maakt u een gebruiker, **solrusr**, dat wordt gebruikt voor het uitvoeren van de service Solr
* Sets **solruser** als eigenaar van `/usr/hdp/current/solr`
* Voegt een [Upstart](http://upstart.ubuntu.com/) configuratie die Solr automatisch wordt gestart.

## <a name="install"></a>Met behulp van scriptacties Solr installeren

Een voorbeeld van een script Solr installeren op een HDInsight-cluster is beschikbaar op de volgende locatie:

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

Voor het maken van een cluster met Solr geïnstalleerd, gebruikt u de stappen in de [maken van HDInsight-clusters](hdinsight-hadoop-create-linux-clusters-portal.md) document. Gebruik de volgende stappen voor het installeren van Solr tijdens het maken:

1. Uit de __samenvatting voor Cluster__ sectie, select__Advanced settings__, klikt u vervolgens __scriptacties__. Gebruik de volgende informatie voor het vullen van het formulier:

   * **NAAM**: Voer een beschrijvende naam voor de scriptactie.
   * **SCRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
   * **HEAD**: Schakel deze optie
   * **WERKNEMER**: Schakel deze optie
   * **ZOOKEEPER**: Schakel deze optie in om te installeren op de Zookeeper-knooppunt
   * **PARAMETERS**: Dit veld leeg laten

2. Aan de onderkant van de **scriptacties** sectie, gebruikt u de **Selecteer** om de configuratie op te slaan. Gebruik tot slot de **volgende** terug te keren naar de __samenvatting voor Cluster__

3. Uit de __samenvatting voor Cluster__ weergeeft, schakelt __maken__ om het cluster te maken.

## <a name="usesolr"></a>Hoe gebruik ik Solr in HDInsight

> [!IMPORTANT]  
> De stappen in deze sectie laten zien basisfunctionaliteit Solr. Zie voor meer informatie over het gebruik van Solr de [Apache Solr site](http://lucene.apache.org/solr/).

### <a name="index-data"></a>Gegevens indexeren

Gebruik de volgende stappen voorbeeldgegevens toevoegen aan Solr, en vervolgens query's uitvoeren:

1. Maak verbinding met het HDInsight-cluster via SSH:

    > [!NOTE]  
    > Vervang `sshuser` met de SSH-gebruiker voor het cluster. Vervang `clustername` met de naam van het cluster.

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

     > [!IMPORTANT]  
     > Een SSH-tunnel stappen verderop in dit document gebruiken voor verbinding met de Solr-webgebruikersinterface. Voor het gebruik van deze stappen, moet u een SSH-tunnel tot stand brengen en configureer vervolgens uw browser om deze te gebruiken.
     >
     > Zie voor meer informatie de [SSH-Tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.

2. Gebruik de volgende opdrachten om voorbeeldgegevens van Solr index:

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    De volgende uitvoer wordt geretourneerd naar de console:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    De `post.jar` hulpprogramma voegt de **solr.xml** en **monitor.xml** documenten naar de index.
  
3. Gebruik de volgende opdracht om te vragen uit de Solr REST-API:

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    Met deze opdracht wordt gezocht **verzameling1** voor alle documenten die overeenkomt met **\*:\*** (gecodeerd als \*% 3A\* in de query-tekenreeks). De volgende JSON-document is een voorbeeld van het antwoord:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }

### <a name="using-the-solr-dashboard"></a>Met behulp van het dashboard Solr

Het dashboard Solr is een webinterface waarmee u werkt met Solr via uw webbrowser. Het dashboard Solr is niet beschikbaar gemaakt rechtstreeks op het Internet van uw HDInsight-cluster. U kunt een SSH-tunnel gebruiken om deze te openen. Zie voor meer informatie over het gebruik van een SSH-tunnel, de [SSH-Tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.

Wanneer u een SSH-tunnel hebt gemaakt, gebruik de volgende stappen uit om het dashboard Solr:

1. Bepaal de naam van de host voor het primaire hoofdknooppunt:

   1. Gebruik SSH om verbinding met het hoofdknooppunt van het cluster te maken. Bijvoorbeeld `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

       Zie voor meer informatie over het gebruik van SSH de [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

   2. Gebruik de volgende opdracht om op te halen van de volledig gekwalificeerde hostnaam:

        ```bash
        hostname -f
        ```

        Met deze opdracht retourneert een waarde die vergelijkbaar is met de naam van de volgende:

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        Sla de waarde die is geretourneerd, zoals het wordt later gebruikt.

2. In uw browser verbinding maken met **http://HOSTNAME:8983/solr/#/**, waarbij **hostnaam** is de naam die u in de vorige stappen hebt bepaald.

    De aanvraag wordt doorgestuurd via de SSH-tunnel naar de web-Solr UI in uw cluster. De pagina wordt weergegeven die vergelijkbaar is met de volgende afbeelding:

    ![Afbeelding van Solr dashboard](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. In het linkerdeelvenster, gebruikt u de **Core Selector** vervolgkeuzelijst te selecteren **verzameling1**. Meerdere vermeldingen moeten ze worden weergegeven onder **verzameling1**.

4. Uit de onderstaande invoer **verzameling1**, selecteer **Query**. De volgende waarden gebruiken voor het vullen van de pagina voor zoeken:

   * In de **q** tekst Voer  **\*:**\*. Deze query retourneert alle documenten die zijn geïndexeerd in Solr. Als u zoeken naar een specifieke tekenreeks in de documenten wilt, kunt u deze tekenreeks hier invoeren.
   * In de **wt** tekst Selecteer indeling van de uitvoer. De standaardwaarde is **json**.

     Selecteer ten slotte de **Query uitvoeren** knop aan de onderkant van de plak zoeken.

     ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     De uitvoer geeft als resultaat van de twee documenten die u eerder naar de index hebt toegevoegd. De uitvoer is vergelijkbaar met de volgende JSON-document:

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, the Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication to other Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over the e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }

### <a name="starting-and-stopping-solr"></a>Starten en stoppen van Solr

Gebruik de volgende opdrachten om handmatig te stoppen en starten Solr:

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a>Geïndexeerde gegevens back-up

Gebruik de volgende stappen voor het back-ups Solr naar de standaardopslag voor uw cluster:

1. Verbinding maken met het cluster via SSH en vervolgens de volgende opdracht gebruiken om op te halen van de hostnaam voor het hoofdknooppunt:

    ```bash
    hostname -f
    ```

2. Gebruik de volgende opdracht om een momentopname van de geïndexeerde gegevens te maken. Vervang **hostnaam** met de naam van de vorige opdracht is geretourneerd:

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    Het antwoord is vergelijkbaar met het volgende XML-bestand:

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. Wijzig de mappen te `/usr/hdp/current/solr/example/solr`. Er is een submap voor elke verzameling. Elke verzameling-map bevat een `data` map met de momentopname voor de verzameling.

4. Gebruik de volgende opdracht voor het maken van een gecomprimeerd archief van de momentopnamemap:

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    Vervang de `snapshot.20150806185338855` waarden met de naam van de momentopname voor de verzameling.

    Deze opdracht maakt u een archief met de naam **snapshot.20150806185338855.tgz**, waarin de inhoud van de **snapshot.20150806185338855** directory.

5. Vervolgens slaat u het archief op de primaire opslag van het cluster met behulp van de volgende opdracht uit:

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

Zie voor meer informatie over het werken met Apache Solr back-up en herstelbewerkingen [ https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups ](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).

## <a name="next-steps"></a>Volgende stappen

* [Apache Giraph installeren op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md). Aanpassing van de cluster Giraph installeren op HDInsight Hadoop-clusters gebruiken. Giraph Hiermee kunt u grafische verwerking met behulp van Hadoop en kan worden gebruikt met Azure HDInsight.

* [Hue installeren op HDInsight-clusters](hdinsight-hadoop-hue-linux.md). Aanpassing van de cluster Hue installeren op HDInsight Hadoop-clusters gebruiken. HUE is een set van webtoepassingen die worden gebruikt om te communiceren met een Hadoop-cluster.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md

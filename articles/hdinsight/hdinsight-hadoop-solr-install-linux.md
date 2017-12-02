---
title: Gebruik scriptactie Solr installeren op Linux gebaseerde HDInsight - Azure | Microsoft Docs
description: Informatie over het installeren van Solr op Linux gebaseerde HDInsight Hadoop-clusters met behulp van scriptacties.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cc93ed5c-a358-456a-91a4-f179185c0e98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: larryfr
ms.openlocfilehash: edee4fbb37744a8ef93c6a0b7e60b0790d1af9c4
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van Solr op HDInsight Hadoop-clusters

Informatie over het installeren van Solr in Azure HDInsight met behulp van de scriptactie. Solr is een krachtige search-platform en biedt zoekmogelijkheden op bedrijfsniveau op gegevens die worden beheerd door Hadoop.

> [!IMPORTANT]
    > De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

> [!IMPORTANT]
> Het voorbeeldscript in dit document gebruikt installeert Solr 4.9 met een specifieke configuratie. Als u wilt configureren van het cluster Solr met verschillende verzamelingen, shards, schema's, replica's, enz., moet u het script en Solr binaire bestanden wijzigen.

## <a name="whatis"></a>Wat is Solr

[Apache Solr](http://lucene.apache.org/solr/features.html) is een platform voor het zoeken van enterprise waarmee krachtige zoekopdracht in volledige tekst van gegevens. Hadoop kunt opslaan en beheren van de enorme hoeveelheden gegevens, biedt Apache Solr de zoekmogelijkheden snel gegevens ophalen.

> [!WARNING]
> Onderdelen van het HDInsight-cluster worden volledig ondersteund door Microsoft.
>
> Aangepaste onderdelen, zoals Solr, ontvangt binnen commercieel redelijke ondersteuning u helpen het probleem verder op te lossen. Ondersteuning van Microsoft zijn mogelijk niet kunnen oplossen van problemen met aangepaste onderdelen. U moet mogelijk de community's van de open-source benaderen voor hulp. Bijvoorbeeld: Er zijn veel community-sites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Ook hebben Apache projecten project-sites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).

## <a name="what-the-script-does"></a>Wat het script doet

Dit script maakt de volgende wijzigingen aan het HDInsight-cluster:

* Solr 4.9 in geïnstalleerd`/usr/hdp/current/solr`
* Hiermee maakt u een gebruiker **solrusr**, dat wordt gebruikt voor het uitvoeren van de service Solr
* Sets **solruser** als eigenaar van`/usr/hdp/current/solr`
* Voegt een [Upstart](http://upstart.ubuntu.com/) configuratie die Solr automatisch wordt gestart.

## <a name="install"></a>Installeren met behulp van scriptacties Solr

Een voorbeeld van een script Solr installeren op een HDInsight-cluster is beschikbaar op de volgende locatie:

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

Voor het maken van een cluster met Solr geïnstalleerd gebruikt u de stappen in de [HDInsight-clusters maken](hdinsight-hadoop-create-linux-clusters-portal.md) document. Gebruik de volgende stappen voor het installeren van Solr tijdens het maken:

1. Van de __Cluster samenvatting__ sectie, select__Advanced settings__, klikt u vervolgens __acties Script__. Gebruik de volgende informatie voor het vullen van het formulier:

   * **NAAM**: een beschrijvende naam voor de scriptactie.
   * **SCRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
   * **HEAD**: Schakel deze optie
   * **WERKNEMER**: Schakel deze optie
   * **ZOOKEEPER**: Schakel deze optie om te installeren op het knooppunt Zookeeper
   * **PARAMETERS**: dit veld leeg laten

2. Aan de onderkant van de **acties Script** sectie, gebruikt u de **Selecteer** om op te slaan van de configuratie. Gebruik tot slot de **volgende** terug te keren naar de __Cluster samenvatting__

3. Van de __Cluster samenvatting__ pagina __maken__ om het cluster te maken.

## <a name="usesolr"></a>Hoe gebruik Solr in HDInsight

> [!IMPORTANT]
> De stappen in deze sectie laten zien basisfunctionaliteit Solr. Zie voor meer informatie over het gebruik van Solr de [Apache Solr site](http://lucene.apache.org/solr/).

### <a name="index-data"></a>Indexgegevens

Gebruik de volgende stappen bijvoorbeeld gegevens toevoegen aan Solr en deze vervolgens een query:

1. Maak verbinding met het HDInsight-cluster via SSH:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

     > [!IMPORTANT]
     > Stappen verderop in dit document wordt een SSL-tunnel verbinding maken met de webgebruikersinterface Solr gebruikt. Voor het gebruik van deze stappen, moet u een SSL-tunnel maakt en configureer vervolgens uw browser om het te gebruiken.
     >
     > Zie voor meer informatie de [SSH-Tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.

2. Gebruik de volgende opdrachten om Solr index voorbeeldgegevens:

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
  
3. Gebruik de volgende opdracht om op te vragen van de Solr REST-API:

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    Met deze opdracht wordt gezocht **collection1** voor documenten die overeenkomen met  **\*:\***  (gecodeerd als \*% 3A\* in de query-tekenreeks). Het volgende JSON-document is een voorbeeld van het antwoord:

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

Het dashboard Solr is een webgebruikersinterface waarmee u werkt met Solr via uw webbrowser. Het dashboard Solr rechtstreeks op het Internet van uw HDInsight-cluster niet wordt weergegeven. U kunt een SSH-tunnel gebruiken om deze te openen. Zie voor meer informatie over het gebruik van een SSH-tunnel de [SSH-Tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.

Wanneer u een SSH-tunnel hebt gemaakt, gebruik de volgende stappen uit om het dashboard Solr:

1. De hostnaam voor de primaire headnode bepalen:

   1. SSH gebruiken voor verbinding met het hoofdknooppunt van het cluster. Bijvoorbeeld `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

       Zie voor meer informatie over het gebruik van SSH, het [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

   2. Gebruik de volgende opdracht om de volledig gekwalificeerde hostnaam:

        ```bash
        hostname -f
        ```

        Met deze opdracht retourneert een waarde die vergelijkbaar is met de naam van de volgende:

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        De waarde die is geretourneerd, niet opslaan omdat het wordt later gebruikt.

2. Verbinding maken met in uw browser **solr-http://HOSTNAME:8983 / #/**, waarbij **hostnaam** is de naam die u in de vorige stappen hebt bepaald.

    De aanvraag wordt doorgestuurd via de SSH-tunnel naar de Solr webgebruikersinterface op uw cluster. De pagina lijkt op de volgende afbeelding:

    ![Afbeelding van Solr dashboard](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. Vanuit het linkerdeelvenster met de **Core Selector** vervolgkeuzelijst selecteren **collection1**. Meerdere vermeldingen moeten ze worden weergegeven onder **collection1**.

4. De onderstaande vermeldingen **collection1**, selecteer **Query**. Gebruik de volgende waarden voor het vullen van de zoekpagina:

   * In de **q** tekst Voer  **\*:**\*. Deze query retourneert de documenten die zijn geïndexeerd in Solr. Als u zoeken naar een specifieke tekenreeks binnen de documenten wilt, kunt u deze tekenreeks hier invoeren.
   * In de **wt** tekst Selecteer indeling van de uitvoer. Standaard is **json**.

     Tot slot selecteert u de **zoekopdracht uitvoeren** knop onder aan de pate zoeken.

     ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     De uitvoer retourneert twee documenten die u eerder naar de index hebt toegevoegd. De uitvoer is vergelijkbaar met het volgende JSON-document:

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

### <a name="starting-and-stopping-solr"></a>Starten en stoppen Solr

Gebruik de volgende opdrachten om handmatig te stoppen en starten Solr:

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a>Back-upgegevens van de geïndexeerde

Gebruik de volgende stappen uit voor de back-ups Solr naar de standaard-opslag voor uw cluster:

1. Verbinding maken met het cluster via SSH en voer vervolgens de volgende opdracht gebruiken om op te halen van de hostnaam voor het hoofdknooppunt:

    ```bash
    hostname -f
    ```

2. Gebruik de volgende opdracht om een momentopname van de geïndexeerde gegevens te maken. Vervang **hostnaam** met de naam die is geretourneerd door de vorige opdracht:

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    Het antwoord is vergelijkbaar met de volgende XML-code:

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. Wijzig de mappen te `/usr/hdp/current/solr/example/solr`. Er is een submap voor elke verzameling. De map voor elke verzameling bevat een `data` map waarin de momentopname voor de verzameling.

4. Gebruik de volgende opdracht voor het maken van een gecomprimeerd archief van de momentopnamemap:

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    Vervang de `snapshot.20150806185338855` waarden met de naam van de momentopname voor uw verzameling.

    Deze opdracht maakt u een archief met de naam **snapshot.20150806185338855.tgz**, waarin de inhoud van de **snapshot.20150806185338855** directory.

5. Vervolgens kunt u opslaan van het archief met van het cluster primaire opslag met de volgende opdracht:

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

Zie voor meer informatie over het werken met Solr back-up en herstelbewerkingen [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).

## <a name="next-steps"></a>Volgende stappen

* [Giraph installeren op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md). Aanpassing van de cluster Giraph installeren op HDInsight Hadoop-clusters gebruiken. Giraph Hiermee kunt u grafiek verwerken met behulp van Hadoop en kan worden gebruikt met Azure HDInsight.

* [Hue installeren op HDInsight-clusters](hdinsight-hadoop-hue-linux.md). Aanpassing van de cluster Hue installeren op HDInsight Hadoop-clusters gebruiken. HUE is een set van webtoepassingen die worden gebruikt om te communiceren met een Hadoop-cluster.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md

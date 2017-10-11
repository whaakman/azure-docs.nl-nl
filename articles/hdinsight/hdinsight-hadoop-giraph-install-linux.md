---
title: Installeren en gebruiken van Giraph op HDInsight (Hadoop) - Azure | Microsoft Docs
description: Informatie over het installeren van Giraph op Linux gebaseerde HDInsight-clusters met behulp van scriptacties. Scriptacties kunnen u het cluster tijdens het maken, aanpassen door het wijzigen van de configuratie van het cluster of het installeren van hulpprogramma's en services.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 6e2f6983e00f874420f7f0907dbc68185f0af713
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Giraph installeren op HDInsight Hadoop-clusters en Giraph gebruiken voor het verwerken van grootschalige grafieken

Informatie over het installeren van Apache Giraph op een HDInsight-cluster. Het script actie-onderdeel van HDInsight kunt u voor het aanpassen van het cluster een bash-script uit te voeren. Scripts kunnen worden gebruikt voor het aanpassen van clusters tijdens en na het maken van het cluster.

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="whatis"></a>Wat is Giraph

[Apache Giraph](http://giraph.apache.org/) Hiermee kunt u grafiek verwerken met behulp van Hadoop en kan worden gebruikt met Azure HDInsight. Grafieken model relaties tussen objecten. De verbindingen tussen routers op een grote netwerk, zoals Internet of relaties tussen personen op sociale netwerken. Grafiek verwerking kunt u tot reden van de relaties tussen de objecten in een grafiek, zoals:

* Identificeren van mogelijke vrienden op basis van uw huidige relaties.

* Identificeren van de kortste afstand tussen twee computers in een netwerk.

* Berekenen van de positie van de pagina van webpagina's.

> [!WARNING]
> Onderdelen van het HDInsight-cluster worden volledig ondersteund: Microsoft Support helpt te isoleren en het oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen, zoals Giraph, ontvangt binnen commercieel redelijke ondersteuning u helpen het probleem verder op te lossen. Microsoft Support kunt u mogelijk het oplossen van het probleem. Als dat niet het geval is, moet u contact opnemen met open-source community's waar grondige kennis van deze technologie kan worden gevonden. Bijvoorbeeld: Er zijn veel community-sites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Ook hebben Apache projecten project-sites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).


## <a name="what-the-script-does"></a>Wat het script doet

Dit script voert de volgende handelingen uit:

* Giraph om te worden geïnstalleerd`/usr/hdp/current/giraph`

* Kopieert de `giraph-examples.jar` bestand standaard storage (WASB) voor uw cluster:`/example/jars/giraph-examples.jar`

## <a name="install"></a>Giraph met behulp van scriptacties installeren

Een voorbeeld van een script Giraph installeren op een HDInsight-cluster is beschikbaar op de volgende locatie:

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Deze sectie geeft instructies voor het gebruik van het voorbeeldscript tijdens het maken van het cluster met behulp van de Azure-portal.

> [!NOTE]
> Een scriptactie kan worden toegepast met behulp van een van de volgende methoden:
> * Azure PowerShell
> * De Azure CLI
> * De HDInsight-SDK voor .NET
> * Azure Resource Manager-sjablonen
> 
> U kunt ook scriptacties toepassen op clusters al wordt uitgevoerd. Zie voor meer informatie [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

1. Beginnen met het maken van een cluster met behulp van de stappen in [maken Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-create-linux-clusters-portal.md), maar niet maken uitvoert.

2. Op de **optionele configuratie** blade Selecteer **scriptacties**, en geef de volgende informatie:

   * **NAAM**: een beschrijvende naam voor de scriptactie.

   * **SCRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **HEAD**: deze vermelding controleren

   * **WERKNEMER**: laat u dit item niet ingeschakeld

   * **ZOOKEEPER**: laat u dit item niet ingeschakeld

   * **PARAMETERS**: dit veld leeg laten

3. Aan de onderkant van de **scriptacties**, gebruiken de **Selecteer** om op te slaan van de configuratie. Gebruik tot slot de **Selecteer** knop aan de onderkant van de **optionele configuratie** blade informatie optionele configuratie wilt opslaan.

4. Doorgaan met het maken van het cluster, zoals beschreven in [maken Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Hoe gebruik Giraph in HDInsight?

Zodra het cluster is gemaakt, gebruikt u de volgende stappen uit om uit te voeren in het voorbeeld SimpleShortestPathsComputation is opgenomen in Giraph. In dit voorbeeld wordt het basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) implementatie voor het vinden van het kortste pad tussen de objecten in een grafiek.

1. Maak verbinding met het HDInsight-cluster via SSH:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Gebruik de volgende opdracht voor het maken van een bestand met de naam **tiny_graph.txt**:

    ```bash
    nano tiny_graph.txt
    ```

    Gebruik de volgende tekst als de inhoud van dit bestand:

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    Deze gegevens worden beschreven van een relatie tussen de objecten in een gerichte grafiek, via de indeling `[source_id, source_value,[[dest_id], [edge_value],...]]`. Elke regel vertegenwoordigt een relatie tussen een `source_id` object en een of meer `dest_id` objecten. De `edge_value` kunnen worden beschouwd als de sterkte of de afstand van de verbinding tussen `source_id` en `dest\_id`.

    Getekend, en met de waarde (gewicht) als de afstand tussen de objecten, de gegevens kan eruitzien als in het volgende diagram:

    ![tiny_graph.txt getekend als cirkels met verschillende afstand tussen regels](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. Gebruiken om het bestand opslaat, **Ctrl + X**, klikt u vervolgens **Y**, en tot slot **Enter** te accepteren van de bestandsnaam.

4. Gebruik de volgende gegevens opslaan in de primaire opslag voor uw HDInsight-cluster:

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. Voer het SimpleShortestPathsComputation voorbeeld met de volgende opdracht:

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    De parameters voor deze opdracht worden beschreven in de volgende tabel:

   | Parameter | Wat het doet |
   | --- | --- |
   | `jar` |Het jar-bestand met de voorbeelden. |
   | `org.apache.giraph.GiraphRunner` |De klasse die wordt gebruikt voor het starten van de voorbeelden. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |Het voorbeeld dat wordt gebruikt. In dit voorbeeld wordt het kortste pad tussen de 1-ID en alle andere id's in de grafiek berekend. |
   | `-ca mapred.job.tracker` |De headnode voor het cluster. |
   | `-vif` |De invoerindeling voor de invoergegevens. |
   | `-vip` |Het bestand invoergegevens. |
   | `-vof` |De indeling van de uitvoer. In dit voorbeeld-ID en -waarde als tekst zonder opmaak. |
   | `-op` |De uitvoerlocatie. |
   | `-w 2` |Het aantal werknemers kunnen gebruiken. In dit voorbeeld 2. |

    Zie voor meer informatie over deze en andere parameters gebruikt met Giraph voorbeelden de [Giraph Quick Start](http://giraph.apache.org/quick_start.html).

6. Zodra de taak is voltooid, de resultaten worden opgeslagen in de **/example/out/shotestpaths** directory. De naam van het uitvoerbestand beginnen met **onderdeel-m -** en moet eindigen met een getal dat aangeeft van de eerste, tweede, enzovoort-bestand. Gebruik de volgende opdracht om de uitvoer weer te geven:

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    De uitvoer ziet er ongeveer de volgende tekst:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    De SimpleShortestPathComputation voorbeeld is harde code vastgelegd beginnen met object-ID 1 en de kortste route naar andere objecten. De uitvoer is in de indeling van `destination_id` en `distance`. De `distance` is de waarde (of het gewicht) van de randen afgelegd tussen object-ID 1 en de doel-ID.

    Deze gegevens te visualiseren, kunt u de resultaten controleren door de kortste paden onderweg tussen ID 1 en alle andere objecten. Het kortste pad tussen 1-ID en -ID 4 is 5. Deze waarde is de totale afstand tussen <span style="color:orange">ID 1 en 3</span>, en vervolgens <span style="color:red">ID 3 en 4</span>.

    ![Tekenen van objecten als cirkels met de kortste paden tussen getekend](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>Volgende stappen

* [Installeren en gebruiken van Hue op HDInsight-clusters](hdinsight-hadoop-hue-linux.md).

* [Solr installeren op HDInsight-clusters](hdinsight-hadoop-solr-install-linux.md).

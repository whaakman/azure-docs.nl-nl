---
title: Giraph installeren en gebruiken op HDInsight (Hadoop) - Azure
description: Leer hoe u Giraph installeren op Linux gebaseerde HDInsight-clusters met behulp van scriptacties. Scriptacties kunnen u het cluster tijdens het maken, aanpassen door het wijzigen van de configuratie van het cluster of het installeren van hulpprogramma's en services.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: a2f964915efda6ce83439c3c1970de58b0467456
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58201718"
---
# <a name="install-apache-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Apache Giraph installeren op HDInsight Hadoop-clusters en Giraph gebruiken om grootschalige grafieken te verwerken

Leer hoe u Apache Giraph installeren op een HDInsight-cluster. Het script action-functie van HDInsight kunt u het aanpassen van uw cluster een bash-script uit te voeren. Scripts kunnen worden gebruikt voor het aanpassen van clusters tijdens en na het maken van clusters.

> [!IMPORTANT]  
> Voor de stappen in dit document hebt u een HDInsight-cluster nodig dat werkt met Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="whatis"></a>Wat is Giraph

[Apache Giraph](https://giraph.apache.org/) Hiermee kunt u grafische verwerking met behulp van Hadoop en kan worden gebruikt met Azure HDInsight. Grafieken model relaties tussen objecten. Bijvoorbeeld, de verbindingen tussen routers in een groot netwerk zoals Internet, of de relaties tussen personen op sociale netwerken. Grafische verwerking kunt u te beredeneren de relaties tussen de objecten in een grafiek, zoals:

* Identificeren van mogelijke vrienden op basis van uw huidige relaties.

* Hoe identificeert u de kortste route tussen twee computers in een netwerk.

* Berekening van de positie van de pagina van webpagina's.

> [!WARNING]  
> Onderdelen van het HDInsight-cluster volledig worden ondersteund: Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen, zoals Giraph, commercieel redelijke ondersteuning om het probleem verder oplossen met u te helpen te ontvangen. Het is mogelijk dat Microsoft Support kunnen de problemen kunt oplossen. Als dat niet het geval is, moet u contact opnemen met open-source-community's waar de uitgebreide expertise voor deze technologie kan worden gevonden. Er zijn bijvoorbeeld veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ https://stackoverflow.com ](https://stackoverflow.com). Ook Apache-projecten project-sites hebben op [ https://apache.org ](https://apache.org), bijvoorbeeld: [Hadoop](https://hadoop.apache.org/).


## <a name="what-the-script-does"></a>Wat het script doet

Met dit script worden de volgende acties uitgevoerd:

* Giraph om te worden geïnstalleerd `/usr/hdp/current/giraph`

* Kopieert de `giraph-examples.jar` bestand standaard storage (WASB) voor uw cluster: `/example/jars/giraph-examples.jar`

## <a name="install"></a>Met behulp van scriptacties Giraph installeren

Een voorbeeld van een script Giraph installeren op een HDInsight-cluster is beschikbaar op de volgende locatie:

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Deze sectie vindt u instructies over het gebruik van het voorbeeldscript tijdens het maken van het cluster met behulp van de Azure-portal.

> [!NOTE]  
> Een scriptactie kan worden toegepast met behulp van een van de volgende methoden:
> * Azure PowerShell
> * De klassieke Azure-CLI
> * De HDInsight .NET SDK
> * Azure Resource Manager-sjablonen
> 
> U kunt ook scriptacties toepassen op clusters al wordt uitgevoerd. Zie voor meer informatie, [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

1. Beginnen met het maken van een cluster met behulp van de stappen in [maken Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-create-linux-clusters-portal.md), maar maken ze niet worden voltooid.

2. In de **optionele configuratie** sectie, selecteer **scriptacties**, en geef de volgende informatie:

   * **NAAM**: Voer een beschrijvende naam voor de scriptactie.

   * **SCRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **HEAD**: Controleer deze vermelding.

   * **WERKNEMER**: Laat u dit item is niet ingeschakeld.

   * **ZOOKEEPER**: Laat u dit item is niet ingeschakeld.

   * **PARAMETERS**: Laat dit veld leeg.

3. Aan de onderkant van de **scriptacties**, gebruikt u de **selecteren** om de configuratie op te slaan. Gebruik tot slot de **Selecteer** knop aan de onderkant van de **optionele configuratie** sectie optionele configuratie-informatie wilt opslaan.

4. Doorgaan met het maken van het cluster, zoals beschreven in [maken Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Hoe gebruik ik Giraph in HDInsight?

Zodra het cluster is gemaakt, gebruikt u de volgende stappen uit om uit te voeren in het voorbeeld SimpleShortestPathsComputation is opgenomen met Giraph. In dit voorbeeld wordt de basis [Pregel](https://people.apache.org/~edwardyoon/documents/pregel.pdf) implementatie voor het zoeken naar het kortste pad tussen objecten in een grafiek.

1. Maak verbinding met het HDInsight-cluster via SSH:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Gebruik de volgende opdracht om te maken van een bestand met de naam **tiny_graph.txt**:

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

    Deze gegevens beschrijft de relatie tussen de objecten in een gerichte graaf met behulp van de indeling `[source_id, source_value,[[dest_id], [edge_value],...]]`. Elke regel vertegenwoordigt een relatie tussen een `source_id` object en een of meer `dest_id` objecten. De `edge_value` kan worden beschouwd als de sterkte of de afstand van de verbinding tussen `source_id` en `dest\_id`.

    Getekend, en u de waarde (of gewicht) als de afstand tussen de objecten, kan de gegevens eruitzien als in het volgende diagram:

    ![tiny_graph.txt getekend als cirkels met verschillende afstand tussen regels](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. Gebruiken om het bestand hebt opgeslagen, **Ctrl + X**, klikt u vervolgens **Y**, en tot slot **Enter** te accepteren van de bestandsnaam.

4. Gebruik de volgende gegevens worden opgeslagen in de primaire opslag voor uw HDInsight-cluster:

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. De SimpleShortestPathsComputation-voorbeeld met de volgende opdracht uitvoeren:

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    De parameters gebruikt met de volgende opdracht worden in de volgende tabel beschreven:

   | Parameter | Wat het doet |
   | --- | --- |
   | `jar` |Het jar-bestand met de voorbeelden. |
   | `org.apache.giraph.GiraphRunner` |De klasse die wordt gebruikt voor het starten van de voorbeelden. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |Het voorbeeld dat wordt gebruikt. In dit voorbeeld berekent het kortste pad tussen 1-ID en alle andere id's in de grafiek. |
   | `-ca mapred.job.tracker` |Het hoofdknooppunt van het cluster. |
   | `-vif` |De ingevoerde indeling moet worden gebruikt voor de invoergegevens. |
   | `-vip` |Het bestand invoergegevens. |
   | `-vof` |De indeling van de uitvoer. In dit voorbeeld, -ID en -waarde als tekst zonder opmaak. |
   | `-op` |De locatie van de uitvoer. |
   | `-w 2` |Het aantal werknemers te gebruiken. In dit voorbeeld 2. |

    Zie voor meer informatie over deze en andere parameters gebruikt met Giraph voorbeelden de [Giraph snelstartgids](https://giraph.apache.org/quick_start.html).

6. Nadat de taak is voltooid, de resultaten worden opgeslagen de **/example/out/shotestpaths** directory. De namen van de uitvoer beginnen met **onderdeel-m -** en moet eindigen met een getal dat aangeeft van de eerste, tweede, enzovoort-bestand. Gebruik de volgende opdracht om de uitvoer weer te geven:

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    De uitvoer wordt weergegeven die vergelijkbaar is met de volgende tekst:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Het voorbeeld is moeilijk te beginnen met gecodeerd SimpleShortestPathComputation object-ID 1 en de kortste route op andere objecten. De uitvoer is in de indeling van `destination_id` en `distance`. De `distance` is de waarde (of het gewicht) van de randen gereisd tussen object-ID 1 en de doel-ID.

    Deze gegevens te visualiseren, kunt u de resultaten controleren door het kortste pad tussen 1-ID en alle andere objecten onderweg. Het kortste pad tussen 1-ID en -ID 4 is 5. Deze waarde is de totale afstand tussen de <span style="color:orange">ID 1 en 3</span>, en vervolgens <span style="color:red">ID 3 en 4</span>.

    ![Tekenen van objecten als cirkels met kortste paden tussen getekend](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>Volgende stappen

* [Installeren en Hue gebruiken op HDInsight-clusters](hdinsight-hadoop-hue-linux.md).

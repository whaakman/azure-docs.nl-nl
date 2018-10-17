---
title: Overzicht van HDInsight 4.0 (preview) - Azure
description: HDInsight 3.6 vergelijken met functies, beperkingen en aanbevelingen voor upgrades van HDInsight 4.0.
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: overview
ms.date: 10/04/2018
ms.openlocfilehash: ade162d0261b765336cbff9ea8a6429f9bd2d871
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801815"
---
# <a name="hdinsight-40-overview-preview"></a>Overzicht van HDInsight 4.0 (preview)

Azure HDInsight is onder zakelijke klanten een van de meest populaire services voor open source Hadoop- en Spark-analyses in Azure. HDInsight (HDI) 4.0 is een cloud-distributie van de Hadoop-onderdelen van het [Hortonworks Data Platform (HDP) 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html). Dit artikel bevat informatie over de meest recente Azure HDInsight-release en over hoe u moet upgraden.

## <a name="whats-new-in-hdi-40"></a>Wat is er nieuw in HDI 4.0?

### <a name="hive-30-and-llap"></a>Hive 3.0 en LLAP

Analytische verwerking met lage latentie (LLAP) in Hive gebruikt permanente query-servers en caching in geheugen om snel resultaten van een SQL-query op gegevens in een externe cloudopslag op te geven. LLAP in Hive maakt gebruikt van een set permanente daemons die fragmenten van Hive-query's uitvoeren. Query's uitvoeren in LLAP is vergelijkbaar met Hive zonder LLAP, maar werkroltaken worden in LLAP-daemons uitgevoerd in plaats van in containers.

Voordelen van LLAP in Hive zijn:

* De mogelijkheid om uitgebreide SQL-analyses uit te voeren, bijvoorbeeld complexe samenvoegingen, subquery's, vensterfuncties, sorteren, door de gebruiker gedefinieerde functies en complexe aggregaties, zonder verlies van de prestaties en schaalbaarheid.

* Interactieve query's op gegevens in dezelfde opslag waar de gegevens worden voorbereid, zodat u voor analytische verwerking geen gegevens hoeft te verplaatsen van een opslag naar een andere engine.

* De queryresultaten kunnen in de cache worden opgeslagen, zodat eerder berekende queryresultaten opnieuw kunnen worden gebruikt, wat tijd en middelen bespaart die anders zouden worden gebruikt om voor de query vereiste clustertaken uit te voeren.

### <a name="hive-dynamic-materialized-views"></a>Dynamisch gerealiseerde weergaven in Hive

Hive ondersteunt nu dynamisch gerealiseerde weergaven of een berekening vooraf van relevante samenvattingen die worden gebruikt om de queryverwerking in datawarehouses te versnellen. Gerealiseerde weergaven worden in Hive op het systeem bewaard en gebruiken naadloos LLAP-versnelling.

### <a name="hive-transactional-tables"></a>Transactionele Hive-tabellen

HDI 4.0 bevat Apache Hive 3, dat ACID-naleving (atoomvorm, consistentie, isolatie en duurzaamheid) vereist voor transactionele tabellen die in het Hive-warehouse worden bewaard. ACID-compatibele tabellen en tabelgegevens worden via Hive geopend en beheerd. Gegevens in CRUD-tabellen (maken, ophalen, bijwerken en verwijderen) moeten de ORC-bestandsindeling (geoptimaliseerde rijkolom) hebben, maar tabellen waarin alleen gegevens worden ingevoerd, ondersteunen alle bestandsindelingen.

* ACID v2 heeft prestatieverbeteringen in zowel het opslagformaat als de engine voor uitvoerbewerkingen. 

* ACID is standaard ingeschakeld voor ondersteuning voor alle gegevensupdates.

* Met verbeterde ACID-mogelijkheden kunt u op rijniveau bijwerken en verwijderen.

* Er is geen prestatieoverhead.

* Er is geen bucketing vereist.

* Spark kan naar ACID-tabellen in Hive lezen en schrijven via Hive Warehouse Connector.

Meer informatie over [Apache Hive 3](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/hive-overview/content/hive_whats_new_in_this_release_hive.html).

### <a name="apache-spark"></a>Apache Spark

Apache Spark ontvang bij te werken tabellen en ACID-transacties via de Hive Warehouse Connector. Met Hive Warehouse Connector registreert u transactionele Hive-tabellen als externe tabellen in Spark voor volledige transactionele functionaliteit. Eerdere versies ondersteunden alleen manipulatie van partitietabellen. Hive Warehouse Connector ondersteunt ook Streaming DataFrames om vanuit Spark lees- en schrijfbewerkingen te streamen naar transactionele en streaming Hive-tabellen.

Spark-executors maken rechtstreeks verbinding met LLAP-daemons van Hive om gegevens op een transactionele manier op te halen en bij te werken, waardoor Hive de controle houdt over de gegevens.

Apache Spark in HDInsight 4.0 ondersteunt de volgende scenario's:

* Training voor machine learning-modellen uitvoeren voor hetzelfde transactionele tabel dat wordt gebruikt voor rapportage.
* ACID-transacties gebruiken om kolommen van Spark ML veilig toe te voegen aan een Hive-tabel.
* Een Spark-streamingtaak uitvoeren in de wijzigingsfeed van een streaming Hive-tabel.
* ORC-bestanden rechtstreeks vanuit een Spark Structured Streaming-taak maken.

U hoeft u geen zorgen te maken dat u per ongeluk rechtstreeks vanuit Spark transactionele Hive-tabellen opent, wat zou leiden tot inconsistente resultaten, dubbele gegevens of de beschadiging van gegevens. In HDI 4.0 worden Spark- en Hive-tabellen bewaard in afzonderlijke Metastores. Gebruik de Hive Data Warehouse Connector om transactionele Hive-tabellen expliciet te registreren als externe Spark-tabellen.

Meer informatie over [Apache Spark](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/spark-overview/content/analyzing_data_with_apache_spark.html).


### <a name="oozie"></a>Oozie

Apache Oozie 4.3.1 is opgenomen in HDI 4.0 met de volgende wijzigingen:

* Oozie voert Hive-acties niet langer uit. De Hive CLI is verwijderd en vervangen door BeeLine.

* U sluit ongewenste afhankelijkheden uit van ShareLib door een uitsluitpatroon op te nemen in het bestand **job.properties**.

Meer informatie over [Apache Oozie](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/patch_oozie.html).

## <a name="how-to-upgrade-to-hdi-40"></a>Upgraden naar HDI 4.0

Zoals bij elke grote release is het belangrijk om uw onderdelen uitgebreid te testen voordat u de nieuwste versie in een productieomgeving implementeert. HDI 4.0 is beschikbaar om het upgradeproces mee te beginnen, maar HDI 3.6 is de standaardoptie om onbedoelde ongelukken te voorkomen.

Er is geen ondersteund upgradepad van eerdere versies van HDI naar HDI 4.0. HDI 4.0 is niet compatibel met eerdere versies, omdat de indelingen van Metastore en blobgegevens zijn veranderd. Het is belangrijk dat u uw nieuwe HDI 4.0-omgeving gescheiden houdt van uw huidige productieomgeving. Als u HDI 4.0 in uw huidige omgeving implementeert, wordt uw Metastore bijgewerkt en dat kan niet ongedaan worden gemaakt.  

## <a name="limitations"></a>Beperkingen

* HDI 4.0 ondersteunt MapReduce niet. Gebruik in plaats daarvan Tez. Meer informatie over [Apache Tez](https://tez.apache.org/).

* Hive View is niet langer beschikbaar in HDI 4.0. 

* Shell-interpreter in Apache Zeppelin wordt niet ondersteund in Spark- en Interactive Query-clusters.

* U kunt LLAP in een Apache Spark-LLAP-cluster niet *uitschakelen*. U kunt alleen LLAP uitschakelen.

* Azure Data Lake Storage Gen2 kan Juypter-notitieblokken niet opslaan in een Apache Spark-cluster.

## <a name="next-steps"></a>Volgende stappen

* [Documentatie voor Azure HDInsight](index.yml)
* [Releaseopmerkingen](hdinsight-release-notes.md)

---
title: Architectuur van Apache Hadoop - Azure HDInsight
description: Hierin worden de Apache Hadoop-opslag en verwerking in HDInsight-clusters.
services: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 066734c88890d5f1a6e42c5350db47d1a398b60a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277576"
---
# <a name="apache-hadoop-architecture-in-hdinsight"></a>Architectuur van Apache Hadoop in HDInsight

Apache Hadoop-bevat twee belangrijkste onderdelen: de Hadoop Distributed File System (HDFS) waarmee u opslag en nog een andere Resource Negotiator (YARN) waarmee de verwerking. Met storage en verwerkingsmogelijkheden voor wordt een cluster kan worden uitgevoerd van MapReduce-programma's, de gewenste verwerking van gegevens uit te voeren.

> [!NOTE]
> Een HDFS wordt doorgaans niet geïmplementeerd in het HDInsight-cluster om opslag te bieden. In plaats daarvan wordt een HDFS-compatibele interfacelaag gebruikt door Hadoop-onderdelen. De mogelijkheid van de werkelijke opslag wordt verstrekt door Azure Storage of Azure Data Lake Store. Voor Hadoop, MapReduce-taken uitvoeren op het HDInsight-cluster worden uitgevoerd als een HDFS aanwezig zijn en dus geen wijzigingen in de ter ondersteuning van hun behoefte aan opslag vereist. Opslag is uitbestede in Hadoop op HDInsight, maar YARN verwerking blijft een belangrijk onderdeel. Zie voor meer informatie, [Inleiding tot Azure HDInsight](hadoop/apache-hadoop-introduction.md).

Dit artikel bevat YARN en hoe deze coördineert de uitvoering van toepassingen op HDInsight.

## <a name="apache-yarn-basics"></a>Apache YARN-basisbeginselen 

YARN regelt en stuurt gegevens worden verwerkt in Hadoop. YARN heeft twee belangrijke services die worden uitgevoerd als de processen op knooppunten in het cluster: 

* ResourceManager 
* NodeManager

De ResourceManager verleent cluster compute-resources voor toepassingen zoals MapReduce-taken. De ResourceManager verleent deze resources als containers, waarbij elke container uit een toewijzing van CPU-kernen en RAM-geheugen bestaat. Als u alle resources die beschikbaar zijn in een cluster gecombineerd en vervolgens de kernen en het geheugen in blokken van gedistribueerd, is elk blok van resources in een container. Elk knooppunt in het cluster heeft een maximale capaciteit voor een bepaald aantal containers, zodat het cluster een vaste limiet voor het aantal containers beschikbaar heeft. De toewijzing van resources in een container kan worden geconfigureerd. 

Wanneer een MapReduce-toepassing wordt uitgevoerd op een cluster, biedt de ResourceManager de toepassing de containers waarin om uit te voeren. De ResourceManager houdt de status van het uitvoeren van toepassingen, beschikbare capaciteit, en toepassingen als ze volledig worden bijgehouden en hun resources vrijgeven. 

Een web server-proces waarmee een webgebruikersinterface voor het controleren van de status van toepassingen wordt ook uitgevoerd door de ResourceManager.

Wanneer een gebruiker een MapReduce-toepassing uit te voeren op het cluster verzendt, wordt de toepassing wordt verzonden naar de ResourceManager. De ResourceManager wijst op zijn beurt een container op beschikbare NodeManager knooppunten toe. De NodeManager-knooppunten zijn waar de toepassing daadwerkelijk wordt uitgevoerd. De eerste container toegewezen wordt een speciale toepassing met de naam van de ApplicationMaster uitgevoerd. Deze ApplicationMaster is verantwoordelijk voor het verkrijgen van resources in de vorm van de volgende containers, die nodig zijn om uit te voeren van het ingediende toepassing. De ApplicationMaster onderzoekt de fasen van de toepassing, zoals de fase van de kaart en verminder de fase en factoren in hoeveel gegevens moeten worden verwerkt. Vervolgens vraagt u de ApplicationMaster (*onderhandelt over*) de bronnen uit de ResourceManager namens de toepassing. De ResourceManager verkrijgt op zijn beurt resources van de NodeManagers in het cluster om de ApplicationMaster voor gebruik bij het uitvoeren van de toepassing. 

Het uitvoeren van de taken die van de toepassing NodeManagers vervolgens rapporteren de voortgang en status aan de ApplicationMaster. De ApplicationMaster rapporteert op zijn beurt de status van de aanvraag terug naar de ResourceManager. De ResourceManager retourneert geen resultaten naar de client.

## <a name="yarn-on-hdinsight"></a>YARN op HDInsight

Alle HDInsight-clustertypen implementeren YARN. De ResourceManager wordt geïmplementeerd voor hoge beschikbaarheid met een primaire en secundaire instantie, die respectievelijk op de eerste en tweede hoofdknooppunten binnen het cluster wordt uitgevoerd. Alleen het ene exemplaar van de ResourceManager is tegelijk actief. De NodeManager-exemplaren worden uitgevoerd op de beschikbare worker-knooppunten in het cluster.

![YARN op HDInsight](./media/hdinsight-hadoop-architecture/yarn-on-hdinsight.png)

## <a name="next-steps"></a>Volgende stappen

* [MapReduce gebruiken in Hadoop op HDInsight](hadoop/hdinsight-use-mapreduce.md)
* [Inleiding tot Azure HDInsight](hadoop/apache-hadoop-introduction.md)

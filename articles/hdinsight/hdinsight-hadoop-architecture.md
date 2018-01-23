---
title: Architectuur van Hadoop - Azure HDInsight | Microsoft Docs
description: Beschrijft de Hadoop-opslag en verwerking op HDInsight-clusters.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: 85383cc32e67db1f7e6964dc0b55bf3977311d40
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="hadoop-architecture-in-hdinsight"></a>Architectuur van Hadoop in HDInsight

Hadoop bevat twee basisonderdelen, hoge dichtheid File System (HDFS) waarmee u opslag en nog een andere Resource onderhandelaar (YARN) waarmee de verwerking. Een cluster wordt met mogelijkheden voor opslag en verwerking kan worden uitgevoerd van MapReduce-programma's uit te voeren van de gewenste gegevens verwerkt.

> [!NOTE]
> Een HDFS wordt doorgaans niet geïmplementeerd in het HDInsight-cluster om opslag te bieden. In plaats daarvan wordt een HDFS-compatibele interfacelaag gebruikt door de Hadoop-onderdelen. De werkelijke opslagcapaciteit wordt verstrekt door Azure Storage of Azure Data Lake Store. Voor Hadoop, MapReduce-taken worden uitgevoerd op het HDInsight-cluster worden uitgevoerd als een HDFS aanwezig zijn en dus vereisen geen wijzigingen ter ondersteuning van hun opslagbehoeften aan. Opslag is externe in Hadoop in HDInsight, maar YARN verwerking een kernonderdeel blijft. 

<!--   As described in [HDInsight architecture](hdinsight-architecture.md)  -->

Dit artikel bevat YARN en hoe deze coördineert de uitvoering van toepassingen op HDInsight.

## <a name="yarn-basics"></a>YARN-basisbeginselen 

YARN regelt en stuurt gegevens worden verwerkt in Hadoop. YARN heeft twee belangrijkste services die worden uitgevoerd als de processen op knooppunten in het cluster: 

* ResourceManager 
* NodeManager

De ResourceManager verleent cluster rekenresources voor toepassingen zoals MapReduce-taken. De ResourceManager verleent deze resources als containers, waarbij elke container uit een toewijzing van CPU-kernen en het RAM-geheugen bestaat. Als u alle resources die beschikbaar zijn in een cluster gecombineerd en vervolgens gedistribueerd ze in blokken van een bepaald aantal kernen en het geheugen, is elk blok van resources in een container. Elk knooppunt in het cluster heeft een capaciteit voor een bepaald aantal containers en daarom het cluster een vaste limiet van het aantal containers beschikbaar heeft. De toewijzing van resources in een container kan worden geconfigureerd. 

Wanneer een MapReduce-toepassing wordt uitgevoerd op een cluster, biedt de ResourceManager de toepassing de containers waarin u wilt uitvoeren. De ResourceManager houdt de status van actieve toepassingen, beschikbare capaciteit en toepassingen, zoals ze volledig worden bijgehouden en hun bronnen release. 

De ResourceManager wordt ook uitgevoerd voor een web server-proces waarmee een web-gebruikersinterface die kunt u de status bewaken van toepassingen. 

Wanneer een gebruiker een MapReduce-toepassing uit te voeren op het cluster verzendt, wordt de aanvraag is ingediend bij de ResourceManager. De ResourceManager wijst op zijn beurt een container op beschikbare NodeManager knooppunten. De knooppunten NodeManager zijn waar de toepassing daadwerkelijk wordt uitgevoerd. De eerste container toegewezen kan een speciale toepassing met de naam van de ApplicationMaster worden uitgevoerd. Deze ApplicationMaster is verantwoordelijk voor het ophalen van resources in de vorm van de volgende containers, die nodig zijn voor het uitvoeren van de opgegeven toepassing. De ApplicationMaster onderzoekt de stadia van de toepassing (de kaart fase en fase verminderen) en factoren op hoeveel gegevens moet worden verwerkt. De ApplicationMaster vraagt vervolgens (*onderhandelt*) de bronnen van de ResourceManager voor de toepassing is. De ResourceManager verleent op zijn beurt bronnen uit de NodeManagers in het cluster de ApplicationMaster voor gebruik bij het uitvoeren van de toepassing. 

Het uitvoeren van de taken die gezamenlijk de toepassing NodeManagers vervolgens rapporteren hun voortgang en status aan de ApplicationMaster. De ApplicationMaster meldt op zijn beurt de status van de toepassing terug naar de ResourceManager. De ResourceManager retourneert de resultaten naar de client.

## <a name="yarn-on-hdinsight"></a>YARN in HDInsight

Alle HDInsight-clustertypen implementeren YARN. De ResourceManager wordt geïmplementeerd voor maximale beschikbaarheid aan een exemplaar van de primaire en secundaire waarmee respectievelijk op de eerste en tweede hoofdknooppunten in het cluster uitgevoerd. Alleen de één exemplaar van de ResourceManager is tegelijk actief. De NodeManager-instanties worden uitgevoerd op de beschikbare worker-knooppunten in het cluster.

![YARN in HDInsight](./media/hdinsight-hadoop-architecture/yarn-on-hdinsight.png)

## <a name="see-also"></a>Zie ook

* [MapReduce in Hadoop in HDInsight gebruiken](hadoop/hdinsight-use-mapreduce.md)

<!--  * [HDInsight Architecture](hdinsight-architecture.md)  -->

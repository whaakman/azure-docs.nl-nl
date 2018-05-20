---
title: Externe metagegevens archieven - Azure HDInsight | Microsoft Docs
description: Externe metagegevens worden gebruikt met HDInsight-clusters.
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
tags: azure-portal
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jgao
ms.openlocfilehash: 2eadee1a8695450e2219031ea1a65ee3624f26b5
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Externe metagegevens winkels in Azure HDInsight gebruiken

Het Hive-metastore in HDInsight is een essentieel onderdeel van Hadoop-architectuur. Een metastore is de centrale schema-opslagplaats die kan worden gebruikt door andere big data access-hulpprogramma's zoals Spark, interactieve Query (LLAP), functie of Pig. HDInsight maakt gebruik van een Azure SQL Database als de Hive-metastore.

![Architectuur van de HDInsight Hive-metagegevens Store](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Er zijn twee manieren waarop die u een metastore voor uw HDInsight-clusters instellen kunt:

* [Standaard-metastore](#default-metastore)
* [Aangepaste metastore](#custom-metastore)

## <a name="default-metastore"></a>Standaard-metastore

Standaard levert HDInsight een metastore met elk clustertype. U kunt in plaats daarvan een aangepaste metastore opgeven. De standaard-metastore omvat de volgende overwegingen:
- Kan zonder extra kosten. HDInsight levert een metastore met elk clustertype zonder extra kosten voor u.
- Elke standaard metastore maakt deel uit van de levensduur van het cluster. Als u verwijdert een cluster dat metastore en metagegevens worden ook verwijderd.
- U kunt de standaard-metastore niet delen met andere clusters.
- De standaard-metastore maakt gebruik van de basis Azure SQL DB, die maximaal 5 DTU (database transaction unit) heeft.
Deze standaard metastore wordt doorgaans gebruikt voor relatief eenvoudige werkbelastingen die meerdere clusters niet nodig en hoeft de metagegevens behouden levenscyclus van het cluster.


## <a name="custom-metastore"></a>Aangepaste metastore

HDInsight biedt ook ondersteuning voor aangepaste metastores wordt aanbevolen voor productieclusters:
- U kunt uw eigen Azure SQL Database opgeven als de metastore.
- De levenscyclus van de metastore is niet gekoppeld aan een levenscyclus clusters, zodat u kunt maken en verwijderen van clusters zonder verlies van metagegevens. Zelfs na het verwijderen en opnieuw maken van het HDInsight-cluster bewaard metagegevens zoals uw Hive-schema.
- Een aangepaste metastore kunt u meerdere clusters en clustertypen koppelen aan die metastore. Een enkele metastore kan bijvoorbeeld worden gedeeld door interactieve Query, Hive en Spark-clusters in HDInsight.
- U betaalt voor de kosten van een metastore (Azure SQL DB) volgens het prestatieniveau dat u kiest.
- U kunt de metastore opschalen naar behoefte.


![HDInsight Hive-metagegevens Store gebruiksvoorbeeld](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

<!-- Image – Typical shared custom Metastore scenario in HDInsight (?) -->



### <a name="select-a-custom-metastore-during-cluster-creation"></a>Selecteer een aangepaste metastore tijdens het maken van het cluster

U kunt uw cluster verwijzen naar een eerder gemaakte Azure SQL Database tijdens het maken van het cluster of u kunt de SQL-Database configureren nadat het cluster is gemaakt. Deze optie wordt opgegeven met de opslag > Metastore instellingen tijdens het maken van een nieuwe Hadoop, Spark of interactieve Hive-cluster van Azure-portal.

![HDInsight Hive-metagegevens Store Azure-portal](./media/hdinsight-use-external-metadata-stores/metadata-store-azure-portal.png)

U kunt ook extra clusters toevoegen aan een aangepaste metastore van Azure-portal of van de Ambari-configuraties (Hive > Geavanceerd)

![HDInsight Hive-metagegevens Store Ambari](./media/hdinsight-use-external-metadata-stores/metadata-store-ambari.png)

## <a name="hive-metastore-best-practices"></a>Hive-metastore aanbevolen procedures

Hier volgen enkele algemene HDInsight Hive-metastore aanbevolen procedures:

- Gebruik een aangepaste metastore waar mogelijk, zoals dit helpt afzonderlijke rekenresources (actief cluster) en metagegevens (opgeslagen in de metastore).
- Beginnen met een laag S2, waardoor 50 DTU en 250 GB aan opslagruimte. Als er een knelpunt is, kunt u de database opschalen.
- Zorg ervoor dat de metastore gemaakt voor een HDInsight-cluster versie wordt niet gedeeld in verschillende versies van HDInsight-cluster. Verschillende versies van Hive gebruiken verschillende schema's. U kunt een metastore niet delen met zowel 1.2 Hive en 2.1 Hive-clusters.
- Back-up van uw aangepaste metastore regelmatig.
- Houd uw metastore en HDInsight-cluster in dezelfde regio.
- Controleer uw metastore voor prestaties en beschikbaarheid met behulp van Azure SQL Database controlehulpprogramma's, zoals de Azure portal of Azure-logboekanalyse.

## <a name="oozie-metastore"></a>Oozie-Metastore

Apache Oozie is een coördinatiesysteem voor werkstromen waarmee Hadoop-taken worden beheerd.  Oozie ondersteunt Hadoop-taken voor Apache MapReduce, Pig, Hive en anderen.  Een metastore Oozie gebruikt voor het opslaan van gegevens over de huidige en voltooide workflows. U kunt Azure SQL Database om de prestaties bij gebruik van Oozie te gebruiken als een aangepaste metastore. De metastore biedt ook toegang tot de taakgegevens Oozie nadat u uw cluster hebt verwijderd.

Zie voor instructies over het maken van een Oozie-metastore met Azure SQL Database [gebruik Oozie voor werkstromen](hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Volgende stappen

- [Clusters in HDInsight met Hadoop, Spark en Kafka instellen](./hdinsight-hadoop-provision-linux-clusters.md)

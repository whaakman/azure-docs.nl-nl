---
title: Externe metagegevensopslag - Azure HDInsight gebruiken
description: Externe metagegevensopslag gebruiken met HDInsight-clusters.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/14/2018
ms.openlocfilehash: 7c58162048de341468b69a29c55edf346b376e9b
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733811"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Externe metagegevensopslag in Azure HDInsight gebruiken

Het Hive-metastore in HDInsight is een essentieel onderdeel van Hadoop-architectuur. Een metastore is de centrale schema-opslagplaats die kan worden gebruikt door andere big data access-hulpprogramma's zoals Spark, interactieve Query (LLAP), Presto of Pig. HDInsight maakt gebruik van een Azure SQL Database als de Hive-metastore.

![HDInsight Hive-metagegevens Store-architectuur](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Er zijn twee manieren waarop die u een metastore voor uw HDInsight-clusters instellen kunt:

* [Standaardmetastore](#default-metastore)
* [Aangepaste metastore](#custom-metastore)

## <a name="default-metastore"></a>Standaardmetastore

HDInsight maakt standaard een metastore met elk clustertype. U kunt in plaats daarvan een aangepaste metastore opgeven. De standaardmetastore bevat de volgende overwegingen:
- Er zijn geen extra kosten. HDInsight maakt een metastore met elk clustertype zonder extra kosten aan u.
- Elke standaardmetastore maakt deel uit van de levenscyclus van de cluster. Wanneer u een cluster verwijdert, worden de bijbehorende metastore en metagegevens worden ook verwijderd.
- U kunt de standaardmetastore niet delen met andere clusters.
- De standaardmetastore maakt gebruik van de basic Azure SQL DB, met maximaal vijf DTU (database transaction units).
Deze standaardmetastore wordt doorgaans gebruikt voor relatief eenvoudige workloads die meerdere clusters vereisen en geen metagegevens behouden levenscyclus van het cluster.


## <a name="custom-metastore"></a>Aangepaste metastore

HDInsight biedt ook ondersteuning voor aangepaste metastores, die worden aanbevolen voor productieclusters:
- U kunt uw eigen Azure SQL-Database opgeven als de metastore.
- De levenscyclus van de metastore is niet gekoppeld aan een levenscyclus van clusters, zodat u kunt maken en -clusters zonder verlies van metagegevens verwijderen. Metagegevens, zoals uw Hive-schema's blijven behouden ook nadat u verwijdert en opnieuw maken van het HDInsight-cluster.
- Een aangepaste metastore kunt u meerdere clusters en clustertypen die metastore koppelen. Een enkele metastore kan bijvoorbeeld worden gedeeld met Interactive Query Hive en Spark-clusters in HDInsight.
- U betaalt voor de kosten van een metastore (Azure SQL database) op basis van het prestatieniveau dat u kiest.
- U kunt de metastore opschalen naar behoefte.

![HDInsight Hive-metagegevens Store Use Case](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)


### <a name="select-a-custom-metastore-during-cluster-creation"></a>Een aangepaste metastore selecteren tijdens het maken van clusters

U kunt uw cluster verwijzen naar een eerder gemaakte Azure SQL Database tijdens het maken of kunt u de SQL-Database configureren nadat het cluster is gemaakt. Deze optie is opgegeven met de Storage-> Metastore-instellingen tijdens het maken van een nieuwe Hadoop, Spark of interactieve Hive-cluster van Azure portal.

![HDInsight Hive-metagegevens Store Azure portal](./media/hdinsight-use-external-metadata-stores/metadata-store-azure-portal.png)

U kunt ook extra clusters toevoegen aan een aangepaste metastore van Azure portal of van de Ambari-configuraties (Hive-> Geavanceerd)

![HDInsight Hive-metagegevens Store Ambari](./media/hdinsight-use-external-metadata-stores/metadata-store-ambari.png)

## <a name="hive-metastore-best-practices"></a>Hive-metastore aanbevolen procedures

Hier volgen enkele algemene HDInsight Hive-metastore aanbevolen procedures:

- Gebruik een aangepaste metastore indien mogelijk, voor de afzonderlijke rekenresources (uw actief cluster) en metagegevens (opgeslagen in de metastore).
- Beginnen met een S2-laag, die 50 dtu's en 250 GB aan opslagruimte. Als u een knelpunt ziet, kunt u de database omhoog schalen.
- Als u van plan meerdere HDInsight-clusters bent voor toegang tot afzonderlijke gegevens, moet u een afzonderlijke database gebruiken voor de metastore op elke cluster. Als u een metastore tussen meerdere HDInsight-clusters delen, betekent dit dat de clusters de dezelfde metagegevens en de onderliggende gegevensbestanden van de gebruiker gebruiken.
- Back-up van uw aangepaste metastore periodiek. Azure SQL Database-back-ups automatisch genereert, maar is afhankelijk van het back-upretentie tijdsbestek. Zie voor meer informatie, [meer informatie over de automatische SQL-Database back-ups](../sql-database/sql-database-automated-backups.md).
- Zoek uw metastore en HDInsight-cluster in dezelfde regio, voor de prestaties van de hoogste en laagste kosten voor netwerk-uitgaand verkeer.
- Controleer uw metastore voor prestaties en beschikbaarheid met behulp van Azure SQL Database controleren hulpmiddelen, zoals de Azure portal of Azure Log Analytics.
- Wanneer een nieuwe, hogere versie van Azure HDInsight is gemaakt op basis van een bestaande aangepaste metastore-database, werkt het systeem het schema van de metastore die zonder de database herstellen vanuit back-up is niet ongedaan worden gemaakt.
- Als u een metastore tussen meerdere clusters delen, controleert u of dat alle clusters zijn dezelfde versie van HDInsight. Verschillende versies van Hive gebruiken verschillende metastore databaseschema's. U kunt een metastore niet delen tussen verschillende Hive 1.2 en 2.1 Hive versioned clusters. 

## <a name="oozie-metastore"></a>Oozie-Metastore

Apache Oozie is een coördinatiesysteem voor werkstromen waarmee Hadoop-taken worden beheerd.  Oozie biedt ondersteuning voor Hadoop-taken voor Apache MapReduce, Pig, Hive en anderen.  Oozie maakt gebruik van een metastore voor het opslaan van informatie over de huidige en voltooide werkstromen. Voor betere prestaties bij het gebruik van Oozie, kunt u Azure SQL Database als een aangepaste metastore. De metastore biedt ook toegang tot taakgegevens Oozie nadat u uw cluster hebt verwijderd.

Zie voor instructies over het maken van een Oozie-metastore met Azure SQL Database [Oozie gebruiken voor werkstromen](hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Volgende stappen

- [Clusters instellen in HDInsight met Hadoop, Spark, Kafka en meer](./hdinsight-hadoop-provision-linux-clusters.md)

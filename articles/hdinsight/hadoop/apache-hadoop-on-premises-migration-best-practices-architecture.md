---
title: On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - architectuur van aanbevolen procedures
description: Informatie over aanbevolen procedures van de architectuur voor de migratie on-premises Hadoop-clusters op Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 7558a853657e3e3764cd8e3faf6dd466e9ead35e
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52994158"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---architecture-best-practices"></a>On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - architectuur van aanbevolen procedures

In dit artikel biedt aanbevelingen voor de architectuur van Azure HDInsight-systemen. Het onderdeel van een serie die biedt best practices om u te helpen migreren on-premises Apache Hadoop-systemen tot Azure HDInsight.

## <a name="use-multiple-workload-optimized-clusters"></a>Meerdere werkbelasting geoptimaliseerde clusters gebruiken

Veel on-premises Apache Hadoop-implementaties bestaan uit één groot cluster die ondersteuning biedt voor veel werkbelastingen. Dit één cluster erg complex zijn en vereisen mogelijk inbreuk op de afzonderlijke services om te controleren alles samenwerken. Migreren on-premises Hadoop-clusters op Azure HDInsight is een wijziging in de aanpak vereist.

Azure HDInsight-clusters zijn ontworpen voor een specifiek type compute-gebruik. Omdat opslag kan worden gedeeld met meerdere clusters, is het mogelijk te maken van meerdere geoptimaliseerd voor werkbelasting rekenclusters om te voldoen aan de behoeften van verschillende taken. Elk clustertype heeft de optimale configuratie voor die specifieke werkbelasting. De volgende tabel bevat de ondersteunde clustertypen in HDInsight en de bijbehorende werkbelastingen.

|**Workload**|**HDInsight-clustertype**|
|---|---|
|Batchverwerking (ETL / ELT)|Hadoop, Spark|
|Datawarehousing|Hadoop, Spark, interactieve Query|
|IoT / Streaming|Kafka, Storm, Spark|
|NoSQL-transacties verwerken|HBase|
|Interactief en sneller query's met caching in het geheugen|Interactive Query|
|Data Science|ML-Services, Spark|

De volgende tabel ziet u de verschillende methoden die kunnen worden gebruikt om een HDInsight-cluster te maken.

|**Hulpprogramma**|**Browsergebaseerde**|**Vanaf de opdrachtregel**|**REST API**|**SDK**|
|---|---|---|---|---|
|[Azure Portal](../hdinsight-hadoop-create-linux-clusters-portal.md)|X||||
|[Azure Data Factory](../hdinsight-hadoop-create-linux-clusters-adf.md)|X|X|X|X|
|[Azure CLI (versie 1.0)](../hdinsight-hadoop-create-linux-clusters-azure-cli.md)||X|||
|[Azure PowerShell](../hdinsight-hadoop-create-linux-clusters-azure-powershell.md)||X|||
|[cURL](../hdinsight-hadoop-create-linux-clusters-curl-rest.md)||X|X||
|[.NET SDK](../hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)||||X|
|[Python SDK](https://docs.microsoft.com/python/api/overview/azure/hdinsight?view=azure-python)||||X|
|[Java SDK](https://docs.microsoft.com/java/api/overview/azure/hdinsight?view=azure-java-stable)||||X|
|[Azure Resource Manager-sjablonen](../hdinsight-hadoop-create-linux-clusters-arm-templates.md)||X|||

Zie voor meer informatie het artikel [clustertypen in HDInsight](../hadoop/apache-hadoop-introduction.md).

## <a name="use-transient-on-demand-clusters"></a>Tijdelijke on-demand clusters gebruiken

HDInsight-clusters gaat niet-gebruikte gedurende lange perioden. Als u wilt besparen op de resourcekosten, HDInsight biedt ondersteuning voor de tijdelijke clusters op aanvraag, die kunnen worden verwijderd wanneer de werkbelasting met succes is voltooid.

Wanneer u een cluster verwijdert, worden de gekoppelde storage-account en de externe metagegevens worden niet verwijderd. Het cluster kan later worden opnieuw gemaakt met de dezelfde opslagaccounts en meta-archieven.

Azure Data Factory kan worden gebruikt voor het plannen van het maken van on-demand HDInsight-clusters. Zie voor meer informatie het artikel [op aanvraag Apache Hadoop-clusters maken in HDInsight met behulp van Azure Data Factory](../hdinsight-hadoop-create-linux-clusters-adf.md).

## <a name="decouple-storage-from-compute"></a>Loskoppelen van de compute-opslag

Typische on-premises Hadoop-implementaties gebruiken dezelfde set machines voor gegevensopslag en verwerking van gegevens. Omdat ze geplaatst zijn, moeten berekeningen en opslag samen worden geschaald.

Opslag niet hoeft te worden geplaatst met compute op HDInsight-clusters, en kan bestaan uit Azure storage, Azure Data Lake Storage of beide. Ontkoppeling compute-opslag biedt de volgende voordelen:

- Gegevens delen tussen verschillende clusters.
- Het gebruik van tijdelijke clusters, omdat de gegevens zijn niet afhankelijk van het cluster.
- Verminderde kosten voor gegevensopslag.
- Opslag schalen en compute afzonderlijk.
- Replicatie van gegevens tussen regio's.

Compute-clusters dicht bij de storage-account-resources in een Azure-regio om te verminderen van kosten voor het scheiden van reken- en worden gemaakt. Snelle netwerken kunt u efficiënt voor de compute-knooppunten voor toegang tot de gegevens in Azure storage.

## <a name="use-external-metadata-stores"></a>Externe metagegevensopslag gebruiken

Er zijn twee belangrijkste metastores die met HDInsight-clusters werken: [Apache Hive](https://hive.apache.org/) en [Apache Oozie](https://oozie.apache.org/). Het Hive-metastore is de centrale schema-opslagplaats die kan worden gebruikt door gegevensverwerking-engines zoals Hadoop, Spark, LLAP, Presto en Apache Pig. De Oozie-metastore opgeslagen informatie over het plannen en de status van in uitvoering en voltooide Hadoop-taken.

HDInsight maakt gebruik van Azure SQL Database voor Hive en Oozie metastores. Er zijn twee manieren voor het instellen van een metastore in HDInsight-clusters:

1. Standaardmetastore

    - Er zijn geen extra kosten.
    - Metastore wordt verwijderd wanneer het cluster wordt verwijderd.
    - Metastore kan niet worden gedeeld tussen verschillende clusters.
    - Maakt gebruik van eenvoudige Azure SQL DB, waarvoor een vijf DTU-limiet.

1. Aangepaste externe metastore

    - een externe Azure SQL Database als de metastore opgeven.
    - Clusters worden gemaakt en verwijderd zonder verlies van metagegevens zoals Hive-schema Oozie taakdetails.
    - Één metastore db kan worden gedeeld met andere soorten clusters.
    - Metastore kan worden opgeschaald naar behoefte.
    - Zie voor meer informatie, [externe metagegevensopslag in Azure HDInsight gebruiken](../hdinsight-use-external-metadata-stores.md).

## <a name="best-practices-for-hive-metastore"></a>Aanbevolen procedures voor Hive-Metastore

Enkele aanbevolen procedures voor HDInsight Hive-metastore zijn als volgt:

- Gebruik een aangepaste externe metastore naar verschillende compute-resources en metagegevens.
- Beginnen met een S2-laag Azure SQL-exemplaar, waarmee u 50 dtu's en 250 GB aan opslagruimte. Als u een knelpunt ziet, kunt u de database omhoog schalen.
- Deel geen de metastore voor één versie van HDInsight-cluster met clusters van een andere versie gemaakt. Verschillende versies van Hive gebruiken verschillende schema's. Bijvoorbeeld, kan niet een metastore worden gedeeld met zowel Hive 1.2 en 2.1 Hive-clusters.
- Back-up van de aangepaste metastore periodiek.
- Houd de metastore en het HDInsight-cluster in dezelfde regio.
- Controleer de metastore voor prestaties en beschikbaarheid met behulp van Azure SQL Database controleren hulpmiddelen, zoals Azure portal of Azure Log Analytics.
- Voer de **tabel analyseren** opdracht zoals vereist voor het genereren van statistieken voor tabellen en kolommen. Bijvoorbeeld `ANALYZE TABLE [table_name] COMPUTE STATISTICS`.

## <a name="best-practices-for-different-workloads"></a>Aanbevolen procedures voor verschillende werkbelastingen

- Overweeg het gebruik van LLAP cluster voor interactieve Hive-query's met betere reactietijd [LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) is een nieuwe functie in Hive 2.0 waarmee de caching van query's in het geheugen. LLAP maakt Hive-query's sneller en maximaal [26 x sneller dan Hive 1.x in sommige gevallen](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).
- Overweeg het gebruik van Spark-taken in plaats van Hive-taken.
- Houd rekening met het vervangen van impala-query's met LLAP query's.
- Houd rekening met het vervangen van MapReduce-taken met Spark-taken.
- Houd rekening met lage latentie Spark batch-taken met behulp van Spark Structured Streaming taken vervangen.
- Overweeg het gebruik van Azure Data Factory (ADF) 2.0 voor het indelen van.
- Houd rekening met Ambari voor Clusterbeheer.
- De opslag van gegevens van on-premises HDFS WASB of ADLS of AD FS wijzigen voor de verwerking van scripts.
- U kunt met het RBAC voor Ranger Hive-tabellen en controle.
- Overweeg het gebruik van cosmos DB in plaats van MongoDB of Cassandra.

## <a name="next-steps"></a>Volgende stappen

Lees het volgende artikel in deze reeks:

- [Aanbevolen procedures van de infrastructuur voor on-premises naar Azure HDInsight Hadoop-migratie](apache-hadoop-on-premises-migration-best-practices-infrastructure.md)

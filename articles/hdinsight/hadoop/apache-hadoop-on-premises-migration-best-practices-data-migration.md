---
title: On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - gegevens migratie aanbevolen procedures
description: Leer gegevens migratie aanbevolen procedures voor het migreren on-premises Hadoop-clusters op Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 54741bd2d76a7ba414613a40e07c47be703aa033
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52994408"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---data-migration-best-practices"></a>On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - gegevens migratie aanbevolen procedures

In dit artikel biedt aanbevelingen voor gegevensmigratie naar Azure HDInsight. Het onderdeel van een serie die biedt best practices om u te helpen migreren on-premises Apache Hadoop-systemen tot Azure HDInsight.

## <a name="migrate-on-premises-data-to-azure"></a>On-premises gegevens migreren naar Azure

Er zijn twee manieren om gegevens te migreren van on-premises naar Azure-omgeving:

1.  Gegevens overdragen via het netwerk met TLS
    1. Via internet - kunt u gegevens overbrengen naar Azure-opslag via een gewone internetverbinding met behulp van een van verschillende hulpprogramma's zoals: Azure Storage Explorer, AzCopy, Azure Powershell en Azure CLI.  Zie [om gegevens te verplaatsen naar en van Azure Storage](../../storage/common/storage-moving-data.md) voor meer informatie.
    2. Express Route - ExpressRoute is een Azure-service waarmee u particuliere verbindingen maken tussen Microsoft-datacenters en infrastructuur on-premises of in een CO-locatiefaciliteit. ExpressRoute-verbindingen niet het openbare Internet en bieden een hogere beveiliging, betrouwbaarheid en snelheid met kortere wachttijden dan gebruikelijke verbindingen via Internet. Zie voor meer informatie, [maken en aanpassen van een ExpressRoute-circuit](../../expressroute/expressroute-howto-circuit-portal-resource-manager.md).
    1. Data Box online-gegevensoverdracht - gegevens in het Edge- en Data Box-Gateway zijn online-gegevens overbrengen producten die fungeren als gateways voor het beheren van gegevens tussen uw locatie en Azure storage. Met Data Box Edge, een on-premises netwerkapparaat, worden gegevens overdragen van en naar Azure en worden gegevens verwerkt met behulp van Edge-rekenprocessen met artificial intelligence (AI). Data Box Gateway is een virtueel apparaat met opslaggatewaymogelijkheden. Zie voor meer informatie, [Azure Data Box-documentatie - Online Transfer](https://docs.microsoft.com/azure/databox-online/).
1.  Verzending van gegevens Offline
    1. Import / Export-service - kunt u fysieke schijven naar Azure verzenden en ze voor u worden geüpload. Zie voor meer informatie, [wat is Azure Import/Export-service?](https://docs.microsoft.com/azure/storage/common/storage-import-export-service).
    1. Gegevens vak offline gegevensoverdracht - gegevens in het Data Box-schijf, en gegevens in het zware apparaten kunnen u grote hoeveelheden gegevens overdragen naar Azure als het netwerk niet als een optie. Deze apparaten voor offlinegegevensoverdracht worden verzonden van het Azure-datacenter naar uw organisatie en vice versa. De apparaten maken gebruik van AES-versleuteling om uw gegevens tijdens de overdracht te beveiligen en ondergaan na het uploaden een grondig opschoningsproces om uw gegevens van het apparaat te verwijderen. Zie voor meer informatie, [Azure Data Box-documentatie - Offline Transfer](https://docs.microsoft.com/azure/databox/).

De volgende tabel bevat geschatte data transfer duur op basis van de gegevens volume en de netwerkbandbreedte. Een Data box gebruiken als de gegevensmigratie wordt verwacht dat meer dan drie weken duren.

|**De hoeveelheid gegevens**|**Netwerkbandbreedte**|||
|---|---|---|---|
|| **45 Mbps (T3)**|**100 Mbps**|**1 Gbps**|**10 Gbps**
|1 TB|2 dagen|1 dag| 2 uur|14 minuten|
|10 TB|22 dagen|tien dagen|1 dag|2 uur|
|35 TB|76 dagen|34 dagen|3 dagen|8 uur|
|80 TB|173 dagen|78 dagen|8 dagen|19 uur|
|100 TB|216 dagen|97 dagen|tien dagen|1 dag|
|200 TB|1 jaar|194 dagen|19 dagen|2 dagen|
|500 TB|Drie jaar|1 jaar|49 dagen|5 dagen|
|1 PB|zes jaar|Drie jaar|97 dagen|tien dagen|
|2 PB|twaalf jaar|5 jaar|194 dagen|19 dagen|

Hulpprogramma's ingebouwd in Azure, zoals DistCp, Azure Data Factory en AzureCp, kunnen worden gebruikt voor gegevensoverdracht via het netwerk. Het hulpprogramma van derden WANDisco kan ook worden gebruikt voor hetzelfde doel. Kafka Mirrormaker en Sqoop kunnen worden gebruikt voor continue gegevensoverdracht van on-premises naar Azure storage-systemen.

## <a name="performance-considerations-with-apache-distcp"></a>Prestatieoverwegingen met Apache DistCp

DistCp is een Apache-project die gebruikmaakt van een kaart MapReduce-taak gegevens overdragen, fouten afhandelen en herstellen van deze fouten. Een lijst met bronbestanden wilt toewijzen aan elke kaart-taak. De taak kaart kopieert alle toegewezen bestanden vervolgens naar de bestemming. Er zijn verschillende technieken kunnen de prestaties van DistCp verbeterd.

### <a name="increase-the-number-of-mappers"></a>Vergroot het aantal Mappers

DistCp probeert te maken van taken van de kaart zodat elke ruwweg hetzelfde aantal bytes kopieert. DistCp taken gebruiken standaard 20 mappers. Met behulp van meer Mappers voor Distcp (met de 'M ' parameter bij de opdrachtregel) verhoogt de parallelle uitvoering tijdens de overdracht van gegevens en verkleint u de lengte van de gegevensoverdracht. Er zijn echter twee dingen aan moet denken bij het verhogen van het aantal Mappers:

1. DistCp van laagste granulariteit is één bestand. Een aantal Mappers meer dan het aantal van de bronbestanden op te geven niet werkt en wordt de beschikbare clusterbronnen verspild.
1. Houd rekening met het beschikbare geheugen van de Yarn op het cluster om het aantal Mappers te bepalen. Elke kaart-taak wordt gestart als een Yarn-container. Ervan uitgaande dat er geen andere zware workloads worden uitgevoerd op het cluster, het aantal Mappers kan worden bepaald door de volgende formule: m = (aantal worker-knooppunten \* YARN-geheugen voor elk werkrolknooppunt) / problemen met YARN containergrootte. Echter, als andere toepassingen geheugen gebruikt, kiest u alleen een gedeelte van de YARN-geheugen voor DistCp taken.

### <a name="use-more-than-one-distcp-job"></a>Meer dan één taak van DistCp gebruiken

Wanneer de grootte van de gegevensset worden verplaatst, groter dan 1 TB is, gebruikt u meer dan één DistCp taak. Met behulp van meer dan één taak beperkt de gevolgen van fouten. Als een taak mislukt, moet u alleen die specifieke taak in plaats van alle taken opnieuw.

### <a name="consider-splitting-files"></a>Bestanden splitsen

Als er een klein aantal grote bestanden, overweeg dan om ze te splitsen in segmenten van 256 MB aan bestanden meer mogelijke gelijktijdigheid met meer Mappers ophalen.

### <a name="use-the-strategy-command-line-parameter"></a>Gebruik de opdrachtregelparameter 'strategie'

Overweeg het gebruik van `strategy = dynamic` parameter vanaf de opdrachtregel. De standaardwaarde van de `strategy` parameter `uniform size`, elke kaart kopieert in dat geval ongeveer hetzelfde aantal bytes. Als deze parameter wordt gewijzigd naar `dynamic`, het bestand van de aanbieding is opgesplitst in verschillende 'chunk files'. Het aantal chunk-bestanden is een veelvoud zijn van het aantal kaarten. Elke taak kaart is een van de segment-bestanden toegewezen. Nadat alle paden in een segment wordt verwerkt, wordt het huidige segment verwijderd en wordt een nieuwe segment is aangeschaft. Het proces wordt voortgezet tot er geen segmenten meer beschikbaar zijn. Met deze 'dynamische' methode kan snellere kaart-taken die moeten worden gebruikt meer paden dan langzamer, dus versnellen van de algehele DistCp-taak.

### <a name="increase-the-number-of-threads"></a>Het aantal threads verhogen

Als het verhogen van de `-numListstatusThreads` parameter verbetert de prestaties. Deze parameter bepaalt het aantal threads worden gebruikt voor het bouwen van bestand aanbieding en 40 is de maximale waarde.

### <a name="use-the-output-committer-algorithm"></a>De uitvoer committer-algoritme gebruiken

Als de parameter doorgeven `-Dmapreduce.fileoutputcommitter.algorithm.version=2` verbetert de prestaties van DistCp. Deze uitvoer committer-algoritme heeft optimalisaties om uitvoerbestanden naar de bestemming te schrijven. De volgende opdracht wordt een voorbeeld waarin het gebruik van verschillende parameters:

```bash
hadoop distcp -Dmapreduce.fileoutputcommitter.algorithm.version=2 -numListstatusThreads 30 -m 100 -strategy dynamic hdfs://nn1:8020/foo/bar wasb://<container_name>@<storage_account_name>.blob.core.windows.net/foo/
```

## <a name="metadata-migration"></a>Migratie van de metagegevens

### <a name="hive"></a>Hive

Het hive-metastore kan worden gemigreerd met behulp van de scripts of met behulp van de database-replicatie.

#### <a name="hive-metastore-migration-using-scripts"></a>Hive-metastore migratie met behulp van scripts

1. Het Hive-DDLs van on-premises Hive-metastore genereren. Deze stap kan worden gedaan met behulp van een [wrapper bash-script](https://github.com/hdinsight/hdinsight.github.io/blob/master/hive/hive-export-import-metastore.md).
1. Bewerk het gegenereerde DDL ter vervanging van HDFS-url met ADLS-WASB/ABFS URL's.
1. Voer de bijgewerkte DDL op de metastore uit het HDInsight-cluster.
1. Zorg ervoor dat de versie van de Hive-metastore compatibel tussen on-premises en cloud.

#### <a name="hive-metastore-migration-using-db-replication"></a>Hive-metastore migratie met behulp van replicatie voor DB

- Instellen van databasereplicatie tussen on-premises Hive-metastore DB en HDInsight metastore DB.
- Gebruik "Hive MetaTool" vervangen door HDFS-url WASB/ADLS/ABFS URL's, bijvoorbeeld:

```bash
./hive --service metatool -updateLocation hdfs://nn1:8020/ wasb://<container_name>@<storage_account_name>.blob.core.windows.net/
```

### <a name="ranger"></a>Ranger

- On-premises Ranger-beleidsregels exporteren naar xml-bestanden.
- Transformeer on-premises bepaald op basis van HDFS paden naar WASB/ADLS met een hulpprogramma zoals XSLT.
- Importeer de beleidsregels die u aan bij de Ranger die worden uitgevoerd op HDInsight.

## <a name="next-steps"></a>Volgende stappen

Lees het volgende artikel in deze reeks:

- [Aanbevolen procedures voor beveiliging en DevOps voor on-premises naar Azure HDInsight Hadoop-migratie](apache-hadoop-on-premises-migration-best-practices-security-devops.md)
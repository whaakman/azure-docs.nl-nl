---
title: Release opmerkingen voor Azure HDInsight
description: Nieuwste opmerkingen bij de release voor Azure HDInsight. Bekijk ontwikkel tips en Details voor Hadoop, Spark, R Server, Hive en meer.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/08/2019
ms.openlocfilehash: 7c31520efd881e8e0b5ed309f62d273bac59c0e3
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68945018"
---
# <a name="release-notes"></a>Releaseopmerkingen

Dit artikel bevat informatie over de **meest recente** updates voor Azure HDInsight-release. Zie voor meer informatie over eerdere versies het [HDInsight Release Notes-archief](hdinsight-release-notes-archive.md).

> [!IMPORTANT]  
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie het [artikel over HDInsight-versie beheer](hdinsight-component-versioning.md)voor meer informatie.

## <a name="summary"></a>Samenvatting

Azure HDInsight is onder zakelijke klanten een van de meest populaire services voor open source Apache Hadoop- en Apache Spark-analyses in Azure.

## <a name="new-features"></a>Nieuwe functies

Zie [what's New in HDI 4,0](../hdinsight/hdinsight-version-release.md)(Engelstalig) voor meer informatie over belang rijke wijzigingen met HDInsight 4,0.

## <a name="component-versions"></a>Onderdeel versies

De officiële Apache-versies van alle HDInsight 4,0-onderdelen worden hieronder vermeld. De vermelde onderdelen zijn releases van de meest recente stabiele versies die beschikbaar zijn.

- Apache Ambari 2.7.1
- Apache Hadoop 3.1.1
- Apache HBase 2.0.0
- Apache Hive 3.1.0
- Apache Kafka 1.1.1, 2.1.0
- Apache mahout 0.9.0 +
- Apache Oozie 4.2.0
- Apache Phoenix 4.7.0
- Apache Pig 0.16.0
- Apache zwerver 0.7.0
- Apache-schuif regelaar 0.92.0
- Apache Spark 2.3.1, 2.4.0
- Apache Sqoop 1.4.7
- Apache TEZ 0.9.1 tot en
- Apache Zeppelin 0.8.0
- Apache ZooKeeper 3.4.6

Latere versies van Apache-onderdelen worden soms gebundeld in de HDP-distributie, naast de hierboven vermelde versies. In dit geval worden deze latere versies weer gegeven in de tabel Technical previews en vervangen door de Apache-onderdeel versies van de bovenstaande lijst in een productie omgeving.

## <a name="apache-patch-information"></a>Informatie over Apache-patch

Zie voor meer informatie over de beschik bare patches in HDInsight 4,0 de vermelding patch voor elk product in de volgende tabel.

| Productnaam | Patch-informatie |
|---|---|
| Ambari | [Informatie over Ambari-patch](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.1.0/bk_ambari-release-notes/content/ambari_relnotes-2.7.1.0-patch-information.html) |
| Hadoop | [Informatie over Hadoop-patch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hadoop.html) |
| HBase | [Informatie over HBase-patch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hbase.html) |
| Hive  | Deze release bevat Hive 3.1.0 zonder extra Apache-patches.  |
| Kafka | Deze release biedt Kafka 1.1.1 zonder extra Apache-patches. |
| Oozie | [Informatie over Oozie-patch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_oozie.html) |
| Phoenix | [Informatie over Phoenix-patch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_phoenix.html) |
| Pig | [Informatie over Pig-patches](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_pig.html) |
| Ranger | [Informatie over de zwerver-patch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_ranger.html) |
| Spark | [Informatie over Spark-patches](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_spark.html) |
| Sqoop | Deze release biedt Sqoop 1.4.7 zonder extra Apache-patches. |
| Tez | Deze release biedt TEZ 0.9.1 tot en zonder extra Apache-patches. |
| Zeppelin | Deze release biedt Zeppelin 0.8.0 zonder extra Apache-patches. |
| Zookeeper | [Informatie over Zookeeper-patch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_zookeeper.html) |

## <a name="fixed-common-vulnerabilities-and-exposures"></a>Opgeloste veelvoorkomende beveiligings problemen en bloot stellingen

Voor meer informatie over beveiligings problemen die in deze release zijn opgelost, verwijzen wij u naar Hortonworks ' [verholpen veelvoorkomende beveiligings problemen en risico's voor HDP 3.0.1](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/cve.html).

## <a name="known-issues"></a>Bekende problemen

### <a name="replication-is-broken-for-secure-hbase-with-default-installation"></a>De replicatie voor beveiligde HBase met de standaard installatie is verbroken

Voor HDInsight 4,0 voert u de volgende stappen uit:

1. Schakel communicatie tussen clusters in.
1. Meld u aan bij de actieve hoofd knooppunt.
1. Down load een script om replicatie in te scha kelen met de volgende opdracht:

    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Typ de opdracht `sudo kinit <domainuser>`.
1. Typ de volgende opdracht om het script uit te voeren:

    ```
    sudo bash hdi_enable_replication.sh -m <hn0> -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```
Voor HDInsight 3,6 gaat u als volgt te werk:

1. Meld u aan bij Active HMaster ZK.
1. Down load een script om replicatie in te scha kelen met de volgende opdracht:
    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Typ de opdracht `sudo kinit -k -t /etc/security/keytabs/hbase.service.keytab hbase/<FQDN>@<DOMAIN>`.
1. Typ de volgende opdracht:

    ```bash
    sudo bash hdi_enable_replication.sh -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```

### <a name="phoenix-sqlline-stops-working-after-migrating-hbase-cluster-to-hdinsight-40"></a>Phoenix sqlline gebruiken werkt niet meer na migratie van HBase-cluster naar HDInsight 4,0

Voer de volgende stappen uit:

1. Verwijder de volgende Phoenix-tabellen:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.MUTEX`
    1. `SYSTEM.CATALOG`
1. Als u geen van de tabellen kunt verwijderen, start u HBase opnieuw om eventuele verbindingen met de tabellen te wissen.
1. Voer `sqlline.py` opnieuw uit. Phoenix maakt alle tabellen die u in stap 1 hebt verwijderd, opnieuw.
1. Het opnieuw genereren van Phoenix-tabellen en-weer gaven voor uw HBase-gegevens.

### <a name="phoenix-sqlline-stops-working-after-replicating-hbase-phoenix-metadata-from-hdinsight-36-to-40"></a>Phoenix sqlline gebruiken werkt niet meer na het repliceren van HBase Bredase meta gegevens van HDInsight 3,6 naar 4,0

Voer de volgende stappen uit:

1. Voordat u de replicatie uitvoert, gaat u naar het doel 4,0- `sqlline.py`cluster en voert u uit. Met deze opdracht worden Phoenix-tabellen `SYSTEM.MUTEX` gegenereerd `SYSTEM.LOG` zoals en die alleen bestaan in 4,0.
1. Verwijder de volgende tabellen:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.CATALOG`
1. De HBase-replicatie starten

## <a name="deprecation"></a>Afschaffing

Apache Storm en ML Services zijn niet beschikbaar in HDInsight 4,0.

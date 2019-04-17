---
title: Opmerkingen bij de release voor Azure HDInsight
description: Meest recente opmerkingen bij de release voor Azure HDInsight. Krijg tips voor ontwikkelaars en details voor Hadoop, Spark, R Server, Hive en meer.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 6ba9e77faeb297f9862b39384d397b478dc7cf36
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617730"
---
# <a name="release-notes-for-azure-hdinsight"></a>Opmerkingen bij de release voor Azure HDInsight

Dit artikel bevat informatie over de **meest recente** LDR-updates voor Azure HDInsight. Zie voor informatie over eerdere versies, [HDInsight Release-opmerkingen bij de Archive](hdinsight-release-notes-archive.md).

> [!IMPORTANT]  
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie, [HDInsight versiebeheer artikel](hdinsight-component-versioning.md).

## <a name="summary"></a>Samenvatting

Azure HDInsight is onder zakelijke klanten een van de meest populaire services voor open source Apache Hadoop- en Apache Spark-analyses in Azure.

## <a name="new-features"></a>Nieuwe functies

Voor meer informatie over belangrijke wijzigingen met HDInsight 4.0., Zie [wat is er nieuw in HDI 4.0?](../hdinsight/hdinsight-version-release.md#whats-new-in-hdi-40).

## <a name="component-versions"></a>Onderdeel-versies

De officiële Apache-versies van alle onderdelen van HDInsight 4.0 worden hieronder vermeld. De onderdelen die worden vermeld, zijn versies van de meest recente stabiele versie beschikbaar.

- Apache Ambari 2.7.1
- Apache Hadoop 3.1.1
- Apache HBase 2.0.0
- Apache Hive 3.1.0
- Apache Kafka 1.1.1
- Apache Mahout 0.9.0+
- Apache Oozie 4.2.0
- Apache Phoenix 4.7.0
- Apache Pig 0.16.0
- Apache Ranger 0.7.0
- Apache Slider 0.92.0
- Apache Spark 2.3.1
- Apache Sqoop 1.4.7
- Apache TEZ 0.9.1 tot en
- Apache Zeppelin 0.8.0
- Apache ZooKeeper 3.4.6

Latere versies van de Apache-onderdelen worden soms gebundeld in de HDP-distributie naast de hierboven vermelde versies. In dit geval deze latere versies worden weergegeven in de tabel Technical Previews en niet voor de versies van de Apache-onderdeel van de bovenstaande lijst in een productieomgeving moeten vervangen.

## <a name="apache-patch-information"></a>Informatie over de Apache beveiligingspatch

Zie voor meer informatie over patches die beschikbaar is in HDInsight 4.0 de patch aanbieding voor elk product in de onderstaande tabel.

| Productnaam | Informatie over de beveiligingspatch |
|---|---|
| Ambari | [Informatie over de Ambari-beveiligingspatch](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.1.0/bk_ambari-release-notes/content/ambari_relnotes-2.7.1.0-patch-information.html) |
| Hadoop | [Informatie over de Hadoop-beveiligingspatch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hadoop.html) |
| HBase | [Informatie over HBase-patch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hbase.html) |
| Hive  | Deze release biedt Hive 3.1.0 met geen extra Apache-patches.  |
| Kafka | Deze release biedt Kafka 1.1.1 met geen extra Apache-patches. |
| Oozie | [Informatie over de Oozie beveiligingspatch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_oozie.html) |
| Phoenix | [Informatie over de Phoenix beveiligingspatch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_phoenix.html) |
| Pig | [Informatie over pig beveiligingspatch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_pig.html) |
| Ranger | [Informatie over de Ranger beveiligingspatch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_ranger.html) |
| Spark | [Informatie over Spark beveiligingspatch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_spark.html) |
| Sqoop | Deze release biedt Sqoop 1.4.7 met geen extra Apache-patches. |
| Tez | Deze release biedt Tez 0.9.1 tot en met geen extra Apache-patches. |
| Zeppelin | Deze release biedt Zeppelin 0.8.0 met geen extra Apache-patches. |
| Zookeeper | [Informatie over zookeeper-beveiligingspatch](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_zookeeper.html) |

## <a name="fixed-common-vulnerabilities-and-exposures"></a>Opgelost Common Vulnerabilities and Exposures

Voor meer informatie over beveiliging problemen zijn opgelost in deze release, Zie Hortonworks [opgelost Common Vulnerabilities and Exposures voor HDP 3.0.1](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/cve.html).

## <a name="known-issues"></a>Bekende problemen

### <a name="replication-is-broken-for-secure-hbase-with-default-installation"></a>Replicatie is verbroken voor HBase beveiligen met standaard-installatie

Voor HDInsight 4.0, voer de volgende stappen uit:

1. Inschakelen van de communicatie tussen clusters.
1. Meld u aan met het actieve hoofdknooppunt.
1. Download een script voor het inschakelen van replicatie met de volgende opdracht:

    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Typ de opdracht `sudo kinit <domainuser>`.
1. Typ de volgende opdracht om uit te voeren van het script:

    ```
    sudo bash hdi_enable_replication.sh -m <hn0> -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```
Voor HDInsight 3.6, het volgende doen:

1. Aanmelden bij active HMaster ZK.
1. Download een script voor het inschakelen van replicatie met de volgende opdracht:
    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Typ de opdracht `sudo kinit -k -t /etc/security/keytabs/hbase.service.keytab hbase/<FQDN>@<DOMAIN>`.
1. Typ de volgende opdracht:

    ```bash
    sudo bash hdi_enable_replication.sh -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```

### <a name="phoenix-sqlline-stops-working-after-migrating-hbase-cluster-to-hdinsight-40"></a>Phoenix Sqlline werkt niet meer na het migreren van HBase-cluster op HDInsight 4.0

Voer de volgende stappen uit:

1. De volgende Phoenix-tabellen verwijderen:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.MUTEX`
    1. `SYSTEM.CATALOG`
1. Als u een van de tabellen niet verwijderen, opnieuw opstarten HBase om te wissen van alle verbindingen met de tabellen.
1. Voer `sqlline.py` opnieuw uit. Phoenix worden opnieuw gemaakt dat alle tabellen die zijn verwijderd in stap 1.
1. Genereer een nieuwe Phoenix-tabellen en weergaven voor uw gegevens in HBase.

### <a name="phoenix-sqlline-stops-working-after-replicating-hbase-phoenix-metadata-from-hdinsight-36-to-40"></a>Phoenix Sqlline werkt niet meer nadat het repliceren van HBase Phoenix metagegevens van HDInsight 3.6 naar 4.0

Voer de volgende stappen uit:

1. Voordat u de replicatie, gaat u naar het doelcluster 4.0 en voer `sqlline.py`. Met deze opdracht genereert Phoenix-tabellen, zoals `SYSTEM.MUTEX` en `SYSTEM.LOG` die alleen zijn opgenomen in 4.0.
1. Verwijder de volgende tabellen:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.CATALOG`
1. De HBase-replicatie starten

## <a name="deprecation"></a>Afschaffing

Apache Storm en ML-services zijn niet beschikbaar is in HDInsight 4.0.

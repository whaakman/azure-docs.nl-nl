---
title: Azure HDInsight versnelde schrijfbewerkingen voor Apache HBase
description: Biedt een overzicht van de functie Azure HDInsight versnelde schrijft, waardoor premium-beheerde schijven gebruikt om de prestaties van de Apache HBase schrijven vooruit logboek te verbeteren.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.date: 4/29/2019
ms.openlocfilehash: 219899c2e336f544ff6572589cc79f84f555490d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233834"
---
# <a name="azure-hdinsight-accelerated-writes-for-apache-hbase"></a>Azure HDInsight versnelde schrijfbewerkingen voor Apache HBase

Dit artikel bevat een achtergrond voor de **versnelde schrijft** schrijfvaardigheden functie voor Apache HBase in Azure HDInsight, en hoe deze effectief kan worden gebruikt om te verbeteren. **Versnelde schrijfbewerkingen** maakt gebruik van [Azure premium SSD beheerde schijven](../../virtual-machines/linux/disks-types.md#premium-ssd) om prestaties van de Apache HBase schrijven vooruit logboek (WAL) te verbeteren. Zie voor meer informatie over Apache HBase, [wat is Apache HBase in HDInsight](apache-hbase-overview.md).

## <a name="overview-of-hbase-architecture"></a>Overzicht van HBase-architectuur

In HBase, een **rij** bestaat uit een of meer **kolommen** en wordt geïdentificeerd door een **rijsleutel**. Meerdere rijen gezamenlijk een **tabel**. De kolommen bevatten **cellen**, die zijn voorzien van een tijdstempel-versies van de waarde in die kolom. Kolommen zijn gegroepeerd in **kolomfamilies**, en alle kolommen in een kolomfamilie samen worden opgeslagen in opslagbestanden met de naam **HFiles**.

**Regio's** in HBase worden gebruikt om taken te verdelen van de gegevensverwerking. HBase slaat eerst de rijen van een tabel in één regio. De rijen worden verdeeld over meerdere regio's als de hoeveelheid gegevens in de tabel wordt verhoogd. **Regioservers** kunnen aanvragen voor meerdere regio's worden verwerkt.

## <a name="write-ahead-log-for-apache-hbase"></a>Ahead van logboek schrijven voor Apache HBase

Gegevensupdates schrijft HBase eerst naar een type doorvoerlogboek een schrijven vooruit logboek (WAL) genoemd. Nadat de update wordt opgeslagen in de WAL, ze worden geschreven naar de in-memory **setSize()** . Wanneer de gegevens in het geheugen de maximale capaciteit bereikt, is geschreven naar de schijf als een **HFile**.

Als een **RegionServer** vastloopt of niet beschikbaar is voordat de geheugenopslag is leeggemaakt, het schrijven vooruit logboek kan worden gebruikt voor de replay van updates. Zonder de WAL als een **RegionServer** vastloopt voordat updates voor een **HFile**, alle updates zijn verloren gaan.

## <a name="accelerated-writes-feature-in-azure-hdinsight-for-apache-hbase"></a>Versnelde schrijfbewerkingen-functie in Azure HDInsight voor Apache HBase

De functie voor versnelde schrijft lost het probleem van hogere schrijven-latentie veroorzaakt door met schrijven vooruit logboeken die zich in opslag in de cloud.  De functie voor versnelde schrijft voor HDInsight Apache HBase-clusters, wordt premium SSD-beheerde schijven voor elke RegionServer (worker-knooppunt). Schrijven dat naar het Hadoop File System (HDFS) gekoppeld aan deze premium-beheerde-schijven in plaats van opslag in de cloud vervolgens door vooruit logboeken worden geschreven.  Premium beheerde schijven gebruiken Solid-State-schijven (SSD's) en bieden uitstekende i/o-prestaties met fouttolerantie.  In tegenstelling tot niet-beheerde schijven als één eenheid voor opslag uitvalt, het heeft geen invloed op andere eenheden voor opslag in dezelfde beschikbaarheidsset.  Beheerde schijven bieden als gevolg hiervan schrijven-wachttijden en betere tolerantie voor uw toepassingen. Zie voor meer informatie over Azure managed disks, [Inleiding tot Azure beheerde schijven](../../virtual-machines/windows/managed-disks-overview.md). 

## <a name="how-to-enable-accelerated-writes-for-hbase-in-hdinsight"></a>Het inschakelen van versnelde schrijft voor HBase in HDInsight

Als u wilt een nieuw HBase-cluster maken met de functie voor versnelde schrijft, volg de stappen in [clusters instellen in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) totdat **stap 3, opslag**. Onder **Metastore-instellingen**, klik op het selectievakje in naast **inschakelen (preview) versnelde schrijft**. Ga vervolgens door met de resterende stappen voor het maken van een cluster.

![Schakel de optie versneld schrijfbewerkingen voor Apache HBase voor HDInsight](./media/apache-hbase-accelerated-writes/accelerated-writes-cluster-creation.png)

## <a name="other-considerations"></a>Andere overwegingen

Als u wilt behouden duurzaamheid van gegevens, moet u een cluster maken met een minimum van drie worker-knooppunten. Nadat u hebt gemaakt, kan niet u het cluster minder dan drie werkknooppunten verkleinen.

Leegmaken- of uitschakelen van de HBase-tabellen voordat u verwijdert het cluster, zodat u toch nog verder logboek schrijven van gegevens.

```
flush 'mytable'
```

```
disable 'mytable'
```

## <a name="next-steps"></a>Volgende stappen

* Officiële documentatie voor Apache HBase op de [functie vooruit logboek schrijven](https://hbase.apache.org/book.html#wal)
* Zie het upgraden van uw HDInsight Apache HBase-cluster voor het gebruik van versnelde schrijft [een Apache HBase-cluster migreren naar een nieuwe versie](apache-hbase-migrate-new-version.md).

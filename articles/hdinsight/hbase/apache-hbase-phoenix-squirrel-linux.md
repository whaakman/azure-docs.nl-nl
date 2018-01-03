---
title: Gebruik van Apache Phoenix en SQLLine met HBase in Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Apache Phoenix in HDInsight. Ook informatie over het installeren en instellen SQLLine op uw computer verbinding maken met een HBase-cluster in HDInsight.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: cda0f33b-a2e8-494c-972f-ae0bb482b818
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/22/2017
ms.author: jgao
ms.openlocfilehash: 70f8786bae555456dd019ad76bda974667cec5ba
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Apache Phoenix gebruiken met HBase op basis van Linux-clusters in HDInsight
Informatie over het gebruik [Apache Phoenix](http://phoenix.apache.org/) in Azure HDInsight en het gebruik van SQLLine. Zie voor meer informatie over Phoenix [Phoenix in 15 minuten of minder](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Zie voor de grammatica Phoenix [Phoenix grammatica](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Zie voor Phoenix versie-informatie over HDInsight [wat is er nieuw in de Hadoop-clusterversies geleverd door HDInsight](../hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>Gebruik SQLLine
[SQLLine](http://sqlline.sourceforge.net/) is een opdrachtregelprogramma SQL uitvoeren.

### <a name="prerequisites"></a>Vereisten
Voordat u SQLLine gebruiken kunt, hebt u de volgende items:

* **Een HBase-cluster in HDInsight**. Als u wilt maken, Zie [aan de slag met Apache HBase in HDInsight](./apache-hbase-tutorial-get-started-linux.md).

Wanneer u verbinding met een HBase-cluster maakt, moet u verbinding maken met een van de ZooKeeper virtuele machines. Elke HDInsight-cluster heeft drie ZooKeeper virtuele machines.

**De hostnaam ZooKeeper ophalen**

1. Ambari openen door te bladeren naar **https://\<clusternaam\>. azurehdinsight.net**.
2. Als u wilt aanmelden, geef de gebruikersnaam voor HTTP (cluster) en het wachtwoord.
3. Selecteer in het menu links **ZooKeeper**. Drie **ZooKeeper Server** exemplaren worden weergegeven.
4. Selecteer een van de **ZooKeeper Server** exemplaren. Op de **samenvatting** deelvenster, zoek de **hostnaam**. Het lijkt op *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**SQLLine gebruiken**

1. Verbinding maken met het cluster via SSH. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Gebruik de volgende opdrachten om uit te voeren SQLLine in SSH:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ZOOKEEPER SERVER FQDN>:2181:/hbase-unsecure
3. Voor het maken van een HBase-tabel en voeg enkele gegeven in, voer de volgende opdrachten:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Zie voor meer informatie de [SQLLine handmatige](http://sqlline.sourceforge.net/#manual) en [Phoenix grammatica](http://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe Apache Phoenix in HDInsight gebruiken. Zie voor meer informatie in deze artikelen:

* [Overzicht van HDInsight HBase][hdinsight-hbase-overview].
  HBase is een Apache, open-source NoSQL-database gebouwd op Hadoop. deze willekeurige toegang en sterke consistentie voor grote hoeveelheden ongestructureerde en semigestructureerde gegevens biedt.
* [HBase-clusters op Azure Virtual Network inrichten][hdinsight-hbase-provision-vnet].
  Met integratie van virtueel netwerk, kunnen HBase-clusters worden geïmplementeerd op hetzelfde virtuele netwerk als uw toepassingen zodat toepassingen rechtstreeks met HBase communiceren kunnen.
* [Configureren van replicatie van HBase in HDInsight](apache-hbase-replication.md). Informatie over het instellen van HBase-replicatie tussen twee Azure-datacenters.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]:apache-hbase-provision-vnet.md
[hdinsight-hbase-overview]:apache-hbase-overview.md



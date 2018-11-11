---
title: Gebruik Apache Phoenix en SQLLine met HBase in Azure HDInsight
description: Leer hoe u Apache Phoenix gebruiken in HDInsight. Ook informatie over het installeren en SQLLine instellen op uw computer verbinding maken met een HBase-cluster in HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/03/2018
ms.author: hrasheed
ms.openlocfilehash: c87596099995cbd34a0ee32ec287de81ec741bda
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51008059"
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Apache Phoenix gebruiken met HBase op basis van Linux-clusters in HDInsight
Meer informatie over het gebruik van [Apache Phoenix](http://phoenix.apache.org/) in Azure HDInsight en SQLLine gebruiken. Zie voor meer informatie over Phoenix [Phoenix in 15 minuten of minder](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Zie voor de grammatica Phoenix [Phoenix grammatica](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Zie voor informatie over HDInsight de Phoenix-versie, [wat is er nieuw in de Hadoop-clusterversies geleverd door HDInsight](../hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>SQLLine gebruiken
[SQLLine](http://sqlline.sourceforge.net/) is een opdrachtregelprogramma voor het uitvoeren van SQL.

### <a name="prerequisites"></a>Vereisten
Voordat u SQLLine gebruiken kunt, moet u de volgende items hebben:

* **Een HBase-cluster in HDInsight**. Zie voor het maken van een [aan de slag met Apache HBase in HDInsight](./apache-hbase-tutorial-get-started-linux.md).

Wanneer u verbinding met een HBase-cluster maakt, moet u verbinding maken met een van de ZooKeeper-VM's. Elke HDInsight-cluster heeft drie ZooKeeper-VM's.

**De naam van de ZooKeeper-host ophalen**

1. Opent Ambari door te bladeren naar **https://\<clusternaam\>. azurehdinsight.net**.
2. Als u wilt aanmelden, voer de gebruikersnaam van HTTP (cluster) en het wachtwoord.
3. Selecteer in het menu links **ZooKeeper**. Drie **ZooKeeper Server** exemplaren worden vermeld.
4. Selecteer een van de **ZooKeeper Server** exemplaren. Op de **samenvatting** in het deelvenster Zoeken de **hostnaam**. Het lijkt op *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**SQLLine gebruiken**

1. Verbinding maken met het cluster met behulp van SSH. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Gebruik de volgende opdrachten om uit te voeren SQLLine in SSH:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ZOOKEEPER SERVER FQDN>:2181:/hbase-unsecure
3. Als u een HBase-tabel maken en voeg enkele gegeven in, voer de volgende opdrachten:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Zie voor meer informatie de [SQLLine handmatig](http://sqlline.sourceforge.net/#manual) en [Phoenix grammatica](http://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u Apache Phoenix gebruiken in HDInsight. Voor meer informatie, Zie de volgende artikelen:

* [Overzicht van HDInsight HBase][hdinsight-hbase-overview].
  HBase is een Apache, open-source NoSQL-database gebouwd op Hadoop. deze willekeurige toegang en sterke consistentie voor grote hoeveelheden ongestructureerde en semigestructureerde gegevens biedt.
* [HBase-clusters inrichten op Azure Virtual Network][hdinsight-hbase-provision-vnet].
  Met integratie van virtuele netwerken, kunnen HBase-clusters worden geïmplementeerd in hetzelfde virtuele netwerk bevinden als uw toepassingen, zodat toepassingen direct met HBase communiceren kunnen.
* [HBase-replicatie configureren in HDInsight](apache-hbase-replication.md). Meer informatie over het instellen van HBase-replicatie in twee datacenters van Azure.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]:apache-hbase-provision-vnet.md
[hdinsight-hbase-overview]:apache-hbase-overview.md



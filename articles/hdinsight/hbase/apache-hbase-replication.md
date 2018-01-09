---
title: Instellen van de replicatie van de HBase-cluster in virtuele netwerken in Azure | Microsoft Docs
description: "Informatie over het instellen van HBase-replicatie van één HDInsight versie naar een andere voor taakverdeling, hoge beschikbaarheid, nul uitvaltijd migratie en -updates en herstel na noodgevallen."
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/03/2018
ms.author: jgao
ms.openlocfilehash: b0a22815dc0bf0ea31e47efe5152498f9aa45de4
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="set-up-hbase-cluster-replication-in-azure-virtual-networks"></a>Instellen van de replicatie van de HBase-cluster in virtuele netwerken in Azure

Informatie over het instellen van de HBase-replicatie binnen een virtueel netwerk of tussen twee virtuele netwerken in Azure.

Replicatie in een cluster maakt gebruik van een bron-push-methode. Een HBase-cluster kan een bron of bestemming, of beide functies tegelijk kunnen worden verwerkt. Replicatie is asynchroon. Het doel van de replicatie is uiteindelijke consistentie. Wanneer de bron een bewerking van een serie kolom ontvangt wanneer replicatie is ingeschakeld, wordt de wijziging wordt doorgegeven aan alle doelclusters. Wanneer gegevens worden gerepliceerd van de ene cluster naar een andere, zijn het broncluster als alle clusters al verbruikt de gegevens worden bijgehouden, om te voorkomen dat replicatie lussen.

In deze zelfstudie maakt instellen u de replicatie van een bron-doel. Zie voor andere clustertopologieën de [Naslaggids voor Apache HBase](http://hbase.apache.org/book.html#_cluster_replication).

Hier volgen de HBase-replicatie gebruik gevallen voor één virtueel netwerk:

* Taakverdeling. U kunt bijvoorbeeld scans of MapReduce-taken uitvoeren op de doelcluster en opnemen van gegevens op de broncluster.
* Toe te voegen met hoge beschikbaarheid.
* Migreren van gegevens van een HBase-cluster naar een andere.
* Een Azure HDInsight-cluster upgraden van één versie naar een andere.

Hier volgen de HBase-replicatie gebruik gevallen voor twee virtuele netwerken:

* Instellen van herstel na noodgevallen.
* Taakverdeling en partitioneren van de toepassing.
* Toe te voegen met hoge beschikbaarheid.

U kunt clusters repliceren met behulp van [script actie](../hdinsight-hadoop-customize-cluster-linux.md) scripts van [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [ophalen van een gratis proefversie van Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="set-up-the-environments"></a>De omgeving instellen

Hebt u drie opties:

- Twee HBase-clusters in een virtuele Azure-netwerk.
- Twee HBase-clusters in twee verschillende virtuele netwerken in dezelfde regio.
- Twee HBase-clusters in twee verschillende virtuele netwerken in twee verschillende regio's (geo-replicatie).

Instellen om u te helpen de omgevingen, wij hebben enkele gemaakt [Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-overview.md). Als u liever voor het instellen van de omgevingen met behulp van andere methoden, Zie:

- [Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
- [HBase-clusters maken in Azure Virtual Network](apache-hbase-provision-vnet.md)

### <a name="set-up-one-virtual-network"></a>Een virtueel netwerk instellen

Twee HBase om clusters te maken in hetzelfde virtuele netwerk, selecteert u de volgende afbeelding. De sjabloon wordt opgeslagen op [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

### <a name="set-up-two-virtual-networks-in-the-same-region"></a>Twee virtuele netwerken in dezelfde regio instellen

Maakt twee virtuele netwerken met peering van virtueel netwerk en twee HBase-clusters in dezelfde regio, selecteert u de volgende afbeelding. De sjabloon wordt opgeslagen op [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>



In dit scenario moeten [virtueel netwerk peering](../../virtual-network/virtual-network-peering-overview.md). De sjabloon kunt virtuele netwerk peering.   

HBase-replicatie maakt gebruik van IP-adressen van de ZooKeeper virtuele machines. U moet instellen statische IP-adressen voor de bestemming HBase ZooKeeper knooppunten.

**Statische IP-adressen configureren**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer op het menu links **resourcegroepen**.
3. Selecteer de resourcegroep die het doel HBase-cluster is. Dit is de resourcegroep die u hebt opgegeven wanneer u de Resource Manager-sjabloon gebruikt om de omgeving te maken. U kunt het filter gebruiken om de lijst. Hier ziet u een lijst met bronnen die de twee virtuele netwerken bevatten.
4. Selecteer het virtuele netwerk waarin het doel HBase-cluster. Selecteer bijvoorbeeld **xxxx vnet2**. Drie apparaten met namen die met beginnen **nic-zookeepermode -** worden vermeld. Deze apparaten zijn de drie ZooKeeper virtuele machines.
5. Selecteer een van de ZooKeeper virtuele machines.
6. Selecteer **IP-configuraties**.
7. Selecteer in de lijst **ipConfig1**.
8. Selecteer **statische**, kopiëren en noteer de werkelijke IP-adres. Tijdens het uitvoeren van de scriptactie replicatie in te schakelen moet u het IP-adres.

  ![HDInsight-HBase-replicatie ZooKeeper statisch IP-adres](./media/apache-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. Herhaal stap 6 instellen van het statische IP-adres voor de andere twee ZooKeeper-knooppunten.

Voor het scenario met meerdere virtuele netwerk, moet u de **- IP-** switch bij het aanroepen van de `hdi_enable_replication.sh` script in te grijpen.

### <a name="set-up-two-virtual-networks-in-two-different-regions"></a>Instellen van twee virtuele netwerken in twee verschillende regio 's

Klik op de volgende afbeelding om twee virtuele netwerken in twee verschillende regio's en de VPN-verbinding tussen de VNets. De sjabloon wordt opgeslagen in [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-geo/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-geo%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

Sommige van de vastgelegde waarden in de sjabloon:

**VNet-1**

| Eigenschap | Waarde |
|----------|-------|
| Locatie | VS - west |
| VNet-naam | &lt;ClusterNamePrevix >-vnet1 |
| Adresruimtevoorvoegsel | 10.1.0.0/16 |
| Subnetnaam | subnet 1 |
| Subnetvoorvoegsel | 10.1.0.0/24 |
| De subnetnaam van het (gateway) | GatewaySubnet (kan niet worden gewijzigd) |
| Subnetvoorvoegsel (gateway) | 10.1.255.0/27 |
| De naam van gateway | vnet1gw |
| Gatewaytype | VPN |
| Type van de VPN-gateway | RouteBased |
| Gateway-SKU | Basic |
| gateway-IP | vnet1gwip |
| Clusternaam | &lt;ClusterNamePrefix > 1 |
| Clusterversie | 3.6 |
| Cluster-type | hbase |
| Aantal cluster worker-knooppunten | 2 |


**VNet 2**

| Eigenschap | Waarde |
|----------|-------|
| Locatie | VS - oost |
| VNet-naam | &lt;ClusterNamePrevix >-vnet2 |
| Adresruimtevoorvoegsel | 10.2.0.0/16 |
| Subnetnaam | subnet 1 |
| Subnetvoorvoegsel | 10.2.0.0/24 |
| De subnetnaam van het (gateway) | GatewaySubnet (kan niet worden gewijzigd) |
| Subnetvoorvoegsel (gateway) | 10.2.255.0/27 |
| De naam van gateway | vnet2gw |
| Gatewaytype | VPN |
| Type van de VPN-gateway | RouteBased |
| Gateway-SKU | Basic |
| gateway-IP | vnet1gwip |
| Clusternaam | &lt;ClusterNamePrefix > 2 |
| Clusterversie | 3.6 |
| Cluster-type | hbase |
| Aantal cluster worker-knooppunten | 2 |

HBase-replicatie maakt gebruik van de IP-adressen van de ZooKeeper virtuele machines. U moet instellen statische IP-adressen voor de bestemming HBase ZooKeeper knooppunten. Als u vaste IP-adres instelt, Zie [instellen twee virtuele netwerken in dezelfde regio](#set-up-two-virtual-networks-in-the-same-region) in dit artikel.

Voor het scenario met meerdere virtuele netwerk, moet u de **- IP-** switch bij het aanroepen van de `hdi_enable_replication.sh` script in te grijpen.

## <a name="load-test-data"></a>Testgegevens laden

Wanneer u een cluster repliceert, moet u de tabellen die u wilt repliceren. In deze sectie kunt u sommige gegevens in de broncluster laden. In de volgende sectie schakelt u replicatie tussen de twee clusters.

Maken van een **contactpersonen** tabel en voeg enkele gegeven in de tabel, volg de instructies voor [HBase-zelfstudie: aan de slag met Apache HBase in HDInsight](apache-hbase-tutorial-get-started-linux.md).

## <a name="enable-replication"></a>Replicatie inschakelen

De volgende stappen wordt beschreven hoe het script actiescript aanroepen vanuit de Azure-portal. Zie voor meer informatie over het uitvoeren van een scriptactie met behulp van Azure PowerShell en het Azure-opdrachtregelprogramma (Azure CLI) [aanpassen HDInsight-clusters met behulp van de scriptactie](../hdinsight-hadoop-customize-cluster-linux.md).

**HBase-replicatie van de Azure-portal in te schakelen**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open de bron HBase-cluster.
3. Selecteer in het menu cluster **scriptacties**.
4. Selecteer boven aan de pagina **nieuwe indienen**.
5. Selecteer of Voer de volgende informatie:

  1. **Naam**: Voer **replicatie inschakelen**.
  2. **Script-URL Bash**: Voer **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.
  3.  **HEAD**: Zorg ervoor dat deze optie is geselecteerd. De knooppunttypen wissen.
  4. **Parameters**: het volgende voorbeeld parameters replicatie inschakelen voor alle bestaande tabellen en kopieert u alle gegevens van de broncluster naar het doelcluster:

          -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata
    
    >[!note]
    >
    > Hostnaam in plaats van FQDN-naam voor de bron- en doelserver cluster DNS-naam gebruiken.

6. Selecteer **Maken**. Het script kan even duren om uit te voeren, vooral wanneer u de **- copydata** argument.

Vereiste argumenten:

|Naam|Beschrijving|
|----|-----------|
|-s,--src-cluster | Hiermee geeft u de DNS-naam van de bron HBase-cluster. Bijvoorbeeld: -s hbsrccluster,--src-cluster hbsrccluster = |
|-d,--zomertijd-cluster | Hiermee geeft u de DNS-naam van het doel (replica) HBase-cluster. Bijvoorbeeld: -s dsthbcluster,--src-cluster dsthbcluster = |
|-sp,--src-ambari-wachtwoord | Hiermee geeft u het beheerderswachtwoord voor Ambari op de bron HBase-cluster. |
|-dp,--zomertijd-ambari-wachtwoord | Hiermee geeft u het beheerderswachtwoord voor Ambari op de doelcluster HBase.|

Optionele argumenten:

|Naam|Beschrijving|
|----|-----------|
|-su,--src-ambari-gebruiker | Hiermee geeft u de gebruikersnaam van de beheerder voor Ambari op de bron HBase-cluster. De standaardwaarde is **admin**. |
|-du,--zomertijd-ambari-gebruiker | Hiermee geeft u de gebruikersnaam van de beheerder voor Ambari op de doelcluster HBase. De standaardwaarde is **admin**. |
|-t,--lijst van de tabel | Hiermee geeft u de tabellen worden gerepliceerd. Bijvoorbeeld:--tabel lijst = 'Tabel1; tabel2; Tabel3'. Als u geen tabellen opgeeft, worden alle bestaande HBase-tabellen gerepliceerd.|
|-m,--machine | Hiermee geeft u het hoofdknooppunt waarop de scriptactie wordt uitgevoerd. De waarde is **hn1** of **hn0**. Omdat de **hn0** hoofdknooppunt is meestal veelgebruikte, wordt u aangeraden **hn1**. Gebruik deze optie als u het script $0 als de scriptactie van een van de HDInsight-portal of Azure PowerShell.|
|-ip | Wanneer u bij het inschakelen van replicatie tussen twee virtuele netwerken vereist. Dit argument fungeert als een overschakelen naar de statische IP-adressen van ZooKeeper-knooppunten uit replica clusters gebruiken in plaats van FQDN-namen. Voordat u replicatie inschakelt, moet u de statische IP-adressen vooraf configureren. |
|-cp - copydata | Hiermee schakelt u de migratie van bestaande gegevens op de tabellen waarvoor replicatie is ingeschakeld. |
|-rpm, -repliceren-phoenix-metagegevens | Hiermee schakelt u replicatie voor Phoenix systeemtabellen. <br><br>*Gebruik deze optie voorzichtig.* U wordt aangeraden Phoenix tabellen op de replica-clusters opnieuw te maken voordat u dit script gebruiken. |
|-h,--help | Geeft informatie over het gebruik. |

De `print_usage()` sectie van de [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) heeft een gedetailleerde beschrijving van parameters.

Nadat de scriptactie is geïmplementeerd, kunt u SSH verbinding maken met het doelcluster HBase en controleer vervolgens of de gegevens zijn gerepliceerd.

### <a name="replication-scenarios"></a>Replicatie-scenario 's

De volgende lijst ziet u enkele gevallen algemeen gebruik en de bijbehorende parameterinstellingen:

- **Replicatie inschakelen voor alle tabellen tussen twee clusters**. Dit scenario vereist geen kopiëren of migreren van bestaande gegevens in de tabellen en gebruikt geen Phoenix tabellen. Gebruik de volgende parameters:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Replicatie inschakelen voor specifieke tabellen**. Schakel replicatie op table1 en table2 Tabel3 door de volgende parameters gebruiken:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Replicatie inschakelen voor specifieke tabellen en kopieer de bestaande gegevens**. Schakel replicatie op table1 en table2 Tabel3 door de volgende parameters gebruiken:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Replicatie inschakelen voor alle tabellen en repliceren Phoenix metagegevens van bron naar doel**. Phoenix metagegevens replicatie is niet ideaal. Deze voorzichtig gebruiken. Gebruik de volgende parameters:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Kopiëren en gegevens migreren

Er zijn twee afzonderlijke script actie scripts beschikbaar zijn voor het kopiëren of gegevens migreren nadat replicatie is ingeschakeld:

- [Script voor kleine tabellen](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (tabellen die zijn enkele in grootte en de algehele kopie is voltooid in minder dan een uur verwacht)

- [Script voor grote tabellen](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (tabellen die naar verwachting langer duurt dan een uur kopiëren)

Kunt u dezelfde procedure die wordt beschreven in [replicatie inschakelen](#enable-replication) de scriptactie niet aanroepen. Gebruik de volgende parameters:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

De `print_usage()` sectie van de [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) is een gedetailleerde beschrijving van parameters.

### <a name="scenarios"></a>Scenario's

- **Specifieke tabellen (test1, test2 en test3) voor alle rijen bewerkt tot op heden kopiëren (huidige tijdstempel)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  Of:

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Kopiëren van specifieke tabellen met een opgegeven tijdperiode**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Schakel replicatie uit

Als replicatie wilt uitschakelen, gebruikt u een ander script actiescript uit [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). Kunt u dezelfde procedure die wordt beschreven in [replicatie inschakelen](#enable-replication) de scriptactie niet aanroepen. Gebruik de volgende parameters:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> <-all|-t "table1;table2;...">  

De `print_usage()` sectie van de [script](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) heeft een gedetailleerde beschrijving van parameters.

### <a name="scenarios"></a>Scenario's

- **Schakel replicatie op alle tabellen**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  of

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari user name> --src-ambari-password=<source cluster Ambari password>

- **Schakel replicatie op de opgegeven tabellen (Tabel1, tabel2 en Tabel3)**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de HBase-replicatie binnen een virtueel netwerk of tussen twee virtuele netwerken instellen. Zie voor meer informatie over HDInsight en HBase, deze artikelen:

* [Aan de slag met Apache HBase in HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Overzicht van HDInsight HBase](./apache-hbase-overview.md)
* [HBase-clusters maken in Azure Virtual Network](./apache-hbase-provision-vnet.md)


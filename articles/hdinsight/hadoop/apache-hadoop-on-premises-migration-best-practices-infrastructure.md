---
title: On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - best practices voor infrastructuur
description: Informatie over aanbevolen procedures van de infrastructuur voor de migratie on-premises Hadoop-clusters op Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: a1dbc04a83e80281fb6cd516c546c1dddff6db37
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50413792"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---infrastructure-best-practices"></a>On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - best practices voor infrastructuur

In dit artikel biedt aanbevelingen voor het beheren van de infrastructuur van Azure HDInsight-clusters. Het onderdeel van een serie die biedt best practices om u te helpen migreren on-premises Apache Hadoop-systemen tot Azure HDInsight.

## <a name="plan-well-for-the-capacity-needed-for-hdinsight-clusters"></a>Plan voor de capaciteit die nodig zijn voor HDInsight-clusters

De belangrijkste opties om u te maken voor de capaciteitsplanning voor HDInsight-cluster zijn als volgt:

- **Kies de regio** -de Azure-regio bepaalt waar het cluster fysiek is ingericht. Om te beperken de latentie van lees- en schrijfbewerkingen, moet het cluster zich in dezelfde regio als de gegevens.
- **Kies opslaglocatie en grootte** -standaardopslag moet zich in dezelfde regio bevinden als het cluster. Voor een cluster 48-knooppunt, is het aanbevolen dat 4 tot en met 8 storage-accounts. Hoewel er mogelijk al voldoende totale opslag, biedt elk opslagaccount aanvullende netwerken bandbreedte voor de compute-knooppunten. Wanneer er meerdere opslagaccounts, gebruikt u een willekeurige naam voor de storage-account zonder een voorvoegsel. Het doel van een willekeurige naam geven, is de kans op opslag knelpunten (beperking) of gemeenschappelijke-modus fouten verminderen voor alle accounts. Voor betere prestaties, gebruik slechts één container per opslagaccount.
- **Kies de VM-grootte en hetzelfde type (ondersteunt nu de G-serie)** : elk clustertype bevat een set knooppunttypen en elk knooppunttype heeft specifieke opties voor de VM-grootte en hetzelfde type. De VM-grootte en het type wordt bepaald door de CPU-verwerking van kracht, RAM-geheugen en de netwerklatentie. Een gesimuleerde werkbelasting kan worden gebruikt om te bepalen van de optimale VM-grootte en het type voor elk knooppunt.
- **Het aantal worker-knooppunten kiezen** -het oorspronkelijke aantal worker-knooppunten kan worden bepaald met behulp van de gesimuleerde werkbelasting. Het cluster kan later worden geschaald door meer worker-knooppunten om te voldoen aan de piekvraag load toe te voegen. Het cluster kan later worden geschaald wanneer de extra worker-knooppunten niet vereist zijn.

Zie voor meer informatie het artikel [plannen van capaciteit voor HDInsight-clusters](../hdinsight-capacity-planning.md)

## <a name="use-the-recommended-virtual-machine-types-for-cluster-nodes"></a>Gebruik de aanbevolen VM-typen voor clusterknooppunten

Zie [standaard configuratie en de virtuele machine knooppuntgrootten voor clusters](../hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) aanbevolen voor de typen virtuele machines voor elk type van HDInsight-cluster.

## <a name="check-the-availability-of-hadoop-components-in-hdinsight"></a>Controleer de beschikbaarheid van Hadoop-onderdelen in HDInsight

Elke versie van HDInsight is een cloud-distributie van een versie van Hortonworks Data Platform (HDP) en bestaat uit een verzameling van onderdelen van Hadoop-ecosysteem. Zie [versiebeheer van HDInsight-onderdeel](../hdinsight-component-versioning.md) voor meer informatie over alle HDInsight-onderdelen en hun huidige versies.

U kunt ook Ambari UI of de Ambari REST-API gebruiken om te controleren of de Hadoop-onderdelen en versies in HDInsight.

Toepassingen of onderdelen die beschikbaar waren in clusters on-premises, maar geen deel uitmaken van de HDInsight-clusters kunnen worden toegevoegd op een Edge-knooppunt of op een virtuele machine in hetzelfde VNet als de HDInsight-cluster. Een derde partij Hadoop-toepassing die is niet beschikbaar in Azure HDInsight kan worden geïnstalleerd met de optie 'Application' in HDInsight-cluster. Aangepaste Hadoop-toepassingen kunnen worden geïnstalleerd op HDInsight-cluster met behulp van 'scriptacties'. De volgende tabel bevat enkele van de algemene toepassingen en hun HDInsight-integratie-opties:

|**Toepassing**|**Integratie**
|---|---|
|Luchtstroom|IaaS of HDInsight Edge-knooppunt
|Alluxio|IaaS  
|Arcadia|IaaS 
|Atlas|Geen (alleen HDP)
|Datameer|HDInsight-Edge-knooppunt
|Datastax (Cassandra)|IaaS (CosmosDB een alternatief in Azure)
|DataTorrent|IaaS 
|Drill|IaaS 
|Ignite|IaaS
|Jethro|IaaS 
|Mapador|IaaS 
|Mongo|IaaS (CosmosDB een alternatief in Azure)
|NiFi|IaaS 
|Presto|IaaS of HDInsight Edge-knooppunt
|Python 2|PaaS 
|Python 3|PaaS 
|R|PaaS 
|SAS|IaaS 
|Vertica|IaaS (RESOURCEKLASSE alternatief op Azure)
|Tableau|IaaS 
|Waterlijn|HDInsight-Edge-knooppunt
|StreamSets|HDInsight-Edge 
|Palantir|IaaS 
|Sailpoint|Iaas 

Zie voor meer informatie het artikel [beschikbaar met verschillende versies van HDInsight Hadoop-onderdelen](../hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions)

## <a name="customize-hdinsight-clusters-using-script-actions"></a>HDInsight clusters aanpassen met scriptacties

HDInsight biedt een methode voor configuratie van het cluster met de naam van een **script actie**. Een scriptactie is Bash-script dat wordt uitgevoerd op de knooppunten in een HDInsight-cluster en kan worden gebruikt om extra onderdelen installeren en configuratie-instellingen wijzigen.

Scriptacties moeten worden opgeslagen op een URI die toegankelijk is vanaf het HDInsight-cluster. Ze kunnen worden gebruikt tijdens of na het maken van clusters en kunnen ook worden beperkt tot alleen op bepaalde knooppunttypen worden uitgevoerd.

Het script kan worden opgeslagen of één keer uitgevoerd. De persistente scripts worden gebruikt voor het aanpassen van de nieuwe werkrolknooppunten toegevoegd aan het cluster via het schalen herverdelen. Een persistent script kan ook wijzigingen toepassen op een ander knooppunttype, zoals een hoofdknooppunt, wanneer vergroten / verkleinen optreden.

HDInsight biedt vooraf geschreven scripts voor het installeren van de volgende onderdelen in HDInsight-clusters:

- Een Azure Storage-account toevoegen
- Hue installeren
- Presto installeren
- Solr installeren
- Giraph installeren
- Hive-bibliotheken vooraf laden
- Mono installeren of bijwerken

> [!Note]
> HDInsight biedt geen rechtstreekse ondersteuning voor aangepaste hadoop-onderdelen of onderdelen worden geïnstalleerd met behulp van scriptacties.

Scriptacties kunnen ook in de Azure Marketplace worden gepubliceerd als een HDInsight-toepassing.

Raadpleeg voor meer informatie de volgende artikelen:

- [Hadoop-toepassingen van derden installeren op HDInsight](../hdinsight-apps-install-applications.md)
- [HDInsight clusters aanpassen met scriptacties](../hdinsight-hadoop-customize-cluster-linux.md)
- [Een HDInsight-toepassing publiceren in de Azure Marketplace](../hdinsight-apps-publish-applications.md)

## <a name="customize-hdinsight-configs-using-bootstrap"></a>Aanpassen met Bootstrap HDInsight-configuraties

Wijzigingen in configuraties in de configuratiebestanden, zoals `core-site.xml`, `hive-site.xml` en `oozie-env.xml` kunnen worden gemaakt met Bootstrap. Het volgende script is een voorbeeld met behulp van Powershell:

```powershell
# hive-site.xml configuration
$hiveConfigValues = @{"hive.metastore.client.socket.timeout"="90"}

$config = New—AzureRmHDInsightClusterConfig '
    | Set—AzureRmHDInsightDefaultStorage
    —StorageAccountName "$defaultStorageAccountName.blob. core.windows.net" `
    —StorageAccountKey "defaultStorageAccountKey " `
    | Add—AzureRmHDInsightConfigValues `
        —HiveSite $hiveConfigValues

New—AzureRmHDInsightCluster `
    —ResourceGroupName $existingResourceGroupName `
    —Cluster-Name $clusterName `
    —Location $location `
    —ClusterSizeInNodes $clusterSizeInNodes `
    —ClusterType Hadoop `
    —OSType Linux `
    —Version "3.6" `
    —HttpCredential $httpCredential `
    —Config $config
```

Zie voor meer informatie het artikel [aanpassen HDInsight-clusters met Bootstrap](../hdinsight-hadoop-customize-cluster-bootstrap.md)

## <a name="use-edge-nodes-on-hadoop-clusters-in-hdinsight-to-access-the-client-tools"></a>Edge-knooppunten op Hadoop-clusters in HDInsight gebruiken voor toegang tot de clienthulpprogramma 's

Een lege edge-knooppunt is een Linux-machine met de dezelfde clienthulpprogramma's geïnstalleerd en geconfigureerd als op de hoofdknooppunten, maar met geen Hadoop-services die worden uitgevoerd. Het edge-knooppunt kan worden gebruikt voor de volgende doeleinden:

- toegang tot het cluster
- clienttoepassingen testen
- clienttoepassingen die als host fungeert

Edge-knooppunten kunnen worden gemaakt en verwijderd via de Azure-portal en kunnen worden gebruikt tijdens of na het cluster maken. Nadat het edge-knooppunt is gemaakt, kunt u verbinding maken met het edge-knooppunt met behulp van SSH en clienthulpprogramma's voor toegang tot het Hadoop-cluster in HDInsight worden uitgevoerd. Het edge-knooppunt ssh-eindpunt is `<EdgeNodeName>.<ClusterName>-ssh.azurehdinsight.net:22`.

Zie voor meer informatie het artikel [lege edge-knooppunten op Hadoop-clusters in HDInsight gebruiken](../hdinsight-apps-use-edge-node.md)

## <a name="use-the-scale-up-and-scale-down-feature-of-clusters"></a>Gebruik de functie omhoog en omlaag schalen van clusters

HDInsight biedt flexibiliteit doordat u de optie voor omhoog en omlaag het aantal worker-knooppunten in uw clusters schalen. Deze functie kunt u een cluster verkleinen na uur of in het weekend en vouw dit uit tijdens piek bedrijfsbehoeften te voldoen.

Clusterschaling kan worden geautomatiseerd met behulp van de volgende methoden:

### <a name="powershell-cmdlet"></a>PowerShell-cmdlet

```powershell
Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```

### <a name="azure-cli"></a>Azure-CLI

```powershell
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```

### <a name="azure-portal"></a>Azure Portal

Als u knooppunten aan uw actieve HDInsight-cluster toevoegt, worden alle taken die in behandeling of wordt uitgevoerd niet beïnvloed. Nieuwe taken kunnen veilig worden verzonden terwijl het proces voor vergroten/verkleinen wordt uitgevoerd. Als de vergroten / verkleinen voor een bepaalde reden mislukken, wordt de fout probleemloos uitgevoerd, verlaat de cluster in een functionele staat.

Echter, als u uw cluster verkleint bent door het verwijderen van knooppunten, eventuele in behandeling of actieve taken mislukken wanneer de vergroten/verkleinen bewerking is voltooid. Deze fout wordt veroorzaakt door een van de services opnieuw te starten tijdens het proces. Om dit probleem op te lossen, kunt u de taken zijn voltooid voordat u uw cluster verkleint, handmatig de taken beëindigd of de taken opnieuw nadat de vergroten/verkleinen bewerking heeft de diagnose afgerond wachten.

Als u uw cluster naar het minimum van een worker-knooppunt verkleinen, zijn HDFS vastgelopen in de veilige modus als de worker-knooppunten opnieuw worden opgestart voor het patchen van of onmiddellijk nadat de vergroten/verkleinen is uitgevoerd. U kunt de volgende opdracht uit om HDFS buiten de veilige modus uitvoeren:

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

Na de veilige modus te verlaten, kunt u handmatig de tijdelijke bestanden verwijderen of wacht Hive uiteindelijk opschonen ze automatisch.

Zie voor meer informatie het artikel [schaal HDInsight-clusters](../hdinsight-scaling-best-practices.md)

## <a name="use-hdinsight-with-azure-virtual-network"></a>Gebruik HDInsight met Azure Virtual Network

Virtuele netwerken van Azure inschakelen Azure-resources, zoals Azure Virtual Machines, veilig kunt communiceren met elkaar, internet, en on-premises netwerken, door te filteren en routering van netwerkverkeer.

Met behulp van Azure Virtual Network met HDInsight kunt de volgende scenario's:

- Verbinding maken met HDInsight rechtstreeks vanuit een on-premises netwerk.
- Verbinding maken met HDInsight gegevens opslaat in een Azure-netwerk.
- Rechtstreeks toegang hebben tot Hadoop-services die niet openbaar beschikbaar zijn via internet. Bijvoorbeeld, Kafka-API's of de HBase-Java-API.

HDInsight kan ofwel worden toegevoegd aan een nieuwe of bestaande Azure Virtual Network. Als HDInsight wordt toegevoegd aan een bestaand Virtueelnetwerk, de bestaande netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes moeten worden bijgewerkt voor onbeperkte toegang tot [verschillende IP-adressen](../hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip-1) in de Azure-Datacenter. Zorg er ook niet op het blokkeren van verkeer naar de [poorten](../hdinsight-extend-hadoop-virtual-network.md#hdinsight-ports) die worden gebruikt door HDInsight-services.

> [!Note]
> HDInsight ondersteunt momenteel geen geforceerde tunneling. Geforceerde tunneling is een subnetinstelling die ervoor zorgt het uitgaande internetverkeer met een apparaat voor controle dat en logboekregistratie. Verwijder geforceerde tunnels zijn voordat het installeren van HDInsight in een subnet of een nieuw subnet maken voor HDInsight. HDInsight ook ondersteunt geen uitgaande netwerkconnectiviteit te beperken.

Raadpleeg voor meer informatie de volgende artikelen:

- [Virtuele-netwerken-overzicht van Azure](../../virtual-network/virtual-networks-overview.md)
- [Azure HDInsight met behulp van een Azure-netwerk uitbreiden](../hdinsight-extend-hadoop-virtual-network.md)

## <a name="use-azure-virtual-network-service-endpoints-to-securely-connect-to-other-azure-services"></a>Gebruik Azure Virtual Network service-eindpunten veilig verbinding maken met andere Azure-services

HDInsight ondersteunt [virtual network-service-eindpunten](../../virtual-network/virtual-network-service-endpoints-overview.md) waarmee u veilig verbinding maken met Azure Blob Storage, Azure Data Lake Storage Gen2, Cosmos DB en SQL-databases. Als u een Service-eindpunt inschakelt voor Azure HDInsight, wordt verkeer stroomt via een beveiligde route van binnen het Azure-datacentrum. U kunt met deze uitgebreide niveau van beveiliging op de netwerklaag, big data storage-accounts aan de opgegeven virtuele netwerken (VNETs) vergrendelen en HDInsight-clusters nog steeds naadloos toegang tot gegevens en verwerkt die gebruiken.

Raadpleeg voor meer informatie de volgende artikelen:

- [Service-eindpunten voor virtueel netwerk](../../virtual-network/virtual-network-service-endpoints-overview.md)
- [Verbeter de beveiliging van HDInsight met service-eindpunten](https://azure.microsoft.com/blog/enhance-hdinsight-security-with-service-endpoints/.md)

## <a name="connect-hdinsight-to-the-on-premises-network"></a>HDInsight verbinden met de on-premises netwerk

HDInsight kan worden verbonden met de on-premises netwerk met behulp van Azure Virtual Networks en een VPN-gateway. De volgende stappen kunnen worden gebruikt om verbinding te maken:

- Gebruik HDInsight in een Azure Virtual Network die verbonden met de on-premises netwerk is.
- Configureer DNS-naamomzetting tussen het virtuele netwerk en de on-premises netwerk.
- Netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes (UDR) voor het beheren van netwerkverkeer configureren.

Zie voor meer informatie het artikel [verbinding maken met HDInsight op uw on-premises netwerk](../connect-on-premises-network.md)

## <a name="next-steps"></a>Volgende stappen

Lees het volgende artikel in deze reeks:

- [Aanbevolen procedures van de opslag voor on-premises naar Azure HDInsight Hadoop-migratie](apache-hadoop-on-premises-migration-best-practices-storage.md)
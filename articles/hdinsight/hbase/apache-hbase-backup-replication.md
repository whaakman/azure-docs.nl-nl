---
title: Instellen van Apache HBase- en Apache Phoenix back-up en replicatie - Azure HDInsight
description: Back-up en replicatie voor HBase en Phoenix instellen.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: d182d23bf4b3f4dc1ed42a737e8fe8b753c035ae
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56340735"
---
# <a name="set-up-backup-and-replication-for-apache-hbase-and-apache-phoenix-on-hdinsight"></a>Instellen van back-up en replicatie voor Apache HBase- en Apache Phoenix op HDInsight

Apache HBase ondersteunt diverse benaderingen voor het beveiligen tegen verlies van gegevens:

* Kopieer de `hbase` map
* Importeren exporteren
* Tabellen kopiëren
* Momentopnamen
* Replicatie

> [!NOTE]  
> Apache Phoenix slaat de metagegevens in HBase-tabellen, zodat de metagegevens van de back-up wanneer u back-up van de catalogus van HBase systeemtabellen.

De volgende secties beschrijven het gebruiksscenario voor elk van deze methoden.

## <a name="copy-the-hbase-folder"></a>De hbase-map kopiëren

Met deze methode kunt u alle gegevens in HBase, zonder dat de mogelijkheid om te selecteren van een subset van tabellen en kolomfamilies kopiëren. Volgende benaderingen kunnen beheersen.

HBase in HDInsight maakt gebruik van de standaardopslag geselecteerd bij het maken van het cluster, Azure Storage-blobs of Azure Data Lake-opslag. HBase slaat in beide gevallen moet de gegevens en metagegevens van bestanden via het volgende pad:

    /hbase

* In een Azure Storage-account de `hbase` zich bevindt in de hoofdmap van de blob-container:

    ```
    wasbs://<containername>@<accountname>.blob.core.windows.net/hbase
    ```

* In Azure Data Lake Storage de `hbase` zich bevindt in het hoofdpad die u hebt opgegeven tijdens het inrichten van een cluster. Het pad naar deze hoofdmap is doorgaans een `clusters` map een submap met de naam van uw HDInsight-cluster:

    ```
    /clusters/<clusterName>/hbase
    ```

In beide gevallen moet de `hbase` map bevat alle gegevens die door HBase is leeggemaakt naar de schijf, maar deze bevat mogelijk niet de gegevens in het geheugen. Voordat u op deze map als een nauwkeurige weergave van de HBase-gegevens vertrouwen kunt, moet u de cluster afsluiten.

Nadat u het cluster verwijdert, kunt u laat u de gegevens op locatie of de gegevens kopiëren naar een nieuwe locatie:

* Maak een nieuw HDInsight-exemplaar dat verwijst naar de huidige opslaglocatie. Het nieuwe exemplaar wordt gemaakt met de bestaande gegevens.

* Kopieer de `hbase` map naar een andere Azure Storage blob-container of de locatie van de Data Lake-opslag en start vervolgens een nieuw cluster met die gegevens. Gebruik voor Azure Storage, [AzCopy](../../storage/common/storage-use-azcopy.md), en voor het gebruik van Data Lake Storage [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md).

## <a name="export-then-import"></a>Importeren exporteren

Op het bron-HDInsight-cluster, gebruikt u de Export-hulpprogramma (opgenomen in HBase) voor het exporteren van gegevens uit een brontabel naar de standaardopslag die is gekoppeld. U kunt vervolgens de geëxporteerde map kopiëren naar de doellocatie voor opslag en uitvoeren van het hulpprogramma voor importeren op de bestemming HDInsight-cluster.

Een tabel, eerste SSH naar het hoofdknooppunt van het HDInsight-broncluster exporteren en voer daarna het volgende `hbase` opdracht:

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>"

Om te importeren een tabel, SSH naar het hoofdknooppunt van het doel HDInsight-cluster en voer daarna het volgende `hbase` opdracht:

    hbase org.apache.hadoop.hbase.mapreduce.Import "<tableName>" "/<path>/<to>/<export>"

Geef het pad van de volledige uitvoer naar de standaardopslag of naar een van de gekoppelde opslagopties. Bijvoorbeeld, in Azure Storage:

    wasbs://<containername>@<accountname>.blob.core.windows.net/<path>

In Azure Data Lake Storage Gen2 is de syntaxis:

    abfs://<containername>@<accountname>.dfs.core.windows.net/<path>

In Azure Data Lake Storage Gen1 is de syntaxis:

    adl://<accountName>.azuredatalakestore.net:443/<path>

Deze aanpak op tabelniveau granulatie. U kunt ook een datumbereik voor de rijen die u wilt opnemen, zodat u kunt het uitvoeren van het proces stapsgewijs opgeven. Elke datum is in milliseconden sinds de Unix-epoche.

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>" <numberOfVersions> <startTimeInMS> <endTimeInMS>

Houd er rekening mee dat u hebt om op te geven van het aantal versies van elke rij te exporteren. Instellen om op te nemen alle versies in het datumbereik, `<numberOfVersions>` op een waarde groter dan de maximaal mogelijke rij-versies, zoals 100000.

## <a name="copy-tables"></a>Tabellen kopiëren

Het hulpprogramma CopyTable kopieert gegevens van een brontabel, per rij naar een bestaande doeltabel met hetzelfde schema als de bron. De doeltabel kan zich op hetzelfde cluster of een ander HBase-cluster.

Om te gebruiken CopyTable binnen een cluster, SSH naar het hoofdknooppunt van het bron-HDInsight-cluster en voer dit `hbase` opdracht:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> <srcTableName>

Als u CopyTable wilt kopiëren naar een tabel in een ander cluster, toevoegen de `peer` overschakelen met het doelcluster adres:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> --peer.adr=<destinationAddress> <srcTableName>

Het doeladres bestaat uit de volgende drie onderdelen:

    <destinationAddress> = <ZooKeeperQuorum>:<Port>:<ZnodeParent>

* `<ZooKeeperQuorum>` is een door komma's gescheiden lijst van Apache ZooKeeper-knooppunten, bijvoorbeeld:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net

* `<Port>` op HDInsight standaard ingesteld op 2181, en `<ZnodeParent>` is `/hbase-unsecure`, dus de volledige `<destinationAddress>` zou zijn:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net:2181:/hbase-unsecure

Zie [handmatig verzamelen van de lijst met Apache ZooKeeper-Quorum](#manually-collect-the-apache-zookeeper-quorum-list) in dit artikel voor meer informatie over het ophalen van deze waarden voor uw HDInsight-cluster.

Het hulpprogramma CopyTable biedt ook ondersteuning voor parameters om op te geven van het tijdsbereik van de rijen te kopiëren, en om op te geven van de subset met kolomfamilies in een tabel om te kopiëren. Als u wilt zien van de volledige lijst met parameters die worden ondersteund door CopyTable, CopyTable worden uitgevoerd zonder parameters:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable

CopyTable scant de volledige tabel broninhoud die worden gekopieerd naar de doeltabel. Hierdoor kunnen de prestaties van uw HBase-cluster terwijl CopyTable wordt uitgevoerd.

> [!NOTE]  
> Voor het automatiseren van het kopiëren van gegevens tussen tabellen, Zie de `hdi_copy_table.sh` script in de [Azure HBase Utils](https://github.com/Azure/hbase-utils/tree/master/replication) -bibliotheek op GitHub.

### <a name="manually-collect-the-apache-zookeeper-quorum-list"></a>Handmatig verzamelen van de Apache ZooKeeper-quorum lijst

Wanneer beide HDInsight-clusters zich in hetzelfde virtuele netwerk, zoals eerder beschreven, is het omzetten van de interne hostnaam automatische. Voor het gebruik van CopyTable voor HDInsight-clusters in twee afzonderlijke virtuele netwerken die zijn verbonden met een VPN-Gateway, moet u IP-adressen van de Zookeeper-knooppunten in het quorum voor de hostopslag verzorgen.

Voor het verkrijgen van de quorum-hostnamen, voer de volgende curl-opdracht:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/configurations?type=hbase-site&tag=TOPOLOGY_RESOLVED" | grep "hbase.zookeeper.quorum"

De curl-opdracht wordt een JSON-document met HBase-configuratie-informatie opgehaald en wordt de grep-opdracht retourneert alleen de vermelding 'hbase.zookeeper.quorum', bijvoorbeeld:

    "hbase.zookeeper.quorum" : "zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net"

De waarde van quorum host namen is de gehele tekenreeks aan de rechterkant van de dubbele punt.

Als u wilt ophalen van de IP-adressen voor deze hosts, gebruikt u de volgende curl-opdracht voor elke host in de vorige lijst:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/hosts/<zookeeperHostFullName>" | grep "ip"

In deze opdracht curl `<zookeeperHostFullName>` is de volledige DNS-naam van een ZooKeeper-host, zoals in het voorbeeld `zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net`. De uitvoer van de opdracht bevat bijvoorbeeld het IP-adres voor de opgegeven host:

    100    "ip" : "10.0.0.9",

Nadat u de IP-adressen voor alle ZooKeeper-knooppunten in uw quorumconfiguratie hebt verzameld, opnieuw het doeladres:

    <destinationAddress>  = <Host_1_IP>,<Host_2_IP>,<Host_3_IP>:<Port>:<ZnodeParent>

In ons voorbeeld:

    <destinationAddress> = 10.0.0.9,10.0.0.8,10.0.0.12:2181:/hbase-unsecure

## <a name="snapshots"></a>Momentopnamen

Momentopnamen kunnen u een point-in-time-up van gegevens nemen in uw HBase-gegevensopslag. Momentopnamen zijn minimale overhead en binnen enkele seconden worden voltooid omdat de momentopnamebewerking van een is in feite een metadata-bewerking voor het vastleggen van de namen van alle bestanden in de opslag op dat moment. Op het moment van een momentopname, worden geen daadwerkelijke gegevens gekopieerd. Momentopnamen zijn afhankelijk van de onveranderbare aard van de gegevens die zijn opgeslagen in HDFS, waarbij updates, verwijderingen en invoegingen alle als nieuwe gegevens weergegeven worden. U kunt herstellen (*kloon*) een momentopname op hetzelfde cluster of een momentopname exporteren naar een ander cluster.

Maken van een momentopname, SSH in met het hoofdknooppunt van uw HDInsight-HBase-cluster en start de `hbase` shell:

    hbase shell

In de hbase-shell, gebruikmaken van de momentopname-opdracht met de namen van de tabel en deze momentopname:

    snapshot '<tableName>', '<snapshotName>'

Om terug te zetten van een momentopname met de naam binnen de `hbase` shell, eerst uitschakelen van de tabel, en vervolgens de momentopname herstellen en in de tabel opnieuw in te schakelen:

    disable '<tableName>'
    restore_snapshot '<snapshotName>'
    enable '<tableName>'

Als u een momentopname herstellen naar een nieuwe tabel, gebruikt u clone_snapshot:

    clone_snapshot '<snapshotName>', '<newTableName>'

Voor het exporteren van een momentopname op HDFS voor gebruik door een ander cluster, maakt u eerst de momentopname zoals eerder beschreven en gebruik vervolgens het hulpprogramma ExportSnapshot. Voer dit hulpprogramma uit binnen de SSH-sessie met het hoofdknooppunt niet binnen de `hbase` shell:

     hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot <snapshotName> -copy-to <hdfsHBaseLocation>

De `<hdfsHBaseLocation>` kan dit een van de opslaglocaties toegankelijk is voor uw broncluster en moet verwijzen naar de hbase-map die wordt gebruikt door het doelcluster. Als u een secundaire Azure Storage-account dat is gekoppeld aan uw broncluster hebt en dat account toegang tot de container die wordt gebruikt door de standaardopslag van het doelcluster opstart biedt, kan u bijvoorbeeld deze opdracht gebruiken:

    hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot 'Snapshot1' -copy-to 'wasbs://secondcluster@myaccount.blob.core.windows.net/hbase'

Nadat de momentopname is geëxporteerd, wordt SSH naar het hoofdknooppunt van het doelcluster opstart en herstel momentopname met de restore_snapshot opdracht als eerder beschreven.

Momentopnamen bieden een volledige back-up van een tabel op het moment van de `snapshot` opdracht. Momentopnamen bieden de mogelijkheid om uit te voeren van incrementele momentopnamen door windows tijd wordt opgelost, noch om op te geven subsets van kolommen families om op te nemen in de momentopname.

## <a name="replication"></a>Replicatie

HBase-replicatie pushes automatisch transacties uit een broncluster van naar een bestemming-cluster, met behulp van een mechanisme voor asynchrone met een minimale overhead op het broncluster. In HDInsight, kunt u replicatie tussen clusters instellen waarbij:

* De bron- en -clusters zich in hetzelfde virtuele netwerk.
* De bron- en bestemmingen clusters zich in verschillende virtuele netwerken die zijn verbonden via een VPN-gateway, maar beide clusters bestaan in dezelfde geografische locatie.
* Het broncluster en bestemmingen clusters zich in verschillende virtuele netwerken die zijn verbonden via een VPN-gateway en elk cluster in een andere geografische locatie bestaat.

De algemene stappen voor het instellen van replicatie zijn:

1. Op de broncluster, de tabellen maken en gegevens invullen.
2. Op de doelcluster door lege doeltabellen te maken met de brontabel schema.
3. Meld u aan het doelcluster als een peer-to-het broncluster.
4. Schakel replicatie op de gewenste brontabellen.
5. Bestaande gegevens kopiëren van de brontabellen naar de doeltabellen.
6. Replicatie kopieert automatisch nieuwe gegevens worden aangebracht aan de brontabellen in doeltabellen.

U schakelt replicatie in HDInsight, een scriptactie van toepassing op uw actieve bron HDInsight-cluster. Zie voor een overzicht van het inschakelen van replicatie in uw cluster, of om te experimenteren met replicatie op de voorbeeld-clusters die in virtuele netwerken met behulp van Azure Resource Management-sjablonen zijn gemaakt, [configureren Apache HBase-replicatie](apache-hbase-replication.md). Dit artikel bevat ook instructies voor het inschakelen van replicatie van Phoenix-metagegevens.

## <a name="next-steps"></a>Volgende stappen

* [Apache HBase-replicatie configureren](apache-hbase-replication.md)

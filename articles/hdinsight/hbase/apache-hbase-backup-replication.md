---
title: HBase en Phoenix back-up- en replicatie - Azure HDInsight instellen | Microsoft Docs
description: Back-up- en replicatie voor HBase en Phoenix instellen.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: f43edaf16784e5ba5cc3d2b39df285582954210b
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34165329"
---
# <a name="set-up-backup-and-replication-for-hbase-and-phoenix-on-hdinsight"></a>Back-up- en replicatie voor HBase en Phoenix op HDInsight instellen

HBase ondersteunt verschillende manieren voor het beveiligen tegen gegevensverlies:

* Kopieer de `hbase` map
* Exporteer vervolgens importeren
* Tabellen kopiëren
* Momentopnamen
* Replicatie

> [!NOTE]
> Apache Phoenix slaat de metagegevens in HBase-tabellen, zodat metagegevens back-up gemaakt is wanneer u back-up van de catalogus van HBase systeemtabellen.

De volgende secties beschrijven de gebruiksscenario voor elk van deze methoden.

## <a name="copy-the-hbase-folder"></a>Kopieer de map hbase

Met deze methode kunt u alle HBase gegevens kopiëren zonder dat de mogelijkheid om een subset van tabellen of kolomfamilies te selecteren. Volgende benaderingen bieden meer controle.

HBase in HDInsight maakt gebruik van de opslag van de standaard geselecteerd bij het maken van het cluster, Azure Storage blobs of Azure Data Lake Store. HBase slaat in beide gevallen moet de gegevens en metagegevens van bestanden onder het volgende pad:

    /hbase

* In een Azure Storage-account de `hbase` zich bevindt in de hoofdmap van de blob-container:

    ```
    wasbs://<containername>@<accountname>.blob.core.windows.net/hbase
    ```

* In Azure Data Lake Store de `hbase` zich bevindt in het hoofdpad die u hebt opgegeven bij het inrichten van een cluster. Deze hoofdpad heeft doorgaans een `clusters` map met een submap met de naam van uw HDInsight-cluster:

    ```
    /clusters/<clusterName>/hbase
    ```

In beide gevallen moet de `hbase` map bevat alle gegevens die door HBase is leeggemaakt op schijf, maar deze mogelijk niet de gegevens in het geheugen. Voordat u op deze map als een nauwkeurige weergave van de HBase-gegevens vertrouwen kunt, moet u het cluster afsluiten.

Nadat u het cluster verwijderd, kunt u laat de gegevens intact of de gegevens kopiëren naar een nieuwe locatie:

* Maak een nieuw HDInsight-exemplaar die verwijst naar de huidige opslaglocatie. Het nieuwe exemplaar wordt gemaakt met de bestaande gegevens.

* Kopieer de `hbase` map naar een andere Azure Storage blob-container of de locatie van de Data Lake Store en start vervolgens een nieuw cluster met die gegevens. Gebruik voor Azure Storage [AzCopy](../../storage/common/storage-use-azcopy.md), en voor gebruik van Data Lake Store [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md).

## <a name="export-then-import"></a>Exporteer vervolgens importeren

Gebruik de Export-hulpprogramma (meegeleverd met HBase) gegevens uit een brontabel exporteren naar de opslag gekoppeld standaard op de bron HDInsight-cluster. Vervolgens kunt u de geëxporteerde map kopiëren naar de doellocatie voor opslag en de Import-hulpprogramma uitvoeren op de bestemming HDInsight-cluster.

Een tabel, eerste SSH in het hoofdknooppunt van uw HDInsight-broncluster exporteren en voer de volgende `hbase` opdracht:

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>"

Een tabel importeren, SSH in het hoofdknooppunt van uw HDInsight-cluster van het doel en voer de volgende `hbase` opdracht:

    hbase org.apache.hadoop.hbase.mapreduce.Import "<tableName>" "/<path>/<to>/<export>"

Geef het pad volledig exporteren naar de standaard-opslag of naar een van de gekoppelde opslagopties. Bijvoorbeeld, in Azure Storage:

    wasbs://<containername>@<accountname>.blob.core.windows.net/<path>

In Azure Data Lake Store is de syntaxis:

    adl://<accountName>.azuredatalakestore.net:443/<path>

Deze aanpak biedt op tabelniveau samenvattingen. U kunt ook een datumbereik voor de rijen die u wilt opnemen, zodat u kunt het proces stapsgewijs uitvoeren opgeven. Elke datum is in milliseconden sinds de Unix-epoche.

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>" <numberOfVersions> <startTimeInMS> <endTimeInMS>

Houd er rekening mee dat u het aantal versies van elke rij moet exporteren opgeven. Instellen zodat alle versies in het datumbereik `<numberOfVersions>` op een waarde groter is dan de maximum mogelijke rij-versies, zoals 100000 liggen.

## <a name="copy-tables"></a>Tabellen kopiëren

Het hulpprogramma CopyTable worden gegevens uit een brontabel, per rij gekopieerd naar een bestaande doeltabel met hetzelfde schema als de bron. De doeltabel kan zich in hetzelfde cluster of een ander HBase-cluster.

Gebruik CopyTable binnen een cluster SSH in het hoofdknooppunt van uw HDInsight-broncluster en voer dit `hbase` opdracht:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> <srcTableName>

Toevoegen als u CopyTable wilt kopiëren naar een tabel op een ander cluster, de `peer` overschakelen met het doelcluster adres:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> --peer.adr=<destinationAddress> <srcTableName>

Het doeladres bestaat uit de volgende drie delen:

    <destinationAddress> = <ZooKeeperQuorum>:<Port>:<ZnodeParent>

* `<ZooKeeperQuorum>` is een door komma's gescheiden lijst met ZooKeeper-knooppunten, bijvoorbeeld:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net

* `<Port>` op de standaardwaarde 2181, HDInsight en `<ZnodeParent>` is `/hbase-unsecure`, dus de volledige `<destinationAddress>` zou zijn:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net:2181:/hbase-unsecure

Zie [handmatig verzamelen van de lijst van de Quorum ZooKeeper](#manually-collect-the-zookeeper-quorum-list) in dit artikel voor meer informatie over het ophalen van deze waarden voor uw HDInsight-cluster.

Het hulpprogramma CopyTable ondersteunt ook de parameters voor het opgeven van het tijdsbereik van rijen te kopiëren en op te geven van de subset met kolomfamilies in een tabel om te kopiëren. Voer de volledige lijst met parameters die worden ondersteund door CopyTable vindt CopyTable zonder parameters:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable

CopyTable scant de gehele tabel broninhoud die zal worden gekopieerd naar de doeltabel. Hierdoor wordt uw HBase-cluster prestaties terwijl CopyTable wordt uitgevoerd.

> [!NOTE]
> Voor het automatiseren van het kopiëren van gegevens tussen tabellen, Zie de `hdi_copy_table.sh` script in de [Azure HBase Utils](https://github.com/Azure/hbase-utils/tree/master/replication) opslagplaats op GitHub.

### <a name="manually-collect-the-zookeeper-quorum-list"></a>Het quorum ZooKeeper lijst handmatig verzamelen

Wanneer beide HDInsight-clusters in hetzelfde virtuele netwerk, zoals eerder beschreven, is interne hostnamen automatisch. Voor het gebruik CopyTable voor HDInsight-clusters in twee afzonderlijke virtuele netwerken verbonden via een VPN-Gateway, moet u de host-IP-adressen van de Zookeeper-knooppunten in het quorum bieden.

Als u wilt de hostnamen quorum aanschaft, voer de volgende curl-opdracht:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/configurations?type=hbase-site&tag=TOPOLOGY_RESOLVED" | grep "hbase.zookeeper.quorum"

De curl-opdracht een JSON-document met HBase-configuratiegegevens worden opgehaald en de grep-opdracht retourneert alleen de vermelding 'hbase.zookeeper.quorum', bijvoorbeeld:

    "hbase.zookeeper.quorum" : "zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net"

De waarde van het quorum host-namen is de volledige tekenreeks aan de rechterkant van de dubbele punt.

Voor het ophalen van de IP-adressen voor deze hosts, gebruikt u de volgende curl-opdracht voor elke host in de vorige lijst:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/hosts/<zookeeperHostFullName>" | grep "ip"

In deze opdracht curl `<zookeeperHostFullName>` is de volledige DNS-naam van een host ZooKeeper, zoals in het voorbeeld `zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net`. De uitvoer van de opdracht bevat bijvoorbeeld het IP-adres voor de opgegeven host:

    100    "ip" : "10.0.0.9",

Nadat u de IP-adressen voor alle ZooKeeper-knooppunten in uw quorumconfiguratie hebt verzameld, opnieuw het doeladres:

    <destinationAddress>  = <Host_1_IP>,<Host_2_IP>,<Host_3_IP>:<Port>:<ZnodeParent>

In ons voorbeeld:

    <destinationAddress> = 10.0.0.9,10.0.0.8,10.0.0.12:2181:/hbase-unsecure

## <a name="snapshots"></a>Momentopnamen

Momentopnamen inschakelen maken van een punt in tijd back-up van gegevens in uw HBase-gegevensarchief. Momentopnamen zijn minimale overhead en binnen enkele seconden wordt voltooid, omdat de bewerking van een momentopname is in feite een Metagegevensbewerking voor het vastleggen van de namen van alle bestanden in de opslag op dat moment. Op het moment van een momentopname, wordt geen actuele gegevens gekopieerd. Momentopnamen zijn afhankelijk van de niet-wijzigbaar aard van de gegevens in HDFS, waarbij updates, verwijderingen en invoegingen worden vertegenwoordigd als nieuwe gegevens. U kunt herstellen (*kloon*) een momentopname op hetzelfde cluster of een momentopname exporteren naar een ander cluster.

Maken van een momentopname, SSH in met het hoofdknooppunt van uw HDInsight-HBase-cluster en start de `hbase` shell:

    hbase shell

In de hbase-shell gebruikmaken van de momentopname-opdracht met de namen van de tabel en deze momentopname met:

    snapshot '<tableName>', '<snapshotName>'

Een momentopname terugzetten met de naam binnen de `hbase` shell, eerst uitschakelen van de tabel en vervolgens de momentopname herstellen en in de tabel opnieuw in te schakelen:

    disable '<tableName>'
    restore_snapshot '<snapshotName>'
    enable '<tableName>'

Als u een momentopname herstellen naar een nieuwe tabel, gebruikt u clone_snapshot:

    clone_snapshot '<snapshotName>', '<newTableName>'

Als u wilt exporteren een momentopname naar HDFS voor gebruik door een ander cluster, eerst de momentopname maken, zoals eerder beschreven en gebruik vervolgens het hulpprogramma ExportSnapshot. Dit hulpprogramma uitvoeren uit binnen de SSH-sessie met het hoofdknooppunt niet binnen de `hbase` shell:

     hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot <snapshotName> -copy-to <hdfsHBaseLocation>

De `<hdfsHBaseLocation>` kan een van de opslaglocaties toegankelijk is voor uw broncluster en moet verwijzen naar de hbase-map die wordt gebruikt door uw doelcluster. Bijvoorbeeld, als u een secundaire Azure Storage-account gekoppeld aan het broncluster hebt en dat account toegang tot de container die wordt gebruikt door de opslag van de standaardwaarde van het doelcluster biedt, kunt u deze opdracht:

    hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot 'Snapshot1' -copy-to 'wasbs://secondcluster@myaccount.blob.core.windows.net/hbase'

Nadat de momentopname is geëxporteerd, opdracht SSH in het hoofdknooppunt van het doelcluster en herstel momentopname met de restore_snapshot zoals eerder beschreven.

Momentopnamen bieden een volledige back-up van een tabel op het moment van de `snapshot` opdracht. Momentopnamen bieden niet de mogelijkheid om uit te voeren incrementele momentopnamen door windows tijd, noch om op te geven subsets van families van de kolommen moeten worden opgenomen in de momentopname.

## <a name="replication"></a>Replicatie

HBase-replicatie verstuurd automatisch transacties uit een broncluster naar een doelcluster met behulp van een asynchrone methode met een minimale overhead op de broncluster. In HDInsight, kunt u replicatie tussen clusters instellen waarbij:

* De clusters bron- en doelserver zich in hetzelfde virtuele netwerk.
* De bron- en doelen clusters zijn in verschillende virtuele netwerken die zijn verbonden via een VPN-gateway, maar beide clusters aanwezig zijn in dezelfde geografische locatie.
* De broncluster en doelen clusters zijn in verschillende virtuele netwerken die zijn verbonden via een VPN-gateway en elk cluster bestaat in een andere geografische locatie.

De algemene stappen voor het instellen van de replicatie zijn:

1. De tabellen maken en vullen gegevens op het broncluster.
2. Op de doelcluster doeltabellen leeg te maken met het schema van de brontabel.
3. Het doelcluster registreren als een peer met de broncluster.
4. Replicatie op de gewenste brontabellen inschakelen.
5. Bestaande gegevens uit de brontabellen kopiëren naar de doeltabellen.
6. Nieuwe gegevens worden aangebracht aan de brontabellen replicatie automatisch gekopieerd naar de doeltabellen.

Om in te schakelen replicatie in HDInsight, een scriptactie van toepassing op uw actieve bron HDInsight-cluster. Zie voor een overzicht van het inschakelen van replicatie in uw cluster, of om te experimenteren met replicatie op de voorbeeld-clusters die zijn gemaakt in virtuele netwerken die gebruikmaken van Azure Resource Management-sjablonen, [configureren HBase-replicatie](apache-hbase-replication.md). Dit artikel bevat ook instructies voor het inschakelen van replicatie van Phoenix metagegevens.

## <a name="next-steps"></a>Volgende stappen

* [HBase-replicatie configureren](apache-hbase-replication.md)

---
title: Schaal cluster formaat, Azure HDInsight
description: Een HDInsight-cluster aan uw workload schalen.
services: hdinsight
author: ashishthaps
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/02/2018
ms.author: ashish
ms.openlocfilehash: 2f1de608a8273d7be50e4ad432ab72052fc0e228
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39596837"
---
# <a name="scale-hdinsight-clusters"></a>HDInsight-clusters schalen

HDInsight biedt flexibiliteit doordat u de optie voor omhoog en omlaag het aantal worker-knooppunten in uw clusters schalen. Hiermee kunt u een cluster verkleinen na uur of in het weekend en vouwt u het tijdens piek bedrijfsbehoeften te voldoen.

Bijvoorbeeld, hebt u enkele batchverwerking in dat geval één keer per dag of één keer per maand, de HDInsight-cluster kan worden opgeschaald een paar minuten vóór de geplande gebeurtenis, zodat er voldoende geheugen en CPU-rekenkracht. U kunt automatiseren met de PowerShell-cmdlet schalen [ `Set–AzureRmHDInsightClusterSize` ](hdinsight-administer-use-powershell.md#scale-clusters).  Later, nadat de verwerking is voltooid en gebruik opnieuw uitvalt, kunt u schalen u het HDInsight-cluster met minder werkrolknooppunten.

* Voor het schalen van uw cluster via [PowerShell](hdinsight-administer-use-powershell.md):

    ```powershell
    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
    ```
    
* Voor het schalen van uw cluster via de [Azure CLI](hdinsight-administer-use-command-line.md):

    ```
    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
    ```
    
* Voor het schalen van uw cluster via de [Azure-portal](https://portal.azure.com), opent u uw HDInsight-cluster deelvenster, selecteert u **cluster schalen** in het menu links en vervolgens in het deelvenster van de cluster schalen, typt u in het aantal worker-knooppunten, en Selecteer opslaan.

    ![Schaal cluster aanpassen](./media/hdinsight-scaling-best-practices/scale-cluster-blade.png)

Met behulp van een van deze methoden, kunt u uw HDInsight-cluster omhoog of omlaag schalen binnen enkele minuten.

## <a name="scaling-impacts-on-running-jobs"></a>Gevolgen voor het uitvoeren van taken schalen

Wanneer u **toevoegen** knooppunten aan uw actieve HDInsight-cluster niet alle taken die in behandeling of actief worden beïnvloed. Bovendien kunnen nieuwe taken worden veilig verzonden terwijl het proces voor vergroten/verkleinen wordt uitgevoerd. Als de vergroten / verkleinen voor een bepaalde reden mislukken, wordt de fout probleemloos uitgevoerd, verlaat de cluster in een functionele staat.

Echter, als u uw cluster door zijn verkleint **verwijderen** knooppunten, alle taken die in behandeling of wordt uitgevoerd, mislukken wanneer de vergroten/verkleinen bewerking is voltooid. Deze fout wordt veroorzaakt door een van de services opnieuw te starten tijdens het proces.

Om dit probleem op te lossen, kunt u de taken zijn voltooid voordat u uw cluster verkleint, handmatig de taken beëindigd of de taken opnieuw nadat de vergroten/verkleinen bewerking heeft de diagnose afgerond wachten.

U ziet een lijst van in behandeling en taken die worden uitgevoerd, kunt u de YARN ResourceManager-interface, volgende stappen:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
2. Selecteer in het menu links **Bladeren**, selecteer **HDInsight-Clusters**, en selecteer vervolgens uw cluster.
3. Selecteer in het deelvenster HDInsight-cluster **Dashboard** in het bovenste menu de UI Ambari te openen. Voer uw referenties voor clusteraanmelding.
4. Klik op **YARN** in de lijst met services in het menu links. Selecteer op de pagina YARN **snelkoppelingen** en beweeg de muisaanwijzer over de actieve hoofdknooppunt en klik vervolgens op **ResourceManager UI**.

    ![ResourceManager-interface](./media/hdinsight-scaling-best-practices/resourcemanager-ui.png)

U hebt rechtstreeks toegang tot de ResourceManager-UI met `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster`.

U ziet een lijst met taken, samen met de huidige status. Is er een taak die momenteel wordt uitgevoerd in de schermafbeelding:

![ResourceManager UI-toepassingen](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

Als u wilt dat actieve toepassing handmatig kill, dan de volgende opdracht uit vanuit de SSH-shell:

```bash
yarn application -kill <application_id>
```

Bijvoorbeeld:

```bash
yarn application -kill "application_1499348398273_0003"
```

## <a name="rebalancing-an-hbase-cluster"></a>Herverdeling van een HBase-cluster

Regioservers worden automatisch verdeeld binnen een paar minuten na voltooiing van de bewerking vergroten/verkleinen. Handmatig evenwichtige regioservers, gebruikt u de volgende stappen uit:

1. Verbinding maken met het HDInsight-cluster via SSH. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Start de HBase-shell:

        hbase shell

3. Gebruik de volgende opdracht om handmatig de regioservers:

        balancer

## <a name="scale-down-implications"></a>Gevolgen verkleinen

Zoals eerder vermeld, worden alle in behandeling of actieve taken beëindigd na het voltooien van een bewerking omlaag schalen. Er zijn echter andere mogelijke gevolgen voor de schaal omlaag die zich kan voordoen.

## <a name="hdinsight-name-node-stays-in-safe-mode-after-scaling-down"></a>HDInsight naam knooppunt blijft in de veilige modus na het omlaag schalen

![Schaal cluster aanpassen](./media/hdinsight-scaling-best-practices/scale-cluster.png)

Als u uw cluster naar het minimum van een worker-knooppunt, zoals wordt weergegeven in de vorige afbeelding krimpen, zijn HDFS vastgelopen in de veilige modus als de worker-knooppunten opnieuw worden opgestart vanwege het toepassen van patches of onmiddellijk nadat de vergroten/verkleinen is uitgevoerd.

De belangrijkste oorzaak hiervan is dat Hive enkele gebruikt `scratchdir` bestanden en standaard wordt verwacht dat de drie replica's van elk blok, maar er is slechts één replica mogelijk als u omlaag naar het ten minste één worker-knooppunt schalen. Als gevolg, de bestanden in de `scratchdir` worden *under-gerepliceerde*. Dit kan ertoe leiden dat HDFS om te blijven in de veilige modus wanneer de services opnieuw na de schaalbewerking gestart zijn.

Als een neerschalen poging gebeurt, is HDInsight is afhankelijk van de Ambari-management-interfaces voor het eerst uit bedrijf nemen de extra ongewenste worker-knooppunten, die hun HDFS-blokken gerepliceerd naar andere online worker-knooppunten, en het cluster veilig omlaag te schalen. HDFS gaat op een veilige modus tijdens het onderhoudsvenster, en moet afkomstig zijn uit als de schaal is voltooid. Het is op dit moment dat de HDFS kan zijn vastgelopen in de veilige modus.

HDFS is geconfigureerd met een `dfs.replication` instellen van 3. Dus zijn de blokken van de tijdelijke bestanden under-gerepliceerde wanneer er minder dan drie werkknooppunten online zijn, omdat er niet de verwachte drie kopieën van elk bestand blok beschikbaar.

U kunt een opdracht om HDFS buiten de veilige modus uitvoeren. Bijvoorbeeld, als u weet dat de enige reden veilige modus ingeschakeld is omdat de tijdelijke bestanden under-gerepliceerde, kunt klikt u vervolgens u veilig laten veilige modus. Dit komt doordat de under-gerepliceerde bestanden tijdelijke werkbestanden Hive zijn.

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

Na de veilige modus te verlaten, kunt u handmatig de tijdelijke bestanden verwijderen of wacht Hive uiteindelijk opschonen ze automatisch.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Voorbeeld van de fouten wanneer de veilige modus is ingeschakeld

* Kan H070 om Hive-sessie te openen. org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **kan map niet maken**  /tmp/hive/hive / 819c215c - 6d 87-4311-97c-8-4f0b9d2adcf0. **De naam van knooppunt zich in de veilige modus**. De gerapporteerde blokken 75 moet extra 12 blokken aan de drempelwaarde 0.9900 van blokken 87 bereiken. Het aantal live datanodes 10 heeft het minimale aantal 0 bereikt. Veilige modus wordt automatisch uitgeschakeld wanneer de drempelwaarden hebben bereikt.

* H100 geen instructie indienen databases weergeven: org.apache.thrift.transport.TTransportException: org.apache.http.conn.HttpHostConnectException: verbinding maken met hn0-clustername.servername.internal.cloudapp.net:10001 [hn0-clustername.servername . Internal.cloudapp.NET/1.1.1.1] is mislukt: **verbinding geweigerd**

* H020 kan geen verbinding maken met .net hn0 hdisrv.servername.bx.internal.cloudapp: 10001: org.apache.thrift.transport.TTransportException: kan geen http-verbinding te maken http://hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/. org.apache.http.conn.HttpHostConnectException: verbinding maken met hn0-hdisrv.servername.bx.internal.cloudapp.net:10001 [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28] is mislukt: verbinding geweigerd: org.apache.thrift.transport.TTransportException: kan geen http-verbinding te maken http://hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/. org.apache.http.conn.HttpHostConnectException: verbinding maken met hn0-hdisrv.servername.bx.internal.cloudapp.net:10001 [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28] is mislukt: **verbinding geweigerd**

* In de Hive-Logboeken: waarschuwen [hoofd]: server. HiveServer2 (HiveServer2.java:startHiveServer2(442)): fout bij het starten van HiveServer2 poging 21, probeert in 60 seconden java.lang.RuntimeException: fout bij het autorisatiebeleid voor toepassen van hive-configuratie: org.apache.hadoop.ipc.RemoteException ( org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **kan map niet maken** /tmp/hive/hive/70a42b8a-9437-466e-acbe-da90b1614374. **De naam van knooppunt zich in de veilige modus**.
    De gerapporteerde blokken 0 moet extra 9 blokken aan de drempelwaarde 0.9900 van blokken 9 bereiken.
    Het aantal live datanodes 10 heeft het minimale aantal 0 bereikt. **Veilige modus wordt automatisch worden uitgeschakeld zodra de drempelwaarden hebben bereikt**.
    op org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1324)

U kunt de naam van knooppuntlogboeken van bekijken de `/var/log/hadoop/hdfs/` map in de buurt van de tijd wanneer het cluster is geschaald om te zien wanneer deze veilige modus hebt ingevoerd. De naam van de logboekbestanden `Hadoop-hdfs-namenode-hn0-clustername.*`.

De hoofdoorzaak van de vorige fouten is dat Hive afhankelijk van tijdelijke bestanden in HDFS is tijdens het uitvoeren van query's. Wanneer u HDFS in veilige modus, kan niet Hive query's uitvoeren omdat er niet op HDFS. De tijdelijke bestanden in HDFS bevinden zich in het lokale station gekoppeld aan de afzonderlijke worker-knooppunt VM's en gerepliceerd met andere worker-knooppunten op drie replica's, minimaal.

De `hive.exec.scratchdir` parameter in Hive is geconfigureerd in `/etc/hive/conf/hive-site.xml`:

```xml
<property>
    <name>hive.exec.scratchdir</name>
    <value>hdfs://mycluster/tmp/hive</value>
</property>
```

### <a name="view-the-health-and-state-of-your-hdfs-file-system"></a>Bekijk de status en de status van uw HDFS-bestandssysteem

Hier vindt u een statusrapport van elk knooppunt naam om te zien of er knooppunten in de veilige modus zijn. Het rapport, voeg SSH toe aan elke hoofdknooppunt weergeven en voer de volgende opdracht uit:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode get
```

![Veilige modus uitschakelen](./media/hdinsight-scaling-best-practices/safe-mode-off.png)

> [!NOTE]
> De `-D` switch is nodig omdat het standaardbestandssysteem in HDInsight Azure Storage of Azure Data Lake Store. `-D` Hiermee geeft u op dat de opdrachten op basis van de lokale HDFS-bestandssysteem uitvoeren.

Vervolgens kunt u een rapport met de details van de HDFS-status weergeven:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -report
```

Met deze opdracht resulteert in het volgende op een gezonde cluster waar alle blokken worden gerepliceerd naar de verwachte mate:

![Veilige modus uitschakelen](./media/hdinsight-scaling-best-practices/report.png)

HDFS ondersteunt de `fsck` opdracht om te controleren op inconsistenties met verschillende bestanden, zoals ontbrekende geblokkeerd voor een bestand of under-gerepliceerde blokken. Om uit te voeren de `fsck` opdracht op basis van de `scratchdir` (tijdelijke scratch-schijf)-bestanden:

```
hdfs fsck -D 'fs.default.name=hdfs://mycluster/' /tmp/hive/hive
```

Wanneer uitgevoerd op een gezonde HDFS-bestandssysteem met geen under-gerepliceerde blokken, ziet u uitvoer die vergelijkbaar is met het volgende:

```
Connecting to namenode via http://hn0-scalin.name.bx.internal.cloudapp.net:30070/fsck?ugi=sshuser&path=%2Ftmp%2Fhive%2Fhive
FSCK started by sshuser (auth:SIMPLE) from /10.0.0.21 for path /tmp/hive/hive at Thu Jul 06 20:07:01 UTC 2017
..Status: HEALTHY
 Total size:    53 B
 Total dirs:    5
 Total files:   2
 Total symlinks:                0 (Files currently being written: 2)
 Total blocks (validated):      2 (avg. block size 26 B)
 Minimally replicated blocks:   2 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     3.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          4
 Number of racks:               1
FSCK ended at Thu Jul 06 20:07:01 UTC 2017 in 3 milliseconds


The filesystem under path '/tmp/hive/hive' is HEALTHY
```

Daarentegen, wanneer de `fsck` opdracht wordt uitgevoerd op een HDFS-bestandssysteem met sommige under-gerepliceerde blokken, de uitvoer is vergelijkbaar met het volgende:

```
Connecting to namenode via http://hn0-scalin.name.bx.internal.cloudapp.net:30070/fsck?ugi=sshuser&path=%2Ftmp%2Fhive%2Fhive
FSCK started by sshuser (auth:SIMPLE) from /10.0.0.21 for path /tmp/hive/hive at Thu Jul 06 20:13:58 UTC 2017
.
/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info:  Under replicated BP-1867508080-10.0.0.21-1499348422953:blk_1073741826_1002. Target Replicas is 3 but found 1 live replica(s), 0 decommissioned replica(s) and 0 decommissioning replica(s).
.
/tmp/hive/hive/e7c03964-ff3a-4ee1-aa3c-90637a1f4591/inuse.info: CORRUPT blockpool BP-1867508080-10.0.0.21-1499348422953 block blk_1073741825

/tmp/hive/hive/e7c03964-ff3a-4ee1-aa3c-90637a1f4591/inuse.info: MISSING 1 blocks of total size 26 B.Status: CORRUPT
 Total size:    53 B
 Total dirs:    5
 Total files:   2
 Total symlinks:                0 (Files currently being written: 2)
 Total blocks (validated):      2 (avg. block size 26 B)
  ********************************
  UNDER MIN REPL'D BLOCKS:      1 (50.0 %)
  dfs.namenode.replication.min: 1
  CORRUPT FILES:        1
  MISSING BLOCKS:       1
  MISSING SIZE:         26 B
  CORRUPT BLOCKS:       1
  ********************************
 Minimally replicated blocks:   1 (50.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       1 (50.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     0.5
 Corrupt blocks:                1
 Missing replicas:              2 (33.333332 %)
 Number of data-nodes:          1
 Number of racks:               1
FSCK ended at Thu Jul 06 20:13:58 UTC 2017 in 28 milliseconds


The filesystem under path '/tmp/hive/hive' is CORRUPT
```

U kunt ook de HDFS-status bekijken in Ambari UI door het selecteren van de **HDFS** service aan de linkerkant of met `https://<HDInsightClusterName>.azurehdinsight.net/#/main/services/HDFS/summary`.

![Ambari HDFS-status](./media/hdinsight-scaling-best-practices/ambari-hdfs.png)

U ziet ook een of meer kritieke fouten op de actieve of stand-by NameNodes. Als u de status van de blokken NameNode, selecteert u de koppeling NameNode naast de waarschuwing.

![NameNode blokken Health](./media/hdinsight-scaling-best-practices/ambari-hdfs-crit.png)

Opschonen van de tijdelijke bestanden die Hiermee verwijdert u de blok-replicatiefouten SSH naar elke hoofdknooppunt en voer de volgende opdracht uit:

```
hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
```

> [!NOTE]
> Met deze opdracht kunt Hive verbreken als sommige taken worden nog steeds uitgevoerd.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode-due-to-under-replicated-blocks"></a>Voorkomen dat HDInsight zitten in de veilige modus vanwege under-gerepliceerde blokken

Er zijn verschillende manieren om te voorkomen dat HDInsight wordt links in de veilige modus:

* Stop alle Hive-taken voor het verkleinen van HDInsight. U kunt ook de schaal omlaag om te voorkomen dat er bij het uitvoeren van Hive-taken plannen.
* Handmatig opschonen van Hive volledig nieuwe `tmp` directory-bestanden in HDFS voordat het omlaag schalen.
* Alleen verkleinen HDInsight met drie werkrolknooppunten, minimum. Voorkomen dat u slechts één worker-knooppunt.
* Voer de opdracht uit om te laten veilige modus, indien nodig.

De volgende secties worden deze opties.

#### <a name="stop-all-hive-jobs"></a>Alle Hive-taken stoppen

Alle Hive-taken stoppen voordat het omlaag schalen naar een worker-knooppunt. Als uw werkbelasting is gepland, Voer uw omlaag schalen wanneer Hive werk is voltooid.

Dit helpt Beperk het aantal nieuwe bestanden in de map tmp (indien aanwezig).

#### <a name="manually-clean-up-hives-scratch-files"></a>Hive van tijdelijke bestanden handmatig opschonen

Als Hive heeft verlaten achter tijdelijke bestanden, en vervolgens u handmatig die bestanden opschonen kunt voordat u omlaag geschaald naar voorkomen dat de veilige modus.

1. Hive-services stoppen en zorg ervoor dat alle query's en taken zijn voltooid.

2. De inhoud van de `hdfs://mycluster/tmp/hive/` directory om te zien als deze bestanden bevat:

    ```
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```
    
    Hier volgt een voorbeeld van uitvoer wanneer bestanden bestaan:

    ```
    sshuser@hn0-scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

3. Als u bekend bent met het dat hive wordt uitgevoerd met deze bestanden, kunt u ze verwijderen. Zorg ervoor dat Hive hoeft niet alle query's uitgevoerd door te kijken in de gebruikersinterface van Yarn ResourceManager-pagina.

    Voorbeeld van de opdrachtregel om bestanden te verwijderen uit HDFS:

    ```
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```
    
#### <a name="scale--hdinsight-to-three-worker-nodes"></a>HDInsight op drie worker-knooppunten schalen

Als zitten in de veilige modus is een permanente probleem en de vorige stappen zijn niet opties, en vervolgens kunt u om te voorkomen dat het probleem door alleen schalen naar drie worker-knooppunten. Dit is mogelijk niet optimaal zijn, vanwege kostenbeperkingen, in vergelijking met het omlaag schalen naar één knooppunt. Echter, met slechts één worker-knooppunt, HDFS kan niet garanderen drie replica's van de gegevens zijn beschikbaar voor het cluster.

#### <a name="run-the-command-to-leave-safe-mode"></a>Voer de opdracht uit om te laten veilige modus

De laatste mogelijkheid is om te bekijken voor de zeldzame gevallen waarin HDFS in veilige modus geplaatst en vervolgens de veilige modus laat opdracht niet uitvoeren. Nadat u hebt vastgesteld dat de reden HDFS heeft ingevoerd de veilige modus is vanwege de Hive-bestanden worden under-gerepliceerde, voert u de volgende opdracht uit om te laten veilige modus:

* HDInsight op Linux:

    ```bash
    hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
    ```
    
* HDInsight op Windows:

    ```bash
    hdfs dfsadmin -fs hdfs://headnodehost:9000 -safemode leave
    ```
    
## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure HDInsight](hadoop/apache-hadoop-introduction.md)
* [Clusters schalen](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [HDInsight-clusters beheren met behulp van de Ambari-Webinterface](hdinsight-hadoop-manage-ambari.md)

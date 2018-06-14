---
title: Schalen van clusters - Azure HDInsight | Microsoft Docs
description: De schaal van een HDInsight-cluster aan uw werkbelasting.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: ashish
ms.openlocfilehash: 8b76d7d0441a5c1c25ad17b73083ec0e4feef1fe
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31414317"
---
# <a name="scale-hdinsight-clusters"></a>HDInsight-clusters schalen

HDInsight biedt elasticiteit doordat u de optie opschalen of terugschroeven van het aantal worker-knooppunten in uw clusters. Hiermee kunt u een cluster verkleinen buiten kantooruren of tijdens het weekend en daarop tijdens piek bedrijfsbehoeften.

Bijvoorbeeld als er een aantal batchverwerking dat gebeurt eenmaal per dag of één keer per maand, het HDInsight-cluster kan enkele minuten duren voordat de geplande gebeurtenis worden uitgebreid zodat er voldoende geheugen en CPU-rekencapaciteit. U kunt automatiseren schaling mogelijk met de PowerShell-cmdlet [ `Set–AzureRmHDInsightClusterSize` ](hdinsight-administer-use-powershell.md#scale-clusters).  Later, nadat het verwerken is voltooid, en gebruik opnieuw uitvalt, kunt u Schalen het HDInsight-cluster naar minder werknemerknooppunten af.

* Schalen van uw cluster via [PowerShell](hdinsight-administer-use-powershell.md):

    ```powershell
    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
    ```
    
* Schalen van uw cluster via de [Azure CLI](hdinsight-administer-use-command-line.md):

    ```
    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
    ```
    
* Schalen van uw cluster via de [Azure-portal](https://portal.azure.com), opent u het deelvenster HDInsight-cluster, selecteer **Scale cluster** op in het menu links en klik vervolgens op het cluster Scale deelvenster, typt u het aantal worker-knooppunten en Selecteer opslaan.

    ![Schaal cluster aanpassen](./media/hdinsight-scaling-best-practices/scale-cluster-blade.png)

Met behulp van een van deze methoden, u kunt uw HDInsight-cluster omhoog of omlaag schalen binnen enkele minuten.

## <a name="scaling-impacts-on-running-jobs"></a>Gevolgen voor de uitvoering van taken schalen

Wanneer u **toevoegen** knooppunten met uw actieve HDInsight-cluster, alle taken die in behandeling of wordt uitgevoerd niet zullen worden beïnvloed. Bovendien, worden nieuwe taken veilig verzonden terwijl het vergroten/verkleinen proces wordt uitgevoerd. Als de vergroten/verkleinen bewerkingen voor een bepaalde reden mislukken, is de fout probleemloos verwerkt verlaat de cluster functioneel is.

Echter, als u het cluster zijn verkleinen **verwijderen** knooppunten, in behandeling of actieve taken mislukken wanneer het vergroten/verkleinen is voltooid. Deze fout wordt veroorzaakt door een aantal van de services opnieuw te starten tijdens het proces.

Om dit probleem op te lossen, kunt u de taken zijn voltooid voordat het verkleinen van uw cluster, handmatig de taken beëindigd of de taken opnieuw indienen nadat de bewerking uit te schalen heeft gesloten wachten.

U ziet een lijst met openstaande en taken die worden uitgevoerd, kunt u de ResourceManager gebruikersinterface van YARN, deze stappen te volgen:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
2. Selecteer op het menu links **Bladeren**, selecteer **HDInsight-Clusters**, en selecteer vervolgens het cluster.
3. Selecteer in het deelvenster HDInsight-cluster **Dashboard** in het bovenste menu openen van de Ambari UI. Voer uw cluster-aanmeldingsreferenties.
4. Klik op **YARN** in de lijst met services op het menu links. Selecteer op de pagina YARN **snelkoppelingen** en beweeg de muisaanwijzer over het actieve hoofdknooppunt en klik vervolgens op **ResourceManager UI**.

    ![ResourceManager gebruikersinterface](./media/hdinsight-scaling-best-practices/resourcemanager-ui.png)

U hebt rechtstreeks toegang tot de ResourceManager UI met `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster`.

U ziet een lijst met taken, samen met hun huidige status. In de schermafbeelding is er één taak die momenteel worden uitgevoerd:

![ResourceManager UI-toepassingen](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

Als u wilt afsluiten handmatig uitgevoerd wordt de toepassing, voer de volgende opdracht vanuit de shell SSH:

```bash
yarn application -kill <application_id>
```

Bijvoorbeeld:

```bash
yarn application -kill "application_1499348398273_0003"
```

## <a name="rebalancing-an-hbase-cluster"></a>Herverdeling van een HBase-cluster

Regio servers worden automatisch verdeeld binnen een paar minuten na voltooiing van de bewerking uit te schalen. Om handmatig een balans vinden tussen regio servers, gebruikt u de volgende stappen uit:

1. Verbinding maken met het HDInsight-cluster via SSH. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Start de HBase-shell:

        hbase shell

3. De volgende opdracht gebruiken om handmatig de regio-servers:

        balancer

## <a name="scale-down-implications"></a>De implicaties van terugschroeven

Zoals eerder vermeld, worden alle taken die in behandeling of wordt uitgevoerd na het voltooien van een schaal van omlaag bewerking beëindigd. Er zijn echter andere mogelijke gevolgen voor de schaal omlaag die zich kan voordoen.

## <a name="hdinsight-name-node-stays-in-safe-mode-after-scaling-down"></a>HDInsight naam knooppunt blijft in de veilige modus na omlaag schalen

![Schaal cluster aanpassen](./media/hdinsight-scaling-best-practices/scale-cluster.png)

Als u uw cluster naar beneden op het minimum van een werkrolknooppunt, verkleint zoals weergegeven in de vorige afbeelding, zijn HDFS vastgelopen in de veilige modus wanneer worker-knooppunten vanwege patchen of direct na het vergroten/verkleinen opnieuw worden opgestart.

De primaire oorzaak hiervan is dat gebruikmaakt van een aantal Hive `scratchdir` bestanden en standaard verwacht drie replica's van elk blok, maar er is slechts één replica mogelijk als u met het minimaal één werkrolknooppunt omlaag schalen. Als gevolg, de bestanden in de `scratchdir` worden *under-gerepliceerde*. Dit kan ertoe leiden dat HDFS om te blijven in de veilige modus als de services opnieuw worden opgestart na de schaalaanpassing.

Wanneer er een schaalwaarde omlaag poging gebeurt, wordt HDInsight is afhankelijk van de Ambari-beheerinterfaces eerst buiten gebruik stellen extra ongewenste worker-knooppunten, die hun HDFS-blokken worden gerepliceerd naar andere online worker-knooppunten, en het cluster veilig omlaag te schalen. HDFS gaat naar een veilige modus tijdens het onderhoudsvenster en moet afkomstig zijn uit zodra de schaal is voltooid. Het is op dit moment dat HDFS zijn vastgelopen in de veilige modus.

HDFS is geconfigureerd met een `dfs.replication` instellen van 3. Dus zijn de blokken van de werkbestanden under-gerepliceerde wanneer er minder dan drie worker-knooppunten online zijn omdat er niet de verwachte drie kopieën van elk bestand blok beschikbaar.

U kunt een opdracht op om te zetten HDFS buiten de veilige modus uitvoeren. Bijvoorbeeld, als u weet dat de enige reden veilige modus ingeschakeld is omdat de tijdelijke bestanden under-gerepliceerde is, kunt klikt u vervolgens u veilig laten veilige modus. Dit is omdat de under-gerepliceerde bestanden tijdelijke werkbestanden Hive.

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

Na het verlaten van de veilige modus kunt u handmatig verwijderen van de tijdelijke bestanden of wacht u totdat Hive uiteindelijk opschonen ze automatisch.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Voorbeeld fouten bij de veilige modus is ingeschakeld

* Kan H070 Hive-sessie te openen. org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **kan map niet maken**  /tmp/hive hive / 819c215c - 6-d 87-4311-97c-8-4f0b9d2adcf0. **De naam van knooppunt is in de veilige modus**. De gerapporteerde blokken 75 moet extra 12 blokken de drempelwaarde 0.9900 van totaal aantal blokken 87 bereiken. Het aantal live datanodes 10 is het minimale aantal 0 bereikt. Veilige modus wordt automatisch uitgeschakeld wanneer de drempelwaarden is bereikt.

* H100 kan verzenden instructie databases weergeven: org.apache.thrift.transport.TTransportException: org.apache.http.conn.HttpHostConnectException: verbinding maken met hn0-clustername.servername.internal.cloudapp.net:10001 [hn0 clustername.servername . Internal.cloudapp.NET/1.1.1.1] is mislukt: **verbinding geweigerd**

* H020 kan geen verbinding maken met .net hn0 hdisrv.servername.bx.internal.cloudapp: 10001: org.apache.thrift.transport.TTransportException: kan geen HTTP-verbinding te maken http://hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/. org.apache.http.conn.HttpHostConnectException: verbinding maken met hn0-hdisrv.servername.bx.internal.cloudapp.net:10001 [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28] is mislukt: verbinding geweigerd: org.apache.thrift.transport.TTransportException: kan geen HTTP-verbinding te maken http://hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/. org.apache.http.conn.HttpHostConnectException: verbinding maken met hn0-hdisrv.servername.bx.internal.cloudapp.net:10001 [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28] is mislukt: **verbinding geweigerd**

* Van de logboeken van de component: [hoofd] waarschuwen: server. HiveServer2 (HiveServer2.java:startHiveServer2(442)): fout bij het starten van HiveServer2 poging 21, wordt het opnieuw geprobeerd 60 seconden java.lang.RuntimeException: fout bij het toepassen van verificatiebeleid van hive-configuratie: org.apache.hadoop.ipc.RemoteException () org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **kan map niet maken** /tmp/hive/hive/70a42b8a-9437-466e-acbe-da90b1614374. **De naam van knooppunt is in de veilige modus**.
    De gerapporteerde blokken 0 moet extra 9 blokken de drempelwaarde 0.9900 van totaal aantal blokken 9 bereiken.
    Het aantal live datanodes 10 is het minimale aantal 0 bereikt. **Veilige modus wordt automatisch worden uitgeschakeld nadat de drempelwaarden hebben bereikt**.
    op org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1324)

U kunt controleren de logboeken van de naam van knooppunt van de `/var/log/hadoop/hdfs/` map in de buurt van de tijd wanneer het cluster is geschaald om te zien wanneer deze veilige modus hebt ingevoerd. De naam van de logboekbestanden `Hadoop-hdfs-namenode-hn0-clustername.*`.

De hoofdoorzaak van de vorige fouten is dat Hive afhankelijk van tijdelijke bestanden in HDFS is tijdens het uitvoeren van query's. Wanneer u HDFS in veilige modus, kan niet Hive query's uitvoeren omdat deze kan niet naar HDFS schrijven. De tijdelijke bestanden in HDFS bevinden zich in de lokale schijf is gekoppeld aan het knooppunt voor zelfstandige virtuele machines en onder een andere worker-knooppunten op drie replica's, minimale gerepliceerd.

De `hive.exec.scratchdir` parameter in de component is geconfigureerd in `/etc/hive/conf/hive-site.xml`:

```xml
<property>
    <name>hive.exec.scratchdir</name>
    <value>hdfs://mycluster/tmp/hive</value>
</property>
```

### <a name="view-the-health-and-state-of-your-hdfs-file-system"></a>De status en de status van uw bestandssysteem HDFS weergeven

U kunt een statusrapport van elk knooppunt van de naam of knooppunten in de veilige modus zijn weergeven. Het rapport, voeg SSH toe aan elke hoofdknooppunt weergeven en voer de volgende opdracht:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode get
```

![Veilige modus uitschakelen](./media/hdinsight-scaling-best-practices/safe-mode-off.png)

> [!NOTE]
> De `-D` switch is nodig omdat het standaardbestandssysteem in HDInsight Azure Storage of Azure Data Lake Store. `-D` Hiermee geeft u de opdrachten worden uitgevoerd op basis van het lokale HDFS-bestandssysteem.

U kunt vervolgens een rapport met de details van de HDFS-status bekijken:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -report
```

Deze opdracht resulteert in het volgende in een gezonde cluster waar alle blokken worden gerepliceerd naar de verwachte mate:

![Veilige modus uitschakelen](./media/hdinsight-scaling-best-practices/report.png)

HDFS ondersteunt de `fsck` opdracht om te controleren op inconsistenties met verschillende bestanden, zoals het ontbreken van een bestand of under-gerepliceerde blokken blokkeert. Om uit te voeren de `fsck` opdracht tegen de `scratchdir` (tijdelijke scratchruimte schijf)-bestanden:

```
hdfs fsck -D 'fs.default.name=hdfs://mycluster/' /tmp/hive/hive
```

Bij uitvoering in een gezonde HDFS-bestandssysteem met geen under-gerepliceerde blokken, ziet u uitvoer ziet er als volgt:

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

Daarentegen, wanneer de `fsck` opdracht wordt uitgevoerd op een HDFS-bestandssysteem met under-gerepliceerde blokken, is de uitvoer ziet er ongeveer als volgt:

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

U kunt ook de HDFS-status in Ambari UI weergeven door het selecteren van de **HDFS** service aan de linkerkant of met `https://<HDInsightClusterName>.azurehdinsight.net/#/main/services/HDFS/summary`.

![Ambari HDFS-status](./media/hdinsight-scaling-best-practices/ambari-hdfs.png)

U ziet mogelijk ook een of meer kritieke fouten op de actieve of stand-by NameNodes. Als u wilt weergeven van de status van de blokken NameNode, selecteer de koppeling NameNode naast de waarschuwing.

![NameNode blokken Health](./media/hdinsight-scaling-best-practices/ambari-hdfs-crit.png)

Opruimen van de werkbestanden die Hiermee verwijdert u het blok replicatiefouten, voeg SSH toe aan elke hoofdknooppunt en voer de volgende opdracht:

```
hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
```

> [!NOTE]
> Met deze opdracht kunt Hive verbroken als sommige taken zijn nog actief.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode-due-to-under-replicated-blocks"></a>Voorkomen dat HDInsight zitten in de veilige modus vanwege under-gerepliceerde blokken

Er zijn verschillende manieren om te voorkomen dat HDInsight blijft in de veilige modus:

* Stop alle Hive-taken voordat het verkleinen van HDInsight. U kunt ook de schaal omlaag om te voorkomen dat conflicterende met het uitvoeren van Hive-taken plannen.
* Handmatig opschonen van Hive begin `tmp` directory bestanden in HDFS voordat het verkleinen.
* Alleen terugschroeven HDInsight met drie werkrolknooppunten, minimale. Voorkomen dat u minimaal één werkrolknooppunt.
* Voer de opdracht te laten veilige modus bevindt, indien nodig.

De volgende secties worden deze opties.

#### <a name="stop-all-hive-jobs"></a>Alle Hive-taken stoppen

Stop alle Hive-taken voordat omlaag schalen met een werkrolknooppunt. Als uw werkbelasting is gepland, wordt uitgevoerd het omlaag schalen nadat Hive werk wordt uitgevoerd.

Dit helpt te beperken van het aantal werkbestanden in de map tmp (indien aanwezig).

#### <a name="manually-clean-up-hives-scratch-files"></a>Het Hive-werkbestanden handmatig opschonen

Geen veilige modus als Hive is achtergelaten tijdelijke bestanden, en vervolgens u handmatig die bestanden opruimen kunt voordat omlaag te schalen.

1. Component services stoppen en zorg ervoor dat alle query's en taken zijn voltooid.

2. De inhoud van de `hdfs://mycluster/tmp/hive/` directory om te zien of deze geen bestanden bevat:

    ```
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```
    
    Hier volgt een voorbeeld van uitvoer wanneer bestanden aanwezig zijn:

    ```
    sshuser@hn0-scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

3. Als u dat hive is klaar met deze bestanden weet, kunt u deze verwijderen. Zorg ervoor dat Hive heeft geen query uitvoeren door te kijken in de gebruikersinterface van Yarn ResourceManager-pagina.

    Voorbeeld van de opdrachtregel om bestanden te verwijderen uit HDFS:

    ```
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```
    
#### <a name="scale--hdinsight-to-three-worker-nodes"></a>Schaal HDInsight met drie werkrolknooppunten

Als zitten in de veilige modus is een permanente probleem en de vorige stappen zijn niet-opties en vervolgens u het probleem te voorkomen kunt door alleen schalen naar beneden op drie worker-knooppunten. Dit is mogelijk niet optimaal vanwege kostenbeperkingen, in vergelijking met één knooppunt omlaag schalen. Echter, met slechts één knooppunt van de werknemer, HDFS kan niet garanderen drie replica's van de gegevens beschikbaar zijn voor het cluster.

#### <a name="run-the-command-to-leave-safe-mode"></a>Voer de opdracht te laten veilige modus

De laatste mogelijkheid is om de zeldzame gevallen waarin HDFS veilige modus invoert en vervolgens de veilige modus laat opdracht niet uitvoeren. Als u hebt vastgesteld dat de reden HDFS heeft ingevoerd de veilige modus is vanwege de Hive-bestanden wordt under-gerepliceerde, voert u de volgende opdracht te laten veilige modus:

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
* [Scale-clusters](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [HDInsight-clusters beheren met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md)

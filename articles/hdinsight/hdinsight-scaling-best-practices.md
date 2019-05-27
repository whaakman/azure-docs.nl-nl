---
title: Schaal cluster formaat, Azure HDInsight
description: Een Azure HDInsight-cluster elastisch zodat deze overeenkomt met uw werkbelasting schalen.
author: ashishthaps
ms.author: ashish
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: 622261d0f7e602635aa6a638357278a9c63a6ecd
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990541"
---
# <a name="scale-hdinsight-clusters"></a>HDInsight-clusters schalen

HDInsight biedt flexibiliteit doordat u de optie voor omhoog en omlaag het aantal worker-knooppunten in uw clusters schalen. Deze flexibiliteit, kunt u verkleinen van een cluster nadat uur of in het weekend en vouwt u het tijdens piek bedrijfsbehoeften te voldoen.

Als u periodieke batchverwerking, kan het HDInsight-cluster worden opgeschaald een paar minuten vóór deze bewerking, zodat uw cluster voldoende geheugen en CPU-kracht heeft.  Later, nadat de verwerking is voltooid en gebruik opnieuw uitvalt, kunt u schalen u het HDInsight-cluster met minder werkrolknooppunten.

U kunt een cluster handmatig met behulp van een van de methoden worden hieronder beschreven, of gebruik [voor automatisch schalen](hdinsight-autoscale-clusters.md) opties om het systeem automatisch omhoog en omlaag schalen in reactie op CPU, geheugen en andere metrische gegevens.

## <a name="utilities-to-scale-clusters"></a>Hulpprogramma's voor het schalen van clusters

Microsoft biedt de volgende hulpprogramma's voor het schalen van clusters:

|Hulpprogramma | Description|
|---|---|
|[PowerShell Az](https://docs.microsoft.com/powershell/azure)|[Set-AzHDInsightClusterSize](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightclustersize) - clusternaam \<Clusternaam > - TargetInstanceCount \<NewSize >|
|[PowerShell AzureRM](https://docs.microsoft.com/powershell/azure/azurerm) |[Set-AzureRmHDInsightClusterSize](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize) - clusternaam \<Clusternaam > - TargetInstanceCount \<NewSize >|
|[Azure-CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)| [AZ hdinsight vergroten of verkleinen](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) --resourcegroep \<resourcegroep >--naam \<Clusternaam >--target-instance-count \<NewSize >|
|[Azure-CLI](hdinsight-administer-use-command-line.md)|Azure hdinsight-cluster formaat \<clusterName > \<doel exemplaren > |
|[Azure Portal](https://portal.azure.com)|Uw HDInsight-cluster deelvenster te openen, selecteert u **clustergrootte** in het menu links en vervolgens in het deelvenster van de grootte van Cluster, typt u in het aantal worker-knooppunten en selecteer opslaan.|  

![Cluster schalen](./media/hdinsight-scaling-best-practices/scale-cluster-blade.png)

Met behulp van een van deze methoden, kunt u uw HDInsight-cluster omhoog of omlaag schalen binnen enkele minuten.

> [!IMPORTANT]  
> * De klassieke Azure-CLI is verouderd en moet alleen worden gebruikt met het klassieke implementatiemodel. Voor alle andere implementaties, gebruikt u de [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).  
> * De PowerShell AzureRM-module is afgeschaft.  Gebruik de [Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) indien mogelijk.

## <a name="impact-of-scaling-operations"></a>Gevolgen van omhoog schalen van bewerkingen

Wanneer u **toevoegen** knooppunten aan uw actief HDInsight-cluster (Omhoog schalen), in behandeling of actieve taken worden niet beïnvloed. Nieuwe taken kunnen veilig worden verzonden terwijl het proces voor vergroten/verkleinen wordt uitgevoerd. Als de vergroten/verkleinen bewerking om een bepaalde reden mislukt, wordt de fout wordt afgehandeld als u wilt laten van uw cluster in een functionele staat.

Als u **verwijderen** knooppunten (neerschalen), een wordt uitgevoerd of in behandeling zijnde taken mislukken wanneer de vergroten/verkleinen bewerking is voltooid. Deze fout wordt veroorzaakt door een van de services opnieuw te starten tijdens het vergroten/verkleinen. Er is ook een risico dat uw cluster vastgelopen in veilige modus krijgt tijdens een handmatige bewerking schalen.

## <a name="how-to-safely-scale-down-a-cluster"></a>Hoe u veilig omlaag een cluster schalen

### <a name="scale-down-a-cluster-with-running-jobs"></a>Verkleinen van een cluster met het uitvoeren van taken

Om te voorkomen dat uw taken die worden uitgevoerd mislukken tijdens een bewerking omlaag schalen, kunt u proberen drie dingen:

1. Wacht tot de taken zijn voltooid voordat u uw cluster verkleint.
1. De taken handmatig beëindigen.
1. De taken opnieuw nadat de bewerking vergroten/verkleinen is gesloten.

U ziet een lijst van in behandeling en taken die worden uitgevoerd, kunt u de YARN **ResourceManager UI**, volgende stappen:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
2. Aan de linkerkant, gaat u naar **alle services** > **Analytics** > **HDInsight-Clusters**, en selecteer vervolgens uw cluster.
3. Vanuit de hoofdweergave, navigeert u naar **Clusterdashboards** > **Ambari home**. Voer de clusterreferenties van uw.
4. Selecteer in de UI Ambari **YARN** in de lijst met services in het menu links.  
5. Selecteer in de pagina YARN **snelkoppelingen** en beweeg de muisaanwijzer over de actieve hoofdknooppunt en selecteer vervolgens **ResourceManager UI**.

    ![ResourceManager UI](./media/hdinsight-scaling-best-practices/resourcemanager-ui.png)

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

### <a name="getting-stuck-in-safe-mode"></a>Zitten in de veilige modus

Wanneer u omlaag een cluster schaalt, HDInsight maakt gebruik van Apache Ambari-beheerinterfaces eerst uit bedrijf nemen de extra worker-knooppunten, die hun HDFS-blokken worden gerepliceerd naar andere online worker-knooppunten. Hierna schaalt HDInsight veilig het cluster omlaag. HDFS gaat in de veilige modus tijdens het vergroten/verkleinen, en moet afkomstig zijn uit als de schaal is voltooid. In sommige gevallen echter HDFS vastloopt in de veilige modus tijdens een bewerking voor vergroten/verkleinen vanwege een te voorzichtige bestandsreplicatie blokkeren.

HDFS is standaard geconfigureerd met een `dfs.replication` van 3, waarmee wordt bepaald hoeveel kopieën van elk bestand blok beschikbaar zijn. Elk exemplaar van een bestand wordt opgeslagen op een ander knooppunt van het cluster.

Wanneer u HDFS detecteert dat het verwachte aantal blok kopieën zijn niet beschikbaar, HDFS in de veilige modus geplaatst en Ambari genereert waarschuwingen. Als HDFS in de veilige modus voor een bewerking voor vergroten/verkleinen geplaatst, maar kan niet veilige modus sluit omdat het vereiste aantal knooppunten worden niet gedetecteerd voor replicatie, kan het cluster zijn vastgelopen in de veilige modus.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Voorbeeld van de fouten wanneer de veilige modus is ingeschakeld

```
org.apache.hadoop.hdfs.server.namenode.SafeModeException: Cannot create directory /tmp/hive/hive/819c215c-6d87-4311-97c8-4f0b9d2adcf0. Name node is in safe mode.
```

```
org.apache.http.conn.HttpHostConnectException: Connect to hn0-clustername.servername.internal.cloudapp.net:10001 [hn0-clustername.servername. internal.cloudapp.net/1.1.1.1] failed: Connection refused
```

U kunt de naam van knooppuntlogboeken van bekijken de `/var/log/hadoop/hdfs/` map in de buurt van de tijd wanneer het cluster is geschaald om te zien wanneer deze veilige modus hebt ingevoerd. De naam van de logboekbestanden `Hadoop-hdfs-namenode-hn0-clustername.*`.

De hoofdoorzaak van de vorige fouten is dat Hive afhankelijk van tijdelijke bestanden in HDFS is tijdens het uitvoeren van query's. Wanneer u HDFS in veilige modus, kan niet Hive query's uitvoeren omdat er niet op HDFS. De tijdelijke bestanden in HDFS bevinden zich in het lokale station gekoppeld aan de afzonderlijke worker-knooppunt VM's en gerepliceerd met andere worker-knooppunten op drie replica's, minimaal.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode"></a>Voorkomen dat HDInsight zitten in de veilige modus

Er zijn verschillende manieren om te voorkomen dat HDInsight wordt links in de veilige modus:

* Stop alle Hive-taken voor het verkleinen van HDInsight. U kunt ook de schaal omlaag om te voorkomen dat er bij het uitvoeren van Hive-taken plannen.
* Handmatig opschonen van Hive volledig nieuwe `tmp` directory-bestanden in HDFS voordat het omlaag schalen.
* Alleen verkleinen HDInsight met drie werkrolknooppunten, minimum. Voorkomen dat u slechts één worker-knooppunt.
* Voer de opdracht uit om te laten veilige modus, indien nodig.

De volgende secties worden deze opties.

#### <a name="stop-all-hive-jobs"></a>Alle Hive-taken stoppen

Alle Hive-taken stoppen voordat het omlaag schalen naar een worker-knooppunt. Als uw werkbelasting is gepland, Voer uw omlaag schalen wanneer Hive werk is voltooid.

Stoppen van de Hive-taken voor het schalen, minimaliseren helpt het aantal nieuwe bestanden in de map tmp (indien aanwezig).

#### <a name="manually-clean-up-hives-scratch-files"></a>Hive van tijdelijke bestanden handmatig opschonen

Als Hive heeft verlaten achter tijdelijke bestanden, en vervolgens u handmatig die bestanden opschonen kunt voordat u omlaag geschaald naar voorkomen dat de veilige modus.

1. Controleer welke locatie wordt gebruikt voor de tijdelijke bestanden Hive door te kijken de `hive.exec.scratchdir` configuratie-eigenschap. Deze parameter is ingesteld in `/etc/hive/conf/hive-site.xml`:

    ```xml
    <property>
        <name>hive.exec.scratchdir</name>
        <value>hdfs://mycluster/tmp/hive</value>
    </property>
    ```

1. Hive-services stoppen en zorg ervoor dat alle query's en taken zijn voltooid.
2. De inhoud van de bovenstaande gevonden scratchdirectory `hdfs://mycluster/tmp/hive/` om te zien als deze bestanden bevat:

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

#### <a name="scale-hdinsight-to-three-or-more-worker-nodes"></a>HDInsight met drie of meer worker-knooppunten schalen

Als uw clusters in de veilige modus zitten vaak wanneer u omlaag naar minder dan drie worker-knooppunten en de vorige stappen niet werken, kunt u uw cluster gaan op de veilige modus helemaal door de beveiliging van ten minste drie werkknooppunten te voorkomen.

Behoud van drie worker-knooppunten is duurder dan het omlaag schalen naar slechts één worker-knooppunt, maar deze wordt voorkomen dat uw cluster zitten in de veilige modus.

#### <a name="run-the-command-to-leave-safe-mode"></a>Voer de opdracht uit om te laten veilige modus

De laatste mogelijkheid is de veilige modus laat opdracht uit te voeren. Als u weet dat de reden voor het invoeren van de veilige modus HDFS vanwege een te voorzichtige bestandsreplicatie Hive is, kunt u de volgende opdracht uit om te laten veilige modus uitvoeren:


```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

### <a name="scale-down-an-apache-hbase-cluster"></a>Omlaag een Apache HBase-cluster schalen

Regioservers worden automatisch verdeeld binnen een paar minuten na het voltooien van een bewerking voor vergroten/verkleinen. Handmatig evenwichtige regioservers, voert u de volgende stappen uit:

1. Verbinding maken met het HDInsight-cluster via SSH. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Start de HBase-shell:

    ```bash
    hbase shell
    ```

3. Gebruik de volgende opdracht om handmatig de regioservers:

    ```bash
    balancer
    ```

## <a name="next-steps"></a>Volgende stappen

* [Automatisch schalen van Azure HDInsight-clusters](hdinsight-autoscale-clusters.md)
* [Inleiding tot Azure HDInsight](hadoop/apache-hadoop-introduction.md)
* [Clusters schalen](hdinsight-administer-use-portal-linux.md#scale-clusters)

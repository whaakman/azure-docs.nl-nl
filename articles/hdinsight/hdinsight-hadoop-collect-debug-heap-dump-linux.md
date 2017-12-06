---
title: Heap dumpbestanden voor Hadoop op HDInsight - Azure-services inschakelen | Microsoft Docs
description: Schakel heap dumpbestanden voor Hadoop-services voor foutopsporing en analyse van Linux gebaseerde HDInsight-clusters.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8f151adb-f687-41e4-aca0-82b551953725
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: larryfr
ms.openlocfilehash: dcc04e5bba28d0cb32e8633542ab8d3c125003ec
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a>Dumpbestanden voor Hadoop-services op Linux gebaseerde HDInsight heap inschakelen

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Dumpbestanden voor heap bevatten een momentopname van de toepassing geheugen, met inbegrip van de waarden van variabelen op het moment dat de dump is gemaakt. Ze zijn daarom nuttig voor het oplossen van problemen die tijdens runtime optreden.

> [!IMPORTANT]
> De stappen in dit document wordt alleen werken met HDInsight-clusters die gebruikmaken van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="whichServices"></a>Services

U kunt de heap dumpbestanden voor de volgende services inschakelen:

* **hcatalog** -tempelton
* **hive** -hiveserver2, metastore, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resourcemanager, nodemanager, timelineserver
* **hdfs** -datanode, secondarynamenode, namenode

U kunt ook heap dumpbestanden voor de kaart inschakelen en de processen die worden uitgevoerd door HDInsight.

## <a name="configuration"></a>Understanding heap dump configuratie

Dumpbestanden voor heap zijn ingeschakeld door de opties (ook wel aangeduid als kiest, of parameters) naar de JVM wanneer een service wordt gestart. Voor de meeste Hadoop-services, kunt u de shell-script gebruikt voor het starten van de service voor het doorgeven van deze opties wijzigen.

In elk script is een exporteren voor  **\* \_OPTS**, die de opties die is doorgegeven aan de JVM bevat. Bijvoorbeeld, in de **hadoop env.sh** script, de regel die begint met `export HADOOP_NAMENODE_OPTS=` bevat de opties voor de service NameNode.

Overzicht en verminderen processen zijn enigszins verschillen, omdat deze bewerkingen een onderliggend proces van de MapReduce-service zijn. Elk toewijzen of verminder proces wordt uitgevoerd in een container onderliggend en er zijn twee items die de JVM-opties bevatten. Beide opgenomen in **mapred site.xml**:

* **mapreduce.Admin.map.child.Java.opts**
* **mapreduce.Admin.reduce.child.Java.opts**

> [!NOTE]
> U wordt aangeraden met Ambari scripts en mapred site.xml-instellingen wijzigen als de Ambari-ingang wijzigingen te repliceren tussen knooppunten in het cluster. Zie de [Ambari met behulp van](#using-ambari) sectie voor specifieke stappen.

### <a name="enable-heap-dumps"></a>Heapdumps inschakelen

De volgende optie kunt heap dumpbestanden wanneer er een OutOfMemoryError optreedt:

    -XX:+HeapDumpOnOutOfMemoryError

De  **+**  geeft aan dat deze optie is ingeschakeld. De standaardwaarde is uitgeschakeld.

> [!WARNING]
> Heap dumpbestanden zijn niet ingeschakeld voor services van Hadoop in HDInsight standaard als de dumpbestanden kunnen oplopen. Als u deze voor het oplossen van inschakelt, moet u deze uitschakelen wanneer u het probleem kunnen reproduceren en de dumpbestanden verzameld.

### <a name="dump-location"></a>Locatie van de dump

De standaardlocatie voor het bestand is de huidige werkmap. U kunt bepalen waar het bestand is opgeslagen met de volgende optie:

    -XX:HeapDumpPath=/path

Bijvoorbeeld, met behulp van `-XX:HeapDumpPath=/tmp` zorgt ervoor dat de dumpbestanden moeten worden opgeslagen in de map van de map.

### <a name="scripts"></a>Scripts

U kunt ook een script activeren wanneer een **OutOfMemoryError** optreedt. Bijvoorbeeld, activeren een melding zodat u weet dat de fout is opgetreden. Gebruik de volgende optie voor het activeren van een script op een __OutOfMemoryError__:

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> Aangezien Hadoop een gedistribueerd systeem is, moet elk script dat wordt gebruikt op alle knooppunten in het cluster dat de service wordt uitgevoerd op worden geplaatst.
> 
> Het script moet ook worden op een locatie die toegankelijk is voor het account van de service wordt uitgevoerd en moet machtigingen voor uitvoeren. Bijvoorbeeld, u kunt desgewenst voor het opslaan van scripts in `/usr/local/bin` en gebruik `chmod go+rx /usr/local/bin/filename.sh` verleent u lees-en uitvoermachtigingen worden verleend.

## <a name="using-ambari"></a>Ambari gebruiken

Gebruik de volgende stappen voor het wijzigen van de configuratie voor een service:

1. Open de Ambari-webgebruikersinterface voor uw cluster. De URL is https://YOURCLUSTERNAME.azurehdinsight.net.

    Wanneer u wordt gevraagd, geverifieerd bij de site met de naam van de HTTP-account (standaard: admin) en het wachtwoord voor uw cluster.

   > [!NOTE]
   > U mogelijk gevraagd een tweede maal door Ambari voor de gebruikersnaam en wachtwoord. Als dit het geval is, voer de dezelfde accountnaam en wachtwoord

2. Met behulp van de lijst met aan de linkerkant, selecteert u het gebied van de service dat u wilt wijzigen. Bijvoorbeeld: **HDFS**. Selecteer in het gebied center de **Configs** tabblad.

    ![Afbeelding van Ambari web met HDFS Configs tabblad is geselecteerd](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. Met behulp van de **Filter...**  -item, voer **kiest**. Alleen items met deze tekst worden weergegeven.

    ![Gefilterde lijst](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. Zoek de  **\* \_OPTS** vermelding voor de service die u wilt inschakelen heap dumpbestanden voor en voeg de opties die u wilt inschakelen. Ik heb toegevoegd in de volgende afbeelding `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` naar de **HADOOP\_NAMENODE\_OPTS** post:

    ![HADOOP_NAMENODE_OPTS met - XX: + HeapDumpOnOutOfMemoryError - XX: HeapDumpPath = / tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > Wanneer het inschakelen van heap dumpbestanden voor de kaart of verminder onderliggend proces zoekt u naar voor de velden met de naam **mapreduce.admin.map.child.java.opts** en **mapreduce.admin.reduce.child.java.opts**.

    Gebruik de **opslaan** knop de wijzigingen wilt opslaan. U kunt een korte notitie met een beschrijving van de wijzigingen.

5. Nadat de wijzigingen zijn toegepast, de **opnieuw opstarten vereist** pictogram wordt weergegeven naast een of meer services.

    ![opnieuw opstarten vereist het pictogram en start opnieuw op de knop](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. Selecteer elke service die u een herstart moet en gebruik de **serviceacties** knop **inschakelen op onderhoudsmodus**. Onderhoudsmodus wordt voorkomen dat waarschuwingen van de service wordt gegenereerd wanneer u deze opnieuw starten.

    ![Menu Onderhoud-modus inschakelen](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. Zodra u de onderhoudsmodus hebt ingeschakeld, gebruikt u de **opnieuw** knop voor de service voor **alle verricht opnieuw starten**

    ![Start opnieuw alle van invloed op een vermelding](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > de vermeldingen voor de **opnieuw** knop kan afwijken voor andere services.

8. Zodra de services opnieuw zijn opgestart, gebruikt u de **serviceacties** knop **inschakelen uit onderhoudsmodus**. Deze Ambari hervatten bewaking voor waarschuwingen voor de service.


---
title: Azure Data Lake Store-prestaties afstemmen richtlijnen | Microsoft Docs
description: Azure Data Lake Store-prestaties afstemmen richtlijnen
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: cgronlun
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: stewu
ms.openlocfilehash: 15832f94b73057a8bfce7be27e3fd57c7771940d
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="tuning-azure-data-lake-store-for-performance"></a>Azure Data Lake Store voor prestaties afstemmen

Data Lake Store ondersteunt hoge gegevensdoorvoer voor i/o-intensieve analyse en gegevensverplaatsing.  In Azure Data Lake Store is het belangrijk om de beste prestaties met behulp van alle beschikbare doorvoer – de hoeveelheid gegevens die kunnen worden gelezen of geschreven per seconde –.  Dit wordt bereikt door het uitvoeren van zoveel leest en schrijft parallel mogelijk.

![Data Lake Store-prestaties](./media/data-lake-store-performance-tuning-guidance/throughput.png)

Azure Data Lake Store kunnen worden geschaald om de benodigde doorvoer voor alle analytics scenario. Een Azure Data Lake Store-account biedt standaard automatisch voldoende doorvoer om te voldoen aan de behoeften van een categorie gebruiksvoorbeelden. Voor de gevallen waarin klanten wordt uitgevoerd in de standaardlimiet worden ADLS-account geconfigureerd om meer doorvoer contact opnemen met Microsoft ondersteuning.

## <a name="data-ingestion"></a>Gegevensopname

Bij het ophalen van gegevens uit een bronsysteem naar ADLS, is het belangrijk in de bron hardware, de netwerkhardware bron en de netwerkverbinding met ADLS het knelpunt kunnen worden.  

![Data Lake Store-prestaties](./media/data-lake-store-performance-tuning-guidance/bottleneck.png)

Het is belangrijk om ervoor te zorgen dat de gegevensverplaatsing wordt niet beïnvloed door deze factoren.

### <a name="source-hardware"></a>Bron-Hardware

Of u van lokale machines of virtuele machines in Azure gebruikmaakt, moet u zorgvuldig de juiste hardware. HDD's van SSD's liever voor bron schijfhardware, en kies vervolgens schijfhardware met sneller aandrijfassen. Voor bron netwerkhardware, gebruikt u de snelste NIC's mogelijk.  In Azure, wordt aangeraden Azure D14 virtuele machines die op de juiste wijze krachtige schijf- en netwerkhardware.

### <a name="network-connectivity-to-azure-data-lake-store"></a>Netwerkverbindingen met Azure Data Lake Store

Het knelpunt kan soms worden door de netwerkverbinding tussen de brongegevens en Azure Data Lake store. Wanneer de brongegevens On-Premises is, overweeg het gebruik van een specifieke verbinding met [Azure ExpressRoute](https://azure.microsoft.com/en-us/services/expressroute/) . Als de brongegevens in Azure, de prestaties worden aanbevolen als de gegevens zich in dezelfde Azure-regio als de Data Lake Store.

### <a name="configure-data-ingestion-tools-for-maximum-parallelization"></a>Gegevensopname hulpprogramma's voor maximale garandeert configureren

Nadat u de bron-hardware hebt opgelost en network connectivity knelpunten hierboven, bent u klaar voor het configureren van de opname-hulpprogramma's. De volgende tabel bevat een overzicht van de instellingen voor de sleutel voor verschillende hulpprogramma's van populaire opname en biedt uitgebreide prestaties artikelen voor hen afstemmen.  Voor meer informatie over het hulpprogramma dat moet worden gebruikt voor uw scenario, gaat u naar dit [artikel](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-scenarios).

| Hulpprogramma               | Instellingen     | Meer informatie                                                                 |
|--------------------|------------------------------------------------------|------------------------------|
| PowerShell       | PerFileThreadCount, ConcurrentFileCount |  [Koppeling](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-powershell#performance-guidance-while-using-powershell) |
| AdlCopy    | Azure Data Lake Analytics-eenheden  |   [Koppeling](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob#performance-considerations-for-using-adlcopy)         |
| DistCp            | -m (toewijzen)   | [Koppeling](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-wasb-distcp#performance-considerations-while-using-distcp)                             |
| Azure Data Factory| parallelCopies    | [Koppeling](../data-factory/copy-activity-performance.md)                          |
| Sqoop           | FS.Azure.Block.Size, -m (toewijzen)    |   [Koppeling](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)        |

## <a name="structure-your-data-set"></a>Structuur van uw gegevensset

Wanneer gegevens worden opgeslagen in Data Lake Store, de grootte van het aantal bestanden en de mappenstructuur van invloed zijn op de prestaties.  De volgende sectie beschreven procedures in de volgende gebieden.  

### <a name="file-size"></a>Bestandsgrootte

Analytics-engines zoals HDInsight en Azure Data Lake Analytics hebben doorgaans een overhead per bestand.  Als u uw gegevens zo veel kleine bestanden opslaat, kan dit de prestaties negatief beïnvloeden.  

Uw gegevens in het algemeen zijn ingedeeld in grotere grootte bestanden voor betere prestaties.  Als een vuistregel organiseert gegevenssets in bestanden van 256MB of hoger. In sommige gevallen, zoals afbeeldingen en binaire gegevens is het niet mogelijk ze parallel te verwerken.  In dergelijke gevallen is het aanbevolen om minder dan 2GB voor afzonderlijke bestanden te houden.

Soms beperkt gegevenspijplijnen controle over de onbewerkte gegevens met een groot aantal kleine bestanden.  Het is raadzaam een 'koken' proces die grotere bestanden moet worden gebruikt voor downstream toepassingen worden gegenereerd.  

### <a name="organizing-time-series-data-in-folders"></a>Time Series-gegevens in mappen ordenen

Hive en ADLA werkbelastingen kunt partitie verwijderen van de tijdreeks gegevens enkele query's bij het lezen van slechts een subset van de gegevens die zorgt voor betere prestaties.    

Deze pijplijnen die timeseries gegevens opnemen, plaatst u vaak hun bestanden met een zeer gestructureerd naamgeving voor bestanden en mappen. Hieronder vindt u een voorbeeld van een zeer gangbaar zien we voor gegevens die is geordend op datum:

    \DataSet\YYYY\MM\DD\datafile_YYYY_MM_DD.tsv

U ziet dat de datum/tijd-gegevens worden weergegeven als de mappen en in de bestandsnaam.

Voor de datum en tijd waarop is het volgende een algemene patroon

    \DataSet\YYYY\MM\DD\HH\mm\datafile_YYYY_MM_DD_HH_mm.tsv

Opnieuw de keuze die u maakt met de map en Bestandsorganisatie moet optimaliseren voor een redelijke aantal bestanden in elke map en de bestandsgrootte.

## <a name="optimizing-io-intensive-jobs-on-hadoop-and-spark-workloads-on-hdinsight"></a>I/o-intensieve taken op de werkbelastingen voor Hadoop en Spark in HDInsight optimaliseren

Taken kunnen worden onderverdeeld in een van de volgende drie categorieën:

* **CPU-intensief.**  Deze taken zijn lang berekening tijdstippen met minimale i/o-tijden.  Voorbeelden zijn machine learning en natuurlijke taal taken verwerken.  
* **Geheugenintensief.**  Deze taken veel geheugen gebruiken.  Voorbeelden zijn PageRank en realtime analytics-taken.  
* **I/o-intensieve.**  Deze taken te besteden aan de meeste van de tijd die tijdens het doorzoeken van i/o.  Een veelvoorkomend voorbeeld is een kopieertaak die alleen lezen en schrijfbewerkingen.  Andere voorbeelden zijn gegevens voorbereiding taken die veel gegevens lezen, voert de gegevenstransformatie van sommige uit en schrijft de gegevens terug naar de store.  

De volgende richtlijnen zijn alleen van toepassing op i/o-intensieve taken.

### <a name="general-considerations-for-an-hdinsight-cluster"></a>Algemene overwegingen voor een HDInsight-cluster

* **HDInsight-versies.** Gebruik de nieuwste versie van HDInsight voor de beste prestaties.
* **Regio's.** De Data Lake Store in dezelfde regio bevinden als het HDInsight-cluster plaatsen.  

Een HDInsight-cluster bestaat uit twee hoofdknooppunten en een aantal worker-knooppunten. Elk werkrolknooppunt biedt een aantal kernen en het geheugen dat wordt bepaald door het VM-type.  Wanneer een taak wordt uitgevoerd, is YARN de resource-onderhandelaar waarmee het beschikbare geheugen en kernen containers maken wordt toegewezen.  Elke container wordt uitgevoerd voor de taken die nodig zijn om de taak te voltooien.  Containers parallel worden verwerkt taken snel worden uitgevoerd. Daarom wordt de prestaties verbeterd door het uitvoeren van parallelle-containers zoveel mogelijk.

Er zijn drie lagen in een HDInsight-cluster kunnen worden afgestemd Verhoog het aantal containers en het gebruik van alle beschikbare doorvoer.  

* **Fysieke laag**
* **YARN-laag**
* **Laag van de werkbelasting**

### <a name="physical-layer"></a>Fysieke laag

**Voer een cluster met meer knooppunten en/of groter formaat virtuele machines.**  Een groter cluster, kunt u meer YARN-containers zoals weergegeven in de volgende afbeelding worden uitgevoerd.

![Data Lake Store-prestaties](./media/data-lake-store-performance-tuning-guidance/VM.png)

**Virtuele machines met meer netwerkbandbreedte gebruiken.**  De hoeveelheid netwerkbandbreedte kan een knelpunt zijn als er minder netwerkbandbreedte dan een Data Lake Store-doorvoer.  Andere virtuele machines hebben verschillende grootten voor netwerk-bandbreedte.  Kies een VM-type met de grootste mogelijke netwerkbandbreedte.

### <a name="yarn-layer"></a>YARN-laag

**Kleinere YARN-containers gebruiken.**  Reduceer de grootte van elke container YARN meer containers maken met dezelfde mate van resources.

![Data Lake Store-prestaties](./media/data-lake-store-performance-tuning-guidance/small-containers.png)

Afhankelijk van uw werkbelasting moet altijd er een YARN container minimumgrootte die nodig is. Als u een container te klein kiest, worden de taken worden uitgevoerd in-geheugen problemen. Doorgaans moet YARN containers niet kleiner zijn dan 1GB. Het is gebruikelijk om te zien van 3GB YARN containers. Voor sommige werkbelastingen moet u grotere YARN-containers.  

**Kernen per YARN-container verhogen.**  Verhoog het aantal kernen dat is toegewezen aan elke container om het aantal parallelle taken die worden uitgevoerd in elke container te vergroten.  Dit werkt voor toepassingen zoals Spark die meerdere taken per container worden uitgevoerd.  Voor toepassingen zoals Hive die een enkele thread in elke container worden uitgevoerd, is het beter om meer containers in plaats van meer kernen per container hebt.   

### <a name="workload-layer"></a>Laag van de werkbelasting

**Alle beschikbare containers gebruiken.**  Stel het aantal taken moet gelijk zijn of groter zijn dan het aantal beschikbare containers, zodat alle resources worden gebruikt.

![Data Lake Store-prestaties](./media/data-lake-store-performance-tuning-guidance/use-containers.png)

**Mislukte taken zijn kostbaar.** Als u elke taak heeft een grote hoeveelheid gegevens verwerken, klikt u vervolgens resulteert mislukken van een taak in een dure probeer het opnieuw.  Daarom is het beter om het maken van meer taken, die elk een kleine hoeveelheid gegevens verwerkt.

Naast de bovenstaande algemene richtlijnen heeft elke toepassing andere parameters die beschikbaar zijn om af te stemmen voor die specifieke toepassing. De volgende tabel bevat enkele van de parameters en koppelingen naar aan de slag met prestaties afstemmen voor elke toepassing.

| Workload               | Parameter om taken te stellen                                                         |
|--------------------|-------------------------------------------------------------------------------------|
| [Spark in HDInisight](data-lake-store-performance-tuning-spark.md)       | <ul><li>NUM Executor</li><li>Executor-geheugen</li><li>Executor kernen</li></ul> |
| [Hive in HDInsight](data-lake-store-performance-tuning-hive.md)    | <ul><li>hive.tez.container.Size</li></ul>         |
| [MapReduce in HDInsight](data-lake-store-performance-tuning-mapreduce.md)            | <ul><li>mapreduce.map.Memory</li><li>Mapreduce.job.Maps</li><li>mapreduce.reduce.Memory</li><li>Mapreduce.job.reduces</li></ul> |
| [Storm op HDInsight](data-lake-store-performance-tuning-storm.md)| <ul><li>Aantal werkprocessen</li><li>Aantal exemplaren van spout executor</li><li>Aantal exemplaren van bolt executor </li><li>Aantal spout taken</li><li>Aantal bolt taken</li></ul>|

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
* [Aan de slag met Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

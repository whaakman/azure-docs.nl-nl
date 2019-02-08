---
title: Azure Data Lake Storage Gen2 MapReduce prestaties richtlijnen over het afstemmen | Microsoft Docs
description: Azure Data Lake Storage Gen2 MapReduce prestaties richtlijnen over het afstemmen
services: storage
author: swums
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: stewu
ms.openlocfilehash: 1df7765f4f006d3d79f867fbf23929f6b9b0cf08
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884389"
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-storage-gen2"></a>Richtlijnen voor MapReduce in HDInsight en Azure Data Lake Storage Gen2 afstemmen van prestaties

Begrijp de factoren waarmee u rekening houden moet wanneer u de prestaties van Mapreduce-taken af te stemmen. In dit artikel bevat informatie over een scala aan richtlijnen voor afstemmen van de prestaties.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een account met Azure Data Lake Storage Gen2**. Zie voor instructies over het maken van een [Quick Start: Maken van een storage-account van Azure Data Lake Storage Gen2](data-lake-storage-quickstart-create-account.md).
* **Azure HDInsight-cluster** met toegang tot een Data Lake Storage Gen2-account. Zie [gebruik Azure Data Lake Storage Gen2 met Azure HDInsight-clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)
* **Met behulp van MapReduce in HDInsight**.  Zie voor meer informatie, [MapReduce in Hadoop op HDInsight gebruiken](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-mapreduce)
* **Richtlijnen voor het Data Lake Storage Gen2 afstemmen van prestaties**.  Zie voor de prestaties van de algemene concepten, [Data Lake Storage Gen2 prestaties afstemmen-richtlijnen](data-lake-storage-performance-tuning-guidance.md)

## <a name="parameters"></a>Parameters

Bij het uitvoeren van MapReduce-taken, vindt hier u de parameters die u configureren kunt voor betere prestaties op Data Lake Storage Gen2:

* **Mapreduce.map.Memory.MB** – de hoeveelheid geheugen worden toegewezen aan elke toewijzen
* **Mapreduce.job.Maps** : het aantal taken per taak kaart
* **Mapreduce.reduce.Memory.MB** – de hoeveelheid geheugen aan elke reducer toewijzen
* **Mapreduce.job.reduces** : het aantal taken per taak verminderen

**Mapreduce.map.Memory / Mapreduce.reduce.memory** dit nummer moet worden aangepast op basis van de hoeveelheid geheugen nodig is voor de kaart en/of beperken van de taak.  De standaardwaarden van mapreduce.map.memory en mapreduce.reduce.memory kunnen worden weergegeven in de Ambari via de Yarn-configuratie.  Navigeer naar YARN in Ambari, en naar het tabblad configuraties.  De YARN-geheugen wordt weergegeven.  

**Mapreduce.job.Maps / Mapreduce.job.reduces** dit bepaalt het maximum aantal mappers of reducers tegelijkertijd kunnen worden gemaakt.  Het aantal splitsingen, bepaalt hoeveel mappers voor de MapReduce-taak wordt gemaakt.  Daarom, krijgt u mogelijk minder mappers dan u hebt aangevraagd of er minder splitsingen dan het aantal mappers aangevraagd zijn.       

## <a name="guidance"></a>Richtlijnen

> [!NOTE]
> De instructies in dit document wordt ervan uitgegaan dat uw toepassing is de enige toepassing die wordt uitgevoerd op uw cluster.

**Stap 1: Aantal actieve taken bepalen**

Standaard MapReduce het hele cluster gebruikt voor uw taak.  U kunt minder van het cluster met behulp van minder mappers dan er beschikbaar containers.        

**Stap 2: Mapreduce.map.memory/mapreduce.reduce.memory instellen**

De grootte van het geheugen voor de kaart en verminder taken zijn afhankelijk van uw specifieke taak.  U kunt de geheugengrootte verminderen als u wilt verhogen gelijktijdigheid.  Het aantal gelijktijdig uitvoeren van taken, is afhankelijk van het aantal containers.  De hoeveelheid geheugen per toewijzen of reducer verlaagt, kunnen meer containers worden gemaakt, waarmee meer mappers en reducers tegelijkertijd kunnen gelijktijdig worden uitgevoerd.  De hoeveelheid geheugen te veel verlagen, kan dit ertoe leiden dat bepaalde processen te weinig geheugen.  Als u een heap-fout optreedt bij het uitvoeren van uw taak, moet u de hoeveelheid geheugen per toewijzen of reducer vergroten.  U moet rekening houden met toevoegen van meer containers wordt toegevoegd extra overhead voor elke extra container, zodat mogelijk de prestaties kan verslechteren.  Een ander alternatief is om op te halen van meer geheugen met behulp van een cluster met grotere hoeveelheden geheugen of het aantal knooppunten in uw cluster verhoogt.  Meer geheugen kunnen meer containers kunnen worden gebruikt, wat betekent dat meer gelijktijdigheid.  

**Stap 3: Bepalen van de totale YARN-geheugen**

Om te stemmen mapreduce.job.maps/mapreduce.job.reduces, moet u rekening houden met de hoeveelheid totale YARN-geheugen beschikbaar voor gebruik.  Deze informatie is beschikbaar in Ambari.  Navigeer naar YARN en weergeven van het tabblad configuraties.  De YARN-geheugen wordt weergegeven in dit venster.  U moet de YARN-geheugen met het aantal knooppunten in uw cluster om op te halen van het totale geheugen van de YARN vermenigvuldigen.

    Total YARN memory = nodes * YARN memory per node

Als u een lege cluster gebruikt, kan het totale YARN-geheugen voor uw cluster zijn op geheugen.  Als u andere toepassingen gebruikt geheugen, kunt klikt u vervolgens u alleen een gedeelte van het geheugen van uw cluster worden gebruikt door het aantal mappers of reducers tegelijkertijd beperken tot het aantal containers dat u wilt gebruiken.  

**Stap 4: Berekenen van de YARN-containers**

YARN-containers dicteren de hoeveelheid gelijktijdigheid beschikbaar voor de taak.  Totale hoeveelheid geheugen voor YARN nemen en die wordt gedeeld door mapreduce.map.memory.  

    # of YARN containers = total YARN memory / mapreduce.map.memory

**Stap 5: Set mapreduce.job.maps/mapreduce.job.reduces**

Mapreduce.job.maps/mapreduce.job.reduces ingesteld op ten minste het aantal beschikbare containers.  U kunt experimenteren verder door het aantal mappers en reducers tegelijkertijd om te zien of u betere prestaties krijgt.  Houd er rekening mee dat meer mappers extra overhead hebben om dat te veel mappers hoeft de prestaties nadelig beïnvloeden.  

CPU-plannings- en CPU-isolatie zijn standaard uitgeschakeld zodat het nummer van de YARN-containers wordt beperkt door het geheugen.

## <a name="example-calculation"></a>Voorbeeld van de berekening

Stel dat we een cluster bestaat uit 8 D14 knooppunten hebben en we willen een i/o-intensieve taak uit te voeren.  Hier volgen de berekeningen die u moet doen:

**Stap 1: Aantal actieve taken bepalen**

In dit voorbeeld gaan we ervan uit dat de taak is de enige taak die wordt uitgevoerd.  

**Stap 2: Mapreduce.map.memory/mapreduce.reduce.memory instellen**

In dit voorbeeld we een i/o-intensieve taak worden uitgevoerd en besluit 3GB aan geheugen voor de taken van de kaart is voldoende.

    mapreduce.map.memory = 3GB

**Stap 3: Bepalen van de totale YARN-geheugen**

    Total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**Stap 4: Berekenen # van YARN-containers**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

**Stap 5: Set mapreduce.job.maps/mapreduce.job.reduces**

    mapreduce.map.jobs = 256

## <a name="examples-to-run"></a>Voorbeelden om uit te voeren

Om te demonstreren hoe MapReduce op Data Lake Storage Gen2 wordt uitgevoerd, klikt u hieronder volgt een voorbeeld van code die is uitgevoerd op een cluster met de volgende instellingen:

* 16 knooppunten D14v2
* Hadoop-cluster met HDI 3.6

Hier volgen enkele Voorbeeldopdrachten MapReduce Teragen, Terasort en Teravalidate uit te voeren voor een beginpunt.  U kunt deze opdrachten op basis van uw resources aanpassen.

**Teragen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 abfs://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 abfs://example/data/1TB-sort-input abfs://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 abfs://example/data/1TB-sort-output abfs://example/data/1TB-sort-validate

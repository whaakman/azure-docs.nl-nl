---
title: Azure Data Lake Storage Gen2 Spark prestaties richtlijnen over het afstemmen | Microsoft Docs
description: Azure Data Lake Storage Gen2 Spark prestaties richtlijnen over het afstemmen
services: storage
author: swums
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: stewu
ms.openlocfilehash: 8be8fa68b48257a8d94d3ba6364d47c522bbf3de
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54401993"
---
# <a name="performance-tuning-guidance-for-spark-on-hdinsight-and-azure-data-lake-storage-gen2"></a>Richtlijnen voor Spark in HDInsight en Azure Data Lake Storage Gen2 afstemmen van prestaties

Bij het afstemmen van prestaties van Spark, moet u rekening houden met het aantal apps dat wordt uitgevoerd op uw cluster.  Standaard kunt u 4 uitvoeren apps gelijktijdig op uw HDI-cluster (Opmerking: de standaardinstelling is onderhevig aan wijzigingen).  U kunt besluiten te minder apps gebruiken, zodat u kunt de standaardinstellingen negeren en meer van het cluster voor deze apps gebruikt.  

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een account met Azure Data Lake Storage Gen2**. Zie voor instructies over het maken van een [Quick Start: Maken van een storage-account van Azure Data Lake Storage Gen2](data-lake-storage-quickstart-create-account.md).
* **Azure HDInsight-cluster** met toegang tot een Data Lake Storage Gen2-account. Zorg ervoor dat u extern bureaublad inschakelen voor het cluster. 
* **Spark-cluster uitgevoerd op Data Lake Storage Gen2**.  Zie voor meer informatie, [gebruik HDInsight Spark-cluster voor het analyseren van gegevens in Data Lake Storage Gen2](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-use-with-data-lake-store)
* **Richtlijnen voor het Data Lake Storage Gen2 afstemmen van prestaties**.  Zie voor de prestaties van de algemene concepten, [Data Lake Storage Gen2 prestaties afstemmen-richtlijnen](data-lake-storage-performance-tuning-guidance.md) 

## <a name="parameters"></a>Parameters

Wanneer de Spark-taken uitvoert, moet u hier de belangrijkste instellingen die kunnen worden afgestemd voor betere prestaties op Data Lake Storage Gen2 zijn:

* **NUM Executor** -het aantal gelijktijdige taken die kunnen worden uitgevoerd.

* **Executor-geheugen** -de hoeveelheid geheugen toegewezen aan elke executor.

* **Executor-cores** -het aantal kernen dat is toegewezen aan elke executor.                     

**NUM Executor** Num Executor stelt het maximum aantal taken dat parallel kan worden uitgevoerd.  Het werkelijke aantal taken dat parallel kan uitgevoerd wordt begrensd door het geheugen en CPU-resources die beschikbaar zijn in uw cluster.

**Executor-geheugen** dit is de hoeveelheid geheugen die aan elke executor is wordt toegewezen.  De hoeveelheid geheugen die nodig zijn voor elke executor is afhankelijk van de taak.  Voor complexe bewerkingen moet het geheugen hoger zijn.  Voor eenvoudige bewerkingen zoals lezen en schrijven, wordt de geheugenvereisten lager zijn.  De hoeveelheid geheugen voor elke executor kan worden weergegeven in de Ambari.  Ambari, Spark naar en het tabblad configuraties.  

**Executor-cores** Hiermee stelt u het aantal kernen per executor, waarmee wordt bepaald hoe het aantal parallelle threads die kunnen worden uitgevoerd per executor gebruikt.  Bijvoorbeeld, als executor-cores = 2, en vervolgens elke executor 2 parallelle taken in de executor uitvoeren kunt.  De executor-cores nodig zijn afhankelijk van de taak.  Intensief i/o-taken vereisen geen een grote hoeveelheid geheugen per taak, zodat elke executor meer parallelle taken kan worden verwerkt.

Standaard worden twee virtuele kernen voor YARN gedefinieerd voor elke fysieke kern bij het uitvoeren van Spark op HDInsight.  Dit nummer biedt een goede balans tussen gelijktijdigheid en de hoeveelheid context overschakelen van meerdere threads.  

## <a name="guidance"></a>Richtlijnen

Tijdens het uitvoeren van Spark analytische workloads om te werken met gegevens in Data Lake Storage Gen2, raden wij aan dat u de meest recente versie van HDInsight om op te halen van de beste prestaties met Data Lake Storage Gen2. Wanneer de taak meer i/o-intensieve is, kunnen bepaalde parameters worden geconfigureerd om prestaties te verbeteren.  Data Lake Storage Gen2 is een zeer schaalbare opslagplatform die hoge doorvoer kan worden verwerkt.  Als de taak voornamelijk uit lees- of schrijfbewerkingen bestaat, kan klikt u vervolgens gelijktijdigheid voor i/o en naar Data Lake Storage Gen2 vergroot prestaties.

Er zijn een paar algemene manieren om te vergroten van de waarde voor concurrency voor i/o-intensieve taken.

**Stap 1: Bepalen hoeveel apps worden uitgevoerd op uw cluster** : U moet weten hoeveel apps worden uitgevoerd op het cluster met inbegrip van het huidige abonnement.  De standaardwaarden voor elke Spark instelling wordt ervan uitgegaan dat er 4 apps die gelijktijdig worden uitgevoerd.  Daarom hebt u alleen 25% van het cluster beschikbaar is voor elke app.  Voor betere prestaties, kunt u de standaardinstellingen onderdrukken door het aantal Executor te wijzigen.  

**Stap 2: Instellen van executor-geheugen** : het eerste wat om in te stellen, is het executor-geheugen.  Het geheugen zijn afhankelijk van de taak die u wilt uitvoeren.  U kunt de gelijktijdigheid verhogen door toe te wijzen minder geheugen per executor.  Als u buiten het geheugen uitzonderingen ziet wanneer u uw taak uitvoert, moet u de waarde voor deze parameter verhogen.  Een alternatief is om op te halen van meer geheugen met behulp van een cluster met grotere hoeveelheden geheugen of het vergroten van uw cluster.  Meer geheugen kunnen meer Executor moet worden gebruikt, wat betekent dat meer gelijktijdigheid.

**Stap 3: Instellen van executor-cores** – voor i/o-intensieve workloads waarvoor geen complexe bewerkingen, is het handig om te beginnen met een groot aantal executor-cores om het aantal parallelle taken per executor te vergroten.  Instellen van executor-cores 4 is een goede start.   

    executor-cores = 4
Het aantal executor-cores verhoogt, krijgt u meer parallelle uitvoering, zodat u met verschillende executor-cores experimenteren kunt.  Voor taken die u meer gecompliceerde bewerkingen hebt, moet u het aantal kernen per executor verminderen.  Als executor-cores is kan groter zijn dan 4, klikt u vervolgens garbagecollection inefficiënt en de prestaties nadelig beïnvloeden.

**Stap 4: YARN-geheugen van de cluster bepalen** : deze informatie is beschikbaar in Ambari.  Navigeer naar YARN en weergeven van het tabblad configuraties.  De YARN-geheugen wordt weergegeven in dit venster.  
Houd er rekening mee wanneer u zich in het venster, ziet u ook de standaardgrootte van de YARN-container.  De grootte van de YARN-container is hetzelfde als geheugen per executor-parameter.

    Total YARN memory = nodes * YARN memory per node
**Stap 5: Num Executor berekenen**

**Geheugen-beperking berekenen** -de parameter num Executor wordt beperkt door het geheugen of CPU.  De beperking van het geheugen wordt bepaald door de hoeveelheid beschikbaar geheugen van de YARN voor uw toepassing.  U moet nemen totale YARN-geheugen en die wordt gedeeld door executor-geheugen.  De beperking moet ongedaan maken voor het aantal apps geschaald zodat we door het aantal apps delen.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
**CPU-beperking berekenen** -beperking van de CPU wordt berekend als het totale aantal virtuele kernen, gedeeld door het aantal kernen per executor.  Er zijn 2 virtuele kernen voor elke fysieke kern.  Net als bij de geheugen-beperking, hebben we te delen door het aantal apps.

    virtual cores = (nodes in cluster * # of physical cores in node * 2)
    CPU constraint = (total virtual cores / # of cores per executor) / # of apps
**Stel num Executor** – de num Executor-parameter wordt bepaald door het minimum van de beperking van het geheugen en de CPU-beperking. 

    num-executors = Min (total virtual Cores / # of cores per executor, available YARN memory / executor-memory)   
Hoe hoger de waarde van num Executor verhoogt niet per se de prestaties.  U moet rekening houden met het toevoegen van meer Executor wordt toegevoegd extra overhead voor elke extra executor, die mogelijk de prestaties kan verslechteren.  NUM Executor wordt begrensd door de clusterresources.    

## <a name="example-calculation"></a>Voorbeeld van de berekening

Stel dat u hebt momenteel een cluster bestaat uit 8 D4v2 knooppunten met 2 apps, inclusief de functie die u wilt uitvoeren.  

**Stap 1: Bepalen hoeveel apps worden uitgevoerd op uw cluster** : u weet dat u 2 hebt apps op uw cluster, inclusief de functie die u wilt uitvoeren.  

**Stap 2: Instellen van executor-geheugen** : in dit voorbeeld we bepalen 6 GB executor-geheugen is voldoende voor i/o-intensieve taak.  

    executor-memory = 6GB
**Stap 3: Instellen van executor-cores** – omdat dit een i/o-intensieve taak, kunnen we het aantal kernen instellen voor elke executor tot en met 4.  Kernen per executor wordt ingesteld op groter is dan 4 garbagecollection verzameling problemen kunnen veroorzaken.  

    executor-cores = 4
**Stap 4: YARN-geheugen van de cluster bepalen** – navigeren We naar Ambari om erachter te komen dat elke D4v2 25 GB van de YARN-geheugen heeft.  Aangezien er 8 knooppunten, wordt het beschikbare geheugen van de YARN vermenigvuldigd met 8.

    Total YARN memory = nodes * YARN memory* per node
    Total YARN memory = 8 nodes * 25GB = 200GB
**Stap 5: Num Executor berekenen** : de parameter num Executor wordt bepaald door het minimum van de beperking van het geheugen en de CPU-beperking gedeeld door het aantal apps die worden uitgevoerd op Spark.    

**Geheugen-beperking berekenen** : de beperking van het geheugen wordt berekend als het totale YARN-geheugen van de hoeveelheid geheugen per executor verdeeld.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
    Memory constraint = (200GB / 6GB) / 2   
    Memory constraint = 16 (rounded)
**CPU-beperking berekenen** -beperking van de CPU wordt berekend als de totale yarn-cores gedeeld door het aantal kernen per executor.
    
    YARN cores = nodes in cluster * # of cores per node * 2   
    YARN cores = 8 nodes * 8 cores per D14 * 2 = 128
    CPU constraint = (total YARN cores / # of cores per executor) / # of apps
    CPU constraint = (128 / 4) / 2
    CPU constraint = 16
**Set num-Executor**

    num-executors = Min (memory constraint, CPU constraint)
    num-executors = Min (16, 16)
    num-executors = 16    


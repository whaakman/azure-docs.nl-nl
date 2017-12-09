---
title: Azure Data Lake Store Spark prestaties afstemmen richtlijnen | Microsoft Docs
description: Azure Data Lake Store Spark prestaties afstemmen richtlijnen
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 3c8ef6fd200f67ebc216c967bb1a6250ddcc15d4
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="performance-tuning-guidance-for-spark-on-hdinsight-and-azure-data-lake-store"></a>Prestaties afstemmen richtlijnen voor Spark in HDInsight en Azure Data Lake Store

Wanneer op de prestaties van Spark afstemmen, moet u rekening houden met het nummer van apps die wordt uitgevoerd op uw cluster.  Standaard kunt u 4 uitvoeren apps gelijktijdig op uw HDI-cluster (Opmerking: de standaardinstelling kan worden gewijzigd).  U kunt besluiten te minder apps gebruiken, zodat u kunt de standaardinstellingen negeren en meer van het cluster te voor deze apps gebruiken.  

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Data Lake Store-account**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight-cluster** met toegang tot een Data Lake Store-account. Zie [een HDInsight-cluster maken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.
* **Spark-cluster uitgevoerd op Azure Data Lake Store**.  Zie voor meer informatie [gebruik HDInsight Spark-cluster voor het analyseren van gegevens in Data Lake Store](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-use-with-data-lake-store)
* **Prestaties afstemmen richtlijnen op ADLS**.  Raadpleeg voor algemene prestaties concepten, [Data Lake Store prestaties afstemmen richtlijnen](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance) 

## <a name="parameters"></a>Parameters

Wanneer Spark van taken wordt uitgevoerd, moet u hier de belangrijkste instellingen die kunnen worden afgestemd om sneller worden ADLS zijn:

* **NUM Executor** -het aantal gelijktijdige taken die kunnen worden uitgevoerd.

* **Executor geheugen** -de hoeveelheid geheugen die aan elke executor toegewezen.

* **Executor kernen** -het aantal kernen dat is toegewezen aan elke executor.                     

**NUM Executor** Num Executor stelt u het maximum aantal taken dat parallel kan worden uitgevoerd.  Het werkelijke aantal taken dat parallel kan worden uitgevoerd, wordt begrensd door het geheugen en CPU-resources in uw cluster.

**Executor geheugen** dit is de hoeveelheid geheugen die aan elke executor wordt wordt toegewezen.  De hoeveelheid geheugen die nodig zijn voor elke executor is afhankelijk van de taak.  Voor complexe bewerkingen moet het geheugen hoger zijn.  Voor eenvoudige bewerkingen zoals lezen en schrijven geheugenvereisten niet lager zijn.  De hoeveelheid geheugen voor elke executor kan worden weergegeven in de Ambari.  In de Ambari, gaat u naar Spark en Raadpleeg het tabblad Configs.  

**Executor kernen** Hiermee stelt u het aantal kernen per executor die bepaalt het aantal parallelle threads dat kan worden uitgevoerd per executor gebruikt.  Bijvoorbeeld, als executor kernen = 2, en vervolgens elk executor 2 parallelle taken in de executor kan worden uitgevoerd.  Het executor-kernen nodig zijn afhankelijk van de taak.  I/o-intensief taken hoeven niet een grote hoeveelheid geheugen per taak, zodat elke executor meer parallelle taken kan verwerken.

Standaard worden twee virtuele YARN cores gedefinieerd voor elke fysieke kernen bij het uitvoeren van Spark in HDInsight.  Dit nummer biedt een goede balans van concurrecy en hoeveelheid context overschakelen vanuit meerdere threads.  

## <a name="guidance"></a>Richtlijnen

Tijdens het uitvoeren van Spark analytische workloads werken met gegevens in Data Lake Store, is het raadzaam dat u de meest recente versie van de HDInsight gebruiken om op te halen van de beste prestaties met Data Lake Store. Wanneer de taak meer i/o-intensieve is, kunnen bepaalde parameters worden geconfigureerd om prestaties te verbeteren.  Azure Data Lake Store is een uiterst schaalbare opslag platform dat maximale doorvoer kan verwerken.  Als de taak hoofdzakelijk uit lees- of schrijfbewerkingen bestaat, kan vervolgens gelijktijdigheid van taken voor i/o en naar Azure Data Lake Store vergroot, prestaties.

Er zijn enkele algemene manieren om te verhogen gelijktijdigheid van taken voor i/o-intensieve taken.

**Stap 1: Bepalen hoeveel apps worden uitgevoerd op uw cluster** – u moet weten hoe veel apps worden uitgevoerd op het cluster met inbegrip van het huidige proces.  De standaardwaarden voor elke Spark instelling wordt ervan uitgegaan dat er 4 apps tegelijkertijd worden uitgevoerd.  Daarom hebt u alleen 25% van het cluster die beschikbaar zijn voor elke app.  Als u betere prestaties, kunt u de standaardwaarden overschrijven door het aantal Executor te wijzigen.  

**Stap 2: Stel executor geheugen** – het eerste wat in te stellen, is het executor-geheugen.  Het geheugen zijn afhankelijk van de taak die u wilt uitvoeren.  U kunt gelijktijdigheid vergroten door minder geheugen per executor toewijzen.  Als u buiten geheugen uitzonderingen ziet wanneer u uw taak uitvoert, moet u de waarde voor deze parameter verhogen.  Een alternatief is om op te halen van meer geheugen met behulp van een cluster met hogere hoeveelheden geheugen of het vergroten van het cluster.  Meer geheugen toe te voegen meer Executor moet worden gebruikt, wat betekent dat meer gelijktijdigheid van taken.

**Stap 3: Stel executor kernen** – voor i/o-intensieve werkbelastingen die geen complexe bewerkingen, is het raadzaam om te beginnen met een groot aantal executor-kernen om het aantal parallelle taken per executor te vergroten.  Executor kernen instelt op 4, is een goede start.   

    executor-cores = 4
Het aantal executor kernen krijgt u meer parallelle uitvoering zodat u met verschillende executor-kernen experimenteren kunt.  Voor taken die u meer complexe bewerkingen hebt, moet u het aantal kernen per executor verminderen.  Als executor kernen kan hoger zijn dan 4, vervolgens garbagecollection inefficiënt en de prestaties nadelig beïnvloeden.

**Stap 4: YARN-geheugen van de cluster bepalen** – deze informatie is beschikbaar in Ambari.  Navigeer naar YARN en Raadpleeg het tabblad Configs.  Het geheugen YARN wordt in dit venster weergegeven.  
Opmerking: wanneer u zich in het venster, u ziet ook de standaardgrootte voor de YARN-container.  De grootte van de container YARN is hetzelfde als geheugen per executor-parameter.

    Total YARN memory = nodes * YARN memory per node
**Stap 5: Num Executor berekenen**

**Geheugen-beperking berekenen** -de parameter num Executor wordt begrensd door geheugen of CPU.  De beperking van het geheugen wordt bepaald door de hoeveelheid beschikbaar geheugen voor YARN voor uw toepassing.  U moet nemen totale YARN-geheugen en die wordt gedeeld door executor-geheugen.  De beperking moet worden opgeheven uitgebreid voor het aantal apps zodat we door het aantal apps delen.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
**CPU-beperking berekenen** -de CPU-beperking wordt berekend als de totale virtuele kernen gedeeld door het aantal kernen per executor.  Er zijn 2 virtuele kernen voor elke fysieke kernen.  Vergelijkbaar met de beperking van het geheugen, hebben we delen door het aantal apps.

    virtual cores = (nodes in cluster * # of physical cores in node * 2)
    CPU constraint = (total virtual cores / # of cores per executor) / # of apps
**Num Executor ingesteld** : de parameter num Executor wordt bepaald door het minimum van de beperkingen van het geheugen en de CPU. 

    num-executors = Min (total virtual Cores / # of cores per executor, available YARN memory / executor-memory)   
Hoe hoger de waarde van getal Executor stijgt niet per se prestaties.  U moet rekening houden met het toevoegen van meer Executor wordt toegevoegd extra overhead voor elke extra executor die u kunt mogelijk de prestaties verslechteren.  NUM Executor wordt begrensd door de clusterresources.    

## <a name="example-calculation"></a>Voorbeeld van de berekening

Stel dat u hebt momenteel een cluster bestaan uit 8 D4v2 knooppunten met 2 apps, inclusief de waarschuwing die u wilt uitvoeren.  

**Stap 1: Bepalen hoeveel apps worden uitgevoerd op uw cluster** – u weet dat u 2 hebt apps op uw cluster, inclusief de waarschuwing die u wilt uitvoeren.  

**Stap 2: Stel executor geheugen** – voor dit voorbeeld we bepalen 6 GB executor-geheugen is geschikt voor i/o-intensieve taak.  

    executor-memory = 6GB
**Stap 3: Stel executor kernen** – omdat dit een i/o-intensieve taak is, kunnen we het aantal kernen ingesteld voor elke executor tot en met 4.  Kernen per executor wordt ingesteld op groter is dan 4 garbagecollection verzameling problemen kan veroorzaken.  

    executor-cores = 4
**Stap 4: YARN-geheugen van de cluster bepalen** – We gaat u naar Ambari om erachter te komen dat elke D4v2 25 GB YARN geheugen heeft.  Aangezien er 8 knooppunten, kan het beschikbare geheugen van YARN 8 wordt vermenigvuldigd.

    Total YARN memory = nodes * YARN memory* per node
    Total YARN memory = 8 nodes * 25GB = 200GB
**Stap 5: Berekenen num Executor** : de parameter num Executor wordt bepaald door het minimum van de beperking van het geheugen en de CPU-beperking wordt gedeeld door het aantal apps die worden uitgevoerd op Spark.    

**Geheugen-beperking berekenen** : de beperking van het geheugen wordt berekend als het totale YARN-geheugen van de hoeveelheid geheugen per executor verdeeld.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
    Memory constraint = (200GB / 6GB) / 2   
    Memory constraint = 16 (rounded)
**CPU-beperking berekenen** -de CPU-beperking wordt berekend als de totale yarn kernen gedeeld door het aantal kernen per executor.
    
    YARN cores = nodes in cluster * # of cores per node * 2   
    YARN cores = 8 nodes * 8 cores per D14 * 2 = 128
    CPU constraint = (total YARN cores / # of cores per executor) / # of apps
    CPU constraint = (128 / 4) / 2
    CPU constraint = 16
**Set num-Executor**

    num-executors = Min (memory constraint, CPU constraint)
    num-executors = Min (16, 16)
    num-executors = 16    


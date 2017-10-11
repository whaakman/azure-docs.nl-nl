---
title: Azure Data Lake Store Hive prestaties afstemmen richtlijnen | Microsoft Docs
description: Azure Data Lake Store Hive prestaties afstemmen richtlijnen
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
ms.openlocfilehash: e10bf8f7cbae2b81d22823ff74fe652c6bcb2da3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a>Prestaties afstemmen richtlijnen voor Hive in HDInsight en Azure Data Lake Store

De standaardinstellingen zijn ingesteld om te voorzien in veel verschillende gebruiksscenario goede prestaties.  Voor i/o-intensieve query's kan Hive voor betere prestaties met ADLS worden afgestemd.  

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Data Lake Store-account**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight-cluster** met toegang tot een Data Lake Store-account. Zie [een HDInsight-cluster maken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.
* **Uitvoeren van Hive in HDInsight**.  Zie voor meer informatie over het uitvoeren van Hive-taken in HDInsight, [op HDInsight Hive gebruiken] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)
* **Prestaties afstemmen richtlijnen op ADLS**.  Raadpleeg voor algemene prestaties concepten, [Data Lake Store prestaties afstemmen richtlijnen](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parameters

Hier volgen de belangrijkste instellingen af te stemmen voor verbeterde ADLS-prestaties:

* **hive.tez.container.Size** – de hoeveelheid geheugen die wordt gebruikt door elke taken

* **grootte van tez.Grouping.min** : minimale grootte van elke toewijzen

* **grootte van tez.Grouping.Max** : maximale grootte van elke toewijzen

* **hive.Exec.reducer.bytes.per.reducer** : grootte van elke reducer

**hive.tez.container.Size** -de grootte van de container bepaalt hoeveel geheugen beschikbaar is voor elke taak.  Dit is de belangrijkste invoer voor het beheren van de concurrency in Hive.  

**grootte van tez.Grouping.min** – deze parameter kunt u de minimale grootte van elke mapper instellen.  Als het aantal mappers die Tez kiest kleiner dan de waarde van deze parameter is, wordt de hier ingestelde waarde gebruikt op Tez.  

**grootte van tez.Grouping.Max** : de parameter kunt u de maximale grootte van elke mapper instellen.  Als het aantal mappers die Tez kiest groter dan de waarde van deze parameter is, wordt de hier ingestelde waarde gebruikt op Tez.  

**hive.Exec.reducer.bytes.per.reducer** – deze parameter wordt de grootte van elke reducer ingesteld.  Standaard is elke reducer 256MB.  

## <a name="guidance"></a>Richtlijnen

**Stel hive.exec.reducer.bytes.per.reducer** : de standaardwaarde goed werkt wanneer de gegevens is gedecomprimeerd.  Voor gegevens die zijn gecomprimeerd, moet u de grootte van de reducer beperken.  

**Stel hive.tez.container.size** – In elk knooppunt geheugen wordt opgegeven door yarn.nodemanager.resource.memory mb en moet worden correct is ingesteld op HDI-cluster standaard.  Zie voor meer informatie over het instellen van het juiste geheugen in YARN [boeken](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

I/o-intensieve werkbelastingen kunnen profiteren van meer parallelle uitvoering door de grootte van de container Tez verlagen. Hiermee geeft u de gebruiker meer containers waardoor gelijktijdigheid van taken.  Sommige Hive-query's vereisen echter een aanzienlijke hoeveelheid geheugen (bijvoorbeeld MapJoin).  Als de taak beschikt niet over voldoende geheugen, krijgt u een out-of geheugen-uitzondering tijdens runtime.  Als u buiten geheugen uitzonderingen ontvangt, moet u het geheugen verhogen.   

Het aantal gelijktijdige taken die worden uitgevoerd of parallelle uitvoering wordt begrensd worden door het totale YARN-geheugen.  Het aantal YARN-containers worden uitgevoerd, bepalen hoeveel gelijktijdige taken kunnen uitvoeren.  Als u de YARN-geheugen per knooppunt zoekt, kunt u naar Ambari gaan.  Navigeer naar YARN en Raadpleeg het tabblad Configs.  Het geheugen YARN wordt in dit venster weergegeven.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
De sleutel voor het verbeteren van de prestaties met ADLS is de concurrency zo veel mogelijk verhogen.  Tez berekent automatisch het aantal taken die moet worden gemaakt, dus u niet hoeft in te stellen.   

## <a name="example-calculation"></a>Voorbeeld van de berekening

Stel dat u hebt een cluster met 8 knooppunten D14.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Beperkingen
**ADLS-beperking** 

Wanneer u de grenzen van de bandbreedte die worden geleverd door ADLS UIf raakt, u zou gaan om te zien van de mislukte taken. Dit kan worden geïdentificeerd door observeren bandbreedteregeling fouten in de logboeken van de taak.  U kunt de parallelle uitvoering verlagen door de Tez-container vergroten.  Als u meer gelijktijdigheid van taken bij uw werk nodig, neem dan contact met ons.   

Om te zien als u zijn ophalen beperkt, moet u de opdracht debug logboekregistratie aan de clientzijde inschakelen. Hier ziet u hoe u dat kunt doen:

1. De volgende eigenschap plaatsen in de eigenschappen log4j in de Hive-configuratie. Dit kan worden gedaan vanuit de weergave Ambari: log4j.logger.com.microsoft.azure.datalake.store=DEBUG alle knooppunten/start de service opnieuw voor de configuratie door te voeren.

2. Als u beperkt ophalen, ziet u de 429 HTTP-foutcode in de hive-logboekbestand. Het hive-logboekbestand bevindt zich in /tmp/&lt;gebruiker&gt;/hive.log

## <a name="further-information-on-hive-tuning"></a>Meer informatie over het afstemmen van Hive

Hier volgen enkele blogs waarmee uw Hive-query's optimaliseren:
* [Hive-query's optimaliseren voor Hadoop in HDInsight](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Het oplossen van de prestaties van de Hive-query 's](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [Ignite Neem contact op optimaliseren Hive in HDInsight](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)

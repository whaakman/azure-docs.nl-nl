---
title: Azure Data Lake Storage Gen2 Hive-prestaties afstemmen richtlijnen | Microsoft Docs
description: Azure Data Lake Storage Gen2 Hive-prestaties richtlijnen over het afstemmen
services: storage
author: swums
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: stewu
ms.openlocfilehash: 9e5570b937fe97cc9b6ccd9ac804a35ff8e07d6f
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883063"
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-storage-gen2"></a>Richtlijnen voor Hive in HDInsight en Azure Data Lake Storage Gen2 afstemmen van prestaties

De standaardinstellingen zijn voor goede prestaties voor veel verschillende use cases ingesteld.  Voor i/o-intensieve query's, kunt Hive aanpassen voor betere prestaties met Azure Data Lake Storage Gen2.  

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **De account van een Data Lake Storage Gen2**. Zie voor instructies over het maken van een [Quick Start: Een Azure Data Lake Storage Gen2 storage-account maken](data-lake-storage-quickstart-create-account.md)
* **Azure HDInsight-cluster** met toegang tot een Data Lake Storage Gen2-account. Zie [gebruik Azure Data Lake Storage Gen2 met Azure HDInsight-clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)
* **Uitvoeren van Hive in HDInsight**.  Zie voor meer informatie over het uitvoeren van Hive-taken in HDInsight, [Hive in HDInsight gebruiken](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive)
* **Richtlijnen voor het Data Lake Storage Gen2 afstemmen van prestaties**.  Zie voor de prestaties van de algemene concepten, [Data Lake Storage Gen2 prestaties afstemmen-richtlijnen](data-lake-storage-performance-tuning-guidance.md)

## <a name="parameters"></a>Parameters

Hier volgen de belangrijkste instellingen om af te stemmen voor betere prestaties van de Data Lake Storage Gen2:

* **hive.tez.container.Size** – de hoeveelheid geheugen die wordt gebruikt door alle taken

* **grootte van tez.Grouping.min** : minimale grootte van elke toewijzen

* **tez.Grouping.Max grootte** : de maximale grootte van elke toewijzen

* **hive.Exec.reducer.bytes.per.reducer** : grootte van elke reducer

**hive.tez.container.Size** -de containergrootte van de bepaalt hoeveel geheugen beschikbaar is voor elke taak.  Dit is de belangrijkste invoer voor het beheren van de waarde voor concurrency in Hive.  

**grootte van tez.Grouping.min** – met deze parameter kunt u de minimale grootte van elke mapper instellen.  Als het aantal mappers die Tez kiest kleiner dan de waarde van deze parameter is, vervolgens Tez gebruiken de hier ingestelde waarde.

**grootte van tez.Grouping.Max** : de parameter kunt u de maximale grootte van elke mapper instellen.  Als het aantal mappers die Tez kiest groter is dan de waarde van deze parameter, vervolgens Tez gebruiken de hier ingestelde waarde.

**hive.Exec.reducer.bytes.per.reducer** – met deze parameter wordt de grootte van elke reducer ingesteld.  Standaard is elke reducer 256MB.  

## <a name="guidance"></a>Richtlijnen

**Stel hive.exec.reducer.bytes.per.reducer** : de standaardwaarde goed werkt wanneer de gegevens is gedecomprimeerd.  Voor gegevens die is gecomprimeerd, moet u de grootte van de reducer verminderen.  

**Stel hive.tez.container.size** – In elk knooppunt geheugen is opgegeven door yarn.nodemanager.resource.memory mb en moet zijn correct ingesteld op HDI-cluster standaard.  Zie voor meer informatie over het instellen van de hoeveelheid geheugen in YARN, [boeken](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

I/o-intensieve werkbelastingen kunnen profiteren van meer parallelle uitvoering door de grootte van de container Tez. Dit geeft de gebruiker meer containers waardoor gelijktijdigheid.  Sommige Hive-query's vereisen echter een aanzienlijke hoeveelheid geheugen (bijvoorbeeld MapJoin).  Als de taak niet voldoende geheugen heeft, krijgt u een out-of geheugen-uitzondering tijdens runtime.  Als u buiten het geheugen uitzonderingen ontvangt, moet u het geheugen vergroten.   

Het aantal gelijktijdige taken die worden uitgevoerd of parallelle uitvoering wordt door het totale geheugen van YARN worden gebonden.  Het nummer van de YARN-containers worden bepaald door het aantal gelijktijdige taken kunnen uitvoeren.  Als u de YARN-geheugen per knooppunt zoekt, kunt u naar Ambari gaan.  Navigeer naar YARN en weergeven van het tabblad configuraties.  De YARN-geheugen wordt weergegeven in dit venster.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
De sleutel voor het verbeteren van de prestaties met behulp van Data Lake Storage Gen2 is de gelijktijdigheid zo veel mogelijk verhogen.  Tez berekent automatisch het aantal taken die moeten worden gemaakt, dus u niet hoeft in te stellen.   

## <a name="example-calculation"></a>Voorbeeld van de berekening

Stel dat u hebt een 8-knooppunt D14-cluster.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="further-information-on-hive-tuning"></a>Meer informatie over het afstemmen van Hive

Hier volgen enkele blogs waarmee uw Hive-query's af te stemmen:
* [Hive-query's optimaliseren voor Hadoop in HDInsight](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Het oplossen van Hive-queryprestaties](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [Ignite-presentatie op optimaliseren Hive in HDInsight](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)

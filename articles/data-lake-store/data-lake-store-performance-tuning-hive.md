---
title: Azure Data Lake Storage Gen1 Hive-prestaties afstemmen richtlijnen | Microsoft Docs
description: Azure Data Lake Storage Gen1 Hive-prestaties richtlijnen over het afstemmen
services: data-lake-store
documentationcenter: ''
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: e9d0ad0398dfc238d48060247cdb6f29b0f34a60
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123331"
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-storage-gen1"></a>Richtlijnen voor Hive in HDInsight en Azure Data Lake Storage Gen1 afstemmen van prestaties

De standaardinstellingen zijn voor goede prestaties voor veel verschillende use cases ingesteld.  Voor i/o-intensieve query's, kunt Hive aanpassen voor betere prestaties met Azure Data Lake Storage Gen1.  

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **De account van een Data Lake Storage Gen1**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)
* **Azure HDInsight-cluster** met toegang tot een Gen1 van Data Lake Storage-account. Zie [een HDInsight-cluster maken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.
* **Uitvoeren van Hive in HDInsight**.  Zie voor meer informatie over het uitvoeren van Hive-taken in HDInsight, () [op HDInsight Hive gebruiken]https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive)
* **Richtlijnen voor het Data Lake Storage Gen1 afstemmen van prestaties**.  Zie voor de prestaties van de algemene concepten, [Data Lake Storage Gen1 prestaties afstemmen-richtlijnen](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parameters

Hier volgen de belangrijkste instellingen om af te stemmen voor betere prestaties van de Data Lake Storage Gen1:

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
De sleutel voor het verbeteren van de prestaties met behulp van Data Lake Storage Gen1 is de gelijktijdigheid zo veel mogelijk verhogen.  Tez berekent automatisch het aantal taken die moeten worden gemaakt, dus u niet hoeft in te stellen.   

## <a name="example-calculation"></a>Voorbeeld van de berekening

Stel dat u hebt een 8-knooppunt D14-cluster.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Beperkingen

**Data Lake Storage Gen1 beperking** 

Als u de grenzen van de bandbreedte die door Data Lake Storage Gen1 bereikt, kunt u zou gaan om te zien van de mislukte taken. Dit kan worden geïdentificeerd met geobserveerd beperkingsfouten in Logboeken van de taak.  U kunt de parallelle uitvoering verlagen door de Tez-container vergroten.  Als u meer gelijktijdigheid nodig voor uw taak, neem dan contact met ons.

Om te controleren als u ophalen beperkt, moet u de logboekregistratie op de client voor foutopsporing inschakelen. Hier ziet hoe u dat kunt doen:

1. Plaats de volgende eigenschap in de log4j-eigenschappen in de Hive-configuratie. Dit kan worden gedaan vanuit Ambari weergave: log4j.logger.com.microsoft.azure.datalake.store=DEBUG alle knooppunten/start de service opnieuw voor de configuratie van kracht te laten worden.

2. Als u beperkt ophalen, ziet u de code van de fout HTTP 429 in het logboekbestand hive. Het hive-logboekbestand bevindt zich in /tmp/&lt;gebruiker&gt;/hive.log

## <a name="further-information-on-hive-tuning"></a>Meer informatie over het afstemmen van Hive

Hier volgen enkele blogs waarmee uw Hive-query's af te stemmen:
* [Hive-query's optimaliseren voor Hadoop in HDInsight](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Het oplossen van Hive-queryprestaties](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [Ignite-presentatie op optimaliseren Hive in HDInsight](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)

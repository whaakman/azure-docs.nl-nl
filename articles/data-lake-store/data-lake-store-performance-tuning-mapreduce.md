---
title: Azure Data Lake Storage Gen1 MapReduce prestaties richtlijnen over het afstemmen | Microsoft Docs
description: Azure Data Lake Storage Gen1 MapReduce prestaties richtlijnen over het afstemmen
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
ms.openlocfilehash: b661499786057a3083f79684dfd12c85266b7b5c
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128788"
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-storage-gen1"></a>Richtlijnen voor MapReduce in HDInsight en Azure Data Lake Storage Gen1 afstemmen van prestaties

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een account met Azure Data Lake Storage Gen1**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)
* **Azure HDInsight-cluster** met toegang tot een Gen1 van Data Lake Storage-account. Zie [een HDInsight-cluster maken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.
* **Met behulp van MapReduce in HDInsight**.  Zie voor meer informatie, [MapReduce in Hadoop op HDInsight gebruiken](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-mapreduce)
* **Richtlijnen voor het Data Lake Storage Gen1 afstemmen van prestaties**.  Zie voor de prestaties van de algemene concepten, [Data Lake Storage Gen1 prestaties afstemmen-richtlijnen](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parameters

Bij het uitvoeren van MapReduce-taken, vindt hier u de belangrijkste parameters die u configureren kunt voor betere prestaties op Data Lake Storage Gen1:

* **Mapreduce.map.Memory.MB** – de hoeveelheid geheugen worden toegewezen aan elke toewijzen
* **Mapreduce.job.Maps** : het aantal taken per taak kaart
* **Mapreduce.reduce.Memory.MB** – de hoeveelheid geheugen aan elke reducer toewijzen
* **Mapreduce.job.reduces** : het aantal taken per taak verminderen

**Mapreduce.map.Memory / Mapreduce.reduce.memory** dit nummer moet worden aangepast op basis van de hoeveelheid geheugen nodig is voor de kaart en/of beperken van de taak.  De standaardwaarden van mapreduce.map.memory en mapreduce.reduce.memory kunnen worden weergegeven in de Ambari via de Yarn-configuratie.  Navigeer naar YARN in Ambari, en naar het tabblad configuraties.  De YARN-geheugen wordt weergegeven.  

**Mapreduce.job.Maps / Mapreduce.job.reduces** dit bepaalt het maximum aantal mappers of reducers tegelijkertijd kunnen worden gemaakt.  Het aantal splitsingen, bepaalt hoeveel mappers voor de MapReduce-taak wordt gemaakt.  Daarom, krijgt u mogelijk minder mappers dan u hebt aangevraagd of er minder splitsingen dan het aantal mappers aangevraagd zijn.       

## <a name="guidance"></a>Richtlijnen

**Stap 1: Het aantal actieve taken bepalen** -standaard MapReduce het hele cluster wordt gebruikt voor de taak.  U kunt minder van het cluster met behulp van minder mappers dan er beschikbaar containers.  De instructies in dit document wordt ervan uitgegaan dat uw toepassing is de enige toepassing die wordt uitgevoerd op uw cluster.      

**Stap 2: Stel mapreduce.map.memory/mapreduce.reduce.memory** : de grootte van het geheugen voor de kaart en verminder taken zijn afhankelijk van uw specifieke taak.  U kunt de geheugengrootte verminderen als u wilt verhogen gelijktijdigheid.  Het aantal gelijktijdig uitvoeren van taken, is afhankelijk van het aantal containers.  De hoeveelheid geheugen per toewijzen of reducer verlaagt, kunnen meer containers worden gemaakt, waarmee meer mappers en reducers tegelijkertijd kunnen gelijktijdig worden uitgevoerd.  De hoeveelheid geheugen te veel verlagen, kan dit ertoe leiden dat bepaalde processen te weinig geheugen.  Als u een heap-fout optreedt bij het uitvoeren van uw taak, moet u de hoeveelheid geheugen per toewijzen of reducer vergroten.  U moet rekening houden met toevoegen van meer containers wordt toegevoegd extra overhead voor elke extra container, zodat mogelijk de prestaties kan verslechteren.  Een ander alternatief is om op te halen van meer geheugen met behulp van een cluster met grotere hoeveelheden geheugen of het aantal knooppunten in uw cluster verhoogt.  Meer geheugen kunnen meer containers kunnen worden gebruikt, wat betekent dat meer gelijktijdigheid.  

**Stap 3: Bepalen totale YARN geheugen** - om af te stemmen mapreduce.job.maps/mapreduce.job.reduces, moet u rekening houden met de hoeveelheid totale YARN-geheugen beschikbaar voor gebruik.  Deze informatie is beschikbaar in Ambari.  Navigeer naar YARN en weergeven van het tabblad configuraties.  De YARN-geheugen wordt weergegeven in dit venster.  U moet de YARN-geheugen met het aantal knooppunten in uw cluster om op te halen van het totale geheugen van de YARN vermenigvuldigen.

    Total YARN memory = nodes * YARN memory per node
Als u een lege cluster gebruikt, kan het totale YARN-geheugen voor uw cluster zijn op geheugen.  Als u andere toepassingen gebruikt geheugen, kunt klikt u vervolgens u alleen een gedeelte van het geheugen van uw cluster worden gebruikt door het aantal mappers of reducers tegelijkertijd beperken tot het aantal containers dat u wilt gebruiken.  

**Stap 4: Aantal YARN-containers berekenen** – YARN-containers dicteren de hoeveelheid gelijktijdigheid beschikbaar voor de taak.  Totale hoeveelheid geheugen voor YARN nemen en die wordt gedeeld door mapreduce.map.memory.  

    # of YARN containers = total YARN memory / mapreduce.map.memory

**Stap 5: Stel mapreduce.job.maps/mapreduce.job.reduces** mapreduce.job.maps/mapreduce.job.reduces ingesteld op ten minste het aantal beschikbare containers.  U kunt experimenteren verder door het aantal mappers en reducers tegelijkertijd om te zien of u betere prestaties krijgt.  Houd er rekening mee dat meer mappers extra overhead hebben om dat te veel mappers hoeft de prestaties nadelig beïnvloeden.  

CPU-plannings- en CPU-isolatie zijn standaard uitgeschakeld zodat het nummer van de YARN-containers wordt beperkt door het geheugen.

## <a name="example-calculation"></a>Voorbeeld van de berekening

Stel dat u momenteel een cluster bestaat uit 8 D14 knooppunten hebt en u wilt een i/o-intensieve taak uit te voeren.  Hier volgen de berekeningen die u moet doen:

**Stap 1: Het aantal actieve taken bepalen** -in ons voorbeeld gaan we ervan uit dat de taak de slechts één die wordt uitgevoerd is.  

**Stap 2: Stel mapreduce.map.memory/mapreduce.reduce.memory** – in ons voorbeeld u een i/o-intensieve taak worden uitgevoerd en besluit 3 GB aan geheugen voor de taken van de kaart is voldoende.

    mapreduce.map.memory = 3GB
**Stap 3: Bepalen totale YARN-geheugen**

    total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**Stap 4: Berekenen # van YARN-containers**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

**Stap 5: Mapreduce.job.maps/mapreduce.job.reduces instellen**

    mapreduce.map.jobs = 256

## <a name="limitations"></a>Beperkingen

**Data Lake Storage Gen1 beperking**

Data Lake Storage Gen1 ingesteld als een service met meerdere tenants, niveau bandbreedtelimieten.  Als u deze limiet bereikt, begint u taakfouten ziet. Dit kan worden geïdentificeerd door geobserveerd beperkingsfouten in Logboeken van de taak.  Als u meer bandbreedte nodig voor uw taak, neem dan contact met ons.   

Om te controleren als u ophalen beperkt, moet u de logboekregistratie op de client voor foutopsporing inschakelen. Hier ziet hoe u dat kunt doen:

1. De volgende eigenschap plaatsen in de eigenschappen log4j in Ambari > YARN > Configuratie > Geavanceerde yarn-log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG

2. Alle knooppunten/start de service opnieuw voor de configuratie van kracht te laten worden.

3. Als u beperkt ophalen, ziet u de code van de fout HTTP 429 in de YARN-logboekbestand. De YARN-logboekbestand bevindt zich in /tmp/&lt;gebruiker&gt;/yarn.log

## <a name="examples-to-run"></a>Voorbeelden uitvoeren

Om te demonstreren hoe MapReduce wordt uitgevoerd op Data Lake Storage Gen1, volgt hieronder een voorbeeld van code die is uitgevoerd op een cluster met de volgende instellingen:

* 16 knooppunten D14v2
* Hadoop-cluster met HDI 3.6

Hier volgen enkele Voorbeeldopdrachten MapReduce Teragen, Terasort en Teravalidate uit te voeren voor een beginpunt.  U kunt deze opdrachten op basis van uw resources aanpassen.

**Teragen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate

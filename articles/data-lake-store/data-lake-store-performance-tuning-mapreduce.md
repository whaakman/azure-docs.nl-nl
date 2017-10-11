---
title: Azure Data Lake Store MapReduce prestaties afstemmen richtlijnen | Microsoft Docs
description: Azure Data Lake Store MapReduce prestaties afstemmen richtlijnen
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
ms.openlocfilehash: 9528148792f083cb0e48d356e61cf61762ee954f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-store"></a>Prestaties afstemmen richtlijnen voor MapReduce op HDInsight en Azure Data Lake Store


## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Data Lake Store-account**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight-cluster** met toegang tot een Data Lake Store-account. Zie [een HDInsight-cluster maken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.
* **Met behulp van MapReduce op HDInsight**.  Zie voor meer informatie [gebruik MapReduce in Hadoop in HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)  
* **Prestaties afstemmen richtlijnen op ADLS**.  Raadpleeg voor algemene prestaties concepten, [Data Lake Store prestaties afstemmen richtlijnen](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)  

## <a name="parameters"></a>Parameters

Wanneer MapReduce-taken wordt uitgevoerd, moet u hier de belangrijkste parameters die u configureren kunt voor het sneller worden ADLS zijn:

* **Mapreduce.map.Memory.MB** – de hoeveelheid geheugen toe te wijzen aan elke toewijzen
* **Mapreduce.job.Maps** : het aantal taken per taak kaart
* **Mapreduce.reduce.Memory.MB** – de hoeveelheid geheugen aan elke reducer toewijzen
* **Mapreduce.job.reduces** : het aantal taken per taak verminderen

**Mapreduce.map.Memory / Mapreduce.reduce.memory** dit nummer moet worden aangepast op basis van hoeveel geheugen nodig is voor de kaart en/of taak verminderen.  De standaardwaarden van mapreduce.map.memory en mapreduce.reduce.memory kunnen worden weergegeven in de Ambari via de Yarn-configuratie.  In de Ambari, gaat u naar YARN en Raadpleeg het tabblad Configs.  Het geheugen YARN wordt weergegeven.  

**Mapreduce.job.Maps / Mapreduce.job.reduces** dit bepaalt het maximum aantal mappers of verkleiningstoestellen worden gemaakt.  Het aantal splitsingen bepaalt hoeveel mappers voor MapReduce-taak wordt gemaakt.  Daarom kunnen er minder mappers dan u hebt opgegeven of er minder splitsingen dan het aantal mappers aangevraagd zijn.       

## <a name="guidance"></a>Richtlijnen

**Stap 1: Aantal actieve taken bepalen** -standaard MapReduce het hele cluster wordt gebruikt voor uw project.  Met behulp van minder mappers dan er beschikbaar containers kunt u minder aan het cluster.  De instructies in dit document wordt ervan uitgegaan dat uw toepassing is de enige uitgevoerd op uw cluster.      

**Stap 2: Stel mapreduce.map.memory/mapreduce.reduce.memory** – de grootte van het geheugen voor de kaart en de taken zijn afhankelijk van uw specifieke taak.  Als u wilt verhogen gelijktijdigheid van taken, kunt u de geheugengrootte reduceren.  Het aantal gelijktijdig actieve taken is afhankelijk van het aantal containers.  De hoeveelheid geheugen per toewijzen of reducer verlaagt, kunnen meer containers worden gemaakt, waarmee meer mappers of verkleiningstoestellen gelijktijdig worden uitgevoerd.  De hoeveelheid geheugen te veel minder kan ertoe leiden dat bepaalde processen onvoldoende geheugen.  Als u een heap-fout krijgt bij het uitvoeren van uw werk, moet u de hoeveelheid geheugen per toewijzen of reducer verhogen.  U moet rekening houden met toevoegen van meer containers worden toegevoegd extra overhead voor elke extra container die u kunt mogelijk de prestaties verslechteren.  Een ander alternatief is om op te halen van meer geheugen met behulp van een cluster met hogere hoeveelheden geheugen of het aantal knooppunten in het cluster.  Meer geheugen toe te voegen meer containers moet worden gebruikt, wat betekent dat meer gelijktijdigheid van taken.  

**Stap 3: Bepalen totale YARN geheugen** - af te stemmen mapreduce.job.maps/mapreduce.job.reduces, moet u rekening houden met de hoeveelheid totale YARN-geheugen beschikbaar voor gebruik.  Deze informatie is beschikbaar in Ambari.  Navigeer naar YARN en Raadpleeg het tabblad Configs.  Het geheugen YARN wordt in dit venster weergegeven.  U moet de YARN-geheugen met het aantal knooppunten in het cluster om op te halen van de totale hoeveelheid geheugen voor YARN vermenigvuldigen.

    Total YARN memory = nodes * YARN memory per node
Als u een leeg cluster gebruikt, kan het totale geheugen YARN voor uw cluster worden op geheugen.  Als andere toepassingen geheugen gebruikt, kunt klikt u vervolgens u alleen een deel van uw cluster geheugen worden gebruikt door te verminderen dat het aantal mappers of verkleiningstoestellen aan het aantal containers die u wilt gebruiken.  

**Stap 4: Aantal YARN containers berekenen** – YARN containers, bepalen het aantal gelijktijdige threads beschikbaar voor de taak.  Totale hoeveelheid geheugen voor YARN nemen en die wordt gedeeld door mapreduce.map.memory.  

    # of YARN containers = total YARN memory / mapreduce.map.memory

**Stap 5: Stel mapreduce.job.maps/mapreduce.job.reduces** mapreduce.job.maps/mapreduce.job.reduces ingesteld op ten minste het aantal beschikbare containers.  U kunt experimenteren verdere door het aantal mappers en verkleiningstoestellen om te zien als u betere prestaties te verhogen.  Houd er rekening mee dat meer mappers extra overhead hebben dus met te veel mappers de prestaties nadelig beïnvloeden.  

Planning van CPU- en CPU-isolatie zijn standaard uitgeschakeld zodat het aantal YARN containers wordt begrensd door geheugen.

## <a name="example-calculation"></a>Voorbeeld van de berekening

Stel dat u hebt momenteel een cluster die bestaan uit 8 D14 knooppunten en u wilt een i/o-intensieve taak uitvoeren.  Hier volgen de berekeningen die u moet doen:

**Stap 1: Aantal actieve taken bepalen** -in ons voorbeeld gaan we ervan uit dat de taak de slechts één die wordt uitgevoerd is.  

**Stap 2: Stel mapreduce.map.memory/mapreduce.reduce.memory** – in ons voorbeeld u een i/o-intensieve taak uitvoert en besluit 3 GB geheugen voor taken van de kaart is voldoende.

    mapreduce.map.memory = 3GB
**Stap 3: Bepalen totale YARN-geheugen**

    total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**Stap 4: Het aantal containers YARN berekenen**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

**Stap 5: Mapreduce.job.maps/mapreduce.job.reduces instellen**

    mapreduce.map.jobs = 256

## <a name="limitations"></a>Beperkingen

**ADLS-beperking**

Als een multitenant-service stelt ADLS limieten van niveau bandbreedte.  Als u deze limiet bereikt, gaat u taakfouten ziet. Dit kan worden geïdentificeerd door observeren bandbreedteregeling fouten in de logboeken van de taak.  Als u meer bandbreedte nodig hebt voor uw project, neem dan contact met ons.   

Om te zien als u zijn ophalen beperkt, moet u de opdracht debug logboekregistratie aan de clientzijde inschakelen. Hier ziet u hoe u dat kunt doen:

1. De volgende eigenschap plaatsen in de eigenschappen log4j in Ambari > YARN > Config > Geavanceerde yarn-log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG

2. Alle knooppunten/start de service opnieuw voor de configuratie door te voeren.

3. Als u beperkt ophalen, ziet u de 429 HTTP-foutcode in de YARN-logboekbestand. De YARN-logboekbestand bevindt zich in /tmp/&lt;gebruiker&gt;/yarn.log

## <a name="examples-to-run"></a>Voorbeelden worden uitgevoerd

Om te demonstreren hoe MapReduce wordt uitgevoerd op Azure Data Lake Store, vindt hieronder u enkele voorbeeldcode die is uitgevoerd op een cluster met de volgende instellingen:

* 16 knooppunten D14v2
* Hadoop-cluster HDI 3.6 uitgevoerd

Hier volgen enkele Voorbeeldopdrachten MapReduce Teragen Terasort en Teravalidate uitvoeren voor een beginpunt.  U kunt deze opdrachten op basis van uw resources aanpassen.

**Teragen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate

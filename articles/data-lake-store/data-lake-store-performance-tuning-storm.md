---
title: Azure Data Lake Store Storm prestaties afstemmen richtlijnen | Microsoft Docs
description: Azure Data Lake Store Storm prestaties afstemmen richtlijnen
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
ms.openlocfilehash: 1dfa93643f45a96ded3fd022aa8b1c71d487acb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="performance-tuning-guidance-for-storm-on-hdinsight-and-azure-data-lake-store"></a>Prestaties afstemmen richtlijnen voor Storm op HDInsight en Azure Data Lake Store

Begrijpen factoren die u overwegen moeten bij het afstemmen van de prestaties van een Azure-Storm-topologie. Bijvoorbeeld, is het belangrijk om te begrijpen van de kenmerken van het werk dat door de spouts en de bolts (of het werk is voor i/o- of geheugenintensief). In dit artikel bevat informatie over een reeks prestaties afstemmen richtlijnen over het oplossen van veelvoorkomende problemen.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Data Lake Store-account**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md).
* **Een Azure HDInsight-cluster** met toegang tot een Data Lake Store-account. Zie [een HDInsight-cluster maken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.
* **Een Storm-cluster op Data Lake Store met**. Zie voor meer informatie [Storm op HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-overview).
* **Prestaties afstemmen richtlijnen voor het Data Lake Store**.  Raadpleeg voor algemene prestaties concepten, [Data Lake Store prestaties afstemmen richtlijnen](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance).  

## <a name="tune-the-parallelism-of-the-topology"></a>De parallelle uitvoering van de topologie afstemmen

U kunt mogelijk de prestaties verbeteren door het verhogen van de gelijktijdigheid van de i/o en naar Data Lake Store. Een Storm-topologie bevat een set van configuraties om te bepalen van de parallelle uitvoering:
* Het aantal werkprocessen (de werknemers worden evenredig verdeeld over de virtuele machines).
* Aantal spout executor exemplaren.
* Aantal bolt executor exemplaren.
* Aantal spout taken.
* Het aantal taken dat bolt.

Overweeg het volgende op een cluster met 4 VM's en 4 werkprocessen, 32 spout Executor en 32 spout-taken en 256 bolt Executor en 512 bolt taken:

Elke supervisor, dat een werkrolknooppunt is, heeft één werknemer proces van de virtuele machine (JVM) Java. Dit proces JVM beheert 4 spout threads en 64 bolt threads. Binnen elke thread worden taken worden opeenvolgend uitgevoerd. Elke thread spout heeft 1 taak met de bovenstaande configuratie, en elke thread bolt heeft 2 taken.

Hier worden de verschillende onderdelen die zijn betrokken in Storm, en hoe ze invloed hebben op het niveau van parallelle uitvoering die u hebt:
* Het hoofdknooppunt (Nimbus in Storm genoemd) wordt gebruikt voor het verzenden en beheren van taken. Deze knooppunten hebben geen invloed op de mate van parallelle uitvoering.
* De leidinggevende-knooppunten. In HDInsight, dit komt overeen met een werkrolknooppunt Azure VM.
* De worker-taken zijn Storm processen in de virtuele machines. Elke worker-taak komt overeen met een JVM-exemplaar. Storm distribueert het aantal werkprocessen dat u met de werkrolknooppunten zo gelijkmatig mogelijk opgeeft.
* Spout en Bolts executor-exemplaren. Elk exemplaar executor overeenkomt met een thread die binnen de werknemers (JVMs) worden uitgevoerd.
* Storm-taken. Dit zijn logische taken dat elk van deze threads uitgevoerd. Dit verandert niet het niveau van parallelle uitvoering, zodat u evalueren moet als u meerdere taken per executor of niet nodig.

### <a name="get-the-best-performance-from-data-lake-store"></a>De beste prestaties ophalen uit Data Lake Store

Als u werkt met Data Lake Store, krijgt u de beste prestaties als u het volgende doen:
* Coalesce uw kleine voegt in grotere (bij voorkeur 4 MB).
* Als veel gelijktijdige aanvragen, zoals u kunt doen. Omdat elke thread bolt blokkerende leest doet, die u wilt hebben ergens in het bereik van 8-12-threads per core. Hierdoor blijft de NIC en de CPU goed worden gebruikt. Een grotere virtuele machine kunt meer gelijktijdige aanvragen.  

### <a name="example-topology"></a>Voorbeeldtopologie

Stel dat u hebt een 8 worker-knooppunten met een D13v2 Azure VM. Deze VM is 8 cores zodat tussen de 8 worker-knooppunten die u 64 totaal aantal kernen hebt.

Stel, dat we 8 bolt threads per core doen. Gegeven 64 kernen betekent dit dat we willen 512 totale bolt executor exemplaren (dat wil zeggen, threads). Stel dat in dit geval we beginnen met een JVM per VM en hoofdzakelijk gelijktijdigheid van de thread binnen de JVM gebruiken als u de gelijktijdigheid van taken. Dit betekent dat moet 8 worker-taken (één per virtuele machine van Azure) en 512 bolt Executor. In deze configuratie Storm probeert voor het distribueren van de werknemers gelijkmatig over worker-knooppunten (ook wel bekend als supervisor knooppunten), zodat elk werkrolknooppunt 1 JVM. Nu binnen de toezicht-houders wil Storm de Executor tussen toezichthouders gelijkmatig distribueren, threads zodat elke supervisor (dat wil zeggen, JVM) 8 elk.

## <a name="tune-additional-parameters"></a>Extra parameters optimaliseren
Nadat u de eenvoudige topologie hebt, kunt u overwegen of u wilt aanpassen op een van de parameters:
* **Aantal JVMs per werkrolknooppunt.** Als er een grote hoeveelheden gegevensstructuur (bijvoorbeeld een opzoektabel) is een afzonderlijk exemplaar in die u als host fungeren in elke JVM-geheugen vereist. U kunt ook kunt u de gegevensstructuur in veel threads hebt u minder JVMs. Voor de bolt i/o maakt het aantal JVMs geen zoveel mogelijk van een verschil als het aantal threads dat via deze JVMs toegevoegd. Voor het gemak is het een goed idee om één JVM per worker. Afhankelijk van wat uw bolt doet of welke toepassingen u verwerking vereist, maar mogelijk moet u dit nummer wijzigen.
* **Het aantal spout Executor.** Omdat het voorgaande voorbeeld bolts gebruikt voor het schrijven naar Data Lake Store, is het aantal spouts niet direct relevant zijn voor de prestaties bolt. Afhankelijk van de hoeveelheid verwerking of i/o-gebeurt er in de spout, is het echter een goed idee om af te stemmen de spouts voor de beste prestaties. Zorg ervoor dat er voldoende spouts kunnen de bolts bezet houden. De frequenties van de uitvoer van de spouts moeten overeenkomen met de doorvoer van de bolts. De configuratie van de werkelijke is afhankelijk van de spout.
* **Het aantal taken.** Elke bolt wordt uitgevoerd met één thread. Extra taken per bolt bieden niet extra gelijktijdigheid van taken. De enige keer dat ze van het voordeel zijn is als het proces van de tuple zijn bevestigd dat een groot deel van uw uitvoeringstijd bolt. Het is een goed idee om groep dat veel tuples in een grotere toevoegen voordat u een bevestiging vanuit de bolt verzenden. In de meeste gevallen dus bieden meerdere taken geen extra voordelen.
* **Lokaal of groepering in willekeurige volgorde.** Wanneer deze instelling is ingeschakeld, worden tuples worden verzonden naar bolts binnen hetzelfde werkproces. Dit vermindert tussen processen aanroepen voor communicatie en netwerken. Dit wordt aanbevolen voor de meeste topologieën.

Dit eenvoudige scenario is een goed uitgangspunt. Testen met uw eigen gegevens aanpassen van de voorgaande parameters voor optimale prestaties.

## <a name="tune-the-spout"></a>De spout afstemmen

U kunt de volgende instellingen voor het afstemmen van de spout wijzigen.

- **Tuple time-out: topology.message.timeout.secs**. Deze instelling bepaalt de hoeveelheid tijd nodig is voltooid en bevestiging, ontvangen een bericht voordat wordt beschouwd als mislukt.

- **Maximaal geheugen per werkproces: worker.childopts**. Deze instelling kunt u aanvullende opdrachtregelparameters voor de Java-werknemers opgeven. De meest gebruikte instelling hier is XmX waarmee wordt bepaald van de maximale hoeveelheid geheugen toegewezen aan een JVM-heap.

- **Maximum aantal spout in behandeling: topology.max.spout.pending**. Deze instelling bepaalt het aantal tuples die in vlucht (nog niet op alle knooppunten in de topologie bevestigd) per thread spout op elk gewenst moment.

 Een goede berekening te doen is het schatten van de grootte van elk van uw tuples. Vervolgens bepalen hoeveel geheugen een spout thread heeft. Het totale geheugen toegewezen aan een thread, gedeeld door deze waarde geeft de bovengrens voor de maximale spout in behandeling zijnde parameter.

## <a name="tune-the-bolt"></a>De bolt afstemmen
Wanneer u naar Data Lake Store schrijft, moet u een synchronisatiebeleid grootte (buffer aan de clientzijde) ingesteld op 4 MB. Een domeincontrollerlocaties leeg of hsync() wordt vervolgens alleen uitgevoerd wanneer de buffergrootte is de waarde. Het Data Lake Store-stuurprogramma op de worker VM komt automatisch deze buffering, tenzij u expliciet een hsync() uitvoeren.

De standaard Data Lake Store Storm bolt heeft een grootte sync Beleidsparameter (fileBufferSize), die kan worden gebruikt om af te stemmen met deze parameter.

In de I/O-intensieve topologieën is het een goed idee om elke bolt thread naar een eigen bestand te schrijven en het beleid in een bestand rotatie (fileRotationSize) instellen. Wanneer het bestand een bepaalde grootte bereikt, wordt de stroom wordt automatisch gewist en een nieuw bestand wordt geschreven naar. De aanbevolen grootte voor rotatie is 1 GB.

### <a name="handle-tuple-data"></a>Tuple gegevens verwerken

In de Storm houdt een spout een tuple totdat deze expliciet wordt bevestigd door de bolt. Als een tuple is gelezen door de bolt maar is niet zijn bevestigd, kan de spout niet hebben gehandhaafd in de back-end van Data Lake Store. Nadat een tuple is bevestigd, wordt de spout persistentie kan worden gegarandeerd door de bolt en kunt vervolgens de brongegevens verwijderen uit welke bron is het lezen van.  

Voor optimale prestaties op Data Lake Store, hebt u de bolt buffer is 4 MB aan gegevens van de tuple. Vervolgens terug te schrijven naar het Data Lake Store einde als één 4 MB geheugen schrijven. Nadat de gegevens is geschreven naar de store (door de aanroepende hflush()) de bolt kunt erkent de gegevens terug naar de spout. Dit is wat de voorbeeld-bolt hier opgegeven doet. Het is ook aanvaardbaar is voor een groter aantal tuples voordat de aanroep hflush() is uitgevoerd en de tuples bevestigd bevatten. Dit verhoogt echter het aantal tuples in vlucht dat de spout moet bevatten en daarom verhoogt de hoeveelheid geheugen vereist is per JVM.

> [!NOTE]
Toepassingen hebben een vereiste voor het bevestigen van tuples vaker (op gegevensgroottes minder dan 4 MB) om andere redenen voor slechte prestaties. Echter, die invloed kunnen zijn op de i/o-doorvoer naar de back-end voor opslag. Weeg zorgvuldig deze afweging tegen de bolt i/o-prestaties.

De frequentie van binnenkomst van tuples niet hoog is, zodat de buffer die is 4 MB lang duurt om op te vullen, dan kunt u dit door beperkende:
* Het aantal bolts te verminderen, dus er zijn minder buffers te vullen.
* Elke x leegmaakacties of elk y milliseconden met een beleid op basis van tijd of het aantal is gebaseerd, waarbij een hflush() is geactiveerd en de tot nu toe geteld tuples back zijn bevestigd.

Houd er rekening mee dat de doorvoer in dit geval lager is, maar met een langzame snelheid van gebeurtenissen, maximale doorvoer niet de doelstelling van het grootste toch is. Deze oplossingen te beperken van de totale tijd die het duurt voordat een tuple doorstromen naar de store. Dit kan van belang als u wilt dat een realtime pijplijn zelfs met een snelheid van de lage gebeurtenissen. Let ook op dat als uw tuple frequentie van binnenkomst laag is, u met de parameter topology.message.timeout_secs aanpassen moet zodat de tuples geen time-out terwijl ze in de buffer opgeslagen of verwerkt.

## <a name="monitor-your-topology-in-storm"></a>Uw Storm-topologie bewaken  
Terwijl uw topologie wordt uitgevoerd, kunt u het bewaken in de gebruikersinterface van Storm. Hier volgen de belangrijkste parameters om te kijken naar:

* **Latentie van totale proces worden uitgevoerd.** Dit is de gemiddelde tijd die nodig is een tuple moet worden verzonden door de spout, verwerkt door de bolt en bevestigd.

* **Totaal aantal bolt proces latentie.** Dit is de gemiddelde tijd besteed aan de tuple van de bolt totdat het ontvangt een bevestiging.

* **Totaal aantal bolt uitvoeren latentie.** Dit is de gemiddelde tijd besteed door de bolt in de execute-methode.

* **Aantal fouten.** Dit verwijst naar het aantal tuples die niet volledig moeten worden verwerkt voordat ze een time-out opgetreden.

* **De capaciteit.** Dit is een meting van hoeveel van uw systeem is. Als dit nummer 1 is, worden uw bolts snel ze kunnen werken. Als deze kleiner dan 1 is, verhoogt u de parallelle uitvoering. Als deze groter dan 1 is, verlaagt u de parallelle uitvoering.

## <a name="troubleshoot-common-problems"></a>Algemene problemen
Hier volgen enkele algemene probleemoplossing-scenario's.
* **Er zijn veel tuples time-out opgetreden.** Bekijk elk knooppunt in de topologie om te bepalen waar de knelpunt is. De meest voorkomende reden hiervoor is dat de bolts zich niet bij de spouts te houden. Dit leidt tot tuples vertraging van de interne buffers tijdens het wachten op worden verwerkt. Houd rekening met de time-outwaarde vergroten of verkleinen van de maximale spout in behandeling.

* **Er is een latentie van hoge totale proces worden uitgevoerd, maar een proces bolt lage latentie.** In dit geval is het mogelijk dat de tuples worden niet wordt bevestigd snel genoeg. Controleer of er een voldoende aantal acknowledgers zijn. Een andere mogelijkheid is dat ze nog in de wachtrij te lang voordat deze worden verwerkt de bolts. De maximale spout in behandeling zijnde afnemen.

* **Er is een bolt hoge latentie uitvoeren.** Dit betekent dat de methode execute() van uw bolt duurt te lang is. De code optimaliseren of kijken schrijven grootten en gedrag leegmaken.

### <a name="data-lake-store-throttling"></a>Beperking van Data Lake Store
Als u op de ondergrenzen van de bandbreedte die worden geleverd door de Data Lake Store, kunt u taakfouten ziet. Controleer de logboeken van de taak voor de beperking van fouten. U kunt de parallelle uitvoering verlagen door het vergroten van de container.    

Om te zien als u zijn ophalen beperkt, schakelt u de logboekregistratie aan de clientzijde voor foutopsporing:

1. In **Ambari** > **Storm** > **Config** > **geavanceerde storm-worker-log4j**, wijzigen  **&lt;root niveau = 'info'&gt;**  naar  **&lt;root niveau = 'debug'&gt;**. Alle knooppunten/start de service opnieuw voor de configuratie van kracht te laten worden.
2. Monitor voor de Storm-topologie op de worker-knooppunten Logboeken (onder /var/log/storm/worker-artifacts /&lt;TopologyName&gt;/&lt;poort&gt;/worker.log) voor Data Lake Store uitzonderingen beperking.

## <a name="next-steps"></a>Volgende stappen
Aanvullende prestaties afstemmen voor Storm in kan worden verwezen [deze blog](https://blogs.msdn.microsoft.com/shanyu/2015/05/14/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs/).

Zie voor een extra voorbeeld wilt uitvoeren, [zo'n op GitHub](https://github.com/hdinsight/storm-performance-automation).

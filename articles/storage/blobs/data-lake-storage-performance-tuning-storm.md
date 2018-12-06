---
title: Azure Data Lake Storage Gen2 Storm prestaties richtlijnen over het afstemmen | Microsoft Docs
description: Azure Data Lake Storage Gen2 Storm prestaties richtlijnen over het afstemmen
services: storage
author: swums
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: stewu
ms.openlocfilehash: 845bb739408cb38d823ae662e261d6955726d28a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975191"
---
# <a name="performance-tuning-guidance-for-storm-on-hdinsight-and-azure-data-lake-storage-gen2"></a>Richtlijnen voor Storm op HDInsight en Azure Data Lake Storage Gen2 afstemmen van prestaties

Begrijp de factoren die moeten worden overwogen bij het afstemmen van de prestaties van een Azure-Storm-topologie. Bijvoorbeeld, is het belangrijk om te begrijpen van de kenmerken van het werk dat door de spouts en de bolts (of het werk is voor i/o- of geheugen intensieve). In dit artikel bevat informatie over een scala aan richtlijnen, met inbegrip van algemene problemen met het afstemmen van de prestaties.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een account met Azure Data Lake Storage Gen2**. Zie voor instructies over het maken van een [Quickstart: maken van een account voor analytische](data-lake-storage-quickstart-create-account.md).
* **Een Azure HDInsight-cluster** met toegang tot een Data Lake Storage Gen2-account. Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.
* **Een Storm-cluster uitgevoerd op Data Lake Storage Gen2**. Zie voor meer informatie, [Storm op HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-overview).
* **Richtlijnen voor het Data Lake Storage Gen2 afstemmen van prestaties**.  Zie voor de prestaties van de algemene concepten, [Data Lake Storage Gen2 afstemmen Prestatierichtlijnen](data-lake-storage-performance-tuning-guidance.md).   

## <a name="tune-the-parallelism-of-the-topology"></a>De parallelle uitvoering van de topologie afstemmen

U kunt mogelijk de prestaties verbeteren door het verhogen van de waarde voor concurrency van de i/o en naar Data Lake Storage Gen2. Een Storm-topologie bevat een set configuraties om te bepalen van de parallelle uitvoering:
* Het aantal werkprocessen (de werknemers zijn evenredig verdeeld over de VM's).
* Het aantal spout executor-exemplaren.
* Het aantal bolt executor-exemplaren.
* Het aantal spout-taken.
* Het aantal taken bolt.

Bijvoorbeeld: op een cluster met 4 virtuele machines en 4 werkprocessen, 32 spout Executor en 32 spout taken en 256 bolt Executor en 512 bolt taken, houd rekening met het volgende:

Elke supervisor, die een worker-knooppunt is, heeft één werknemer proces voor virtuele Java virtuele machine (JVM). Dit proces JVM beheert 4 spout-threads en 64 bolt threads. Binnen elke thread worden taken worden opeenvolgend uitgevoerd. Elke thread spout heeft 1 taak met de vorige configuratie, en elke thread bolt heeft 2 taken.

Hier volgen de verschillende onderdelen betrokken in Storm, en hoe ze van invloed op het niveau van parallelle uitvoering die u hebt:
* Het hoofdknooppunt (Nimbus in Storm genoemd) wordt gebruikt voor het indienen en beheren van taken. Deze knooppunten hebben geen invloed op de mate van parallelle uitvoering.
* De supervisorknooppunten. In HDInsight, dit komt overeen met een werkrolknooppunt virtuele Azure-machine.
* De worker-taken zijn Storm-processen die worden uitgevoerd in de virtuele machines. Elke werknemertaak komt overeen met een JVM-exemplaar. Storm verdeelt het aantal werkprocessen die u met de werkrolknooppunten zo gelijkmatig mogelijk opgeeft.
* Spout en executor-exemplaren voor bolt. Elk exemplaar executor komt overeen met een thread die binnen de werknemers (JVMs) worden uitgevoerd.
* Storm-taken. Dit zijn logische taken dat elk van deze threads uitvoeren. Het niveau van parallelle uitvoering, worden niet gewijzigd, zodat u evalueren moet als u meerdere taken per executor nodig of niet hebt.

### <a name="get-the-best-performance-from-data-lake-storage-gen2"></a>De beste prestaties ophalen uit Data Lake Storage Gen2

Als u werkt met Data Lake Storage Gen2, krijgt u de beste prestaties als u het volgende doen:
* Samenvoegen uw kleine naar grotere toegevoegd.
* Als veel gelijktijdige aanvragen kunt u doen. Omdat elke thread bolt blokkerende leesbewerkingen presteert, die u wilt hebben ergens in het bereik van 8-12-threads per kern. Hierdoor blijft de NIC en de CPU en gebruikt. Een grotere virtuele machine kunt meer gelijktijdige aanvragen.  

### <a name="example-topology"></a>Voorbeeldtopologie

Stel, dat u hebt een 8 worker-knooppunt-cluster met een D13v2 Azure virtuele machine. Deze virtuele machine heeft 8 kernen, zodat tussen de 8 worker-knooppunten die u 64 totaal aantal kerngeheugens hebt.

Stel dat we doen 8 bolt threads per kern. Opgegeven 64 kernen betekent dat we willen 512 totale bolt executor-exemplaren (dat wil zeggen, threads). Stel dat in dit geval we beginnen met een JVM per VM en vooral de gelijktijdigheid van de thread binnen de JVM gebruiken om te bereiken van gelijktijdigheid. Dit betekent dat we moet 8 worker-taken (één per Azure-VM) en 512 bolt Executor. Storm probeert deze configuratie voor het distribueren van de werknemers gelijkmatig over worker-knooppunten (ook wel bekend als supervisorknooppunten), zodat elke worker-knooppunt 1 JVM. Nu binnen de toezichthouders wil Storm de Executor tussen supervisors gelijkmatig verdelen, threads zodat elke supervisor (dat wil zeggen, JVM) 8 elk.

## <a name="tune-additional-parameters"></a>Aanvullende parameters optimaliseren
Nadat u de basistopologie hebt, kunt u overwegen of u wilt een van de parameters aanpassen:
* **Aantal JVMs per worker-knooppunt.** Als u beschikt over de structuur van een grote hoeveelheden gegevens (bijvoorbeeld een lookup-tabel) is die u als host fungeren in elke JVM-geheugen vereist een afzonderlijk exemplaar. U kunt ook de gegevensstructuur veel threads gebruiken, hebt u minder JVMs. Voor i/o van het bolt maakt het aantal JVMs geen zoveel mogelijk van een verschil als het aantal threads dat in deze JVMs toegevoegd. Voor het gemak is het een goed idee om één JVM per worker. Afhankelijk van wat uw bolt doet of welke toepassing u verwerking vereist, maar mogelijk moet u dit nummer wijzigen.
* **Het aantal spout Executor.** Omdat het voorgaande voorbeeld bolts voor het schrijven naar Data Lake Storage Gen2 gebruikt, is het aantal spouts niet rechtstreeks relevant zijn voor de prestaties bolt. Echter, afhankelijk van de hoeveelheid verwerking of i/o in de spout gebeurt, is het een goed idee om af te stemmen de spouts voor de beste prestaties. Zorg ervoor dat u onvoldoende spouts hebt kunnen de bolts Bezig te houden. De tarieven van de uitvoer van de spouts moeten overeenkomen met de doorvoer van de bolts. De daadwerkelijke configuratie is afhankelijk van de spout.
* **Het aantal taken.** Elke bolt wordt uitgevoerd als een enkele thread. Extra taken per bolt niet elke extra gelijktijdigheid te bieden. De enige keer dat ze van voordeel zijn is als het proces van de tuple zijn bevestigd dat een groot deel van de uitvoeringstijd bolt. Het is een goed idee om groep dat veel tuples in een grotere toevoegen voordat u een bevestiging vanuit de bolt verzenden. In de meeste gevallen bieden meerdere taken dus geen extra voordelen.
* **Lokale of groepering in willekeurige volgorde.** Wanneer deze instelling is ingeschakeld, worden tuples worden verzonden naar bolts binnen hetzelfde werkproces. Dit vermindert het aanroepen van communicatie en netwerk tussen processen. Dit wordt aanbevolen voor de meeste topologieën.

Dit eenvoudige scenario is een goed uitgangspunt. Testen met uw eigen gegevens aanpassen van de bovenstaande parameters om optimale prestaties te behalen.

## <a name="tune-the-spout"></a>De spout afstemmen

U kunt de volgende instellingen voor het afstemmen van de spout wijzigen.

- **Tuple time-out: topology.message.timeout.secs**. Deze instelling bepaalt de hoeveelheid tijd nodig is om te voltooien en bevestiging, ontvangen een bericht voordat deze wordt beschouwd als mislukt.

- **Maximaal geheugen per werkproces: worker.childopts**. Deze instelling kunt u aanvullende opdrachtregelparameters voor de Java-werknemers opgeven. De meest gebruikte instelling hier is XmX, waarmee wordt bepaald hoe de maximale hoeveelheid geheugen toegewezen aan van een JVM-heap.

- **Maximum aantal spout in behandeling: topology.max.spout.pending**. Deze instelling bepaalt het aantal tuples die in vlucht (nog niet bevestigd op alle knooppunten in de topologie) per spout-thread op elk gewenst moment.

 Er is een goede berekening te doen om in te schatten van de grootte van elk van uw tuples. Vervolgens bepaalt hoeveel geheugen een spout-thread heeft. Het totale geheugen toegewezen aan een thread, gedeeld door deze waarde geeft de bovengrens voor de maximale spout in afwachting van parameter.

De standaard Data Lake Storage Gen2 Storm-bolt heeft een grootte van synchronisatie-Beleidsparameter (fileBufferSize) die kan worden gebruikt om af te stemmen met deze parameter.

In I/O-intensieve topologieën is het een goed idee om elke bolt thread naar een eigen bestand te schrijven en om in te stellen van een bestandsbeleid draaiing (fileRotationSize). Wanneer het bestand een bepaalde grootte bereikt, wordt de stroom moet automatisch worden leeggemaakt en een nieuw bestand wordt geschreven naar. De aanbevolen grootte voor rotatie is 1 GB.

## <a name="monitor-your-topology-in-storm"></a>Bewaken van uw Storm-topologie  
Terwijl uw topologie wordt uitgevoerd, kunt u het bewaken in de gebruikersinterface van Storm. Hier volgen de belangrijkste parameters om te kijken naar:

* **Totaal aantal proces tot uitvoering van latentie.** Dit is de gemiddelde tijd die nodig is een tuple moet worden gegenereerd door de spout, verwerkt door de bolt en bevestigd.

* **Totaal aantal bolt proceslatentie.** Dit is de gemiddelde tijd besteed aan de tuple van de bolt totdat er een bevestiging.

* **Totaal aantal bolt latentie voor uitvoering.** Dit is de gemiddelde tijd besteed aan het door de bolt in de execute-methode.

* **Het aantal fouten.** Dit verwijst naar het aantal tuples die niet volledig worden verwerkt voordat ze een time-out.

* **De capaciteit.** Dit is een maateenheid voor hoe bezet is van uw systeem. Als dit nummer 1 is, worden uw bolts zo snel mogelijk kunnen werken. Als dit minder dan 1 is, verhoogt u de parallelle uitvoering. Als dit groter dan 1 is, beperkt u de parallelle uitvoering.

## <a name="troubleshoot-common-problems"></a>Algemene problemen
Hier volgen enkele algemene probleemoplossing-scenario's.
* **Veel tuples time-outs zijn opgetreden.** Zoeken op elk knooppunt in de topologie om te bepalen waar het knelpunt is. De meest voorkomende reden hiervoor is dat de bolts zijn niet in staat om te blijven met de spouts. Dit leidt tot tuples overvol van de interne buffers tijdens het wachten op om te worden verwerkt. Houd rekening met de time-outwaarde vergroten of verkleinen van de maximale spout in behandeling.

* **Er is een latentie van de uitvoering van hoge totale proces, maar de latentie voor een lage bolt-verwerking.** In dit geval is het mogelijk dat de tuples worden niet worden bevestigd snel genoeg. Controleer of er een voldoende aantal acknowledgers zijn. Een andere mogelijkheid is dat ze te in de wachtrij te lang vinden zijn voordat deze worden verwerkt de bolts. Hiermee verkleint u de maximale spout in behandeling.

* **Er is een hoge bolt latentie voor uitvoering.** Dit betekent dat de methode Execute()) van uw bolt te lang duurt. Optimaliseren van de code of Ga naar schrijfgrootten en gedrag leegmaken.

### <a name="data-lake-storage-gen2-throttling"></a>Data Lake Storage Gen2 beperking
Als u de grenzen van de bandbreedte die door Data Lake Storage Gen2 bereikt, ziet u mogelijk mislukte taken. Raadpleeg de logboeken van de taak voor de beperking van fouten. U kunt de parallelle uitvoering verlagen door het vergroten van de container.    

Inschakelen om te controleren als u ophalen beperkt, logboekregistratie aan de clientzijde voor de foutopsporing:

1. In **Ambari** > **Storm** > **Config** > **geavanceerde storm-worker-log4j**, wijzigen **&lt;hoofd-niveau = 'info'&gt;** naar  **&lt;hoofd-niveau = "debug"&gt;**. Alle knooppunten/start de service opnieuw voor de configuratie van kracht te laten worden.
2. Monitor voor de Storm-topologie-op de worker-knooppunten Logboeken (onder /var/log/storm/worker-artifacts /&lt;TopologyName&gt;/&lt;poort&gt;/worker.log) voor Data Lake Storage Gen2 beperking uitzonderingen.

## <a name="next-steps"></a>Volgende stappen
Prestaties afstemmen voor Storm kan naar worden verwezen [deze blog](https://blogs.msdn.microsoft.com/shanyu/2015/05/14/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs/).

Zie voor een aanvullende voorbeeld om uit te voeren, [deze een op GitHub](https://github.com/hdinsight/storm-performance-automation).

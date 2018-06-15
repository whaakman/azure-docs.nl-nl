---
title: Taken met Spark-Streaming exact maken-eenmaal gebeurtenis verwerking - Azure HDInsight | Microsoft Docs
description: Het instellen van het Spark-Streaming voor het verwerken van een gebeurtenis slechts eenmaal.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ramoha
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: ramoha
ms.openlocfilehash: 48b1d3811f3a8f6190c58e9646ab0d820859fc21
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34164855"
---
# <a name="create-spark-streaming-jobs-with-exactly-once-event-processing"></a>Taken met Spark-Streaming exact maken-eenmaal gebeurtenis verwerken

Stroom verwerking toepassingen nemen verschillende benaderingen hoe ze opnieuw verwerken om berichten te na een storing in het systeem handelen:

* Ten minste eenmaal: elk bericht is gegarandeerd worden verwerkt, maar het kan meer dan één keer verwerkt.
* Maximaal één keer: elk bericht kan of kunnen niet worden verwerkt. Als een bericht is verwerkt, wordt slechts één keer wordt verwerkt.
* Slechts één keer: elk bericht is gegarandeerd slechts eenmaal worden verwerkt.

Dit artikel ziet u het configureren van Spark-Streaming voor exact-één keer worden verwerkt.

## <a name="exactly-once-semantics-with-spark-streaming"></a>Exact-eenmaal semantiek met Spark-Streaming

Ga eerst na hoe alle system foutpunten opnieuw opstarten nadat ik een probleem en hoe kunt u voorkomen dat gegevens verloren gaan. Een Spark-Streaming-toepassing heeft:

* Een invoerbron
* Een of meer ontvanger processen die gegevens van de invoerbron ophalen
* Taken die de gegevens verwerken
* Uitvoerlocatie
* Een stuurprogramma-proces dat de taak langlopende beheert

Exact-zodra semantiek vereisen dat gegevens niet verloren op elk gewenst moment en die berichtverwerking kan opnieuw worden gestart, ongeacht waar de fout zich voordoet.

### <a name="replayable-sources"></a>Replayable bronnen

De bron die uw toepassing Spark-Streaming de gebeurtenissen van leest moet zijn *replayable*. Dit betekent dat in gevallen waarin het bericht is opgehaald, maar vervolgens het systeem is mislukt voordat het bericht kan worden bewaard of verwerkt, de bron moet hetzelfde bericht opnieuw opgeven.

Geef replayable bronnen in Azure, zowel Azure Event Hubs en Kafka op HDInsight. Een ander voorbeeld van een replayable bron is een fouttolerante bestandssysteem zoals HDFS, Azure Storage-blobs of Azure Data Lake Store, waar alle gegevens definitief verloren en blijven op elk gewenst moment u kunt de gegevens in zijn geheel opnieuw te lezen.

### <a name="reliable-receivers"></a>Betrouwbare ontvangers

Gegevensbronnen in Spark-Streaming Event Hubs en Kafka hebt *betrouwbare ontvangers*, waarbij elke ontvanger van wordt bijgehouden voortgangsgegevens lezen van de bron. Een betrouwbare ontvanger persistente de status in de fouttolerante opslag, binnen ZooKeeper of in Spark-Streaming controlepunten naar HDFS geschreven. Als deze een ontvanger mislukt en later wordt opnieuw gestart, deze kan worden opgepikt waar deze gebleven was.

### <a name="use-the-write-ahead-log"></a>Het logboek schrijven-Ahead gebruiken

Spark-Streaming ondersteunt het gebruik van een vooraf geschreven logboek, waarbij elke ontvangen gebeurtenis eerst naar van Spark controlepunt directory in de fouttolerante opslag geschreven en vervolgens opgeslagen in een robuuste gedistribueerd gegevensset (RDD). De fouttolerante opslag is in Azure, HDFS ondersteund door Azure Storage of Azure Data Lake Store. In uw toepassing Spark-Streaming het logboek schrijven-Ahead is ingeschakeld voor alle ontvangers door in te stellen de `spark.streaming.receiver.writeAheadLog.enable` configuratie-instelling voor `true`. Het logboek schrijven-Ahead biedt fouttolerantie voor mislukte van zowel het stuurprogramma en de Executor.

Voor werknemers met het uitvoeren van taken in de gebeurtenisgegevens worden opgeslagen, is elke RDD per definitie beide gerepliceerd en verdeeld over meerdere werknemers. Als een taak mislukt omdat het werkproces dat deze is vastgelopen uitvoert, de taak opnieuw wordt gestart op een andere werknemer die een replica van gegevens van de gebeurtenis heeft, zodat de gebeurtenis is niet verloren gaan.

### <a name="use-checkpoints-for-drivers"></a>Controlepunten voor stuurprogramma's gebruiken

De taak stuurprogramma's moeten opnieuw worden gestart. Als het uitvoeren van uw toepassing Spark-Streaming stuurprogramma vastloopt, duurt het naar beneden aan alle actieve ontvangers, taken en eventuele RDDs gebeurtenisgegevens op te slaan. In dit geval moet u kunt de voortgang van de taak opslaan zodat u deze later kunt hervatten. Dit wordt bereikt door het plaatsen van controlepunten de omgeleid acyclische grafiek (DAG) van de DStream periodiek naar fouttolerante opslag. De metagegevens van de DAG bevat de configuratie die wordt gebruikt voor het maken van de streaming-toepassing, de bewerkingen die de toepassing definiëren en batches die in de wachtrij geplaatst, maar nog niet voltooid. Deze metagegevens kan opnieuw worden gestart vanaf het controlepunt-informatie van een mislukte stuurprogramma. Wanneer het stuurprogramma opnieuw is opgestart, wordt het nieuwe ontvangers dat zelf de gebeurtenisgegevens naar RDDs uit het logboek schrijven-Ahead herstellen gestart.

Controlepunten zijn ingeschakeld in het Spark-Streaming in twee stappen. 

1. Configureer het pad voor de opslag voor de controlepunten in het object StreamingContext:

    waarde opnemen ws = nieuwe StreamingContext (spark, Seconds(1)) ssc.checkpoint("/path/to/checkpoints")

    In HDInsight, moeten deze controlepunten worden opgeslagen op de standaard-opslag gekoppeld aan het cluster, Azure Storage of Azure Data Lake Store.

2. Geef vervolgens een controlepunt-interval (in seconden) op de DStream. Tijdens een interval, de gegevens van de gebruikersstatus is afgeleid van de invoer-gebeurtenis permanent wordt opgeslagen in opslag. Gegevens van de permanente status kan verminderen de berekening nodig hebt bij het opnieuw opbouwen van de status van de bron-gebeurtenis.

    waarde opnemen regels ssc.socketTextStream ('hostnaam' 9999) = lines.checkpoint(30) ssc.start() ssc.awaitTermination()

### <a name="use-idempotent-sinks"></a>Gebruik de idempotent sinks

De bestemming sink waarnaar uw taak resultaten schrijft moet verwerken van de situatie waarin deze meer dan één keer hetzelfde resultaat wordt gegeven. De sink moet kunnen dergelijke dubbele resultaten detecteren en te negeren. Een *idempotent* sink kan meerdere keren worden aangeroepen met dezelfde gegevens zonder wijzigingen van status.

U kunt de idempotent put maken door het implementeren van logica waarmee wordt eerst gecontroleerd of de aanwezigheid van het binnenkomende resultaat in het gegevensarchief. Als het resultaat is al bestaat, wordt het schrijven moet worden weergegeven vanuit het perspectief van de Spark-taak is mislukt, maar in werkelijkheid uw data store de dubbele gegevens genegeerd. Als het resultaat niet bestaat, kunnen de sink moet deze nieuwe resultaat invoegen in de opslag. 

U kunt bijvoorbeeld een opgeslagen procedure met Azure SQL Database die gebeurtenissen in een tabel invoegen. Deze opgeslagen procedure zoekt eerst naar de gebeurtenis met sleutelvelden, en alleen als er geen overeenkomende gebeurtenis gevonden is de record ingevoegd in de tabel.

Een ander voorbeeld is het gebruik van een gepartitioneerde bestandssysteem, zoals Azure Storage-blobs of Azure Data Lake store. In dit geval hoeft de sink logische niet te controleren op het bestaan van een bestand. Als het bestand voor de gebeurtenis bestaat, is het eenvoudig door dezelfde gegevens overschreven. Anders wordt een nieuw bestand gemaakt in de berekende pad.

## <a name="next-steps"></a>Volgende stappen

* [Spark-Streaming-overzicht](apache-spark-streaming-overview.md)
* [Maken van maximaal beschikbare Spark-Streaming-taken in YARN](apache-spark-streaming-high-availability.md)

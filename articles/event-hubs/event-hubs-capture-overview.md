---
title: Overzicht van Azure Event Hubs vastleggen | Microsoft Docs
description: Vastleggen van telemetriegegevens met Event Hubs vastleggen
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e53cdeea-8a6a-474e-9f96-59d43c0e8562
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: sethm;darosa
ms.openlocfilehash: fbd4aef62891341ad3760b74cd8aaee7abf7b827
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="azure-event-hubs-capture"></a>Azure Event Hubs vastleggen

Azure Event Hubs vastleggen, kunt u automatisch de streaminggegevens in Event Hubs te leveren een [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/) of [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) -account van uw keuze, met de extra flexibiliteit het opgeven van een interval van tijd of grootte. Het instellen van het vastleggen is snel, er zijn geen administratieve kosten uit te voeren en wordt automatisch geschaald met Event Hubs [doorvoereenheden](event-hubs-features.md#capacity). Event Hubs vastleggen is de eenvoudigste manier om te streamen gegevens laden in Azure, en kunt u zich kunt richten op het verwerken van gegevens in plaats van op het opnemen van gegevens.

Event Hubs vastleggen, kunt u in real-time en op basis van batch pijplijnen op de dezelfde stroom verwerken. Dit betekent dat u kunt oplossingen bouwt die groeien met uw behoeften gedurende een bepaalde periode. Of u bij het bouwen van batch-systemen vandaag nog met het oog op toekomstige realtime verwerking of u wilt een efficiënte koude pad toevoegen aan een bestaande realtime oplossing, is het vastleggen van Event Hubs werken met gegevensstromen eenvoudiger.

## <a name="how-event-hubs-capture-works"></a>Event Hubs vastleggen werking

Event Hubs is een duurzame buffer tijd bewaren voor telemetrie inkomend, vergelijkbaar met een gedistribueerde logboek. De sleutel voor schaling in Event Hubs is de [gepartitioneerde consumer model](event-hubs-features.md#partitions). Elke partitie is een onafhankelijke segment van gegevens en onafhankelijk is verbruikt. Na verloop van tijd deze gegevens van jaar uit, op basis van de configureerbare bewaarperiode. Als gevolg hiervan een bepaalde gebeurtenis hub nooit ' te vol raakt. "

Event Hubs vastleggen, kunt u uw eigen Azure Blob storage-account en een container of een Azure Data Lake Store-account worden gebruikt voor het opslaan van de vastgelegde gegevens opgeven. Deze accounts kunnen zich in dezelfde regio bevinden als uw event hub of in een andere regio, toe te voegen aan de flexibiliteit van de functie voor het vastleggen van Event Hubs.

Gegevensopname is geschreven in [Apache Avro] [ Apache Avro] indeling: een compact, snelle, binaire indeling die uitgebreide gegevensstructuren inlineschema biedt. Deze indeling wordt veel gebruikt in de Hadoop-ecosysteem, Stream Analytics en Azure Data Factory. Meer informatie over het werken met Avro vindt verderop in dit artikel.

### <a name="capture-windowing"></a>Windowing vastleggen

Event Hubs vastleggen, kunt u een venster instellen om te bepalen vast te leggen. Dit venster is een minimale grootte en de configuratie van de tijd met een 'eerste WINS-beleid,' wat betekent dat de eerste trigger is aangetroffen, een capture-bewerking wordt. Als u een vijftien minuten hebt, is 100 MB venster vastleggen en verzenden van 1 MB per seconde, de grootte van venster triggers voordat het tijdvenster. Elke partitie afzonderlijk vastgelegd en schrijft een voltooide blok-blob op het moment van vastleggen, met de naam van de tijd waarop de capture-interval is opgetreden. De naamconventie voor opslag is als volgt:

```
{Namespace}/{EventHub}/{PartitionId}/{Year}/{Month}/{Day}/{Hour}/{Minute}/{Second}
```

Houd er rekening mee dat de datumwaarden worden opgevuld met nullen; de bestandsnaam van een voorbeeld is mogelijk:

```
https://mystorageaccount.blob.core.windows.net/mycontainer/mynamespace/myeventhub/0/2017/12/08/03/03/17.avro
```

### <a name="scaling-to-throughput-units"></a>Doorvoereenheden schalen

Event Hubs-verkeer wordt beheerd door [doorvoereenheden](event-hubs-features.md#capacity). Eén doorvoereenheid kunt 1 MB per seconde of 1000 gebeurtenissen per seconde van inkomende en twee keer de omvang van uitgaande gegevens. Standaard Event Hubs kunnen worden geconfigureerd met 1-20 doorvoereenheden en u kunt meer aanschaffen bij een quotum verhogen [ondersteuningsaanvraag][support request]. Gebruik dat buiten uw aangeschafte doorvoereenheden wordt beperkt. Event Hubs vastleggen kopieert gegevens rechtstreeks vanuit de interne opslag van de Event Hubs het overslaan van doorvoer eenheid uitgaande quota en opslaan van uw uitgaande voor andere lezers verwerking zoals Stream Analytics of Spark.

Na de configuratie vastleggen van Event Hubs wordt automatisch uitgevoerd wanneer u uw eerste gebeurtenis verzenden en wordt hervat. Event Hubs schrijft lege bestanden voor de downstreamverwerking te weten dat het proces werkt te vereenvoudigen, wanneer er geen gegevens zijn. Deze procedure biedt een voorspelbare uitgebracht en de markering die kan worden ingevoerd uw batch-processors.

## <a name="setting-up-event-hubs-capture"></a>Instellen van het vastleggen van Event Hubs

U kunt vastleggen configureren op de event hub maken tijd met behulp van de [Azure-portal](https://portal.azure.com), of met behulp van Azure Resource Manager-sjablonen. Raadpleeg voor meer informatie de volgende artikelen:

- [Inschakelen van Event Hubs vastleggen met behulp van de Azure-portal](event-hubs-capture-enable-through-portal.md)
- [Maken van een naamruimte Event Hubs met een event hub en vastleggen met behulp van een Azure Resource Manager-sjabloon inschakelen](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-the-captured-files-and-working-with-avro"></a>De vastgelegde bestanden verkennen en werken met Avro

Vastleggen van Event Hubs maakt bestanden in de Avro-indeling die is opgegeven op de geconfigureerde periode. U kunt deze bestanden weergeven in een hulpprogramma zoals [Azure Opslagverkenner][Azure Storage Explorer]. U kunt downloaden de bestanden lokaal te werken.

De bestanden die wordt geproduceerd door het vastleggen van Event Hubs hebben de volgende Avro-schema:

![][3]

Een eenvoudige manier om te verkennen Avro-bestanden is via de [Avro extra] [ Avro Tools] jar van Apache. Na het downloaden van dit jar, ziet u het schema van een specifiek Avro-bestand met de volgende opdracht:

```
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

Met deze opdracht retourneert

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

U kunt ook de Avro-hulpprogramma's gebruiken voor het bestand converteren naar JSON-indeling en andere bewerkingen uitvoeren.

Als u meer geavanceerde verwerken, downloaden en installeren van Avro voor uw keuze van platform. Op het moment van schrijven van dit er implementaties beschikbaar zijn voor C, C++, C\#, Java, NodeJS, Perl, PHP, Python en Ruby.

Apache Avro is voltooid aan de slag-handleidingen voor [Java] [ Java] en [Python][Python]. U kunt ook lezen de [aan de slag met Event Hubs vastleggen](event-hubs-capture-python.md) artikel.

## <a name="how-event-hubs-capture-is-charged"></a>Hoe Event Hubs vastleggen wordt in rekening gebracht

Event Hubs vastleggen datalimiet geldt ook voor doorvoereenheden: als een per uur kosten. De kosten is rechtstreeks evenredig met het aantal doorvoereenheden hebt aangeschaft voor de naamruimte. Als doorvoereenheden worden verhoogd of verlaagd, wordt Event Hubs vastleggen meters verhogen en minder overeenkomende prestaties bieden. De meters optreden in combinatie. Zie voor prijsinformatie, [prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/). 

## <a name="next-steps"></a>Volgende stappen

Event Hubs vastleggen is de eenvoudigste manier om gegevens naar Azure. Met Azure Data Lake en Azure Data Factory Azure HDInsight kunt u batchverwerking uitvoeren en andere analyses met behulp van bekende hulpprogramma's en platforms van uw keuze op elke schaal moet u.

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Aan de slag verzenden en ontvangen van gebeurtenissen](event-hubs-dotnet-framework-getstarted-send.md)
* [Event Hubs-overzicht][Event Hubs overview]

[Apache Avro]: http://avro.apache.org/
[support request]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[Azure Storage Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-capture-overview/event-hubs-capture3.png
[Avro Tools]: http://www-us.apache.org/dist/avro/avro-1.8.2/java/avro-tools-1.8.2.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Event Hubs overview]: event-hubs-what-is-event-hubs.md

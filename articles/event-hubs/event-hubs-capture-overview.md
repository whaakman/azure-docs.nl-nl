---
title: Overzicht van Azure Eventhubs Capture | Microsoft Docs
description: Telemetrische gegevens vastleggen met Event Hubs Capture
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: e53cdeea-8a6a-474e-9f96-59d43c0e8562
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 91263d7f6e8c0c2774df560a45eeff9efc672343
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408421"
---
# <a name="azure-event-hubs-capture"></a>Azure Event Hubs vastleggen

Azure Event Hubs Capture kunt u automatisch de gegevensstroom bezorgen in Event Hubs kunt u een [Azure Blob-opslag](https://azure.microsoft.com/services/storage/blobs/) of [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) -account van uw keuze, met de extra flexibiliteit een tijds- of grootte-interval op te geven. Instellen van Capture is snel, er zijn geen administratieve kosten uit te voeren en wordt automatisch geschaald met Event Hubs [doorvoereenheden](event-hubs-features.md#capacity). Event Hubs Capture is de eenvoudigste manier om het streaming-gegevens laden in Azure, en kunt u zich richten op gegevensverwerking in plaats van gegevensregistratie.

Event Hubs Capture kunt u realtime en op basis van een batch-pijplijnen in dezelfde stroom verwerken. Dit betekent dat u oplossingen die meegroeien met uw behoeften na verloop van tijd kunt bouwen. Of u op basis van een batch-systemen vandaag nog met het oog voor toekomstige verwerking in realtime bouwt, of u wilt een efficiënte koude pad toevoegen aan een bestaande oplossing voor realtime, maakt Event Hubs Capture werken met het streamen van gegevens gemakkelijker.

## <a name="how-event-hubs-capture-works"></a>Event Hubs Capture werking

Eventhubs is een duurzame buffer retentietijd voor telemetrie inkomend verkeer, vergelijkbaar met een gedistribueerde logboek. De sleutel voor het schalen in Event Hubs is de [basis van gepartitioneerd gebruik model](event-hubs-features.md#partitions). Elke partitie is een onafhankelijke segment van de gegevens en onafhankelijk van elkaar worden verbruikt. Na verloop van tijd leeftijden deze gegevens op uit, op basis van de configureerbare bewaarperiode. "Als gevolg hiervan een bepaalde event hub nooit te vol raakt."

Event Hubs Capture kunt u uw eigen Azure Blob storage-account en een container of een Azure Data Lake Store-account, die worden gebruikt voor het opslaan van de vastgelegde gegevens opgeven. Deze accounts kunnen zich in dezelfde regio als uw event hub of in een andere regio toe te voegen aan de flexibiliteit van de Event Hubs Capture-functie.

Vastgelegde gegevens worden geschreven [Apache Avro] [ Apache Avro] indeling: een compacte, snel, binaire indeling die uitgebreide gegevensstructuren met inline-schema biedt. Deze indeling wordt veel gebruikt in de Hadoop-ecosysteem, Stream Analytics en Azure Data Factory. Meer informatie over het werken met Avro vindt u verderop in dit artikel.

### <a name="capture-windowing"></a>Windowing vastleggen

Event Hubs Capture kunt u het instellen van een venster voor het beheren van vast te leggen. Dit venster is een minimale grootte en de configuratie van de tijd met 'eerste WINS-beleid,' wat betekent dat een capture-bewerking zorgt ervoor dat de eerste trigger is aangetroffen. Als u een vijftien minuten hebt, wordt 100 MB venster vastleggen en verzenden van 1 MB per seconde, de grootte van venster triggers voor het tijdvenster. Elke partitie afzonderlijk vastgelegd en schrijft een voltooide blok-blob op het moment van opname, met de naam van de tijd waarbinnen de capture-interval is opgetreden. De naamconventie voor opslag is als volgt:

```
{Namespace}/{EventHub}/{PartitionId}/{Year}/{Month}/{Day}/{Hour}/{Minute}/{Second}
```

Houd er rekening mee dat de date-waarden worden opgevuld met nullen; een voorbeeld van de bestandsnaam is mogelijk:

```
https://mystorageaccount.blob.core.windows.net/mycontainer/mynamespace/myeventhub/0/2017/12/08/03/03/17.avro
```

### <a name="scaling-to-throughput-units"></a>Schalen naar doorvoereenheden

Verkeer van Event Hubs wordt bepaald door [doorvoereenheden](event-hubs-features.md#capacity). Eén doorvoereenheid toestaat 1 MB per seconde of 1000 gebeurtenissen per seconde van inkomend verkeer en twee keer de omvang van uitgaand verkeer. Event Hubs Standard aan kan worden geconfigureerd met 1-20 doorvoereenheden, en u kunt meer kopen met een quotum verhogen [ondersteuningsaanvraag][support request]. Gebruik bij meer dan uw aangeschafte doorvoereenheden wordt vertraagd. Event Hubs Capture kopieert gegevens rechtstreeks van de interne opslag van de Event Hubs voor het overslaan van doorvoer eenheid uitgaande quota en opslaan van uw uitgaand verkeer voor andere lezers van de verwerking, zoals Stream Analytics of Apache Spark.

Wanneer geconfigureerd, Event Hubs Capture wordt automatisch uitgevoerd wanneer u uw eerste gebeurtenis verzenden en verder wordt uitgevoerd. Event Hubs schrijft lege bestanden te vereenvoudigen voor uw downstream-verwerkingen weten of het proces werkt, als er geen gegevens. Deze procedure biedt een voorspelbare uitgebracht en markering waarin uw batch-processors.

## <a name="setting-up-event-hubs-capture"></a>Instellen van Event Hubs Capture

U kunt u Capture configureren op de event hub met behulp de [Azure-portal](https://portal.azure.com), of met behulp van Azure Resource Manager-sjablonen. Raadpleeg voor meer informatie de volgende artikelen:

- [Event Hubs Capture met behulp van de Azure-portal inschakelen](event-hubs-capture-enable-through-portal.md)
- [Een Event Hubs-naamruimte maken met een event hub en Capture inschakelen met behulp van een Azure Resource Manager-sjabloon](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-the-captured-files-and-working-with-avro"></a>De vastgelegde bestanden verkennen en werken met Avro

Event Hubs Capture, maakt bestanden in de Avro-indeling, zoals opgegeven op de geconfigureerde periode. U kunt deze bestanden weergeven in een hulpprogramma zoals [Azure Storage Explorer][Azure Storage Explorer]. U kunt downloaden de bestanden lokaal te werken.

De bestanden die worden geproduceerd door Event Hubs Capture hebben de volgende Avro-schema:

![][3]

Een eenvoudige manier om te verkennen Avro-bestanden is met behulp van de [Avro-hulpprogramma's] [ Avro Tools] jar van Apache. Nadat u hebt deze jar gedownload, ziet u het schema van een specifiek Avro-bestand met de volgende opdracht:

```shell
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

Deze opdracht wordt geretourneerd

```json
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

Als u meer geavanceerde verwerken, downloaden en installeren van Avro naar keuze van platform. Op het moment van dit artikel is geschreven, er zijn implementaties beschikbaar voor C, C++, C\#, Java, NodeJS, Perl, PHP, Python en Ruby.

Apache Avro is voltooid aan de slag-handleidingen voor [Java] [ Java] en [Python][Python]. U kunt ook lezen de [aan de slag met Event Hubs Capture](event-hubs-capture-python.md) artikel.

## <a name="how-event-hubs-capture-is-charged"></a>Hoe Event Hubs Capture wordt in rekening gebracht

Op dezelfde manier naar Event Hubs Capture is gebruik in eenheden gegevensdoorvoer: als een uurtarief. De kosten is rechtstreeks in verhouding met het aantal doorvoereenheden hebt aangeschaft voor de naamruimte. Als doorvoereenheden worden verhoogd of verlaagd, wordt Event Hubs Capture meters verhogen en verlagen om overeenkomende prestaties te bieden. De meters die zich voordoen in combinatie. Zie voor prijsgegevens [prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/). 

## <a name="integration-with-event-grid"></a>Integratie met Event Grid 
U kunt een Azure Event Grid-abonnement maken met een Event Hubs-naamruimte als de bron. De volgende zelfstudie leert u hoe u een Event Grid-abonnement maken met een event hub als een bron en een Azure Functions-app als een sink: [proces en de vastgelegde gegevens van Event Hubs migreren naar een SQL Data Warehouse met behulp van Event Grid en Azure Functions](store-captured-data-data-warehouse.md).


## <a name="next-steps"></a>Volgende stappen

Event Hubs Capture is de eenvoudigste manier om gegevens in Azure. Met Azure Data Lake, Azure Data Factory en Azure HDInsight, kunt u batchverwerking uitvoeren en andere analyses met vertrouwde hulpprogramma's en platforms van uw keuze, op elke schaal u nodig hebt.

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

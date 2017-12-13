---
title: 'Gegevensverbinding: gegevensstroom invoer van een stroom gebeurtenissen | Microsoft Docs'
description: Meer informatie over het instellen van een gegevensverbinding met Stream Analytics 'invoer' genoemd. Invoer bevatten een gegevensstroom van gebeurtenissen en ook verwijzen naar gegevens.
keywords: gegevensstroom, gegevensverbinding, stroom gebeurtenissen
services: stream-analytics
documentationcenter: 
author: SnehaGunda
manager: kfile
editor: cgronlun
ms.assetid: 8155823c-9dd8-4a6b-8393-34452d299b68
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 12/11/2017
ms.author: sngun
ms.openlocfilehash: e8b55269e861dc010c911491d52973b674dd50ca
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Gegevensverbinding: meer informatie over gegevens invoer van de stroom van gebeurtenissen voor Stream Analytics
De gegevensverbinding met een Stream Analytics-taak is een stream van gebeurtenissen van een gegevensbron waarnaar wordt verwezen als de taak *invoer*. Stream Analytics is een uitstekende integratie met Azure stream gegevensbronnen, waaronder [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/), en [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/). Deze invoerbronnen kunnen zijn in dezelfde Azure-abonnement als analytics-taak, of vanuit een ander abonnement.

## <a name="data-input-types-data-stream-and-reference-data"></a>Gegevens invoertypen: de stroom- en referentiegegevens samenvoegt gegevens
Als u gegevens naar een gegevensbron is gepusht, heeft gebruikt door de Stream Analytics-taak en verwerkt in realtime. Invoer worden onderverdeeld in twee typen: gegevens stream invoer en verwijzen naar gegevens invoer.

### <a name="data-stream-inputs"></a>Gegevensstroom invoer
Een gegevensstroom is een niet-gebonden reeks gebeurtenissen gedurende een bepaalde periode. Stream Analytics-taken moeten ten minste één gegevensstroominvoer bevatten. Event Hubs, IoT-Hub en Blob-opslag worden ondersteund als invoer gegevensbronnen stroom. Event hubs worden gebruikt voor het verzamelen van meerdere apparaten en services. Deze stromen kunnen sociale media activiteitsfeeds, gegevens over voorraad handel of gegevens van sensoren bevatten. IoT hubs zijn geoptimaliseerd voor het verzamelen van gegevens van verbonden apparaten in scenario's voor Internet der dingen (IoT).  BLOB-opslag kan worden gebruikt als een invoerbron voor het opnemen van grote hoeveelheden gegevens als een stream, zoals logboekbestanden.  

### <a name="reference-data"></a>Referentiegegevens
Stream Analytics ondersteunt ook bekend als invoer *referentiegegevens*. Dit zijn aanvullende gegevens die statische is of waarmee traag wordt gewijzigd. Dit wordt meestal gebruikt voor het uitvoeren van de correlatie en zoekacties. Bijvoorbeeld, u mogelijk koppelt gegevens in de invoer van de gegevensstroom tot gegevens in de referentiegegevens eruit als u een SQL-join statische waarden wilt uitvoeren. Azure Blob-opslag is momenteel de enige ondersteunde invoerbron voor referentiegegevens. Verwijzing data source blobs zijn beperkt tot 100 MB groot.

Zie voor meer informatie over het maken van verwijzing gegevens invoer, [gebruik referentiegegevens](stream-analytics-use-reference-data.md).  

## <a name="compression"></a>Compressie

Azure Stream Analytics ondersteunt compressie via alle stream invoer gegevensbronnen (Event Hubs, IoT-Hub en Blob-opslag). Deze functie voegt een nieuwe optie in de vervolgkeuzelijst aan de **nieuwe invoer** blade in Azure Portal, zodat u kunt eventueel ook gegevensstromen comprimeren. Op dit moment ondersteund verwijzing typen zijn - None, GZip, en Deflate-compressie. Ondersteuning voor compressie is niet beschikbaar voor referentiegegevens.

U hoeft niet te compressietype opgeven met de Avro-serialisatie. Als de invoergegevens Avro is gecomprimeerd, wordt dit transparant verwerkt. 

## <a name="create-data-stream-input-from-event-hubs"></a>Gegevensstroominvoer van Event Hubs maken

Azure Event Hubs is zeer schaalbaar gebeurtenis ingestors publiceren / abonneren. Een event hub kunt miljoenen gebeurtenissen per seconde verzamelen zodat u kunt verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en toepassingen. Event Hubs en Stream Analytics samen bieden u een end-to-end-oplossing voor realtime analyses: Event Hubs kunt u gebeurtenissen in Azure-kanaal in realtime en Stream Analytics-taken die gebeurtenissen in realtime kunnen verwerken. U kunt bijvoorbeeld web klikken, sensormetingen of online logboekgebeurtenissen verzenden naar Event Hubs. Vervolgens kunt u de Stream Analytics-taken voor het gebruik van Event Hubs als de invoergegevens stromen voor realtime filteren, aggregeren en correlatie maken.

De standaard tijdstempel van gebeurtenissen die afkomstig zijn van Event Hubs in Stream Analytics is de tijdstempel die de gebeurtenis ontvangen in de event hub, namelijk `EventEnqueuedUtcTime`. Voor het verwerken van de gegevens als een stream met een tijdstempel in het geval van nettolading, moet u de [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) sleutelwoord.

### <a name="consumer-groups"></a>Consumergroepen
Configureer elke Stream Analytics event hub-invoer hebben een eigen consumergroep. Wanneer een taak een self-join bevat of wanneer er meerdere invoer, kan sommige invoer worden gelezen door meer dan één lezer downstream. Deze situatie heeft gevolgen voor het aantal lezers in een enkel consumergroep. Om te voorkomen de Event Hubs-limiet van vijf lezers per klantengroep per partitie overschrijdt, wordt het aanbevolen om aan te wijzen een consumergroep voor elke Stream Analytics-taak. Er is een limiet van 20 consumergroepen per event hub. Zie voor meer informatie [Event Hubs-programmeergids](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-an-event-hub-as-a-data-stream-input"></a>Een event hub configureren als een gegevensstroom invoer
De volgende tabel bevat uitleg over elke eigenschap in de **nieuwe invoer** blade in de Azure portal als u een event hub als invoer configureert.

| Eigenschap | Beschrijving |
| --- | --- |
| **Invoeralias** |Een beschrijvende naam die u in de taak query gebruiken om te verwijzen naar deze gegevens. |
| **Service bus-naamruimte** |Een naamruimte Azure Service Bus is een container voor een set berichtentiteiten. Wanneer u een nieuwe event hub maakt, maakt u ook een Service Bus-naamruimte. |
| **Naam Event hub** |De naam van de event hub gebruiken als invoer. |
| **Naam Event hub-beleid** |Het beleid voor gedeelde toegang dat toegang tot de event hub biedt. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| **Event hub consumergroep** (optioneel) |De consumentengroep te gebruiken voor het opnemen van gegevens van de event hub. Als geen consumentengroepen is opgegeven, gebruikt de Stream Analytics-taak de standaardgroep voor consumenten. Het is raadzaam om gebruik te maken van een afzonderlijke klantengroep voor elke Stream Analytics-taak. |
| **Gebeurtenis serialisatie-indeling** |De serialisatie-indeling (JSON, CSV of Avro) van de inkomende gegevensstroom. |
| **Codering** | UTF-8 is momenteel de enige ondersteunde coderingsindeling. |
| **Compressie** (optioneel) | Het compressietype (geen, GZip en Deflate) van de gegevensstroom inkomende. |

Als de gegevens afkomstig van een event hub, hebt u toegang naar de volgende metagegevensvelden in de Stream Analytics-query:

| Eigenschap | Beschrijving |
| --- | --- |
| **EventProcessedUtcTime** |De datum en tijd waarop de gebeurtenis is verwerkt door de Stream Analytics. |
| **EventEnqueuedUtcTime** |De datum en tijd waarop de gebeurtenis is ontvangen door de Event Hubs. |
| **PartitionId** |De op nul gebaseerde partitie-ID voor de invoer-adapter. |

Bijvoorbeeld, kunt met behulp van deze velden, u een query op het volgende voorbeeld:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

> [!NOTE]
> Wanneer u Event Hub als een eindpunt voor IoT Hub Routes, u kunt toegang tot de IoT Hub medadata met behulp van de [functie GetMetadataPropertyValue](https://msdn.microsoft.com/en-us/library/azure/mt793845.aspx).
> 

## <a name="create-data-stream-input-from-iot-hub"></a>Gegevensstroominvoer uit IoT Hub maken
Azure Iot Hub is een uiterst schaalbare voor publiceren / abonneren event ingestor is geoptimaliseerd voor IoT-scenario's.

De standaard tijdstempel van gebeurtenissen die afkomstig zijn van een IoT-hub in Stream Analytics is de tijdstempel die de gebeurtenis ontvangen in de IoT-hub is `EventEnqueuedUtcTime`. Voor het verwerken van de gegevens als een stream met een tijdstempel in het geval van nettolading, moet u de [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) sleutelwoord.

> [!NOTE]
> Alleen berichten met een `DeviceClient` eigenschap kan worden verwerkt.
> 
> 

### <a name="consumer-groups"></a>Consumergroepen
Configureer elke Stream Analytics IoT-hub invoeren om de eigen consumergroep. Wanneer een taak een self-join bevat of wanneer er meerdere invoer, kan sommige invoer worden gelezen door meer dan één lezer downstream. Deze situatie heeft gevolgen voor het aantal lezers in een enkel consumergroep. Om te voorkomen van meer dan de Azure IoT Hub-limiet van vijf lezers per klantengroep per partitie, maar het is een best practice om aan te wijzen een consumergroep voor elke Stream Analytics-taak.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Een IoT-hub configureren als een gegevensstroom invoer
De volgende tabel bevat uitleg over elke eigenschap in de **nieuwe invoer** blade in de Azure portal als u een IoT-hub als invoer configureert.

| Eigenschap | Beschrijving |
| --- | --- |
| **Invoeralias** |Een beschrijvende naam die u in de taak query gebruiken om te verwijzen naar deze gegevens.|
| **IoT-hub** |De naam van de IoT-hub te gebruiken als invoer. |
| **Eindpunt** |Het eindpunt voor de IoT-hub.|
| **Naam van beleid voor gedeelde toegang** |Het beleid voor gedeelde toegang dat toegang tot de IoT-hub biedt. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| **Beleid voor gedeelde toegangssleutel** |De gedeelde toegangssleutel toegang verlenen aan de IoT-hub. |
| **Consumergroep** (optioneel) |De consumentengroep te gebruiken voor het opnemen van gegevens van de IoT-hub. Als geen consumentengroepen is opgegeven, een Stream Analytics-taak maakt gebruik van de standaardgroep voor consumenten. Het is raadzaam om gebruik te maken van een andere klantengroep voor elke Stream Analytics-taak. |
| **Gebeurtenis serialisatie-indeling** |De serialisatie-indeling (JSON, CSV of Avro) van de inkomende gegevensstroom. |
| **Codering** |UTF-8 is momenteel de enige ondersteunde coderingsindeling. |
| **Compressie** (optioneel) | Het compressietype (geen, GZip en Deflate) van de gegevensstroom inkomende. |

Als de gegevens afkomstig van een iothub, hebt u toegang naar de volgende metagegevensvelden in de Stream Analytics-query:

| Eigenschap | Beschrijving |
| --- | --- |
| **EventProcessedUtcTime** |De datum en tijd waarop de gebeurtenis is verwerkt. |
| **EventEnqueuedUtcTime** |De datum en tijd waarop de gebeurtenis is ontvangen door de IoT-hub. |
| **PartitionId** |De op nul gebaseerde partitie-ID voor de invoer-adapter. |
| **IoTHub.MessageId** | Een ID die wordt gebruikt voor communicatie in twee richtingen in IoT-hub correleren. |
| **IoTHub.CorrelationId** |Een ID die wordt gebruikt voor antwoorden op berichten en feedback van IoT-hub. |
| **IoTHub.ConnectionDeviceId** |De verificatie-ID gebruikt om dit bericht te verzenden. Deze waarde wordt vermeld op servicebound berichten door de IoT-hub. |
| **IoTHub.ConnectionDeviceGenerationId** |De generatie-ID van het geverifieerde apparaat dat is gebruikt voor dit bericht verzendt. Deze waarde wordt vermeld op servicebound berichten door de IoT-hub. |
| **IoTHub.EnqueuedTime** |De tijd waarop het bericht is ontvangen door de IoT-hub. |
| **IoTHub.StreamId** |Een aangepaste gebeurteniseigenschap toegevoegd door het apparaat van de afzender. |


## <a name="create-data-stream-input-from-blob-storage"></a>Gegevensstroominvoer van Blob-opslag maken
Azure Blob-opslag biedt een rendabele en schaalbare oplossing voor scenario's met grote hoeveelheden ongestructureerde gegevens op te slaan in de cloud. Gegevens in Blob storage is meestal beschouwd als gegevens in rust. Maar kan deze worden verwerkt als een gegevensstroom door Stream Analytics. Een typisch scenario voor Blob storage-ingangen met Stream Analytics is de verwerking van logboekbestanden. In dit scenario worden telemetriegegevens van een systeem is opgenomen en moet worden geparseerd en om uit te pakken betekenisvolle gegevens verwerkt.

De standaard tijdstempel van Blob storage-gebeurtenissen in de Stream Analytics is het dat de blob het laatst is gewijzigd tijdstempel is `BlobLastModifiedUtcTime`. Voor het verwerken van de gegevens als een stream met een tijdstempel in het geval van nettolading, moet u de [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) sleutelwoord.

CSV-indeling invoer *vereisen* koprij en geef de velden voor de gegevensset. Alle koptekstvelden rij moeten bovendien uniek zijn.

> [!NOTE]
> Stream Analytics ondersteunt geen inhoud toevoegen aan een bestaand blob-bestand. Stream Analytics wordt elk bestand slechts eenmaal weergeven en wijzigingen die in het bestand moet worden uitgevoerd nadat de taak van de gegevens lezen niet zijn verwerkt. Beste manier is om alle gegevens voor een blob-bestand tegelijk uploaden en vervolgens aanvullende nieuwere gebeurtenissen toe te voegen aan een andere, nieuwe blob-bestand.
> 

### <a name="configure-blob-storage-as-a-data-stream-input"></a>Blob-opslag configureren als een gegevensstroom invoer

De volgende tabel bevat uitleg over elke eigenschap in de **nieuwe invoer** blade in de Azure portal als u een Blob-opslag als invoer configureert.

| Eigenschap | Beschrijving |
| --- | --- |
| **Invoeralias** | Een beschrijvende naam die u in de taak query gebruiken om te verwijzen naar deze gegevens. |
| **Opslagaccount** | De naam van het opslagaccount waarin de blob-bestanden bevinden. |
| **De sleutel van opslagaccount** | De geheime sleutel die is gekoppeld aan het opslagaccount. |
| **Container** | De container voor de blob invoer. Containers bieden een logische groepering van blobs die zijn opgeslagen in de Microsoft Azure Blob-service. Wanneer u een blob naar de Azure Blob storage-service uploaden, moet u een container voor blob opgeven. |
| **Het pad** (optioneel) | Het pad dat wordt gebruikt om de blobs in de opgegeven container te vinden. Binnen het pad kunt u een of meer exemplaren van de volgende drie variabelen opgeven: `{date}`, `{time}`, of`{partition}`<br/><br/>Voorbeeld 1:`cluster1/logs/{date}/{time}/{partition}`<br/><br/>Voorbeeld 2:`cluster1/logs/{date}`<br/><br/>De `*` teken is niet-toegestane waarde voor het voorvoegsel op dat pad. Alleen geldige <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob-tekens</a> zijn toegestaan. |
| **De datumnotatie** (optioneel) | Als u de datum-variabele in het pad voor de datumnotatie waarin de bestanden zijn ingedeeld gebruiken. Voorbeeld:`YYYY/MM/DD` |
| **Tijdnotatie** (optioneel) |  Als u de time-variabele in het pad voor de indeling waarin de bestanden zijn ingedeeld gebruiken. De enige ondersteunde waarde is momenteel `HH`. |
| **Gebeurtenis serialisatie-indeling** | De serialisatie-indeling (JSON, CSV of Avro) voor inkomende gegevensstromen. |
| **Codering** | Voor de CSV en JSON is UTF-8 momenteel de enige ondersteunde coderingsindeling. |
| **Compressie** (optioneel) | Het compressietype (geen, GZip en Deflate) van de gegevensstroom inkomende. |

Als de gegevens afkomstig van een Blob storage-bron, hebt u toegang naar de volgende metagegevensvelden in de Stream Analytics-query:

| Eigenschap | Beschrijving |
| --- | --- |
| **BlobName** |De naam van de blob-invoerbron die afkomstig zijn van de gebeurtenis. |
| **EventProcessedUtcTime** |De datum en tijd waarop de gebeurtenis is verwerkt door de Stream Analytics. |
| **BlobLastModifiedUtcTime** |De datum en tijd waarop de blob voor het laatst is gewijzigd. |
| **PartitionId** |De op nul gebaseerde partitie-ID voor de invoer-adapter. |

Bijvoorbeeld, kunt met behulp van deze velden, u een query op het volgende voorbeeld:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
U hebt geleerd over opties voor gegevensverbinding in Azure voor uw Stream Analytics-taken. Voor meer informatie over Stream Analytics, Zie:

* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

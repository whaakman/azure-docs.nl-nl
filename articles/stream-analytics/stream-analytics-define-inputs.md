---
title: Van stroomgegevens als invoer in Azure Stream Analytics
description: Meer informatie over het instellen van een verbinding van de gegevens in Azure Stream Analytics. Invoer bevatten een gegevensstroom van gebeurtenissen en ook verwijzen naar gegevens.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: 1fc1791d75355cc30f2ef43fc17e39a868e2c756
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="stream-data-as-input-into-stream-analytics"></a>Van stroomgegevens als invoer Stream Analytics

Stream Analytics heeft klas integratie met Azure-gegevensstromen als invoer van drie soorten resources:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) 

Deze invoer resources kunnen bevinden zich in dezelfde Azure-abonnement als uw Stream Analytics-taak of vanuit een ander abonnement.

### <a name="compression"></a>Compressie
Stream Analytics ondersteunt compressie via alle stream invoer gegevensbronnen. Ondersteunde verwijzingstypen zijn: None, GZip, en Deflate-compressie. Ondersteuning voor compressie is niet beschikbaar voor referentiegegevens. Als de invoerindeling voor de Avro-gegevens die zijn gecomprimeerd, wordt dit transparant verwerkt. U hoeft niet te compressietype opgeven met de Avro-serialisatie. 

## <a name="create-or-edit-inputs"></a>Maken of bewerken van invoer
Voor het maken van nieuwe invoer en weergeven of bewerken van bestaande invoer op uw streaming-taak, kunt u de Azure-portal:
1. Open de [Azure-portal](https://portal.azure.com) en selecteer uw Stream Analytics-taak.
2. Selecteer de **invoer** optie onder de **instellingen** kop. 
4. De **invoer** pagina worden alle bestaande invoer. 
5. Op de **invoer** pagina **Stroominvoer toevoegen** of **verwijzing invoer toevoegen** om de detailpagina te openen.
6. Selecteer een bestaande invoer om te bewerken van de details en selecteer **opslaan** bewerken van een bestaande invoer.
7. Selecteer **Test** op de pagina ingevoerde gegevens om te verifiëren dat de verbindingsopties geldig en actief zijn. 
8. Met de rechtermuisknop op de naam van een bestaande invoer en selecteer **voorbeeldgegevens uit invoer** nodig zijn voor verder onderzoek.


## <a name="stream-data-from-event-hubs"></a>Van stroomgegevens uit Event Hubs

Azure Event Hubs is zeer schaalbaar gebeurtenis ingestors publiceren / abonneren. Een event hub kunt miljoenen gebeurtenissen per seconde verzamelen zodat u kunt verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en toepassingen. Event Hubs en Stream Analytics bieden samen u een end-to-end-oplossing voor realtime analyses. Event Hubs kunnen u gebeurtenissen in Azure-kanaal in realtime en Stream Analytics-taken die gebeurtenissen in realtime kunnen verwerken. U kunt bijvoorbeeld web klikken, sensormetingen of online logboekgebeurtenissen verzenden naar Event Hubs. Vervolgens kunt u de Stream Analytics-taken voor het gebruik van Event Hubs als de invoergegevens stromen voor realtime filteren, aggregeren en correlatie maken.

De standaard tijdstempel van gebeurtenissen die afkomstig zijn van Event Hubs in Stream Analytics is de tijdstempel die de gebeurtenis ontvangen in de event hub, namelijk `EventEnqueuedUtcTime`. Voor het verwerken van de gegevens als een stream met een tijdstempel in het geval van nettolading, moet u de [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) sleutelwoord.

### <a name="consumer-groups"></a>Consumergroepen
Configureer elke Stream Analytics event hub-invoer hebben een eigen consumergroep. Wanneer een taak een self-join bevat of wanneer er meerdere invoer, kan sommige invoer worden gelezen door meer dan één lezer downstream. Deze situatie heeft gevolgen voor het aantal lezers in een enkel consumergroep. Om te voorkomen de Event Hubs-limiet van vijf lezers per klantengroep per partitie overschrijdt, wordt het aanbevolen om aan te wijzen een consumergroep voor elke Stream Analytics-taak. Er is een limiet van 20 consumergroepen per event hub. Zie voor meer informatie [Event Hubs-programmeergids](../event-hubs/event-hubs-programming-guide.md).

### <a name="stream-data-from-event-hubs"></a>Van stroomgegevens uit Event Hubs
De volgende tabel bevat uitleg over elke eigenschap in de **nieuwe invoer** pagina in de Azure-portal op invoer van de gegevensstroom van een Event Hub:

| Eigenschap | Beschrijving |
| --- | --- |
| **Invoeralias** |Een beschrijvende naam die u in de taak query gebruiken om te verwijzen naar deze gegevens. |
| **Abonnement** | Kies het abonnement waarin de Event hub-bron bestaat. | 
| **Event Hub-naamruimte** | De Event Hub-naamruimte is een container voor een set met berichtentiteiten. Wanneer u een nieuwe event hub maakt, maakt u ook de naamruimte. |
| **Naam Event Hub** | De naam van de event hub gebruiken als invoer. |
| **Naam Event Hub-beleid** | Het beleid voor gedeelde toegang dat toegang tot de Event Hub biedt. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Deze optie wordt automatisch ingevuld tenzij u de optie voor het bieden van de Event Hub-instellingen handmatig.|
| **Event Hub consumergroep** (aanbevolen) | Het is raadzaam een afzonderlijke consumergroep gebruiken voor elke Stream Analytics-taak. Deze tekenreeks identificeert de consumergroep te gebruiken voor het opnemen van gegevens van de event hub. Als geen consumentengroepen is opgegeven, maakt de consumergroep $Default gebruik van de Stream Analytics-taak.  |
| **Gebeurtenis serialisatie-indeling** | De serialisatie-indeling (JSON, CSV of Avro) van de inkomende gegevensstroom.  Zorg ervoor dat de JSON-indeling wordt uitgelijnd met de specificatie en bevat geen voorloop 0 voor decimale getallen. |
| **Codering** | UTF-8 is momenteel de enige ondersteunde coderingsindeling. |
| **Compressie gebeurtenistype** | Het compressietype is gebruikt om te lezen van de inkomende gegevensstroom zoals geen (standaard), GZip en Deflate. |

Als de gegevens afkomstig van een invoer van de stroom Event Hub, hebt u toegang naar de volgende metagegevensvelden in de Stream Analytics-query:

| Eigenschap | Beschrijving |
| --- | --- |
| **EventProcessedUtcTime** |De datum en tijd waarop de gebeurtenis is verwerkt door de Stream Analytics. |
| **EventEnqueuedUtcTime** |De datum en tijd waarop de gebeurtenis is ontvangen door de Event Hubs. |
| **partitionId** |De op nul gebaseerde partitie-ID voor de invoer-adapter. |

Bijvoorbeeld, kunt met behulp van deze velden, u een query op het volgende voorbeeld:

```sql
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
```

> [!NOTE]
> Wanneer u Event Hub als een eindpunt voor IoT Hub Routes, u kunt toegang tot de IoT Hub medadata met behulp van de [functie GetMetadataPropertyValue](https://msdn.microsoft.com/library/azure/mt793845.aspx).
> 

## <a name="stream-data-from-iot-hub"></a>Van stroomgegevens uit IoT Hub
Azure Iot Hub is een uiterst schaalbare voor publiceren / abonneren event ingestor is geoptimaliseerd voor IoT-scenario's.

De standaard tijdstempel van gebeurtenissen die afkomstig zijn van een IoT-Hub in Stream Analytics is de tijdstempel die de gebeurtenis ontvangen in de IoT-Hub is `EventEnqueuedUtcTime`. Voor het verwerken van de gegevens als een stream met een tijdstempel in het geval van nettolading, moet u de [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) sleutelwoord.

> [!NOTE]
> Alleen berichten met een `DeviceClient` eigenschap kan worden verwerkt.
> 

### <a name="consumer-groups"></a>Consumergroepen
Configureer elke Stream Analytics IoT Hub-invoer hebben een eigen consumergroep. Wanneer een taak een self-join bevat of wanneer er meerdere invoer, kan sommige invoer worden gelezen door meer dan één lezer downstream. Deze situatie heeft gevolgen voor het aantal lezers in een enkel consumergroep. Om te voorkomen van meer dan de Azure IoT Hub-limiet van vijf lezers per klantengroep per partitie, maar het is een best practice om aan te wijzen een consumergroep voor elke Stream Analytics-taak.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Een IoT-Hub configureren als een gegevensstroom invoer
De volgende tabel bevat uitleg over elke eigenschap in de **nieuwe invoer** pagina in de Azure portal als u een IoT-Hub als een stroom die invoer configureert.

| Eigenschap | Beschrijving |
| --- | --- |
| **Invoeralias** | Een beschrijvende naam die u in de taak query gebruiken om te verwijzen naar deze gegevens.|
| **Abonnement** | Kies het abonnement waarin de IoT Hub-bron bestaat. | 
| **IoT Hub** | De naam van de IoT-Hub te gebruiken als invoer. |
| **Endpoint** | Het eindpunt voor de IoT-Hub.|
| **Naam van beleid voor gedeelde toegang** | Het beleid voor gedeelde toegang dat toegang tot de IoT-Hub biedt. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| **Beleid voor gedeelde toegangssleutel** | De gedeelde toegangssleutel toegang verlenen aan de IoT-Hub.  Deze optie wordt automatisch ingevuld tenzij u de optie de Iot-Hub-instellingen opgeven handmatig. |
| **Klantengroep** | Het is raadzaam dat u een andere consumergroep voor elke Stream Analytics-taak gebruiken. De consumergroep wordt gebruikt voor het opnemen van gegevens van de IoT-Hub. Stream Analytics maakt gebruik van de consumergroep $Default tenzij u iets anders opgeeft.  |
| **Gebeurtenis serialisatie-indeling** | De serialisatie-indeling (JSON, CSV of Avro) van de inkomende gegevensstroom.  Zorg ervoor dat de JSON-indeling wordt uitgelijnd met de specificatie en bevat geen voorloop 0 voor decimale getallen. |
| **Codering** | UTF-8 is momenteel de enige ondersteunde coderingsindeling. |
| **Compressie gebeurtenistype** | Het compressietype is gebruikt om te lezen van de inkomende gegevensstroom zoals geen (standaard), GZip en Deflate. |


Wanneer u stroomgegevens van een IoT-Hub, hebt u toegang naar de volgende metagegevensvelden in de Stream Analytics-query:

| Eigenschap | Beschrijving |
| --- | --- |
| **EventProcessedUtcTime** | De datum en tijd waarop de gebeurtenis is verwerkt. |
| **EventEnqueuedUtcTime** | De datum en tijd waarop de gebeurtenis is ontvangen door de IoT-Hub. |
| **partitionId** | De op nul gebaseerde partitie-ID voor de invoer-adapter. |
| **IoTHub.MessageId** | Een ID die wordt gebruikt voor communicatie in twee richtingen in IoT-Hub correleren. |
| **IoTHub.CorrelationId** | Een ID die wordt gebruikt voor antwoorden op berichten en feedback van IoT-Hub. |
| **IoTHub.ConnectionDeviceId** | De verificatie-ID gebruikt om dit bericht te verzenden. Deze waarde wordt vermeld op servicebound berichten door de IoT-Hub. |
| **IoTHub.ConnectionDeviceGenerationId** | De generatie-ID van het geverifieerde apparaat dat is gebruikt voor dit bericht verzendt. Deze waarde wordt vermeld op servicebound berichten door de IoT-Hub. |
| **IoTHub.EnqueuedTime** | De tijd waarop het bericht is ontvangen door de IoT-Hub. |
| **IoTHub.StreamId** | Een aangepaste gebeurteniseigenschap toegevoegd door het apparaat van de afzender. |


## <a name="stream-data-from-blob-storage"></a>Van stroomgegevens uit Blob storage
Azure Blob-opslag biedt een rendabele en schaalbare oplossing voor scenario's met grote hoeveelheden ongestructureerde gegevens op te slaan in de cloud. Gegevens in Blob storage is meestal beschouwd als gegevens in rust. Blob-gegevens kunnen echter worden verwerkt als een gegevensstroom door Stream Analytics. 

De verwerking van logboekbestanden is een veelgebruikte scenario voor het gebruik van Blob storage invoer met Stream Analytics. In dit scenario wordt telemetrie-gegevensbestanden zijn vastgelegd van een systeem en moet worden geparseerd en om uit te pakken betekenisvolle gegevens verwerkt.

De standaard tijdstempel van Blob storage-gebeurtenissen in de Stream Analytics is het dat de blob het laatst is gewijzigd tijdstempel is `BlobLastModifiedUtcTime`. Voor het verwerken van de gegevens als een stream met een tijdstempel in het geval van nettolading, moet u de [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) sleutelwoord.

CSV-indeling invoer *vereisen* koprij voor het definiëren van velden voor de gegevensset en alle koptekstvelden rij moet uniek zijn.

Stream Analytics ondersteunt momenteel niet deserialiseren AVRO die zijn gegenereerd door de Event Hub vastleggen of aangepaste eindpunt IoT Hub Azure Storage-Container.

> [!NOTE]
> Stream Analytics ondersteunt geen inhoud toevoegen aan een bestaand blob-bestand. Stream Analytics wordt elk bestand slechts eenmaal weergeven en wijzigingen die in het bestand moet worden uitgevoerd nadat de taak van de gegevens lezen niet zijn verwerkt. Beste manier is om alle gegevens voor een blob-bestand tegelijk uploaden en vervolgens aanvullende nieuwere gebeurtenissen toe te voegen aan een andere, nieuwe blob-bestand.
> 

### <a name="configure-blob-storage-as-a-stream-input"></a>Blob-opslag configureren als een stroom die invoer 

De volgende tabel bevat uitleg over elke eigenschap in de **nieuwe invoer** pagina in de Azure portal wanneer u een Blob-opslag als een stroom die invoer configureert.

| Eigenschap | Beschrijving |
| --- | --- |
| **Invoeralias** | Een beschrijvende naam die u in de taak query gebruiken om te verwijzen naar deze gegevens. |
| **Abonnement** | Kies het abonnement waarin de IoT Hub-bron bestaat. | 
| **Opslagaccount** | De naam van het opslagaccount waarin de blob-bestanden bevinden. |
| **De sleutel van opslagaccount** | De geheime sleutel die is gekoppeld aan het opslagaccount. Deze optie wordt automatisch ingevuld tenzij u de optie voor het bieden van de Blob storage-instellingen handmatig. |
| **Container** | De container voor de blob invoer. Containers bieden een logische groepering van blobs die zijn opgeslagen in de Microsoft Azure Blob-service. Wanneer u een blob naar de Azure Blob storage-service uploaden, moet u een container voor blob opgeven. U kunt een **gebruik bestaande** container of **nieuw** hebben een nieuwe container gemaakt.|
| **Het pad** (optioneel) | Het pad dat wordt gebruikt om de blobs in de opgegeven container te vinden. Binnen het pad kunt u een of meer exemplaren van de volgende drie variabelen opgeven: `{date}`, `{time}`, of `{partition}`<br/><br/>Voorbeeld 1: `cluster1/logs/{date}/{time}/{partition}`<br/><br/>Voorbeeld 2: `cluster1/logs/{date}`<br/><br/>De `*` teken is niet-toegestane waarde voor het voorvoegsel op dat pad. Alleen geldige <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob-tekens</a> zijn toegestaan. |
| **De datumnotatie** (optioneel) | Als u de datum-variabele in het pad voor de datumnotatie waarin de bestanden zijn ingedeeld gebruiken. Voorbeeld: `YYYY/MM/DD` |
| **Tijdnotatie** (optioneel) |  Als u de time-variabele in het pad voor de indeling waarin de bestanden zijn ingedeeld gebruiken. De enige ondersteunde waarde is momenteel `HH` uur. |
| **Gebeurtenis serialisatie-indeling** | De serialisatie-indeling (JSON, CSV of Avro) van de inkomende gegevensstroom.  Zorg ervoor dat de JSON-indeling wordt uitgelijnd met de specificatie en bevat geen voorloop 0 voor decimale getallen. |
| **Codering** | Voor de CSV en JSON is UTF-8 momenteel de enige ondersteunde coderingsindeling. |
| **Compressie** | Het compressietype is gebruikt om te lezen van de inkomende gegevensstroom zoals geen (standaard), GZip en Deflate. |

Als de gegevens afkomstig van een Blob storage-bron, hebt u toegang naar de volgende metagegevensvelden in de Stream Analytics-query:

| Eigenschap | Beschrijving |
| --- | --- |
| **BlobName** |De naam van de blob-invoerbron die afkomstig zijn van de gebeurtenis. |
| **EventProcessedUtcTime** |De datum en tijd waarop de gebeurtenis is verwerkt door de Stream Analytics. |
| **BlobLastModifiedUtcTime** |De datum en tijd waarop de blob voor het laatst is gewijzigd. |
| **partitionId** |De op nul gebaseerde partitie-ID voor de invoer-adapter. |

Bijvoorbeeld, kunt met behulp van deze velden, u een query op het volgende voorbeeld:

```sql
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Snelstartgids: Een Stream Analytics-taak maken met behulp van de Azure-portal](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

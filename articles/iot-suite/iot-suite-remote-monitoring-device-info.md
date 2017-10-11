---
title: Metagegevens van apparaten informatie in de oplossing voor externe controle | Microsoft Docs
description: Een beschrijving van de vooraf geconfigureerde oplossing voor externe controle IoT Azure en de bijbehorende architectuur.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1b334769-103b-4eb0-a293-184f3d1ba9a3
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: f8fd452806a0a0b98cf8e434c9bd55700083a6c5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Metagegevens van apparaten informatie in de vooraf geconfigureerde oplossing voor externe controle

De Azure IoT Suite vooraf geconfigureerde oplossing voor externe controle demonstreert een benadering voor het beheren van metagegevens van apparaten. In dit artikel bevat een overzicht van de aanpak van die deze oplossing nodig is om te begrijpen:

* De metagegevens van welke apparaten de oplossing worden opgeslagen.
* Hoe de oplossing de metagegevens van de apparaten beheert.

## <a name="context"></a>Context

Externe controle vooraf geconfigureerde oplossing maakt gebruik van [Azure IoT Hub] [ lnk-iot-hub] waarmee uw apparaten gegevens verzenden naar de cloud. De oplossing bevat gegevens over de apparaten in drie verschillende locaties:

| Locatie | Gegevens die zijn opgeslagen | Implementatie |
| -------- | ------------------ | -------------- |
| ID-register | Apparaat-id, verificatiesleutels, de status ingeschakeld | Ingebouwd in IoT-Hub |
| Apparaat horende | Metagegevens: gemelde eigenschappen, gewenste eigenschappen, tags | Ingebouwd in IoT-Hub |
| Cosmos DB | Geschiedenis van de opdracht en methode | Aangepaste voor oplossing |

IoT-Hub bevat een [register voor apparaat-id's] [ lnk-identity-registry] voor het beheren van toegang tot een IoT-hub en maakt gebruik van [apparaat horende] [ lnk-device-twin] voor het beheren van metagegevens van apparaten. Er is ook een externe controle oplossings-specifieke *apparaatregister* die worden opgeslagen geschiedenis van de opdracht en de methode. Maakt gebruik van de oplossing voor externe controle een [Cosmos DB] [ lnk-docdb] database voor het implementeren van een aangepast archief voor de opdracht en methode geschiedenis.

> [!NOTE]
> De vooraf geconfigureerde oplossing voor externe controle houdt het register voor apparaat-id's gesynchroniseerd met de informatie in de database van de Cosmos-database. Beide dezelfde apparaat-id gebruiken voor het aanduiden van elk apparaat dat is verbonden met uw IoT-hub.

## <a name="device-metadata"></a>Metagegevens van apparaten

IoT Hub onderhoudt een [apparaat twin] [ lnk-device-twin] voor elke gesimuleerde en het fysieke apparaat is verbonden met een oplossing voor externe controle. De oplossing maakt gebruik van apparaat horende voor het beheren van de metagegevens gekoppeld aan apparaten. Een apparaat-twin is een JSON-document bijgehouden door de IoT Hub en de oplossing maakt gebruik van de IoT Hub-API om te communiceren met horende apparaten.

Een apparaat-twin drie soorten metagegevens worden opgeslagen:

- *Eigenschappen gerapporteerd* naar een IoT-hub worden verzonden door een apparaat. In de oplossing voor externe controle gesimuleerde apparaten verzenden gemelde eigenschappen bij het opstarten en in reactie op **Changedevicestate** opdrachten en -methoden. U kunt weergeven gemelde eigenschappen in de **lijst met apparaten** en **details apparaat** in de portal van de oplossing. Gerapporteerde eigenschappen zijn alleen-lezen.
- *Eigenschappen gewenst* door apparaten worden opgehaald uit de IoT-hub. Het is de verantwoordelijkheid van het apparaat nodig configuratiewijziging op het apparaat. Het is ook de verantwoordelijkheid van het apparaat voor het rapporteren van de wijziging terug naar de hub als een eigenschap gemeld. U kunt een waarde van de gewenste eigenschap via de portal van de oplossing instellen.
- *Labels* bestaan alleen in de apparaat-twin en zijn nooit gesynchroniseerd met een apparaat. U kunt in de oplossingsportal labelwaarden en ze gebruiken bij het filteren van de lijst met apparaten. De oplossing gebruikt ook een code voor het identificeren van het pictogram moet worden weergegeven voor een apparaat in de portal van de oplossing.

Voorbeeld gerapporteerd eigenschappen van de gesimuleerde apparaten zijn onder meer de fabrikant, modelnummer breedtegraad en lengtegraad. De lijst met ondersteunde methodes gesimuleerde apparaten ook geretourneerd als een eigenschap gemeld.

> [!NOTE]
> De code van het gesimuleerde apparaat gebruikt alleen de gewenste eigenschappen **Desired.Config.TemperatureMeanValue** en **Desired.Config.TelemetryInterval** om de gerapporteerde eigenschappen bij te werken die naar IoT Hub worden teruggestuurd. Alle andere gewenste eigenschap wijzigingsaanvragen worden genegeerd.

Een apparaat informatie metagegevens JSON-document opgeslagen in de database apparaat register Cosmos DB heeft de volgende structuur:

```json
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

> [!NOTE]
> Gegevens van een apparaat kan ook betekenen metagegevens om te beschrijven de telemetrie die het apparaat naar IoT Hub verzendt. De oplossing voor externe controle gebruikt deze metagegevens telemetrie naar de weergave van het dashboard [dynamische telemetrie][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Levenscyclus

Wanneer u eerst een apparaat in de portal van de oplossing maakt, maakt de oplossing een vermelding in de database van de Cosmos-database voor het opslaan van de geschiedenis van de opdracht en de methode. De oplossing maakt op dit moment ook een vermelding voor het apparaat in het identiteitenregister van apparaten, waardoor de sleutels die het apparaat gebruikt om te verifiëren met IoT Hub worden gegenereerd. Dit leidt ook tot een apparaat-twin.

Wanneer een apparaat is eerst verbinding met de oplossing maakt, stuurt het gerapporteerde eigenschappen en een informatiebericht van het apparaat. De gerapporteerde eigenschapswaarden worden automatisch opgeslagen in de apparaat-twin. De gerapporteerde eigenschappen zijn de fabrikant, modelnummer, serienummer en een lijst van ondersteunde methoden. Het apparaat informatie-bericht bevat de lijst met het apparaat inclusief informatie over alle parameters van de ondersteunt opdrachten. Wanneer de oplossing voor dit bericht ontvangt, wordt de informatie van de apparaten in de database van de Cosmos-database bijgewerkt.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Apparaat-informatie in de oplossingsportal weergeven en bewerken

De lijst met apparaten in de oplossingsportal standaard als kolommen in de volgende eigenschappen van de apparaten weergegeven: **Status**, **DeviceId**, **fabrikant**, **modelnummer**, **serienummer**, **Firmware**, **Platform**, **Processor**, en **geïnstalleerd RAM-geheugen**. U kunt de kolommen aanpassen door te klikken op **kolom editor**. Eigenschappen voor het apparaat **breedtegraad** en **lengtegraad** station van de locatie in de Bing-kaart op het dashboard.

![Editor voor kolom in de lijst met apparaten][img-device-list]

In de **Apparaatdetails** deelvenster in de oplossingsportal kunt u de eigenschappen van de gewenste en labels bewerken (gerapporteerd eigenschappen zijn alleen-lezen).

![Deelvenster Apparaatdetails][img-device-edit]

De oplossingsportal kunt u een apparaat verwijdert uit uw oplossing. Wanneer u een apparaat verwijdert, wordt de oplossing voor de apparaatvermelding verwijdert uit het identiteitenregister en verwijdert vervolgens de apparaat-twin. De oplossing ook verwijderd informatie met betrekking tot het apparaat uit de database van de Cosmos-database. Voordat u een apparaat verwijdert kunt, moet u deze uitschakelen.

![Apparaat verwijderen][img-device-remove]

## <a name="device-information-message-processing"></a>Apparaat informatie berichtverwerking

Berichten met apparaatgegevens verzonden door een apparaat zijn telemetrieberichten. Berichten met apparaatgegevens omvatten de opdrachten die een apparaat kan reageren, en eventuele opdrachtgeschiedenis. IoT-Hub zelf heeft geen informatie over de metagegevens in het informatiebericht van een apparaat en het bericht verwerkt op dezelfde manier die alle apparaat-naar-cloud-berichten worden verwerkt. In de oplossing voor externe controle een [Azure Stream Analytics] [ lnk-stream-analytics] (ASA)-taak de berichten uit IoT Hub kan lezen. De **DeviceInfo** stream analytics-taak filters voor berichten met **'ObjectType': 'DeviceInfo'** en stuurt ze de **EventProcessorHost** hostexemplaar dat wordt uitgevoerd in een web-taak. Logica in de **EventProcessorHost** exemplaar gebruikt de apparaat-id voor de Cosmos-DB-record vinden voor het specifieke apparaat en update voor de record.

> [!NOTE]
> Een informatiebericht van het apparaat is een standaard apparaat-naar-cloud-bericht. De oplossing wordt onderscheid gemaakt tussen berichten met apparaatgegevens en telemetrieberichten met behulp van de ASA-query's.

## <a name="next-steps"></a>Volgende stappen

Nu u klaar bent met het leren hoe u de vooraf geconfigureerde oplossingen kunt aanpassen, vindt u enkele van de andere functies en mogelijkheden van de vooraf geconfigureerde IoT Suite-oplossingen:

* [Overzicht van voorspeld onderhoud vooraf geconfigureerde oplossing][lnk-predictive-overview]
* [Veelgestelde vragen over IoT Suite][lnk-faq]
* [Fundamentele IoT-beveiliging][lnk-security-groundup]

<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md

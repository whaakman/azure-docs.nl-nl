---
title: Metagegevens van apparaten informatie in de oplossing voor externe controle | Microsoft Docs
description: Een beschrijving van de vooraf geconfigureerde oplossing voor externe controle IoT Azure en de bijbehorende architectuur.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 1b334769-103b-4eb0-a293-184f3d1ba9a3
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 4efea316c05f566add3e175bc5bb18842225ede3
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35758158"
---
# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Metagegevens van apparaten informatie in de vooraf geconfigureerde oplossing voor externe controle

De Azure IoT Suite vooraf geconfigureerde oplossing voor externe controle ziet u een benadering voor het beheren van metagegevens van apparaten. In dit artikel bevat een overzicht van de aanpak van die deze oplossing nodig waarmee u kunt om te begrijpen:

* De metagegevens van welke apparaten de oplossing worden opgeslagen.
* De oplossing beheert hoe de metagegevens van apparaten.

## <a name="context"></a>Context

De externe controle vooraf geconfigureerde oplossing maakt gebruik van [Azure IoT Hub] [ lnk-iot-hub] om in te schakelen van uw apparaten om gegevens te verzenden naar de cloud. De informatie over apparaten worden opgeslagen in drie verschillende locaties:

| Locatie | Gegevens die zijn opgeslagen | Implementatie |
| -------- | ------------------ | -------------- |
| ID-register | Apparaat-id, verificatiesleutels, ingeschakelde status | Ingebouwd in IoT Hub |
| Apparaatdubbels | Metagegevens: gerapporteerde eigenschappen, gewenste eigenschappen, tags | Ingebouwd in IoT Hub |
| Cosmos DB | Geschiedenis van de opdracht en de methode | Aangepaste voor oplossing |

IoT Hub bevat een [apparaatidentiteitsregister] [ lnk-identity-registry] voor het beheren van toegang tot een IoT-hub en maakt gebruik van [apparaatdubbels] [ lnk-device-twin] voor het beheren van metagegevens van apparaten. Er is ook een externe bewaking oplossingsspecifieke *apparaatregister* die worden opgeslagen geschiedenis van de opdracht en de methode. Maakt gebruik van de oplossing voor externe controle een [Cosmos DB] [ lnk-docdb] database voor het implementeren van een aangepast archief voor de opdracht en de methode geschiedenis.

> [!NOTE]
> De vooraf geconfigureerde oplossing voor externe controle houdt u het register voor apparaat-id's gesynchroniseerd met de informatie in de Cosmos DB-database. Beide dezelfde apparaat-id gebruiken voor het aanduiden van elk apparaat dat is verbonden met uw IoT-hub.

## <a name="device-metadata"></a>Metagegevens van apparaten

IoT Hub onderhoudt een [apparaatdubbel] [ lnk-device-twin] voor elke gesimuleerde en fysieke apparaat dat is verbonden met een oplossing voor externe controle. De oplossing gebruikt apparaatdubbels voor het beheren van de metagegevens die zijn gekoppeld aan apparaten. Een apparaatdubbel is een JSON-document onderhouden door IoT Hub en de oplossing maakt gebruik van de IoT Hub-API om te communiceren met dubbele apparaten.

Een apparaatdubbel slaat drie typen van de metagegevens van:

- *Gerapporteerde eigenschappen* worden verzonden naar een IoT-hub door een apparaat. In de oplossing voor externe controle gesimuleerde apparaten verzenden gerapporteerde eigenschappen bij het opstarten en in reactie op **Changedevicestate** opdrachten en methoden. U vindt gerapporteerde eigenschappen in de **apparatenlijst** en **Apparaatdetails** in de portal van de oplossing. Gerapporteerde eigenschappen zijn alleen-lezen.
- *Gewenste eigenschappen* door apparaten worden opgehaald uit de IoT-hub. Het is de verantwoordelijkheid van het apparaat voor het maken van een vereiste configuratie wijzigen op het apparaat. Het is ook de verantwoordelijkheid van het apparaat voor het rapporteren van de wijziging naar de hub als een gerapporteerde eigenschap. U kunt de waarde van een gewenste eigenschap via de oplossingsportal instellen.
- *Tags* alleen in de apparaatdubbel bestaan en nooit worden gesynchroniseerd met een apparaat. U kunt tagwaarden in de portal van de oplossing en ze gebruiken wanneer u de lijst met apparaten filteren. De oplossing gebruikt ook een code voor het identificeren van het pictogram om weer te geven voor een apparaat in de portal van de oplossing.

Voorbeeld van de gerapporteerde eigenschappen van de gesimuleerde apparaten omvatten de fabrikant, modelnummer, breedtegraad en lengtegraad. De lijst van ondersteunde methoden gesimuleerde apparaten ook geretourneerd als een gerapporteerde eigenschap.

> [!NOTE]
> De code van het gesimuleerde apparaat gebruikt alleen de gewenste eigenschappen **Desired.Config.TemperatureMeanValue** en **Desired.Config.TelemetryInterval** om de gerapporteerde eigenschappen bij te werken die naar IoT Hub worden teruggestuurd. Alle andere gewenste eigenschap wijzigingsaanvragen worden genegeerd.

Een apparaat informatie metagegevens JSON-document die zijn opgeslagen in de apparaat-register Cosmos DB-database heeft de volgende structuur:

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
> Gegevens van een apparaat kan ook metagegevens om te beschrijven van de telemetrie die het apparaat naar IoT Hub verzendt. Deze metagegevens telemetrie om aan te passen hoe worden weergegeven in het dashboard maakt gebruik van de oplossing voor externe controle [dynamische telemetrie][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Levenscyclus

Wanneer u eerst een apparaat in de portal van de oplossing maakt, maakt de oplossing een vermelding in de Cosmos DB-database voor het opslaan van de geschiedenis van de opdracht en de methode. De oplossing maakt een vermelding voor het apparaat op dit moment ook in het id-register van apparaat, waardoor de sleutels die het apparaat gebruikt om te verifiëren met IoT Hub worden gegenereerd. Het maakt ook een apparaatdubbel.

Wanneer een apparaat is eerst verbinding met de oplossing maakt, stuurt het gerapporteerde eigenschappen en een bericht met apparaatgegevens. De gerapporteerde eigenschapswaarden worden automatisch opgeslagen in de apparaatdubbel. De gerapporteerde eigenschappen bevatten de fabrikant van apparaat, modelnummer serienummer en een lijst met ondersteunde methoden. Het bericht met apparaatgegevens bevat de lijst met de opdrachten die ondersteuning biedt voor het apparaat inclusief informatie over de opdrachtparameters van elke. Wanneer de oplossing voor dit bericht ontvangt, werkt de apparaatgegevens in de Cosmos DB-database.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Weergeven en bewerken van gegevens van een apparaat in de portal van de oplossing

De lijst met apparaten in de portal van de oplossing wordt standaard weergegeven van de volgende apparaateigenschappen als kolommen: **Status**, **DeviceId**, **fabrikant**, **Model Aantal**, **serienummer**, **Firmware**, **Platform**, **Processor**, en  **Geïnstalleerd RAM-geheugen**. U kunt de kolommen aanpassen door te klikken op **kolomeditor**. De apparaateigenschappen **breedtegraad** en **lengtegraad** station van de locatie in de Bing-kaart op het dashboard.

![Kolomeditor in de lijst met apparaten][img-device-list]

In de **Apparaatdetails** deelvenster in de oplossingsportal kunt u gewenste eigenschappen en tags bewerken (gerapporteerde eigenschappen zijn alleen-lezen).

![Deelvenster Apparaatdetails][img-device-edit]

U kunt de portal van de oplossing gebruiken om een apparaat verwijderen uit uw oplossing. Wanneer u een apparaat verwijdert, wordt de oplossing verwijdert u de apparaatvermelding van id-register en verwijdert vervolgens het dubbele apparaat. De oplossing verwijdert u ook informatie met betrekking tot het apparaat van de Cosmos DB-database. Voordat u een apparaat verwijdert kunt, moet u deze uitschakelen.

![Apparaat verwijderen][img-device-remove]

## <a name="device-information-message-processing"></a>Apparaat informatie berichtverwerking

Berichten met apparaatgegevens verzonden door een apparaat, verschilt van berichten over telemetrie. Berichten met apparaatgegevens zijn de opdrachten die een apparaat kan reageren en de opdrachtgeschiedenis van elke. IoT-Hub zelf heeft geen kennis van de metagegevens die zijn opgenomen in een bericht met apparaatgegevens en verwerkt het bericht op dezelfde manier als die alle apparaat-naar-cloud-berichten worden verwerkt. In de oplossing voor externe controle een [Azure Stream Analytics] [ lnk-stream-analytics] (ASA)-taak de berichten uit IoT Hub kan lezen. De **DeviceInfo** filters voor berichten met stream analytics-taak **"ObjectType": "DeviceInfo"** en stuurt ze door de **EventProcessorHost** hostexemplaar die wordt uitgevoerd in een webtaak. Logica in de **EventProcessorHost** instantie de apparaat-ID gebruikt om te zoeken naar de Cosmos DB-record voor het specifieke apparaat en werk het record.

> [!NOTE]
> Een bericht met apparaatgegevens is een standard-apparaat-naar-cloud-bericht. De oplossing wordt onderscheid gemaakt tussen berichten met apparaatgegevens en berichten over telemetrie met behulp van de ASA-query's.

## <a name="next-steps"></a>Volgende stappen

Nu u klaar bent met het leren hoe u de vooraf geconfigureerde oplossingen kunt aanpassen, kunt u enkele van de andere functies en mogelijkheden van de vooraf geconfigureerde IoT Suite-oplossingen verkennen:

* [Overzicht van de voorspeld onderhoud vooraf geconfigureerde oplossing][lnk-predictive-overview]
* [Veelgestelde vragen over IoT Suite][lnk-faq]
* [Fundamentele IoT-beveiliging][lnk-security-groundup]

<!-- Images and links -->
[img-device-list]: media/iot-suite-v1-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-v1-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-v1-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dynamic-telemetry]: iot-suite-v1-dynamic-telemetry.md

[lnk-predictive-overview]:../iot-accelerators/iot-accelerators-predictive-overview.md
[lnk-faq]: iot-suite-v1-faq.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up

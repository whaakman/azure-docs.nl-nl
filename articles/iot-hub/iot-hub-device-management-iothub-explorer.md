---
title: Azure IoT-Apparaatbeheer met iothub explorer | Microsoft Docs
description: De iothub-explorer CLI-hulpprogramma gebruiken voor Azure IoT Hub Apparaatbeheer, met daarin de rechtstreekse methoden en de Twin eigenschappen van de gewenste opties.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot-Apparaatbeheer, Apparaatbeheer via azure iot hub, apparaat management iot, Apparaatbeheer via iot hub
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: xshi
ms.openlocfilehash: 5b7a5057bdfb5920fbb5759bed1f5561cfa1d7e0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a>Iothub-explorer gebruiken voor het beheer van Azure IoT Hub-apparaten

![End-to-end-diagram](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[iothub explorer](https://github.com/azure/iothub-explorer) is een CLI-hulpprogramma dat u op een host uitvoert computer voor het beheren van apparaat-id's in het register van IoT hub. Het wordt geleverd met opties die u kunt verschillende taken uit te voeren.

| Beheeroptie          | Taak                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Directe methoden             | Een apparaat fungeren zoals starten of stoppen van verzenden van berichten of het apparaat opnieuw maken.                                        |
| Eigenschappen van de gewenste Twin    | Een apparaat in een bepaalde status, zoals een LED in groen instellen of het instellen van het interval voor het verzenden van telemetrie tot 30 minuten geplaatst.         |
| Dubbele eigenschappen gerapporteerd   | Ophalen van de gerapporteerde status van een apparaat. Bijvoorbeeld, meldt het apparaat dat de LED nu knippert.                                    |
| Labels Twin                  | Apparaatspecifieke metagegevens niet opslaan in de cloud. Bijvoorbeeld, de implementatielocatie van een snoep-machine.                         |
| Cloud-naar-apparaat-berichten   | Meldingen verzenden naar een apparaat. Bijvoorbeeld, "het is zeer waarschijnlijk regen vandaag. Vergeet niet om een overkoepelende."              |
| Apparaat twin query 's        | Query uitvoeren op alle apparaten horende om op te halen die met een willekeurige voorwaarden, zoals de apparaten die gebruikt worden om te identificeren. |

Zie voor meer gedetailleerde uitleg over de verschillen en richtlijnen over het gebruik van deze opties, [apparaat-naar-cloud communicatie richtlijnen](iot-hub-devguide-d2c-guidance.md) en [Cloud-naar-apparaat communicatie richtlijnen](iot-hub-devguide-c2d-guidance.md).

> [!NOTE]
> Apparaatdubbels zijn JSON-documenten waarin statusinformatie van een apparaat (metagegevens, configuraties en voorwaarden) zijn opgeslagen. IoT Hub persistente een apparaat twin voor elk apparaat dat verbinding met het maakt. Zie voor meer informatie over apparaat horende [aan de slag met apparaat horende](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Wat u leert

U leert met iothub-explorer met de verschillende beheeropties op uw ontwikkelcomputer.

## <a name="what-you-do"></a>Wat u doet

Voer iothub explorer met verschillende opties.

## <a name="what-you-need"></a>Wat u nodig hebt

- Zelfstudie [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) voltooid die wordt ingegaan op de volgende vereisten:
  - Een actief Azure-abonnement.
  - Een Azure-IoT-hub in uw abonnement.
  - Een clienttoepassing dat berichten naar uw Azure-IoT-hub verzendt.
- Zorg ervoor dat uw apparaat wordt uitgevoerd met de clienttoepassing tijdens deze zelfstudie.
- iothub-explorer [iothub explorer installeren](https://github.com/azure/iothub-explorer) op uw ontwikkelcomputer.

## <a name="connect-to-your-iot-hub"></a>Verbinding maken met uw IoT-hub

Verbinding maken met uw IoT-hub met de volgende opdracht:

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a>Iothub explorer gebruiken met directe methoden

Aanroepen de `start` methode in de app apparaat berichten verzenden naar uw IoT-hub met de volgende opdracht:

```bash
iothub-explorer device-method <your device Id> start
```

Aanroepen de `stop` methode in de app apparaat om te stoppen met het verzenden van berichten naar uw IoT-hub met de volgende opdracht:

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a>Gebruik iothub-explorer met de gewenste eigenschappen van twin

Instellen van een interval van de gewenste eigenschap 3000 = met de volgende opdracht:

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

Deze eigenschap kan worden gelezen door uw apparaat.

## <a name="use-iothub-explorer-with-twins-reported-properties"></a>Gebruik iothub-explorer met de dubbele eigenschappen gerapporteerd

Haal de gerapporteerde eigenschappen van het apparaat met de volgende opdracht:

```bash
iothub-explorer get-twin <your device id>
```

Een van de eigenschappen is $metadata. $lastUpdated waarin de laatste keer dat dit apparaat worden verzonden of ontvangen van een bericht.

## <a name="use-iothub-explorer-with-twins-tags"></a>Gebruik iothub-explorer met de labels van twin

De labels en eigenschappen van het apparaat weergegeven door met de volgende opdracht:

```bash
iothub-explorer get-twin <your device id>
```

Voeg een veld rol = temperatuur en vochtigheid aan het apparaat met de volgende opdracht:

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"

```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a>Gebruik iothub-explorer met de Cloud-naar-apparaat-berichten

Een 'Hallo wereld'-bericht verzenden naar het apparaat met de volgende opdracht:

```bash
iothub-explorer send <device-id> "Hello World"
```

Zie [iothub-explorer gebruiken om te verzenden en ontvangen van berichten tussen het apparaat en IoT Hub](iot-hub-explorer-cloud-device-messaging.md) voor een echte scenario van het gebruik van deze opdracht.

## <a name="use-iothub-explorer-with-device-twins-queries"></a>Met iothub-Verkenner terwijl het apparaat horende query 's

Query uitvoeren op apparaten met een label van de rol = 'temperatuur en vochtigheid' door de volgende opdracht uit te voeren:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Query uitvoeren op alle apparaten, behalve die met een label van de rol = 'temperatuur en vochtigheid' door de volgende opdracht uit te voeren:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd iothub-explorer met de verschillende opties gebruiken.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

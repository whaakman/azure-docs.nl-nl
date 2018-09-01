---
title: Apparaat-schema in de oplossing voor apparaten simulatie - Azure | Microsoft Docs
description: Dit artikel beschrijft de JSON-schema waarmee een gesimuleerd apparaat in de oplossing voor het simuleren van apparaten worden gedefinieerd.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: b325873819caff139727ec15d6aecd2d4be89c9e
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338152"
---
# <a name="understand-the-device-model-schema"></a>Schema van het Apparaatmodel begrijpen

U kunt de oplossingsverbetering voor Apparaatsimulatie gebruiken voor het genereren van de belasting testen voor oplossingen die gebruikmaken van IoT Hub. Wanneer u de Apparaatsimulatie-oplossing implementeert, wordt automatisch een verzameling van gesimuleerde apparaten ingericht. U kunt de bestaande gesimuleerde apparaten aanpassen of maak uw eigen.

Dit artikel beschrijft het schema voor het model van apparaten die Hiermee geeft u de mogelijkheden en het gedrag van een gesimuleerd apparaat. Het model is opgeslagen in een JSON-bestand.

De volgende artikelen zijn gerelateerd aan het huidige artikel:

* [Gedrag van het Apparaatmodel implementeren](iot-accelerators-device-simulation-device-behavior.md) beschrijving van de JavaScript-bestanden die u gebruikt voor het implementeren van het gedrag van een gesimuleerd apparaat.
* [Maak een nieuw gesimuleerd apparaat](iot-accelerators-device-simulation-create-simulated-device.md) alles bij elkaar geplaatst en laat zien u hoe u een nieuw gesimuleerd apparaattype implementeert in uw oplossing.

In dit artikel leert u het volgende:

>[!div class="checklist"]
> * Een JSON-bestand gebruiken om een gesimuleerd Apparaatmodel te definiëren
> * Stel de eigenschappen van het gesimuleerde apparaat
> * Geef de telemetrie het gesimuleerde apparaat verzendt
> * Specificeert de methoden die het apparaat reageert op voor het cloud-naar-apparaat

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt beschreven hoe u om uw eigen aangepaste gesimuleerde apparaat-model te maken. In dit artikel hebt u geleerd hoe aan:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Een JSON-bestand gebruiken om een gesimuleerd Apparaatmodel te definiëren
> * Stel de eigenschappen van het gesimuleerde apparaat
> * Geef de telemetrie het gesimuleerde apparaat verzendt
> * Specificeert de methoden die het apparaat reageert op voor het cloud-naar-apparaat

Nu dat u hebt geleerd over de JSON-schema, de voorgestelde volgende stap is te leren hoe u [implementeren van het gedrag van uw gesimuleerde apparaat](iot-accelerators-device-simulation-device-behavior.md).

Zie voor meer informatie voor ontwikkelaars over de Apparaatsimulatie-oplossing, de [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/device-simulation-dotnet/wiki/Simulation-Service-Developer-Reference-Guide).

---
title: Apparaat-schema in de oplossing voor externe controle - Azure | Microsoft Docs
description: Dit artikel beschrijft de JSON-schema waarmee een gesimuleerd apparaat in de oplossing voor externe controle worden gedefinieerd.
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 12/18/2018
ms.topic: conceptual
ms.openlocfilehash: 3c2bca45a7927689992cf44fb70199e246641760
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54445419"
---
# <a name="understand-the-device-model-schema"></a>Schema van het Apparaatmodel begrijpen

U kunt gesimuleerde apparaten in de oplossing voor externe controle gebruiken om het gedrag te testen. De oplossing voor externe controle bevat een apparaat simuleren-service voor het uitvoeren van de gesimuleerde apparaten. Wanneer u de oplossing voor externe controle implementeert, wordt automatisch een verzameling van gesimuleerde apparaten ingericht. U kunt de bestaande gesimuleerde apparaten aanpassen of maak uw eigen.

Dit artikel beschrijft het schema voor het model van apparaten die Hiermee geeft u de mogelijkheden en het gedrag van een gesimuleerd apparaat. Het model is opgeslagen in een JSON-bestand.

> [!NOTE]
> Dit apparaat modelschema is alleen voor de gesimuleerde apparaten die worden gehost in de device simulatie-service. Als u maken van een echt apparaat wilt, Zie [uw apparaat aansluiten op de oplossingsverbetering voor externe controle](iot-accelerators-connecting-devices.md).

De volgende artikelen zijn gerelateerd aan het huidige artikel:

* [Gedrag van het Apparaatmodel implementeren](iot-accelerators-remote-monitoring-device-behavior.md) beschrijving van de JavaScript-bestanden die u gebruikt voor het implementeren van het gedrag van een gesimuleerd apparaat.
* [Maak een nieuw gesimuleerd apparaat](iot-accelerators-remote-monitoring-create-simulated-device.md) alles bij elkaar geplaatst en laat zien u hoe u een nieuw gesimuleerd apparaattype implementeert in uw oplossing.

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

Nu dat u hebt geleerd over de JSON-schema, de voorgestelde volgende stap is te leren hoe u [implementeren van het gedrag van uw gesimuleerde apparaat](iot-accelerators-remote-monitoring-device-behavior.md).

Zie voor meer informatie voor ontwikkelaars over de oplossing voor externe controle:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

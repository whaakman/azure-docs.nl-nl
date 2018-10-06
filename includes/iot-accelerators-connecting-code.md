---
title: bestand opnemen
description: bestand opnemen
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 09/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: c79b6f854dc78670a7eb8a1275c3e2fc46fcdd99
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831684"
---
### <a name="code-walkthrough"></a>Code-overzicht

Deze sectie beschrijft een aantal van de belangrijkste onderdelen van de voorbeeldcode en wordt uitgelegd hoe deze in verband met de oplossingsverbetering voor externe controle.

Het volgende fragment toont hoe de gerapporteerde eigenschappen die de mogelijkheden van het apparaat beschrijven worden gedefinieerd. Deze eigenschappen zijn onder andere:

- De locatie van het apparaat om in te schakelen de oplossingsverbetering voor het apparaat toevoegen aan de kaart.
- De huidige firmwareversie.
- De lijst met methoden die biedt ondersteuning voor het apparaat.
- Het schema van de telemetrieberichten die door het apparaat verzonden.

[!code-cpp[Define data structures for Chiller](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=datadefinition "Define data structures for Chiller")]

Het voorbeeld bevat een **serializeToJson** -functie die deze gegevensstructuur met behulp van de bibliotheek Parson serialiseert.

Het voorbeeld bevat verschillende callback-functies die gegevens naar de console afdrukken als de client met de oplossingsversnellers communiceert:

- **connection_status_callback**
- **send_confirm_callback**
- **reported_state_callback**
- **device_method_callback**

Het volgende codefragment bevat de **device_method_callback** functie. Deze functie bepaalt de actie te ondernemen wanneer een methodeaanroep wordt ontvangen van de solution accelerator. De functie ontvangt een verwijzing naar de **Koelunit** gegevens structureren de **userContextCallback** parameter. De waarde van **userContextCallback** wordt ingesteld wanneer de callback-functie is geconfigureerd in de **belangrijkste** functie:

[!code-cpp[Device method callback](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=devicemethodcallback "Device method callback")]

Wanneer de oplossingsverbetering voor de firmware-update-methode aanroept, wordt het voorbeeld gedeserialiseerd van de JSON-nettolading en een achtergrond-thread voor het voltooien van het updateproces wordt gestart. Het volgende codefragment bevat de **do_firmware_update** die wordt uitgevoerd op de thread:

[!code-cpp[Firmware update thread](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=firmwareupdate "Firmware update thread")]

Het volgende fragment toont hoe de client een bericht telemetrie verzendt naar de solution accelerator. De eigenschappen van berichten zijn onder andere het berichtschema om te helpen de oplossingsverbetering voor de telemetrie weergegeven op het dashboard:

[!code-cpp[Send telemetry](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=sendmessage "Send telemetry")]

De **belangrijkste** functie in het voorbeeld:

- Wordt geïnitialiseerd en de SDK-subsysteem wordt afgesloten.
- Initialiseert de **Koelunit** gegevensstructuur.
- De gerapporteerde eigenschappen stuurt naar de solution accelerator.
- Hiermee configureert u het apparaat methode callback-functie.
- Stuurt gesimuleerde telemetriewaarden naar de solution accelerator.

[!code-cpp[Main](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=main "Main")]

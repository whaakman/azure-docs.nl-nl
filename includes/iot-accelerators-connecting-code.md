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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61450211"
---
### <a name="code-walkthrough"></a>Code-overzicht

Deze sectie beschrijft een aantal van de belangrijkste onderdelen van de voorbeeldcode en wordt uitgelegd hoe deze in verband met de oplossingsverbetering voor externe controle.

Het volgende fragment toont hoe de gerapporteerde eigenschappen die de mogelijkheden van het apparaat beschrijven worden gedefinieerd. Deze eigenschappen zijn onder andere:

- De locatie van het apparaat om in te schakelen de oplossingsverbetering voor het apparaat toevoegen aan de kaart.
- De huidige firmwareversie.
- De lijst met methoden die biedt ondersteuning voor het apparaat.
- Het schema van de telemetrieberichten die door het apparaat verzonden.



Het voorbeeld bevat een **serializeToJson** -functie die deze gegevensstructuur met behulp van de bibliotheek Parson serialiseert.

Het voorbeeld bevat verschillende callback-functies die gegevens naar de console afdrukken als de client met de oplossingsversnellers communiceert:

- **connection_status_callback**
- **send_confirm_callback**
- **reported_state_callback**
- **device_method_callback**

Het volgende codefragment bevat de **device_method_callback** functie. Deze functie bepaalt de actie te ondernemen wanneer een methodeaanroep wordt ontvangen van de solution accelerator. De functie ontvangt een verwijzing naar de **Koelunit** gegevens structureren de **userContextCallback** parameter. De waarde van **userContextCallback** wordt ingesteld wanneer de callback-functie is geconfigureerd in de **belangrijkste** functie:



Wanneer de oplossingsverbetering voor de firmware-update-methode aanroept, wordt het voorbeeld gedeserialiseerd van de JSON-nettolading en een achtergrond-thread voor het voltooien van het updateproces wordt gestart. Het volgende codefragment bevat de **do_firmware_update** die wordt uitgevoerd op de thread:



Het volgende fragment toont hoe de client een bericht telemetrie verzendt naar de solution accelerator. De eigenschappen van berichten zijn onder andere het berichtschema om te helpen de oplossingsverbetering voor de telemetrie weergegeven op het dashboard:



De **belangrijkste** functie in het voorbeeld:

- Wordt geïnitialiseerd en de SDK-subsysteem wordt afgesloten.
- Initialiseert de **Koelunit** gegevensstructuur.
- De gerapporteerde eigenschappen stuurt naar de solution accelerator.
- Hiermee configureert u het apparaat methode callback-functie.
- Stuurt gesimuleerde telemetriewaarden naar de solution accelerator.



---
title: Azure IoT Hub naar Azure Event Hubs vergelijken | Microsoft Docs
description: Een vergelijking van de markering functionele verschillen en gebruiksvoorbeelden IoT Hub en Event Hubs Azure-services. De vergelijking omvat ondersteunde protocollen, Apparaatbeheer, bewaking, en bestanden kunnen uploaden.
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: b515e05d16dda83c7d865113d5d3578c44be084f
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Vergelijking van Azure IoT Hub en Azure Event Hubs
Een van de belangrijkste gebruiksvoorbeelden voor IoT Hub is voor het verzamelen van telemetriegegevens van apparaten. Daarom vaak IoT Hub wordt vergeleken met [Azure Event Hubs][Azure Event Hubs]. Zoals IoT Hub is Event Hubs een verwerkingsservice waarmee gebeurtenissen en telemetriegegevens verzamelen in de cloud op grote schaal met weinig latentie en hoge betrouwbaarheid.

De services hebben echter veel verschillen, die in de volgende tabel worden beschreven:

| Onderwerp | IoT Hub | Event Hubs |
| --- | --- | --- |
| Communicatiepatronen | Hiermee [apparaat-naar-cloud communicatie] [ lnk-d2c-guidance] (messaging, file uploads en gerapporteerde eigenschappen) en [cloud-naar-apparaat communicatie] [ lnk-c2d-guidance] (direct methoden, gewenste eigenschappen, messaging). |U kunt alleen gebeurtenis Inkomend (meestal beschouwd als voor apparaat-naar-cloud-scenario's). |
| Informatie over de status van apparaat | [Apparaat horende] [ lnk-twins] kunt opslaan en opvragen van informatie over de status van apparaten. | Er is geen informatie over de status van het apparaat kan worden opgeslagen. |
| Ondersteuning voor het protocol van apparaten |Biedt ondersteuning voor protocollen MQTT, MQTT over WebSockets, AMQP, AMQP via WebSockets en HTTPS. Bovendien IoT Hub werkt met de [protocolgateway van Azure IOT][lnk-azure-protocol-gateway], een aanpasbare protocol-gateway-implementatie voor de ondersteuning van aangepaste protocollen. |Ondersteunt AMQP, AMQP via WebSockets en HTTPS. |
| Beveiliging |Biedt per apparaat-id en terughalen toegangsbeheer. Zie de [sectie beveiliging van de IoT Hub developer guide]. |Biedt Event Hubs-wide [gedeeld toegangsbeleid][Event Hubs - security], met intrekken van beperkte ondersteuning voor via [van uitgeversbeleid][Event Hubs publisher policies]. IoT-oplossingen zijn vaak vereist voor het implementeren van een aangepaste oplossing ter ondersteuning van referenties per apparaat en anti-adresvervalsing metingen. |
| Controle van bewerkingen |Hiermee kunt IoT-oplossingen om u te abonneren op een uitgebreide reeks apparaat identity management en connectiviteit gebeurtenissen, zoals afzonderlijke apparaat verificatiefouten, beperking en uitzonderingen onjuiste notatie. Deze gebeurtenissen kunnen u snel connectiviteitsproblemen op het niveau van de afzonderlijke apparaten kan identificeren. |Beschrijft alleen cumulatieve metrische gegevens. |
| Schalen |Is geoptimaliseerd voor ondersteuning van miljoenen gelijktijdig verbonden apparaten. |De verbindingen conform meters [Azure Event Hubs quota][Azure Event Hubs quotas]. Event Hubs daarentegen, kunt u opgeven van de partitie voor elk bericht verzonden. |
| Apparaat-SDK 's |Biedt [apparaat-SKD's] [ Azure IoT SDKs] voor een groot aantal verschillende platforms en talen naast directe MQTT AMQP en HTTPS-API's. |Wordt ondersteund op .NET, Java en C, ook naar AMQP- en HTTPS verzenden-interfaces. |
| Bestand uploaden |Hiermee kunt IoT-oplossingen voor het uploaden van bestanden van apparaten naar de cloud. Bevat een bestand meldingseindpunt voor workflow-integratie en een operationele bewaking categorie voor de ondersteuning voor foutopsporing. | Niet ondersteund. |
| Routeren van berichten naar meerdere eindpunten | Maximaal 10 aangepaste eindpunten worden ondersteund. Regels bepalen hoe berichten worden doorgestuurd naar aangepaste eindpunten. Zie voor meer informatie [berichten verzenden en ontvangen met IoT Hub][lnk-devguide-messaging]. | Vereist aanvullende code worden geschreven en gehost voor berichtverzending. |

Kortom, zelfs als het enige gebruiksvoorbeeld telemetrie voor apparaat-naar-cloud-inkomend biedt IoT Hub een service die is ontworpen voor connectiviteit van IoT-apparaten. Vouw de waardevoorstellen voor deze scenario's met IoT-specifieke functies blijft. Event Hubs is ontworpen voor gebeurtenis inkomend grootschalige, zowel in de context van scenario's inter datacenter en intra-datacenter.

Dit gebeurt niet IoT Hub en Event Hubs in dezelfde oplossing gebruiken. IoT Hub verwerkt de apparaat-naar-cloud-communicatie en Event Hubs later stadium gebeurtenis inkomend in realtime verwerking engines afgehandeld.

### <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het plannen van uw IoT Hub-implementatie, [schalen en HA DR][lnk-scaling].

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op de edge-apparaten met Azure IoT rand][lnk-iotedge]

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[sectie beveiliging van de IoT Hub developer guide]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-features.md#event-publishers
[Azure Event Hubs quotas]: ../event-hubs/event-hubs-quotas.md
[Azure IoT SDKs]: https://github.com/Azure/azure-iot-sdks
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md

---
title: Opties voor Azure IoT Hub cloud-naar-apparaat | Microsoft Docs
description: Handleiding voor ontwikkelaars - richtlijnen voor wanneer rechtstreekse methoden van apparaat twin gewenste eigenschappen of cloud-naar-apparaat-berichten gebruiken voor communicatie van cloud-naar-apparaat.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: ff81be4bbf6d297c623c5d98b5dc22a540112fcc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634434"
---
# <a name="cloud-to-device-communications-guidance"></a>Cloud-naar-apparaat communicatie richtlijnen
IoT Hub biedt drie opties voor apparaat-apps om functionaliteit om een back-endserver voor apps weer te geven:

* [Directe methoden] [ lnk-methods] voor communicatie waarvoor onmiddellijke bevestiging van het resultaat. Rechtstreekse methoden worden vaak gebruikt voor interactief beheer van apparaten zoals het inschakelen van een ventilator.
* [Dubbele eigenschappen van de gewenste] [ lnk-twins] voor langlopende opdrachten bedoeld voor het plaatsen van het apparaat in een bepaalde status gewenst. Bijvoorbeeld, het interval voor het verzenden van telemetrie ingesteld op 30 minuten.
* [Cloud-naar-apparaatberichten] [ lnk-c2d] voor eenzijdige meldingen naar de apparaat-app.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Hier volgt een gedetailleerde vergelijking van de verschillende communicatieopties van cloud-naar-apparaat.

|  | Directe methoden | De gewenste eigenschappen van Twin | Cloud-naar-apparaat-berichten |
| ---- | ------- | ---------- | ---- |
| Scenario | Opdrachten waarvoor onmiddellijke bevestiging, zoals het inschakelen van een ventilator nodig. | Langlopende opdrachten bedoeld om het apparaat in een bepaalde gewenste status te plaatsen. Bijvoorbeeld, het interval voor het verzenden van telemetrie ingesteld op 30 minuten. | Eenzijdige meldingen naar de apparaat-app. |
| Gegevensstroom | Twee richtingen. De app voor het apparaat kan reageren op de methode meteen. De back-end oplossing ontvangt de uitkomst contextueel op de aanvraag. | Eenzijdige. De apparaat-app ontvangt een melding met de eigenschap wijziging. | Eenzijdige. De apparaat-app ontvangt het bericht
| Duurzaamheid | Niet-verbonden apparaten geen contact wordt opgenomen. De back-end oplossing wordt gemeld dat het apparaat niet is verbonden. | Eigenschapswaarden worden bewaard in de apparaat-twin. Apparaat wordt lezen op de volgende opnieuw verbinding te maken. Eigenschapswaarden zijn worden opgehaald met de [IoT Hub-querytaal][lnk-query]. | Berichten worden behouden door de IoT Hub voor maximaal 48 uur. |
| Doelen | Met behulp van één apparaat **deviceId**, of meerdere apparaten met behulp van [taken][lnk-jobs]. | Met behulp van één apparaat **deviceId**, of meerdere apparaten met behulp van [taken][lnk-jobs]. | Eén apparaat door **deviceId**. |
| Grootte | Directe methode maximale nettolading is 128 KB. | Maximum aantal gewenste grootte van de eigenschappen van 8 KB. | Berichten van maximaal 64 KB. |
| Frequentie | Hoog. Zie voor meer informatie [IoT Hub beperkt][lnk-quotas]. | Gemiddeld. Zie voor meer informatie [IoT Hub beperkt][lnk-quotas]. | Laag. Zie voor meer informatie [IoT Hub beperkt][lnk-quotas]. |
| Protocol | Beschikbaar met MQTT of AMQP. | Beschikbaar met MQTT of AMQP. | Beschikbaar op alle protocollen. Apparaat moet controleren wanneer u HTTPS. |

Meer informatie over het gebruik van directe methoden, eigenschappen van de gewenste en cloud-naar-apparaat-berichten in de volgende zelfstudies:

* [Directe methoden gebruiken][lnk-methods-tutorial], voor rechtstreekse methoden;
* [Gewenste eigenschappen gebruiken om apparaten te configureren][lnk-twin-properties]voor apparaat twin de gewenste eigenschappen; 
* [Cloud-naar-apparaat-berichten verzenden][lnk-c2d-tutorial], voor cloud-naar-apparaat-berichten.

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-tutorial]: iot-hub-node-node-c2d.md

---
title: Opties voor Azure IoT-Hub cloud-naar-apparaat | Microsoft Docs
description: Ontwikkelaars begeleiden - richtlijnen voor wanneer directe methoden, apparaatdubbel en de gewenste eigenschappen of cloud-naar-apparaat-berichten worden gebruikt voor communicatie met cloud-naar-apparaat.
author: fsautomata
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: 2c2701b2ccfceef7f64078d13501aa9184fc87a3
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451626"
---
# <a name="cloud-to-device-communications-guidance"></a>Cloud-to-device communications guidance

IoT Hub biedt drie opties voor apparaat-apps aan functionaliteit aan een back-end-app weergegeven:

* [Directe methoden](iot-hub-devguide-direct-methods.md) voor communicatie waarvoor onmiddellijke bevestiging van het resultaat. Directe methoden worden vaak gebruikt voor interactieve beheren van apparaten, zoals een ventilator inschakelen.

* [De gewenste eigenschappen van dubbele](iot-hub-devguide-device-twins.md) voor langlopende opdrachten die zijn bedoeld om het apparaat in een bepaalde status gewenste. Bijvoorbeeld, het interval voor het verzenden van telemetrie ingesteld op 30 minuten.

* [Cloud-naar-apparaatberichten](iot-hub-devguide-messages-c2d.md) voor eenrichtingsverkeer meldingen aan de apparaat-app.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Hier volgt een gedetailleerde vergelijking van de verschillende opties voor communicatie van de cloud-naar-apparaat.

|  | Directe methoden | Van de apparaatdubbel-gewenste eigenschappen | Cloud-naar-apparaat-berichten |
| ---- | ------- | ---------- | ---- |
| Scenario | Opdrachten waarvoor onmiddellijke bevestiging, zoals het inschakelen van een fan. | Langlopende opdrachten is bedoeld om het apparaat in een bepaalde gewenste status. Bijvoorbeeld, het interval voor het verzenden van telemetrie ingesteld op 30 minuten. | Eenzijdige meldingen aan de apparaat-app. |
| Gegevensstroom | Beide richtingen. De apparaat-app kunt meteen aan de methode reageren. De back-end oplossing ontvangt het resultaat contextueel aan de aanvraag. | Één richting. De apparaat-app ontvangt een melding met de eigenschap wijzigen. | Één richting. De apparaat-app ontvangt het bericht
| Duurzaamheid | Niet-verbonden apparaten zijn geen contact gemaakt. De back-end van de oplossing wordt geïnformeerd dat het apparaat niet is verbonden. | Eigenschapswaarden worden bewaard in de apparaatdubbel. Apparaat wordt lezen op de volgende opnieuw verbinden. Eigenschapswaarden worden opgehaald met de [IoT Hub-querytaal](iot-hub-devguide-query-language.md). | Berichten kunnen worden bewaard met IoT Hub tot 48 uur. |
| Doelen | Met behulp van één apparaat **deviceId**, of meerdere apparaten met behulp van [taken](iot-hub-devguide-jobs.md). | Met behulp van één apparaat **deviceId**, of meerdere apparaten met behulp van [taken](iot-hub-devguide-jobs.md). | Één apparaat door **deviceId**. |
| Grootte | Directe methode die maximale nettolading is 128 KB. | Maximum aantal gewenste eigenschappen grootte is 8 KB. | Berichten van maximaal 64 KB. |
| Frequentie | Hoog. Zie voor meer informatie, [IoT-Hub beperkt](iot-hub-devguide-quotas-throttling.md). | Gemiddeld. Zie voor meer informatie, [IoT-Hub beperkt](iot-hub-devguide-quotas-throttling.md). | Laag. Zie voor meer informatie, [IoT-Hub beperkt](iot-hub-devguide-quotas-throttling.md). |
| Protocol | Beschikbaar met behulp van MQTT- of AMQP. | Beschikbaar met behulp van MQTT- of AMQP. | Beschikbaar op alle protocollen. Apparaat moet worden gecontroleerd bij het gebruik van HTTPS. |

Meer informatie over het gebruik van directe methoden, gewenste eigenschappen en cloud-naar-apparaat-berichten in de volgende zelfstudies:

* [Directe methoden gebruiken](quickstart-control-device-node.md)
* [Gewenste eigenschappen gebruiken om apparaten te configureren](tutorial-device-twins.md) 
* [Cloud-naar-apparaat-berichten verzenden](iot-hub-node-node-c2d.md)
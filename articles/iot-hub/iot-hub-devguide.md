---
title: Ontwikkelaarshandleiding voor Azure IoT Hub | Microsoft Docs
description: De Azure IoT Hub developer guide bevat discussies van eindpunten, beveiliging, het id-register, Apparaatbeheer, directe methoden, apparaatdubbels, het uploaden van bestanden, taken, de IoT Hub-querytaal en -berichten.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: 0246b19c0155fe2707ac780c7acb61be424d2caf
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57008928"
---
# <a name="azure-iot-hub-developer-guide"></a>Handleiding voor ontwikkelaars van Azure IoT Hub

Azure IoT Hub is een volledig beheerde service die stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Azure IoT Hub beschikt u over:

* Veilige communicatie met behulp van beveiligingsreferenties per apparaat en toegangsbeheer.

* Meerdere opties voor communicatie van grootschalige voor apparaat-naar-cloud en cloud-naar-apparaat.

* Waarin u kunt zoeken opslag van informatie over de status van de per apparaat en de metagegevens.

* Eenvoudig apparaatconnectiviteit met de apparaatbibliotheken voor de meest populaire talen en platforms.

Deze IoT Hub developer guide bevat de volgende artikelen:

* [Richtlijnen voor communicatie van apparaat-naar-cloud](iot-hub-devguide-d2c-guidance.md) kunt u tussen het apparaat-naar-cloud-berichten van de apparaatdubbel-gerapporteerde eigenschappen en uploaden van bestanden kiezen.

* [Richtlijnen voor communicatie met cloud-naar-apparaat](iot-hub-devguide-c2d-guidance.md) kunt u tussen directe methoden, apparaatdubbel en de gewenste eigenschappen en cloud-naar-apparaat-berichten kiezen.

* [Apparaat-naar-cloud en cloud-naar-apparaat met IoT Hub-berichten](iot-hub-devguide-messaging.md) beschrijft de berichtfuncties (apparaat-naar-cloud en cloud-naar-apparaat) die IoT-Hub toont.

  * [Apparaat-naar-cloud-berichten verzenden naar IoT Hub](iot-hub-devguide-messages-d2c.md).

  * [Apparaat-naar-cloud-berichten lezen van het ingebouwde eindpunt](iot-hub-devguide-messages-read-builtin.md).

  * [Aangepaste eindpunten en routeringsregels gebruiken voor apparaat-naar-cloud-berichten](iot-hub-devguide-messages-read-custom.md).

  * [Cloud-naar-apparaat-berichten uit IoT Hub verzenden](iot-hub-devguide-messages-c2d.md).

  * [Maken en berichten van de IoT Hub](iot-hub-devguide-messages-construct.md).

* [Uploaden van bestanden vanaf een apparaat](iot-hub-devguide-file-upload.md) wordt beschreven hoe u bestanden vanaf een apparaat kunt uploaden. Het artikel bevat ook informatie over onderwerpen als de meldingen van het uploadproces kunt verzenden.

* [Apparaat-id's in IoT Hub beheren](iot-hub-devguide-identity-registry.md)wordt beschreven welke gegevens van elke IoT hub-identiteitsopslag register. Het artikel wordt ook beschreven hoe u kunt benaderen en aanpassen.

* [Toegang tot IoT Hub regelen](iot-hub-devguide-security.md) beschrijving van het beveiligingsmodel dat wordt gebruikt voor het verlenen van toegang tot IoT Hub-functionaliteit voor beide apparaten en onderdelen in de cloud. Het artikel bevat informatie over het gebruik van tokens en X.509-certificaten en details van de machtigingen die u kunt verlenen.

* [Apparaatdubbels gebruiken voor het synchroniseren van de status en configuraties](iot-hub-devguide-device-twins.md) beschrijft de *apparaatdubbel* concept. Het artikel ook de functionaliteit van apparaatdubbels descibes weergegeven, zoals een apparaat synchroniseren met de apparaatdubbel. Het artikel bevat informatie over de gegevens die zijn opgeslagen in een apparaatdubbel.

* [Een rechtstreekse methode aanroepen op een apparaat](iot-hub-devguide-direct-methods.md) beschrijving van de levenscyclus van een rechtstreekse methode. Het artikel wordt beschreven hoe u methoden aanroepen op een apparaat van de back-end-app en het verwerken van de directe methode die op uw apparaat.

* [Taken op meerdere apparaten plannen](iot-hub-devguide-jobs.md) wordt beschreven hoe u taken op meerdere apparaten kunt plannen. Het artikel wordt beschreven hoe u taken die taken uitvoeren als het uitvoeren van een rechtstreekse methode, het bijwerken van een apparaat met behulp van een apparaatdubbel verzenden. Ook wordt beschreven hoe u de status van een taak opvragen.

* [Verwijzen naar - een communicatieprotocol kiezen](iot-hub-devguide-protocols.md) beschrijving van de communicatieprotocollen die IoT Hub voor communicatie van het apparaat ondersteunt en staan de poorten die geopend moeten zijn.

* [Referentie - eindpunten van IoT-Hub](iot-hub-devguide-endpoints.md) beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont. Het artikel wordt ook beschreven hoe u extra eindpunten in uw IoT-hub kunt maken en het gebruik van een veldgateway connectiviteit met uw IoT Hub-eindpunten in scenario's met niet-standaard.

* [Referentie - IoT-Hub-querytaal voor apparaatdubbels, taken en berichtroutering](iot-hub-devguide-query-language.md) beschrijft die IoT Hub-querytaal waarmee u gegevens ophalen uit uw hub over uw apparaatdubbels en taken.

* [Referentie - quota en beperkingen](iot-hub-devguide-quotas-throttling.md) bevat een overzicht van de quota die zijn ingesteld in de IoT Hub-service en de beperking die wordt uitgevoerd wanneer u een quotum overschrijdt.

* [Verwijzen naar - prijzen](iot-hub-devguide-pricing.md) bevat algemene informatie over verschillende SKU's en prijzen voor IoT Hub en meer informatie over hoe de verschillende functies van de IoT Hub als berichten worden gemeten door IoT Hub.

* [Referentie - apparaat en service-SDK's](iot-hub-devguide-sdks.md) geeft een lijst van de Azure IoT SDK's voor het ontwikkelen van apparaat- en service-apps die interactie met uw IoT-hub hebben. Het artikel bevat koppelingen naar online API-documentatie.

* [Referentie - IoT-Hub MQTT-ondersteuning](iot-hub-mqtt-support.md) bevat gedetailleerde informatie over hoe het MQTT-protocol in IoT Hub worden ondersteund. Het artikel wordt de ondersteuning voor het MQTT-protocol ingebouwd in de Azure IoT SDK's beschreven en vindt u informatie over het gebruik van het MQTT-protocol rechtstreeks.

* [Verklarende woordenlijst](iot-hub-devguide-glossary.md) een lijst met algemene IoT-Hub-termen.
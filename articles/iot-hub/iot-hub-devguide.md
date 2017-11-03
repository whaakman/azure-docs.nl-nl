---
title: Ontwikkelaarshandleiding voor Azure IoT Hub | Microsoft Docs
description: De Azure IoT Hub developer guide bevat discussies van eindpunten, beveiliging, de id-register, Apparaatbeheer, direct methoden, apparaat horende, bestandsuploads, taken, de querytaal IoT Hub en -berichten.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: d534ff9d-2de5-4995-bb2d-84a02693cb2e
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: dobett
ms.openlocfilehash: 27b296092335ec5b95e8f259756aaf9572da1c16
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-iot-hub-developer-guide"></a>Azure IoT Hub developer guide

Azure IoT Hub is een volledig beheerde service waarmee stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing.

Azure IoT Hub beschikt u over:

* Veilige communicatie met behulp van per apparaat beveiligingsreferenties en toegangscontrole.
* Opties voor meerdere apparaat-naar-cloud- en cloud-naar-apparaat hyperschaal communicatie.
* Waarop opslag van informatie over de status per apparaat en de metagegevens.
* Eenvoudig apparaten connectiviteit met de apparaatbibliotheken voor de meest populaire talen en platforms.

Deze IoT Hub developer guide bevat de volgende artikelen:

* [Apparaat-naar-cloud communicatie richtlijnen] [ lnk-d2c-guidance] kunt u tussen apparaat-naar-cloud-berichten van apparaat twin gemelde eigenschappen en uploaden bestand kiezen.
* [Cloud-naar-apparaat communicatie richtlijnen] [ lnk-c2d-guidance] kunt u tussen rechtstreekse methoden van apparaat twin gewenste eigenschappen en cloud-naar-apparaat-berichten kiezen.
* [Apparaat-naar-cloud- en cloud-naar-apparaat met IoT Hub berichten] [ devguide-messaging] wordt beschreven welke berichten voorzieningen (apparaat-naar-cloud en cloud-naar-apparaat) IoT-Hub toont.
  * [Apparaat-naar-cloud-berichten verzenden met IoT Hub][devguide-messages-d2c].
  * [Apparaat-naar-cloud-berichten lezen van het ingebouwde eindpunt][devguide-builtin].
  * [Gebruik aangepaste eindpunten en routeringsregels voor apparaat-naar-cloudberichten][devguide-custom].
  * [Verzenden van cloud naar apparaat-berichten uit IoT Hub][devguide-messages-c2d].
  * [Maken en IoT Hub berichten lezen][devguide-format].
* [Uploaden van bestanden van een apparaat] [ devguide-upload] wordt beschreven hoe u de bestanden van een apparaat kunt uploaden. Het artikel bevat ook informatie over onderwerpen als de meldingen het uploadproces kunt verzenden.
* [Apparaat-id's van IoT-Hub beheren] [ devguide-identities] wordt beschreven welke gegevens elk IoT-hub register identiteitsopslag en hoe u toegang tot en wijzigt.
* [Toegangsbeheer met IoT Hub] [ devguide-security] beschrijving van het beveiligingsmodel dat wordt gebruikt voor het verlenen van toegang tot de functionaliteit van de IoT Hub voor beide apparaten en in de cloud van onderdelen. Het artikel bevat informatie over het gebruik van tokens en X.509-certificaten en details van de machtigingen die u kunt verlenen.
* [Horende apparaten gebruiken voor het synchroniseren van de status en configuraties] [ devguide-device-twins] beschrijft de *apparaat twin* concept en de functionaliteit zoals het synchroniseren van een apparaat met de apparaat-twin worden getoond. Het artikel bevat informatie over de gegevens die zijn opgeslagen in een twin apparaat.
* [Een directe methode aangeroepen voor een apparaat] [ devguide-directmethods] beschrijving van de levenscyclus van een directe methode, informatie over het aanroepen van methoden op een apparaat vanuit uw back-end-app en verwerken van de directe methode op uw apparaat.
* [Plannen van taken op meerdere apparaten] [ devguide-jobs] wordt beschreven hoe u taken op meerdere apparaten kunt plannen. Het artikel wordt beschreven hoe verzenden van taken waarmee taken worden uitgevoerd als een directe methode, het bijwerken van een apparaat met een apparaat-twin uitvoeren. Ook wordt beschreven hoe u de status van een taak opvragen.
* [Verwijzen naar: Kies een communicatieprotocol] [ devguide-protocol] beschrijft de communicatieprotocollen dat IoT Hub voor apparaatcommunicatie ondersteunt en zijn de poorten die geopend worden moeten.
* [Referentie - IoT-hubeindpunten] [ devguide-endpoints] beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont. Tevens wordt beschreven hoe u extra eindpunten in uw IoT-hub kunt maken en het gebruik van een veldgateway voor verbindingen met uw IoT Hub-eindpunten in niet-standaard scenario's.
* [Referentie - querytaal IoT Hub voor apparaat horende, taken en berichtroutering] [ devguide-query] die IoT Hub-querytaal waarmee u gegevens ophalen van uw hub over uw apparaat horende en taken worden beschreven.
* [Referentie - quota's en beperking] [ devguide-quotas] bevat een overzicht van de quota die zijn ingesteld in de IoT Hub-service en de beperking die wordt uitgevoerd wanneer u een quotum overschrijdt.
* [Verwijzen naar - prijzen] [ devguide-pricing] biedt algemene informatie over de verschillende SKU's en prijzen voor IoT Hub en meer informatie over hoe de verschillende functies van de IoT Hub worden gemeten als berichten door de IoT-Hub.
* [Referentie - apparaat en de service SDK's] [ devguide-sdks] geeft een lijst van de Azure IoT SDK's voor het ontwikkelen van apps voor het apparaat en de service die met uw IoT-hub communiceren. Het artikel bevat koppelingen naar online API-documentatie.
* [Referentie - ondersteuning voor IoT Hub MQTT] [ devguide-mqtt] bevat gedetailleerde informatie over hoe het protocol MQTT ondersteuning biedt voor IoT Hub. Het artikel wordt de ondersteuning voor het protocol MQTT ingebouwd in de Azure IoT SDK's beschreven en vindt u informatie over het gebruik van het protocol MQTT rechtstreeks.
* [Verklarende woordenlijst] [ devguide-glossary] een lijst met algemene IoT-Hub-termen.

[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md
[devguide-pricing]: iot-hub-devguide-pricing.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[devguide-messages-d2c]: iot-hub-devguide-messages-d2c.md
[devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[devguide-custom]: iot-hub-devguide-messages-read-custom.md
[devguide-messages-c2d]: iot-hub-devguide-messages-c2d.md
[devguide-format]: iot-hub-devguide-messages-construct.md
[devguide-protocol]: iot-hub-devguide-protocols.md

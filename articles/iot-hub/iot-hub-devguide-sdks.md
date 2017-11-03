---
title: De Azure IoT SDK's begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - informatie over en koppelingen naar de verschillende Azure IoT-apparaat en de service SDK's die u gebruiken kunt om apps op apparaten en back-end-apps te ontwikkelen.
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: c5c9a497-bb03-4301-be2d-00edfb7d308f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e299de0953cefac925b0015a15983d25d456576f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="understand-and-use-azure-iot-sdks"></a>Begrijpen en gebruiken van Azure IoT SDK 's

Er zijn drie categorieën van SDK's (SDK's) voor het werken met IoT Hub:

* **Apparaat-SDK's** kunt u voor het bouwen van apps die worden uitgevoerd op uw IoT-apparaten. Deze apps verzenden van telemetrie naar uw IoT-hub en eventueel berichten ontvangen van uw IoT-hub.

* **Service-SDK's** helpen u voor het beheren van uw IoT-hub en, desgewenst berichten verzenden naar uw IoT-apparaten.

* **Azure IoT-rand** kunt u bouwen gateways voor apparaten die geen van een van de ondersteunde protocollen gebruikmaken. Gateways kunnen ook verwerkt berichten op de rand.

SDK's zijn beschikbaar voor de ondersteuning van meerdere programmeertalen.

## <a name="azure-iot-device-sdks"></a>Apparaat met Azure IoT SDK 's

Het apparaat met Microsoft Azure IoT SDK's bevatten code voor gebouw apparaten en toepassingen die verbinding maken met en worden beheerd door de services van Azure IoT Hub.

Het volgende apparaat met Azure IoT SDK's kunnen worden gedownload van GitHub:

* [Azure IoT-apparaat SDK voor .NET][lnk-dotnet-device-sdk]
* [Azure IoT-apparaat SDK voor Java][lnk-java-device-sdk]
* [Azure IoT-apparaat SDK voor Node.js][lnk-node-device-sdk]
* [Azure IoT-apparaat SDK voor Python][lnk-python-device-sdk]
* [Azure IoT-apparaat SDK voor C] [ lnk-c-device-sdk] geschreven in C ANSI (C99) voor brede platformcompatibiliteit en draagbaarheid. Er zijn twee apparaat clientbibliotheken voor de laag niveau C **iothub_client** en de **serialisatiefunctie**.

> [!NOTE]
> Zie het Leesmij-bestanden in de GitHub-opslagplaatsen voor informatie over het gebruik van taal en managers pakket platform-specifieke binaire bestanden en afhankelijkheden te installeren op uw ontwikkelcomputer.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>Compatibiliteit met besturingssystemen platform en hardware

Zie voor meer informatie over de SDK compatibiliteit met bepaalde hardware-apparaten, de [Azure gecertificeerd voor IoT-apparaat catalogus][lnk-certified].

## <a name="azure-iot-service-sdks"></a>Azure IoT service SDK 's

De Azure IoT service SDK's bevatten code om het te bevorderen toepassingen maken die communiceren rechtstreeks met IoT Hub voor het beheren van apparaten en beveiliging.

De volgende Azure IoT service SDK's kunnen worden gedownload van GitHub:

* [Azure IoT service SDK voor .NET][lnk-dotnet-service-sdk]
* [Azure IoT service SDK voor Java][lnk-java-service-sdk]
* [Azure IoT service SDK voor Node.js][lnk-node-service-sdk]
* [Azure IoT service SDK voor Python][lnk-python-service-sdk]
* [Azure IoT service SDK voor C][lnk-c-service-sdk]

> [!NOTE]
> Zie het Leesmij-bestanden in de GitHub-opslagplaatsen voor informatie over het gebruik van taal en managers pakket platform-specifieke binaire bestanden en afhankelijkheden te installeren op uw ontwikkelcomputer.

## <a name="azure-iot-edge"></a>Azure IoT Edge

Azure IoT-rand bevat de volgende infrastructuur en modules om oplossingen van IoT gateway te maken. U kunt IoT rand voor het maken van gateways die zijn afgestemd op een end-to-end-scenario uitbreiden.

U kunt downloaden [Azure IoT rand] [ lnk-iot-edge] vanuit GitHub.

## <a name="online-api-reference-documentation"></a>Online documentatie voor API-verwijzing

De volgende lijst bevat koppelingen naar online API-naslagdocumentatie voor Azure IoT-apparaat, service en bibliotheken van de gateway:

* [Internet der dingen (IoT) .NET][lnk-dotnet-ref]
* [Azure IoT-apparaat SDK voor Java][lnk-java-ref]
* [Azure IoT service SDK voor Java][lnk-java-service-ref]
* [Azure IoT-apparaat SDK voor Node.js][lnk-node-ref]
* [Azure IoT service SDK voor Node.js][lnk-node-service-ref]
* [Azure IoT-apparaat SDK voor C][lnk-c-ref]
* [IoT-Hub REST][lnk-rest-ref]
* [Azure IoT-rand][lnk-gateway-ref]

## <a name="next-steps"></a>Volgende stappen

Er zijn andere onderwerpen met naslaginformatie in deze handleiding voor ontwikkelaars van IoT-Hub:

* [Eindpunten van IoT-Hub][lnk-devguide-endpoints]
* [IoT Hub-querytaal voor apparaat horende, taken en het routeren van berichten][lnk-devguide-query]
* [Quota's en beperking][lnk-devguide-quotas]
* [IoT Hub MQTT-ondersteuning][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-c-service-sdk]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_service_client
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/service
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/device
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/service
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device
[lnk-python-service-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/service
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-iot-edge]: https://github.com/Azure/iot-edge

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth
[lnk-node-service-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-gateway-ref]: http://azure.github.io/iot-edge/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

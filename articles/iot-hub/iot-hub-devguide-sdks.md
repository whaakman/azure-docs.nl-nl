---
title: De Azure IoT SDK's begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - informatie over en koppelingen naar de verschillende Azure IoT-apparaat en de service SDK's die u gebruiken kunt om apps op apparaten en back-end-apps te ontwikkelen.
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: c5c9a497-bb03-4301-be2d-00edfb7d308f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/12/2018
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 71d02e3f8bf0757d7ea0622e76b95acda1fefc95
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Begrijpen en gebruiken van Azure IoT Hub SDK 's

Er zijn twee categorieën van SDK's (SDK's) voor het werken met IoT Hub:

* **Apparaat-SDK's** kunt u voor het bouwen van apps die worden uitgevoerd op uw IoT-apparaten. Deze apps verzenden van telemetrie naar uw IoT-hub en eventueel ontvangen berichten, taak, methode of twin updates van uw IoT-hub.

* **Service-SDK's** kunt u beheren van uw IoT-hub en eventueel berichten verzenden, -taken plannen, direct methoden aanroepen of updates van de gewenste eigenschap verzenden naar uw IoT-apparaten.

Meer informatie over de voordelen van het ontwikkelen met Azure IoT SDK's [hier][lnk-benefits-blog].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>Apparaat met Azure IoT SDK 's

Het apparaat met Microsoft Azure IoT SDK's bevatten code voor gebouw apparaten en toepassingen die verbinding maken met en worden beheerd door de services van Azure IoT Hub.

Azure IoT Hub apparaat SDK voor .NET: 
* Installeer vanaf [Nuget][lnk-nuget-csharp-device]
* [Broncode][lnk-dotnet-sdk]
* [API-referentiemateriaal][lnk-dotnet-ref]

Azure IoT Hub apparaat SDK voor C: geschreven in C ANSI (C99) voor brede platformcompatibiliteit en draagbaarheid
* Installeer vanaf [apt get, MBED, Arduino IDE of Nuget][lnk-c-package]
* [Broncode][lnk-c-sdk]
* [API-referentiemateriaal][lnk-c-ref]

Azure IoT Hub apparaat SDK voor Java: 
* Toevoegen aan [Maven] [ lnk-maven-device] project
* [Broncode][lnk-java-sdk]
* [API-referentiemateriaal][lnk-java-ref]

Azure IoT Hub apparaat SDK voor Node.js: 
* Installeer vanaf [npm][lnk-npm-device]
* [Broncode][lnk-node-sdk]
* [API-referentiemateriaal][lnk-node-ref]

Azure IoT Hub apparaat SDK voor Python: 
* Installeer vanaf [pip][lnk-pip-device]
* [Broncode][lnk-python-sdk]

> [!NOTE]
> Zie het Leesmij-bestanden in de GitHub-opslagplaatsen voor informatie over het gebruik van taal en managers pakket platform-specifieke binaire bestanden en afhankelijkheden te installeren op uw ontwikkelcomputer.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>Compatibiliteit met besturingssystemen platform en hardware

Zie voor meer informatie over de SDK compatibiliteit met bepaalde hardware-apparaten, de [Azure gecertificeerd voor IoT-apparaat catalogus] [ lnk-certified] of afzonderlijke opslagplaats.

## <a name="azure-iot-service-sdks"></a>Azure IoT service SDK 's

De Azure IoT service SDK's bevatten code om het te bevorderen toepassingen maken die communiceren rechtstreeks met IoT Hub voor het beheren van apparaten en beveiliging.

Azure IoT Hub service SDK voor .NET:
* Downloaden van [Nuget][lnk-nuget-csharp-service]
* [Broncode][lnk-dotnet-sdk]
* [API-referentiemateriaal][lnk-dotnet-service-ref]

Azure IoT Hub service SDK voor Java: 
* Toevoegen aan [Maven] [ lnk-maven-service] project
* [Broncode][lnk-java-sdk]
* [API-referentiemateriaal][lnk-java-service-ref]

Azure IoT Hub service SDK voor Node.js: 
* Downloaden van [npm][lnk-npm-service]
* [Broncode][lnk-node-sdk]
* [API-referentiemateriaal][lnk-node-service-ref]

Azure IoT Hub service SDK voor Python: 
* Downloaden van [pip][lnk-pip-service]
* [Broncode][lnk-python-sdk]

Azure IoT Hub service SDK voor C: 
* Downloaden van [apt get, MBED, Arduino IDE of Nuget][lnk-c-package]
* [Broncode][lnk-c-sdk]

> [!NOTE]
> Zie het Leesmij-bestanden in de GitHub-opslagplaatsen voor informatie over het gebruik van taal en managers pakket platform-specifieke binaire bestanden en afhankelijkheden te installeren op uw ontwikkelcomputer.


## <a name="next-steps"></a>Volgende stappen

Er zijn andere onderwerpen met naslaginformatie in deze handleiding voor ontwikkelaars van IoT-Hub:

* [Eindpunten van IoT-Hub][lnk-devguide-endpoints]
* [IoT Hub-querytaal voor apparaat horende, taken en het routeren van berichten][lnk-devguide-query]
* [Quota's en beperking][lnk-devguide-quotas]
* [IoT Hub MQTT-ondersteuning][lnk-devguide-mqtt]
* [IoT Hub REST API-referentiemateriaal][lnk-rest-ref]

<!-- Links and images -->

[lnk-c-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-dotnet-sdk]: https://github.com/Azure/azure-iot-sdk-csharp
[lnk-java-sdk]: https://github.com/Azure/azure-iot-sdk-java
[lnk-node-sdk]: https://github.com/Azure/azure-iot-sdk-node
[lnk-python-sdk]: https://github.com/Azure/azure-iot-sdk-python
[lnk-certified]: https://catalog.azureiotsuite.com/

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet
[lnk-dotnet-service-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service
[lnk-node-service-ref]: https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest

[lnk-maven-device]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk
[lnk-maven-service]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk
[lnk-npm-device]: https://www.npmjs.com/package/azure-iot-device
[lnk-npm-service]: https://www.npmjs.com/package/azure-iothub
[lnk-nuget-csharp-device]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-csharp-service]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-c-package]: https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md
[lnk-pip-device]: https://pypi.python.org/pypi/azure-iothub-device-client/
[lnk-pip-service]: https://pypi.python.org/pypi/azure-iothub-service-client/


[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-benefits-blog]: https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/

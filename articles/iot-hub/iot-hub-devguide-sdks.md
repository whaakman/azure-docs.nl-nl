---
title: De Azure IoT SDK's begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - informatie over en koppelingen naar de verschillende Azure IoT-apparaat en service-SDK's die u gebruiken kunt om apps op apparaten en back-end-apps te bouwen.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: dobett
ms.openlocfilehash: 710de8021abfa5b1fc17491af6b8b9f2bdd3a19f
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42919034"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Begrijpen en gebruiken van Azure IoT Hub SDK 's

Er zijn twee soorten software development kits (SDK's) voor het werken met IoT Hub:

* **Apparaat-SDK's** kunt u het bouwen van apps die worden uitgevoerd op uw IoT-apparaten. Deze apps verzenden van telemetrie naar uw IoT-hub en eventueel berichten, taak, methode of dubbele updates ontvangen van uw IoT-hub.

* **Service-SDK's** kunt u uw IoT-hub, beheren en (optioneel) berichten te verzenden, taken plannen, directe methoden aanroepen of gewenste eigenschap updates verzenden naar uw IoT-apparaten.

Meer informatie over de voordelen van het ontwikkelen met behulp van Azure IoT SDK's [hier][lnk-benefits-blog].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>Azure IoT device SDK 's

De Microsoft Azure IoT device SDK's bevatten code die het mogelijk maakt gebouw apparaten en toepassingen die verbinding maken met en worden beheerd door Azure IoT Hub-services.

Azure IoT Hub apparaat-SDK voor .NET: 
* Installeren van [Nuget][lnk-nuget-csharp-device]
* [Broncode][lnk-dotnet-sdk]
* [API-verwijzing][lnk-dotnet-ref]

Azure IoT Hub device-SDK voor C: die zijn geschreven in C ANSI (C99) voor portabiliteit en brede platformcompatibiliteit
* Installeren van [apt-get, MBED, Arduino IDE of Nuget][lnk-c-package]
* [Broncode][lnk-c-sdk]
* [API-verwijzing][lnk-c-ref]

Azure IoT Hub apparaat-SDK voor Java: 
* Toevoegen aan [Maven] [ lnk-maven-device] project
* [Broncode][lnk-java-sdk]
* [API-verwijzing][lnk-java-ref]

Azure IoT Hub apparaat-SDK voor Node.js: 
* Installeren van [npm][lnk-npm-device]
* [Broncode][lnk-node-sdk]
* [API-verwijzing][lnk-node-ref]

Azure IoT Hub apparaat-SDK voor Python: 
* Installeren van [pip][lnk-pip-device]
* [Broncode][lnk-python-sdk]

Azure IoT Hub apparaat-SDK voor iOS: 
* Installeren van [CocoaPod][lnk-cocoa-device]
* [Voorbeelden][lnk-ios-sample]

> [!NOTE]
> Raadpleeg het Leesmij-bestanden in de GitHub-opslagplaatsen voor informatie over het gebruik van taal en platform-specifieke Pakketbeheer binaire bestanden en afhankelijkheden voor netwerkapparaten installeren op uw ontwikkelcomputer.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>OS-platform en de hardware-compatibiliteit

Ondersteunde platforms voor de SDK's vindt u in deze [document](iot-hub-device-sdk-platform-support.md).
Zie voor meer informatie over de compatibiliteit van de SDK met de specifieke hardware, de [Azure Certified voor IoT-apparaatcatalogus] [ lnk-certified] of afzonderlijke-opslagplaats.

## <a name="azure-iot-service-sdks"></a>Azure IoT service SDK 's

De Azure IoT service SDK's bevatten code toepassingen ontwikkelen die communiceren rechtstreeks met IoT Hub voor het beheren van apparaten en beveiliging in het kader.

Azure IoT Hub-service SDK voor .NET:
* Downloaden van [Nuget][lnk-nuget-csharp-service]
* [Broncode][lnk-dotnet-sdk]
* [API-verwijzing][lnk-dotnet-service-ref]

Azure IoT Hub-service SDK voor Java: 
* Toevoegen aan [Maven] [ lnk-maven-service] project
* [Broncode][lnk-java-sdk]
* [API-verwijzing][lnk-java-service-ref]

Azure IoT Hub-service SDK voor Node.js: 
* Downloaden van [npm][lnk-npm-service]
* [Broncode][lnk-node-sdk]
* [API-verwijzing][lnk-node-service-ref]

Azure IoT Hub-service SDK voor Python: 
* Downloaden van [pip][lnk-pip-service]
* [Broncode][lnk-python-sdk]

Azure IoT Hub-service SDK voor C: 
* Downloaden van [apt-get, MBED, Arduino IDE of Nuget][lnk-c-package]
* [Broncode][lnk-c-sdk]

Azure IoT Hub-service SDK voor iOS: 
* Installeren van [CocoaPod][lnk-cocoa-service]
* [Voorbeelden][lnk-ios-sample]

> [!NOTE]
> Raadpleeg het Leesmij-bestanden in de GitHub-opslagplaatsen voor informatie over het gebruik van taal en platform-specifieke Pakketbeheer binaire bestanden en afhankelijkheden voor netwerkapparaten installeren op uw ontwikkelcomputer.



## <a name="next-steps"></a>Volgende stappen

Azure IoT SDK's bieden ook een set hulpprogramma's om te helpen bij de ontwikkeling:
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics): een platformoverschrijdende opdrachtregel-hulpprogramma om problemen vast te stellen met betrekking tot verbinding met IoT Hub.
* [apparatenverkenner](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer): een Windows-bureaubladtoepassing verbinding maken met uw IoT-Hub.

Er zijn andere onderwerpen met naslaginformatie in deze IoT Hub developer guide:

* [IoT Hub-eindpunten][lnk-devguide-endpoints]
* [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering][lnk-devguide-query]
* [Quota en beperkingen][lnk-devguide-quotas]
* [IoT Hub MQTT-ondersteuning][lnk-devguide-mqtt]
* [IoT Hub REST API-naslaginformatie][lnk-rest-ref]
* [Azure IoT SDK platform-ondersteuning](iot-hub-device-sdk-platform-support.md)

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
[lnk-c-package]: https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md
[lnk-pip-device]: https://pypi.python.org/pypi/azure-iothub-device-client/
[lnk-pip-service]: https://pypi.python.org/pypi/azure-iothub-service-client/


[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-benefits-blog]: https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/
[lnk-cocoa-device]: https://cocoapods.org/pods/AzureIoTHubClient
[lnk-ios-sample]: https://github.com/Azure-Samples/azure-iot-samples-ios
[lnk-cocoa-service]: https://cocoapods.org/pods/AzureIoTHubServiceClient

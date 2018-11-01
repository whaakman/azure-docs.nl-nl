---
title: De Azure IoT SDK's begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - informatie over en koppelingen naar de verschillende Azure IoT-apparaat en service-SDK's die u gebruiken kunt om apps op apparaten en back-end-apps te bouwen.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dobett
ms.openlocfilehash: 9daca1876a291cb1f726e7c0eb4840c57f5c54c8
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741314"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Begrijpen en gebruiken van Azure IoT Hub SDK 's

Er zijn drie soorten software development kits (SDK's) voor het werken met IoT Hub:

* **Apparaat-SDK's** kunt u het bouwen van apps die worden uitgevoerd op uw IoT-apparaten met behulp van apparaatclient of module. Deze apps verzenden van telemetrie naar uw IoT-hub en eventueel berichten, taak, methode of dubbele updates ontvangen van uw IoT-hub.  U kunt ook module client schrijven [modules](../iot-edge/iot-edge-modules.md) voor [Azure IoT Edge-runtime](../iot-edge/about-iot-edge.md).

* **Service-SDK's** kunt u uw IoT-hub, beheren en (optioneel) berichten te verzenden, taken plannen, directe methoden aanroepen of gewenste eigenschap updates verzenden naar uw IoT-apparaten of modules.

* **Device Provisioning-SDK's** kunt u voor het inrichten van apparaten naar uw IoT-Hub met behulp van de [Device Provisioning Service](../iot-dps/about-iot-dps.md).

Meer informatie over de [voordelen van het ontwikkelen met behulp van Azure IoT SDK's](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>Azure IoT device SDK 's

De Microsoft Azure IoT device SDK's bevatten code die het mogelijk maakt gebouw apparaten en toepassingen die verbinding maken met en worden beheerd door Azure IoT Hub-services.

Azure IoT Hub apparaat-SDK voor .NET: 

* Installeren van [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/)
* [Broncode](https://github.com/Azure/azure-iot-sdk-csharp)
* [API-naslaginformatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [Moduleverwijzing](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)

Azure IoT Hub device-SDK voor C, die zijn geschreven in C ANSI (C99) voor portabiliteit en brede platformcompatibiliteit:

* Installeren van [apt-get, MBED, Arduino IDE of Nuget](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Broncode](https://github.com/Azure/azure-iot-sdk-c)
* [API-naslaginformatie](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
* [Moduleverwijzing](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_module_client.h)

Azure IoT Hub apparaat-SDK voor Java: 

* Toevoegen aan [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk) project
* [Broncode](https://github.com/Azure/azure-iot-sdk-java)
* [API-naslaginformatie](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device)
* [Moduleverwijzing](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._module_client?view=azure-java-stable)

Azure IoT Hub apparaat-SDK voor Node.js: 

* Installeren van [npm](https://www.npmjs.com/package/azure-iot-device)
* [Broncode](https://github.com/Azure/azure-iot-sdk-node)
* [API-naslaginformatie](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest)
* [Moduleverwijzing](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest)

Azure IoT Hub apparaat-SDK voor Python: 

* Installeren van [pip](https://pypi.python.org/pypi/azure-iothub-device-client/)
* [Broncode](https://github.com/Azure/azure-iot-sdk-python)
* API-verwijzing: Zie [C-API-verwijzing](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

Azure IoT Hub apparaat-SDK voor iOS: 

* Installeren van [CocoaPod](https://cocoapods.org/pods/AzureIoTHubClient)
* [Voorbeelden](https://github.com/Azure-Samples/azure-iot-samples-ios)
* API-verwijzing: Zie [C-API-verwijzing](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

> [!NOTE]
> Raadpleeg het Leesmij-bestanden in de GitHub-opslagplaatsen voor informatie over het gebruik van taal en platform-specifieke Pakketbeheer binaire bestanden en afhankelijkheden voor netwerkapparaten installeren op uw ontwikkelcomputer.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>OS-platform en de hardware-compatibiliteit

Ondersteunde platforms voor de SDK's kunnen u vinden in [platformondersteuning voor Azure IoT SDK's](iot-hub-device-sdk-platform-support.md).

Zie voor meer informatie over de compatibiliteit van de SDK met de specifieke hardware, de [Azure Certified voor IoT-apparaatcatalogus](https://catalog.azureiotsuite.com/) of afzonderlijke-opslagplaats.

## <a name="azure-iot-service-sdks"></a>Azure IoT service SDK 's

De Azure IoT service SDK's bevatten code toepassingen ontwikkelen die communiceren rechtstreeks met IoT Hub voor het beheren van apparaten en beveiliging in het kader.

Azure IoT Hub-service SDK voor .NET:

* Downloaden van [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices/)
* [Broncode](https://github.com/Azure/azure-iot-sdk-csharp)
* [API-naslaginformatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices)

Azure IoT Hub-service SDK voor Java: 

* Toevoegen aan [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk
) project
* [Broncode](https://github.com/Azure/azure-iot-sdk-java)
* [API-naslaginformatie](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service)

Azure IoT Hub-service SDK voor Node.js: 

* Downloaden van [npm](https://www.npmjs.com/package/azure-iothub)
* [Broncode](https://github.com/Azure/azure-iot-sdk-node)
* [API-naslaginformatie](https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest)

Azure IoT Hub-service SDK voor Python: 

* Downloaden van [pip](https://pypi.python.org/pypi/azure-iothub-service-client/)
* [Broncode](https://github.com/Azure/azure-iot-sdk-python)

Azure IoT Hub-service SDK voor C: 

* Downloaden van [apt-get, MBED, Arduino IDE of Nuget](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Broncode](https://github.com/Azure/azure-iot-sdk-c)

Azure IoT Hub-service SDK voor iOS: 

* Installeren van [CocoaPod](https://cocoapods.org/pods/AzureIoTHubServiceClient)
* [Voorbeelden](https://github.com/Azure-Samples/azure-iot-samples-ios)

> [!NOTE]
> Raadpleeg het Leesmij-bestanden in de GitHub-opslagplaatsen voor informatie over het gebruik van taal en platform-specifieke Pakketbeheer binaire bestanden en afhankelijkheden voor netwerkapparaten installeren op uw ontwikkelcomputer.

## <a name="device-provisioning-sdks"></a>SDK's voor apparaatinrichting

De **Microsoft Azure Provisioning SDK's** kunt u voor het inrichten van apparaten naar uw IoT-Hub met behulp van de [Device Provisioning Service](../iot-dps/about-iot-dps.md).

Azure Provisioning apparaat- en service-SDK's voor C#:

* [Provisioning device SDK-client](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/device)
* [Provisioning serviceclient SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/service)

Azure Provisioning apparaat- en service-SDK's voor Java:

* [Provisioning device SDK-client](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-device-client)
* [Provisioning serviceclient SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-service-client)

Azure Provisioning apparaat- en service-SDK's voor Node.js:

* [Provisioning device SDK-client](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/device)
* [Provisioning serviceclient SDK](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service)

Azure Provisioning apparaat- en service-SDK's voor Python:

* [Provisioning device SDK-client](https://github.com/Azure/azure-iot-sdk-python/blob/master/provisioning_device_client)
* [Provisioning serviceclient SDK](https://github.com/Azure/azure-iot-sdk-python/tree/master/provisioning_service_client)

Azure Provisioning apparaat- en service-SDK's voor C:

* [Provisioning device SDK-client](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [Provisioning serviceclient SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_service_client)

## <a name="next-steps"></a>Volgende stappen

Azure IoT SDK's bieden ook een set hulpprogramma's om te helpen bij de ontwikkeling:
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics): een platformoverschrijdende opdrachtregel-hulpprogramma om problemen vast te stellen met betrekking tot verbinding met IoT Hub.
* [apparatenverkenner](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer): een Windows-bureaubladtoepassing verbinding maken met uw IoT-Hub.

Er zijn andere onderwerpen met naslaginformatie in deze IoT Hub developer guide:

* [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md)
* [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering](iot-hub-devguide-query-language.md)
* [Quota en beperkingen](iot-hub-devguide-quotas-throttling.md)
* [IoT Hub MQTT-ondersteuning](iot-hub-mqtt-support.md)
* [IoT Hub REST API-naslaginformatie](/rest/api/iothub/)
* [Ondersteuning voor het platform van Azure IoT-SDK](iot-hub-device-sdk-platform-support.md)

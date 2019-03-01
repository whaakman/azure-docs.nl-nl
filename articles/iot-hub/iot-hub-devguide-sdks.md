---
title: De Azure IoT SDK's begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - informatie over en koppelingen naar de verschillende Azure IoT-apparaat en service-SDK's die u gebruiken kunt om apps op apparaten en back-end-apps te bouwen.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/14/2018
ms.openlocfilehash: fcb6f564c97632e4d62f7f052ec5ecc35a9e479a
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57011242"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Begrijpen en gebruiken van Azure IoT Hub SDK 's

Er zijn twee soorten software development kits (SDK's) voor het werken met IoT Hub:

* **IoT Hub apparaat-SDK's** kunt u het bouwen van apps die worden uitgevoerd op uw IoT-apparaten met behulp van apparaatclient of module. Deze apps verzenden van telemetrie naar uw IoT-hub en eventueel berichten, taak, methode of dubbele updates ontvangen van uw IoT-hub.  U kunt ook module client schrijven [modules](../iot-edge/iot-edge-modules.md) voor [Azure IoT Edge-runtime](../iot-edge/about-iot-edge.md).

* **IoT Hub-Service-SDK's** inschakelen u back endtoepassingen voor het beheren van uw IoT-hub en (optioneel) het verzenden van berichten, plannen van taken, directe methoden aanroepen of gewenste eigenschap updates verzenden naar uw IoT-apparaten of modules.

Bovendien we bieden ook een set SDK's voor het werken met de [Device Provisioning Service](../iot-dps/about-iot-dps.md).
* **Apparaat-SDK's inrichten** kunt u het bouwen van apps die worden uitgevoerd op uw IoT-apparaten om te communiceren met de Device Provisioning Service.

* **Service-SDK's inrichten** kunt u back endtoepassingen bouwen voor uw registraties in de Device Provisioning Service beheren.

Meer informatie over de [voordelen van het ontwikkelen met behulp van Azure IoT SDK's](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]


### <a name="os-platform-and-hardware-compatibility"></a>OS-platform en de hardware-compatibiliteit

Ondersteunde platforms voor de SDK's kunnen u vinden in [platformondersteuning voor Azure IoT SDK's](iot-hub-device-sdk-platform-support.md).

Zie voor meer informatie over de compatibiliteit van de SDK met de specifieke hardware, de [Azure Certified voor IoT-apparaatcatalogus](https://catalog.azureiotsolutions.com/) of afzonderlijke-opslagplaats.

## <a name="azure-iot-hub-device-sdks"></a>Azure IoT Hub apparaat-SDK 's

De Microsoft Azure IoT device SDK's bevatten code die vereenvoudigt het uitvoeren van toepassingen ontwikkelen die verbinding maken met en worden beheerd door Azure IoT Hub-services.

Azure IoT Hub apparaat-SDK voor .NET: 

* Downloaden van [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/).  De naamruimte is Microsoft.Azure.Devices.Clients die IoT Hub apparaat-Clients (DeviceClient, ModuleClient) bevat.
* [Broncode](https://github.com/Azure/azure-iot-sdk-csharp)
* [API-naslaginformatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [Moduleverwijzing](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)

Azure IoT Hub device-SDK voor C (ANSI C - C99):

* Installeren van [apt-get, MBED, Arduino IDE of iOS](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md#packages-and-libraries)
* [Broncode](https://github.com/Azure/azure-iot-sdk-c)
* [Compileren van de C-apparaat-SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#compiling-the-c-device-sdk)
* [API-naslaginformatie](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
* [Moduleverwijzing](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h)
* [Overzetten van de C-SDK op andere platforms](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)
* [Documentatie voor ontwikkelaars](https://github.com/Azure/azure-iot-sdk-c/tree/master/doc) voor informatie over cross-compileren, aan de slag op verschillende platforms, enzovoort.

Azure IoT Hub apparaat-SDK voor Java: 

* Toevoegen aan [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk) project
* [Broncode](https://github.com/Azure/azure-iot-sdk-java)
* [API-naslaginformatie](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device)
* [Moduleverwijzing](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable)

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

## <a name="azure-iot-hub-service-sdks"></a>Azure IoT Hub-Service-SDK 's

De Azure IoT service SDK's bevatten code toepassingen ontwikkelen die communiceren rechtstreeks met IoT Hub voor het beheren van apparaten en beveiliging in het kader.

Azure IoT Hub-service SDK voor .NET:

* Downloaden van [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices/).  De naamruimte is Microsoft.Azure.Devices die IoT Hub-Service-Clients (RegistryManager, ServiceClients) bevat.
* [Broncode](https://github.com/Azure/azure-iot-sdk-csharp)
* [API-naslaginformatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices)

Azure IoT Hub-service SDK voor Java: 

* Toevoegen aan [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk) project
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

## <a name="microsoft-azure-provisioning-sdks"></a>Microsoft Azure Provisioning SDK 's

De **Microsoft Azure Provisioning SDK's** kunt u voor het inrichten van apparaten naar uw IoT-Hub met behulp van de [Device Provisioning Service](../iot-dps/about-iot-dps.md).

Azure Provisioning apparaat- en service-SDK's voor C#:

* Downloaden van [apparaat-SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Client/) en [SDK-Service](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) vanuit NuGet.
* [Broncode](https://github.com/Azure/azure-iot-sdk-csharp/)
* [API-naslaginformatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.provisioning.client?view=azure-dotnet)

Azure Provisioning apparaat- en service-SDK's voor C:

* Installeren van [apt-get, MBED, Arduino IDE of iOS](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md#packages-and-libraries)
* [Broncode](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [API-naslaginformatie](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

Azure Provisioning apparaat- en service-SDK's voor Java:

* Toevoegen aan [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk) project
* [Broncode](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning)
* [API-naslaginformatie](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.provisioning.device?view=azure-java-stable)

Azure Provisioning apparaat- en service-SDK's voor Node.js:

* [Broncode](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning)
* [API-naslaginformatie](https://docs.microsoft.com/javascript/api/overview/azure/iothubdeviceprovisioning?view=azure-node-latest)
* Download [Device SDK](https://badge.fury.io/js/azure-iot-provisioning-device) en [Service SDK](https://badge.fury.io/js/azure-iot-provisioning-service) vanuit npm

Azure Provisioning apparaat- en service-SDK's voor Python:

* [Broncode](https://github.com/Azure/azure-iot-sdk-python)
* Download [Device SDK](https://pypi.org/project/azure-iot-provisioning-device-client/) en [Service SDK](https://pypi.org/project/azure-iothub-provisioningserviceclient/) van pip

## <a name="next-steps"></a>Volgende stappen

Azure IoT SDK's bieden ook een set hulpprogramma's om te helpen bij de ontwikkeling:
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics): een platformoverschrijdende opdrachtregel-hulpprogramma om problemen vast te stellen met betrekking tot verbinding met IoT Hub.
* [apparatenverkenner](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer): een Windows-bureaubladtoepassing verbinding maken met uw IoT-Hub.

Relevante docs met betrekking tot het ontwikkelen met behulp van de Azure IoT SDK's:
* Meer informatie over [over het beheren van verbindingen en betrouwbare berichten](iot-hub-reliability-features-in-sdks.md) met behulp van de IoT Hub SDK's.
* Meer informatie over het [ontwikkelen voor mobiele platforms](iot-hub-how-to-develop-for-mobile-devices.md) , zoals iOS en Android.
* [Ondersteuning voor het platform van Azure IoT-SDK](iot-hub-device-sdk-platform-support.md)


Er zijn andere onderwerpen met naslaginformatie in deze IoT Hub developer guide:

* [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md)
* [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering](iot-hub-devguide-query-language.md)
* [Quota en beperkingen](iot-hub-devguide-quotas-throttling.md)
* [IoT Hub MQTT-ondersteuning](iot-hub-mqtt-support.md)
* [IoT Hub REST API-naslaginformatie](/rest/api/iothub/)

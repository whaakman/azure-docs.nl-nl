---
title: Ontwikkelen voor mobiele apparaten met behulp van Azure IoT SDK's | Microsoft Docs
description: "Developer guide: meer informatie over het ontwikkelen voor mobiele apparaten met behulp van Azure IoT Hub SDK's."
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: yizhon
ms.openlocfilehash: 4a94abe69b525dc1b03fe2c1ae9593f3c6399f56
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339750"
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Ontwikkelen voor mobiele apparaten met behulp van Azure IoT SDK 's

Dingen die u in de praktijk kunnen verwijzen naar een breed scala aan apparaten met verschillende mogelijkheden: sensoren, microcontrollers, slimme apparaten, industriële gateways en zelfs mobiele apparaten.  Een mobiel apparaat mag een IoT-apparaat, waar deze apparaat-naar-cloud telemetrie wordt verzonden en beheerd door de cloud.  Het uitvoeren van een back-end-service-toepassing die andere IoT-apparaten worden beheerd apparaat kan ook zijn.  In beide gevallen [Azure IoT Hub SDK's](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) kan worden gebruikt voor het ontwikkelen van toepassingen die voor mobiele apparaten werken.  

## <a name="develop-for-native-ios-platform"></a>Ontwikkelen voor systeemeigen iOS-platform

Azure IoT Hub SDK's bieden ondersteuning voor systeemeigen iOS-platform via C-SDK van Azure IoT Hub.  U kunt deze beschouwen als een iOS-SDK die u in uw Swift of Objective C XCode-project opnemen kunt.  Er zijn twee manieren waarop u met de C-SDK voor iOS:

* De CocoaPod-bibliotheken in XCode-project rechtstreeks gebruiken.  

* De broncode voor C-SDK downloaden en bouwen voor iOS-platform na de [bouwen instructie](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) voor MacOS.  

Azure IoT Hub C-SDK is geschreven in C99 voor maximale plaatsingsmogelijkheden op verschillende platforms.  De overzetten proces omvat het schrijven van een thin acceptatie-laag voor de platform-specifieke onderdelen, die u hier kunt vinden voor [iOS](https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx).  De functies in de C-SDK kunnen worden gebruikt op iOS-platform, met inbegrip van de Azure IoT Hub-primitieven ondersteund en SDK-specifieke functies zoals opnieuw proberen het beleid voor netwerk-betrouwbaarheid.  De interface voor iOS SDK is ook vergelijkbaar met de interface voor C-SDK van Azure IoT Hub.  

Deze documentatie helpen bij het ontwikkelen van een, apparaattoepassing of service-toepassing op een iOS-apparaat:

* [Snelstartgids: Telemetrie verzenden van een apparaat naar een IoT-hub](quickstart-send-telemetry-ios.md)  

* [Berichten verzenden vanuit de cloud naar uw apparaat met IoT hub](iot-hub-ios-swift-c2d.md) 

## <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Ontwikkelen met Azure IoT Hub CocoaPod bibliotheken

Azure IoT Hub SDK's-releases een reeks CocoaPod Objective-C-bibliotheken voor iOS-ontwikkeling.  Zie voor de meest recente lijst CocoaPod bibliotheken [CocoaPods voor Microsoft Azure IoT](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md).  Zodra de relevante bibliotheken zijn opgenomen in uw XCode-project, zijn er twee manieren om te schrijven met dat IOT Hub gerelateerde code:

* Objective-C-functie: als uw project is geschreven in Objective-C, kunt u API's aanroepen vanuit de C-SDK van Azure IoT Hub rechtstreeks.  Als uw project is geschreven in Swift, kunt u bellen `@objc func` voor het maken van uw functie en gaat u verder met het schrijven van alle logics met betrekking tot Azure IoT Hub met behulp van C of Objective-C-code.  Een aantal voorbeelden van beide kan worden gevonden in de [voorbeeldopslagplaats](https://github.com/Azure-Samples/azure-iot-samples-ios).  

* Gebruikmaken van C-voorbeelden: als u een toepassing C-apparaat hebt geschreven, u ernaar kunt verwijzen rechtstreeks in uw XCode-project:

    * Het bestand sample.c toevoegen aan uw XCode-project in XCode.  
    
    * De headerbestand toevoegen aan uw afhankelijkheid.  Een header-bestand is opgenomen in de [voorbeeldopslagplaats](https://github.com/Azure-Samples/azure-iot-samples-ios) als voorbeeld. Voor meer informatie gaat u naar de documentatiepagina van Apple voor [Objective-C](https://developer.apple.com/documentation/objectivec).

## <a name="next-steps"></a>Volgende stappen

* [IoT Hub REST API-naslaginformatie](https://docs.microsoft.com/rest/api/iothub/)
* [Azure IoT C SDK-broncode](https://github.com/Azure/azure-iot-sdk-c)

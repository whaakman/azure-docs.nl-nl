---
title: Ontwikkelen voor mobiele apparaten met behulp van Azure IoT SDK's | Microsoft Docs
description: Ontwikkelaars begeleiden - informatie over het ontwikkelen voor mobiele apparaten met Azure IoT Hub SDK's.
author: yzhong94
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: yizhon
ms.openlocfilehash: 9c4b4a61698556f14a6362984b04e5cc409b5763
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634944"
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Ontwikkelen voor mobiele apparaten met behulp van Azure IoT SDK 's

Dingen in het Internet der dingen kunnen verwijzen naar een breed scala aan apparaten met verschillende mogelijkheden: sensoren, microcontrollers smart-apparaten, industriële gateways en zelfs de mobiele apparaten.  Een mobiel apparaat mag een IoT-apparaat, waar het apparaat-naar-cloud telemetrie verzendt en beheerd door de cloud.  Het kan ook worden het apparaat met een back-end-service-toepassing die andere IoT-apparaten beheert.  In beide gevallen [Azure IoT Hub SDK's] [ lnk-sdk-overview] kan worden gebruikt voor het ontwikkelen van toepassingen die voor mobiele apparaten werken.  

## <a name="develop-for-native-ios-platform"></a>Ontwikkelen voor systeemeigen iOS-platform

Azure IoT Hub SDK's bieden ondersteuning voor systeemeigen iOS-platform via Azure IoT Hub C SDK.  U kunt dit beschouwen als een iOS-SDK die u in uw Swift of Objective C XCode-project opnemen kunt te gebruiken.  Er zijn twee manieren naar de C-SDK voor iOS gebruiken:
- De CocoaPod-bibliotheken in XCode-project rechtstreeks gebruiken.  
- Download de broncode voor C-SDK en bouwen voor iOS-platform na de [bouwen instructie] [ lnk-c-devbox] voor Mac OS.  

Azure IoT Hub C SDK is geschreven in C99 voor maximale draagbaarheid op verschillende platforms.  Schrijven van een thin acceptatie-laag voor de platform-specifieke onderdelen, die u hier kunt vinden voor het overzetten proces omvat het [iOS][lnk-ios-pal].  De functies in de C-SDK kunnen worden gebruikt op iOS-platform, met inbegrip van de Azure IoT Hub primitieven ondersteund en SDK-specifieke functies zoals probeer het beleid voor de betrouwbaarheid van het netwerk.  De interface voor iOS SDK is ook vergelijkbaar met de interface voor C-SDK van Azure IoT Hub.  

Deze documentatie helpt bij het ontwikkelen van een apparaattoepassing of servicetoepassing op een iOS-apparaat:
- [Snelstartgids: Verzenden van telemetrie vanaf een apparaat naar een IoT-hub][lnk-device-ios-quickstart]  
- [Verzenden van berichten vanuit de cloud op uw apparaat met iothub][lnk-service-ios-quickstart]  

### <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Ontwikkelen met Azure IoT Hub CocoaPod bibliotheken

Azure IoT Hub SDK's versies een set van Objective-C CocoaPod bibliotheken voor iOS-ontwikkeling.  Zie voor de meest recente lijst met CocoaPod bibliotheken [CocoaPods voor Microsoft Azure IoT][lnk-cocoapod-list].  Zodra de relevante bibliotheken zijn opgenomen in uw XCode-project, zijn er twee manieren om te schrijven dat IOT Hub gerelateerde code:
- De functie objective-C: als uw project is geschreven in Objective-C, kunt u API's direct aanroepen vanuit Azure IoT Hub C SDK.  Als uw project is geschreven in Swift, kunt u bellen ``@objc func`` voordat uw functie te maken en doorgaan met het schrijven van alle logics die betrekking hebben op Azure IoT Hub met C of Objective-C-code.  Een reeks voorbeelden demonstreren van beide vindt u in de [voorbeeld opslagplaats][lnk-ios-samples-repo].  
- Gebruikmaken van C voorbeelden: als u een toepassing C-apparaat hebt geschreven, u kunt verwijzen naar deze rechtstreeks in uw XCode-project:
    - Het bestand sample.c toevoegen aan uw XCode-project in XCode.  
    - Het headerbestand toevoegen aan uw afhankelijkheid.  Een header-bestand is opgenomen in de [voorbeeld opslagplaats] [ lnk-ios-samples-repo] als voorbeeld.  Voor meer informatie gaat u naar pagina met Apple documentatie voor [Objective-C](https://developer.apple.com/documentation/objectivec).

## <a name="next-steps"></a>Volgende stappen
* [IoT Hub REST API-referentiemateriaal][lnk-rest-ref]
* [Azure IoT C SDK broncode][lnk-c-sdk]

[lnk-sdk-overview]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks
[lnk-c-devbox]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-device-ios-quickstart]:https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-ios
[lnk-service-ios-quickstart]: https://docs.microsoft.com/azure/iot-hub/iot-hub-ios-swift-c2d
[lnk-cocoapod-list]: https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md
[lnk-ios-samples-repo]: https://github.com/Azure-Samples/azure-iot-samples-ios
[lnk-ios-pal]: https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx
[lnk-c-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/

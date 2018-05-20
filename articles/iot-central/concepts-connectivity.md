---
title: Connectiviteit van apparaten in Azure IoT centrale | Microsoft Docs
description: Dit artikel bevat belangrijke concepten met betrekking tot connectiviteit van apparaten in Azure IoT centrale
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 00b621a4635ef1ceda26772ac5876fa2599b56f8
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="device-connectivity-in-azure-iot-central"></a>Connectiviteit van apparaten in Azure IoT centrale

Dit artikel bevat belangrijke concepten met betrekking tot connectiviteit van apparaten in Microsoft Azure IoT Central.

Alle apparaten die verbinding maakt met uw Azure IoT centrale toepassing verbinding maakt met de [eindpunten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints) die door de IoT-hub maakt gebruik van Azure IoT centrale beschikbaar worden gesteld. IoT-Hub kunt schaalbare, veilige en betrouwbare verbindingen van uw verbonden apparaten.

## <a name="sdk-support"></a>SDK-ondersteuning

De apparaat-SDK's van Azure-aanbieding de eenvoudigste manier voor u de code op uw apparaten die verbinding maakt met uw Azure IoT centrale toepassing implementeren. Het volgende apparaat-SDK's zijn beschikbaar:

- [Azure IoT SDK voor C](https://github.com/azure/azure-iot-sdk-c)
- [Azure IoT SDK voor Python](https://github.com/azure/azure-iot-sdk-python)
- [Azure IoT SDK voor Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Azure IoT SDK voor Java](https://github.com/azure/azure-iot-sdk-java)
- [Azure IoT SDK voor .NET](https://github.com/azure/azure-iot-sdk-csharp)

Elk apparaat verbinding maakt met een unieke verbindingsreeks dat het apparaat identificeert. Een apparaat kan alleen verbinding maken met de IoT-hub waar deze is geregistreerd. Wanneer u een echte apparaat in uw Azure IoT centrale toepassing maakt, genereert de toepassing een verbindingsreeks kunt gebruiken.

## <a name="sdk-features-and-iot-hub-connectivity"></a>SDK-functies en connectiviteit van IoT-Hub

Alle communicatie met IoT-Hub maakt gebruik van de volgende opties voor netwerkconnectiviteit van IoT-Hub:

- [Apparaat-naar-cloud-berichten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Apparaat horende](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

De volgende tabel ziet u hoe Azure IoT centrale apparaatfuncties toegewezen doorsturen naar IoT Hub-functies:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Meting: telemetrie | Apparaat-naar-cloud-berichten |
| Apparaateigenschappen | Apparaat-twin gerapporteerd eigenschappen |
| Instellingen | Apparaat-twin gewenst en eigenschappen gerapporteerd |

Zie voor meer informatie over het gebruik van de apparaat-SDK's, een van de volgende artikelen bijvoorbeeld code:

- [Een algemene Node.js-client verbindt met uw Azure IoT centrale toepassing](howto-connect-nodejs.md)
- [Een frambozen Pi apparaat aansluit op uw Azure IoT centrale toepassing](howto-connect-raspberry-pi-python.md)
- [Een DevDiv kit apparaat aansluit op uw Azure IoT centrale toepassing](howto-connect-devkit.md).

De volgende video biedt een overzicht van een echte apparaat verbinding te maken met Azure IoT centrale:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Connect-real-devices-to-Microsoft-IoT-Central/Player]

## <a name="protocols"></a>Protocollen

De apparaat-SDK's ondersteunen de volgende netwerkprotocollen om verbinding te maken naar een iothub:

- MQTT
- AMQP
- HTTPS

Zie voor meer informatie over deze verschil protocollen en richtlijnen over het kiezen van een [kiezen een communicatieprotocol](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Als uw apparaat kan niet een van de ondersteunde protocollen, kunt u Azure IoT rand bij conversie protocol. IoT-rand ondersteunt andere intelligence op de edge-scenario's voor verwerking naar de rand van de Azure IoT centrale toepassing offloaden.

## <a name="security"></a>Beveiliging

Alle gegevens die worden uitgewisseld tussen apparaten en uw Azure IoT centrale is versleuteld. IoT Hub verifieert elke aanvraag naar een apparaat dat verbinding met een van de IoT Hub apparaat gerichte eindpunten maakt. Om te voorkomen referenties uitwisselen via de kabel, een apparaat ondertekende tokens voor verificatie gebruikt. Voor meer informatie ziet, [toegangsbeheer met IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> Apparaten die verbinding met Azure IoT centrale maken moeten op dit moment wordt de SAS-tokens gebruiken. X.509-certificaten worden niet ondersteund voor apparaten die verbinding met Azure IoT centrale maken.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd over connectiviteit van apparaten in Azure IoT centraal, vindt hier u de voorgestelde volgende stappen uit:

- [Bereid en verbinding maken met een apparaat DevKit](howto-connect-devkit.md)
- [Bereid en verbinding maken met een Pi frambozen](howto-connect-raspberry-pi-python.md)
- [Een algemene Node.js-client verbindt met uw Azure IoT centrale toepassing](howto-connect-nodejs.md)

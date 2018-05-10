---
title: Azure IoT Hub - aan de slag IoT-apparaten verbinding laten maken met de cloud | Microsoft Docs
description: Meer informatie over het aansluiten van uw IoT-boards en starter Kit op Azure IoT Hub. Uw apparaten kunnen verzenden telemetrie naar IoT Hub en IoT-Hub kunt controleren en beheren van uw apparaten.
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
keywords: Azure iot hub-zelfstudie
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: dafb8aca34a5a41f45f76d526aa3b8f3b1b792c4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="azure-iot-hub-get-started-with-real-devices"></a>Azure IoT-Hub aan de slag met echte apparaten

U kunt Azure IoT Hub en het apparaat met Azure IoT SDK's gebruiken om oplossingen voor Internet der dingen (IoT) te maken:

* Azure IoT Hub is een volledig beheerde service in de cloud die veilig verbindt, bewaakt en beheert uw IoT-apparaten. Gebruik de Azure IoT-apparaat-SDK's voor het implementeren van uw IoT-apparaten.
* Gebruik een IoT-gateway in complexere IoT-scenario's. Bijvoorbeeld, waarbij u moet rekening houden factoren zoals de oudere apparaten, kosten van bandbreedte, beveiliging en privacy-beleid of gegevensverwerking rand. In deze scenario's, gebruikt u [Azure IoT rand](https://docs.microsoft.com/azure/iot-edge/) voor het implementeren van een gateway die apparaten verbinding met uw IoT-hub.

## <a name="what-the-how-to-articles-cover"></a>Wat de artikelen met procedures hebben betrekking op

Deze artikelen vindt u Azure IoT Hub en het apparaat-SDK's. De artikelen hebben betrekking op algemene IoT-scenario's om aan te tonen van de mogelijkheden van IoT-Hub. De artikelen ook te laten zien hoe IoT Hub worden gecombineerd met andere Azure-services en hulpprogramma's voor het bouwen van krachtige IoT-oplossingen. In de artikelen gebruikt u echte IoT-apparaten.

## <a name="set-up-your-device"></a>Uw apparaat instellen

Verbinding maken met een IoT-apparaat of de gateway naar Azure IoT Hub:

| IoT-apparaat                       | Programmeertaal |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                       | [Arduino in VSCode][DevKit]     |
| Intel Edison                     | [Node.js][Ed_Nd], [C][Ed_C]    |
| Adafruit Doezelaar HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 ding Dev       | [Arduino][Th_Ard]              |
| Adafruit Doezelaar M0              | [Arduino][M0_Ard]              |
| Online apparaatsimulator         | [Raspberry Pi (Node.js)][Ol_Sim] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md

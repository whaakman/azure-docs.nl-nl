---
title: Fysieke apparaten verbinden met Azure IoT Hub aan de slag | Microsoft Docs
description: Informatie over het aansluiten van fysieke apparaten en boards met Azure IoT Hub. Uw apparaten kunnen verzenden telemetrie naar IoT Hub en IoT-Hub kunt controleren en beheren van uw apparaten.
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: Azure iot hub-zelfstudie
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: f4128b6b049aa876e170c56dcf2e40720644dc3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a>Azure IoT-Hub aan de slag met zelfstudies voor fysieke apparaten

Deze zelfstudies vindt u Azure IoT Hub en het apparaat-SDK's. De zelfstudies betrekking hebben op algemene IoT-scenario's om aan te tonen van de mogelijkheden van IoT-Hub. De zelfstudies ook te laten zien hoe IoT Hub worden gecombineerd met andere Azure-services en hulpprogramma's voor het bouwen van krachtige IoT-oplossingen. De zelfstudies die worden vermeld in de volgende tabel beschreven hoe u voor het maken van fysieke IoT-apparaten.

| IoT-apparaat                       | Programmeertaal |
|---------------------------------|----------------------|
| Raspberry Pi                    | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                      | [Arduino in VSCode][DevKit]     |
| Intel Edison                    | [Node.js][Ed_Nd], [C][Ed_C]           |
| Adafruit Doezelaar HUZZAH ESP8266 | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 ding Dev      | [Arduino][Th_Ard]              |
| Adafruit Doezelaar M0             | [Arduino][M0_Ard]              |

Bovendien kunt u een gateway aan de rand van de IoT waarmee apparaten verbinding maken met uw IoT-hub.

| Gateway-apparaat               | Programmeertaal | Platform         |
|------------------------------|----------------------|------------------|
| Intel NUC (model DE3815TYKE) | C                    | [De o Linux][NUC_Lnx] |

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
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md

---
title: Verbinding maken met een Pi frambozen Azure IoT Suite met C met echte sensoren | Microsoft Docs
description: Gebruik de Microsoft Azure IoT Starter Kit voor de Raspberry Pi 3 en Azure IoT Suite. Gebruik C verbinding maken met uw frambozen-Pi de oplossing voor externe controle verzenden van telemetrie van sensoren naar de cloud, en reageren op de methoden die worden aangeroepen vanuit het dashboard van oplossing.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 635eb9d4e85eaf43dc83f2bd5a0d0f7c32620d98
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-c"></a>Uw frambozen Pi 3 verbinding met de oplossing voor externe controle en verzend telemetrie vanuit een echte sensor met C

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Deze zelfstudie laat zien hoe de Microsoft Azure IoT Starter Kit voor frambozen Pi 3 gebruiken voor het ontwikkelen van een temperatuur en vochtigheid lezer die met de cloud communiceren kan. De zelfstudie wordt gebruikt:

- Raspbian OS, de programmeertaal en Microsoft Azure IoT SDK voor C voor het implementeren van een voorbeeld-apparaat.
- IoT Suite remote monitoring vooraf geconfigureerde oplossing als de cloud-gebaseerde back-end.

## <a name="overview"></a>Overzicht

In deze zelfstudie maakt uitvoeren u de volgende stappen:

- Implementeer een exemplaar van de vooraf geconfigureerde oplossing voor externe controle op uw Azure-abonnement. Deze stap implementeert automatisch en meerdere Azure-services configureert.
- Instellen van het apparaat en de sensoren om te communiceren met uw computer en de oplossing voor externe controle.
- Werk de voorbeeldcode van de apparaten verbinding maken met de oplossing voor externe controle en verzenden van telemetrie die u op het dashboard van de oplossing weergeven kunt.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prerequisites](../../includes/iot-suite-v1-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> De oplossing voor externe controle levert een set van Azure-services in uw Azure-abonnement. De implementatie duidt op een echte enterprise-architectuur. Om te voorkomen dat een Azure-verbruik onnodige kosten, verwijdert u uw exemplaar van de vooraf geconfigureerde oplossing op azureiotsuite.com wanneer u klaar bent met het. Als u de vooraf geconfigureerde oplossing meer nodig hebt, kunt u het eenvoudig opnieuw. Zie voor meer informatie over het verbruik verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor demonstratiedoeleinden][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a>Downloaden en configureren van de steekproef

U kunt nu downloaden en configureren van de externe clienttoepassing bewaking op uw frambozen Pi.

### <a name="clone-the-repositories"></a>De opslagplaatsen klonen

Als u dit nog niet hebt gedaan, moet u de vereiste opslagplaatsen klonen met de volgende opdrachten in een terminal op uw Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
git clone --recursive https://github.com/WiringPi/WiringPi.git
```

### <a name="update-the-device-connection-string"></a>De verbindingsreeks apparaat bijwerken

Open het voorbeeld-bronbestand in de **nano** editor met de volgende opdracht:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/remote_monitoring/remote_monitoring.c
```

Zoek de volgende regels:

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

Vervang de tijdelijke aanduiding voor waarden met het apparaat en IoT Hub informatie u gemaakt en opgeslagen aan het begin van deze zelfstudie. Sla de wijzigingen (**Ctrl-O**, **Enter**) en sluit de editor af (**Ctrl X**).

## <a name="build-the-sample"></a>Het voorbeeld bouwen

De vereiste pakketten voor de Microsoft Azure IoT-apparaat-SDK voor C met de volgende opdrachten in een terminal op de frambozen Pi installeren:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

U kunt nu de bijgewerkte oplossing bouwen op de Pi frambozen:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
```

U kunt nu het voorbeeldprogramma uitvoeren op de frambozen Pi. Voer de opdracht:

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

De volgende voorbeelduitvoer volgt een voorbeeld van de uitvoer die u bij de opdrachtprompt op de Pi frambozen zien:

![De uitvoer van de app Raspberry Pi][img-raspberry-output]

Druk op **Ctrl-C** om af te sluiten van het programma op elk gewenst moment.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) voor meer voorbeelden en documentatie over Azure IoT.

[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-basic/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

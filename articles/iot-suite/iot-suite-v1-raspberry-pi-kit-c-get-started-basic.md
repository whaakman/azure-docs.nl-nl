---
title: Verbinding maken met een Raspberry Pi met C met echte sensoren van Azure IoT Suite | Microsoft Docs
description: Gebruik de Microsoft Azure IoT Starter Kit voor de Raspberry Pi 3 en Azure IoT Suite. Gebruik C uw Raspberry Pi verbinden met de oplossing voor externe controle, telemetrie van sensors verzenden naar de cloud en reageren op methoden aangeroepen vanuit het dashboard van de oplossing.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 635eb9d4e85eaf43dc83f2bd5a0d0f7c32620d98
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611304"
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-c"></a>Verbinding maken met uw Raspberry Pi 3 op de oplossing voor externe controle en telemetrie verzendt van een echte sensor met c#

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Deze zelfstudie leert u hoe u kunt de Microsoft Azure IoT Starter Kit voor Raspberry Pi 3 gebruiken voor het ontwikkelen van een temperatuur en vochtigheid-reader die met de cloud communiceren kan. De zelfstudie wordt gebruikgemaakt van:

- Raspbian OS-, de programmeertaal en de Microsoft Azure IoT SDK voor C voor het implementeren van een voorbeeld-apparaat.
- Externe controle met IoT Suite vooraf geconfigureerde oplossing als de cloud-gebaseerde back-end.

## <a name="overview"></a>Overzicht

In deze zelfstudie voert u de volgende stappen uit:

- Implementeer een exemplaar van de vooraf geconfigureerde oplossing voor externe controle met uw Azure-abonnement. Deze stap automatisch implementeert en configureert u meerdere Azure-services.
- Instellen van uw apparaten en sensoren te communiceren met uw computer en de oplossing voor externe controle.
- Bijwerken van de voorbeeldcode van de apparaten verbinding maken met de oplossing voor externe controle en telemetrie die u op het dashboard van de oplossing weergeven kunt verzendt.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prerequisites](../../includes/iot-suite-v1-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> De oplossing voor externe controle inricht een set Azure-services in uw Azure-abonnement. De implementatie is inclusief een echte bedrijfsarchitectuur. Onnodige Azure-verbruik om kosten te voorkomen, verwijdert u uw exemplaar van de vooraf geconfigureerde oplossing op azureiotsuite.com wanneer u klaar bent met het. Als u de vooraf geconfigureerde oplossing meer nodig hebt, kunt u deze eenvoudig opnieuw. Zie voor meer informatie over het verbruik te verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor deze demo][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a>Downloaden en configureren van het voorbeeld

U kunt nu downloaden en configureren van de externe clienttoepassing bewaking op uw Raspberry Pi.

### <a name="clone-the-repositories"></a>Klonen van de opslagplaatsen

Als u dit nog niet hebt gedaan, moet u de vereiste opslagplaatsen klonen door het uitvoeren van de volgende opdrachten in een terminalvenster op uw Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
git clone --recursive https://github.com/WiringPi/WiringPi.git
```

### <a name="update-the-device-connection-string"></a>De verbindingsreeks van het apparaat bijwerken

Open het bronbestand voorbeeld in de **nano** editor met behulp van de volgende opdracht uit:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/remote_monitoring/remote_monitoring.c
```

Zoek de volgende regels:

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

Vervang de tijdelijke aanduiding door het apparaat en IoT Hub gegevens die u hebt gemaakt en opgeslagen aan het begin van deze zelfstudie. Sla de wijzigingen (**Ctrl-O**, **Enter**) en de editor af te sluiten (**Ctrl X**).

## <a name="build-the-sample"></a>Het voorbeeld maken

De vereiste pakketten voor de Microsoft Azure IoT Device-SDK voor C door het uitvoeren van de volgende opdrachten in een terminal op de Raspberry Pi installeren:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

U kunt nu de bijgewerkte Voorbeeldoplossing voortbouwen op de Raspberry Pi:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
```

U kunt nu het voorbeeldprogramma uitvoeren op de Raspberry Pi. Voer de opdracht:

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

De volgende voorbeelduitvoer wordt een voorbeeld van de uitvoer u bij de opdrachtprompt op de Raspberry Pi ziet:

![Uitvoer van de app Raspberry Pi][img-raspberry-output]

Druk op **Ctrl-C** om het programma op elk gewenst moment af te sluiten.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure IoT-ontwikkelaarscentrum](https://azure.microsoft.com/develop/iot/) voor meer voorbeelden en documentatie over Azure IoT.

[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-basic/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

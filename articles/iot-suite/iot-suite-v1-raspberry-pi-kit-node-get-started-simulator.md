---
title: Verbinding maken met een Raspberry Pi met behulp van Node.js met gesimuleerde telemetrie van Azure IoT Suite | Microsoft Docs
description: Gebruik de Microsoft Azure IoT Starter Kit voor de Raspberry Pi 3 en Azure IoT Suite. Node.js gebruiken om te verbinden met uw Raspberry Pi de oplossing voor externe controle gesimuleerde telemetrie verzenden naar de cloud en reageren op methoden aangeroepen vanuit het dashboard van de oplossing.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-suite
ms.devlang: node.js
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 53297049fd36eae3839c6a8146afc336b8f5cd02
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38676484"
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a>Verbinding maken met uw Raspberry Pi 3 op de oplossing voor externe controle en verzendt u gesimuleerde telemetrie met behulp van Node.js

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Deze zelfstudie leert u hoe u kunt de Raspberry Pi 3 gebruiken om te simuleren temperatuur en vochtigheid gegevens worden verzonden naar de cloud. De zelfstudie wordt gebruikgemaakt van:

- Raspbian OS-, de programmeertaal Node.js en de Microsoft Azure IoT SDK voor Node.js voor het implementeren van een voorbeeld-apparaat.
- Externe controle met IoT Suite vooraf geconfigureerde oplossing als de cloud-gebaseerde back-end.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> De oplossing voor externe controle inricht een set Azure-services in uw Azure-abonnement. De implementatie is inclusief een echte bedrijfsarchitectuur. Onnodige Azure-verbruik om kosten te voorkomen, verwijdert u uw exemplaar van de vooraf geconfigureerde oplossing op azureiotsuite.com wanneer u klaar bent met het. Als u de vooraf geconfigureerde oplossing meer nodig hebt, kunt u deze eenvoudig opnieuw. Zie voor meer informatie over het verbruik te verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor deze demo][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-the-sample"></a>Downloaden en configureren van het voorbeeld

U kunt nu downloaden en configureren van de externe clienttoepassing bewaking op uw Raspberry Pi.

### <a name="install-nodejs"></a>Node.js installeren

Als u dit nog niet hebt gedaan, installeert u Node.js op uw Raspberry Pi. De IoT-SDK voor Node.js is versie 0.11.5 van Node.js of hoger vereist. De volgende stappen laten zien hoe u Node.js v6.10.2 op uw Raspberry Pi installeren:

1. Gebruik de volgende opdracht uit om bij te werken uw Raspberry Pi:

    ```sh
    sudo apt-get update
    ```

1. Gebruik de volgende opdracht om te downloaden van de Node.js-binaire bestanden naar uw Raspberry Pi:

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Gebruik de volgende opdracht voor het installeren van de binaire bestanden:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Gebruik de volgende opdracht om te controleren of dat u hebt Node.js v6.10.2 is geïnstalleerd:

    ```sh
    node --version
    ```

### <a name="clone-the-repositories"></a>Klonen van de opslagplaatsen

Als u dit nog niet hebt gedaan, moet u de vereiste opslagplaatsen klonen door het uitvoeren van de volgende opdrachten in een terminalvenster op uw Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>De verbindingsreeks van het apparaat bijwerken

Open het bronbestand voorbeeld in de **nano** editor met behulp van de volgende opdracht uit:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

Zoek de regel:

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

Vervang de tijdelijke aanduiding door het apparaat en IoT Hub gegevens die u hebt gemaakt en opgeslagen aan het begin van deze zelfstudie. Sla de wijzigingen (**Ctrl-O**, **Enter**) en de editor af te sluiten (**Ctrl X**).

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Voer de volgende opdrachten om de vereiste pakketten voor het voorbeeld te installeren:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

U kunt nu het voorbeeldprogramma uitvoeren op de Raspberry Pi. Voer de opdracht:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

De volgende voorbeelduitvoer wordt een voorbeeld van de uitvoer u bij de opdrachtprompt op de Raspberry Pi ziet:

![Uitvoer van de app Raspberry Pi][img-raspberry-output]

Druk op **Ctrl-C** om het programma op elk gewenst moment af te sluiten.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure IoT-ontwikkelaarscentrum](https://azure.microsoft.com/develop/iot/) voor meer voorbeelden en documentatie over Azure IoT.

[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

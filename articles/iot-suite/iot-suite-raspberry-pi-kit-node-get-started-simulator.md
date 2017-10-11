---
title: Een Pi frambozen verbinden met behulp van Node.js met gesimuleerde telemetrie van Azure IoT Suite | Microsoft Docs
description: Gebruik de Microsoft Azure IoT Starter Kit voor de Raspberry Pi 3 en Azure IoT Suite. Gebruik Node.js verbinding maken met uw frambozen-Pi de oplossing voor externe controle gesimuleerde telemetrie verzenden naar de cloud, en reageren op de methoden die worden aangeroepen vanuit het dashboard van oplossing.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: node.js
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: 43fbfe2f2c5fb86e496c4419f9565365cf89830a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a>Uw frambozen Pi 3 verbinding met de oplossing voor externe controle en verzenden van gesimuleerde telemetrie met behulp van Node.js

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Deze zelfstudie laat zien hoe u met de frambozen Pi 3 simuleren temperatuur en vochtigheid gegevens worden verzonden naar de cloud. De zelfstudie wordt gebruikt:

- Raspbian OS, de programmeertaal Node.js en Microsoft Azure IoT SDK voor Node.js voor het implementeren van een voorbeeld-apparaat.
- IoT Suite remote monitoring vooraf geconfigureerde oplossing als de cloud-gebaseerde back-end.

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> De oplossing voor externe controle levert een set van Azure-services in uw Azure-abonnement. De implementatie duidt op een echte enterprise-architectuur. Om te voorkomen dat een Azure-verbruik onnodige kosten, verwijdert u uw exemplaar van de vooraf geconfigureerde oplossing op azureiotsuite.com wanneer u klaar bent met het. Als u de vooraf geconfigureerde oplossing meer nodig hebt, kunt u het eenvoudig opnieuw. Zie voor meer informatie over het verbruik verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor demonstratiedoeleinden][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-the-sample"></a>Downloaden en configureren van de steekproef

U kunt nu downloaden en configureren van de externe clienttoepassing bewaking op uw frambozen Pi.

### <a name="install-nodejs"></a>Node.js installeren

Als u dit nog niet hebt gedaan, installeert u Node.js op uw frambozen Pi. De IoT-SDK voor Node.js vereist versie 0.11.5 van Node.js of hoger. De volgende stappen ziet u hoe Node.js v6.10.2 installeren op uw Pi frambozen:

1. Gebruik de volgende opdracht om bij te werken uw Pi frambozen:

    ```sh
    sudo apt-get update
    ```

1. Gebruik de volgende opdracht voor het downloaden van de Node.js-binaire bestanden naar uw Pi frambozen:

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Gebruik de volgende opdracht voor het installeren van de binaire bestanden:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Gebruik de volgende opdracht om te controleren of dat u hebt de Node.js-v6.10.2 is geïnstalleerd:

    ```sh
    node --version
    ```

### <a name="clone-the-repositories"></a>De opslagplaatsen klonen

Als u dit nog niet hebt gedaan, moet u de vereiste opslagplaatsen klonen met de volgende opdrachten in een terminal op uw Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>De verbindingsreeks apparaat bijwerken

Open het voorbeeld-bronbestand in de **nano** editor met de volgende opdracht:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

Zoek de regel:

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

Vervang de tijdelijke aanduiding voor waarden met het apparaat en IoT Hub informatie u gemaakt en opgeslagen aan het begin van deze zelfstudie. Sla de wijzigingen (**Ctrl-O**, **Enter**) en sluit de editor af (**Ctrl X**).

## <a name="run-the-sample"></a>Het voorbeeld uitvoert

Voer de volgende opdrachten voor het installeren van de vereiste pakketten voor de steekproef:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

U kunt nu het voorbeeldprogramma uitvoeren op de frambozen Pi. Voer de opdracht:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

De volgende voorbeelduitvoer volgt een voorbeeld van de uitvoer die u bij de opdrachtprompt op de Pi frambozen zien:

![De uitvoer van de app Raspberry Pi][img-raspberry-output]

Druk op **Ctrl-C** om af te sluiten van het programma op elk gewenst moment.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) voor meer voorbeelden en documentatie over Azure IoT.

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

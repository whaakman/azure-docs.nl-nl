---
title: Verbinding maken met een Pi frambozen Azure IoT Suite met echte sensoren met behulp van Node.js | Microsoft Docs
description: Gebruik de Microsoft Azure IoT Starter Kit voor de Raspberry Pi 3 en Azure IoT Suite. Gebruik Node.js verbinding maken met uw frambozen-Pi de oplossing voor externe controle verzenden van telemetrie van sensoren naar de cloud, en reageren op de methoden die worden aangeroepen vanuit het dashboard van oplossing.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 9bb4f62b1a3de68ce8796b60fe76cd97b9ab9ade
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-nodejs"></a>Uw frambozen Pi 3 verbinding met de oplossing voor externe controle en verzend telemetrie vanuit een echte sensor met behulp van Node.js

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Deze zelfstudie laat zien hoe de Microsoft Azure IoT Starter Kit voor frambozen Pi 3 gebruiken voor het ontwikkelen van een temperatuur en vochtigheid lezer die met de cloud communiceren kan. De zelfstudie wordt gebruikt:

- Raspbian OS, de programmeertaal Node.js en Microsoft Azure IoT SDK voor Node.js voor het implementeren van een voorbeeld-apparaat.
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

### <a name="install-nodejs"></a>Node.js installeren

Installeer Node.js op uw Raspberry Pi. De IoT-SDK voor Node.js vereist versie 0.11.5 van Node.js of hoger. De volgende stappen ziet u hoe Node.js v6.10.2 installeren op uw Pi frambozen:

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

Als u dit nog niet hebt gedaan, moet u de vereiste opslagplaatsen klonen door de volgende opdrachten uitvoeren op uw Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git`
```

### <a name="update-the-device-connection-string"></a>De verbindingsreeks apparaat bijwerken

Open het voorbeeld-bronbestand in de **nano** editor met de volgende opdracht:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

Zoek de regel:

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

Vervang de tijdelijke aanduiding voor waarden met het apparaat en IoT Hub informatie u gemaakt en opgeslagen aan het begin van deze zelfstudie. Sla de wijzigingen (**Ctrl-O**, **Enter**) en sluit de editor af (**Ctrl X**).

## <a name="run-the-sample"></a>Het voorbeeld uitvoert

Voer de volgende opdrachten voor het installeren van de vereiste pakketten voor de steekproef:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic
npm install
```

U kunt nu het voorbeeldprogramma uitvoeren op de frambozen Pi. Voer de opdracht:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

De volgende voorbeelduitvoer volgt een voorbeeld van de uitvoer die u bij de opdrachtprompt op de Pi frambozen zien:

![De uitvoer van de app Raspberry Pi][img-raspberry-output]

Druk op **Ctrl-C** om af te sluiten van het programma op elk gewenst moment.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) voor meer voorbeelden en documentatie over Azure IoT.

[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-basic/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

---
title: Verbinding maken met een Raspberry Pi met behulp van Node.js ter ondersteuning van firmware-updates van Azure IoT Suite | Microsoft Docs
description: Gebruik de Microsoft Azure IoT Starter Kit voor de Raspberry Pi 3 en Azure IoT Suite. Node.js gebruiken om te verbinden met uw Raspberry Pi de oplossing voor externe controle, telemetrie van sensors verzenden naar de cloud en voert u een externe firmware-update.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-suite
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 31bbeff8049c6005671b991f965fae7316e3adf6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38309588"
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-enable-remote-firmware-updates-using-nodejs"></a>Verbinding maken met uw Raspberry Pi 3 op de oplossing voor externe controle en inschakelen van externe firmware-updates met behulp van Node.js

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Deze zelfstudie leert u hoe de Microsoft Azure IoT Starter Kit voor Raspberry Pi 3 als u wilt gebruiken:

* Ontwikkel een temperatuur en vochtigheid-reader die met de cloud communiceren kan.
* Inschakelen en uitvoeren van een externe firmware update bijwerken de clienttoepassing op de Raspberry Pi.

De zelfstudie wordt gebruikgemaakt van:

- Raspbian OS-, de programmeertaal Node.js en de Microsoft Azure IoT SDK voor Node.js voor het implementeren van een voorbeeld-apparaat.
- Externe controle met IoT Suite vooraf geconfigureerde oplossing als de cloud-gebaseerde back-end.

## <a name="overview"></a>Overzicht

In deze zelfstudie voert u de volgende stappen uit:

- Implementeer een exemplaar van de vooraf geconfigureerde oplossing voor externe controle met uw Azure-abonnement. Deze stap automatisch implementeert en configureert u meerdere Azure-services.
- Instellen van uw apparaten en sensoren te communiceren met uw computer en de oplossing voor externe controle.
- Bijwerken van de voorbeeldcode van de apparaten verbinding maken met de oplossing voor externe controle en telemetrie die u op het dashboard van de oplossing weergeven kunt verzendt.
- Gebruik de voorbeeldcode voor het apparaat bij te werken van de clienttoepassing.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prerequisites](../../includes/iot-suite-v1-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> De oplossing voor externe controle inricht een set Azure-services in uw Azure-abonnement. De implementatie is inclusief een echte bedrijfsarchitectuur. Onnodige Azure-verbruik om kosten te voorkomen, verwijdert u uw exemplaar van de vooraf geconfigureerde oplossing op azureiotsuite.com wanneer u klaar bent met het. Als u de vooraf geconfigureerde oplossing meer nodig hebt, kunt u deze eenvoudig opnieuw. Zie voor meer informatie over het verbruik te verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor deze demo][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi.md)]

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

Als u dit nog niet hebt gedaan, moet u de vereiste opslagplaatsen klonen door het uitvoeren van de volgende opdrachten op uw Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>De verbindingsreeks van het apparaat bijwerken

Open het voorbeeld-configuratiebestand in de **nano** editor met behulp van de volgende opdracht uit:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

Vervang de tijdelijke aanduiding door de apparaat-id en de IoT Hub gegevens die u hebt gemaakt en opgeslagen aan het begin van deze zelfstudie.

Wanneer u klaar bent, wordt de inhoud van het bestand deviceinfo ziet als in het volgende voorbeeld:

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

Sla de wijzigingen (**Ctrl-O**, **Enter**) en de editor af te sluiten (**Ctrl X**).

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Voer de volgende opdrachten om de vereiste pakketten voor het voorbeeld te installeren:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advance/1.0
npm install
```

U kunt nu het voorbeeldprogramma uitvoeren op de Raspberry Pi. Voer de opdracht:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/1.0/remote_monitoring.js
```

De volgende voorbeelduitvoer wordt een voorbeeld van de uitvoer u bij de opdrachtprompt op de Raspberry Pi ziet:

![Uitvoer van de app Raspberry Pi][img-raspberry-output]

Druk op **Ctrl-C** om het programma op elk gewenst moment af te sluiten.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced.md)]

1. Klik in het dashboard van de oplossing op **apparaten** Bezoek de **apparaten** pagina. Selecteer uw Raspberry Pi in de **apparatenlijst**. Kies vervolgens **methoden**:

    ![Lijst met apparaten in het dashboard][img-list-devices]

1. Op de **aanroepmethode** pagina, kies **InitiateFirmwareUpdate** in de **methode** vervolgkeuzelijst.

1. In de **FWPackageURI** veld **https://raw.githubusercontent.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit/master/advanced/2.0/raspberry.js**. Dit bestand bevat de implementatie van versie 2.0 van de firmware.

1. Kies **InvokeMethod**. De app op de Raspberry Pi stuurt een bevestiging terug naar het dashboard van de oplossing. Vervolgens wordt de firmware-updateproces gestart door het downloaden van de nieuwe versie van de firmware:

    ![Geschiedenis van methoden weergeven][img-method-history]

## <a name="observe-the-firmware-update-process"></a>Bekijk de firmware bijwerken van proces

Hier ziet u de firmware updateproces terwijl deze wordt uitgevoerd op het apparaat en door de gerapporteerde eigenschappen op het dashboard van de oplossing weer te geven:

1. U kunt de voortgang in van het updateproces bekijken op de Raspberry Pi:

    ![Voortgang van de update weergeven][img-update-progress]

    > [!NOTE]
    > De externe controle-app wordt opnieuw gestart op de achtergrond wanneer de update is voltooid. Gebruik de opdracht `ps -ef` om te controleren of deze wordt uitgevoerd. Als u het proces is beëindigd wilt, gebruikt u de `kill` opdracht met de proces-id.

1. U ziet de status van de firmware-update, zoals gemeld door het apparaat, in de portal van de oplossing. De volgende schermafbeelding ziet u de status en de duur van elke fase van het updateproces en de nieuwe firmwareversie:

    ![Taakstatus weergeven][img-job-status]

    Als u terug naar het dashboard gaat, kunt u controleren of dat het apparaat wordt nog steeds verzenden van telemetrie volgen van de firmware-update.

> [!WARNING]
> Als u de oplossing voor externe controle die wordt uitgevoerd in uw Azure-account, worden in rekening gebracht voor de tijd dat deze wordt uitgevoerd. Zie voor meer informatie over het verbruik te verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor deze demo][lnk-demo-config]. De vooraf geconfigureerde oplossing verwijderen uit uw Azure-account wanneer u klaar bent met het gebruik ervan.

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure IoT-ontwikkelaarscentrum](https://azure.microsoft.com/develop/iot/) voor meer voorbeelden en documentatie over Azure IoT.


[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

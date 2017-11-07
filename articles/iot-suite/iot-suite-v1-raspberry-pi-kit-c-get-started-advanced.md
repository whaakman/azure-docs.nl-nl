---
title: Verbinding maken met een frambozen Pi met C ter ondersteuning van de firmware-updates van Azure IoT Suite | Microsoft Docs
description: Gebruik de Microsoft Azure IoT Starter Kit voor de Raspberry Pi 3 en Azure IoT Suite. Gebruik C verbinding maken met uw frambozen-Pi de oplossing voor externe controle verzenden van telemetrie van sensoren naar de cloud en uitvoeren van een externe firmware-update.
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
ms.openlocfilehash: 8160752b0116c3ef3e6b6ab7920bb35e471f180b
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-enable-remote-firmware-updates-using-c"></a>Uw frambozen Pi 3 verbinding met de oplossing voor externe controle en externe firmware-updates met C inschakelen

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Deze zelfstudie laat zien hoe u de Microsoft Azure IoT Starter Kit voor frambozen Pi 3 te gebruiken:

* Ontwikkel een temperatuur en vochtigheid lezer die met de cloud communiceren kan.
* Inschakelen en voer een externe firmware update bijwerken de clienttoepassing op de frambozen Pi.

De zelfstudie wordt gebruikt:

* Raspbian OS, de programmeertaal en Microsoft Azure IoT SDK voor C voor het implementeren van een voorbeeld-apparaat.
* IoT Suite remote monitoring vooraf geconfigureerde oplossing als de cloud-gebaseerde back-end.

## <a name="overview"></a>Overzicht

In deze zelfstudie maakt uitvoeren u de volgende stappen:

* Implementeer een exemplaar van de vooraf geconfigureerde oplossing voor externe controle op uw Azure-abonnement. Deze stap implementeert automatisch en meerdere Azure-services configureert.
* Instellen van het apparaat en de sensoren om te communiceren met uw computer en de oplossing voor externe controle.
* Werk de voorbeeldcode van de apparaten verbinding maken met de oplossing voor externe controle en verzenden van telemetrie die u op het dashboard van de oplossing weergeven kunt.
* Met de voorbeeldcode van het apparaat kunt bijwerken van de clienttoepassing.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prerequisites](../../includes/iot-suite-v1-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> De oplossing voor externe controle levert een set van Azure-services in uw Azure-abonnement. De implementatie duidt op een echte enterprise-architectuur. Om te voorkomen dat een Azure-verbruik onnodige kosten, verwijdert u uw exemplaar van de vooraf geconfigureerde oplossing op azureiotsuite.com wanneer u klaar bent met het. Als u de vooraf geconfigureerde oplossing meer nodig hebt, kunt u het eenvoudig opnieuw. Zie voor meer informatie over het verbruik verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor demonstratiedoeleinden][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a>Downloaden en configureren van de steekproef

U kunt nu downloaden en configureren van de externe clienttoepassing bewaking op uw frambozen Pi.

### <a name="clone-the-repositories"></a>De opslagplaatsen klonen

Als u dit nog niet hebt gedaan, moet u de vereiste opslagplaatsen klonen door de volgende opdrachten uitvoeren op uw Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>De verbindingsreeks apparaat bijwerken

Open het voorbeeldconfiguratiebestand in de **nano** editor met de volgende opdracht:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

Vervang de tijdelijke aanduiding voor waarden met de apparaat-ID en IoT-Hub u gemaakt en opgeslagen aan het begin van deze zelfstudie.

Wanneer u klaar bent, wordt de inhoud van het bestand deviceinfo moeten eruitzien als in het volgende voorbeeld:

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

Sla de wijzigingen (**Ctrl-O**, **Enter**) en sluit de editor af (**Ctrl X**).

## <a name="build-the-sample"></a>Het voorbeeld bouwen

Als u dit nog niet hebt gedaan, installeert u de vereiste pakketten voor de Microsoft Azure IoT-apparaat-SDK voor C met de volgende opdrachten in een terminal op de Pi frambozen:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

U kunt nu de Voorbeeldoplossing bouwen met de Pi frambozen:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
```

U kunt nu het voorbeeldprogramma uitvoeren op de frambozen Pi. Voer de opdracht:

  ```sh
  sudo ~/cmake/remote_monitoring/remote_monitoring
  ```

De volgende voorbeelduitvoer volgt een voorbeeld van de uitvoer die u bij de opdrachtprompt op de Pi frambozen zien:

![De uitvoer van de app Raspberry Pi][img-raspberry-output]

Druk op **Ctrl-C** om af te sluiten van het programma op elk gewenst moment.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced.md)]

1. Klik in het dashboard van de oplossing op **apparaten** bezoeken de **apparaten** pagina. Selecteer uw Raspberry Pi in de **lijst met apparaten**. Kies vervolgens **methoden**:

    ![Lijst met apparaten in het dashboard][img-list-devices]

1. Op de **methode Invoke** pagina **InitiateFirmwareUpdate** in de **methode** vervolgkeuzelijst.

1. In de **FWPackageURI** veld **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**. Dit archief bevat de implementatie van versie 2.0 van de firmware.

1. Kies **InvokeMethod**. De app op de Pi frambozen stuurt een bevestiging terug naar het dashboard van de oplossing. Vervolgens wordt het updateproces firmware gestart door de nieuwe versie van de firmware te downloaden:

    ![Overzicht van de methode weergeven][img-method-history]

## <a name="observe-the-firmware-update-process"></a>Houd rekening met de firmware proces niet bijwerken

U kunt zien dat de firmware proces niet bijwerken omdat deze wordt uitgevoerd op het apparaat en de gerapporteerde door eigenschappen te bekijken in het dashboard van oplossing:

1. U kunt de voortgang op van het updateproces bekijken op de Pi frambozen:

    ![Voortgang van bijwerken weergeven][img-update-progress]

    > [!NOTE]
    > De externe controle app achtergrond opnieuw opgestart wanneer de update is voltooid. Gebruik de opdracht `ps -ef` om te controleren of deze wordt uitgevoerd. Als u het proces is beëindigd wilt, gebruikt u de `kill` opdracht met de proces-id.

1. U kunt de status van de firmware-update weergeven, zoals gemeld door het apparaat, in de portal van de oplossing. De volgende schermafbeelding ziet u de status en de duur van elke fase van het updateproces kan controleren en de nieuwe firmwareversie:

    ![De status van de taak weergeven][img-job-status]

    Als u terug naar het dashboard navigeert, kunt u controleren of dat het apparaat is nog steeds verzenden van telemetrie na de firmware-update.

> [!WARNING]
> Als u de oplossing voor externe controle uitgevoerd in uw Azure-account laat, wordt u gefactureerd voor de tijd die wordt uitgevoerd. Zie voor meer informatie over het verbruik verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor demonstratiedoeleinden][lnk-demo-config]. De vooraf geconfigureerde oplossing verwijderen uit uw Azure-account wanneer u klaar bent met het gebruik van maken.

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) voor meer voorbeelden en documentatie over Azure IoT.


[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
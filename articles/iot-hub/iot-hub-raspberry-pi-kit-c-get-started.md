---
title: Raspberry Pi naar cloud (C) - frambozen Pi verbinding maken met Azure IoT Hub | Microsoft Docs
description: Informatie over het instellen en frambozen Pi verbinden met Azure IoT Hub voor frambozen Pi gegevens verzenden naar het Azure-cloud-platform in deze zelfstudie.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot raspberry pi, raspberry pi iothub, raspberry pi verzenden gegevens naar de cloud, raspberry pi naar cloud
ms.assetid: 68c0e730-1dc8-4e26-ac6b-573b217b302d
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/12/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d643647d4103acd511ed270132c844da12f2ac9b
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2017
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-c"></a>Verbinding maken met Raspberry Pi Azure IoT Hub (C)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In deze zelfstudie maakt beginnen u door te leren over het werken met frambozen Pi met Raspbian. Vervolgens leert u hoe u uw apparaten probleemloos verbinding met de cloud via [Azure IoT Hub](iot-hub-what-is-iot-hub.md). Voor voorbeelden van Windows 10 IoT-Core, gaat u naar de [Windows-ontwikkelaarscentrum](http://www.windowsondevices.com/).

Heb je nog een kit? Probeer [frambozen Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md). Een nieuwe kit kopen of [hier](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Wat u doet

* Een iothub maken.
* Een apparaat registreren voor Pi in uw IoT-hub.
* Setup Raspberry Pi.
* Een voorbeeld van een toepassing uitvoeren op Pi sensorgegevens verzenden naar uw IoT-hub.

Verbinding maken met frambozen Pi naar een IoT-hub die u maakt. Vervolgens voert u een voorbeeld van toepassing op Pi temperatuur en vochtigheid om gegevens te verzamelen van een sensor BME280. U kunt ten slotte de sensorgegevens verzendt naar uw IoT-hub.

## <a name="what-you-learn"></a>Wat u leert

* Het maken van een Azure-IoT-hub en het nieuwe apparaat-verbindingsreeks ophalen.
* Klik hier voor meer informatie over het Pi verbinden met een sensor BME280.
* Klik hier voor meer informatie over het verzamelen van sensorgegevens door het uitvoeren van een voorbeeldtoepassing met Pi.
* Hoe sensorgegevens verzendt naar uw IoT-hub.

## <a name="what-you-need"></a>Wat u nodig hebt

![Wat u nodig hebt](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* De frambozen Pi 2 of frambozen Pi 3-kaart.
* Een actief Azure-abonnement. Als u een Azure-account geen [maken van een gratis Azure-proefaccount](https://azure.microsoft.com/free/) over een paar minuten.
* Een monitor, een USB-toetsenbord en muis die verbinding met Pi maken.
* Een Mac of een PC die Windows of Linux wordt uitgevoerd.
* Een internetverbinding.
* Een 16 GB of hoger microSD-kaart.
* Een USB-SD adapter of microSD-kaart branden van de installatiekopie van het besturingssysteem op de microSD-kaart.
* Een 5-v 2 amp voeding met 6 mond micro USB-kabel.

De volgende items zijn optioneel:

* Een samengesteld Adafruit BME280 temperatuur, druk en vochtigheid sensor.
* Een breadboard.
* 6 F/M meestal bedrading.
* Een gedempt 10 mm LED.


> [!NOTE] 
Deze items zijn optioneel, omdat de ondersteuning voor het voorbeeld van code sensorgegevens gesimuleerd.


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a>Raspberry Pi instellen

### <a name="install-the-raspbian-operating-system-for-pi"></a>Het besturingssysteem Raspbian voor Pi installeren

Bereid de microSD-kaart voor de installatie van de installatiekopie van het Raspbian.

1. Raspbian downloaden.
   1. [Raspbian Jessie met bureaublad downloaden](https://www.raspberrypi.org/downloads/raspbian/) (het ZIP-bestand).
   1. Pak de installatiekopie van het Raspbian naar een map op uw computer.
1. Installeer Raspbian naar de microSD-kaart.
   1. [Download en installeer het hulpprogramma Etcher SD-kaart brander](https://etcher.io/).
   1. Voer Etcher en selecteer de installatiekopie van het Raspbian die u hebt opgehaald in stap 1.
   1. Selecteer het station microSD-kaart. Houd er rekening mee dat Etcher mogelijk al hebt geselecteerd de juiste station.
   1. Klik op Flash voor het installeren van Raspbian naar de microSD-kaart.
   1. Verwijder de microSD-kaart van uw computer wanneer de installatie voltooid is. Het is veilig worden verwijderd, de microSD-kaart rechtstreeks omdat Etcher automatisch uitwerpen of ontkoppelt de microSD-kaart is voltooid.
   1. Plaats de microSD-kaart in Pi.

### <a name="enable-ssh-and-spi"></a>SSH- en SPI inschakelen

1. Verbinding maken met Pi de monitor, toetsenbord en muis Pi start en meld u daarna Raspbian met behulp van `pi` als de gebruikersnaam en `raspberry` als het wachtwoord.
1. Klik op het pictogram Raspberry > **voorkeuren** > **frambozen Pi configuratie**.

   ![Het menu Raspbian voorkeuren](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. Op de **Interfaces** tabblad, stelt u **SPI** en **SSH** naar **inschakelen**, en klik vervolgens op **OK**. Als u geen fysieke sensoren hebben en gesimuleerde sensorgegevens wilt gebruiken, is deze stap is optioneel.

   ![SPI en SSH met Raspberry Pi inschakelen](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
Voor het inschakelen van SSH en SPI, vindt u meer verwijzing documenten op [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) en [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-the-sensor-to-pi"></a>Verbinding maken met de sensor Pi

Gebruik de bedrading breadboard en meestal als volgt een LED en een BME280 tot Pi verbinden. Als u de sensor geen [deze sectie overslaan](#connect-pi-to-the-network).

![De verbinding frambozen Pi en temperatuursensor](media/iot-hub-raspberry-pi-kit-c-get-started/3_raspberry-pi-sensor-connection.png)

De sensor BME280 kunt temperatuur en vochtigheid gegevens verzamelen. En de LED knippert als er een communicatie tussen het apparaat en de cloud. 

Gebruik de volgende bedrading voor sensor-pincodes:

| Start (Sensor & LED)     | Einde (BMC)            | Kleur van de kabel   |
| -----------------------  | ---------------------- | ------------: |
| LED VDD (Pin 5G)         | GPIO 4 (pincode 7)         | Witte kabel   |
| LED GND (Pin 6G)         | GND (pincode 6)            | Zwarte kabel   |
| VDD (Pin 18F)            | 3, 3v PWR (Pin 17)      | Witte kabel   |
| GND (Pin 20F)            | GND (Pin 20)           | Zwarte kabel   |
| SCK (Pin 21F)            | SPI0 SCLK (Pin 23)     | Oranje-kabel  |
| SDO (Pin 22F)            | SPI0 MISO (Pin 21)     | Gele-kabel  |
| SDI (Pin 23F)            | SPI0 MOSI (Pin 19)     | Groen-kabel   |
| CS (Pin 24F)             | SPI0 CS (pincode 24)       | Blauw-kabel    |

Klik hier voor weergave [frambozen Pi 2 en 3 pincode toewijzingen](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) ter referentie.

Nadat u hebt BME280 is verbonden met uw Pi frambozen, moet zoals hieronder installatiekopie.

![Verbonden Pi en BME280](media/iot-hub-raspberry-pi-kit-c-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Pi verbinding met het netwerk

Pi inschakelen met behulp van het micro USB-kabel en de voeding. Via het Ethernet-kabel Pi verbinden met het bekabelde netwerk of volgen de [instructies van de frambozen Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi verbinding met uw draadloze netwerk. Nadat uw Pi is verbonden met het netwerk, moet u Noteer de [IP-adres van uw Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Verbonden met een bekabeld netwerk](media/iot-hub-raspberry-pi-kit-c-get-started/5_power-on-pi.jpg)


## <a name="run-a-sample-application-on-pi"></a>Een voorbeeld van een toepassing uitvoeren op Pi

### <a name="login-to-your-raspberry-pi"></a>Meld u aan bij uw Raspberry Pi

1. Gebruik een van de volgende SSH-clients van de hostcomputer verbinding maken met uw frambozen Pi.
   
   **Windows-gebruikers**
   1. Download en installeer [PuTTY](http://www.putty.org/) voor Windows. 
   1. Kopieer het IP-adres van uw sectie Pi in de Host-naam (of IP-adres) en selecteer SSH als het verbindingstype.
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac- en Ubuntu-gebruikers**
   
   Gebruik de ingebouwde SSH-client op Ubuntu- of Mac OS. U wilt uitvoeren `ssh pi@<ip address of pi>` Pi via SSH verbinding maken.
   > [!NOTE] 
   De standaardgebruikersnaam `pi` , en het wachtwoord is `raspberry`.


### <a name="configure-the-sample-application"></a>De voorbeeldtoepassing configureren

1. Kloon de voorbeeldtoepassing met de volgende opdracht:

   ```bash
   sudo apt-get install git-core
   git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-client-app.git
   ```

2. Setup-script uitvoeren:

   ```bash
   cd ./iot-hub-c-raspberrypi-client-app
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```

   > [!NOTE] 
   > Als u **beschikt niet over van een fysieke BME280**, kunt u '--simulated-data' als opdrachtregelparameter temperatuur en vochtigheid gegevens simuleren. `sudo ./setup.sh --simulated-data`

### <a name="build-and-run-the-sample-application"></a>De voorbeeldtoepassing bouwen en uitvoeren

1. De voorbeeldtoepassing met de volgende opdracht:

   ```bash
   cmake . && make
   ```
   ![Uitvoer bouwen](media/iot-hub-raspberry-pi-kit-c-get-started/7_build-output.png)

1. De voorbeeldtoepassing met de volgende opdracht uitvoeren:

   ```bash
   sudo ./app '<DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   Zorg ervoor dat u kopiëren en plakken de apparaat-verbindingsreeks in enkele aanhalingstekens.


Hier ziet u de volgende uitvoer waarin de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub.

![Output - sensorgegevens verzonden van frambozen Pi naar uw IoT-hub](media/iot-hub-raspberry-pi-kit-c-get-started/8_run-output.png)

## <a name="next-steps"></a>Volgende stappen

U kunt een voorbeeldtoepassing sensorgegevens verzamelen en verzenden naar uw IoT-hub hebt uitgevoerd. Zie voor de berichten die uw Pi frambozen is verzonden naar uw IoT-hub of verzenden berichten naar uw Pi frambozen in een opdrachtregelinterface de [beheren cloud apparaat messaging met iothub explorer zelfstudie](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

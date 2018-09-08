---
title: Raspberry Pi tot cloud (C) - verbinding maken met Raspberry Pi met Azure IoT Hub | Microsoft Docs
description: Informatie over het installeren en verbinden van Raspberry Pi met Azure IoT Hub voor Raspberry Pi gegevens verzenden naar de Azure-cloudplatform in deze zelfstudie.
author: rangv
manager: ''
keywords: Azure iot raspberry pi, raspberry pi iot-hub, raspberry pi verzenden gegevens naar de cloud, raspberry pi naar de cloud
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 8962f41d971ac88134f639a61ef0b42c60bbd419
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161489"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-c"></a>Raspberry Pi verbinden met Azure IoT Hub (C)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In deze zelfstudie maakt begint u door te leren van de basisbeginselen van het werken met Raspberry Pi met Raspbian. Vervolgens leert u hoe u uw apparaten naadloos verbinding naar de cloud met behulp van [Azure IoT Hub](about-iot-hub.md). Voor voorbeelden van Windows 10 IoT Core, gaat u naar de [Windows Dev Center](http://www.windowsondevices.com/).

Heb je nog een kit? Probeer [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md). Een nieuwe kit kopen of [hier](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Wat u allemaal doen

* Een IoT-hub maken.
* Registreer een apparaat voor Pi in uw IoT-hub.
* Raspberry Pi instellen.
* Een voorbeeldtoepassing uitvoert op Pi om sensorgegevens te verzenden naar uw IoT hub.

Raspberry Pi verbinden met een IoT-hub die u maakt. Vervolgens voert u een voorbeeld van toepassing op Pi temperatuur en vochtigheid gegevens verzamelen uit een sensor BME280. Ten slotte, stuurt u de sensorgegevens naar uw IoT-hub.

## <a name="what-you-learn"></a>Wat u leert

* Over het maken van een Azure-IoT-hub en uw nieuwe apparaat-verbindingsreeks ophalen.
* Klik hier voor meer informatie over het verbinden van Pi met een sensor BME280.
* Klik hier voor meer informatie over het verzamelen van sensorgegevens door het uitvoeren van een voorbeeld van toepassing op Pi.
* Klik hier voor meer informatie over het verzenden van gegevens naar uw IoT hub.

## <a name="what-you-need"></a>Wat u nodig hebt

![Wat u nodig hebt](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* Het bord Raspberry Pi 2 of Raspberry Pi 3.
* Een actief Azure-abonnement. Als u een Azure-account geen [Azure een gratis proefaccount maken](https://azure.microsoft.com/free/) in een paar minuten.
* Een monitor, een USB-toetsenbord en muis die verbinding met Pi maken.
* Een Mac- of een PC waarop Windows of Linux wordt uitgevoerd.
* Een internetverbinding.
* Een 16 GB of hoger microSD-kaart.
* Een USB-SD adapter of microSD-kaart branden van de installatiekopie van het besturingssysteem op de microSD-kaart.
* Een 5-v 2-en voeding met de 6-mond micro USB-kabel.

De volgende items zijn optioneel:

* Een geassembleerde Adafruit BME280 temperatuur, druk te verlichten en vochtigheid sensor.
* Een breadboard.
* 6-F/M meestal bedrading.
* Een gedempt 10-mm-LED.


> [!NOTE] 
Deze items zijn optioneel, omdat de ondersteuning voor het voorbeeld van code sensorgegevens gesimuleerde.


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a>Raspberry Pi instellen

### <a name="install-the-raspbian-operating-system-for-pi"></a>Het besturingssysteem Raspbian voor Pi installeren

Bereid de microSD-kaart voor de installatie van de installatiekopie van het Raspbian.

1. Download Raspbian.
   1. [Download Raspbian Jessie met Desktop](https://www.raspberrypi.org/downloads/raspbian/) (het ZIP-bestand).
   1. Pak de installatiekopie van het Raspbian naar een map op uw computer.
1. Installeer Raspbian naar de microSD-kaart.
   1. [Download en installeer het hulpprogramma Etcher SD-kaart brander](https://etcher.io/).
   1. Voer Etcher en selecteer de Raspbian-installatiekopie die u hebt opgehaald in stap 1.
   1. Selecteer het station microSD-kaart. Houd er rekening mee dat Etcher mogelijk al hebt geselecteerd de juiste station.
   1. Klik op Flash voor het installeren van Raspbian naar de microSD-kaart.
   1. De microSD-kaart van de computer verwijderen wanneer de installatie is voltooid. Het is veilig verwijderen van de microSD-kaart rechtstreeks omdat Etcher automatisch uitwerpen of losgekoppeld van de microSD-kaart na voltooiing.
   1. Plaats de microSD-kaart in Pi.

### <a name="enable-ssh-and-spi"></a>SSH en SPI inschakelen

1. Pi verbinden met de monitor, toetsenbord en muis, Pi starten en meld u vervolgens aan Raspbian met behulp van `pi` als de gebruikersnaam en `raspberry` als het wachtwoord.
1. Klik op het pictogram Raspberry > **voorkeuren** > **Raspberry Pi configuratie**.

   ![Het menu Raspbian voorkeuren](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. Op de **Interfaces** tabblad **SPI** en **SSH** naar **inschakelen**, en klik vervolgens op **OK**. Als u geen fysieke sensoren hebt en gesimuleerde sensorgegevens wilt gebruiken, is deze stap is optioneel.

   ![SPI en SSH op Raspberry Pi inschakelen](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
Om in te schakelen SSH en SPI, vindt u meer referentiedocumenten op [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) en [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-the-sensor-to-pi"></a>Verbinding maken met de sensor Pi

Gebruik de breadboard en meestal kabels verbinden met een LED en een BME280 Pi als volgt. Als u geen de sensor [deze sectie overslaan](#connect-pi-to-the-network).

![De sensor en Raspberry Pi-verbinding](media/iot-hub-raspberry-pi-kit-c-get-started/3_raspberry-pi-sensor-connection.png)

De sensor BME280 kunt temperatuur en vochtigheid gegevens verzamelen. En de LED knippert als er een communicatie tussen het apparaat en de cloud. 

Voor sensor pincodes, gebruikt u de volgende bedrading:

| Start (Sensor & LED)     | Einde (bord)            | Kleur van de kabel   |
| -----------------------  | ---------------------- | ------------: |
| LED VDD (pincode 5G)         | GPIO 4 (Pin 7)         | Wit-kabel   |
| LED GND (pincode 6G)         | GND (pincode 6)            | Zwarte kabel   |
| VDD (Pin 18F)            | 3, 3v PWR (Pin 17)      | Wit-kabel   |
| GND (Pin 20F)            | GND (Pin 20)           | Zwarte kabel   |
| SCK (Pin 21F)            | SPI0 SCLK (Pin 23)     | Oranje-kabel  |
| SDO (Pin 22F)            | SPI0 MISO (Pin 21)     | Gele-kabel  |
| SDI (Pin 23F)            | SPI0 MOSI (Pin 19)     | Groen-kabel   |
| CS (Pin 24F)             | SPI0 CS (Pin 24 uur per dag)       | Blauwe-kabel    |

Klik om weer te geven [Raspberry Pi 2 en 3 pincode toewijzingen](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) ter referentie.

Nadat u hebt BME280 is verbonden met uw Raspberry Pi, moet zijn, zoals onder afbeelding.

![Verbonden Pi en BME280](media/iot-hub-raspberry-pi-kit-c-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Pi verbinding met het netwerk

Pi inschakelen met behulp van de micro USB-kabel en de voeding. De Ethernet-kabel gebruiken om te verbinden met het bekabelde netwerk Pi of gaat u als volgt de [instructies van de basis van Raspberry Pi](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi verbinden met het draadloze netwerk. Nadat uw Pi is verbonden met het netwerk, moet u Noteer de [IP-adres van uw Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Verbonden met het bekabelde netwerk](media/iot-hub-raspberry-pi-kit-c-get-started/5_power-on-pi.jpg)


## <a name="run-a-sample-application-on-pi"></a>Een voorbeeld van toepassing op Pi uitvoeren

### <a name="login-to-your-raspberry-pi"></a>Meld u aan bij uw Raspberry Pi

1. Gebruik een van de volgende SSH-clients vanaf uw hostcomputer verbinding maken met uw Raspberry Pi.
   
   **Windows-gebruikers**
   1. Download en installeer [PuTTY](http://www.putty.org/) voor Windows. 
   1. Kopieer het IP-adres van de sectie Pi in de Host-naam (of IP-adres) en SSH als het verbindingstype selecteren.
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac- en Ubuntu-gebruikers**
   
   Gebruik de ingebouwde SSH-client op Ubuntu- of Mac OS. Mogelijk moet u uitvoeren `ssh pi@<ip address of pi>` om Pi via SSH verbinding te maken.
   > [!NOTE] 
   De standaardgebruikersnaam is `pi` , en het wachtwoord is `raspberry`.


### <a name="configure-the-sample-application"></a>De voorbeeld-App configureren

1. Kloon de voorbeeld-App met de volgende opdracht:

   ```bash
   sudo apt-get install git-core
   git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-client-app.git
   ```

2. Voer de setup-script:

   ```bash
   cd ./iot-hub-c-raspberrypi-client-app
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```

   > [!NOTE] 
   > Als u **geen een fysieke BME280**, kunt u '--gesimuleerde gegevens ' als opdrachtregelparameter temperatuur en vochtigheid gegevens simuleren. `sudo ./setup.sh --simulated-data`

### <a name="build-and-run-the-sample-application"></a>De voorbeeldtoepassing bouwen en uitvoeren

1. De voorbeeldtoepassing met de volgende opdracht:

   ```bash
   cmake . && make
   ```
   ![Uitvoer maken](media/iot-hub-raspberry-pi-kit-c-get-started/7_build-output.png)

1. De voorbeeldtoepassing uitvoeren met de volgende opdracht:

   ```bash
   sudo ./app '<DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   Zorg ervoor dat u kopiëren en plakken de apparaatverbindingsreeks in enkele aanhalingstekens.


Hier ziet u de volgende uitvoer ziet u de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub.

![Uitvoer - sensorgegevens van Raspberry Pi verzonden naar uw IoT-hub](media/iot-hub-raspberry-pi-kit-c-get-started/8_run-output.png)

## <a name="next-steps"></a>Volgende stappen

U kunt een voorbeeld van toepassing voor het verzamelen van gegevens en verzenden naar uw IoT-hub hebt uitgevoerd. Zie voor de berichten die uw Raspberry Pi naar uw IoT-hub of verzenden berichten naar uw Raspberry Pi verzonden de [gebruik Azure IoT Toolkit-extensie voor Visual Studio-Code voor het verzenden en ontvangen van berichten tussen uw apparaat en IoT-Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

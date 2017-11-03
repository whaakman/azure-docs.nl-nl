---
title: Raspberry Pi naar cloud (Node.js) - frambozen Pi verbinding maken met Azure IoT Hub | Microsoft Docs
description: Informatie over het instellen en frambozen Pi verbinden met Azure IoT Hub voor frambozen Pi gegevens verzenden naar het Azure-cloud-platform in deze zelfstudie.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot raspberry pi, raspberry pi iothub, raspberry pi verzenden gegevens naar de cloud, raspberry pi naar cloud
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/14/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7bf423fd05d6651bf16693e6d6930fada8b5da70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a>Raspberry Pi verbinden met Azure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In deze zelfstudie maakt beginnen u door te leren over het werken met frambozen Pi met Raspbian. Vervolgens leert u hoe u uw apparaten probleemloos verbinding met de cloud via [Azure IoT Hub](iot-hub-what-is-iot-hub.md). Voor voorbeelden van Windows 10 IoT-Core, gaat u naar de [Windows-ontwikkelaarscentrum](http://www.windowsondevices.com/).

Heb je nog een kit? Probeer [frambozen Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md). Een nieuwe kit kopen of [hier](https://azure.microsoft.com/develop/iot/starter-kits).


## <a name="what-you-do"></a>Wat u doet

* Een iothub maken.
* Een apparaat registreren voor Pi in uw IoT-hub.
* Frambozen Pi instellen.
* Een voorbeeld van een toepassing uitvoeren op Pi sensorgegevens verzenden naar uw IoT-hub.

## <a name="what-you-learn"></a>Wat u leert

* Het maken van een Azure-IoT-hub en het nieuwe apparaat-verbindingsreeks ophalen.
* Klik hier voor meer informatie over het Pi verbinden met een sensor BME280.
* Klik hier voor meer informatie over het verzamelen van sensorgegevens door het uitvoeren van een voorbeeldtoepassing met Pi.
* Hoe sensorgegevens verzendt naar uw IoT-hub.

## <a name="what-you-need"></a>Wat u nodig hebt

![Wat u nodig hebt](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* Een frambozen Pi 2 of frambozen Pi 3-kaart.
* Een actief Azure-abonnement. Als u een Azure-account geen [maken van een gratis Azure-proefaccount](https://azure.microsoft.com/free/) over een paar minuten.
* Een monitor, een USB-toetsenbord en muis die verbinding met Pi maakt.
* Een Mac of een PC waarop Windows of Linux wordt uitgevoerd.
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
Als u de optionele items niet hebt, kunt u gesimuleerde sensorgegevens.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="set-up-raspberry-pi"></a>Frambozen Pi instellen

### <a name="install-the-raspbian-operating-system-for-pi"></a>Het besturingssysteem Raspbian voor Pi installeren

Bereid de microSD-kaart voor de installatie van de installatiekopie van het Raspbian.

1. Raspbian downloaden.
   1. [Download Raspbian Stretch](http://downloads.raspberrypi.org/raspbian/images/raspbian-2017-07-05/) (het ZIP-bestand).

   > [!WARNING]
   > Gebruik bovenstaande koppeling voor het downloaden van `raspbian-2017-07-5` zip-installatiekopie. De nieuwste versie van Raspbian installatiekopieën heeft enkele bekende problemen met bedrading Pi-knooppunt, wat leiden fout in de volgende stappen tot kan.
   1. Pak de installatiekopie van het Raspbian naar een map op uw computer.

1. Installeer Raspbian naar de microSD-kaart.
   1. [Download en installeer het hulpprogramma Etcher SD-kaart brander](https://etcher.io/).
   1. Voer Etcher en selecteer de installatiekopie van het Raspbian die u hebt opgehaald in stap 1.
   1. Selecteer het station microSD-kaart. Etcher heeft mogelijk al het juiste station geselecteerd.
   1. Klik op Flash voor het installeren van Raspbian naar de microSD-kaart.
   1. Verwijder de microSD-kaart van uw computer wanneer de installatie voltooid is. Het is veilig worden verwijderd, de microSD-kaart rechtstreeks omdat Etcher automatisch uitwerpen of ontkoppelt de microSD-kaart is voltooid.
   1. Plaats de microSD-kaart in Pi.

### <a name="enable-ssh-and-i2c"></a>SSH- en I2C inschakelen

1. Pi verbinding met de monitor, toetsenbord en muis. 
1. Start Pi en meld u in Raspbian met behulp van `pi` als de gebruikersnaam en `raspberry` als het wachtwoord.
1. Klik op het pictogram Raspberry > **voorkeuren** > **frambozen Pi configuratie**.

   ![Het menu Raspbian voorkeuren](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. Op de **Interfaces** tabblad, stelt u **I2C** en **SSH** naar **inschakelen**, en klik vervolgens op **OK**. Als u geen fysieke sensoren hebben en gesimuleerde sensorgegevens wilt gebruiken, is deze stap is optioneel.

   ![I2C en SSH met Raspberry Pi inschakelen](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
Voor het inschakelen van SSH en I2C, vindt u meer verwijzing documenten op [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) en [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).

### <a name="connect-the-sensor-to-pi"></a>Verbinding maken met de sensor Pi

Gebruik de bedrading breadboard en meestal als volgt een LED en een BME280 tot Pi verbinden. Als u de sensor geen [deze sectie overslaan](#connect-pi-to-the-network).

![De verbinding frambozen Pi en temperatuursensor](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

De sensor BME280 kunt temperatuur en vochtigheid gegevens verzamelen. De LED knippert als het apparaat een bericht naar de cloud verzendt. 

Gebruik de volgende bedrading voor sensor-pincodes:

| Start (Sensor & LED)     | Einde (BMC)            | Kleur van de kabel   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 5G)             | 3, 3v PWR (pincode 1)       | Witte kabel   |
| GND (Pin 7G)             | GND (pincode 6)            | Bruine-kabel   |
| SDI (Pin 10G)            | I2C1 SDA (pincode 3)       | Rode-kabel     |
| SCK (Pin 8G)             | I2C1 SCL (pincode 5)       | Oranje-kabel  |
| LED VDD (Pin 18F)        | GPIO 24 (Pin 18)       | Witte kabel   |
| LED GND (Pin 17F)        | GND (Pin 20)           | Zwarte kabel   |

Klik hier voor weergave [frambozen Pi 2 en 3 pincode toewijzingen](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) ter referentie.

Nadat u hebt BME280 is verbonden met uw Pi frambozen, moet zoals hieronder installatiekopie.

![Verbonden Pi en BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Pi verbinding met het netwerk

Pi inschakelen met behulp van het micro USB-kabel en de voeding. Via het Ethernet-kabel Pi verbinden met het bekabelde netwerk of volgen de [instructies van de frambozen Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi verbinding met uw draadloze netwerk. Nadat uw Pi is verbonden met het netwerk, moet u Noteer de [IP-adres van uw Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Verbonden met een bekabeld netwerk](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Zorg ervoor dat Pi is verbonden met hetzelfde netwerk bevindt als uw computer. Bijvoorbeeld, als uw computer is verbonden met een draadloos netwerk als Pi is verbonden met een bekabeld netwerk, ziet u mogelijk niet het IP-adres in de uitvoer devdisco.

## <a name="run-a-sample-application-on-pi"></a>Een voorbeeld van een toepassing uitvoeren op Pi

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a>Klonen van de voorbeeldtoepassing en de vereiste pakketten installeren

1. Verbinding maken met uw frambozen Pi met een van de volgende SSH-clients van uw hostcomputer:
   
   **Windows-gebruikers**
   1. Download en installeer [PuTTY](http://www.putty.org/) voor Windows. 
   1. Kopieer het IP-adres van uw sectie Pi in de Host-naam (of IP-adres) en selecteer SSH als het verbindingstype.
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac- en Ubuntu-gebruikers**
   
   Gebruik de ingebouwde SSH-client op Ubuntu- of Mac OS. U wilt uitvoeren `ssh pi@<ip address of pi>` Pi via SSH verbinding maken.
   > [!NOTE] 
   De standaardgebruikersnaam `pi` en het wachtwoord is `raspberry`.

1. Node.js en NPM naar uw Pi installeren.
   
   Controleer eerst uw Node.js-versie. 
   
   ```bash
   node -v
   ```

   Als de versie is lager dan 4.x, of als er geen Node.js op uw Pi, installeer de nieuwste versie.

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. Kloon de voorbeeldtoepassing.

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. Alle pakketten installeren voor het voorbeeld. De installatie bevat Azure IoT-device SDK, BME280 Sensor-bibliotheek en bedrading Pi-bibliotheek.

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   Het duurt enkele minuten duren, het installatieproces, afhankelijk van uw netwerkverbinding.

### <a name="configure-the-sample-application"></a>De voorbeeldtoepassing configureren

1. Open het configuratiebestand met de volgende opdrachten:

   ```bash
   nano config.json
   ```

   ![Configuratiebestand](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   Er zijn twee items in dit bestand dat u kunt configureren. De eerste is `interval`, definieert het tijdsinterval (in milliseconden) tussen de berichten die worden verzonden naar de cloud. Het tweede is `simulatedData`, dit is een Booleaanse waarde voor of gesimuleerde sensorgegevens of niet.

   Als u **hoeft niet de sensor**stelt de `simulatedData` van waarde naar `true` ervoor dat de voorbeeldtoepassing maken en gebruiken van gesimuleerde sensorgegevens.

1. Opslaan en sluiten door te typen van de besturingselement-O > Enter > Control X.

### <a name="run-the-sample-application"></a>De voorbeeldtoepassing uitvoeren

De voorbeeldtoepassing met de volgende opdracht uitvoeren:

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   Zorg ervoor dat u kopiëren en plakken de apparaat-verbindingsreeks in enkele aanhalingstekens.


Hier ziet u de volgende uitvoer waarin de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub.

![Output - sensorgegevens verzonden van frambozen Pi naar uw IoT-hub](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a>Volgende stappen

U kunt een voorbeeldtoepassing sensorgegevens verzamelen en verzenden naar uw IoT-hub hebt uitgevoerd. Zie de berichten die uw Pi frambozen is verzonden naar uw IoT-hub of berichten verzenden naar uw Pi frambozen in een opdrachtregelinterface de [beheren cloud apparaat messaging met iothub explorer zelfstudie](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

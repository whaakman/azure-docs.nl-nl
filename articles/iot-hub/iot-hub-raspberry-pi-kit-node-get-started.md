---
title: Raspberry Pi tot cloud (Node.js) - verbinding maken met Raspberry Pi met Azure IoT Hub | Microsoft Docs
description: Informatie over het installeren en verbinden van Raspberry Pi met Azure IoT Hub voor Raspberry Pi gegevens verzenden naar de Azure-cloudplatform in deze zelfstudie.
author: rangv
manager: ''
keywords: Azure iot raspberry pi, raspberry pi iot-hub, raspberry pi verzenden gegevens naar de cloud, raspberry pi naar de cloud
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 3f34167cfe689734ec5d5954a1c24a09a1e8d3bd
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515009"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a>Raspberry Pi verbinden met Azure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In deze zelfstudie maakt begint u door te leren van de basisbeginselen van het werken met Raspberry Pi met Raspbian. Vervolgens leert u hoe u uw apparaten naadloos verbinding naar de cloud met behulp van [Azure IoT Hub](about-iot-hub.md). Voor voorbeelden van Windows 10 IoT Core, gaat u naar de [Windows Dev Center](http://www.windowsondevices.com/).

Heb je nog een kit? Probeer [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md). Een nieuwe kit kopen of [hier](https://azure.microsoft.com/develop/iot/starter-kits).


## <a name="what-you-do"></a>Wat u allemaal doen

* Een IoT-hub maken.
* Registreer een apparaat voor Pi in uw IoT-hub.
* Raspberry Pi instellen.
* Een voorbeeldtoepassing uitvoert op Pi om sensorgegevens te verzenden naar uw IoT hub.

## <a name="what-you-learn"></a>Wat u leert

* Over het maken van een Azure-IoT-hub en uw nieuwe apparaat-verbindingsreeks ophalen.
* Klik hier voor meer informatie over het verbinden van Pi met een sensor BME280.
* Klik hier voor meer informatie over het verzamelen van sensorgegevens door het uitvoeren van een voorbeeld van toepassing op Pi.
* Klik hier voor meer informatie over het verzenden van gegevens naar uw IoT hub.

## <a name="what-you-need"></a>Wat u nodig hebt

![Wat u nodig hebt](./media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* Een bord Raspberry Pi 2 of Raspberry Pi 3.
* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
* Een monitor, een USB-toetsenbord en muis die verbinding met Pi maakt.
* Een Mac of een computer die door Windows of Linux wordt uitgevoerd.
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
> Als u de optionele items niet hebt, kunt u gesimuleerde sensorgegevens gebruiken.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Voor IoT hub-verbindingsreeks ophalen

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="set-up-raspberry-pi"></a>Raspberry Pi instellen

### <a name="install-the-raspbian-operating-system-for-pi"></a>Het besturingssysteem Raspbian voor Pi installeren

Bereid de microSD-kaart voor de installatie van de installatiekopie van het Raspbian.

1. Download Raspbian.

   a. [Download Raspbian Stretch](http://downloads.raspberrypi.org/raspbian/images/raspbian-2017-07-05/) (het ZIP-bestand).

   > [!WARNING]
   > Gebruik bovenstaande koppeling downloaden `raspbian-2017-07-5` zip-installatiekopie. De nieuwste versie van Raspbian installatiekopieën heeft enkele bekende problemen met bedrading-Pi-knooppunt, wat leiden fouten in de volgende stappen tot kan.
 
   b. Pak de installatiekopie van het Raspbian naar een map op uw computer.

2. Installeer Raspbian naar de microSD-kaart.

   a. [Download en installeer het hulpprogramma Etcher SD-kaart brander](https://etcher.io/).

   b. Voer Etcher en selecteer de Raspbian-installatiekopie die u hebt opgehaald in stap 1.

   c. Selecteer het station microSD-kaart. Etcher heeft mogelijk al het juiste station geselecteerd.

   d. Klik op Flash voor het installeren van Raspbian naar de microSD-kaart.

   e. De microSD-kaart van de computer verwijderen wanneer de installatie is voltooid. Het is veilig verwijderen van de microSD-kaart rechtstreeks omdat Etcher automatisch uitwerpen of losgekoppeld van de microSD-kaart na voltooiing.

   f. Plaats de microSD-kaart in Pi.

### <a name="enable-ssh-and-i2c"></a>SSH en I2C inschakelen

1. Pi verbinden met de monitor, toetsenbord en muis. 

2. Start Pi en meld u in Raspbian met behulp van `pi` als de gebruikersnaam en `raspberry` als het wachtwoord.

3. Klik op het pictogram Raspberry > **voorkeuren** > **Raspberry Pi configuratie**.

   ![Het menu Raspbian voorkeuren](./media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

4. Op de **Interfaces** tabblad **I2C** en **SSH** naar **inschakelen**, en klik vervolgens op **OK**. Als u geen fysieke sensoren hebt en gesimuleerde sensorgegevens wilt gebruiken, is deze stap is optioneel.

   ![I2C en SSH op Raspberry Pi inschakelen](./media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
> Om in te schakelen SSH en I2C, vindt u meer referentiedocumenten op [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) en [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).

### <a name="connect-the-sensor-to-pi"></a>Verbinding maken met de sensor Pi

Gebruik de breadboard en meestal kabels verbinden met een LED en een BME280 Pi als volgt. Als u geen de sensor [deze sectie overslaan](#connect-pi-to-the-network).

![De sensor en Raspberry Pi-verbinding](./media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

De sensor BME280 kunt temperatuur en vochtigheid gegevens verzamelen. De LED knippert wanneer het apparaat een bericht naar de cloud verzendt. 

Voor sensor pincodes, gebruikt u de volgende bedrading:

| Start (Sensor & LED)     | Einde (bord)            | Kleur van de kabel   |
| -----------------------  | ---------------------- | ------------: |
| VDD (pincode 5G)             | 3, 3v PWR (pincode 1)       | Wit-kabel   |
| GND (Pin 7G)             | GND (pincode 6)            | Bruine-kabel   |
| SDI (Pin 10G)            | I2C1 SDA (Pin 3)       | Rode-kabel     |
| SCK (Pin 8G)             | I2C1 SQL (pincode 5)       | Oranje-kabel  |
| LED VDD (Pin 18F)        | GPIO 24 uur per dag (Pin 18)       | Wit-kabel   |
| LED GND (Pin 17F)        | GND (Pin 20)           | Zwarte kabel   |

Klik om weer te geven [Raspberry Pi 2 en 3 pincode toewijzingen](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) ter referentie.

Nadat u hebt BME280 is verbonden met uw Raspberry Pi, moet zijn, zoals onder afbeelding.

![Verbonden Pi en BME280](./media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Pi verbinding met het netwerk

Pi inschakelen met behulp van de micro USB-kabel en de voeding. De Ethernet-kabel gebruiken om te verbinden met het bekabelde netwerk Pi of gaat u als volgt de [instructies van de basis van Raspberry Pi](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi verbinden met het draadloze netwerk. Nadat uw Pi is verbonden met het netwerk, moet u Noteer de [IP-adres van uw Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Verbonden met het bekabelde netwerk](./media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Zorg ervoor dat Pi is verbonden met hetzelfde netwerk bevinden als uw computer. Bijvoorbeeld, als uw computer is verbonden met een draadloos netwerk terwijl Pi is verbonden met een bekabeld netwerk, ziet u mogelijk niet het IP-adres in de uitvoer devdisco.

## <a name="run-a-sample-application-on-pi"></a>Een voorbeeld van toepassing op Pi uitvoeren

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a>Kloon de voorbeeldtoepassing en installeer de vereiste pakketten

1. Verbinding maken met uw Raspberry Pi met een van de volgende SSH-clients vanaf uw hostcomputer:
   
   **Windows-gebruikers**
  
   a. Download en installeer [PuTTY](http://www.putty.org/) voor Windows. 

   b. Kopieer het IP-adres van de sectie Pi in de Host-naam (of IP-adres) en SSH als het verbindingstype selecteren.
   
   ![PuTTy](./media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac- en Ubuntu-gebruikers**
   
   Gebruik de ingebouwde SSH-client op Ubuntu- of Mac OS. Mogelijk moet u uitvoeren `ssh pi@<ip address of pi>` om Pi via SSH verbinding te maken.

   > [!NOTE] 
   > De standaardgebruikersnaam is `pi` en het wachtwoord is `raspberry`.

2. Node.js en NPM installeren op uw Pi.
   
   Controleer eerst uw Node.js-versie. 
   
   ```bash
   node -v
   ```

   Als de versie is lager dan 4.x, of als er geen Node.js op uw Pi, installeer de nieuwste versie.

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

3. Kloon de voorbeeld-App.

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

4. Installeer alle pakketten voor het voorbeeld. De installatie van de bevat Azure IoT device-SDK, BME280 Sensor-bibliotheek en bedrading Pi-bibliotheek.

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   >Het duurt enkele minuten klaar bent met het installatieproces, afhankelijk van uw netwerkverbinding.

### <a name="configure-the-sample-application"></a>De voorbeeld-App configureren

1. Open het configuratiebestand dat door het uitvoeren van de volgende opdrachten:

   ```bash
   nano config.json
   ```

   ![Configuratiebestand](./media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   Er zijn twee items in dit bestand dat u kunt configureren. Ten eerste `interval`, waarmee het tijdsinterval (in milliseconden) tussen de berichten die worden verzonden naar de cloud wordt gedefinieerd. Het tweede is `simulatedData`, dit is een Booleaanse waarde voor of gesimuleerde sensorgegevens of niet.

   Als u **hoeft niet de sensor**, stel de `simulatedData` waarde die moet worden `true` om de voorbeeld-App maken en gebruiken van gesimuleerde sensorgegevens.

2. Opslaan en sluiten door te typen besturingselement-O > Enter > Control-X.

### <a name="run-the-sample-application"></a>De voorbeeldtoepassing uitvoeren

De voorbeeldtoepassing uitvoeren met de volgende opdracht:

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   > Zorg ervoor dat u kopiëren en plakken de apparaatverbindingsreeks in enkele aanhalingstekens.


Hier ziet u de volgende uitvoer ziet u de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub.

![Uitvoer - sensorgegevens van Raspberry Pi verzonden naar uw IoT-hub](./media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a>Volgende stappen

U kunt een voorbeeld van toepassing voor het verzamelen van gegevens en verzenden naar uw IoT-hub hebt uitgevoerd. Zie voor de berichten die uw Raspberry Pi naar uw IoT-hub of verzenden berichten naar uw Raspberry Pi verzonden de [gebruik Azure IoT Toolkit-extensie voor Visual Studio-Code voor het verzenden en ontvangen van berichten tussen uw apparaat en IoT-Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

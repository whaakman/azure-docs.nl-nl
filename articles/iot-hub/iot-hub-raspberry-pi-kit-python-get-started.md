---
title: Raspberry Pi tot cloud (Python) - verbinding maken met Raspberry Pi met Azure IoT Hub | Microsoft Docs
description: Informatie over het installeren en verbinden van Raspberry Pi met Azure IoT Hub voor Raspberry Pi gegevens verzenden naar de Azure-cloudplatform in deze zelfstudie.
author: rangv
manager: ''
keywords: Azure iot raspberry pi, raspberry pi iot-hub, raspberry pi verzenden gegevens naar de cloud, raspberry pi naar de cloud
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 47a9b04e98337bb9ef4222b3f124d3fb063913a2
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187108"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-python"></a>Raspberry Pi verbinden met Azure IoT Hub (Python)

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

## <a name="set-up-raspberry-pi"></a>Raspberry Pi instellen

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

### <a name="enable-ssh-and-i2c"></a>SSH en I2C inschakelen

1. Pi verbinden met de monitor, toetsenbord en muis, Pi starten en meld u vervolgens aan Raspbian met behulp van `pi` als de gebruikersnaam en `raspberry` als het wachtwoord.
1. Klik op het pictogram Raspberry > **voorkeuren** > **Raspberry Pi configuratie**.

   ![Het menu Raspbian voorkeuren](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. Op de **Interfaces** tabblad **I2C** en **SSH** naar **inschakelen**, en klik vervolgens op **OK**. Als u geen fysieke sensoren hebt en gesimuleerde sensorgegevens wilt gebruiken, is deze stap is optioneel.

   ![I2C en SSH op Raspberry Pi inschakelen](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
Om in te schakelen SSH en I2C, vindt u meer referentiedocumenten op [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) en [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-the-sensor-to-pi"></a>Verbinding maken met de sensor Pi

Gebruik de breadboard en meestal kabels verbinden met een LED en een BME280 Pi als volgt. Als u geen de sensor [deze sectie overslaan](#connect-pi-to-the-network).

![De sensor en Raspberry Pi-verbinding](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

De sensor BME280 kunt temperatuur en vochtigheid gegevens verzamelen. En de LED knippert als er een communicatie tussen het apparaat en de cloud. 

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

![Verbonden Pi en BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Pi verbinding met het netwerk

Pi inschakelen met behulp van de micro USB-kabel en de voeding. De Ethernet-kabel gebruiken om te verbinden met het bekabelde netwerk Pi of gaat u als volgt de [instructies van de basis van Raspberry Pi](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi verbinden met het draadloze netwerk. Nadat uw Pi is verbonden met het netwerk, moet u Noteer de [IP-adres van uw Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Verbonden met het bekabelde netwerk](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Zorg ervoor dat Pi is verbonden met hetzelfde netwerk bevinden als uw computer. Bijvoorbeeld, als uw computer is verbonden met een draadloos netwerk terwijl Pi is verbonden met een bekabeld netwerk, ziet u mogelijk niet het IP-adres in de uitvoer devdisco.

## <a name="run-a-sample-application-on-pi"></a>Een voorbeeld van toepassing op Pi uitvoeren

### <a name="install-the-prerequisite-packages"></a>Installeer de vereiste pakketten

Gebruik een van de volgende SSH-clients vanaf uw hostcomputer verbinding maken met uw Raspberry Pi.
   
   **Windows-gebruikers**
   1. Download en installeer [PuTTY](http://www.putty.org/) voor Windows. 
   1. Kopieer het IP-adres van de sectie Pi in de Host-naam (of IP-adres) en SSH als het verbindingstype selecteren.
   
   
   **Mac- en Ubuntu-gebruikers**
   
   Gebruik de ingebouwde SSH-client op Ubuntu- of Mac OS. Mogelijk moet u uitvoeren `ssh pi@<ip address of pi>` om Pi via SSH verbinding te maken.
   > [!NOTE] 
   De standaardgebruikersnaam is `pi` , en het wachtwoord is `raspberry`.


### <a name="configure-the-sample-application"></a>De voorbeeld-App configureren

1. Kloon de voorbeeld-App met de volgende opdracht:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-python-raspberrypi-client-app.git
   ```
1. Open het configuratiebestand dat door het uitvoeren van de volgende opdrachten:

   ```bash
   cd iot-hub-python-raspberrypi-client-app
   nano config.py
   ```

   Er zijn 5 macro's in dit bestand kunt u configurate. Ten eerste `MESSAGE_TIMESPAN`, waarmee wordt het tijdsinterval (in milliseconden) gedefinieerd tussen twee berichten verzenden naar de cloud. De tweede waarde `SIMULATED_DATA`, dit is een Booleaanse waarde voor of gesimuleerde sensorgegevens of niet. `I2C_ADDRESS` is het adres I2C die is verbonden met uw BME280 sensor. `GPIO_PIN_ADDRESS` is het adres GPIO voor uw LED. Het laatste item `BLINK_TIMESPAN`, die de timespan gedefinieerd wanneer uw LED is ingeschakeld in milliseconden.

   Als u **hoeft niet de sensor**, stel de `SIMULATED_DATA` waarde die moet worden `True` om de voorbeeld-App maken en gebruiken van gesimuleerde sensorgegevens.

1. Opslaan en sluiten door te drukken besturingselement-O > Enter > Control-X.

### <a name="build-and-run-the-sample-application"></a>De voorbeeldtoepassing bouwen en uitvoeren

1. De voorbeeldtoepassing met de volgende opdracht. Omdat de Azure IoT SDK's voor Python wrappers boven op de Azure IoT-SDK voor C van apparaat, moet u de C-bibliotheken worden gecompileerd als u wilt of nodig hebt voor het genereren van de Python-bibliotheken van de broncode.

   ```bash
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```
   > [!NOTE] 
   U kunt ook opgeven de versie die u wilt dat door het uitvoeren van `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`. Als u een script zonder parameter uitvoert, het script automatisch detecteren welke versie van python geïnstalleerd (zoekvolgorde 2.7 3.4 -> 3.5 ->). Zorg ervoor dat uw Python-versie consistent blijft tijdens het bouwen en uitvoeren. 
   
   > [!NOTE] 
   Op het bouwen van de Python-clientbibliotheek (iothub_client.so) op Linux-apparaten die minder dan 1 GB RAM-geheugen, ziet u mogelijk bouwen zitten op 98% tijdens het maken van iothub_client_python.cpp zoals hieronder wordt weergegeven `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`. Als u dit probleem ondervindt, controleert u het geheugengebruik van het apparaat via `free -m command` in een andere terminalvenster gedurende die tijd. Als u onvoldoende geheugen tijdens het compileren van iothub_client_python.cpp bestand uitvoert, hebt u mogelijk tijdelijk vergroten de wisselruimte als u meer beschikbaar geheugen op het apparaat in de Python-client-side SDK-bibliotheek met succes te bouwen.
   
1. De voorbeeldtoepassing uitvoeren met de volgende opdracht:

   ```bash
   python app.py '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Zorg ervoor dat u kopiëren en plakken de apparaatverbindingsreeks in enkele aanhalingstekens. En als u de python 3, dan kunt u de opdracht `python3 app.py '<your Azure IoT hub device connection string>'`.


   Hier ziet u de volgende uitvoer ziet u de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub.

   ![Uitvoer - sensorgegevens van Raspberry Pi verzonden naar uw IoT-hub](media/iot-hub-raspberry-pi-kit-c-get-started/success.png
)

## <a name="next-steps"></a>Volgende stappen

U kunt een voorbeeld van toepassing voor het verzamelen van gegevens en verzenden naar uw IoT-hub hebt uitgevoerd. Zie voor de berichten die uw Raspberry Pi naar uw IoT-hub of verzenden berichten naar uw Raspberry Pi in een opdrachtregelinterface verzonden de [met iothub-explorer-zelfstudie berichten op cloudapparaten beheren](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

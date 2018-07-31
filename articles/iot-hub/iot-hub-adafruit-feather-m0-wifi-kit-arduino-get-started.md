---
title: M0 naar de cloud--Feather M0 WiFi verbindt met Azure IoT Hub | Microsoft Docs
description: Informatie over het instellen en Adafruit Feather M0 WiFi verbinden met Azure IoT Hub kunt u gegevens verzenden naar de Azure-cloudplatform in deze zelfstudie.
author: rangv
manager: nasing
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: c9d786651198eeffd839f52b3d3c8e6c1a1168c4
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345417"
---
# <a name="connect-adafruit-feather-m0-wifi-to-azure-iot-hub-in-the-cloud"></a>Adafruit Feather M0 WiFi verbinden met Azure IoT Hub in de cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Verbinding tussen een BME280, Feather M0 WiFi en IoT-Hub](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

In deze zelfstudie begint u door te leren van de basisbeginselen van het werken met uw Arduino-bord. Vervolgens leert u hoe u uw apparaten naadloos verbinding naar de cloud met behulp van [Azure IoT Hub](about-iot-hub.md).

## <a name="what-you-do"></a>Wat u allemaal doen

Adafruit Feather M0 WiFi verbinden met een IoT-hub die u maakt. Vervolgens voert u een voorbeeld van toepassing op M0 WiFi de temperatuur en vochtigheid gegevens verzamelen uit een BME280. Ten slotte, stuurt u de sensorgegevens naar uw IoT-hub.


## <a name="what-you-learn"></a>Wat u leert

* Over het maken van een IoT-hub en een apparaat registreren voor Feather M0 WiFi
* Feather M0 WiFi verbinden met de sensor en uw computer
* Over het verzamelen van gegevens door te voeren van een voorbeeld van toepassing op Feather M0 WiFi
* Over het verzenden van de sensorgegevens naar uw IoT-hub

## <a name="what-you-need"></a>Wat u nodig hebt

![Onderdelen die nodig zijn voor de zelfstudie](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

Als u wilt deze bewerking is voltooid, moet u de volgende onderdelen van uw Feather M0 WiFi-startpakket:

* Het bord Feather M0 WiFi
* Een Micro USB-Type een USB-kabel

U moet ook de volgende bewerkingen voor uw ontwikkelomgeving:

* Een actief Azure-abonnement. Als u een Azure-account geen [Azure een gratis proefaccount maken](https://azure.microsoft.com/free/) in een paar minuten.
* Een Mac of een PC met Windows- of Ubuntu.
* Een draadloos netwerk voor Feather M0 WiFi-verbinding maken met.
* Een internetverbinding beschikken om het downloaden van het configuratieprogramma.
* [Arduino IDE](https://www.arduino.cc/en/main/software) versie 1.6.8 of hoger. Eerdere versies werken niet met de Azure IoT Hub-bibliotheek.

Als u geen een sensor, zijn de volgende items zijn optioneel. U hebt ook de mogelijkheid van het gebruik van gesimuleerde sensorgegevens:

* Een BME280 temperatuur en vochtigheid sensor
* Een breadboard
* M/M meestal kabels

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-the-sensor-and-your-computer"></a>Feather M0 WiFi verbinden met de sensor en uw computer

In deze sectie maakt verbinding u de sensoren met het mededelingenbord. Vervolgens aansluit u uw apparaat op uw computer om verder gebruik.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-m0-wifi"></a>Verbinding maken met een DHT22 temperatuur en vochtigheid sensor Feather M0 WiFi

Gebruik de kabels breadboard en meestal om de verbinding te maken. Als u geen een sensor, deze sectie overslaan omdat u gesimuleerde sensorgegevens in plaats daarvan kunt gebruiken.

![Naslaginformatie over verbindingen](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


Voor sensor pincodes, gebruikt u de volgende bedrading:


| Start (sensor)           | Einde (bord)            | Kleur van de kabel   |
| -----------------------  | ---------------------- | ------------- |
| VDD (Pin 27A)            | 3v (pincode 3A)            | Rode-kabel     |
| GND (Pin 29 bis)            | GND (pincode 6A)           | Zwarte kabel   |
| SCK (Pin 30)            | SCK (Pin 12A)          | Gele-kabel  |
| SDO (Pin 31 bis)            | MI (Pin 14A)           | Wit-kabel   |
| SDI (Pin 32 bis)            | M0 (Pin 13 bis)           | Blauwe-kabel    |
| CS (Pin 33A)             | GPIO 5 (Pin 15J)       | Oranje-kabel  |

Zie voor meer informatie, [Adafruit BME280 vochtigheid, atmosferische druk druk + temperatuur Sensor groepen](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) en [Adafruit Feather M0 WiFi pin-outs gegeven](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).

Nu moeten uw Feather M0 WiFi zijn verbonden met een sensor werken.

![Verbinding maken met DHT22 met Feather Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-to-your-computer"></a>Feather M0 WiFi aansluiten op uw computer

Gebruik de Micro USB-poort Type A USB-kabel op Feather M0 WiFi aansluit op uw computer, zoals wordt weergegeven:

![Feather Huzzah aansluiten op uw computer](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Seriële poort machtigingen (alleen Ubuntu) toevoegen

Als u Ubuntu gebruikt, zorg er dan voor dat u hebt de machtigingen om te werken op de USB-poort van Feather M0 WiFi. Als u wilt toevoegen de machtigingen van de seriële poort, de volgende stappen uit:


1. Voer bij een terminal de volgende opdrachten:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   U kunt een van de volgende uitvoer:

   * CRW-rw---1 hoofdmap uucp xxxxxxxx
   * CRW-rw---1 hoofdmap bellen xxxxxxxx

   In de uitvoer ziet u dat `uucp` of `dialout` is de naam van de eigenaar van de USB-poort.

2. De gebruiker toevoegen aan de groep, kunt u de volgende opdracht uitvoeren:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   In de vorige stap hebt u de naam van de groep eigenaar hebt verkregen `<group-owner-name>`. De naam van de Ubuntu-gebruiker is `<username>`.

3. Voor de wijziging worden weergegeven, Ubuntu afmelden en vervolgens weer aanmelden.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Gegevens verzamelen en te verzenden naar uw IoT-hub

In deze sectie maakt u implementeren en uitvoeren van een voorbeeld van toepassing op Feather M0 WiFi. De voorbeeld-App kunt de blink LED op Feather M0 WiFi. Vervolgens stuurt de temperatuur en vochtigheid gegevens verzameld van de sensor BME280 naar uw IoT hub.

### <a name="get-the-sample-application-from-github-and-prepare-the-arduino-ide"></a>De voorbeeldtoepassing ophalen van GitHub en voorbereiden van de Arduino IDE

De voorbeeldtoepassing wordt gehost op GitHub. Kloon de voorbeeldopslagplaats met de voorbeeld-App vanuit GitHub. Als u wilt de voorbeeldopslagplaats klonen, de volgende stappen uit:

1. Open een opdrachtprompt of een terminal-venster.

2. Ga naar een map waar u de voorbeeldtoepassing om te worden opgeslagen.

3. Voer de volgende opdracht uit:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-the-package-for-feather-m0-wifi-in-the-arduino-ide"></a>Installeer het pakket voor Feather M0 WiFi in de Arduino IDE

1. Open de map waarin de voorbeeldtoepassing is opgeslagen.

2. Open het bestand app.ino in de app-map in de Arduino IDE.

   ![De voorbeeldtoepassing openen in Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


3. Klik op **bestand** > **voorkeuren** (Windows/Linux) of **Arduino** > **voorkeuren** (Mac) en kopieer en Plak de koppeling hieronder in de **extra borden Manager-URL's** optie in de voorkeuren Arduino IDE.
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

4. Klik op **extra** > **bord** > **borden Manager**, en installeer vervolgens de `Arduino SAMD Boards` versie `1.6.2` of hoger. 

5. Installeer vervolgens in het venster dezelfde `Adafruit SAMD Boards` pakket naar het bestand bord definities hebt toegevoegd.

   ![Het pakket esp8266 is geïnstalleerd](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

6. Klik op **extra** > **bord** > **Adafruit M0 WiFi**.

7. Installeren van stuurprogramma's (voor Windows). Wanneer u Feather M0 WiFi aansluit, moet u mogelijk een stuurprogramma hebt geïnstalleerd. Klik op [de downloadkoppeling op de webpagina](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) voor het downloaden van het installatieprogramma stuurprogramma. Volg de stappen om de stuurprogramma's die u wilt installeren.

### <a name="install-necessary-libraries"></a>Vereiste bibliotheken installeren

1. Klik in de Arduino IDE op **schets** > **bibliotheek opnemen** > **bibliotheken beheren**.

2. Zoek naar de volgende bibliotheek de naam van één voor één. Voor elke bibliotheek die u vindt, klikt u op **installeren**:

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. Handmatig installeren `Adafruit_WINC1500`. Ga naar de [Adafruit_WINC1500 webpagina](https://github.com/adafruit/Adafruit_WINC1500) en klikt u op **klonen of downloaden** > **ZIP downloaden**. In uw Arduino IDE, gaat u naar **schets** > **bibliotheek opnemen** > **.zip bibliotheek toevoegen** en toevoegen van het zip-bestand.

### <a name="use-the-sample-application-if-you-dont-have-a-real-bme280-sensor"></a>De voorbeeld-App gebruiken als u geen een echte BME280 sensor

Als u een echte BME280 sensor geen hebt, kunt de voorbeeldtoepassing temperatuur en vochtigheid gegevens simuleren. Als u de voorbeeldtoepassing om te gebruiken van gesimuleerde gegevens instelt, de volgende stappen uit:

1. Open de `config.h` -bestand in de `app` map.

2. Zoek de volgende regel code en wijzig de waarde van `false` naar `true`.

   ```c
   define SIMULATED_DATA true
   ```

   ![Configureert de voorbeeldtoepassing gesimuleerde gegevens gebruiken](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. Klik op `Control-s` op te slaan.

### <a name="deploy-the-sample-application-to-feather-m0-wifi"></a>De voorbeeldtoepassing om te Feather M0 WiFi implementeren

1. Klik in de Arduino IDE op **hulpprogramma** > **poort**, en klik vervolgens op de seriële poort voor Feather M0 WiFi.

2. Klik op **schets** > **uploaden** te bouwen en implementeren van de voorbeeldtoepassing om te Feather M0 WiFi.

### <a name="enter-your-credentials"></a>Voer uw referenties in

Nadat het uploaden voltooid is, volgt u deze stappen om uw referenties invoeren:

1. Klik in de Arduino IDE op **extra** > **seriële Monitor**.

2. Selecteer in de rechterbenedenhoek van het venster seriële monitor **geen einde van regel** in de vervolgkeuzelijst aan de linkerkant.

3. Selecteer **115200 baudrate** in de vervolgkeuzelijst aan de rechterkant.

4. In het invoervak bovenaan, voer de volgende informatie als u wordt gevraagd om te verstrekken en klikt u op **verzenden**:

   * Wi-Fi-SSID
   * Wi-Fi-wachtwoord
   * Apparaat-verbindingsreeks

> [!Note]
> De referentie-informatie wordt opgeslagen in de EEPROM van Feather M0 WiFi. Als u op de knop opnieuw instellen op het prikbord van Feather M0 WiFi klikt, wordt de voorbeeldtoepassing gevraagd als u wilt de gegevens wissen. Voer `Y` om te wissen van de gegevens. U wordt gevraagd een tweede keer gegevens op te geven.

### <a name="verify-that-the-sample-application-is-running-successfully"></a>Controleer of de voorbeeldtoepassing met succes is uitgevoerd

Als u de volgende uitvoer van het venster seriële monitor en de knipperende LED op Feather M0 WiFi ziet, wordt de voorbeeldtoepassing is uitgevoerd:

![Uiteindelijke uitvoer in Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a>Volgende stappen

U hebt Feather M0 WiFi verbonden met uw IoT-hub en de vastgelegde gegevens verzonden naar uw IoT-hub. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]


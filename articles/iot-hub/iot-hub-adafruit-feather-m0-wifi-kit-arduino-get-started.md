---
title: 'M0 naar cloud: doezelaar M0 Wi-Fi verbinding met Azure IoT Hub | Microsoft Docs'
description: Informatie over het instellen en Adafruit Doezelaar M0 Wi-Fi verbinding met Azure IoT Hub gegevens verzenden naar het Azure-cloud-platform in deze zelfstudie.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/16/2017
ms.author: xshi
ms.openlocfilehash: 9b278735ce3af9e6e61a85c5e95ea218622361c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-adafruit-feather-m0-wifi-to-azure-iot-hub-in-the-cloud"></a>Adafruit Doezelaar M0 Wi-Fi verbinding met Azure IoT Hub in de cloud
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Verbinding tussen een BME280, Doezelaar M0 Wi-Fi en IoT-Hub](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

In deze zelfstudie maakt beginnen u door te leren over het werken met uw Arduino het mededelingenbord. Vervolgens leert u hoe u uw apparaten probleemloos verbinding met de cloud via [Azure IoT Hub](iot-hub-what-is-iot-hub.md).

## <a name="what-you-do"></a>Wat u doet

Adafruit Doezelaar M0 Wi-Fi verbinding met een IoT-hub die u maakt. Vervolgens voert u een voorbeeld van toepassing op M0 Wi-Fi de temperatuur en vochtigheid gegevens te verzamelen van een BME280. U kunt ten slotte de sensorgegevens verzendt naar uw IoT-hub.


## <a name="what-you-learn"></a>Wat u leert

* Het maken van een IoT-hub en een apparaat registreren voor Doezelaar M0 Wi-Fi
* Hoe Doezelaar M0 Wi-Fi verbinding met de sensor en uw computer
* Het verzamelen van sensorgegevens door het uitvoeren van een voorbeeld van toepassing op Doezelaar M0 Wi-Fi
* Hoe de sensorgegevens verzendt naar uw IoT-hub

## <a name="what-you-need"></a>Wat u nodig hebt

![Onderdelen die nodig zijn voor de zelfstudie](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

Om deze bewerking niet voltooien, moet u de volgende onderdelen van uw Doezelaar M0 Wi-Fi Starter Kit:

* De doezelaar M0 Wi-Fi-kaart
* Een Micro USB-Type A USB-kabel

U moet ook de volgende bewerkingen voor uw ontwikkelomgeving:

* Een actief Azure-abonnement. Als u een Azure-account geen [maken van een gratis Azure-proefaccount](https://azure.microsoft.com/free/) over een paar minuten.
* Een Mac of een PC met Windows of Ubuntu.
* Een draadloos netwerk voor Doezelaar M0 Wi-Fi verbinding maken met.
* Een internetverbinding beschikken om het downloaden van het hulpprogramma voor serverconfiguratie.
* [Arduino IDE](https://www.arduino.cc/en/main/software) versie 1.6.8 of hoger. Eerdere versies werkt niet met de Azure IoT Hub-bibliotheek.

Als u een sensor hebt, zijn de volgende items zijn optioneel. U hebt ook de mogelijkheid van het gebruik van gesimuleerde sensorgegevens:

* Een BME280 temperatuur en vochtigheid sensor
* Een breadboard
* M/M meestal kabels

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-the-sensor-and-your-computer"></a>Doezelaar M0 Wi-Fi verbinding met de sensor en uw computer
In deze sectie kunt u de sensoren verbinding met het mededelingenbord. Vervolgens aansluit u uw apparaat op uw computer voor gebruik.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-m0-wifi"></a>Verbinding maken met een DHT22 temperatuur en vochtigheid sensor Doezelaar M0 Wi-Fi

Gebruik de bedrading breadboard en meestal de verbinding te maken. Als u geen een sensor, deze sectie overslaan omdat u gesimuleerde sensorgegevens in plaats daarvan kunt gebruiken.

![Verbindingen verwijzing](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


Gebruik de volgende bedrading voor sensor-pincodes:


| Start (sensor)           | Einde (BMC)            | Kleur van de kabel   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 27A)            | 3v (3A pincode)            | Rode-kabel     |
| GND (Pin 29 bis)            | GND (6A pincode)           | Zwarte kabel   |
| SCK (pincode 30)            | SCK (Pin 12A)          | Gele-kabel  |
| SDO (Pin 31A)            | MI (Pin 14A)           | Witte kabel   |
| SDI (Pin 32 bis)            | M0 (Pin 13A)           | Blauw-kabel    |
| CS (Pin 33A)             | GPIO 5 (Pin 15J)       | Oranje-kabel  |

Zie voor meer informatie [Adafruit BME280 vochtigheid + barometerdruk + temperatuur Sensor evenement](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) en [Adafruit Doezelaar M0 Wi-Fi pin-outs gegeven](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).



Nu moeten uw Doezelaar M0 Wi-Fi zijn verbonden met een sensor werken.

![DHT22 verbinden met Doezelaar Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-to-your-computer"></a>Doezelaar M0 Wi-Fi aansluiten op uw computer

Gebruik de Micro USB-poort Type A USB-kabel Doezelaar M0 Wi-Fi verbinding te maken met uw computer, zoals wordt weergegeven:

![Doezelaar Huzzah aansluiten op uw computer](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Machtigingen van de seriële poort (alleen Ubuntu) toevoegen

Als u Ubuntu gebruikt, zorg er dan voor dat u gemachtigd bent om te werken op het USB-poort van Doezelaar M0 WiFi-netwerk. Als u wilt toevoegen seriële poort machtigingen, de volgende stappen uit:


1. Voer de volgende opdrachten in een terminal:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   U kunt een van de volgende uitvoer:

   * CRW-rw---1 hoofdmap uucp xxxxxxxx
   * CRW-rw---1 hoofdmap bellen xxxxxxxx

   U ziet dat in de uitvoer `uucp` of `dialout` is de naam van de eigenaar van de USB-poort.

2. De gebruiker toevoegen aan de groep, voer de volgende opdracht:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   In de vorige stap hebt u de naam van de eigenaar verkregen `<group-owner-name>`. De gebruikersnaam Ubuntu `<username>`.

3. Voor de wijziging wilt weergeven, meld u af bij Ubuntu en vervolgens weer aanmelden.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Sensorgegevens verzamelen en verzenden naar uw IoT-hub

In deze sectie die u kunt implementeren en uitvoeren van een voorbeeld van toepassing op Doezelaar M0 Wi-Fi. De voorbeeldtoepassing maakt de knipperen LED op Doezelaar M0 Wi-Fi. Deze stuurt de temperatuur en vochtigheid gegevens verzameld van de sensor BME280 naar uw IoT-hub.

### <a name="get-the-sample-application-from-github-and-prepare-the-arduino-ide"></a>Ophalen van de voorbeeldtoepassing uit GitHub en de Arduino IDE voorbereiden

De voorbeeldtoepassing wordt gehost op GitHub. Kloon de opslagplaats voorbeeld waarin de voorbeeldtoepassing vanuit GitHub. De voorbeeld-opslagplaats klonen, de volgende stappen uit:

1. Open een opdrachtprompt of een terminalvenster.

2. Ga naar een map waar u de voorbeeldtoepassing moet worden opgeslagen.
3. Voer de volgende opdracht uit:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-the-package-for-feather-m0-wifi-in-the-arduino-ide"></a>Installeer het pakket voor Doezelaar M0 Wi-Fi in de Arduino IDE

1. Open de map waar de voorbeeldtoepassing wordt opgeslagen.

2. Open het bestand app.ino in de map van de app in de Arduino IDE.

   ![De voorbeeldtoepassing in Arduino IDE openen](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. Klik op **bestand** > **voorkeuren** (Windows of Linux) of **Arduino** > **voorkeuren** (Mac) en kopieer en plak de onderstaande koppeling naar de **extra Boards Manager-URL's** optie in de Arduino IDE-voorkeuren.
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. Klik op **extra** > **mededelingenbord** > **Boards Manager**, en installeer vervolgens de `Arduino SAMD Boards` versie `1.6.2` of hoger. 

1. Installeer in hetzelfde venster `Adafruit SAMD Boards` pakket de definities van het mededelingenbord bestand toevoegen.

   ![Het pakket esp8266 is geïnstalleerd](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. Klik op **extra** > **mededelingenbord** > **Adafruit M0 Wi-Fi**.

5. Stuurprogramma's installeren (voor Windows). Wanneer u Doezelaar M0 Wi-Fi aansluit, moet u mogelijk om een stuurprogramma te installeren. Klik op [de downloadkoppeling op de webpagina](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) voor het downloaden van het installatieprogramma stuurprogramma. Volg de stappen om de stuurprogramma's die u wilt installeren.

### <a name="install-necessary-libraries"></a>Vereiste bibliotheken installeren

1. Klik in de IDE Arduino **schema** > **bibliotheek omvatten** > **bibliotheken beheren**.

2. Zoeken naar de volgende bibliotheek benoemt één voor één. Voor elke bibliotheek die u vindt, klikt u op **installeren**:

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. Handmatig installeren `Adafruit_WINC1500`. Ga naar [deze website](https://github.com/adafruit/Adafruit_WINC1500) en klik op **klonen of downloaden** > **ZIP downloaden**. In uw Arduino IDE, gaat u naar **schema** > **bibliotheek omvatten** > **.zip bibliotheek toevoegen** en voeg het zip-bestand.

### <a name="use-the-sample-application-if-you-dont-have-a-real-bme280-sensor"></a>De voorbeeldtoepassing gebruiken als u een echte BME280-temperatuursensor hebt

Als u een echte BME280 sensor geen hebt, kunt de voorbeeldtoepassing temperatuur en vochtigheid gegevens simuleren. U stelt de voorbeeldtoepassing gesimuleerde gegevens gebruiken door de volgende stappen uit:

1. Open de `config.h` bestand de `app` map.

2. Zoek de volgende regel code en wijzig de waarde van `false` naar `true`:

   ```c
   define SIMULATED_DATA true
   ```
   ![Configureren van de voorbeeldtoepassing gesimuleerde gegevens gebruiken](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. Sla het bestand met `Control-s`.

### <a name="deploy-the-sample-application-to-feather-m0-wifi"></a>De voorbeeldtoepassing met Doezelaar M0 Wi-Fi implementeren

1. Klik in de IDE Arduino **hulpprogramma** > **poort**, en klik vervolgens op de seriële poort voor Doezelaar M0 Wi-Fi.

2. Klik op **schema** > **uploaden** kunnen bouwen en implementeren van de voorbeeldtoepassing met Doezelaar M0 Wi-Fi.

### <a name="enter-your-credentials"></a>Voer uw referenties in

Nadat het uploaden voltooid is, voert u deze stappen om uw referenties invoeren:

1. Klik in de IDE Arduino **extra** > **seriële Monitor**.

2. Selecteer in de rechterbenedenhoek van het venster seriële monitor **er is geen afsluitende regel** in de vervolgkeuzelijst aan de linkerkant.
3. Selecteer **115200 baud** in de vervolgkeuzelijst aan de rechterkant.
4. In het invoervak bovenaan, voer de volgende informatie als u wordt gevraagd om deze en klikt u op **verzenden**:

   * Wi-Fi-SSID
   * Wi-Fi-wachtwoord
   * Apparaat-verbindingsreeks

> [!Note]
> De referentie-informatie wordt opgeslagen in het EEPROM van Doezelaar M0 WiFi-netwerk. Als u op de knop herstellen op het mededelingenbord Doezelaar M0 Wi-Fi klikt, wordt de voorbeeldtoepassing gevraagd als u wilt de gegevens wissen. Voer `Y` om te wissen van de gegevens. U wordt gevraagd om de informatie van een tweede keer te geven.

### <a name="verify-that-the-sample-application-is-running-successfully"></a>Controleer of de voorbeeldtoepassing met succes is uitgevoerd

Als u de volgende uitvoer van het monitorvenster seriële en de knipperende LED op Doezelaar M0 Wi-Fi ziet, wordt de voorbeeldtoepassing is uitgevoerd:

![Uiteindelijke uitvoer Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a>Volgende stappen

U hebt Doezelaar M0 Wi-Fi verbonden met uw IoT-hub en de vastgelegde sensorgegevens verzonden naar uw IoT-hub. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]


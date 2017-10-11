---
title: ESP8266 naar cloud - Doezelaar HUZZAH ESP8266 verbinden met Azure IoT Hub | Microsoft Docs
description: Informatie over het instellen en Adafruit Doezelaar HUZZAH ESP8266 verbinden met Azure IoT Hub voor gegevens verzenden naar het Azure-cloud-platform in deze zelfstudie.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: c505aacf-89a8-40ed-a853-493b75bec524
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: xshi
ms.openlocfilehash: 6a450579c848fe6030a328ddf410f139baae2324
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Adafruit Doezelaar HUZZAH ESP8266 verbinden met Azure IoT Hub in de cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Verbinding tussen DHT22 Doezelaar HUZZAH ESP8266 en IoT-Hub](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Wat u doet


Verbinding maken met Adafruit Doezelaar HUZZAH ESP8266 naar een IoT-hub die u maakt. Vervolgens voert u een voorbeeld van toepassing op ESP8266 de temperatuur en vochtigheid om gegevens te verzamelen van een sensor DHT22. U kunt ten slotte de sensorgegevens verzendt naar uw IoT-hub.

> [!NOTE]
> Als u andere boards ESP8266 gebruikt, kunt u deze stappen om te verbinden met uw IoT-hub nog steeds volgen. Afhankelijk van de ESP8266 bestuur u gebruikt, moet u mogelijk opnieuw configureren de `LED_PIN`. Bijvoorbeeld, als u ESP8266 van AI-Thinker, u wijzigt in `0` naar `2`. Heb je nog een kit? Ophalen van de [Azure-website](http://azure.com/iotstarterkits).




## <a name="what-you-learn"></a>Wat u leert

* Het maken van een IoT-hub en een apparaat registreren voor Doezelaar HUZZAH ESP8266
* Hoe Doezelaar HUZZAH ESP8266 verbinding met de sensor en uw computer
* Het verzamelen van sensorgegevens door het uitvoeren van een voorbeeld van toepassing op Doezelaar HUZZAH ESP8266
* Hoe de sensorgegevens verzendt naar uw IoT-hub

## <a name="what-you-need"></a>Wat u nodig hebt

![Onderdelen die nodig zijn voor de zelfstudie](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

Om deze bewerking niet voltooien, moet u de volgende onderdelen van uw Doezelaar HUZZAH ESP8266 Starter Kit:

* De doezelaar HUZZAH ESP8266-kaart
* Een Micro USB-Type A USB-kabel

U moet ook de volgende bewerkingen voor uw ontwikkelomgeving:

* Een actief Azure-abonnement. Als u een Azure-account geen [maken van een gratis Azure-proefaccount](https://azure.microsoft.com/free/) over een paar minuten.
* Mac of PC met Windows of Ubuntu.
* Doezelaar HUZZAH ESP8266 verbinding maken met draadloze netwerk.
* Internet-verbinding voor het downloaden van het hulpprogramma voor serverconfiguratie.
* [Arduino IDE](https://www.arduino.cc/en/main/software) versie 1.6.8 of hoger. Eerdere versies werkt niet met de AzureIoT-bibliotheek.

De volgende items zijn optioneel als u een sensor geen hebt. U hebt ook de optie van het gebruik van gesimuleerde sensorgegevens.

* Een Adafruit DHT22 temperatuur en vochtigheid-temperatuursensor
* Een breadboard
* M/M meestal kabels


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>Doezelaar HUZZAH ESP8266 verbinden met de sensor en uw computer
In deze sectie kunt u de sensoren verbinding met het mededelingenbord. Vervolgens aansluit u uw apparaat op uw computer voor gebruik.
### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>Verbinding maken met een DHT22 temperatuur en vochtigheid sensor Doezelaar HUZZAH ESP8266

Gebruik de bedrading breadboard en meestal als volgt verbinding te maken. Als u geen een sensor, deze sectie overslaan omdat u gesimuleerde sensorgegevens in plaats daarvan kunt gebruiken.

![Verbindingen verwijzing](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


Gebruik de volgende bedrading voor sensor-pincodes:


| Start (Sensor)           | Einde (BMC)           | Kleur van de kabel   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 31F)            | 3v (vastmaken 58H)           | Rode-kabel     |
| GEGEVENS (Pin 32F)           | GPIO 2 (Pin 46)       | Blauw-kabel    |
| GND (Pin 34F)            | GND (PIn 56I)          | Zwarte kabel   |

Zie voor meer informatie [Adafruit DHT22 sensor setup](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) en [Adafruit Doezelaar HUZZAH Esp8266 pin-outs gegeven](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).



Nu moeten uw Doezelaar Huzzah ESP8266 zijn verbonden met een sensor werken.

![DHT22 verbinden met Doezelaar Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Doezelaar HUZZAH ESP8266 aansluiten op uw computer

Zoals u volgende, gebruikt u de USB Micro naar Type een USB-kabel Doezelaar HUZZAH ESP8266 aansluiten op uw computer.

![Doezelaar Huzzah aansluiten op uw computer](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Machtigingen van de seriële poort (alleen Ubuntu) toevoegen


Als u Ubuntu gebruikt, zorg er dan voor dat u gemachtigd bent om te werken op de USB-poort van Doezelaar HUZZAH ESP8266. Als u wilt toevoegen seriële poort machtigingen, de volgende stappen uit:


1. Voer de volgende opdrachten in een terminal:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   U kunt een van de volgende uitvoer:

   * CRW-rw---1 hoofdmap uucp xxxxxxxx
   * CRW-rw---1 hoofdmap bellen xxxxxxxx

   U ziet dat in de uitvoer `uucp` of `dialout` is de naam van de eigenaar van de USB-poort.

1. De gebruiker toevoegen aan de groep met de volgende opdracht:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`is de naam van de eigenaar die u in de vorige stap hebt verkregen. `<username>`uw gebruikersnaam Ubuntu is.

1. Meld u af bij Ubuntu en vervolgens opnieuw aan te melden om de wijziging moet worden weergegeven.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Sensorgegevens verzamelen en verzenden naar uw IoT-hub

In deze sectie die u kunt implementeren en uitvoeren van een voorbeeld van toepassing op Doezelaar HUZZAH ESP8266. De voorbeeldtoepassing de LED Doezelaar HUZZAH ESP8266 knippert en verzendt de temperatuur en vochtigheid gegevens verzameld van de sensor DHT22 naar uw IoT-hub.

### <a name="get-the-sample-application-from-github"></a>Ophalen van de voorbeeldtoepassing uit GitHub

De voorbeeldtoepassing wordt gehost op GitHub. Kloon de opslagplaats voorbeeld waarin de voorbeeldtoepassing vanuit GitHub. De voorbeeld-opslagplaats klonen, de volgende stappen uit:

1. Open een opdrachtprompt of een terminalvenster.
1. Ga naar een map waar u de voorbeeldtoepassing moet worden opgeslagen.
1. Voer de volgende opdracht uit:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

Installeer het pakket voor Doezelaar HUZZAH ESP8266 in Arduino IDE:

1. Open de map waar de voorbeeldtoepassing wordt opgeslagen.
1. Open het bestand app.ino in de map van de app in de Arduino IDE.

   ![De voorbeeldtoepassing in Arduino IDE openen](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. Klik in de IDE Arduino **bestand** > **voorkeuren**.
1. In de **voorkeuren** dialoogvenster vak, klik op het pictogram naast de **extra Boards Manager-URL's** vak.
1. Voer de volgende URL in het pop-upvenster en klik vervolgens op **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Wijs de url van een pakket in Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. In de **voorkeur** in het dialoogvenster, klikt u op **OK**.
1. Klik op **extra** > **mededelingenbord** > **Boards Manager**, en zoek vervolgens naar esp8266.

   Boards Manager geeft aan dat ESP8266 met een versie van 2.2.0 of hoger is geïnstalleerd.

   ![Het pakket esp8266 is geïnstalleerd](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. Klik op **extra** > **mededelingenbord** > **Adafruit HUZZAH ESP8266**.

### <a name="install-necessary-libraries"></a>Vereiste bibliotheken installeren

1. Klik in de IDE Arduino **schema** > **bibliotheek omvatten** > **bibliotheken beheren**.
1. Zoeken naar de volgende bibliotheek benoemt één voor één. Voor elke bibliotheek die u vindt, klikt u op **installeren**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Geen een echte DHT22 sensor?

De voorbeeldtoepassing kunt temperatuur en vochtigheid gegevens simuleren als u een echte DHT22 sensor geen hebt. U stelt de voorbeeldtoepassing gesimuleerde gegevens gebruiken door de volgende stappen uit:

1. Open de `config.h` bestand de `app` map.
1. Zoek de volgende regel code en wijzig de waarde van `false` naar `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Configureren van de voorbeeldtoepassing gesimuleerde gegevens gebruiken](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. Sla het bestand met `Control-s`.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>De voorbeeldtoepassing voor Doezelaar HUZZAH ESP8266 implementeren

1. Klik in de IDE Arduino **hulpprogramma** > **poort**, en klik vervolgens op de seriële poort voor Doezelaar HUZZAH ESP8266.
1. Klik op **schema** > **uploaden** kunnen bouwen en implementeren van de voorbeeldtoepassing voor Doezelaar HUZZAH ESP8266.

### <a name="enter-your-credentials"></a>Voer uw referenties in

Nadat het uploaden voltooid is, voert u deze stappen om uw referenties invoeren:

1. Klik in de IDE Arduino **extra** > **seriële Monitor**.
1. In het venster seriële monitor ziet u de twee lijsten in de vervolgkeuzelijst in de rechterbenedenhoek.
1. Selecteer **er is geen afsluitende regel** voor de linker vervolgkeuzelijst.
1. Selecteer **115200 baud** voor de lijst rechts vervolgkeuzelijst.
1. Voer de volgende gegevens in het invoervak zich boven aan het venster seriële monitor, als u wordt gevraagd om ze en klik vervolgens op **verzenden**.
   * Wi-Fi-SSID
   * Wi-Fi-wachtwoord
   * Apparaat-verbindingsreeks

> [!Note]
> De referentie-informatie wordt opgeslagen in de EEPROM van Doezelaar HUZZAH ESP8266. Als u op de knop herstellen op het mededelingenbord Doezelaar HUZZAH ESP8266 klikt, wordt de voorbeeldtoepassing gevraagd als u wilt de gegevens wissen. Voer `Y` te beschikken over de gegevens gewist. U wordt gevraagd om de informatie van een tweede keer te geven.

### <a name="verify-the-sample-application-is-running-successfully"></a>Controleer of dat de voorbeeldtoepassing met succes wordt uitgevoerd

Als u de volgende uitvoer van het monitorvenster seriële en de knipperende LED op Doezelaar HUZZAH ESP8266 ziet, wordt de voorbeeldtoepassing met succes uitgevoerd.

![Uiteindelijke uitvoer Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Volgende stappen

U hebt een Doezelaar HUZZAH ESP8266 verbonden met uw IoT-hub, en de vastgelegde sensorgegevens verzonden naar uw IoT-hub. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]


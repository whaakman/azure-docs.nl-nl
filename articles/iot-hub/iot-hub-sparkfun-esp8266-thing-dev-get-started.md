---
title: ESP8266 naar cloud - Sparkfun ESP8266 ding Dev verbinding maken met Azure IoT Hub | Microsoft Docs
description: Informatie over het instellen en Sparkfun ESP8266 ding Dev verbinden met Azure IoT Hub voor gegevens verzenden naar het Azure-cloud-platform in deze zelfstudie.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 587fe292-9602-45b4-95ee-f39bba10e716
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 557f0cdf375b345e0dbe0526f5a5bd3c050dec38
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="connect-sparkfun-esp8266-thing-dev-to-azure-iot-hub-in-the-cloud"></a>Sparkfun ESP8266 ding Dev verbinden met Azure IoT Hub in de cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![verbinding tussen DHT22, wat Dev en IoT-Hub](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a>Wat u doet

Verbinding maken met Sparkfun ESP8266 ding Dev naar een IoT-hub die u maakt. Voer vervolgens een voorbeeld van toepassing op ESP8266 temperatuur en vochtigheid om gegevens te verzamelen van een sensor DHT22. Ten slotte de sensorgegevens verzenden naar uw IoT-hub.

> [!NOTE]
> Als u andere boards ESP8266 gebruikt, kunt u deze stappen om te verbinden met uw IoT-hub nog steeds volgen. Afhankelijk van de ESP8266 bestuur u gebruikt, moet u mogelijk opnieuw configureren de `LED_PIN`. Bijvoorbeeld, als u ESP8266 van AI Thinker gebruikt, u kunt wijzigen in `0` naar `2`. Een kit nog geen hebt?: klik op [hier](http://azure.com/iotstarterkits)

## <a name="what-you-will-learn"></a>Wat u leert

* Het maken van een IoT-hub en een apparaat registreren voor voor ding ontwikkelaars
* Hoe ding Dev verbinding met de sensor en uw computer.
* Het verzamelen van sensorgegevens door het uitvoeren van een voorbeeld van toepassing op wat ontwikkelaars
* Hoe de sensorgegevens verzendt naar uw IoT-hub.

## <a name="what-you-will-need"></a>Wat u nodig hebt

![Onderdelen die nodig zijn voor de zelfstudie](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

Om deze bewerking niet voltooien, moet u de volgende onderdelen van uw ding Developer Starter Kit:

* De Sparkfun ESP8266 ding Dev-kaart.
* Een Micro USB-Type A USB-kabel.

Ook moet u het volgende voor uw ontwikkelomgeving:

* Een actief Azure-abonnement. Als u een Azure-account geen [maken van een gratis Azure-proefaccount](https://azure.microsoft.com/free/) over een paar minuten.
* Mac of PC met Windows of Ubuntu.
* Sparkfun ESP8266 ding Dev verbinding maken met draadloze netwerk.
* Internet-verbinding voor het downloaden van het hulpprogramma voor serverconfiguratie.
* [Arduino IDE](https://www.arduino.cc/en/main/software) versie 1.6.8 (of nieuwer), eerdere versies werkt niet met de AzureIoT-bibliotheek.

De volgende items zijn optioneel als u een sensor geen hebt. U hebt ook de optie van het gebruik van gesimuleerde sensorgegevens.

* Een Adafruit DHT22-sensor van temperatuur en vochtigheid.
* Een breadboard.
* M/M meestal bedrading.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-the-sensor-and-your-computer"></a>ESP8266 ding Dev verbinden met de sensor en uw computer

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-esp8266-thing-dev"></a>Verbinding maken met een DHT22 temperatuur en vochtigheid sensor ESP8266 ding Dev

Gebruik de bedrading breadboard en meestal als volgt verbinding te maken. Als u geen een sensor, deze sectie overslaan omdat u gesimuleerde sensorgegevens in plaats daarvan kunt gebruiken.

![Verbindingen verwijzing](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

We gebruiken de volgende bedrading voor sensor-pincodes:

| Start (Sensor)           | Einde (BMC)           | Kleur van de kabel   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 27F)            | 3v (8A pincode)           | Rode-kabel     |
| GEGEVENS (Pin 28 septies)           | GPIO 2 (9A pincode)       | Witte kabel    |
| GND (Pin 30F)            | GND (7J pincode)          | Zwarte kabel   |


- Zie voor meer informatie: [DHT22 sensor setup](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) en [Sparkfun ESP8266 ding Dev-specificatie](https://www.sparkfun.com/products/13711)

Nu moeten uw Sparkfun ESP8266 ding Dev zijn verbonden met een sensor werken.

![verbinding maken met dht22 met ESP8266 ding Dev](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-to-your-computer"></a>Sparkfun ESP8266 ding Dev aansluiten op uw computer

Gebruik de Micro USB-poort Type A USB-kabel Sparkfun ESP8266 ding Dev op uw computer als volgt verbinding te maken.

![Doezelaar huzzah aansluiten op uw computer](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a>Machtigingen van de seriële poort – Ubuntu alleen toevoegen

Als u Ubuntu gebruikt, controleert u of dat een normale gebruiker heeft de machtigingen om te werken op de USB-poort van Sparkfun ESP8266 ding ontwikkelaars Als u wilt toevoegen seriële poort machtigingen voor een normale gebruiker, de volgende stappen uit:

1. Voer de volgende opdrachten in een terminal:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   U kunt een van de volgende uitvoer:

   * CRW-rw---1 hoofdmap uucp xxxxxxxx
   * CRW-rw---1 hoofdmap bellen xxxxxxxx

   U ziet in de uitvoer van de `uucp` of `dialout` die groepsnaam van de eigenaar van de USB-poort.

1. De gebruiker toevoegen aan de groep met de volgende opdracht:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`is de naam van de eigenaar die u in de vorige stap hebt verkregen. `<username>`uw gebruikersnaam Ubuntu is.

1. Ubuntu afmelden en aanmelden voor deze opnieuw om de wijziging door te voeren.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Sensorgegevens verzamelen en verzenden naar uw IoT-hub

In deze sectie kunt u gegevens kunt implementeren en uitvoeren van een voorbeeldtoepassing op Sparkfun ESP8266 ding ontwikkelaars De voorbeeldtoepassing de LED Sparkfun ESP8266 ding Dev knippert en stuurt de temperatuur en vochtigheid gegevens verzameld van de sensor DHT22 naar uw IoT-hub.

### <a name="get-the-sample-application-from-github"></a>Ophalen van de voorbeeldtoepassing uit GitHub

De voorbeeldtoepassing wordt gehost op GitHub. Kloon de opslagplaats voorbeeld waarin de voorbeeldtoepassing vanuit GitHub. De voorbeeld-opslagplaats klonen, de volgende stappen uit:

1. Open een opdrachtprompt of een terminalvenster.
1. Ga naar een map waar u de voorbeeldtoepassing moet worden opgeslagen.
1. Voer de volgende opdracht uit:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

Installeer het pakket voor Sparkfun ESP8266 ding Dev Arduino IDE:

1. Open de map waar de voorbeeldtoepassing wordt opgeslagen.
1. Open het bestand app.ino in de map van de app in Arduino IDE.

   ![de voorbeeldtoepassing in arduino ide openen](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. Klik in de IDE Arduino **bestand** > **voorkeuren**.
1. In de **voorkeuren** dialoogvenster vak, klik op het pictogram naast de **extra Boards Manager-URL's** in het tekstvak.
1. Voer de volgende URL in het pop-upvenster en klik vervolgens op **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Wijs de url van een pakket in arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. In de **voorkeur** in het dialoogvenster, klikt u op **OK**.
1. Klik op **extra** > **mededelingenbord** > **Boards Manager**, en zoek vervolgens naar esp8266.
   ESP8266 met een versie van 2.2.0 of later moet worden geïnstalleerd.

   ![Het pakket esp8266 is geïnstalleerd](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. Klik op **extra** > **mededelingenbord** > **Sparkfun ESP8266 ding Dev**.

### <a name="install-necessary-libraries"></a>Vereiste bibliotheken installeren

1. Klik in de IDE Arduino **schema** > **bibliotheek omvatten** > **bibliotheken beheren**.
1. Zoeken naar de volgende bibliotheek benoemt één voor één. Voor elk van de bibliotheek die u wilt zoeken, klikt u op **installeren**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Geen een echte DHT22 sensor?

De voorbeeldtoepassing kunt temperatuur en vochtigheid gegevens simuleren als u een echte DHT22 sensor geen hebt. Schakel in de voorbeeldtoepassing gesimuleerde gegevens gebruiken door de volgende stappen uit:

1. Open de `config.h` bestand de `app` map.
1. Zoek de volgende regel code en wijzig de waarde van `false` naar `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Configureren van de voorbeeldtoepassing gesimuleerde gegevens gebruiken](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. Sla met `Control-s`.

### <a name="deploy-the-sample-application-to-sparkfun-esp8266-thing-dev"></a>De voorbeeldtoepassing voor Sparkfun ESP8266 ding Dev implementeren

1. Klik in de IDE Arduino **hulpprogramma** > **poort**, en klik vervolgens op de seriële poort voor voor Sparkfun ESP8266 ding ontwikkelaars
1. Klik op **schema** > **uploaden** kunnen bouwen en implementeren van de voorbeeldtoepassing voor Sparkfun ESP8266 ding ontwikkelaars

> [!Note]
> Als u van Mac OS gebruikmaakt kan waarschijnlijk ziet u de volgende berichten tijdens het uploaden. `warning: espcomm_sync failed`,`error: espcomm_open failed`. Open uw ternimal-venster en klaar bent met het onderstaande bewerkingen u dit probleem kunt oplossen.
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a>Voer uw referenties in

Nadat het uploaden voltooid is, volg u de stappen om uw referenties invoeren:

1. Klik in de IDE Arduino **extra** > **seriële Monitor**.
1. In het venster seriële monitor ziet u de twee lijsten in de vervolgkeuzelijst in de rechterbenedenhoek.
1. Selecteer **er is geen afsluitende regel** voor de linker vervolgkeuzelijst.
1. Selecteer **115200 baud** voor de lijst rechts vervolgkeuzelijst.
1. Voer de volgende gegevens in het invoervak zich boven aan het venster seriële monitor, als u wordt gevraagd om ze en klik vervolgens op **verzenden**.
   * Wi-Fi-SSID
   * Wi-Fi-wachtwoord
   * Apparaat-verbindingsreeks

> [!Note]
> De referentie-informatie wordt opgeslagen in de EEPROM van Sparkfun ESP8266 ding ontwikkelaars Als u op de knop herstellen op het mededelingenbord Sparkfun ESP8266 ding Dev, de voorbeeldtoepassing wordt gevraagd of u wilt de gegevens wissen. Voer `Y` om de informatie die wordt gewist en wordt u gevraagd de gegevens opnieuw te verstrekken.

### <a name="verify-the-sample-application-is-running-successfully"></a>Controleer of dat de voorbeeldtoepassing met succes wordt uitgevoerd

Als u de volgende uitvoer van het monitorvenster seriële en de knipperende LED op Sparkfun ESP8266 ding Dev ziet, wordt de voorbeeldtoepassing met succes uitgevoerd.

![uiteindelijke uitvoer arduino IDE](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Volgende stappen

U hebt een Dev Sparkfun ESP8266 wat is verbonden met uw IoT-hub en de vastgelegde sensorgegevens verzonden naar uw IoT-hub. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

---
title: ESP8266 naar de cloud - Sparkfun ESP8266 Thing Dev verbinding maken met Azure IoT Hub | Microsoft Docs
description: Informatie over het installeren en verbinden Sparkfun ESP8266 Thing Dev met Azure IoT Hub voor deze gegevens verzenden naar de Azure-cloudplatform in deze zelfstudie.
author: rangv
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 75ff53d5be29af08bb8e9c1b41f61040e5710cf7
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452512"
---
# <a name="connect-sparkfun-esp8266-thing-dev-to-azure-iot-hub-in-the-cloud"></a>Sparkfun ESP8266 Thing Dev verbinden met Azure IoT Hub in de cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![verbinding tussen DHT22 Thing Dev en IoT-Hub](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a>Wat u doet

Sparkfun ESP8266 Thing Dev verbinden met een IoT-hub die u maakt. Voer een voorbeeld van toepassing op ESP8266 temperatuur en vochtigheid gegevens verzamelen uit een sensor DHT22. Ten slotte de sensorgegevens verzenden naar uw IoT-hub.

> [!NOTE]
> Als u van andere kaarten ESP8266 gebruikmaakt, kunt u nog steeds deze stappen om te verbinden met uw IoT-hub volgen. Afhankelijk van de ESP8266 bord dat u gebruikt, moet u mogelijk opnieuw configureren de `LED_PIN`. Bijvoorbeeld, als u van ESP8266 van AI-Thinker gebruikmaakt, u kunt wijzigen in `0` naar `2`. Hebt u een kit nog?: klik op [hier](http://azure.com/iotstarterkits)

## <a name="what-you-will-learn"></a>U leert

* Over het maken van een IoT-hub en een apparaat registreren voor ding ontwikkelactiviteiten.
* Klik hier voor meer informatie over het verbinden van Thing Dev met de sensor en uw computer.
* Over het verzamelen van gegevens door te voeren van een voorbeeld van toepassing op wat ontwikkelactiviteiten.
* Klik hier voor meer informatie over het verzenden van de sensorgegevens naar uw IoT-hub.

## <a name="what-you-will-need"></a>Wat u nodig hebt

![Onderdelen die nodig zijn voor de zelfstudie](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

Als u wilt deze bewerking is voltooid, moet u de volgende onderdelen van uw Thing Dev-startpakket:

* De Sparkfun ESP8266 Thing Dev-bord.
* Een Micro USB-Type een USB-kabel.

U kunt ook het volgende nodig voor uw ontwikkelomgeving:

* Een actief Azure-abonnement. Als u een Azure-account geen [Azure een gratis proefaccount maken](https://azure.microsoft.com/free/) in een paar minuten.
* Mac- of PC met Windows- of Ubuntu.
* Sparkfun ESP8266 Thing Dev verbinding maken met draadloos netwerk.
* Verbinding met Internet te downloaden van het configuratieprogramma.
* [Arduino IDE](https://www.arduino.cc/en/main/software) versie 1.6.8 (of nieuwer), eerdere versies werken niet met de AzureIoT-bibliotheek.

De volgende items zijn optioneel in het geval u een sensor geen hebt. U hebt ook de mogelijkheid van het gebruik van gesimuleerde sensorgegevens.

* Een Adafruit DHT22-sensor van temperatuur en vochtigheid.
* Een breadboard.
* M/M meestal bedrading.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-the-sensor-and-your-computer"></a>Verbinding maken met de ESP8266 Thing Dev met de sensor en uw computer

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-esp8266-thing-dev"></a>Verbinding maken met een DHT22 temperatuur en vochtigheid sensor ESP8266 Thing Dev

De kabels breadboard en meestal gebruiken om de verbinding als volgt. Als u geen een sensor, deze sectie overslaan omdat u gesimuleerde sensorgegevens in plaats daarvan kunt gebruiken.

![Naslaginformatie over verbindingen](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

Voor sensor pincodes gebruiken we de volgende bedrading:

| Start (Sensor)           | Einde (bord)           | Kleur van de kabel   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 27F)            | 3v (pincode 8A)           | Rode-kabel     |
| GEGEVENS (28 septies pincode)           | GPIO 2 (pincode 9a hebt)       | Wit-kabel    |
| GND (Pin 30F)            | GND (pincode 7J)          | Zwarte kabel   |


- Zie voor meer informatie: [DHT22 sensorinstallatie](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) en [Sparkfun ESP8266 Thing Dev-specificatie](https://www.sparkfun.com/products/13711)

Nu moeten uw Sparkfun ESP8266 Thing Dev zijn verbonden met een sensor werken.

![verbinding maken met dht22 met ESP8266 Thing Dev](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-to-your-computer"></a>Sparkfun ESP8266 Thing Dev aansluiten op uw computer

Gebruik de Micro USB-poort Type A USB-kabel verbinding als volgt Sparkfun ESP8266 Thing Dev maken met uw computer.

![feather huzzah aansluiten op uw computer](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a>Machtigingen van de seriële poort: alleen Ubuntu toevoegen

Als u Ubuntu gebruikt, controleert u of dat een normale gebruiker heeft de machtigingen om te werken op de USB-poort van Sparkfun ESP8266 Thing ontwikkelactiviteiten. Als u wilt toevoegen de seriële poort machtigingen voor een normale gebruiker, de volgende stappen uit:

1. Voer de volgende opdrachten uit op een terminal:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   U kunt een van de volgende uitvoer:

   * CRW-rw---1 hoofdmap uucp xxxxxxxx
   * CRW-rw---1 hoofdmap bellen xxxxxxxx

   In de uitvoer ziet u `uucp` of `dialout` dat wil zeggen de groep eigenaar-naam van de USB-poort.

1. De gebruiker toevoegen aan de groep met de volgende opdracht:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` is de naam van de eigenaar die u in de vorige stap hebt verkregen. `<username>` is de naam van de Ubuntu-gebruiker.

1. Meld u af Ubuntu en meld u deze opnieuw om de wijziging door te voeren.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Gegevens verzamelen en te verzenden naar uw IoT-hub

In deze sectie maakt u gegevens kunt implementeren en uitvoeren van een voorbeeld van toepassing op Sparkfun ESP8266 Thing ontwikkelactiviteiten. De voorbeeldtoepassing de Sparkfun ESP8266 Thing Dev-LED knippert en de temperatuur en vochtigheid gegevens verzameld van de sensor DHT22 naar uw IoT hub verzendt.

### <a name="get-the-sample-application-from-github"></a>Ophalen van de voorbeeld-App vanuit GitHub

De voorbeeldtoepassing wordt gehost op GitHub. Kloon de voorbeeldopslagplaats met de voorbeeld-App vanuit GitHub. Als u wilt de voorbeeldopslagplaats klonen, de volgende stappen uit:

1. Open een opdrachtprompt of een terminal-venster.
1. Ga naar een map waar u de voorbeeldtoepassing om te worden opgeslagen.
1. Voer de volgende opdracht uit:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

Het pakket voor Sparkfun ESP8266 Thing Dev in Arduino IDE installeren:

1. Open de map waarin de voorbeeldtoepassing is opgeslagen.
1. Open het bestand app.ino in de app-map in Arduino IDE.

   ![de voorbeeldtoepassing openen in arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. Klik in de Arduino IDE op **bestand** > **voorkeuren**.
1. In de **voorkeuren** dialoogvenster vak, klikt u op het pictogram naast de **extra borden Manager-URL's** in het tekstvak.
1. Voer de volgende URL in het pop-upvenster en klik vervolgens op **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![verwijzen naar de url van een pakket met arduino IDE](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. In de **voorkeur** in het dialoogvenster, klikt u op **OK**.
1. Klik op **extra** > **bord** > **borden Manager**, en zoek vervolgens de esp8266.
   ESP8266 met een versie van 2.2.0 of hoger moet worden geïnstalleerd.

   ![Het pakket esp8266 is geïnstalleerd](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. Klik op **extra** > **bord** > **Sparkfun ESP8266 Thing Dev**.

### <a name="install-necessary-libraries"></a>Vereiste bibliotheken installeren

1. Klik in de Arduino IDE op **schets** > **bibliotheek opnemen** > **bibliotheken beheren**.
1. Zoek naar de volgende bibliotheek de naam van één voor één. Voor elk van de bibliotheek die u wilt zoeken, klikt u op **installeren**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Hebt u geen een echte DHT22 sensor?

De voorbeeld-App kunt temperatuur en vochtigheid gegevens simuleren in het geval u een echte DHT22 sensor geen hebt. Inschakelen van de voorbeeldtoepassing om te gebruiken van gesimuleerde gegevens, als volgt te werk:

1. Open de `config.h` -bestand in de `app` map.
1. Zoek de volgende regel code en wijzig de waarde van `false` naar `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Configureert de voorbeeldtoepassing gesimuleerde gegevens gebruiken](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. Bespaar met `Control-s`.

### <a name="deploy-the-sample-application-to-sparkfun-esp8266-thing-dev"></a>De voorbeeldtoepassing om te Sparkfun ESP8266 Thing Dev implementeren

1. Klik in de Arduino IDE op **hulpprogramma** > **poort**, en klik vervolgens op de seriële poort voor Sparkfun ESP8266 Thing ontwikkelactiviteiten.
1. Klik op **schets** > **uploaden** te bouwen en implementeren van de voorbeeldtoepassing om te Sparkfun ESP8266 Thing ontwikkelactiviteiten.

> [!Note]
> Als u Mac OS kan waarschijnlijk ziet u de volgende berichten tijdens het uploaden. `warning: espcomm_sync failed`,`error: espcomm_open failed`. Open het venster ternimal en voltooien onder Acties om op te lossen dit probleem.
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a>Voer uw referenties in

Nadat het uploaden voltooid is, volgt u de stappen uit om uw referenties invoeren:

1. Klik in de Arduino IDE op **extra** > **seriële Monitor**.
1. In het venster van de seriële monitor, ziet u de twee lijsten in de vervolgkeuzelijst op de rechterbenedenhoek.
1. Selecteer **geen einde van regel** voor de linker vervolgkeuzelijst.
1. Selecteer **115200 baudrate** voor de juiste vervolgkeuzelijst.
1. Voer de volgende informatie in het invoervak zich aan de bovenkant van het venster seriële monitor, als u wordt gevraagd te regelen, en klik vervolgens op **verzenden**.
   * Wi-Fi-SSID
   * Wi-Fi-wachtwoord
   * Apparaat-verbindingsreeks

> [!Note]
> De referentie-informatie wordt opgeslagen in de EEPROM van Sparkfun ESP8266 Thing ontwikkelactiviteiten. Als u op de knop opnieuw instellen op de Sparkfun ESP8266 Thing Dev-bord, de voorbeeldtoepassing wordt gevraagd of u wilt de gegevens wissen. Voer `Y` dat de gegevens worden gewist en u wordt gevraagd gegevens opnieuw op te geven.

### <a name="verify-the-sample-application-is-running-successfully"></a>Controleer of dat de voorbeeldtoepassing met succes wordt uitgevoerd

Als u de volgende uitvoer van het venster seriële monitor en de knipperende LED op Sparkfun ESP8266 Thing Dev ziet, wordt de voorbeeldtoepassing is uitgevoerd.

![uiteindelijke uitvoer in arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Volgende stappen

U hebt een Sparkfun ESP8266 Thing Dev verbonden met uw IoT-hub en de vastgelegde gegevens verzonden naar uw IoT-hub. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

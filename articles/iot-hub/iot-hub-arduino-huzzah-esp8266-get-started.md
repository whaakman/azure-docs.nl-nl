---
title: ESP8266 naar de cloud - Feather HUZZAH ESP8266 verbindt met Azure IoT Hub | Microsoft Docs
description: Informatie over het installeren en verbinden van Adafruit Feather HUZZAH ESP8266 met Azure IoT Hub voor deze gegevens verzenden naar de Azure-cloudplatform in deze zelfstudie.
author: rangv
manager: nasing
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: e49a3063d6314947c59c5ae863077748d997f2b5
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515468"
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Adafruit Feather HUZZAH ESP8266 verbinden met Azure IoT Hub in de cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Verbinding tussen DHT22 Feather HUZZAH ESP8266 en IoT-Hub](./media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Wat u allemaal doen

Adafruit Feather HUZZAH ESP8266 verbinden met een IoT-hub die u maakt. Vervolgens voert u een voorbeeld van toepassing op ESP8266 de temperatuur en vochtigheid gegevens verzamelen uit een sensor DHT22. Ten slotte, stuurt u de sensorgegevens naar uw IoT-hub.

> [!NOTE]
> Als u andere borden ESP8266 gebruikt, kunt u nog steeds deze stappen om te verbinden met uw IoT-hub volgen. Afhankelijk van de ESP8266 bord dat u gebruikt, moet u mogelijk opnieuw configureren de `LED_PIN`. Bijvoorbeeld, als u ESP8266 van AI-Thinker, u kunt wijzigen in `0` naar `2`. Heb je nog een kit? Deze ophalen van de [Azure-website](http://azure.com/iotstarterkits).

## <a name="what-you-learn"></a>Wat u leert

* Over het maken van een IoT-hub en een apparaat registreren voor Feather HUZZAH ESP8266
* Verbinding maken tussen Feather HUZZAH ESP8266 met de sensor en uw computer
* Over het verzamelen van gegevens door te voeren van een voorbeeld van toepassing op Feather HUZZAH ESP8266
* Over het verzenden van de sensorgegevens naar uw IoT-hub

## <a name="what-you-need"></a>Wat u nodig hebt

![Onderdelen die nodig zijn voor de zelfstudie](./media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

Als u wilt deze bewerking is voltooid, moet u de volgende onderdelen van uw Feather HUZZAH ESP8266 Starter Kit:

* Het bord Feather HUZZAH ESP8266
* Een Micro USB-Type een USB-kabel

U moet ook de volgende bewerkingen voor uw ontwikkelomgeving:

* Een actief Azure-abonnement. Als u een Azure-account geen [Azure een gratis proefaccount maken](https://azure.microsoft.com/free/) in een paar minuten.
* Een Mac of een PC met Windows- of Ubuntu.
* Een draadloos netwerk voor Feather HUZZAH ESP8266 verbinden.
* Een internetverbinding beschikken om het downloaden van het configuratieprogramma.
* [Visual Studio Code-extensie voor Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino).

> [!Note]
> De Arduino IDE-versie die wordt gebruikt door Visual Studio Code-extensie voor Arduino moet versie 1.6.8 of hoger. Eerdere versies werken niet met de AzureIoT-bibliotheek.

De volgende items zijn optioneel in het geval u een sensor geen hebt. U hebt ook de mogelijkheid van het gebruik van gesimuleerde sensorgegevens.

* Een Adafruit DHT22 temperatuur en vochtigheid sensor
* Een breadboard
* M/M meestal kabels

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Voor IoT hub-verbindingsreeks ophalen

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>Verbinding maken met Feather HUZZAH ESP8266 met de sensor en uw computer

In deze sectie maakt verbinding u de sensoren met het mededelingenbord. Vervolgens aansluit u uw apparaat op uw computer om verder gebruik.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>Verbinding maken met een DHT22 temperatuur en vochtigheid sensor Feather HUZZAH ESP8266

De kabels breadboard en meestal gebruiken om de verbinding als volgt. Als u geen een sensor, deze sectie overslaan omdat u gesimuleerde sensorgegevens in plaats daarvan kunt gebruiken.

![Naslaginformatie over verbindingen](./media/iot-hub-arduino-huzzah-esp8266-get-started/17_connections_on_breadboard.png)

Voor sensor pincodes, gebruikt u de volgende bedrading:

| Start (Sensor)           | Einde (bord)            | Kleur van de kabel   |
| -----------------------  | ---------------------- | ------------  |
| VDD (Pin 31F)            | 3v (pincode 58 uur)           | Rode-kabel     |
| GEGEVENS (Pin 32F)           | GPIO 2 (Pin 46)       | Blauwe-kabel    |
| GND (Pin 34F)            | GND (PIn 56I)          | Zwarte kabel   |

Zie voor meer informatie, [Adafruit DHT22 sensorinstallatie](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) en [Adafruit Feather HUZZAH Esp8266 pin-outs gegeven](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).

Nu moeten uw Feather Huzzah ESP8266 zijn verbonden met een sensor werken.

![Verbinding maken met DHT22 met Feather Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Feather HUZZAH ESP8266 aansluiten op uw computer

Zoals hierna ziet, gebruikt u de Micro USB-poort Type A USB-kabel Feather HUZZAH ESP8266 aansluiten op uw computer.

![Feather Huzzah aansluiten op uw computer](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Seriële poort machtigingen (alleen Ubuntu) toevoegen

Als u Ubuntu gebruikt, zorg er dan voor dat u hebt de machtigingen op de USB-poort van Feather HUZZAH ESP8266 functioneren. Als u wilt toevoegen de machtigingen van de seriële poort, de volgende stappen uit:

1. Voer de volgende opdrachten uit op een terminal:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   U kunt een van de volgende uitvoer:

   * CRW-rw---1 hoofdmap uucp xxxxxxxx
   * CRW-rw---1 hoofdmap bellen xxxxxxxx

   In de uitvoer ziet u dat `uucp` of `dialout` is de naam van de eigenaar van de USB-poort.

2. De gebruiker toevoegen aan de groep met de volgende opdracht:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` is de naam van de eigenaar die u in de vorige stap hebt verkregen. `<username>` is de naam van de Ubuntu-gebruiker.

3. Ubuntu afmelden en vervolgens weer aanmelden om de wijziging wordt weergegeven.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Gegevens verzamelen en te verzenden naar uw IoT-hub

In deze sectie maakt u implementeren en uitvoeren van een voorbeeld van toepassing op Feather HUZZAH ESP8266. De voorbeeldtoepassing de LED Feather HUZZAH ESP8266 knippert en de temperatuur en vochtigheid gegevens verzameld van de sensor DHT22 naar uw IoT hub verzendt.

### <a name="get-the-sample-application-from-github"></a>Ophalen van de voorbeeld-App vanuit GitHub

De voorbeeldtoepassing wordt gehost op GitHub. Kloon de voorbeeldopslagplaats met de voorbeeld-App vanuit GitHub. Als u wilt de voorbeeldopslagplaats klonen, de volgende stappen uit:

1. Open een opdrachtprompt of een terminal-venster.

2. Ga naar een map waar u de voorbeeldtoepassing om te worden opgeslagen.

3. Voer de volgende opdracht uit:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

   Vervolgens installeert u het pakket voor Feather HUZZAH ESP8266 in Visual Studio Code.

4. Open de map waarin de voorbeeldtoepassing is opgeslagen.

5. Open het bestand app.ino in de app-map in de Visual Studio-Code.

   ![De voorbeeldtoepassing openen in Visual Studio Code](media/iot-hub-arduino-huzzah-esp8266-get-started/10_vscode-open-sample-app.png)

6. Voer in de Visual Studio Code `F1`.

7. Type **Arduino** en selecteer **Arduino: bord Manager**.

8. In de **Arduino-bord Manager** tabblad **extra URL's**.

   ![VS Code Arduino-bord Manager](media/iot-hub-arduino-huzzah-esp8266-get-started/11_vscode-arduino-board-manager.png)

9. In de **gebruikersinstellingen** venster, kopieer en plak het volgende aan het einde van het bestand

   ```
   "arduino.additionalUrls": "http://arduino.esp8266.com/stable/package_esp8266com_index.json"
   ```
   
   ![URL van de Arduino-pakket in VS Code configureren](media/iot-hub-arduino-huzzah-esp8266-get-started/12_vscode-package-url.png)

10. Sla het bestand op en sluit de **gebruikersinstellingen** tabblad.

11. Klik op **vernieuwen pakket indexen**. Nadat het vernieuwen is voltooid, zoeken naar **esp8266**.

12. Klik op **installeren** knop voor esp8266.

   Boards Manager geeft aan dat de ESP8266 met een versie van 2.2.0 of hoger is geïnstalleerd.

   ![Het pakket esp8266 is geïnstalleerd](media/iot-hub-arduino-huzzah-esp8266-get-started/13_vscode-esp8266-installed.png)

13. Voer `F1`, typt u **Arduino** en selecteer **Arduino: bord Config**.

14. Klik in het vak voor **bord geselecteerd:** en het type **esp8266**en selecteer vervolgens **Adafruit HUZZAH ESP8266 (esp8266)**.

   ![Selecteer esp8266 bord](media/iot-hub-arduino-huzzah-esp8266-get-started/14_vscode-select-esp8266.png)

### <a name="install-necessary-libraries"></a>Vereiste bibliotheken installeren

1. Voer in de Visual Studio Code, `F1`, typt u **Arduino** en selecteer **Arduino: Library Manager**.

2. Zoek naar de volgende bibliotheek de naam van één voor één. Voor elke bibliotheek die u vindt, klikt u op **installeren**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Hebt u geen een echte DHT22 sensor?

De voorbeeld-App kunt temperatuur en vochtigheid gegevens simuleren in het geval u een echte DHT22 sensor geen hebt. Als u de voorbeeldtoepassing om te gebruiken van gesimuleerde gegevens instelt, de volgende stappen uit:

1. Open de `config.h` -bestand in de `app` map.

2. Zoek de volgende regel code en wijzig de waarde van `false` naar `true`:

   ```c
   define SIMULATED_DATA true
   ```
   
   ![Configureert de voorbeeldtoepassing gesimuleerde gegevens gebruiken](media/iot-hub-arduino-huzzah-esp8266-get-started/15_vscode-configure-app-use-simulated-data.png)

3. Sla het bestand op.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>De voorbeeldtoepassing om te Feather HUZZAH ESP8266 implementeren

1. Klik in de Visual Studio-Code op **<Select Serial Port>** op de status van de staaf-, en klik vervolgens op de seriële poort voor Feather HUZZAH ESP8266.

2. Voer `F1`, typt u **Arduino** en selecteer **Arduino: uploaden** te bouwen en implementeren van de voorbeeldtoepassing om te Feather HUZZAH ESP8266.

### <a name="enter-your-credentials"></a>Voer uw referenties in

Nadat het uploaden voltooid is, volgt u deze stappen om uw referenties invoeren:

1. Arduino IDE openen, klikt u op **extra** > **seriële Monitor**.

2. In het venster van de seriële monitor, ziet u de twee lijsten in de vervolgkeuzelijst in de rechterbenedenhoek.

3. Selecteer **geen einde van regel** voor de linker vervolgkeuzelijst.

4. Selecteer **115200 baudrate** voor de juiste vervolgkeuzelijst.

5. Voer de volgende informatie in het invoervak zich aan de bovenkant van het venster seriële monitor, als u wordt gevraagd te regelen, en klik vervolgens op **verzenden**.

   * Wi-Fi-SSID
   * Wi-Fi-wachtwoord
   * Apparaat-verbindingsreeks

> [!Note]
> De referentie-informatie wordt opgeslagen in de EEPROM van Feather HUZZAH ESP8266. Als u op de knop opnieuw instellen op het prikbord van Feather HUZZAH ESP8266 klikt, wordt de voorbeeldtoepassing gevraagd als u wilt de gegevens wissen. Voer `Y` te beschikken over de gegevens worden gewist. U wordt gevraagd een tweede keer gegevens op te geven.

### <a name="verify-the-sample-application-is-running-successfully"></a>Controleer of dat de voorbeeldtoepassing met succes wordt uitgevoerd

Als u de volgende uitvoer van het venster seriële monitor en de knipperende LED op Feather HUZZAH ESP8266 ziet, wordt de voorbeeldtoepassing is uitgevoerd.

![Uiteindelijke uitvoer in Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/16_arduino-ide-final-output.png)

## <a name="next-steps"></a>Volgende stappen

U hebt een Feather HUZZAH ESP8266 verbonden met uw IoT-hub, en de vastgelegde gegevens verzonden naar uw IoT-hub. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
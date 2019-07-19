---
title: ESP8266 naar Cloud-Connect Doezelaar HUZZAH ESP8266 naar Azure IoT Hub | Microsoft Docs
description: Meer informatie over het instellen en verbinden van Adafruit doezelen HUZZAH ESP8266 naar Azure IoT Hub voor IT om gegevens te verzenden naar het Azure-Cloud platform in deze zelf studie.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: wesmc
ms.openlocfilehash: 8e97a979c37af8ade51b4ff6ca4b2c5b4eec126e
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68232729"
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Adafruit doezelen HUZZAH ESP8266 in de cloud verbinden met Azure IoT Hub

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Verbinding tussen DHT22, Doezelaar HUZZAH ESP8266 en IoT Hub](./media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Wat u doet

Verbind Adafruit doezelen HUZZAH ESP8266 met een IoT-hub die u maakt. Vervolgens voert u een voorbeeld toepassing uit op ESP8266 om de Tempe ratuur-en vochtigheids gegevens te verzamelen van een DHT22-sensor. Ten slotte verzendt u de sensor gegevens naar uw IoT-hub.

> [!NOTE]
> Als u andere ESP8266-kaarten gebruikt, kunt u deze stappen volgen om deze te verbinden met uw IoT-hub. Afhankelijk van het ESP8266-bord dat u gebruikt, moet u de `LED_PIN`opnieuw configureren. Als u bijvoorbeeld ESP8266 van AI-denker gebruikt, kunt u deze wijzigen van `0` in. `2` Hebt u nog geen Kit? Down load het via de [Azure-website](https://azure.com/iotstarterkits).

## <a name="what-you-learn"></a>Wat u leert

* Een IoT-hub maken en een apparaat registreren voor de doezelaar HUZZAH ESP8266
* Doezelen HUZZAH ESP8266 verbinden met de sensor en uw computer
* Sensor gegevens verzamelen door een voorbeeld toepassing uit te voeren op doezel HUZZAH ESP8266
* De sensor gegevens verzenden naar uw IoT-hub

## <a name="what-you-need"></a>Wat u nodig hebt

![Onderdelen die nodig zijn voor de zelf studie](./media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

Als u deze bewerking wilt volt ooien, hebt u de volgende onderdelen nodig van uw doezel HUZZAH ESP8266 Starter Kit:

* Het HUZZAH ESP8266-bord van doezelen
* Een micro-USB om een USB-kabel te typen

U hebt ook de volgende zaken nodig voor uw ontwikkel omgeving:

* Een actief Azure-abonnement. Als u geen Azure-account hebt, kunt u binnen een paar minuten [een gratis Azure-proef account maken](https://azure.microsoft.com/free/) .
* Een Mac of PC met Windows of Ubuntu.
* [GIT](https://git-scm.com/download)
* [Arduino](https://www.arduino.cc/en/main/software#download)
* [Adafruit Huzzah ESP8266-stuur programma voor USB](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide)
* Een draadloos netwerk voor doezel HUZZAH ESP8266 waarmee verbinding moet worden gemaakt.
* Een Internet verbinding om het configuratie programma te downloaden.
* [Visual Studio code Extension voor Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino).

> [!Note]
> De Arduino IDE-versie die door Visual Studio code Extension voor Arduino wordt gebruikt, moet versie 1.6.8 of hoger zijn. Eerdere versies werken niet met de AzureIoT-bibliotheek.

De volgende items zijn optioneel als u geen sensor hebt. U kunt ook gesimuleerde sensor gegevens gebruiken.

* Een Adafruit DHT22-Tempe ratuur en vochtigheids sensor
* Een breadboard
* Lijn/M-jumper draden

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>De HUZZAH-ESP8266 van doezelen verbinden met de sensor en uw computer

In deze sectie verbindt u de Sens oren met uw bord. Vervolgens sluit u uw apparaat op de computer aan om verder te kunnen gebruiken.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>Een DHT22-temperatuur-en vochtigheids sensor aansluiten op doezelen HUZZAH ESP8266

Gebruik de breadboard-en jumper draden om de verbinding als volgt te maken. Als u geen sensor hebt, kunt u deze sectie overs Laan omdat u in plaats daarvan gesimuleerde sensor gegevens gebruikt.

![Naslag informatie over verbindingen](./media/iot-hub-arduino-huzzah-esp8266-get-started/17_connections_on_breadboard.png)

Voor sensor pincodes gebruikt u de volgende bedrading:

| Begin (sensor)           | Einde (Board)            | Kabel kleur   |
| -----------------------  | ---------------------- | ------------  |
| VDD (PIN 31F)            | 3V (PIN 58H)           | Rode kabel     |
| GEGEVENS (pincode 32F)           | GPIO 2 (PIN 46A)       | Blauwe kabel    |
| GND (PIN 34F)            | GND (PIn 56I)          | Zwarte kabel   |

Zie [ADAFRUIT DHT22 sensor Setup](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) and [Adafruit doezelen HUZZAH Esp8266 pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts)voor meer informatie.

Uw Doezelaar Huzzah ESP8266 moet nu zijn verbonden met een werk sensor.

![DHT22 verbinden met doezel Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Doezel HUZZAH ESP8266 verbinden met uw computer

Zoals volgende wordt weer gegeven, gebruikt u de micro USB om een USB-kabel te typen om de doezel HUZZAH ESP8266 op uw computer te verbinden.

![Doezel Huzzah koppelen aan uw computer](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Machtigingen voor seriële poorten toevoegen (alleen Ubuntu)

Als u Ubuntu gebruikt, zorg er dan voor dat u de juiste machtigingen hebt om te werken op de USB-poort van de doezel HUZZAH ESP8266. Voer de volgende stappen uit om machtigingen voor een seriële poort toe te voegen:

1. Voer de volgende opdrachten uit op een Terminal:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   U krijgt een van de volgende uitvoer:

   * CRW-RW----1 root uucp xxxxxxxx
   * CRW-RW----1 root dialout xxxxxxxx

   In de uitvoer ziet `uucp` u of `dialout` de naam van de groeps eigenaar van de USB-poort is.

2. Voeg de gebruiker toe aan de groep door de volgende opdracht uit te voeren:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`is de naam van de groeps eigenaar die u hebt verkregen in de vorige stap. `<username>`is uw Ubuntu-gebruikers naam.

3. Meld u af bij Ubuntu en meld u opnieuw aan om de wijziging weer te geven.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Sensor gegevens verzamelen en deze naar uw IoT-hub verzenden

In deze sectie implementeert en voert u een voorbeeld toepassing uit op doezel HUZZAH ESP8266. De voorbeeld toepassing knippert de LED op doezelen HUZZAH ESP8266 en verzendt de gegevens over de Tempe ratuur en vochtigheid die van de DHT22-sensor zijn verzameld, naar uw IoT-hub.

### <a name="get-the-sample-application-from-github"></a>De voorbeeld toepassing ophalen uit GitHub

De voorbeeld toepassing wordt gehost op GitHub. Kloon de voor beeld-opslag plaats die de voorbeeld toepassing bevat van GitHub. Voer de volgende stappen uit om de voorbeeld opslagplaats te klonen:

1. Open een opdracht prompt of een Terminal venster.

2. Ga naar een map waarin u wilt dat de voorbeeld toepassing wordt opgeslagen.

3. Voer de volgende opdracht uit:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

   Installeer vervolgens het pakket voor de doezel HUZZAH ESP8266 in Visual Studio code.

4. Open de map waarin de voorbeeld toepassing is opgeslagen.

5. Open het bestand app. Ino in de map app in de Visual Studio-code.

   ![De voorbeeld toepassing openen in Visual Studio code](media/iot-hub-arduino-huzzah-esp8266-get-started/10_vscode-open-sample-app.png)

6. Typ `F1`in de Visual Studio-code.

7. Typ **Arduino** en selecteer **Arduino: Board Manager**.

8. Klik op het tabblad **Arduino board Manager** op **extra url's**.

   ![VS code Arduino board Manager](media/iot-hub-arduino-huzzah-esp8266-get-started/11_vscode-arduino-board-manager.png)

9. Kopieer en plak in het venster **gebruikers instellingen** het volgende aan het einde van het bestand.

   ```json
   "arduino.additionalUrls": "https://arduino.esp8266.com/stable/package_esp8266com_index.json"
   ```

   ![De Arduino-pakket-URL in VS code configureren](media/iot-hub-arduino-huzzah-esp8266-get-started/12_vscode-package-url.png)

10. Sla het bestand op en sluit het tabblad **gebruikers instellingen** .

11. Klik op **pakket indexen vernieuwen**. Nadat het vernieuwen is voltooid, zoekt u naar **esp8266**.

12. Klik op de knop **installeren** voor esp8266.

    Beheer van prik borden geeft aan dat ESP8266 met een versie van 2.2.0 of hoger is geïnstalleerd.

    ![Het esp8266-pakket is geïnstalleerd](media/iot-hub-arduino-huzzah-esp8266-get-started/13_vscode-esp8266-installed.png)

13. Enter `F1`, typt u **Arduino** en selecteert **u Arduino: Bord configuratie**.

14. Klik op het selectie vakje voor het **geselecteerde bord:** en typ **esp8266**en selecteer vervolgens **Adafruit HUZZAH esp8266 (esp8266)** .

    ![Esp8266-bord selecteren](media/iot-hub-arduino-huzzah-esp8266-get-started/14_vscode-select-esp8266.png)

### <a name="install-necessary-libraries"></a>Benodigde bibliotheken installeren

1. Typ in de Visual Studio code `F1`, typ **Arduino** en selecteer **Arduino: Bibliotheek beheer**.

2. Zoek de volgende bibliotheek namen één voor één. Klik voor elke bibliotheek die u zoekt op **installeren**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Hebt u geen echte DHT22-sensor?

De voorbeeld toepassing kan de Tempe ratuur-en vochtigheids gegevens simuleren voor het geval u geen echte DHT22-sensor hebt. Voer de volgende stappen uit om de voorbeeld toepassing in te stellen voor het gebruik van gesimuleerde gegevens:

1. Open het `config.h` bestand in de `app` map.

2. Zoek de volgende regel code en wijzig de waarde van `false` in: `true`

   ```c
   define SIMULATED_DATA true
   ```

   ![De voorbeeld toepassing configureren voor het gebruik van gesimuleerde gegevens](media/iot-hub-arduino-huzzah-esp8266-get-started/15_vscode-configure-app-use-simulated-data.png)

3. Sla het bestand op.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>De voorbeeld toepassing implementeren voor het doezelen van HUZZAH ESP8266

1. Klik in Visual Studio code op  **\<seriële poort selecteren >** op de status balk en klik vervolgens op de seriële poort voor de doezelaar HUZZAH ESP8266.

2. Enter `F1`, typt u **Arduino** en selecteert **u Arduino: Upload** om de voorbeeld toepassing te bouwen en te implementeren voor het doezelen van HUZZAH ESP8266.

### <a name="enter-your-credentials"></a>Voer uw referenties in

Nadat het uploaden is voltooid, voert u de volgende stappen uit om uw referenties in te voeren:

1. Open Arduino IDE, klik op **extra** > **seriële monitor**.

2. In het venster seriële monitor ziet u de twee vervolg keuzelijsten in de rechter benedenhoek.

3. Selecteer **geen lijn einde** voor de vervolg keuzelijst aan de linkerkant.

4. Selecteer **115200 baud** voor de juiste vervolg keuzelijst.

5. Voer in het invoervak boven in het venster seriële monitor de volgende informatie in als u wordt gevraagd om deze op te geven en klik vervolgens op **verzenden**.

   * Wi-Fi SSID
   * Wi-Fi-wacht woord
   * Apparaat connection string

> [!Note]
> De referentie gegevens worden opgeslagen in de EEPROM van doezelen HUZZAH ESP8266. Als u op de knop opnieuw instellen op het ESP8266 van de doezelaar HUZZAH klikt, wordt u gevraagd of u de gegevens wilt wissen. Voer `Y` in om de gegevens te wissen. U wordt gevraagd de informatie een tweede keer op te geven.

### <a name="verify-the-sample-application-is-running-successfully"></a>Controleren of de voorbeeld toepassing correct wordt uitgevoerd

Als u de volgende uitvoer ziet in het venster serieel monitor en de knipperende LED op doezelen HUZZAH ESP8266, wordt de voorbeeld toepassing uitgevoerd.

![Laatste uitvoer in Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/16_arduino-ide-final-output.png)

## <a name="read-the-messages-received-by-your-hub"></a>Lees de berichten die door uw hub zijn ontvangen

Een manier om berichten te bewaken die door uw IoT-hub van uw apparaat worden ontvangen, is door de Azure IoT-Hulpprogram Ma's voor Visual Studio code te gebruiken. Zie [Azure IOT-Hulpprogram ma's voor Visual Studio code gebruiken voor het verzenden en ontvangen van berichten tussen uw apparaat en IOT hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md)voor meer informatie.

Ga verder met de volgende sectie voor meer manieren om gegevens te verwerken die door uw apparaat worden verzonden.

## <a name="next-steps"></a>Volgende stappen

U hebt verbinding gemaakt met een HUZZAH-ESP8266 voor de IoT-hub en u hebt de vastgelegde sensor gegevens verzonden naar uw IoT-hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
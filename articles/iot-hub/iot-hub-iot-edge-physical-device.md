---
title: Een fysiek apparaat gebruiken met Azure IoT rand | Microsoft Docs
description: Het gebruik van een apparaat Texas instrumenten SensorTag om gegevens te verzenden naar een IoT-hub via een IoT Edge gateway uitgevoerd op een apparaat frambozen Pi 3. De gateway is gebouwd met behulp van Azure IoT rand.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: andbuc
ms.openlocfilehash: b24828ee1a09ba8e5f657954e11936f124270173
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="forward-device-to-cloud-messages-to-iot-hub-using-azure-iot-edge-on-a-raspberry-pi"></a>Apparaat-naar-cloud-berichten naar IoT Hub met Azure IoT Edge op een Pi frambozen doorsturen

Dit overzicht van de [Bluetooth lage energie voorbeeld] [ lnk-ble-samplecode] leest u hoe u [Azure IoT rand] [ lnk-sdk] naar:

* Apparaat-naar-cloud telemetrie doorsturen naar IoT Hub vanaf een fysiek apparaat.
* Route-opdrachten uit IoT Hub naar een fysiek apparaat.

Dit overzicht omvat:

* **Architectuur**: belangrijke architecturale informatie over het lage energie Bluetooth-voorbeeld.
* **Ontwikkeling en uitvoering**: de stappen die nodig zijn om het voorbeeld te bouwen en uit te voeren.

## <a name="architecture"></a>Architectuur

De procedure ziet u hoe het bouwen en uitvoeren van een gateway aan de rand van IoT op een frambozen Pi 3 die Raspbian Linux wordt uitgevoerd. De gateway is gebouwd met behulp van IoT rand. Het voorbeeld maakt gebruik van een apparaat Texas instrumenten SensorTag Bluetooth laag energieverbruik (uitschakelen) temperatuur gegevens te verzamelen.

Bij het uitvoeren van de rand van de IoT-gateway het:

* Maakt verbinding met een SensorTag-apparaat met behulp van de Bluetooth-laag energieverbruik (uitschakelen)-protocol.
* Maakt verbinding met IoT Hub met behulp van het HTTPS-protocol.
* Verzendt telemetrie van het apparaat SensorTag IoT-hub.
* Routes opdrachten uit IoT Hub met het SensorTag-apparaat.

De gateway bevat de volgende IoT Edge-modules:

* Een *uitschakelen module* die interfaces met een apparaat uitschakelen temperatuur gegevens ontvangen van het apparaat en opdrachten naar het apparaat verzenden.
* A *uitschakelen cloud naar apparaat module* die JSON-berichten uit IoT Hub wordt verzonden naar instructies voor het uitschakelen zet de *uitschakelen module*.
* Een *berichtenlogboek module* die registreert alle gateway-berichten naar een lokaal bestand.
* Een *identiteit toewijzing module* die tussen uitschakelen apparaat MAC-adressen en Azure IoT Hub apparaat-id's worden omgezet.
* Een *IoT Hub module* die telemetrische gegevens uploadt naar een IoT-hub en apparaatopdrachten ontvangt van een IoT-hub.
* Een *uitschakelen printer module* die wordt geïnterpreteerd telemetrie van het apparaat uitschakelen en afdrukken bestellen opgemaakte gegevens naar de console voor probleemoplossing en foutopsporing inschakelen.

### <a name="how-data-flows-through-the-gateway"></a>Hoe gegevens loopt via de gateway

Het volgende blokdiagram illustreert de telemetrie uploaden van gegevensstroom pijplijn:

![Telemetrie uploaden gateway pijplijn](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

De stappen waarmee een item van telemetrie vanaf een apparaat uitschakelen met IoT Hub onderweg zijn:

1. Het apparaat uitschakelen genereert een voorbeeld van een temperatuur en verzendt het via Bluetooth naar de module uitschakelen in de gateway.
1. De module uitschakelen ontvangt van het voorbeeld en publiceert deze op de broker samen met het MAC-adres van het apparaat.
1. De toewijzing identity module neemt over dit bericht en gebruikt een interne tabel voor het MAC-adres van het apparaat vertalen naar een IoT Hub apparaat-id. Een IoT Hub apparaat-id bestaat uit een apparaat-ID en de apparaatsleutel.
1. De toewijzing identity module publiceert een nieuw bericht met de volgende informatie:
   - Voorbeeldgegevens temperatuur
   - Het MAC-adres van het apparaat
   - De apparaat-ID
   - Sleutel voor het apparaat  
1. De module IoT Hub ontvangt dit nieuwe bericht (gegenereerd door de module van de toewijzing identiteit) en publiceert deze IoT-hub.
1. De module berichtenlogboek registreert alle berichten van de broker met een lokaal bestand.

Het volgende blokdiagram wordt het apparaat opdracht gegevensstroom pijplijn:

![Apparaat opdracht gateway-pipeline](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. De module IoT Hub periodiek de iothub voor nieuwe opdracht-berichten.
1. Wanneer de IoT Hub-module een nieuwe opdrachtbericht ontvangt, publiceert deze naar de broker.
1. De toewijzing identity module haalt het bericht met de opdracht en gebruikt een interne tabel om te vertalen van de IoT Hub apparaat-ID voor de MAC-adres van een apparaat. Vervolgens wordt een nieuw bericht met het MAC-adres van het doelapparaat in de eigenschappen van kaart van het bericht gepubliceerd.
1. De module uitschakelen Cloud-naar-apparaat dit bericht wordt opgehaald en zet deze om in de juiste instructie uitschakelen voor de module uitschakelen. Vervolgens wordt een nieuw bericht gepubliceerd.
1. De module uitschakelen dit bericht wordt opgehaald en voert de i/o-instructie door de communicatie met het apparaat uitschakelen.
1. De module berichtenlogboek registreert alle berichten van de broker naar een schijfbestand.

## <a name="prerequisites"></a>Vereisten

U hebt een actief Azure-abonnement nodig om deze zelfstudie te voltooien.

> [!NOTE]
> Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk-free-trial] voor meer informatie.

SSH-client moet u op de computer waarmee u kunt extern toegang tot de opdrachtregel op de frambozen Pi.

- Windows bevat geen een SSH-client. Wordt u aangeraden [PuTTY](http://www.putty.org/).
- De meeste Linux-distributies en Mac OS omvatten het SSH-opdrachtregelprogramma. Zie voor meer informatie [SSH met behulp van Linux- of Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

## <a name="prepare-your-hardware"></a>Bereid uw hardware

Deze zelfstudie wordt ervan uitgegaan dat u gebruikt een [Texas instrumenten SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) apparaat is verbonden met een frambozen Pi 3 Raspbian uitgevoerd.

### <a name="install-raspbian"></a>Raspbian installeren

U kunt een van de volgende opties voor het installeren van Raspbian op uw apparaat frambozen Pi 3 gebruiken.

* U kunt de nieuwste versie van Raspbian installeren met de [NOOBS] [ lnk-noobs] grafische gebruikersinterface.
* Handmatig [downloaden] [ lnk-raspbian] en de meest recente installatiekopie van het besturingssysteem Raspbian schrijven naar een SD-kaart.

### <a name="sign-in-and-access-the-terminal"></a>Aanmelden en toegang tot de terminal

U hebt twee opties voor toegang tot een terminal omgeving op uw Pi frambozen:

* Als u een toetsenbord en de monitor die zijn verbonden met uw Pi frambozen hebt, kunt u de gebruikersinterface van Raspbian voor toegang tot een terminalvenster.

* Toegang tot de opdrachtregel op uw frambozen Pi gebruik van SSH op uw computer.

#### <a name="use-a-terminal-window-in-the-gui"></a>Gebruik een terminalvenster in de gebruikersinterface

De standaardreferenties voor Raspbian zijn gebruikersnaam **pi** en het wachtwoord **frambozen**. U kunt starten in de taakbalk in de gebruikersinterface van de **Terminal** hulpprogramma met het pictogram dat op een monitor lijkt.

#### <a name="sign-in-with-ssh"></a>Meld u aan met SSH

U kunt SSH gebruiken voor vanaf de opdrachtregel toegang tot uw frambozen Pi. Het artikel [SSH (Secure Shell)] [ lnk-pi-ssh] wordt beschreven hoe SSH configureren op uw Pi frambozen en verbinding maken van [Windows] [ lnk-ssh-windows] of [Linux en Mac OS][lnk-ssh-linux].

Aanmelden met gebruikersnaam **pi** en het wachtwoord **frambozen**.

### <a name="install-bluez-537"></a>BlueZ 5.37 installeren

De modules uitschakelen Neem contact op met de Bluetooth-hardware, via de stack BlueZ. U moet versie 5.37 van BlueZ voor de modules correct te laten werken. Deze instructies, Controleer of dat de juiste versie van BlueZ is geïnstalleerd.

1. De huidige bluetooth-daemon stoppen:

    ```sh
    sudo systemctl stop bluetooth
    ```

1. De afhankelijkheden BlueZ installeren:

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf libtool glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. Download de broncode BlueZ van bluez.org:

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. Pak de broncode:

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. Wijzig de mappen in de zojuist gemaakte map:

    ```sh
    cd bluez-5.37
    ```

1. Configureer de code BlueZ kunnen worden gebouwd:

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. Bouw BlueZ:

    ```sh
    make
    ```

1. BlueZ installeren als deze is voltooid bouwcontract:

    ```sh
    sudo make install
    ```

1. Wijziging systemd-serviceconfiguratie voor Bluetooth-zodat deze naar de nieuwe bluetooth-daemon in het bestand verwijst `/lib/systemd/system/bluetooth.service`. De regel 'ExecStart' vervangen door de volgende tekst:

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-to-the-sensortag-device-from-your-raspberry-pi-3-device"></a>Connectiviteit inschakelen op het apparaat SensorTag van uw apparaat frambozen Pi 3

Voordat u het voorbeeld uitvoert, moet u controleren of uw frambozen Pi 3 verbinding op het apparaat SensorTag maken kunt.

1. Zorg ervoor dat de `rfkill` hulpprogramma is geïnstalleerd:

    ```sh
    sudo apt-get install rfkill
    ```

1. Blokkering opheffen van bluetooth op het frambozen Pi 3 en controleer of het versienummer **5.37**:

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. Om in te voeren de interactieve bluetooth-shell, start de bluetooth-service en voer de **bluetoothctl** opdracht:

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Voer de opdracht **inschakelopdrachten** tot de macht van de bluetooth-controller. De opdracht retourneert de uitvoer lijkt op het volgende voorbeeld:

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. Voer de opdracht in de interactieve bluetooth-shell **scannen op** om te scannen voor Bluetooth-apparaten. De opdracht retourneert de uitvoer lijkt op het volgende voorbeeld:

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. Controleer het SensorTag-apparaat kunnen worden gedetecteerd door op de kleine knop (de groene LED moet flash). De frambozen Pi 3 moet het apparaat SensorTag detecteren:

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    In dit voorbeeld ziet u dat het MAC-adres van het apparaat SensorTag **A0:E6:F8:B5:F6:00**.

1. Schakel scannen door te voeren de **scannen uitschakelen** opdracht:

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. Verbinding maken met uw SensorTag-apparaat door te voeren met behulp van het MAC-adres **verbinding \<MAC-adres\>**. De volgende voorbeelduitvoer wordt voor de duidelijkheid afgekort:

    ```sh
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```

    > U kunt de GATT kenmerken van het apparaat opnieuw met lijst de **lijst kenmerken** opdracht.

1. U kunt nu loskoppelen van het apparaat via de **verbreken** opdracht en sluit vervolgens af van de Bluetooth-shell met behulp de **afsluiten** opdracht:

    ```sh
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

U kunt nu de uitschakelen IoT Edge-voorbeeld uitvoeren op uw frambozen Pi 3.

## <a name="run-the-iot-edge-ble-sample"></a>Het uitschakelen van IoT-Edge-voorbeeld uitvoeren

Voor het uitvoeren van de steekproef IoT rand uitschakelen, moet u drie taken voltooien:

* Configureer twee voorbeeld-apparaten in uw IoT-Hub.
* IoT-Edge op uw apparaat frambozen Pi 3 bouwen.
* Configureren en uitvoeren van de steekproef uitschakelen op uw apparaat frambozen Pi 3.

Op het moment van schrijven IoT Edge biedt alleen ondersteuning voor modules uitschakelen in gateways met op Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Twee voorbeeld-apparaten in uw IoT-Hub configureren

* [Een iothub maken] [ lnk-create-hub] in uw Azure-abonnement, moet u de naam van uw hub voor dit scenario. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* Toevoegen van een apparaat aangeroepen **SensorTag_01** naar uw IoT-hub en maak een notitie van de sleutel-Id en het apparaat. U kunt de [apparaat explorer of iothub-explorer] [ lnk-explorer-tools] hulpprogramma's voor dit apparaat toevoegen aan de IoT-hub die u in de vorige stap hebt gemaakt en de sleutel ophalen. U toewijzen dit apparaat aan het apparaat SensorTag bij het configureren van de gateway.

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a>Azure IoT-rand bouwen op uw Raspberry Pi 3

Afhankelijkheden voor Azure IoT rand installeren:

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev libtool
```

Gebruik de volgende opdrachten voor het klonen van IoT-rand en alle bijbehorende submodules naar uw basismap:

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

Wanneer u een volledige kopie van de rand van de IoT-opslagplaats op uw frambozen Pi 3 hebt, kunt u met behulp van de volgende opdracht uit de map waarin de SDK:

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-the-ble-sample-on-your-raspberry-pi-3"></a>Configureren en uitvoeren van de steekproef uitschakelen op uw frambozen Pi 3

Bootstrap- en het voorbeeld uitvoert, configureert elke zijde van de IoT-module die deel uitmaakt in de gateway. Deze configuratie is opgegeven in een JSON-bestand en u moet alle vijf deelnemende IoT rand modules configureren. Er is een voorbeeld van een JSON-bestand in de opslagplaats aangeroepen **gateway\_sample.json** dat u als startpunt gebruiken kunt voor het bouwen van uw eigen configuratiebestand. Dit bestand bevindt zich in de **ble_gateway/samples/src** map in de lokale kopie van de rand van de IoT-opslagplaats.

De volgende secties wordt beschreven hoe dit configuratiebestand voor de uitschakelen bewerken. Er wordt ervan uitgegaan dat de rand van de IoT-opslagplaats de **/home/pi/iot-edge /** map op uw frambozen Pi 3. Als de opslagplaats ergens anders is, de paden overeenkomstig aanpassen.

#### <a name="logger-configuration"></a>Berichtenlogboek configuratie

Ervan uitgaande dat de gateway-opslagplaats bevindt zich in de **/home/pi/iot-edge /** map, de module berichtenlogboek als volgt configureren:

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a>De moduleconfiguratie uitschakelen

De voorbeeldconfiguratie van het voor het apparaat uitschakelen wordt ervan uitgegaan dat een Texas instrumenten SensorTag-apparaat. Een standaard uitschakelen-apparaat dat kan worden uitgevoerd als een randapparaat GATT moet werken, maar u moet mogelijk de GATT kenmerkende-id's en de gegevens bij te werken. Het MAC-adres van uw apparaat SensorTag toevoegen:

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

Als u een apparaat SensorTag niet gebruikt, raadpleegt u de documentatie voor uw apparaat uitschakelen om te bepalen of u moet de GATT kenmerk id's en gegevenswaarden bijwerken.

#### <a name="iot-hub-module"></a>IoT Hub-module

Voeg de naam van uw IoT Hub. De waarde voor het domeinachtervoegsel is doorgaans **azure devices.net**:

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Configuratie van de identiteit van toewijzing

Voeg de MAC-adres van uw apparaat SensorTag en het apparaat-ID en sleutel van de **SensorTag_01** apparaat die u hebt toegevoegd aan uw IoT-Hub:

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Module printerconfiguratie uitschakelen

```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a>De Moduleconfiguratie BLEC2D

```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a>Configuratie van de routering

De volgende configuratie zorgt ervoor dat de volgende routering tussen de rand van de IoT-modules:

* De **berichtenlogboek** module ontvangt en registreert alle berichten.
* De **SensorTag** module verzendt berichten naar zowel de **toewijzing** en **uitschakelen Printer** modules.
* De **toewijzing** module verzendt berichten naar de **IoTHub** module up worden verzonden naar uw IoT-Hub.
* De **IoTHub** module verzendt berichten terug naar de **toewijzing** module.
* De **toewijzing** module verzendt berichten naar de **BLEC2D** module.
* De **BLEC2D** module verzendt berichten terug naar de **Sensor Tag** module.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

Om het voorbeeld uitvoert, geeft u het pad naar het JSON-configuratiebestand als een parameter voor de **uitschakelen\_gateway** binaire. De volgende opdracht wordt ervan uitgegaan dat u gebruikt de **gateway_sample.json** configuratiebestand. Voer deze opdracht uit uit de **iot-edge** map op de Pi frambozen:

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

U moet mogelijk de kleine schakelaar op het apparaat SensorTag detecteerbaar voordat u het voorbeeld uitvoert.

Wanneer u het voorbeeld uitvoert, kunt u de [apparaat explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) of de [iothub explorer](https://github.com/Azure/iothub-explorer) hulpprogramma voor het bewaken van de rand van de IoT-gateway van het apparaat SensorTag verzendt berichten. Bijvoorbeeld, kunt met iothub explorer u controleren apparaat-naar-cloud-berichten met de volgende opdracht:

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a>Cloud-naar-apparaat-berichten verzenden

De module uitschakelen ondersteunt ook opdrachten uit IoT Hub verzenden naar het apparaat. U kunt de [apparaat explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) of de [iothub explorer](https://github.com/Azure/iothub-explorer) hulpprogramma voor de JSON berichten verzenden waarmee de module uitschakelen gateway worden doorgestuurd naar het apparaat uitschakelen.

Als u het Texas instrumenten SensorTag-apparaat gebruikt, kunt u op de rode LED, groene LED of alarm door opdrachten uit IoT Hub verzendt. Voordat u opdrachten uit IoT Hub verzendt, moet u eerst de volgende twee JSON-berichten verzenden in volgorde. U kunt vervolgens een van de opdrachten voor het inschakelen van de lichten of alarm verzenden.

1. Alle LED's en het alarm opnieuw instellen (deze uit te schakelen):

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. I/o's configureren als 'extern':

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

U kunt nu een van de volgende opdrachten om in te schakelen op de lichten of alarm op het apparaat SensorTag verzenden:

* De rode LED inschakelen:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* De groene LED inschakelen:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* Het alarm inschakelen:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a>Volgende stappen

Als u wilt krijgen tot een uitgebreidere kennis van IoT-rand en Experimenteer met enkele codevoorbeelden, gaat u naar de volgende zelfstudies voor ontwikkelaars en bronnen:

* [Azure IoT-rand][lnk-sdk]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md

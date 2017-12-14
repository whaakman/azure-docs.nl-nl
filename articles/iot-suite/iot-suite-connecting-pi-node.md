---
title: Raspberry Pi voor externe controle in Node.js - Azure inrichten | Microsoft Docs
description: Beschrijft hoe een frambozen Pi apparaat aansluit op de Azure IoT Suite vooraf geconfigureerde oplossing voor externe controle met behulp van een toepassing die is geschreven in Node.js.
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: d2cc72f26568a362049f24323ffa598b6012d77f
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Verbind het apparaat frambozen Pi met de vooraf geconfigureerde oplossing voor externe controle (Node.js)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Deze zelfstudie laat zien hoe een fysiek apparaat verbindt met de vooraf geconfigureerde oplossing voor externe controle. In deze zelfstudie gebruikt u Node.js, dit is een goede optie voor omgevingen met minimale resourcebeperkingen.

### <a name="required-hardware"></a>Vereiste hardware

Een desktopcomputer zodat u kunt extern verbinding maken met de opdrachtregel op de frambozen Pi.

[Microsoft IoT Starter Kit voor frambozen Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) of gelijkwaardige onderdelen. Deze zelfstudie maakt gebruik van de volgende items van de kit:

- Raspberry Pi 3
- MicroSD-kaart (met NOOBS)
- Een Mini USB-kabel
- Een Ethernet-kabel

### <a name="required-desktop-software"></a>Vereiste bureaublad software

SSH-client moet u op de computer waarmee u kunt extern toegang tot de opdrachtregel op de frambozen Pi.

- Windows bevat geen een SSH-client. Wordt u aangeraden [PuTTY](http://www.putty.org/).
- De meeste Linux-distributies en Mac OS omvatten het SSH-opdrachtregelprogramma. Zie voor meer informatie [SSH met behulp van Linux- of Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Vereiste frambozen Pi software

Als u dit nog niet hebt gedaan, Node.js-versie 4.0.0 of hoger installeren op uw frambozen Pi. De volgende stappen ziet u hoe Node.js v6.11.4 installeren op uw Pi frambozen:

1. Verbinding maken met uw frambozen Pi met `ssh`. Zie voor meer informatie [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) op de [frambozen Pi website](https://www.raspberrypi.org/).

1. Gebruik de volgende opdracht om bij te werken uw Pi frambozen:

    ```sh
    sudo apt-get update
    ```

1. Gebruik de volgende opdracht voor het downloaden van de Node.js-binaire bestanden naar uw Pi frambozen:

    ```sh
    wget https://nodejs.org/dist/v6.11.4/node-v6.11.4-linux-armv7l.tar.gz
    ```

1. Gebruik de volgende opdracht voor het installeren van de binaire bestanden:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.11.4-linux-armv7l.tar.gz
    ```

1. Gebruik de volgende opdracht om te controleren of dat u hebt de Node.js-v6.11.4 is geïnstalleerd:

    ```sh
    node --version
    ```

## <a name="create-a-nodejs-solution"></a>Een Node.js-oplossing maken

De volgende stappen uit met behulp van de `ssh` verbinding met uw Pi frambozen:

1. Maak een map `RemoteMonitoring` in uw basismap op de frambozen Pi. Navigeer naar deze map op de opdrachtregel:

    ```sh
    cd ~
    mkdir RemoteMonitoring
    cd RemoteMonitoring
    ```

1. Als u wilt downloaden en installeren van de pakketten die u moet voltooien van de voorbeeld-app, voer de volgende opdrachten:

    ```sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. In de `RemoteMonitoring` map maken van een bestand met de naam **remote_monitoring.js**. Open dit bestand in een teksteditor. Op de Pi frambozen, kunt u de `nano` of `vi` teksteditors.

1. In de **remote_monitoring.js** bestand, voeg de volgende `require` instructies:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. Voeg de volgende variabelendeclaraties achter de `require`-instructies toe. Vervang de tijdelijke aanduiding voor waarden `{Device Id}` en `{Device Key}` met waarden die u hebt genoteerd voor het apparaat dat u hebt ingericht in de oplossing voor externe controle. De hostnaam van de IoT-Hub van de oplossing gebruiken om te vervangen `{IoTHub Name}`. Bijvoorbeeld, als de hostnaam van uw IoT Hub is `contoso.azure-devices.net`, Vervang `{IoTHub Name}` met `contoso`:

    ```nodejs
    var connectionString = 'HostName={IoTHub Name}.azure-devices.net;DeviceId={Device Id};SharedAccessKey={Device Key}';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Sommige base om telemetriegegevens te definiëren, voeg de volgende variabelen:

    ```nodejs
    var temperature = 50;
    var temperatureUnit = 'F';
    var humidity = 50;
    var humidityUnit = '%';
    var pressure = 55;
    var pressureUnit = 'psig';
    ```

1. Bepaalde eigenschapswaarden definiëren, voeg de volgende variabelen:

    ```nodejs
    var temperatureSchema = 'chiller-temperature;v1';
    var humiditySchema = 'chiller-humidity;v1';
    var pressureSchema = 'chiller-pressure;v1';
    var interval = "00:00:05";
    var deviceType = "Chiller";
    var deviceFirmware = "1.0.0";
    var deviceFirmwareUpdateStatus = "";
    var deviceLocation = "Building 44";
    var deviceLatitude = 47.638928;
    var deviceLongitude = -122.13476;
    ```

1. Voeg de volgende variabele om de gemelde eigenschappen om te verzenden naar de oplossing te geven. Deze eigenschappen zijn metagegevens om te beschrijven van de methoden en telemetrie van het apparaat wordt gebruikt:

    ```nodejs
    var reportedProperties = {
      "Protocol": "MQTT",
      "SupportedMethods": "Reboot,FirmwareUpdate,EmergencyValveRelease,IncreasePressure",
      "Telemetry": {
        "TemperatureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
          "MessageSchema": {
            "Name": temperatureSchema,
            "Format": "JSON",
            "Fields": {
              "temperature": "Double",
              "temperature_unit": "Text"
            }
          }
        },
        "HumiditySchema": {
          "Interval": interval,
          "MessageTemplate": "{\"humidity\":${humidity},\"humidity_unit\":\"${humidity_unit}\"}",
          "MessageSchema": {
            "Name": humiditySchema,
            "Format": "JSON",
            "Fields": {
              "humidity": "Double",
              "humidity_unit": "Text"
            }
          }
        },
        "PressureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"pressure\":${pressure},\"pressure_unit\":\"${pressure_unit}\"}",
          "MessageSchema": {
            "Name": pressureSchema,
            "Format": "JSON",
            "Fields": {
              "pressure": "Double",
              "pressure_unit": "Text"
            }
          }
        }
      },
      "Type": deviceType,
      "Firmware": deviceFirmware,
      "FirmwareUpdateStatus": deviceFirmwareUpdateStatus,
      "Location": deviceLocation,
      "Latitude": deviceLatitude,
      "Longitude": deviceLongitude
    }
    ```

1. Voeg de volgende Help-functie Bewerkingsresultaten om af te drukken:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. De volgende Help-functie wilt willekeurige de telemetrie-waarden toevoegen:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Voeg de volgende functie voor het afhandelen van directe methodeaanroepen van de oplossing. De oplossing maakt gebruik van rechtstreekse methoden om te reageren op apparaten:

    ```nodejs
    function onDirectMethod(request, response) {
      // Implement logic asynchronously here.
      console.log('Simulated ' + request.methodName);

      // Complete the response
      response.send(200, request.methodName + ' was called on the device', function (err) {
        if (!!err) {
          console.error('An error ocurred when sending a method response:\n' +
            err.toString());
        } else {
          console.log('Response to method \'' + request.methodName +
            '\' sent successfully.');
        }
      });
    }
    ```

1. Voeg de volgende code voor het verzenden van telemetriegegevens naar de oplossing. De client-app worden eigenschappen toegevoegd aan het bericht voor het identificeren van het berichtschema:

    ```node.js
    function sendTelemetry(data, schema) {
      var d = new Date();
      var payload = JSON.stringify(data);
      var message = new Message(payload);
      message.properties.add('$$CreationTimeUtc', d.toISOString());
      message.properties.add('$$MessageSchema', schema);
      message.properties.add('$$ContentType', 'JSON');

      console.log('Sending device message data:\n' + payload);
      client.sendEvent(message, printErrorFor('send event'));
    }
    ```

1. Voeg de volgende code voor het maken van een clientexemplaar:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Voeg de volgende code toe:

    - De verbinding openen.
    - Een handler voor de gewenste eigenschappen instellen.
    - Eigenschappen van de gerapporteerde verzenden.
    - Registreer de handlers voor de rechtstreekse methoden.
    - Beginnen met het verzenden van telemetrie.

    ```nodejs
    client.open(function (err) {
      if (err) {
        printErrorFor('open')(err);
      } else {
        // Create device Twin
        client.getTwin(function (err, twin) {
          if (err) {
            console.error('Could not get device twin');
          } else {
            console.log('Device twin created');

            twin.on('properties.desired', function (delta) {
              // Handle desired properties set by solution
              console.log('Received new desired properties:');
              console.log(JSON.stringify(delta));
            });

            // Send reported properties
            twin.properties.reported.update(reportedProperties, function (err) {
              if (err) throw err;
              console.log('twin state reported');
            });

            // Register handlers for all the method names we are interested in.
            // Consider separate handlers for each method.
            client.onDeviceMethod('Reboot', onDirectMethod);
            client.onDeviceMethod('FirmwareUpdate', onDirectMethod);
            client.onDeviceMethod('EmergencyValveRelease', onDirectMethod);
            client.onDeviceMethod('IncreasePressure', onDirectMethod);
          }
        });

        // Start sending telemetry
        var sendTemperatureInterval = setInterval(function () {
          temperature += generateRandomIncrement();
          var data = {
            'temperature': temperature,
            'temperature_unit': temperatureUnit
          };
          sendTelemetry(data, temperatureSchema)
        }, 5000);

        var sendHumidityInterval = setInterval(function () {
          humidity += generateRandomIncrement();
          var data = {
            'humidity': humidity,
            'humidity_unit': humidityUnit
          };
          sendTelemetry(data, humiditySchema)
        }, 5000);

        var sendPressureInterval = setInterval(function () {
          pressure += generateRandomIncrement();
          var data = {
            'pressure': pressure,
            'pressure_unit': pressureUnit
          };
          sendTelemetry(data, pressureSchema)
        }, 5000);

        client.on('error', function (err) {
          printErrorFor('client')(err);
          if (sendTemperatureInterval) clearInterval(sendTemperatureInterval);
          if (sendHumidityInterval) clearInterval(sendHumidityInterval);
          if (sendPressureInterval) clearInterval(sendPressureInterval);
          client.close(printErrorFor('client.close'));
        });
      }
    });
    ```

1. Sla de wijzigingen in de **remote_monitoring.js** bestand.

1. Start de voorbeeldtoepassing, voeren de volgende opdracht achter de opdrachtprompt op de Pi frambozen:

    ```sh
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

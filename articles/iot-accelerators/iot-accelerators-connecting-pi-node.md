---
title: Inrichten van Raspberry Pi tot externe controle in Node.js - Azure | Microsoft Docs
description: Beschrijft hoe u een Raspberry Pi-apparaat verbinden met de oplossingsversnellers bewaking op afstand met behulp van een toepassing die is geschreven in Node.js.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 01/24/2018
ms.author: dobett
ms.openlocfilehash: fe0a84d9d88f5287ca3a114225bde619f9312e69
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53628354"
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-solution-accelerator-nodejs"></a>Uw Raspberry Pi-apparaat verbinden met de Remote Monitoring solution accelerator (Node.js)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Deze zelfstudie leert u hoe u een echt apparaat verbinden met de oplossingsverbetering voor externe controle. In deze zelfstudie gebruikt u Node.js, dit is een goede optie voor omgevingen met minimale resourcebeperkingen.

Als u liever een apparaat simuleren, Zie [maken en test een nieuw gesimuleerd apparaat](iot-accelerators-remote-monitoring-create-simulated-device.md).

### <a name="required-hardware"></a>Vereiste hardware

Een desktopcomputer waarmee u kunt extern verbinding maken met de opdrachtregel op de Raspberry Pi.

[Microsoft IoT Starter Kit voor Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) of equivalente onderdelen. In deze zelfstudie wordt de volgende items uit de kit:

- Raspberry Pi 3
- MicroSD-kaart (met NOOBS)
- Een Mini USB-kabel
- Een Ethernet-kabel

### <a name="required-desktop-software"></a>Vereiste bureaublad-software

SSH-client moet u op de computer waarmee u kunt voor externe toegang tot de opdrachtregel op de Raspberry Pi.

- Windows bevat geen een SSH-client. Wordt u aangeraden [PuTTY](https://www.putty.org/).
- De meeste Linux-distributies en Mac OS omvatten het SSH-opdrachtregelprogramma. Zie voor meer informatie, [SSH met behulp van Linux- of Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Vereiste software voor Raspberry Pi

Als u dit nog niet hebt gedaan, installeert u Node.js versie 4.0.0 of hoger op uw Raspberry Pi. De volgende stappen laten zien hoe u Node.js v6 installeren op uw Raspberry Pi:

1. Verbinding maken met uw Raspberry Pi met `ssh`. Zie voor meer informatie, [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) op de [Raspberry Pi website](https://www.raspberrypi.org/).

1. Gebruik de volgende opdracht uit om bij te werken uw Raspberry Pi:

    ```sh
    sudo apt-get update
    ```

1. Gebruik de volgende opdrachten om te verwijderen van een bestaande installatie van Node.js uit uw Raspberry Pi:

    ```sh
    sudo apt-get remove nodered -y
    sudo apt-get remove nodejs nodejs-legacy -y
    sudo apt-get remove npm  -y
    ```

1. Gebruik de volgende opdracht om te downloaden en Node.js v6 installeren op uw Raspberry Pi:

    ```sh
    curl -sL https://deb.nodesource.com/setup_6.x | sudo bash -
    sudo apt-get install nodejs npm
    ```

1. Gebruik de volgende opdracht om te controleren of dat u hebt Node.js v6.11.4 is geïnstalleerd:

    ```sh
    node --version
    ```

## <a name="create-a-nodejs-solution"></a>Een Node.js-oplossing maken

De volgende stappen met behulp van de `ssh` verbinding met uw Raspberry Pi:

1. Maak een map genaamd `remotemonitoring` in de basismap van de Raspberry Pi. Navigeer naar deze map op uw opdrachtregel:

    ```sh
    cd ~
    mkdir remotemonitoring
    cd remotemonitoring
    ```

1. Als u wilt downloaden en installeren van de pakketten die u nodig hebt voor de voorbeeld-app, voer de volgende opdrachten:

    ```sh
    npm install async azure-iot-device azure-iot-device-mqtt
    ```

1. In de `remotemonitoring` map, maakt u een bestand met de naam **remote_monitoring.js**. Open dit bestand in een teksteditor. Aan de Raspberry Pi, kunt u de `nano` of `vi` teksteditors.

1. In de **remote_monitoring.js** bestand, voeg de volgende `require` instructies:

    ```nodejs
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    var async = require('async');
    ```

1. Voeg de volgende variabelendeclaraties achter de `require`-instructies toe. Vervang de tijdelijke aanduidingswaarde `{device connection string}` met de waarde die u hebt genoteerd voor het apparaat dat u hebt ingericht in de oplossing voor externe controle:

    ```nodejs
    var connectionString = '{device connection string}';
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

1. Voor het definiëren van sommige eigenschapswaarden, voeg de volgende variabelen:

    ```nodejs
    var temperatureSchema = 'chiller-temperature;v1';
    var humiditySchema = 'chiller-humidity;v1';
    var pressureSchema = 'chiller-pressure;v1';
    var deviceType = "Chiller";
    var deviceFirmware = "1.0.0";
    var deviceFirmwareUpdateStatus = "";
    var deviceLocation = "Building 44";
    var deviceLatitude = 47.638928;
    var deviceLongitude = -122.13476;
    var deviceOnline = true;
    ```

1. Voeg de volgende variabele voor het definiëren van de gerapporteerde eigenschappen om te verzenden naar de oplossing. Deze eigenschappen omvatten metagegevens om te beschrijven van de methoden en telemetrie van het apparaat wordt gebruikt:

    ```nodejs
    var reportedProperties = {
      "Protocol": "MQTT",
      "SupportedMethods": "Reboot,FirmwareUpdate,EmergencyValveRelease,IncreasePressure",
      "Telemetry": {
        "TemperatureSchema": {
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
      "Longitude": deviceLongitude,
      "Online": deviceOnline
    }
    ```

1. Voeg de volgende Help-functie resultaten van de bewerking om af te drukken:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Voeg de volgende helperfunctie te gebruiken op een willekeurige kleur geven de telemetriewaarden:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Voeg de volgende algemene functie voor het afhandelen van rechtstreekse methodeaanroepen van de oplossing. De functie geeft informatie weer over de directe methode die is aangeroepen, maar in dit voorbeeld het apparaat op geen enkele manier niet wijzigen. De oplossing maakt gebruik van directe methoden om te reageren op apparaten:

    ```nodejs
    function onDirectMethod(request, response) {
      // Implement logic asynchronously here.
      console.log('Simulated ' + request.methodName);

      // Complete the response
      response.send(200, request.methodName + ' was called on the device', function (err) {
        if (err) console.error('Error sending method response :\n' + err.toString());
        else console.log('200 Response to method \'' + request.methodName + '\' sent successfully.');
      });
    }
    ```

1. Voeg de volgende functie voor het afhandelen van de **FirmwareUpdate** rechtstreekse methodeaanroepen van de oplossing. De functie controleert of de parameters die worden doorgegeven in de nettolading van directe methode en de simulatie van een firmware-update wordt asynchroon uitgevoerd:

    ```node.js
    function onFirmwareUpdate(request, response) {
      // Get the requested firmware version from the JSON request body
      var firmwareVersion = request.payload.Firmware;
      var firmwareUri = request.payload.FirmwareUri;
      
      // Ensure we got a firmware values
      if (!firmwareVersion || !firmwareUri) {
        response.send(400, 'Missing firmware value', function(err) {
          if (err) console.error('Error sending method response :\n' + err.toString());
          else console.log('400 Response to method \'' + request.methodName + '\' sent successfully.');
        });
      } else {
        // Respond the cloud app for the device method
        response.send(200, 'Firmware update started.', function(err) {
          if (err) console.error('Error sending method response :\n' + err.toString());
          else {
            console.log('200 Response to method \'' + request.methodName + '\' sent successfully.');

            // Run the simulated firmware update flow
            runFirmwareUpdateFlow(firmwareVersion, firmwareUri);
          }
        });
      }
    }
    ```

1. Voeg de volgende functie voor het simuleren van een stroom langlopende firmware bijwerken die wordt uitgevoerd aan de oplossing doorgegeven:

    ```node.js
    // Simulated firmwareUpdate flow
    function runFirmwareUpdateFlow(firmwareVersion, firmwareUri) {
      console.log('Simulating firmware update flow...');
      console.log('> Firmware version passed: ' + firmwareVersion);
      console.log('> Firmware URI passed: ' + firmwareUri);
      async.waterfall([
        function (callback) {
          console.log("Image downloading from " + firmwareUri);
          var patch = {
            FirmwareUpdateStatus: 'Downloading image..'
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(10000, callback);
        },
        function (callback) {
          console.log("Downloaded, applying firmware " + firmwareVersion);
          deviceOnline = false;
          var patch = {
            FirmwareUpdateStatus: 'Applying firmware..',
            Online: false
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(8000, callback);
        },
        function (callback) {
          console.log("Rebooting");
          var patch = {
            FirmwareUpdateStatus: 'Rebooting..'
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(10000, callback);
        },
        function (callback) {
          console.log("Firmware updated to " + firmwareVersion);
          deviceOnline = true;
          var patch = {
            FirmwareUpdateStatus: 'Firmware updated',
            Online: true,
            Firmware: firmwareVersion
          };
          reportUpdateThroughTwin(patch, callback);
          callback(null);
        }
      ], function(err) {
        if (err) {
          console.error('Error in simulated firmware update flow: ' + err.message);
        } else {
          console.log("Completed simulated firmware update flow");
        }
      });

      // Helper function to update the twin reported properties.
      function reportUpdateThroughTwin(patch, callback) {
        console.log("Sending...");
        console.log(JSON.stringify(patch, null, 2));
        client.getTwin(function(err, twin) {
          if (!err) {
            twin.properties.reported.update(patch, function(err) {
              if (err) callback(err);
            });      
          } else {
            if (err) callback(err);
          }
        });
      }

      function sleep(milliseconds, callback) {
        console.log("Simulate a delay (milleseconds): " + milliseconds);
        setTimeout(function () {
          callback(null);
        }, milliseconds);
      }
    }
    ```

1. Voeg de volgende code voor het verzenden van telemetriegegevens naar de oplossing. Eigenschappen van de client-app toegevoegd aan het bericht om te identificeren van het berichtschema:

    ```node.js
    function sendTelemetry(data, schema) {
      if (deviceOnline) {
        var d = new Date();
        var payload = JSON.stringify(data);
        var message = new Message(payload);
        message.properties.add('$$CreationTimeUtc', d.toISOString());
        message.properties.add('$$MessageSchema', schema);
        message.properties.add('$$ContentType', 'JSON');

        console.log('Sending device message data:\n' + payload);
        client.sendEvent(message, printErrorFor('send event'));
      } else {
        console.log('Offline, not sending telemetry');
      }
    }
    ```

1. Voeg de volgende code voor het maken van een clientexemplaar:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Voeg de volgende code toe:

    * Open de verbinding.
    * Instellen van een handler voor de gewenste eigenschappen.
    * Gerapporteerde eigenschappen verzenden.
    * Registreer handlers voor de directe methoden. Het voorbeeld wordt een afzonderlijke handler voor de directe methode die firmware-update.
    * Beginnen met het verzenden van telemetrie.

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
              console.log('Twin state reported');
            });

            // Register handlers for all the method names we are interested in.
            // Consider separate handlers for each method.
            client.onDeviceMethod('Reboot', onDirectMethod);
            client.onDeviceMethod('FirmwareUpdate', onFirmwareUpdate);
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

1. Sla de wijzigingen aan de **remote_monitoring.js** bestand.

1. Voer de volgende opdracht achter de opdrachtprompt op de Raspberry Pi voor het starten van de voorbeeldtoepassing:

    ```sh
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

---
title: Verbinding maken met een apparaat met behulp van Node.js | Microsoft Docs
description: Beschrijft hoe een apparaat aansluit op de Azure IoT Suite vooraf geconfigureerde oplossing voor externe controle met behulp van een toepassing die is geschreven in Node.js.
services: 
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
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 87a2e97638508eef1d90a219cfb38d1fcac81d55
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Verbind het apparaat met de vooraf geconfigureerde oplossing voor externe controle (Node.js)
[!INCLUDE [iot-suite-v1-selector-connecting](../../includes/iot-suite-v1-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a>Een node.js-voorbeeld-oplossing maken

Zorg dat Node.js-versie 0.11.5 of hoger is geïnstalleerd op uw ontwikkelcomputer. U kunt uitvoeren `node --version` op de opdrachtregel om te controleren van de versie.

1. Maak een map **RemoteMonitoring** op uw ontwikkelcomputer. Navigeer naar deze map in uw omgeving vanaf de opdrachtregel.

1. Voer de volgende opdrachten om te downloaden en installeren van de pakketten die u moet de voorbeeld-app te voltooien:

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. In de **RemoteMonitoring** map maken van een bestand met de naam **remote_monitoring.js**. Open dit bestand in een teksteditor.

1. In de **remote_monitoring.js** bestand, voeg de volgende `require` instructies:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. Voeg de volgende variabelendeclaraties achter de `require`-instructies toe. Vervang de waarden van de tijdelijke aanduidingen [apparaat-id] en [apparaatsleutel] door de waarden die u voor het apparaat hebt genoteerd in het dashboard van de oplossing voor externe controle. Gebruik de hostnaam van de IoT Hub uit het oplossingsdashboard om [IoTHub-naam] te vervangen. Als uw IoT Hub-hostnaam bijvoorbeeld **contoso.azure devices.net** is, vervangt u [IoTHub-naam] door **contoso**:

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Voeg de volgende variabelen om sommige telemetriegegevens base te definiëren:

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. De volgende Help-functie om af te drukken Bewerkingsresultaten toevoegen:

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

1. Voeg de volgende definitie voor de **DeviceInfo** object van het apparaat verzendt bij het opstarten:

    ```nodejs
    var deviceMetaData = {
        'ObjectType': 'DeviceInfo',
        'IsSimulatedDevice': 0,
        'Version': '1.0',
        'DeviceProperties': {
            'DeviceID': deviceId,
            'HubEnabledState': 1
        }
    };
    ```

1. Voeg de volgende definitie voor de apparaat-twin waarden gerapporteerd. Deze definitie bevat beschrijvingen van de directe methoden die biedt ondersteuning voor het apparaat:

    ```nodejs
    var reportedProperties = {
        "Device": {
            "DeviceState": "normal",
            "Location": {
                "Latitude": 47.642877,
                "Longitude": -122.125497
            }
        },
        "Config": {
            "TemperatureMeanValue": 56.7,
            "TelemetryInterval": 45
        },
        "System": {
            "Manufacturer": "Contoso Inc.",
            "FirmwareVersion": "2.22",
            "InstalledRAM": "8 MB",
            "ModelNumber": "DB-14",
            "Platform": "Plat 9.75",
            "Processor": "i3-9",
            "SerialNumber": "SER99"
        },
        "Location": {
            "Latitude": 47.642877,
            "Longitude": -122.125497
        },
        "SupportedMethods": {
            "Reboot": "Reboot the device",
            "InitiateFirmwareUpdate--FwPackageURI-string": "Updates device Firmware. Use parameter FwPackageURI to specifiy the URI of the firmware file"
        },
    }
    ```

1. Voeg de volgende functie voor het afhandelen van de **opnieuw opstarten** directe aanroep van methode:

    ```nodejs
    function onReboot(request, response) {
        // Implement actual logic here.
        console.log('Simulated reboot...');

        // Complete the response
        response.send(200, "Rebooting device", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

1. Voeg de volgende functie voor het afhandelen van de **InitiateFirmwareUpdate** directe aanroep van methode. Deze directe methode wordt een parameter voor de locatie opgeven van de firmware-installatiekopie te downloaden en initieert de firmware bijwerken op het apparaat asynchroon:

    ```nodejs
    function onInitiateFirmwareUpdate(request, response) {
        console.log('Simulated firmware update initiated, using: ' + request.payload.FwPackageURI);

        // Complete the response
        response.send(200, "Firmware update initiated", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });

        // Add logic here to perform the firmware update asynchronously
    }
    ```

1. Voeg de volgende code voor het maken van een clientexemplaar:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Voeg de volgende code toe:

    * De verbinding openen.
    * Verzenden van de **DeviceInfo** object.
    * Een handler voor de gewenste eigenschappen instellen.
    * Eigenschappen van de gerapporteerde verzenden.
    * Registreer de handlers voor de rechtstreekse methoden.
    * Beginnen met het verzenden van telemetrie.

    ```nodejs
    client.open(function (err) {
        if (err) {
            printErrorFor('open')(err);
        } else {
            console.log('Sending device metadata:\n' + JSON.stringify(deviceMetaData));
            client.sendEvent(new Message(JSON.stringify(deviceMetaData)), printErrorFor('send metadata'));

            // Create device twin
            client.getTwin(function(err, twin) {
                if (err) {
                    console.error('Could not get device twin');
                } else {
                    console.log('Device twin created');

                    twin.on('properties.desired', function(delta) {
                        console.log('Received new desired properties:');
                        console.log(JSON.stringify(delta));
                    });

                    // Send reported properties
                    twin.properties.reported.update(reportedProperties, function(err) {
                        if (err) throw err;
                        console.log('twin state reported');
                    });

                    // Register handlers for direct methods
                    client.onDeviceMethod('Reboot', onReboot);
                    client.onDeviceMethod('InitiateFirmwareUpdate', onInitiateFirmwareUpdate);
                }
            });

            // Start sending telemetry
            var sendInterval = setInterval(function () {
                temperature += generateRandomIncrement();
                externalTemperature += generateRandomIncrement();
                humidity += generateRandomIncrement();

                var data = JSON.stringify({
                    'DeviceID': deviceId,
                    'Temperature': temperature,
                    'Humidity': humidity,
                    'ExternalTemperature': externalTemperature
                });

                console.log('Sending device event data:\n' + data);
                client.sendEvent(new Message(data), printErrorFor('send event'));
            }, 5000);

            client.on('error', function (err) {
                printErrorFor('client')(err);
                if (sendInterval) clearInterval(sendInterval);
                client.close(printErrorFor('client.close'));
            });
        }
    });
    ```

1. Sla de wijzigingen in de **remote_monitoring.js** bestand.

1. Voer de volgende opdracht achter de opdrachtprompt de voorbeeldtoepassing starten:
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-v1-visualize-connecting](../../includes/iot-suite-v1-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

---
title: Verbinding maken met een algemene Node.js-clienttoepassing op Azure IoT Central | Microsoft Docs
description: Als ontwikkelaar in een apparaat, hoe u een algemene Node.js-apparaat verbinden met uw Azure IoT Central-toepassing.
author: dominicbetts
ms.author: dobett
ms.date: 10/26/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: cc6857750534dad2ded29178eb3f140fc25cce0d
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54410486"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Verbinding maken met een algemene clienttoepassing aan uw Azure IoT Central-toepassing (Node.js)

Dit artikel wordt beschreven hoe u als ontwikkelaar apparaat om een algemene Node.js-toepassing die een echt apparaat naar uw Microsoft Azure IoT Central-toepassing verbinding te maken.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

1. Een Azure IoT Central-toepassing. Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).
1. Een ontwikkelcomputer met [Node.js](https://nodejs.org/) versie 4.0.0 of hoger is geïnstalleerd. U kunt uitvoeren `node --version` vanaf de opdrachtregel om uw versie te controleren. Node.js is beschikbaar voor een groot aantal verschillende besturingssystemen.

## <a name="create-a-device-template"></a>Een apparaat-sjabloon maken

U moet een sjabloon van het apparaat met de volgende metingen en apparaateigenschappen die zijn gedefinieerd in uw Azure IoT Central-toepassing:

### <a name="telemetry-measurements"></a>Telemetrie-metingen

Voeg de volgende telemetrie in de **metingen** pagina:

| Weergavenaam | Veldnaam  | Eenheden | Min. | Max. | Aantal decimalen |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Temperatuur  | temperatuur | F     | 60  | 110 | 0              |
| Vochtigheid     | vochtigheid    | %     | 0   | 100 | 0              |
| Druk     | pressure    | kPa   | 80  | 110 | 0              |

> [!NOTE]
  Het gegevenstype van de meting telemetrie is een drijvende-kommagetal zijn.

Veldnamen precies zoals weergegeven in de tabel in de sjabloon voor het apparaat invoeren. Als de veldnamen niet de namen van eigenschappen in de bijbehorende apparaatcode overeenkomen, kan de telemetrie kan niet worden weergegeven in de toepassing.

### <a name="state-measurements"></a>Status metingen

Voeg de volgende status hebben in de **metingen** pagina:

| Weergavenaam | Veldnaam  | Waarde 1 | Weergavenaam | Waarde 2 | Weergavenaam |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Ventilatormodus     | ventilatormodus     | 1       | In uitvoering      | 0       | Gestopt      |

> [!NOTE]
  Het gegevenstype van de meting van de status is een tekenreeks.

Veldnamen precies zoals weergegeven in de tabel in de sjabloon voor het apparaat invoeren. Als de veldnamen niet de namen van eigenschappen in de bijbehorende apparaatcode overeenkomen, kan de status kan niet worden weergegeven in de toepassing.

### <a name="event-measurements"></a>Gebeurtenis-metingen

Voeg de volgende gebeurtenis in de **metingen** pagina:

| Weergavenaam | Veldnaam  | Severity |
| ------------ | ----------- | -------- |
| Oververhitting  | overheat    | Fout    |

> [!NOTE]
  Het gegevenstype van de meting van de gebeurtenis is een tekenreeks.

### <a name="device-properties"></a>Apparaateigenschappen

Voeg de volgende apparaateigenschappen in de **eigenschappenpagina**:

| Weergavenaam        | Veldnaam        | Gegevenstype |
| ------------------- | ----------------- | --------- |
| Serienummer       | Serienummer      | tekst      |
| De fabrikant van apparaat | fabrikant      | tekst      |

Voer de veldnamen precies zoals weergegeven in de tabel in de sjabloon voor het apparaat. Als de veldnamen niet de namen van eigenschappen in de bijbehorende apparaatcode overeenkomen, kan de toepassing niet de waarde van de eigenschap apparaat weergeven.

### <a name="settings"></a>Instellingen

Voeg de volgende **getal** instellingen in de **instellingenpagina**:

| Weergavenaam    | Veldnaam     | Eenheden | Aantal decimalen | Min. | Max.  | Oorspronkelijk |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Snelheid van ventilator       | fanSpeed       | rpm   | 0        | 0   | 3000 | 0       |
| Temperatuur instellen | temperatuurInstellen | F     | 0        | 20  | 200  | 80      |

Voer veldnaam precies zoals weergegeven in de tabel in de sjabloon voor het apparaat. Als de veldnamen niet de namen van eigenschappen in de bijbehorende apparaatcode overeenkomen, kan het apparaat geen waarde van de instelling ontvangen.

## <a name="add-a-real-device"></a>Echt apparaat toevoegen

Voeg een echt apparaat van de sjabloon van het apparaat u maken en noteer de apparaatverbindingsreeks in uw Azure IoT Central-toepassing. Zie voor stapsgewijze instructies over het verbinden van een Node.js-toepassing naar IoT Central [verbindingsreeks voor een echt apparaat van de toepassing genereren](tutorial-add-device.md#generate-connection-string-for-real-device-from-application) en [voorbereiden van de clientcode](tutorial-add-device.md#prepare-the-client-code) in de zelfstudies > Een apparaat toevoegen.

### <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken

De volgende stappen laten zien over het maken van een clienttoepassing die het echte apparaat dat u hebt toegevoegd aan de toepassing implementeert. Hier geeft de Node.js-toepassing echt apparaat. 

1. Maak de map `connected-air-conditioner-adv` op uw computer. Navigeer naar die map in uw opdrachtregelomgeving.

1. Voer de volgende opdrachten voor het initialiseren van uw Node.js-project:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Maak een bestand met de naam **connectedAirConditionerAdv.js** in de `connected-air-conditioner-adv` map.

1. Voeg de volgende `require` instructies toe aan het begin van de **connectedAirConditionerAdv.js** bestand:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    ```

1. Voeg de volgende variabelendeclaraties aan het bestand toe:

    ```javascript
    var connectionString = '{your device connection string}';
    var targetTemperature = 0;
    var client = clientFromConnectionString(connectionString);
    ```

  > [!NOTE]
  > Azure IoT Central is overgeschakeld naar het gebruik van Azure IoT Hub Device Provisioning service (DPS) voor alle apparaatverbindingen, volgt u deze instrustions naar [de apparaat-verbindingsreeks ophalen](concepts-connectivity.md#getting-device-connection-string) en Ga door met de rest van de zelfstudie. Voor meer informatie vindt u ook een gedetailleerd aantal instructies in [voorbereiden van de clientcode](tutorial-add-device.md#prepare-the-client-code) in zelfstudies > een apparaat toevoegen.


  Bijwerken van de tijdelijke aanduiding `{your device connection string}` door de verbindingsreeks van het apparaat. In dit voorbeeld wordt geïnitialiseerd `targetTemperature` op nul, eventueel tilt u de huidige lezen van het apparaat of -waarde van het dubbele apparaat. 

1. Voor het verzenden van telemetrie, status en gebeurtenis metingen aan uw Azure IoT Central-toepassing, voeg de volgende functie naar het bestand:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var humidity = 70 + (Math.random() * 10);
      var pressure = 90 + (Math.random() * 5);
      var fanmode = 0;
      var data = JSON.stringify({ 
        temperature: temperature, 
        humidity: humidity, 
        pressure: pressure,
        fanmode: (temperature > 25) ? "1" : "0",
        overheat: (temperature > 35) ? "ER123" : undefined });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

1. Voor het verzenden van eigenschappen van een apparaat aan uw Azure IoT Central-toepassing, voeg de volgende functie toe aan het bestand:

    ```javascript
    // Send device properties.
    function sendDeviceProperties(twin) {
      var properties = {
        serialNumber: '123-ABC',
        manufacturer: 'Contoso'
      };
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Voeg de definitie van de volgende voor het definiëren van de instellingen voor die het apparaat reageert op:

    ```javascript
    // Add any settings your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'fanSpeed': (newValue, callback) => {
          // Simulate it taking 1 second to set the fan speed.
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Voor het afhandelen van bijgewerkte instellingen vanuit uw Azure IoT Central-toepassing, moet u het volgende toevoegen aan het bestand:

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

1. Voeg het volgende voor het voltooien van de verbinding met Azure IoT Central en koppelt de functies in de clientcode:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            sendDeviceProperties(twin);
            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    // Start the device (connect it to Azure IoT Central).
    client.open(connectCallback);
    ```

## <a name="run-your-nodejs-application"></a>Uw Node.js-toepassing uitvoeren

Voer de volgende opdracht in uw omgeving opdrachtregel:

```cmd/sh
node connectedAirConditionerAdv.js
```

Als operator in uw Azure IoT Central-toepassing voor uw echte apparaat kunt u:

* De telemetrie bekijken die op de **metingen** pagina:

    ![Telemetrie bekijken](media/howto-connect-nodejs/viewtelemetry.png)

* Bekijk de eigenschapswaarden van het apparaat verzonden van uw apparaat op de **eigenschappen** pagina. De eigenschappen van apparaat tegels worden bijgewerkt als de verbinding geslaagd is. 

    ![Eigenschappen van apparaat weergeven](media/howto-connect-nodejs/viewproperties.png)

* Stel de temperatuur ventilator snelheid en het doel van de **instellingen** pagina. De waarden voor de instellingen worden gesynchroniseerd als de verbinding geslaagd is. 

    ![Snelheid van de set-ventilator](media/howto-connect-nodejs/setfanspeed.png)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een algemene Node.js-client verbinden met uw Azure IoT Central-toepassing, vindt hier u de voorgestelde volgende stappen:
* [Voorbereiden en verbinding maken met een Raspberry Pi](howto-connect-raspberry-pi-python.md)
<!-- Next how-tos in the sequence -->

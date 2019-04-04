---
title: Verbinding maken met een algemene Node.js-clienttoepassing op Azure IoT Central | Microsoft Docs
description: Als de ontwikkelaar van een apparaat, hoe u een algemene Node.js-apparaat verbinden met uw Azure IoT Central-toepassing.
author: dominicbetts
ms.author: dobett
ms.date: 02/04/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 4c04d9dbaf0065f2e68182c9ad84181845dee3e9
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905321"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Verbinding maken met een algemene clienttoepassing aan uw Azure IoT Central-toepassing (Node.js)

Dit artikel wordt beschreven hoe u als ontwikkelaar apparaat om een algemene Node.js-toepassing die een echt apparaat naar uw Microsoft Azure IoT Central-toepassing verbinding te maken.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

1. Een Azure IoT Central-toepassing. Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).
1. Een ontwikkelcomputer met [Node.js](https://nodejs.org/) versie 4.0.0 of hoger is geïnstalleerd. U kunt uitvoeren `node --version` vanaf de opdrachtregel om uw versie te controleren. Node.js is beschikbaar voor een groot aantal verschillende besturingssystemen.

## <a name="create-a-device-template"></a>Een apparaat-sjabloon maken

In uw Azure IoT Central-toepassing moet u de sjabloon van een apparaat met de volgende metingen, apparaateigenschappen, instellingen en -opdrachten:

### <a name="telemetry-measurements"></a>Telemetrie-metingen

Voeg de volgende telemetrie toe op de **metingen** pagina:

| Weergavenaam | Veldnaam  | Eenheden | Min. | Max. | Aantal decimalen |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Temperatuur  | temperatuur | F     | 60  | 110 | 0              |
| Vochtigheid     | vochtigheid    | %     | 0   | 100 | 0              |
| Druk     | pressure    | kPa   | 80  | 110 | 0              |

> [!NOTE]
> Het gegevenstype van de meting telemetrie is een drijvende-kommagetal zijn.

Veldnamen precies zoals weergegeven in de tabel in de sjabloon voor het apparaat invoeren. Als de veldnamen niet overeenkomt met de namen van eigenschappen in de bijbehorende apparaatcode, kan de telemetrie kan niet worden weergegeven in de toepassing.

### <a name="state-measurements"></a>Status metingen

Voeg de volgende status hebben op de **metingen** pagina:

| Weergavenaam | Veldnaam  | Waarde 1 | Weergavenaam | Waarde 2 | Weergavenaam |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Ventilatormodus     | ventilatormodus     | 1       | In uitvoering      | 0       | Gestopt      |

> [!NOTE]
> Het gegevenstype van de meting van de status is een tekenreeks.

Veldnamen precies zoals weergegeven in de tabel in de sjabloon voor het apparaat invoeren. Als de veldnamen niet overeenkomt met de namen van eigenschappen in de bijbehorende apparaatcode, wordt de status niet weergeven in de toepassing.

### <a name="event-measurements"></a>Gebeurtenis-metingen

De volgende gebeurtenis toevoegen aan de **metingen** pagina:

| Weergavenaam | Veldnaam  | Severity |
| ------------ | ----------- | -------- |
| Oververhitting  | overheat    | Fout    |

> [!NOTE]
> Het gegevenstype van de meting van de gebeurtenis is een tekenreeks.

### <a name="device-properties"></a>Apparaateigenschappen

Voeg de volgende apparaateigenschappen toe op de **eigenschappen** pagina:

| Weergavenaam        | Veldnaam        | Gegevenstype |
| ------------------- | ----------------- | --------- |
| Serienummer       | Serienummer      | tekst      |
| De fabrikant van apparaat | fabrikant      | tekst      |

Voer de veldnamen precies zoals weergegeven in de tabel in de sjabloon voor het apparaat. Als de veldnamen niet overeenkomt met de namen van eigenschappen in de bijbehorende apparaatcode, kan de eigenschappen kunnen niet worden weergegeven in de toepassing.

### <a name="settings"></a>Instellingen

Voeg de volgende **getal** instellingen op de **instellingen** pagina:

| Weergavenaam    | Veldnaam     | Eenheden | Aantal decimalen | Min. | Max.  | Oorspronkelijk |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Snelheid van ventilator       | fanSpeed       | rpm   | 0        | 0   | 3000 | 0       |
| Temperatuur instellen | temperatuurInstellen | F     | 0        | 20  | 200  | 80      |

Voer veldnaam precies zoals weergegeven in de tabel in de sjabloon voor het apparaat. Als de veldnamen niet overeenkomt met de namen van eigenschappen in de bijbehorende apparaatcode, kan het apparaat kan geen waarde van de instelling ontvangen.

### <a name="commands"></a>Opdrachten

Voeg de volgende opdracht uit op de **opdrachten** pagina:

| Weergavenaam    | Veldnaam     | Standaardtime-out | Gegevenstype |
| --------------- | -------------- | --------------- | --------- |
| Aftelling       | aftelling      | 30              | getal    |

De volgende invoer veld toevoegen aan de aftelling-opdracht:

| Weergavenaam    | Veldnaam     | Gegevenstype | Value |
| --------------- | -------------- | --------- | ----- |
| Geteld vanaf      | countFrom      | getal    | 10    |

Veldnamen precies zoals weergegeven in de tabellen in de sjabloon voor het apparaat invoeren. Als de veldnamen niet overeenkomt met de namen van eigenschappen in de bijbehorende apparaatcode, kan het apparaat de opdracht niet verwerken.

## <a name="add-a-real-device"></a>Echt apparaat toevoegen

Voeg een echt apparaat aan de apparaat-sjabloon die u in de vorige sectie hebt gemaakt in uw Azure IoT Central-toepassing.

Volg de instructies in de zelfstudie 'Een apparaat toevoegen' voor [genereren van een verbindingsreeks voor de echt apparaat](tutorial-add-device.md#generate-connection-string). U kunt deze verbindingsreeks gebruiken in de volgende sectie:

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

    Bijwerken van de tijdelijke aanduiding `{your device connection string}` met de [apparaatverbindingsreeks](tutorial-add-device.md#generate-connection-string). In dit voorbeeld u initialiseren `targetTemperature` op nul, kunt u de huidige lezen van het apparaat of een waarde van het dubbele apparaat.

1. Voor het verzenden van telemetrie, status en metingen van de gebeurtenis voor uw Azure IoT Central-toepassing, de volgende functie toevoegen aan het bestand:

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
    // Send device reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
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

1. Voeg de volgende code voor het afhandelen van een aftelling opdracht verzonden vanaf de IoT Central-toepassing:

    ```javascript
    // Handle countdown command
    function onCountdown(request, response) {
      console.log('Received call to countdown');

      var countFrom = (typeof(request.payload.countFrom) === 'number' && request.payload.countFrom < 100) ? request.payload.countFrom : 10;

      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          client.getTwin((err, twin) => {
            function doCountdown(){
              if ( countFrom >= 0 ) {
                var patch = {
                  countdown:{
                    value: countFrom
                  }
                };
                sendDeviceProperties(twin, patch);
                countFrom--;
                setTimeout(doCountdown, 2000 );
              }
            }

            doCountdown();
          });
        }
      });
    }
    ```

1. Voeg de volgende code toe voor het voltooien van de verbinding met Azure IoT Central en het aansluiten van de functies in de clientcode:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Create handler for countdown command
        client.onDeviceMethod('countdown', onCountdown);

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            var properties = {
              serialNumber: '123-ABC',
              manufacturer: 'Contoso'
            };
            sendDeviceProperties(twin, properties);

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

* Bekijk de eigenschapswaarden van het apparaat verzonden van uw apparaat op de **eigenschappen** pagina. De eigenschap voor tegels bijwerken wanneer het apparaat verbinding maakt:

    ![Eigenschappen van apparaat weergeven](media/howto-connect-nodejs/viewproperties.png)

* Stel de temperatuur ventilator snelheid en het doel van de **instellingen** pagina:

    ![Snelheid van de set-ventilator](media/howto-connect-nodejs/setfanspeed.png)

* Aanroepen van de opdracht aftelling vanaf de **opdrachten** pagina:

    ![Aanroepopdracht aftelling](media/howto-connect-nodejs/callcountdown.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een algemene Node.js-client verbinden met uw Azure IoT Central-toepassing, de voorgestelde volgende stap is te leren hoe u [voorbereiden en verbinding maken met een Raspberry Pi](howto-connect-raspberry-pi-python.md).

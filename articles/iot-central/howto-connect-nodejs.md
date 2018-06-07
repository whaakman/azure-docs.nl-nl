---
title: Verbinding maken met een algemene Node.js-clienttoepassing op Azure IoT centrale | Microsoft Docs
description: Als een ontwikkelaar van het apparaat, hoe u een algemene Node.js-apparaat aansluit op uw Azure IoT centrale toepassing.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 42ede975f2cfde2d9c0a61d15ba1af412a88c556
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628535"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Verbinding maken met een algemene clienttoepassing naar uw Azure IoT centrale (Node.js)

Dit artikel wordt beschreven hoe u, als een ontwikkelaar apparaat verbinding maken van een algemene Node.js-toepassing die een fysiek apparaat aan uw Microsoft Azure IoT centrale toepassing vertegenwoordigt.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

1. Een Azure IoT centrale toepassing. Zie voor meer informatie [maken van uw Azure IoT centrale toepassing](howto-create-application.md).
1. Een ontwikkelcomputer met [Node.js](https://nodejs.org/) versie 4.0.0 of hoger is geïnstalleerd. U kunt uitvoeren `node --version` op de opdrachtregel om te controleren van uw versie. Node.js is beschikbaar voor een groot aantal verschillende besturingssystemen.

In de Azure IoT centrale toepassing moet u een sjabloon van het apparaat met de volgende metingen en apparaateigenschappen gedefinieerd:

### <a name="telemetry-measurements"></a>Telemetrie metingen

Voeg de volgende telemetrie in de **metingen** pagina:

| Weergavenaam | Veldnaam  | Eenheden | Min. | Max. | Decimalen |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Temperatuur  | temperatuur | F     | 60  | 110 | 0              |
| Vochtigheid     | Vochtigheid    | %     | 0   | 100 | 0              |
| Druk     | pressure    | kPa   | 80  | 110 | 0              |

> [!NOTE]
  Het gegevenstype van de meting telemetrie is dubbel.

Voer veldnamen precies zoals weergegeven in de tabel in de sjabloon voor het apparaat. Als de veldnamen niet overeenkomen, kan de telemetrie kan niet worden weergegeven in de toepassing.

### <a name="state-measurements"></a>Status metingen

Voeg de volgende status in de **metingen** pagina:

| Weergavenaam | Veldnaam  | Waarde 1 | Weergavenaam | Waarde 2 | Weergavenaam |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Ventilatormodus     | ventilatormodus     | 1       | In uitvoering      | 0       | Gestopt      |

> [!NOTE]
  Het gegevenstype van de meting van de status is een tekenreeks.

Voer veldnamen precies zoals weergegeven in de tabel in de sjabloon voor het apparaat. Als de veldnamen niet overeenkomen, kan de status kan niet worden weergegeven in de toepassing.

### <a name="event-measurements"></a>Gebeurtenis metingen

Voeg de volgende gebeurtenis in de **metingen** pagina:

| Weergavenaam | Veldnaam  | Severity |
| ------------ | ----------- | -------- |
| Oververhitting  | oververhit    | Fout    |

> [!NOTE]
  Het gegevenstype van de meting van de gebeurtenis is een tekenreeks.

### <a name="device-properties"></a>Apparaateigenschappen

Voeg de volgende eigenschappen van de apparaten in de **eigenschappenpagina**:

| Weergavenaam        | Veldnaam        | Gegevenstype |
| ------------------- | ----------------- | --------- |
| Serienummer       | Serienummer      | tekst      |
| De fabrikant van apparaat | fabrikant      | tekst      |

Voer de veldnamen precies zoals weergegeven in de tabel in de sjabloon voor het apparaat. Als de veldnamen niet overeenkomen, kan de toepassing niet de waarde van eigenschap weergeven.

### <a name="settings"></a>Instellingen

Voeg de volgende **getal** instellingen in de **instellingenpagina**:

| Weergavenaam    | Veldnaam     | Eenheden | Decimalen | Min. | Max.  | Oorspronkelijk |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Snelheid van ventilator       | fanSpeed       | rpm   | 0        | 0   | 3000 | 0       |
| Temperatuur instellen | temperatuurInstellen | F     | 0        | 20  | 200  | 80      |

Geef veldnaam precies zoals weergegeven in de tabel in de sjabloon voor het apparaat. Als de veldnamen niet overeenkomen, kan het apparaat geen waarde van de instelling ontvangen.

### <a name="add-a-real-device"></a>Echt apparaat toevoegen

In uw Azure IoT centrale toepassing, voegt u een echte apparaat van de sjabloon van het apparaat u maken en noteer de verbindingsreeks van het apparaat. Zie voor meer informatie [een echte apparaat toevoegt aan uw Azure IoT centrale toepassing](tutorial-add-device.md)

## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken

De volgende stappen laten zien hoe een clienttoepassing maken die het echte apparaat dat u hebt toegevoegd aan de toepassing implementeert.

1. Maak de map `connected-air-conditioner-adv` op uw computer. Navigeer naar de map in uw omgeving vanaf de opdrachtregel.

1. Als u wilt initialiseren uw Node.js-project, voer de volgende opdrachten:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Maken van een bestand met de naam **connectedAirConditionerAdv.js** in de `connected-air-conditioner-adv` map.

1. Voeg de volgende `require` instructies aan het begin van de **connectedAirConditionerAdv.js** bestand:

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

    Bijwerken van de tijdelijke aanduiding voor `{your device connection string}` met de verbindingsreeks van uw apparaat. U hebt deze waarde gekopieerd uit de detailpagina verbinding wanneer u uw apparaat echte toegevoegd. In dit voorbeeld wordt geïnitialiseerd `targetTemperature` aan nul, u kunt eventueel nemen de huidige lezen van het apparaat of de waarde van het apparaat twin. 

1. Voor het verzenden van telemetrie, status en gebeurtenis metingen aan uw Azure IoT centrale toepassing, aan het bestand met toevoegen van de volgende functie:

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

    1. Toevoegen om de eigenschappen van een apparaat verzendt naar uw Azure IoT centrale toepassing, de volgende functie aan het bestand:

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

1. Toevoegen om de instellingen voor die het apparaat reageert op definieert, de volgende definitie:

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

1. Voeg de volgende naar het bestand voor het afhandelen van de bijgewerkte instellingen van uw Azure IoT centrale toepassing:

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

1. Voeg de volgende voor het voltooien van de verbinding met Azure IoT centrale en de functies in de clientcode aansluiten:

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

## <a name="run-your-nodejs-application"></a>Voer uw Node.js-toepassing

Voer de volgende opdracht in uw omgeving opdrachtregel:

```cmd/sh
node connectedAirConditionerAdv.js
```

Als operator in uw Azure IoT centrale toepassing voor het echte apparaat kunt u:

* De telemetrie weergeven op de **metingen** pagina:

    ![Telemetrie bekijken](media/howto-connect-nodejs/viewtelemetry.png)

* De apparaat-eigenschapswaarden op van het apparaat verzonden weergeven de **eigenschappen** pagina.

    ![Apparaateigenschappen weergeven](media/howto-connect-nodejs/viewproperties.png)

* Stel de temperatuur ventilator snelheid en doel van de **instellingen** pagina.

    ![Snelheid van de set-ventilator](media/howto-connect-nodejs/setfanspeed.png)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een algemene Node.js-client verbindt met uw Azure IoT centrale toepassing, vindt hier u de voorgestelde volgende stappen uit:
* [Raspberry Pi voorbereiden en verbinden](howto-connect-raspberry-pi-python.md)
<!-- Next how-tos in the sequence -->

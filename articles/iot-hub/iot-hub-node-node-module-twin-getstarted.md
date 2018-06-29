---
title: Aan de slag met Azure IoT Hub module identiteits- en module twin (Node.js) | Microsoft Docs
description: Informatie over het module-id maken en bijwerken van de module twin met IoT SDK's voor Node.js.
author: chrissie926
manager: ''
ms.service: iot-hub
services: iot-hub
ms.devlang: node
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: menchi
ms.openlocfilehash: e1fcca7f37f928c488b1111d6d3299cb03c73cc1
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036183"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-nodejs-back-end-and-nodejs-device"></a>Aan de slag met IoT Hub module identiteits- en module twin met behulp van Node.js-back-end en Node.js-apparaat

> [!NOTE]
> [Module-id's en moduledubbels](iot-hub-devguide-module-twins.md) zijn vergelijkbaar met Azure IoT Hub-apparaat-id's en apparaatdubbels, maar bieden een hogere granulariteit. Met een Azure IoT Hub-apparaat-id en apparaatdubbel kan een apparaat worden geconfigureerd via de back-endtoepassing en kunt u inzicht krijgen in de toestand van een apparaat. Een module-id en moduledubbel bieden deze mogelijkheden voor afzonderlijke onderdelen van een apparaat. Op apparaten met meerdere onderdelen, zoals apparaten met een besturingssysteem of firmware, kunt u afzonderlijke configuraties en voorwaarden voor elk onderdeel instellen.

Aan het einde van deze zelfstudie hebt u twee Node.js-apps:

* **CreateIdentities**: deze toepassing maakt een apparaat-id, een module-id en de bijbehorende beveiligingssleutel waarmee uw apparaat- en moduleclients verbonden kunnen worden.
* **UpdateModuleTwinReportedProperties**: deze toepassing verzendt bijgewerkte gerapporteerde eigenschappen van de moduledubbel naar uw IoT Hub.

> [!NOTE]
> Raadpleeg het artikel [Azure IoT-SDKs][lnk-hub-sdks] voor meer informatie over de verschillende Azure IoT-SDK's die u kunt gebruiken om beide toepassingen zo te maken dat ze zowel op het apparaat als op de back-end van uw oplossing kunnen worden uitgevoerd.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)
* Een IoT-Hub.
* Installeer de meest recente [Node.js SDK](https://github.com/Azure/azure-iot-sdk-node).


U hebt nu uw IoT Hub gemaakt en u hebt de hostnaam en de IoT Hub-verbindingsreeks die u nodig hebt voor de rest van deze handleiding.

<a id="DeviceIdentity_csharp"></a>
## <a name="create-a-device-identity-and-a-module-identity-in-iot-hub"></a>Een apparaat-id en de identiteit van een module in IoT Hub maken

In deze sectie maakt u een Node.js-app die u een apparaat-id en de identiteit van een module in het identiteitenregister van uw IoT-hub maakt. Een apparaat of module kan alleen verbinding maken met de IoT-hub als het apparaat of de module is vermeld in het identiteitsregister. Zie de sectie Id-register in de [ontwikkelaarshandleiding voor IoT Hub][lnk-devguide-identity] voor meer informatie. Als u deze consoletoepassing uitvoert, worden er een unieke id en een unieke sleutel gemaakt voor zowel het apparaat als de module. Deze waarden worden door het apparaat en de module gebruikt om zichzelf te identificeren bij het verzenden van apparaat-naar-cloud-berichten naar IoT Hub. De id's zijn hoofdlettergevoelig.

1.  Maak een map voor het opslaan van uw code.
2. Binnen die map voor het eerst uitvoert **npm init -y** voor het maken van een leeg package.json met standaardwaarden. Dit is het projectbestand voor uw code.
3. Voer **npm -S installeren azure-iothub@modules-preview**  voor het installeren van de SDK-service binnen de **node_modules** submap. 

    > [!NOTE] 
    > De naam van de submap node_modules maakt gebruik van de module word betekent 'een bibliotheek van knooppunt'. De term hier heeft niets te maken met IoT Hub-modules.

4. Het volgende .js-bestand maken in uw directory. Noem het **add.js**. Kopieer en plak uw hub verbindingsreeks en hubnaam.

    ```javascript
    var Registry = require('azure-iothub').Registry;
    var uuid = require('uuid');
    // Copy/paste your connection string and hub name here
    var serviceConnectionString = '<hub connection string from portal>';
    var hubName = '<hub name>.azure-devices.net';
    // Create an instance of the IoTHub registry
    var registry = Registry.fromConnectionString(serviceConnectionString);
    // Insert your device ID and moduleId here.
    var deviceId = 'myFirstDevice';
    var moduleId = 'myFirstModule';
    // Create your device as a SAS authentication device
    var primaryKey = new Buffer(uuid.v4()).toString('base64');
    var secondaryKey = new Buffer(uuid.v4()).toString('base64');
    var deviceDescription = {
      deviceId: deviceId,
      status: 'enabled',
      authentication: {
        type: 'sas',
        symmetricKey: {
          primaryKey: primaryKey,
          secondaryKey: secondaryKey
        }
      }
    };

    // First, create a device identity
    registry.create(deviceDescription, function(err) {
      if (err) {
        console.log('Error creating device identity: ' + err);
        process.exit(1);
      }
      console.log('device connection string = "HostName=' + hubName + ';DeviceId=' + deviceId + ';SharedAccessKey=' + primaryKey + '"');

    // Then add a module to that device
      registry.addModule({ deviceId: deviceId, moduleId: moduleId }, function(err) {
        if (err) {
          console.log('Error creating module identity: ' + err);
          process.exit(1);
        }

    // Finally, retrieve the module details from the hub so we can construct the connection string
        registry.getModule(deviceId, moduleId, function(err, foundModule) {
          if (err) {
            console.log('Error getting module back from hub: ' + err);
            process.exit(1);
          }
          console.log('module connection string = "HostName=' + hubName + ';DeviceId=' + foundModule.deviceId + ';ModuleId='+foundModule.moduleId+';SharedAccessKey=' + foundModule.authentication.symmetricKey.primaryKey + '"');
          process.exit(0);
        });
      });
    });

    ```

Deze app maakt een apparaat-id met de ID **myFirstDevice** en de identiteit van een module met ID **myFirstModule** onder apparaat **myFirstDevice**. (Als deze module-id al in het id-register staat, haalt de code gewoon de bestaande modulegegevens op.) De app geeft vervolgens de primaire sleutel voor die identiteit weer. U gebruikt deze sleutel in de gesimuleerde module-app om verbinding te maken met uw IoT-hub.

5. Deze uitvoeren met behulp van knooppunt add.js. Deze krijgt u een verbindingsreeks voor uw apparaat-id en een andere naam voor de identiteit van de module.

    > [!NOTE]
    > In het identiteitsregister van IoT Hub worden alleen apparaat- en module-id's opgeslagen waarmee veilig toegang tot de IoT-hub kan worden verkregen. In het identiteitsregister worden apparaat-id's en -sleutels opgeslagen die als beveiligingsreferenties worden gebruikt. In het identiteitsregister wordt ook een vlag ingeschakeld/uitgeschakeld voor elk apparaat opgeslagen die u kunt gebruiken om de toegang tot dat apparaat uit te schakelen. Als uw toepassing andere apparaatspecifieke metagegevens moet opslaan, moet deze een toepassingsspecifieke opslagmethode gebruiken. Er is geen vlag voor ingeschakeld/uitgeschakeld voor module-id's. Zie de [ontwikkelaarshandleiding voor IoT Hub][lnk-devguide-identity] voor meer informatie.


<a id="D2C_csharp"></a>
## <a name="update-the-module-twin-using-nodejs-device-sdk"></a>De module-twin SDK voor Node.js-apparaat bijwerken

In deze sectie maakt u een Node.js-app op uw gesimuleerde apparaat die updates van de module-twin eigenschappen gerapporteerd.

1. **Haal nu de moduleverbindingsreeks op** als u zich aanmeldt bij [Azure Portal][lnk-portal]. Navigeer naar uw IoT Hub en klik op IoT-apparaten. Zoek naar MyFirstDevice, open het apparaat en u ziet dat myFirstModule is gemaakt. Kopieer de moduleverbindingsreeks. Deze is vereist voor de volgende stap.

    ![Details van de Azure Portal-module][15]

2. Vergelijkbaar met die u hebt in de bovenstaande stap, maakt u een map voor uw apparaatcode en NPM gebruiken voor het initialiseren en het apparaat-SDK installeren (**npm -S installeren azure-iot-device-amqp@modules-preview** ).

    > [!NOTE]
    > De npm-installatieopdracht denken wellicht traag. Worden geduld, wordt deze binnenhalen omlaag veel code uit de opslagplaats van het pakket.

    > [!NOTE] 
    > Als er een foutbericht dat aangeeft npm ERR! register fout bij het parseren json, dit is het veilig negeren. Als er een foutbericht dat aangeeft npm ERR! register fout bij het parseren json, dit is het veilig negeren.

3. Maak een bestand twin.js aangeroepen. Kopieer en plak uw module-id-reeks.

    ```javascript
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-amqp').Amqp;
    // Copy/paste your module connection string here.
    var connectionString = '<insert module connection string here>';
    // Create a client using the Amqp protocol.
    var client = Client.fromConnectionString(connectionString, Protocol);
    client.on('error', function (err) {
      console.error(err.message);
    });
    // connect to the hub
    client.open(function(err) {
      if (err) {
        console.error('error connecting to hub: ' + err);
        process.exit(1);
      }
      console.log('client opened');
    // Create device Twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('error getting twin: ' + err);
          process.exit(1);
        }
    // Output the current properties
        console.log('twin contents:');
        console.log(twin.properties);
    // Add a handler for desired property changes
        twin.on('properties.desired', function(delta) {
            console.log('new desired properties received:');
            console.log(JSON.stringify(delta));
        });
    // create a patch to send to the hub
        var patch = {
          updateTime: new Date().toString(),
          firmwareVersion:'1.2.1',
          weather:{
            temperature: 72,
            humidity: 17
          }
        };
    // send the patch
        twin.properties.reported.update(patch, function(err) {
          if (err) throw err;
          console.log('twin state reported');
        });
      });
    });
    ```

2. Voer nu dit met de opdracht **knooppunt twin.js**.

    ```
    F:\temp\module_twin>node twin.js
    client opened
    twin contents:
    { reported: { update: [Function: update], '$version': 1 },
      desired: { '$version': 1 } }
    new desired properties received:
    {"$version":1}
    twin state reported
    ```

## <a name="next-steps"></a>Volgende stappen

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Aan de slag met apparaatbeheer][lnk-device-management]
* [Aan de slag met IoT Edge][lnk-iot-edge]


<!-- Images. -->
[15]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/module-detail.JPG
<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
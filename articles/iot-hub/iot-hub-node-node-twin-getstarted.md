---
title: Aan de slag met Azure IoT Hub apparaat horende (knooppunt) | Microsoft Docs
description: Het gebruik van Azure IoT Hub apparaat horende labels toevoegen en vervolgens met de query voor een IoT Hub. U de Azure IoT SDK's voor Node.js gebruiken voor het implementeren van de gesimuleerde apparaattoepassing en een service-app die de labels worden toegevoegd en de IoT Hub-query wordt uitgevoerd.
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: 314c88e4-cce1-441c-b75a-d2e08e39ae7d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: elioda
ms.openlocfilehash: df49f054b5eb26c3d68f088bc05f5209cf2ebccf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-device-twins-node"></a>Aan de slag met apparaat horende (knooppunt)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Aan het einde van deze zelfstudie hebt u twee console Node.js-apps:

* **AddTagsAndQuery.js**, een back-end-app van Node.js, dat labels toegevoegd en wordt opgevraagd horende apparaten.
* **TwinSimulatedDevice.js**, een Node.js-app, dat een apparaat simuleert dat verbinding met uw IoT-hub aan de apparaat-id eerder hebt gemaakt maakt, en rapporteert de voorwaarde van de verbinding.

> [!NOTE]
> Het artikel [Azure IoT SDK's] [ lnk-hub-sdks] bevat informatie over de Azure IoT SDK's dat u gebruiken kunt om zowel apparaatgegevens als back-end-apps te bouwen.
> 
> 

Voor deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-service-app"></a>De service-app maken
In deze sectie maakt u een Node.js-consoletoepassing waarmee locatie metagegevens wordt toegevoegd aan de apparaat-twin gekoppeld **myDeviceId**. Deze vervolgens het apparaat horende opgeslagen in de IoT-hub te selecteren van de apparaten die zich in de Verenigde Staten en de waarden die rapporteren een mobiele verbinding een query.

1. Maak een nieuwe lege map genaamd **addtagsandqueryapp**. In de **addtagsandqueryapp** map, maak een nieuw package.json-bestand met de volgende opdracht achter de opdrachtprompt. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **addtagsandqueryapp** map, voer de volgende opdracht voor het installeren van de **azure-iothub** pakket:
   
    ```
    npm install azure-iothub --save
    ```
3. Start een teksteditor en maak een nieuwe **AddTagsAndQuery.js** bestand de **addtagsandqueryapp** map.
4. Voeg de volgende code naar de **AddTagsAndQuery.js** -bestand en vervang de **{iot hub verbindingsreeks}** aanduiding voor items met de IoT Hub-verbindingsreeks die u tijdens het maken van uw hub hebt gekopieerd:
   
        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
   
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });
   
    De **register** object bevat de methoden die zijn vereist voor interactie met horende apparaten van de service. De vorige code eerst initialiseert de **register** object en vervolgens haalt de apparaat-twin voor **myDeviceId**, en ten slotte de labels bijgewerkt met informatie over de gewenste locatie.
   
    Na het bijwerken van de code roept de **queryTwins** functie.
5. Voeg de volgende code toe aan het einde van **AddTagsAndQuery.js** voor het implementeren van de **queryTwins** functie:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
   
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };
   
    De vorige code twee query's uitvoert: de eerste selecteert alleen de apparaat-horende apparaten zich in de **Redmond43** plant en de tweede verfijning de query voor het selecteren van alleen de apparaten die ook via het mobiele netwerk zijn verbonden.
   
    De vorige code bij het maken van de **query** object, geeft u een maximum aantal geretourneerde documenten. De **query** object bevat een **hasMoreResults** Boole-eigenschap die u gebruiken kunt om aan te roepen de **nextAsTwin** methoden meerdere keren voor het ophalen van alle resultaten. Een methode aangeroepen **volgende** is beschikbaar voor de resultaten die bijvoorbeeld niet apparaat horende zijn resultaten van query's voor aggregatie.
6. Voer de toepassing met:
   
        node AddTagsAndQuery.js
   
    U ziet één apparaat in de resultaten van de query wordt gevraagd voor alle apparaten vinden in **Redmond43** en er is geen voor de query die de resultaten beperkt tot apparaten die gebruikmaken van een mobiel netwerk.
   
    ![][1]

In de volgende sectie maakt u een apparaat-app die rapporten de informatie over de connectiviteit en het resultaat van de query in de vorige sectie wordt gewijzigd.

## <a name="create-the-device-app"></a>De apparaat-app maken
In deze sectie maakt u een Node.js-consoletoepassing die is verbonden met uw hub als **myDeviceId**, en vervolgens de updates die de apparaat-twin de eigenschappen van de informatie dat is verbonden met een mobiel netwerk bevatten gerapporteerd.


1. Maak een nieuwe lege map genaamd **reportconnectivity**. In de **reportconnectivity** map, maak een nieuw package.json-bestand met de volgende opdracht achter de opdrachtprompt. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **reportconnectivity** map, voer de volgende opdracht voor het installeren van de **azure-iot-device**, en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Start een teksteditor en maak een nieuwe **ReportConnectivity.js** bestand de **reportconnectivity** map.
4. Voeg de volgende code naar de **ReportConnectivity.js** -bestand en vervang de **{verbindingsreeks apparaat}** aanduiding voor items met de apparaat-verbindingsreeks die u tijdens het maken van de gekopieerd**myDeviceId** apparaat-id:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
   
    De **Client** object bevat de methoden die u nodig hebt om te communiceren met horende apparaten van het apparaat. De vorige code nadat het initialiseren van de **Client** object, haalt de apparaat-twin voor **myDeviceId** en de bijbehorende eigenschap gerapporteerde verbindingsgegevens bijgewerkt door de.
5. De apparaat-app uitvoeren
   
        node ReportConnectivity.js
   
    U ziet het bericht `twin state reported`.
6. Nu dat het apparaat heeft gemeld dat de gegevens over de connectiviteit, moet deze worden weergegeven in beide query's. Ga terug in de **addtagsandqueryapp** map en voer de query opnieuw uit:
   
        node AddTagsAndQuery.js
   
    Deze tijd **myDeviceId** moet worden weergegeven in beide queryresultaten.
   
    ![][3]

## <a name="next-steps"></a>Volgende stappen
In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U metagegevens van apparaten als labels toegevoegd vanuit een back-end-app en een gesimuleerde apparaattoepassing geschreven naar apparaten connectiviteit rapportgegevens in de apparaat-twin. Ook hebt u geleerd hoe deze gegevens met behulp van de SQL-achtige IoT Hub-querytaal opvragen.

Gebruik de volgende bronnen voor meer informatie over hoe:

* verzenden van telemetrie vanaf apparaten met de [aan de slag met IoT Hub] [ lnk-iothub-getstarted] zelfstudie
* apparaten configureren met de gewenste eigenschappen van apparaat twin met de [gebruik gewenst eigenschappen voor het configureren van apparaten] [ lnk-twin-how-to-configure] zelfstudie
* beheren van apparaten interactief (zoals het inschakelen van een ventilator van een gebruiker beheerde app), met de [direct methoden gebruiken] [ lnk-methods-tutorial] zelfstudie.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

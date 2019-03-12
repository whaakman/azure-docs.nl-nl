---
title: Aan de slag met Azure IoT Hub-apparaatdubbels (Node) | Microsoft Docs
description: Het gebruik van Azure IoT Hub-apparaatdubbels tags toevoegen en vervolgens een IoT Hub-query. U kunt de Azure IoT SDK's voor Node.js gebruiken voor het implementeren van het gesimuleerde apparaat-app en een service-app die wordt toegevoegd de labels en de IoT Hub-query wordt uitgevoerd.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 08/25/2017
ms.author: elioda
ms.openlocfilehash: 397f4f48437b48934171cc0b90ad04715f2ee21b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57541261"
---
# <a name="get-started-with-device-twins-node"></a>Aan de slag met apparaatdubbels (Node)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Aan het einde van deze zelfstudie hebt u twee Node.js-consoletoepassingen:

* **AddTagsAndQuery.js**, een Node.js back-end-app, die wordt toegevoegd tags en apparaatdubbels-query's.
* **TwinSimulatedDevice.js**, een Node.js-app, die een apparaat simuleert dat verbinding met uw IoT-hub aan de apparaat-id die eerder hebt gemaakt maakt, en rapporten van de voorwaarde van de verbinding.

> [!NOTE]
> Het artikel [Azure IoT SDK's] [ lnk-hub-sdks] bevat informatie over de Azure IoT SDK's die u gebruiken kunt om apparaat- en back-end-apps te bouwen.
> 
> 

Voor deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Verbindingsreeks voor IoT-hub ophalen

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-service-app"></a>De service-app maken
In deze sectie maakt u een Node.js-consoletoepassing die metagegevens van de locatie toegevoegd aan het dubbele apparaat dat is gekoppeld aan **myDeviceId**. Deze query vervolgens de dubbele apparaten die zijn opgeslagen in de IoT-hub selecteren van de apparaten die zich in de Verenigde Staten en die een mobiele verbinding wilt rapporteren.

1. Maak een nieuwe lege map genaamd **addtagsandqueryapp**. In de **addtagsandqueryapp** map, maak een nieuw package.json-bestand met de volgende opdracht in uw opdrachtvenster. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. In het opdrachtprompt in de **addtagsandqueryapp** map, voer de volgende opdracht voor het installeren van de **azure-iothub** pakket:
   
    ```
    npm install azure-iothub --save
    ```
3. Maak met een teksteditor een nieuw **AddTagsAndQuery.js** -bestand in de **addtagsandqueryapp** map.
4. Voeg de volgende code aan de **AddTagsAndQuery.js** -bestand en vervang de **{iot hub-verbindingsreeks}** tijdelijke aanduiding door de IoT Hub-verbindingsreeks die u hebt gekopieerd tijdens het maken van uw hub:
   
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
   
    De **register** object bevat de methoden die zijn vereist om te communiceren met de dubbele apparaten uit de service. De vorige code eerst initialiseert de **register** object en vervolgens haalt de apparaatdubbel voor **myDeviceId**, en ten slotte de tags bijgewerkt met informatie over de gewenste locatie.
   
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
   
    De vorige code twee query's uitvoert: de eerste selecteert alleen de apparaatdubbels van apparaten die zich in de **Redmond43** plant en het tweede verfijning van de query voor het selecteren van alleen de apparaten die ook zijn verbonden via mobiel netwerk.
   
    De vorige code bij het maken van de **query** object, geeft u een maximum aantal geretourneerde documenten. De **query** -object bevat een **hasMoreResults** Booleaanse eigenschap die u gebruiken kunt om aan te roepen de **nextAsTwin** methoden meerdere keren om op te halen van alle resultaten. Een methode aangeroepen **volgende** is beschikbaar voor de resultaten die niet de dubbele apparaten, bijvoorbeeld: resultaten van aggregatie-query's.
6. Voer de toepassing met:
   
        node AddTagsAndQuery.js
   
    U ziet één apparaat in de resultaten voor het stellen van de query voor alle apparaten in **Redmond43** en niets in de query die de resultaten beperkt tot apparaten die gebruikmaken van een mobiel netwerk.
   
    ![][1]

In de volgende sectie maakt u een apparaat-app die de gegevens van hostconnectiviteit rapporten en het resultaat van de query in de vorige sectie wordt gewijzigd.

## <a name="create-the-device-app"></a>De apparaat-app maken
In deze sectie maakt u een Node.js-consoletoepassing die verbinding met uw hub als maakt **myDeviceId**, en vervolgens de updates die de apparaatdubbel de gerapporteerde eigenschappen bevat de informatie die is verbonden met een mobiel netwerk.


1. Maak een nieuwe lege map genaamd **reportconnectivity**. In de **reportconnectivity** map, maak een nieuw package.json-bestand met de volgende opdracht in uw opdrachtvenster. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. In het opdrachtprompt in de **reportconnectivity** map, voer de volgende opdracht voor het installeren van de **azure-iot-device**, en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Maak met een teksteditor een nieuw **ReportConnectivity.js** -bestand in de **reportconnectivity** map.
4. Voeg de volgende code aan de **ReportConnectivity.js** -bestand en vervang de **{apparaatverbindingsreeks}** tijdelijke aanduiding door de verbindingsreeks die u hebt gekopieerd tijdens het maken van de **myDeviceId** apparaat-id:
   
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
   
    De **Client** object bevat de methoden die u nodig hebt om te communiceren met de dubbele apparaten van het apparaat. De vorige code, nadat het initialiseren van de **Client** object, haalt de apparaatdubbel voor **myDeviceId** en updates van de gerapporteerde eigenschap met de gegevens van hostconnectiviteit.
5. De apparaat-app uitvoeren
   
        node ReportConnectivity.js
   
    Als het goed is, ziet u nu het bericht `twin state reported`.
6. Nu dat het apparaat heeft gemeld dat de gegevens over de connectiviteit, wordt deze weergegeven in beide query's. Ga terug in de **addtagsandqueryapp** map en voer de query opnieuw uit:
   
        node AddTagsAndQuery.js
   
    Deze keer **myDeviceId** moet worden weergegeven in de resultaten van beide query.
   
    ![][3]

## <a name="next-steps"></a>Volgende stappen
In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U metagegevens van apparaten als labels toegevoegd vanuit een back-end-app en een gesimuleerde apparaat-app geschreven naar apparaatgegevens met connectiviteit van rapport in de apparaatdubbel. U hebt ook geleerd hoe u query's van deze informatie met behulp van de SQL-achtige IoT Hub-querytaal.

Gebruik de volgende bronnen voor meer informatie over het:

* verzenden van telemetrie van apparaten met de [aan de slag met IoT Hub] [ lnk-iothub-getstarted] zelfstudie
* apparaten configureren met de gewenste eigenschappen van het dubbele apparaat met de [gebruik gewenste eigenschappen om apparaten te configureren] [ lnk-twin-how-to-configure] zelfstudie
* Beheren van apparaten interactief (zoals het inschakelen van een fan, van een gebruiker beheerde app), met de [directe methoden gebruiken] [ lnk-methods-tutorial] zelfstudie.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: quickstart-send-telemetry-node.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/quickstart-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: tutorial-device-twins.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-methods-tutorial]: quickstart-control-device-node.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

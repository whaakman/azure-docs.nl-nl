---
title: Gebruik Azure IoT Hub apparaat twin eigenschappen (knooppunt) | Microsoft Docs
description: Het gebruik van Azure IoT Hub apparaat horende apparaten configureren. U de Azure IoT SDK's voor Node.js gebruiken voor het implementeren van een gesimuleerde apparaattoepassing en een service-app die de apparaatconfiguratie van een met behulp van een apparaat-twin wijzigt.
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: elioda
ms.openlocfilehash: 6ff6f1c331d5a77e7ac0a47af6806f5d90fb0fdc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-desired-properties-to-configure-devices-node"></a>Gewenste eigenschappen gebruiken voor het configureren van apparaten (knooppunt)
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Aan het einde van deze zelfstudie hebt u twee console Node.js-apps:

* **SimulateDeviceConfiguration.js**, een gesimuleerde apparaattoepassing die wordt gewacht op een gewenste configuratie-update en rapporteert de status van een gesimuleerde configuratie updateproces.
* **SetDesiredConfigurationAndQuery.js**, een Node.js back-end-app, die de gewenste configuratie ingesteld op een apparaat en het configuratieproces van de update-query's.

> [!NOTE]
> Het artikel [Azure IoT SDK's] [ lnk-hub-sdks] bevat informatie over de Azure IoT SDK's dat u gebruiken kunt om zowel apparaatgegevens als back-end-apps te bouwen.
> 
> 

Voor deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

Als u hebt gevolgd de [aan de slag met apparaat horende] [ lnk-twin-tutorial] zelfstudie, u hebt al een IoT-hub en een apparaat-id genoemd **myDeviceId**; en u kunt doorgaan met de [de gesimuleerde apparaattoepassing maken] [ lnk-how-to-configure-createapp] sectie.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-simulated-device-app"></a>De gesimuleerde apparaattoepassing maken
In deze sectie maakt u een Node.js-consoletoepassing die is verbonden met uw hub als **myDeviceId**, wordt gewacht op een gewenste configuratie-update en vervolgens rapporten updates op het updateproces gesimuleerde configuratie.

1. Maak een nieuwe lege map genaamd **simulatedeviceconfiguration**. In de **simulatedeviceconfiguration** map, maak een nieuw package.json-bestand met de volgende opdracht achter de opdrachtprompt. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **simulatedeviceconfiguration** map, voer de volgende opdracht voor het installeren van de **azure-iot-device**, en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Start een teksteditor en maak een nieuwe **SimulateDeviceConfiguration.js** bestand de **simulatedeviceconfiguration** map.
4. Voeg de volgende code naar de **SimulateDeviceConfiguration.js** -bestand en vervang de **{verbindingsreeks apparaat}** tijdelijke aanduiding met de apparaat-verbindingsreeks die u hebt gekopieerd tijdens het maken van de **myDeviceId** apparaat-id:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    De **Client** object bevat de methoden die zijn vereist voor interactie met horende apparaten van het apparaat. De vorige code nadat het initialiseren van de **Client** object, haalt de apparaat-twin voor **myDeviceId**, en koppelt u een handler voor de update op de gewenste eigenschappen. De handler controleert of dat er een wijzigingsaanvraag voor de huidige configuratie door te vergelijken met de configIds is, wordt een methode die de configuratiewijziging begint aanroept.
   
    Houd er rekening mee dat omwille van de eenvoud de vorige code gebruikmaakt van een vastgelegde standaard voor de eerste configuratie. Een echte app zou waarschijnlijk dat de configuratie van een lokale opslag laden.
   
   > [!IMPORTANT]
   > Gewenste eigenschap wijzigingsgebeurtenissen altijd één keer worden verzonden op het apparaatverbinding, Controleer of er een werkelijke wijziging in de eigenschappen van de gewenste is voordat u een actie uitvoert.
   > 
   > 
5. Voeg de volgende methoden voordat de `client.open()` aanroepen:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    De **initConfigChange** methode gemelde eigenschappen van het lokale apparaat twin-object wordt bijgewerkt met de configuratie-update-aanvraag en stelt de status op **in behandeling**, werkt u vervolgens de twin apparaat van de service. Nadat het apparaat twin is bijgewerkt, wordt een langdurige proces dat wordt beëindigd bij de uitvoering van gesimuleerd **completeConfigChange**. Deze methode werkt u het lokale apparaat-twin gemelde eigenschappen van de status is ingesteld op **geslaagd** en verwijderen van de **pendingConfig** object. De apparaat-twin op de service wordt vervolgens bijgewerkt.
   
    Dat het, voor het opslaan van bandbreedte, gerapporteerd eigenschappen zijn bijgewerkt door te geven alleen de eigenschappen te wijzigen (met de naam **patch** in de bovenstaande code), in plaats van het hele document vervangen.
   
   > [!NOTE]
   > Deze zelfstudie wordt een gedrag voor updates voor gelijktijdige configuratie niet simuleren. Bepaalde configuratie-update-processen mogelijk wijzigingen van de doelconfiguratie voor terwijl de update wordt uitgevoerd, anderen mogelijk moet u ze in de wachtrij en anderen met een fout opgetreden weigeren kunnen. Zorg ervoor dat rekening houden met het gewenste gedrag voor uw specifieke configuratie-proces en de juiste logica toevoegen voordat u begint de wijziging in de configuratie.
   > 
   > 
6. Voer de app voor apparaat:
   
        node SimulateDeviceConfiguration.js
   
    U ziet het bericht `retrieved device twin`. De app actief houden.

## <a name="create-the-service-app"></a>De service-app maken
In deze sectie maakt u een Node.js-consoletoepassing die updates de *gewenst eigenschappen* op het apparaat twin gekoppeld **myDeviceId** met een nieuwe telemetrie configuration-object. Vervolgens zoekt de horende apparaat is opgeslagen in de IoT-hub en ziet u het verschil tussen de gewenste en gerapporteerde configuraties van het apparaat.

1. Maak een nieuwe lege map genaamd **setdesiredandqueryapp**. In de **setdesiredandqueryapp** map, maak een nieuw package.json-bestand met de volgende opdracht achter de opdrachtprompt. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **setdesiredandqueryapp** map, voer de volgende opdracht voor het installeren van de **azure-iothub** pakket:
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. Start een teksteditor en maak een nieuwe **SetDesiredAndQuery.js** bestand de **setdesiredandqueryapp** map.
4. Voeg de volgende code naar de **SetDesiredAndQuery.js** -bestand en vervang de **{iot hub verbindingsreeks}** aanduiding voor items met de IoT Hub-verbindingsreeks die u tijdens het maken van uw hub hebt gekopieerd:
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    De **register** object bevat de methoden die zijn vereist voor interactie met horende apparaten van de service. De vorige code nadat het initialiseren van de **register** object, haalt de apparaat-twin voor **myDeviceId**, en de gewenste eigenschappen bijgewerkt met een nieuwe telemetrie configuration-object. Daarna roept deze de **queryTwins** werken gebeurtenis 10 seconden.

    > [!IMPORTANT]
    > Deze toepassing een IoT Hub query elke 10 seconden ter illustratie. Met query's gebruikersgerichte om rapporten te genereren over verschillende apparaten en niet voor het detecteren van wijzigingen. Als uw oplossing realtime meldingen over apparaatgebeurtenissen vereist, gebruikt u [twin meldingen][lnk-twin-notifications].
    > 
    >.

1. Voeg de volgende code precies vóór de `registry.getDeviceTwin()` aanroepen voor het implementeren van de **queryTwins** functie:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    De vorige code query's de horende apparaten opgeslagen in de IoT-hub en de gewenste en gerapporteerde telemetrie-configuraties worden afgedrukt. Raadpleeg de [IoT Hub-querytaal] [ lnk-query] voor informatie over het uitgebreide om rapporten te genereren in al uw apparaten.
2. Met **SimulateDeviceConfiguration.js** wordt uitgevoerd, voert u de toepassing met:
   
        node SetDesiredAndQuery.js 5m
   
    Ziet u de configuratie van de gerapporteerde gewijzigd van **geslaagd** naar **in behandeling** naar **geslaagd** opnieuw met de nieuwe actieve verzenden frequentie van vijf minuten in plaats van 24 uur.
   
   > [!IMPORTANT]
   > Er is een vertraging van maximaal een minuut tussen het apparaat rapport opnieuw en het queryresultaat. Dit is het inschakelen van de query-infrastructuur om te werken op zeer grote schaal. Voor het ophalen van consistente weergaven van een enkel apparaat twin gebruiken de **getDeviceTwin** methode in de **register** klasse.
   > 
   > 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie stelt u een gewenste configuratie als *gewenst eigenschappen* vanuit een back-end-app en een gesimuleerde apparaattoepassing om te detecteren die wijziging en een updateproces van meerdere stappen zijn status als reporting simuleren geschreven *eigenschappen gerapporteerd* aan de apparaat-twin.

Gebruik de volgende bronnen voor meer informatie over hoe:

* verzenden van telemetrie vanaf apparaten met de [aan de slag met IoT Hub] [ lnk-iothub-getstarted] zelfstudie
* schema of bewerkingen uitvoeren op grote sets van apparaten, Zie de [planning en broadcast taken] [ lnk-schedule-jobs] zelfstudie.
* beheren van apparaten interactief (zoals het inschakelen van een ventilator van een gebruiker beheerde app), met de [direct methoden gebruiken] [ lnk-methods-tutorial] zelfstudie.

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app

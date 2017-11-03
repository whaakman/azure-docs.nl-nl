---
title: Gebruik Azure IoT Hub apparaat twin eigenschappen (.NET/knooppunt) | Microsoft Docs
description: Het gebruik van Azure IoT Hub apparaat horende apparaten configureren. U het apparaat met Azure IoT SDK voor Node.js gebruiken voor het implementeren van een gesimuleerde apparaattoepassing en de Azure IoT service SDK voor .NET voor het implementeren van een service-app die de apparaatconfiguratie van een met behulp van een apparaat-twin wijzigt.
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: elioda
ms.openlocfilehash: 8f9626fc47e7d32cb104b960bea9b7de9efa6f3e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-desired-properties-to-configure-devices"></a>Gewenste eigenschappen gebruiken om apparaten te configureren
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Aan het einde van deze zelfstudie hebt u twee console apps:

* **SimulateDeviceConfiguration.js**, een gesimuleerde apparaattoepassing die wordt gewacht op een gewenste configuratie-update en rapporteert de status van een gesimuleerde configuratie updateproces.
* **SetDesiredConfigurationAndQuery**, een .NET back-end-app, die de gewenste configuratie ingesteld op een apparaat en het configuratieproces van de update-query's.

> [!NOTE]
> Het artikel [Azure IoT SDK's] [ lnk-hub-sdks] bevat informatie over de Azure IoT SDK's dat u gebruiken kunt om zowel apparaatgegevens als back-end-apps te bouwen.
> 
> 

Voor deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.

Als u hebt gevolgd de [aan de slag met apparaat horende] [ lnk-twin-tutorial] zelfstudie, u hebt al een IoT-hub en een apparaat-id genoemd **myDeviceId**. In dat geval kunt u doorgaan met de [de gesimuleerde apparaattoepassing maken] [ lnk-how-to-configure-createapp] sectie.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>De gesimuleerde apparaattoepassing maken
In deze sectie maakt u een Node.js-consoletoepassing die is verbonden met uw hub als **myDeviceId**, wordt gewacht op een gewenste configuratie-update en vervolgens rapporten updates op het updateproces gesimuleerde configuratie.

1. Maak een nieuwe lege map genaamd **simulatedeviceconfiguration**. In de **simulatedeviceconfiguration** map, maak een nieuw package.json-bestand met de volgende opdracht achter de opdrachtprompt. Accepteer alle standaardwaarden.
   
    ```
    npm init
    ```
1. Bij de opdrachtprompt in de **simulatedeviceconfiguration** map, voer de volgende opdracht voor het installeren van de **azure-iot-device** en **azure-iot-device-mqtt** pakketten:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. Start een teksteditor en maak een nieuwe **SimulateDeviceConfiguration.js** bestand de **simulatedeviceconfiguration** map.
1. Voeg de volgende code naar de **SimulateDeviceConfiguration.js** -bestand en vervang de **{verbindingsreeks apparaat}** tijdelijke aanduiding met de apparaat-verbindingsreeks die u hebt gekopieerd tijdens het maken van de **myDeviceId** apparaat-id:
   
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
   
    De **Client** object bevat de methoden die zijn vereist voor interactie met horende apparaten van het apparaat. Deze code initialiseert de **Client** object, haalt de apparaat-twin voor **myDeviceId**, en koppelt u een handler voor de update op *gewenst eigenschappen*. De handler controleert of dat er een wijzigingsaanvraag voor de huidige configuratie door te vergelijken met de configIds is, wordt een methode die de configuratiewijziging begint aanroept.
   
    Houd er rekening mee dat omwille van de eenvoud deze code maakt gebruik van een vastgelegde standaard voor de eerste configuratie. Een echte app zou waarschijnlijk dat de configuratie van een lokale opslag laden.
   
   > [!IMPORTANT]
   > Gewenste eigenschap wijzigingsgebeurtenissen zijn altijd één keer op het apparaatverbinding verzonden. Controleer of er is een werkelijke wijziging in de eigenschappen van de gewenste voordat u een actie uitvoert.
   > 
   > 
1. Voeg de volgende methoden voordat de `client.open()` aanroepen:
   
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
   
    De **initConfigChange** methode de gerapporteerde eigenschappen op het lokale apparaat twin-object wordt bijgewerkt met de configuratie-update-aanvraag en stelt de status op **in behandeling**, werkt u vervolgens de apparaat-twin op de de service. Nadat het apparaat twin is bijgewerkt, wordt een langdurige proces dat wordt beëindigd bij de uitvoering van gesimuleerd **completeConfigChange**. Deze methode werkt u de eigenschappen van lokale gemelde de status is ingesteld op **geslaagd** en verwijderen van de **pendingConfig** object. De apparaat-twin op de service wordt vervolgens bijgewerkt.
   
    Dat het, voor het opslaan van bandbreedte, gerapporteerd eigenschappen zijn bijgewerkt door te geven alleen de eigenschappen te wijzigen (met de naam **patch** in de bovenstaande code), in plaats van het hele document vervangen.
   
   > [!NOTE]
   > Deze zelfstudie wordt een gedrag voor updates voor gelijktijdige configuratie niet simuleren. Bepaalde configuratie-update-processen mogelijk wijzigingen van de doelconfiguratie voor terwijl de update wordt uitgevoerd, enkele mogelijk moet u ze in de wachtrij en sommige met een fout opgetreden weigeren kan. Zorg ervoor dat rekening houden met het gewenste gedrag voor uw specifieke configuratie-proces en de juiste logica toevoegen voordat u begint de wijziging in de configuratie.
   > 
   > 
1. Voer de app voor apparaat:
   
        node SimulateDeviceConfiguration.js
   
    U ziet het bericht `retrieved device twin`. De app actief houden.

## <a name="create-the-service-app"></a>De service-app maken
In deze sectie maakt u een console-app voor .NET-updates de *gewenst eigenschappen* op het apparaat twin gekoppeld **myDeviceId** met een nieuwe telemetrie configuration-object. Vervolgens zoekt de horende apparaat is opgeslagen in de IoT-hub en ziet u het verschil tussen de gewenste en gerapporteerde configuraties van het apparaat.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **SetDesiredConfigurationAndQuery**.
   
    ![Nieuw Windows Classic Desktop-project in Visual C#][img-createapp]
1. Klik in Solution Explorer met de rechtermuisknop op de **SetDesiredConfigurationAndQuery** project en klik vervolgens op **NuGet-pakketten beheren...** .
1. Klik in de **NuGet Package Manager** op **Browse** en zoek naar **microsoft.azure.devices**. Accepteer de gebruiksvoorwaarden en klik op **Install** om het **Microsoft.Azure.Devices**-pakket te installeren. Met deze procedure worden de [Azure IoT-service-SDK][lnk-nuget-service-sdk], het NuGet-pakket en de bijbehorende afhankelijkheden gedownload en geïnstalleerd. Ook worden verwijzingen hiernaar toegevoegd.
   
    ![Sluit het venster Nuget Package Manager.][img-servicenuget]
1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de vorige sectie.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Voeg de volgende methode toe aan de klasse **Program**:
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            try
            {
                while (true)
                {
                    var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                    var results = await query.GetNextAsTwinAsync();
                    foreach (var result in results)
                    {
                        Console.WriteLine("Config report for: {0}", result.DeviceId);
                        Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine();
                    }
                    Thread.Sleep(10000);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
            }
        }
   
    De **register** object bevat de methoden die zijn vereist voor interactie met horende apparaten van de service. Deze code initialiseert de **register** object, haalt de apparaat-twin voor **myDeviceId**, en werkt vervolgens de gewenste eigenschappen met een nieuwe telemetrie configuration-object.
    Daarna wordt deze query's van het apparaat horende opgeslagen in de IoT-hub elke 10 seconden en de gewenste en gerapporteerde telemetrie-configuraties worden afgedrukt. Raadpleeg de [IoT Hub-querytaal] [ lnk-query] voor informatie over het uitgebreide om rapporten te genereren in al uw apparaten.
   
   > [!IMPORTANT]
   > Deze toepassing een IoT Hub query elke 10 seconden ter illustratie. Met query's gebruikersgerichte om rapporten te genereren over verschillende apparaten en niet voor het detecteren van wijzigingen. Als uw oplossing realtime meldingen over apparaatgebeurtenissen vereist, gebruikt u [twin meldingen][lnk-twin-notifications].
   > 
   > 
1. Voeg tot slot de volgende regels toe aan de methode **Main**:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. Open in Solution Explorer de **opstartprojecten instellen...**  en zorg ervoor dat de **actie** voor **SetDesiredConfigurationAndQuery** project **Start**. De oplossing bouwen.
1. Met **SimulateDeviceConfiguration.js** wordt uitgevoerd, voert u de .NET-toepassing vanuit Visual Studio met **F5** en ziet u de configuratie van de gerapporteerde gewijzigd van **geslaagd**naar **in behandeling** naar **geslaagd** opnieuw met de nieuwe actieve verzenden frequentie van vijf minuten in plaats van 24 uur.

 ![Apparaten die zijn geconfigureerd][img-deviceconfigured]
   
   > [!IMPORTANT]
   > Er is een vertraging van maximaal een minuut tussen het apparaat rapport opnieuw en het queryresultaat. Dit is het inschakelen van de query-infrastructuur om te werken op zeer grote schaal. Voor het ophalen van consistente weergaven van een enkel apparaat twin gebruiken de **getDeviceTwin** methode in de **register** klasse.
   > 
   > 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie stelt u een gewenste configuratie als *gewenst eigenschappen* back-end van de oplossing en een app voor het apparaat om te detecteren die wijziging en een updateproces van meerdere stappen de status ervan via de gerapporteerde eigenschappen reporting simuleren geschreven.

Gebruik de volgende bronnen voor meer informatie over hoe:

* verzenden van telemetrie vanaf apparaten met de [aan de slag met IoT Hub] [ lnk-iothub-getstarted] zelfstudie
* schema of bewerkingen uitvoeren op grote sets van apparaten, Zie de [planning en broadcast taken] [ lnk-schedule-jobs] zelfstudie.
* beheren van apparaten interactief (zoals het inschakelen van een ventilator van een gebruiker beheerde app), met de [direct methoden gebruiken] [ lnk-methods-tutorial] zelfstudie.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-node-twin-how-to-configure/deviceconfigured.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

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
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-csharp-node-twin-how-to-configure.md#create-the-simulated-device-app

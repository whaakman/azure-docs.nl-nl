---
title: Aan de slag met Azure IoT Hub Apparaatbeheer (.NET/knooppunt) | Microsoft Docs
description: "Het gebruik van Azure IoT Hub Apparaatbeheer initiëren externe apparaten opnieuw worden opgestart. U kunt het apparaat met Azure IoT SDK voor Node.js gebruiken voor het implementeren van een gesimuleerde apparaattoepassing met een directe methode en de service Azure IoT SDK voor .NET voor het implementeren van een service-app die de directe methode aanroept."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: juanpere
ms.openlocfilehash: 5d0b7b1ab5893e55a6e2aa16451b6a9fc1481966
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2017
---
# <a name="get-started-with-device-management-netnode"></a>Aan de slag met Apparaatbeheer (.NET/knooppunt)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

In deze handleiding ontdekt u hoe u:

* De Azure portal gebruiken voor het maken van een IoT-Hub en een apparaat-id maakt in uw IoT-hub.
* Maak een gesimuleerde apparaattoepassing met een directe methode die het apparaat opnieuw wordt opgestart. Rechtstreekse methoden worden aangeroepen vanuit de cloud.
* Een .NET-consoletoepassing die de directe methode voor opnieuw opstarten in de gesimuleerde apparaattoepassing via uw IoT-hub maken.

Aan het einde van deze zelfstudie hebt u een Node.js-consoletoepassing apparaat en een app in de back-end voor .NET (C#)-console:

**dmpatterns_getstarted_device.js**, die verbinding maakt met uw IoT-hub aan de apparaat-id eerder hebt gemaakt, ontvangt u een directe methode voor opnieuw opstarten, simuleert fysieke opnieuw worden opgestart en rapporten van de tijd voor de laatste keer opnieuw opstarten.

**TriggerReboot**, die een directe methode wordt aangeroepen in de gesimuleerde apparaattoepassing geeft het antwoord weer en geeft de bijgewerkte eigenschappen gerapporteerd.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Node.js-versie 4.0.x of hoger <br/>  [Uw ontwikkelingsomgeving voorbereiden] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie installeren op Windows of Linux.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Activeren van een extern opnieuw opstarten op het apparaat met een directe methode
In deze sectie maakt u een .NET-consoletoepassing (met C#) die een externe opnieuw opstarten op een apparaat met een directe methode start. De app gebruikmaakt van apparaat twin query's voor het detecteren van de laatste keer opnieuw opstarten voor dat apparaat.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de nieuwe oplossing met behulp van de projectsjabloon **Console App (.NET Framework)**. Zorg ervoor dat de versie van .NET Framework minimaal 4.5.1 is. Noem het project **TriggerReboot**.

    ![Nieuw Windows Classic Desktop-project in Visual C#][img-createapp]

2. Klik in Solution Explorer met de rechtermuisknop op de **TriggerReboot** project en klik vervolgens op **NuGet-pakketten beheren**.
3. Klik in de **NuGet Package Manager** op **Browse** en zoek naar **microsoft.azure.devices**. Accepteer de gebruiksvoorwaarden en klik op **Install** om het **Microsoft.Azure.Devices**-pakket te installeren. Met deze procedure worden de [Azure IoT-service-SDK][lnk-nuget-service-sdk], het NuGet-pakket en de bijbehorende afhankelijkheden gedownload en geïnstalleerd. Ook worden verwijzingen hiernaar toegevoegd.

    ![Sluit het venster Nuget Package Manager.][img-servicenuget]
4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de sectie 'Een IoT-hub maken'. 
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "myDeviceId";
        
6. Voeg de volgende methode voor de **programma** klasse.  Deze code haalt de apparaat-twin voor het apparaat rebooting en levert de gerapporteerde eigenschappen.
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. Voeg de volgende methode voor de **programma** klasse.  Deze code wordt het opnieuw opstarten op het apparaat met een directe methode gestart.

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. Voeg tot slot de volgende regels toe aan de methode **Main**:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
8. De oplossing bouwen.

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie wordt u

* U maakt een Node.js-console-app die reageert op een directe methode die door de cloud wordt aangeroepen.
* Een gesimuleerd apparaat opnieuw activeren
* Gebruik de gemelde eigenschappen om het apparaat twin query's om apparaten te identificeren en wanneer ze het laatst opnieuw opgestart

1. Maak een nieuwe lege map genaamd **manageddevice**.  Maak in de map **simulateddevice** een bestand met de naam package.json door achter de opdrachtprompt de volgende opdracht op te geven.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **manageddevice** map, voer de volgende opdracht voor het installeren van de **azure-iot-device** apparaat-SDK-pakket en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Start een teksteditor en maak een nieuwe **dmpatterns_getstarted_device.js** bestand de **manageddevice** map.
4. Voeg de volgende 'vereist' instructies aan het begin van de **dmpatterns_getstarted_device.js** bestand:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Voeg een **connectionString**-variabele toe en gebruik deze om een **client**exemplaar te maken.  De verbindingsreeks vervangen door de verbindingsreeks van uw apparaat.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. De volgende functie voor het implementeren van de directe methode op het apparaat toevoegen
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. Voeg de volgende code voor het openen van de verbinding met uw IoT-hub en de listener directe methode te starten:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. Sla op en sluit de **dmpatterns_getstarted_device.js** bestand.
   
> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).


## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt in de **manageddevice** map, voer de volgende opdracht om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Uitvoeren van de C#-consoletoepassing **TriggerReboot**. Met de rechtermuisknop op de **TriggerReboot** project, selecteer **Debug**, en selecteer vervolgens **nieuw exemplaar gestart**.

3. U ziet de reactie van het apparaat aan de directe methode in de console.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

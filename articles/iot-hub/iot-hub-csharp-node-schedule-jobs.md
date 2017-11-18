---
title: Plannen van taken met Azure IoT Hub (.NET/knooppunt) | Microsoft Docs
description: Klik hier voor meer informatie over het plannen van een taak Azure IoT Hub een directe methode op meerdere apparaten aan te roepen. U het apparaat met Azure IoT SDK voor Node.js gebruiken voor het implementeren van het gesimuleerde apparaat-apps en de service Azure IoT SDK voor .NET voor het implementeren van een app service de taak uit te voeren.
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: juanpere
ms.openlocfilehash: e6795f09e275f9fcd38000d48710560244abc11d
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="schedule-and-broadcast-jobs-netnodejs"></a>Planning en broadcast-taken (.NET/Node.js)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub gebruiken om te plannen en bijhouden van taken die miljoenen apparaten worden bijgewerkt. Taken die moeten worden gebruikt:

* Gewenste eigenschappen bijwerken
* Labels bijwerken
* Directe methoden aanroepen

Een taak een van deze acties worden verpakt en volgt de uitvoering op basis van een verzameling apparaten die is gedefinieerd door een apparaat twin query. Een back-endserver voor apps kunt bijvoorbeeld een taak gebruiken om aan te roepen een directe methode op 10.000 apparaten die de apparaten opnieuw wordt opgestart. U opgeven welke set apparaten met een apparaat twin query en plannen van de taak wilt uitvoeren op een later tijdstip. De voortgang van de taak houdt aangezien aan elk van de apparaten ontvangen en uitvoeren van de directe methode voor opnieuw opstarten.

Zie voor meer informatie over elk van deze mogelijkheden:

* Apparaat-twin en eigenschappen: [aan de slag met apparaat horende] [ lnk-get-started-twin] en [zelfstudie: het gebruik van twin apparaateigenschappen][lnk-twin-props]
* Directe methoden: [IoT Hub developer guide - rechtstreekse methoden] [ lnk-dev-methods] en [zelfstudie: directe methoden gebruiken][lnk-c2d-methods]

In deze handleiding ontdekt u hoe u:

* Maakt een apparaat-app die u een directe methode aangeroepen implementeert **lockDoor** die kan worden aangeroepen door de back-endserver voor apps. De apparaat-app ontvangt ook gewenste wijzigingen van de back-endserver voor apps.
* Maakt een back-end-app die u maakt een taak aan te roepen de **lockDoor** directe methode op meerdere apparaten. Een andere taak verzendt de gewenste eigenschap updates op meerdere apparaten.

Aan het einde van deze zelfstudie hebt u een Node.js-consoletoepassing apparaat en een app in de back-end voor .NET (C#)-console:

**simDevice.js** die is verbonden met uw IoT-hub implementeert het **lockDoor** directe methode en ingangen gewenst eigenschapswijzigingen.

**ScheduleJob** die gebruikmaakt van taken aan te roepen de **lockDoor** directe methode en het bijwerken van de eigenschappen van de gewenste twin apparaten op meerdere apparaten.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Node.js versie 4.0.x of hoger. Het artikel [uw ontwikkelingsomgeving voorbereiden] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie installeren op Windows of Linux.
* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Taken voor het aanroepen van een directe methode en het verzenden van apparaat twin updates plannen

In deze sectie maakt u een .NET-consoletoepassing maken (met C#) die gebruikmaakt van taken aan te roepen de **lockDoor** directe methode en gewenste eigenschap updates verzenden naar meerdere apparaten.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **ScheduleJob**.

    ![Nieuw Windows Classic Desktop-project in Visual C#][img-createapp]

1. Klik in Solution Explorer met de rechtermuisknop op de **ScheduleJob** project en klik vervolgens op **NuGet-pakketten beheren...** .
1. Klik in de **NuGet Package Manager** op **Browse** en zoek naar **microsoft.azure.devices**. Accepteer de gebruiksvoorwaarden en klik op **Install** om het **Microsoft.Azure.Devices**-pakket te installeren. Deze stap downloadt, installeert en voegt u een verwijzing naar de [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-pakket en de bijbehorende afhankelijkheden.

    ![Sluit het venster Nuget Package Manager.][img-servicenuget]
1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Voeg de volgende `using` instructie als deze niet al aanwezig in de standaard-instructies.

    ```csharp
    using System.Threading.Tasks;
    ```

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduiding met de IoT Hub-verbindingsreeks voor de hub die u in de vorige sectie hebt gemaakt.

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && (result.Status != JobStatus.Failed));
    }
    ```

1. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("lockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));

        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            "deviceId='myDeviceId'",
            directMethod,
            DateTime.Now,
            10);

        Console.WriteLine("Started Method Job");
    }
    ```

1. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        var twin = new Twin();
        twin.Properties.Desired["Building"] = "43";
        twin.Properties.Desired["Floor"] = "3";
        twin.ETag = "*";

        JobResponse result = await jobClient.ScheduleTwinUpdateAsync(jobId,
            "deviceId='myDeviceId'",
            twin,
            DateTime.Now,
            10);

        Console.WriteLine("Started Twin Update Job");
    }
    ```

1. Voeg tot slot de volgende regels toe aan de methode **Main**:

    ```csharp
    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER to run the next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```

1. Open in Solution Explorer de **opstartprojecten instellen...**  en zorg ervoor dat de **actie** voor **ScheduleJob** project **Start**. De oplossing bouwen.

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken

In deze sectie maakt u een Node.js-consoletoepassing dat met een directe methode is aangeroepen door de cloud overeenkomt, die een herstart van het gesimuleerde apparaat wordt geactiveerd en maakt gebruik van de gerapporteerde eigenschappen zodat apparaat twin query's om apparaten te identificeren en wanneer ze laatste opnieuw opgestart.

1. Maak een nieuwe lege map genaamd **simDevice**.  In de **simDevice** map, maakt u een package.json-bestand met de volgende opdracht achter de opdrachtprompt.  Accepteer alle standaardwaarden:

    ```cmd/sh
    npm init
    ```

1. Bij de opdrachtprompt in de **simDevice** map, voer de volgende opdracht voor het installeren van de **azure-iot-device** en **azure-iot-device-mqtt** pakketten:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Start een teksteditor en maak een nieuwe **simDevice.js** bestand de **simDevice** map.

1. Voeg de volgende 'vereist' instructies aan het begin van de **simDevice.js** bestand:

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. Voeg een **connectionString**-variabele toe en gebruik deze om een **client**exemplaar te maken. Zorg ervoor dat de tijdelijke aanduidingen vervangen door waarden die geschikt is voor uw installatie.

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Voeg de volgende functie voor het afhandelen van de **lockDoor** methode.

    ```nodejs
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```

1. Voeg de volgende code voor het registreren van de handler voor de **lockDoor** methode.

    ```nodejs
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```

1. Sla op en sluit de **simDevice.js** bestand.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt in de **simDevice** map, voer de volgende opdracht om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.

    ```cmd/sh
    node simDevice.js
    ```

1. Uitvoeren van de C#-consoletoepassing **ScheduleJob** door met de rechtermuisknop op de **ScheduleJob** project selecteren **Debug** en **nieuw exemplaar gestart**.

1. U ziet de uitvoer van apparaat en de back-end-apps.

    ![De apps voor het plannen van taken uitvoeren][img-schedulejobs]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een taak om een directe methode voor een apparaat en het bijwerken van de eigenschappen van de apparaat-twin plannen.

Lees het volgende om door te gaan aan de slag met IoT Hub en device management patronen, zoals extern via de lucht firmware-update [zelfstudie: hoe u een firmware-update][lnk-fwupdate].

Zie voor meer informatie over het implementeren van AI op de edge-apparaten met Azure IoT Edge, [aan de slag met IoT rand][lnk-iot-edge].

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-node-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

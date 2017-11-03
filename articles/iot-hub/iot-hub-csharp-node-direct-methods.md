---
title: Gebruik Azure IoT Hub direct methoden (.NET/knooppunt) | Microsoft Docs
description: Het gebruik van Azure IoT Hub rechtstreekse methoden. U kunt het apparaat met Azure IoT SDK voor Node.js gebruiken voor het implementeren van een gesimuleerde apparaattoepassing met een directe methode en de service Azure IoT SDK voor .NET voor het implementeren van een service-app die de directe methode aanroept.
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2017
ms.author: nberdy
ms.openlocfilehash: 76f1d32b4afeacae1488b4cf28be6c8cf7f4ea37
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-netnode"></a>Directe methoden (.NET/knooppunt) gebruiken
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

In deze zelfstudie gaan we een .NET- en Node.js-consoletoepassing ontwikkelen:

* **CallMethodOnDevice.sln**, een .NET back-end-app, die een methode wordt aangeroepen in de gesimuleerde apparaattoepassing en het antwoord wordt weergegeven.
* **SimulatedDevice.js**, een Node.js-app, die overeenkomt met een apparaat verbinding te maken met uw iothub met de identiteit van het apparaat eerder hebt gemaakt en reageert op de methode aangeroepen door de cloud.

> [!NOTE]
> Het artikel [Azure IoT-SDK's][lnk-hub-sdks] bevat informatie over de verschillende Azure IoT-SDK's die u kunt gebruiken om beide toepassingen zo te maken dat ze zowel op het apparaat als op de back-end van uw oplossing kunnen worden uitgevoerd.
> 
> 

Voor deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie maakt u een Node.js-consoletoepassing dat met een methode aangeroepen door de oplossing voor back-end overeenkomt.

1. Maak een nieuwe lege map met de naam **simulateddevice**. In de map **simulateddevice** maakt u een package.json-bestand door in het opdrachtprompt de volgende opdracht op te geven. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **simulateddevice** map, voer de volgende opdracht voor het installeren van de **azure-iot-device** en **azure-iot-device-mqtt** pakketten:
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Start een teksteditor en maak een bestand in de **simulateddevice** map en noem deze **SimulatedDevice.js**.
4. Voeg de volgende `require` instructies toe aan het begin van het bestand **SimulatedDevice.js**:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Voeg een **connectionString** variabele en deze gebruiken voor het maken een **DeviceClient** exemplaar. Vervang **{verbindingsreeks apparaat}** tekenreeks met de apparaatverbinding u hebt gegenereerd tijdens de *maken van een apparaat-id* sectie:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Voeg de volgende functie voor het implementeren van de directe methode op het apparaat toe:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Maak verbinding met uw IoT-hub en de methode-listener initialiseren:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Sla het bestand **SimulatedDevice.js** op en sluit het.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. In productiecode moet u beleid voor opnieuw proberen (zoals verbinding opnieuw), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Een directe methode is aangeroepen voor een apparaat
In deze sectie maakt u een .NET consoletoepassing maken die een methode wordt aangeroepen in de gesimuleerde apparaattoepassing en wordt vervolgens het antwoord.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Zorg ervoor dat de versie van .NET Framework minimaal 4.5.1 is. Noem het project **CallMethodOnDevice**.
   
    ![Nieuw Windows Classic Desktop-project in Visual C#][10]
2. Klik in Solution Explorer met de rechtermuisknop op de **CallMethodOnDevice** project en klik vervolgens op **NuGet-pakketten beheren...** .
3. Klik in de **NuGet Package Manager** op **Browse** en zoek naar **microsoft.azure.devices**. Accepteer de gebruiksvoorwaarden en klik op **Install** om het **Microsoft.Azure.Devices**-pakket te installeren. Met deze procedure worden de [Azure IoT-service-SDK][lnk-nuget-service-sdk], het NuGet-pakket en de bijbehorende afhankelijkheden gedownload en geïnstalleerd. Ook worden verwijzingen hiernaar toegevoegd.
   
    ![Sluit het venster Nuget Package Manager.][11]

4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de vorige sectie.
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Voeg de volgende methode toe aan de klasse **Program**:
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line to be written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    Deze methode wordt aangeroepen voor een directe methode met de naam `writeLine` op de `myDeviceId` apparaat. Vervolgens worden de respons van het apparaat in de console geschreven. Houd er rekening mee hoe het is mogelijk om op te geven van een time-outwaarde voor het apparaat om te reageren.
7. Voeg tot slot de volgende regels toe aan de methode **Main**:
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

## <a name="run-the-applications"></a>De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. In de Visual Studio Solution Explorer met de rechtermuisknop op uw oplossing en klik vervolgens op **Opstartprojecten instellen...** . Selecteer **één opstartproject**, en selecteer vervolgens de **CallMethodOnDevice** -project in de vervolgkeuzelijst.

2. Bij een opdrachtprompt in de **simulateddevice** map, voer de volgende opdracht om te beginnen met luisteren voor methodeaanroepen vanuit uw IoT-Hub:
   
    ```
    node SimulatedDevice.js
    ```
   Wachten op het gesimuleerde apparaat te openen:![][7]
3. Nu dat het apparaat is verbonden en wachten op methode-aanroepen uitgevoerd .NET **CallMethodOnDevice** app de methode in de gesimuleerde apparaattoepassing aan te roepen. U ziet de reactie van het apparaat in de console geschreven.
   
    ![][8]
4. Het apparaat reageert vervolgens aan de methode door dit bericht afdrukken:
   
    ![][9]

## <a name="next-steps"></a>Volgende stappen
In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt deze apparaat-id gebruikt om in te schakelen van de gesimuleerde apparaattoepassing om te reageren op de methoden die worden aangeroepen door de cloud. Hebt u ook een app roept methoden op het apparaat en de reactie van het apparaat wordt gemaakt. 

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Aan de slag met IoT Hub]
* [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Zie voor meer informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten aanroepen, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Aan de slag met IoT Hub]: iot-hub-node-node-getstarted.md

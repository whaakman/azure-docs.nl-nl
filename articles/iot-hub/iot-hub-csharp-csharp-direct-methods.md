---
title: Gebruik Azure IoT Hub direct methoden (.NET/.NET) | Microsoft Docs
description: Het gebruik van Azure IoT Hub rechtstreekse methoden. U het apparaat met Azure IoT SDK voor .NET gebruiken voor het implementeren van een gesimuleerde apparaattoepassing met een directe methode en de service Azure IoT SDK voor .NET voor het implementeren van een service-app die de directe methode aanroept.
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: dkshir
ms.openlocfilehash: 9ce1fbebb6417c10618aa182e3c1d9ddf8132fb6
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2017
---
# <a name="use-direct-methods-netnet"></a>Directe methoden (.NET/.NET) gebruiken
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

In deze zelfstudie gaan we twee .NET-console-apps ontwikkelen:

* **CallMethodOnDevice**, een back-end-app, die een methode wordt aangeroepen in de gesimuleerde apparaattoepassing en het antwoord weergegeven.
* **SimulateDeviceMethods**, een consoletoepassing waarmee een verbinding te maken met uw IoT-hub aan de apparaat-id apparaat simuleert eerder hebt gemaakt en reageert de methode wordt aangeroepen door de cloud.

> [!NOTE]
> Het artikel [Azure IoT-SDK's][lnk-hub-sdks] bevat informatie over de verschillende Azure IoT-SDK's die u kunt gebruiken om beide toepassingen zo te maken dat ze zowel op het apparaat als op de back-end van uw oplossing kunnen worden uitgevoerd.
> 
> 

Voor deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Als u de apparaat-id in plaats daarvan programmatisch maken wilt, leest u de bijbehorende sectie in het [uw gesimuleerde apparaat verbonden met uw iothub met .NET] [ lnk-device-identity-csharp] artikel.


## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie maakt u een .NET-consoletoepassing dat met een methode aangeroepen door de oplossing voor back-end overeenkomt.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **SimulateDeviceMethods**.
   
    ![Nieuwe Visual C# klassieke Windows-apparaat-app][img-createdeviceapp]
    
1. Klik in Solution Explorer met de rechtermuisknop op de **SimulateDeviceMethods** project en klik vervolgens op **NuGet-pakketten beheren...** .
1. In de **NuGet Package Manager** Selecteer **Bladeren** en zoek naar **microsoft.azure.devices.client**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices.Client** Inpakken en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT-device SDK] [ lnk-nuget-client-sdk] NuGet-pakket en de bijbehorende afhankelijkheden.
   
    ![NuGet-Pakketbeheer venster Client-app][img-clientnuget]
1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de verbindingsreeks voor apparaten die u in de vorige sectie hebt genoteerd.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. Voeg de volgende voor het implementeren van de directe methode op het apparaat:

        static Task<MethodResponse> WriteLineToConsole(MethodRequest methodRequest, object userContext)
        {
            Console.WriteLine();
            Console.WriteLine("\t{0}", methodRequest.DataAsJson);
            Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

            string result = "'Input was written to log.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

1. Voeg de volgende code naar de **Main** methode voor het openen van de verbinding met uw IoT-hub en de methode-listener initialiseren:
   
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

            // setup callback for "writeLine" method
            Client.SetMethodHandlerAsync("writeLine", WriteLineToConsole, null).Wait();
            Console.WriteLine("Waiting for direct method call\n Press enter to exit.");
            Console.ReadLine();

            Console.WriteLine("Exiting...");

            // as a good practice, remove the "writeLine" handler
            Client.SetMethodHandlerAsync("writeLine", null, null).Wait();
            Client.CloseAsync().Wait();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
        
1. In de Visual Studio Solution Explorer met de rechtermuisknop op uw oplossing en klik vervolgens op **Opstartprojecten instellen...** . Selecteer **één opstartproject**, en selecteer vervolgens de **SimulateDeviceMethods** -project in de vervolgkeuzelijst.        

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. In productiecode moet u beleid voor opnieuw proberen (zoals verbinding opnieuw), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Een directe methode is aangeroepen voor een apparaat
In deze sectie maakt u een .NET consoletoepassing maken die een methode wordt aangeroepen in de gesimuleerde apparaattoepassing en wordt vervolgens het antwoord.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Zorg ervoor dat de versie van .NET Framework minimaal 4.5.1 is. Noem het project **CallMethodOnDevice**.
   
    ![Nieuw Windows Classic Desktop-project in Visual C#][img-createserviceapp]
2. Klik in Solution Explorer met de rechtermuisknop op de **CallMethodOnDevice** project en klik vervolgens op **NuGet-pakketten beheren...** .
3. Klik in de **NuGet Package Manager** op **Browse** en zoek naar **microsoft.azure.devices**. Accepteer de gebruiksvoorwaarden en klik op **Install** om het **Microsoft.Azure.Devices**-pakket te installeren. Met deze procedure worden de [Azure IoT-service-SDK][lnk-nuget-service-sdk], het NuGet-pakket en de bijbehorende afhankelijkheden gedownload en geïnstalleerd. Ook worden verwijzingen hiernaar toegevoegd.
   
    ![Sluit het venster Nuget Package Manager.][img-servicenuget]

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

1. In de Visual Studio Solution Explorer met de rechtermuisknop op uw oplossing en klik vervolgens op **Opstartprojecten instellen...** . Selecteer **één opstartproject**, en selecteer vervolgens de **CallMethodOnDevice** -project in de vervolgkeuzelijst.

## <a name="run-the-applications"></a>De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. Voer de app voor .NET-apparaat **SimulateDeviceMethods**. Deze zou moeten starten luisteren naar methodeaanroepen van uw IoT-Hub: 

    ![Apparaat-app uitvoeren][img-deviceapprun]
1. Nu dat het apparaat is verbonden en wachten op methode-aanroepen uitgevoerd .NET **CallMethodOnDevice** app de methode in de gesimuleerde apparaattoepassing aan te roepen. U ziet de reactie van het apparaat in de console geschreven.
   
    ![Service-app uitvoeren][img-serviceapprun]
1. Het apparaat reageert vervolgens aan de methode door dit bericht afdrukken:
   
    ![Directe methode wordt aangeroepen op het apparaat][img-directmethodinvoked]

## <a name="next-steps"></a>Volgende stappen
In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt deze apparaat-id gebruikt om in te schakelen van de gesimuleerde apparaattoepassing om te reageren op de methoden die worden aangeroepen door de cloud. Hebt u ook een app roept methoden op het apparaat en de reactie van het apparaat wordt gemaakt. 

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Aan de slag met IoT Hub]
* [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Zie voor meer informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten aanroepen, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

<!-- Images. -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-direct-methods/device-app-nuget.png
[img-createserviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-service-app.png
[img-servicenuget]: ./media/iot-hub-csharp-csharp-direct-methods/service-app-nuget.png
[img-deviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-device-app.png
[img-serviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-service-app.png
[img-directmethodinvoked]: ./media/iot-hub-csharp-csharp-direct-methods/direct-method-invoked.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[Aan de slag met IoT Hub]: iot-hub-node-node-getstarted.md

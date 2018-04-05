---
title: Plannen van taken met Azure IoT Hub (.NET/.NET) | Microsoft Docs
description: Klik hier voor meer informatie over het plannen van een taak Azure IoT Hub een directe methode op meerdere apparaten aan te roepen. U het apparaat met Azure IoT SDK voor .NET gebruiken voor het implementeren van het gesimuleerde apparaat-apps en een app service de taak uit te voeren.
services: iot-hub
documentationcenter: .net
author: msebolt
manager: timlt
editor: ''
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 012/16/2018
ms.author: v-masebo
ms.openlocfilehash: 59d78c390e479acd16d47b70e8af5e164d746905
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="schedule-and-broadcast-jobs-netnet"></a>Planning en broadcast-taken (.NET/.NET)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub gebruiken om te plannen en bijhouden van taken die miljoenen apparaten worden bijgewerkt. Taken die moeten worden gebruikt:

* Gewenste eigenschappen bijwerken
* Labels bijwerken
* Directe methoden aanroepen

Een taak een van deze acties worden verpakt en volgt de uitvoering op basis van een verzameling apparaten die is gedefinieerd door een apparaat twin query. Een back-endserver voor apps kunt bijvoorbeeld een taak gebruiken om aan te roepen een directe methode op 10.000 apparaten die de apparaten opnieuw wordt opgestart. U opgeven welke set apparaten met een apparaat twin query en plannen van de taak wilt uitvoeren op een later tijdstip. De voortgang van de taak houdt aangezien aan elk van de apparaten ontvangen en uitvoeren van de directe methode voor opnieuw opstarten.

Zie voor meer informatie over elk van deze mogelijkheden:

* Apparaat-twin en eigenschappen: [aan de slag met apparaat horende] [ lnk-get-started-twin] en [zelfstudie: het gebruik van twin apparaateigenschappen][lnk-twin-props]
* Directe methoden: [IoT Hub developer guide - rechtstreekse methoden] [ lnk-dev-methods] en [zelfstudie: directe methoden gebruiken][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze handleiding ontdekt u hoe u:

* Maakt een apparaat-app die u een directe methode aangeroepen implementeert **LockDoor** die kan worden aangeroepen door de back-endserver voor apps.
* Maakt een back-end-app die u maakt een taak aan te roepen de **LockDoor** directe methode op meerdere apparaten. Een andere taak verzendt de gewenste eigenschap updates op meerdere apparaten.

Aan het einde van deze zelfstudie hebt u twee .NET (C#) console apps:

**SimulateDeviceMethods** die is verbonden met uw IoT-hub en implementeert de **LockDoor** directe methode.

**ScheduleJob** die gebruikmaakt van taken aan te roepen de **LockDoor** directe methode en het bijwerken van de eigenschappen van de gewenste twin apparaten op meerdere apparaten.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie maakt u een .NET-consoletoepassing dat met een directe methode is aangeroepen door de oplossing voor back-end overeenkomt.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **SimulateDeviceMethods**.
   
    ![Nieuwe Visual C# klassieke Windows-apparaat-app][img-createdeviceapp]
    
1. Klik in Solution Explorer met de rechtermuisknop op de **SimulateDeviceMethods** project en klik vervolgens op **NuGet-pakketten beheren...** .

1. In de **NuGet Package Manager** Selecteer **Bladeren** en zoek naar **microsoft.azure.devices.client**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices.Client** Inpakken en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT-device SDK] [ lnk-nuget-client-sdk] NuGet-pakket en de bijbehorende afhankelijkheden.
   
    ![NuGet-Pakketbeheer venster Client-app][img-clientnuget]

1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;

    using Newtonsoft.Json;
    ```

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de verbindingsreeks voor apparaten die u in de vorige sectie hebt genoteerd:

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;

1. Add the following to implement the direct method on the device:

    ```csharp
    static Task<MethodResponse> LockDoor(MethodRequest methodRequest, object userContext)
    {
        Console.WriteLine();
        Console.WriteLine("Locking Door!");
        Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);
            
        string result = "'Door was locked.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }

1. Add the following to implement the device twins listener on the device:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

1. Voeg de volgende code naar de **Main** methode voor het openen van de verbinding met uw IoT-hub en de methode-listener initialiseren:
   
    ```csharp
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

        Client.SetMethodHandlerAsync("LockDoor", LockDoor, null);
        Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null);

        Console.WriteLine("Waiting for direct method call and device twin update\n Press enter to exit.");
        Console.ReadLine();

        Console.WriteLine("Exiting...");

        Client.SetMethodHandlerAsync("LockDoor", null, null);
        Client.CloseAsync().Wait();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
1. Sla uw werk en bouwen van uw oplossing.         

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. In productiecode moet u beleid voor opnieuw proberen (zoals verbinding opnieuw), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout][lnk-transient-faults].
> 


## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Taken voor het aanroepen van een directe methode en het verzenden van apparaat twin updates plannen

In deze sectie maakt u een .NET-consoletoepassing maken (met C#) die gebruikmaakt van taken aan te roepen de **LockDoor** directe methode en gewenste eigenschap updates verzenden naar meerdere apparaten.

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
    using System.Threading;
    using System.Threading.Tasks;
    ```

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingen door de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de vorige sectie en de naam van uw apparaat.

    ```csharp
    static string connString = "<yourIotHubConnectionString>";
    static string deviceId = "<yourDeviceId>";
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
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("LockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));
       
        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            $"DeviceId IN ['{deviceId}']",
            directMethod,
            DateTime.UtcNow,
            (long)TimeSpan.FromMinutes(2).TotalSeconds);

        Console.WriteLine("Started Method Job");
    }
    ```

1. Toevoegen van een andere methode voor de **programma** klasse:

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        Twin twin = new Twin(deviceId);
        twin.Tags = new TwinCollection();
        twin.Tags["Building"] = "43";
        twin.Tags["Floor"] = "3";
        twin.ETag = "*";

        twin.Properties.Desired["LocationUpdate"] = DateTime.UtcNow;

        JobResponse createJobResponse = jobClient.ScheduleTwinUpdateAsync(
            jobId,
            $"DeviceId IN ['{deviceId}']", 
            twin, 
            DateTime.UtcNow, 
            (long)TimeSpan.FromMinutes(2).TotalSeconds).Result;

        Console.WriteLine("Started Twin Update Job");
    }
    ```

    > [!NOTE]
    > Zie voor meer informatie over de querysyntaxis van de [IoT Hub-querytaal][lnk-query].
    > 

1. Voeg tot slot de volgende regels toe aan de methode **Main**:

    ```csharp
    Console.WriteLine("Press ENTER to start running jobs.");
    Console.ReadLine();

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

1. Sla uw werk en bouwen van uw oplossing. 


## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. In de Visual Studio Solution Explorer met de rechtermuisknop op uw oplossing en klik vervolgens op **bouwen**. **Meerdere opstartprojecten**. Zorg ervoor dat `SimulateDeviceMethods` wordt boven aan de lijst die wordt gevolgd door `ScheduleJob`. Beide hun acties ingesteld op **Start** en klik op **OK**.

1. Voer de projecten door te klikken **Start** of Ga naar de **Debug** en klik op **foutopsporing starten**.

1. U ziet de uitvoer van apparaat en de back-end-apps.

    ![De apps voor het plannen van taken uitvoeren][img-schedulejobs]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een taak om een directe methode voor een apparaat en het bijwerken van de eigenschappen van de apparaat-twin plannen.

Lees het volgende om door te gaan aan de slag met IoT Hub en device management patronen, zoals extern via de lucht firmware-update [zelfstudie: hoe u een firmware-update][lnk-fwupdate].

Zie voor meer informatie over het implementeren van AI op de edge-apparaten met Azure IoT Edge, [aan de slag met IoT rand][lnk-iot-edge].

<!-- images -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-schedule-jobs/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-schedule-jobs/device-app-nuget.png
[img-servicenuget]: media/iot-hub-csharp-csharp-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-csharp-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-twin-props]: iot-hub-csharp-csharp-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-csharp-csharp-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-csharp-csharp-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://docs.microsoft.com/azure/architecture/best-practices/transient-faults
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-query]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-query-language

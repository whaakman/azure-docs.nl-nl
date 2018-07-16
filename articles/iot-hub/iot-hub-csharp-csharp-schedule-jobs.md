---
title: Plannen van taken met Azure IoT Hub (.NET/.NET) | Microsoft Docs
description: Klik hier voor meer informatie over het plannen van een taak is Azure IoT Hub een rechtstreekse methode aanroepen op meerdere apparaten. U de Azure IoT-device-SDK voor .NET gebruiken voor het implementeren van de gesimuleerde apparaat-apps en een app service de taak uit te voeren.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/06/2018
ms.author: dobett
ms.openlocfilehash: 71de0a86594737f5f581f9a1f638bab0f7b06e8e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056739"
---
# <a name="schedule-and-broadcast-jobs-netnet"></a>Taken plannen en uitzenden (.NET/.NET)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Gebruik Azure IoT Hub voor het plannen en bijhouden van taken door miljoenen apparaten bij te werken. Taken die moeten worden gebruikt:

* Gewenste eigenschappen bijwerken
* Bijwerken van tags
* Directe methoden aanroepen

Een taak een van deze acties wordt verpakt en houdt de uitvoering op basis van een set met apparaten die door een dubbele apparaatquery is gedefinieerd. Een back-end-app kunt bijvoorbeeld een taak aan een rechtstreekse methode aanroepen op 10.000 apparaten die de apparaten opnieuw is opgestart. U geeft de set met apparaten met een dubbel apparaatquery en plannen van de taak wordt uitgevoerd op een later tijdstip. De voortgang van de taak worden bijgehouden als elk van de apparaten ontvangen en voer de directe methode die opnieuw worden opgestart.

Zie voor meer informatie over elk van deze mogelijkheden:

* Apparaatdubbel en eigenschappen: [aan de slag met apparaatdubbels] [ lnk-get-started-twin] en [zelfstudie: apparaatdubbeleigenschappen gebruiken][lnk-twin-props]
* Directe methoden: [het Ontwikkelaarshandleiding voor IoT Hub - directe methoden] [ lnk-dev-methods] en [zelfstudie: directe methoden gebruiken][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze zelfstudie ontdekt u hoe u:

* Maakt een apparaat-app die u een rechtstreekse methode met de naam implementeert **LockDoor** die kan worden aangeroepen door de back-end-app.
* Maakt een back-end-app die u maakt een taak aan te roepen de **LockDoor** directe methode op meerdere apparaten. Een andere taak verzendt gewenste eigenschap updates voor meerdere apparaten.

Aan het einde van deze zelfstudie, beschikt u over twee .NET (C#)-consoletoepassingen:

**SimulateDeviceMethods** die is verbonden met uw IoT-hub en implementeert de **LockDoor** directe methode.

**ScheduleJob** die gebruikmaakt van taken om aan te roepen de **LockDoor** directe methode en bijwerken van de gewenste apparaatdubbeleigenschappen op meerdere apparaten.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie maakt u een .NET-consoletoepassing die op een rechtstreekse methode aangeroepen door de oplossing back end reageert.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **SimulateDeviceMethods**.
   
    ![Nieuwe Visual C# Windows klassieke apparaat-app][img-createdeviceapp]
    
1. Klik in Solution Explorer met de rechtermuisknop op de **SimulateDeviceMethods** project en klik vervolgens op **NuGet-pakketten beheren...** .

1. In de **NuGet Package Manager** venster **Bladeren** en zoek naar de **microsoft.azure.devices.client**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices.Client** verpakt en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT device-SDK] [ lnk-nuget-client-sdk] NuGet-pakket en de bijbehorende afhankelijkheden.
   
    ![NuGet-Pakketbeheer venster Client-app][img-clientnuget]

1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;

    using Newtonsoft.Json;
    ```

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de apparaatverbindingsreeks die u in de vorige sectie hebt genoteerd:

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;
    ```

1. Voeg het volgende voor het implementeren van de directe methode die op het apparaat:

    ```csharp
    static Task<MethodResponse> LockDoor(MethodRequest methodRequest, object userContext)
    {
        Console.WriteLine();
        Console.WriteLine("Locking Door!");
        Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);
            
        string result = "'Door was locked.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }
    ```

1. Voeg het volgende voor het implementeren van de device twins listener op het apparaat:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

1. Voeg de volgende code om de **Main** methode voor het openen van de verbinding met uw IoT-hub en de methode-listener initialiseren:
   
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
        
1. Sla uw werk op en bouw uw oplossing.         

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u beleid voor opnieuw proberen (zoals de verbinding opnieuw proberen), zoals aangegeven in het MSDN-artikel implementeren [afhandeling van tijdelijke fouten][lnk-transient-faults].
> 


## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Taken plannen voor een rechtstreekse methode aanroepen en het verzenden van de apparaatdubbel werkt bij

In deze sectie maakt u een .NET-consoletoepassing maken (met behulp van C#) die gebruikmaakt van taken om aan te roepen de **LockDoor** directe methode en de gewenste eigenschap updates verzenden naar meerdere apparaten.

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

1. Voeg de volgende `using` instructie als deze niet al aanwezig zijn in de standaard-instructies.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingen door de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de vorige sectie en de naam van uw apparaat.

    ```csharp
    static JobClient jobClient;
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

1. Toevoegen van een andere methode naar de **programma** klasse:

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
    > Zie voor meer informatie over de syntaxis van query's, [IoT Hub-querytaal][lnk-query].
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

1. Sla uw werk op en bouw uw oplossing. 


## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. In de Visual Studio Solution Explorer met de rechtermuisknop op uw oplossing en klik vervolgens op **bouwen**. **Meerdere opstartprojecten**. Zorg ervoor dat `SimulateDeviceMethods` is aan de bovenkant van de lijst die wordt gevolgd door `ScheduleJob`. Beide hun acties ingesteld op **Start** en klikt u op **OK**.

1. De projecten uitvoeren door te klikken op **Start** of gaat u naar de **Debug** en klik op **Start Debugging**.

1. U ziet de uitvoer van zowel de apparaat- en de back-end-apps.

    ![De apps voor het plannen van taken uitvoeren][img-schedulejobs]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een taak voor het plannen van een rechtstreekse methode aan een apparaat en het bijwerken van eigenschappen van het dubbele apparaat.

Lees het volgende om door te gaan aan de slag met IoT Hub en patronen voor Apparaatbeheer zoals het op afstand via de lucht firmware-update [zelfstudie: hoe u een firmware-update doet][lnk-fwupdate].

Zie voor meer informatie over het implementeren van AI op edge-apparaten met Azure IoT Edge, [aan de slag met IoT Edge][lnk-iot-edge].

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
[lnk-fwupdate]: tutorial-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://docs.microsoft.com/azure/architecture/best-practices/transient-faults
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-query]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-query-language

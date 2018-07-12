---
title: Apparaat firmware-update met Azure IoT Hub (.NET/.NET) | Microsoft Docs
description: Het beheer van apparaten op Azure IoT Hub gebruiken om een firmware-update van het apparaat. U de Azure IoT-device-SDK voor .NET gebruiken om een gesimuleerde apparaat-app en de Azure IoT service SDK voor .NET voor het implementeren van een service-app die wordt geactiveerd de firmware-update te implementeren.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 10/19/2017
ms.author: dobett
ms.openlocfilehash: 1bf7a647ab2fdc175231133b0cfd8abdd51b6d43
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723929"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnet"></a>Apparaatbeheer gebruiken om een apparaat firmware-update (.NET/.NET)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Inleiding
In de [aan de slag met Apparaatbeheer] [ lnk-dm-getstarted] zelfstudie, hebt u gezien hoe u de [apparaatdubbel] [ lnk-devtwin] en [directe methoden ] [ lnk-c2dmethod] primitieven aan een apparaat op afstand opnieuw opstarten. In deze zelfstudie maakt gebruik van de dezelfde IoT Hub-primitieven en ziet u hoe u een end-to-end gesimuleerde firmware-update doet.  Dit patroon wordt gebruikt in de firmware-update-implementatie voor de [Raspberry Pi apparaat implementatie voorbeeld][lnk-rpi-implementation].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze handleiding ontdekt u hoe u:

* Een .NET-consoletoepassing die roept de **firmwareUpdate** directe methode in de gesimuleerde apparaattoepassing via uw IoT-hub.
* Maakt een gesimuleerde apparaat-app die u implementeert een **firmwareUpdate** directe methode. Deze methode initieert een proces met meerdere fasen dat wacht om te downloaden van de firmware-installatiekopie, wordt de firmware-installatiekopie gedownload en ten slotte past de firmware-installatiekopie. Tijdens elke fase van de update wordt het apparaat gebruikt van de gerapporteerde eigenschappen om te rapporteren over hun voortgang.

Aan het einde van deze zelfstudie hebt u een apparaat-app van .NET (C#)-console en een back-end-.NET (C#) console-app:

* **SimulatedDeviceFwUpdate**, deze toepassing koppelt uw IoT-hub aan de apparaat-id die eerder hebt gemaakt, ontvangt de **firmwareUpdate** directe methode, wordt uitgevoerd via een proces met meerdere staat voor het simuleren van een firmware-update met inbegrip van: wachten op de installatiekopie downloaden, de nieuwe installatiekopie downloaden en ten slotte de installatiekopie toe te passen.

* **TriggerFWUpdate**, waarbij wordt de service-SDK gebruikt voor het extern aanroepen van de **firmwareUpdate** directe methode op het gesimuleerde apparaat, wordt het antwoord weergegeven en periodiek (elke 500ms) geeft de bijgewerkte gerapporteerd de eigenschappen.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

Ga als volgt de [aan de slag met Apparaatbeheer](iot-hub-csharp-csharp-device-management-get-started.md) artikel om te maken van uw IoT-hub en uw IoT Hub-verbindingsreeks ophalen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Activeren van een externe firmware-update op het apparaat met een rechtstreekse methode
In deze sectie maakt u een .NET-console-app (met behulp van C#) die een externe firmware-update op een apparaat initieert. De app maakt gebruik van een rechtstreekse methode om de update te initiëren en apparaatdubbel-query's gebruikt om periodiek de status van de actieve firmware-update.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **TriggerFWUpdate**.

    ![Nieuw Windows Classic Desktop-project in Visual C#][img-createserviceapp]

2. Klik in Solution Explorer met de rechtermuisknop op de **TriggerFWUpdate** project en klik vervolgens op **NuGet-pakketten beheren**.
3. Klik in de **NuGet Package Manager** op **Browse** en zoek naar **microsoft.azure.devices**. Accepteer de gebruiksvoorwaarden en klik op **Install** om het **Microsoft.Azure.Devices**-pakket te installeren. Met deze procedure worden de [Azure IoT-service-SDK][lnk-nuget-service-sdk], het NuGet-pakket en de bijbehorende afhankelijkheden gedownload en geïnstalleerd. Ook worden verwijzingen hiernaar toegevoegd.

    ![Sluit het venster Nuget Package Manager.][img-servicenuget]
4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:

    ```csharp   
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: De tijdelijke aanduiding voor waarden vervangen door de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de vorige sectie en de ID van uw apparaat.
   
    ```csharp   
    static RegistryManager registryManager;
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static string targetDevice = "{deviceIdForTargetDevice}";
    ```
        
6. Voeg de volgende methode toe aan de klasse **Program**. Deze methode controleert het dubbele apparaat voor de bijgewerkte status van elke 500 milliseconden. Er wordt geschreven naar de console alleen wanneer de status is gewijzigd. Voor dit voorbeeld, om te voorkomen dat extra IoT Hub-berichten in uw abonnement, verbruikt polling stopt wanneer het apparaat de status van meldt **applyComplete** of een fout.  
   
    ```csharp   
    public static async Task QueryTwinFWUpdateReported(DateTime startTime)
    {
        DateTime lastUpdated = startTime;

        while (true)
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);

            if (twin.Properties.Reported.GetLastUpdated().ToUniversalTime() > lastUpdated.ToUniversalTime())
            {
                lastUpdated = twin.Properties.Reported.GetLastUpdated().ToUniversalTime();
                Console.WriteLine("\n" + twin.Properties.Reported["iothubDM"].ToJson());

                var status = twin.Properties.Reported["iothubDM"]["firmwareUpdate"]["status"].Value;
                if ((status == "downloadFailed") || (status == "applyFailed") || (status == "applyComplete"))
                {
                    Console.WriteLine("\nStop polling.");
                    return;
                }
            }
            await Task.Delay(500);
        }
    }
    ```
        
7. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp   
    public static async Task StartFirmwareUpdate()
    {
        client = ServiceClient.CreateFromConnectionString(connString);
        CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
        method.ResponseTimeout = TimeSpan.FromSeconds(30);
        method.SetPayloadJson(
            @"{
               fwPackageUri : 'https://someurl'
            }");

        CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

        Console.WriteLine("Invoked firmware update on device.");
    }
    ```

8. Voeg de volgende regels aan de **Main** methode. Hiermee maakt u een register manager om te lezen van het dubbele apparaat met, begint de polling-taak op een werkthread en activeert de firmware-update.
   
    ```csharp   
    registryManager = RegistryManager.CreateFromConnectionString(connString);

    Task queryTask = Task.Run(() => (QueryTwinFWUpdateReported(DateTime.Now)));

    StartFirmwareUpdate().Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```
        
9. Bouw de oplossing.

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie doet u het volgende:

* Een .NET-consoletoepassing die op een rechtstreekse methode aangeroepen door de cloud reageert maken
* Een firmware-update die is geactiveerd door een back-endservice via een rechtstreekse methode simuleren
* U gebruikt de gerapporteerde eigenschappen om apparaatdubbelquery's in te schakelen die de apparaten identificeren en vaststellen wanneer er voor het laatst een firmware-update op deze apparaten is uitgevoerd.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **SimulateDeviceFWUpdate**.
   
    ![Nieuwe Visual C# Windows klassieke apparaat-app][img-createdeviceapp]
    
2. Klik in Solution Explorer met de rechtermuisknop op de **SimulateDeviceFWUpdate** project en klik vervolgens op **NuGet-pakketten beheren**.
3. In de **NuGet Package Manager** venster **Bladeren** en zoek naar de **microsoft.azure.devices.client**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices.Client** verpakt en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT device-SDK] [ lnk-nuget-client-sdk] NuGet-pakket en de bijbehorende afhankelijkheden.
   
    ![NuGet-Pakketbeheer venster Client-app][img-clientnuget]
4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
    ```csharp   
    using Newtonsoft.Json.Linq;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de apparaatverbindingsreeks die u hebt genoteerd in de **maken van een apparaat-id** sectie.
   
    ```csharp   
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. Voeg de volgende methode toe voor statusrapportage terug naar de cloud via het dubbele apparaat: 

    ```csharp   
    static async Task reportFwUpdateThroughTwin(Twin twin, TwinCollection fwUpdateValue)
    {
        try
        {
            TwinCollection patch = new TwinCollection();
            TwinCollection iothubDM = new TwinCollection();

            iothubDM["firmwareUpdate"] = fwUpdateValue;
            patch["iothubDM"] = iothubDM;

            await Client.UpdateReportedPropertiesAsync(patch);
            Console.WriteLine("Twin state reported: {0}", fwUpdateValue["status"]);
        }
        catch 
        {
            Console.WriteLine("Error updating device twin");
            throw;
        }
    }
    ```

7. De volgende methode toe om te simuleren het downloaden van de firmware-installatiekopie:
        
    ```csharp   
    static async Task<byte[]> simulateDownloadImage(string imageUrl)
    {
        var image = "[fake image data]";

        Console.WriteLine("Downloading image from " + imageUrl);

        await Task.Delay(4000);

        return Encoding.ASCII.GetBytes(image);
            
    }
    ```

8. De volgende methode toe om te simuleren met de firmware-installatiekopie wordt toegepast op het apparaat:
        
    ```csharp   
    static async Task simulateApplyImage(byte[] imageData)
    {
        if (imageData == null)
        {
            throw new ArgumentNullException();
        }

        await Task.Delay(4000);

    }
    ```
 
9.  De volgende methode toe om te simuleren wachten om te downloaden van de firmware-installatiekopie. Status bijwerken naar **wachten** en andere eigenschappen van de firmware bijwerken op het dubbele wissen. Deze eigenschappen zijn uitgeschakeld als u wilt verwijderen van bestaande waarden uit eerdere firmware-updates. Dit is nodig omdat de gerapporteerde eigenschappen worden verzonden als een PATCH-bewerking (een delta-) en niet een PUT-bewerking (een volledige set eigenschappen die alle eerdere waarden vervangt). Normaal gesproken worden apparaten op de hoogte gesteld van een beschikbare update, waarna een door de beheerder gedefinieerd beleid ervoor zorgt dat het apparaat de update downloadt en toepast. In deze functie moet de logica voor het inschakelen van dat beleid worden uitgevoerd. 
        
    ```csharp   
    static async Task waitToDownload(Twin twin, string fwUpdateUri)
    {
        var now = DateTime.Now;
        TwinCollection status = new TwinCollection();
        status["fwPackageUri"] = fwUpdateUri;
        status["status"] = "waiting";
        status["error"] = null;
        status["startedWaitingTime"] = DateTime.Now;
        status["downloadCompleteTime"] = null;
        status["startedApplyingImage"] = null;
        status["lastFirmwareUpdate"] = null;

        await reportFwUpdateThroughTwin(twin, status);

        await Task.Delay(2000);
    }
    ```

10. Voeg de volgende methode om uit te voeren van de download. Wordt de status bijgewerkt **downloaden** via het dubbele apparaat roept de downloadmethode simuleren en rapporteert de status van **downloadComplete** of **downloadFailed** via het dubbele afhankelijk van de resultaten van de downloadbewerking. 
        
    ```csharp   
    static async Task<byte[]> downloadImage(Twin twin, string fwUpdateUri)
    {
        try
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloading";
            await reportFwUpdateThroughTwin(twin, statusUpdate);

            byte[] imageData = await simulateDownloadImage(fwUpdateUri);

            statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloadComplete";
            statusUpdate["downloadCompleteTime"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            return imageData;
        }
        catch (Exception ex)
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloadFailed";
            statusUpdate["error"] = new TwinCollection();
            statusUpdate["error"]["code"] = ex.GetType().ToString();
            statusUpdate["error"]["message"] = ex.Message;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            throw;
        }
    }
    ```

11. Voeg de volgende methode voor het toepassen van de installatiekopie. Wordt de status bijgewerkt **toepassen** via het dubbele apparaat aanroepen van het simuleren van toepassing methode en de status van updates voor installatiekopie **applyComplete** of **applyFailed** via de dubbele afhankelijk van de resultaten van de bewerking toepassen. 
        
    ```csharp   
    static async Task applyImage(Twin twin, byte[] imageData)
    {
        try
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applying";
            statusUpdate["startedApplyingImage"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);

            await simulateApplyImage(imageData);

            statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applyComplete";
            statusUpdate["lastFirmwareUpdate"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
        }
        catch (Exception ex)
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applyFailed";
            statusUpdate["error"] = new TwinCollection();
            statusUpdate["error"]["code"] = ex.GetType().ToString();
            statusUpdate["error"]["message"] = ex.Message;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            throw;
        }
    }
    ```

12. De volgende methode toegevoegd voor het sequentiëren van de firmware-update-bewerking wachten om de installatiekopie via de installatiekopie wordt toegepast op het apparaat te downloaden:
        
    ```csharp   
    static async Task doUpdate(string fwUpdateUrl)
    {
        try
        {
            Twin twin = await Client.GetTwinAsync();
            await waitToDownload(twin, fwUpdateUrl);
            byte[] imageData = await downloadImage(twin, fwUpdateUrl);
            await applyImage(twin, imageData);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error during update: {0}", ex.Message);
        }
    }
    ```

13. Voeg de volgende methode voor het afhandelen van de **updateFirmware** directe methode vanuit de cloud. Het haalt de URL naar de firmware-update uit de berichtnettolading van het en doorgegeven aan de **doUpdate** taak, die op een andere threadpool thread wordt gestart. Deze retourneert onmiddellijk vervolgens het antwoord van de methode naar de cloud.
        
    ```csharp   
    static Task<MethodResponse> onFirmwareUpdate(MethodRequest methodRequest, object userContext)
    {
        string fwUpdateUrl = (string)JObject.Parse(methodRequest.DataAsJson)["fwPackageUri"];
        Console.WriteLine("\nMethod: {0} triggered by service, URI is: {1}", methodRequest.Name, fwUpdateUrl);

        Task updateTask = Task.Run(() => (doUpdate(fwUpdateUrl)));

        string result = "'FirmwareUpdate started.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }
    ```
> [!NOTE]
> Deze methode wordt geactiveerd voor de gesimuleerde update uit te voeren als een **taak** en vervolgens meteen reageert op de methodeaanroep, de service gemeld dat de firmware-update is gestart. Updatestatus en voltooiing zal worden verzonden naar de service via de gerapporteerde eigenschappen van het dubbele apparaat. Er wordt gereageerd op het aanroepen van de methode bij het starten van de update in plaats van na de voltooiing, omdat:
> * Een echte updateproces is zeer waarschijnlijk langer duurt dan de time-out van de aanroep van methode.
> * Een echte updateproces is het zeer waarschijnlijk om af te dwingen een opnieuw opstarten, die zou maken van deze app opnieuw starten de **MethodRequest** object niet beschikbaar. (Bijwerken van de gerapporteerde eigenschappen is echter mogelijk zelfs na het opnieuw opstarten.) 

14. Voeg de volgende code om de **Main** methode voor het openen van de verbinding met uw IoT-hub en de methode-listener initialiseren:
   
    ```csharp   
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
        
        // setup callback for "firmware update" method
        Client.SetMethodHandlerAsync("firmwareUpdate", onFirmwareUpdate, null).Wait();
        Console.WriteLine("Waiting for firmware update direct method call\n Press enter to exit.");
        Console.ReadLine();

        Console.WriteLine("Exiting...");

        // as a good practice, remove the firmware update handler
        Client.SetMethodHandlerAsync("firmwareUpdate", null, null).Wait();
        Client.CloseAsync().Wait();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
15. Bouw de oplossing.       

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).


## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.
1. De .NET-apparaat-app uitvoeren **SimulatedDeviceFWUpdate**.  Met de rechtermuisknop op de **SimulatedDeviceFWUpdate** project, selecteer **Debug**, en selecteer vervolgens **nieuwe instantie starten**. Deze moet beginnen met het luisteren naar methodeaanroepen vanuit uw IoT-Hub: 

2. Zodra het apparaat is verbonden en methodeaanroepen, wachten van de .NET uitvoeren **TriggerFWUpdate** app om aan te roepen van de firmware-update-methode in het gesimuleerde apparaat-app. Met de rechtermuisknop op de **TriggerFWUpdate** project, selecteer **Debug**, en selecteer vervolgens **nieuwe instantie starten**. U ziet nu bijwerken van de takenreeks die is geschreven in de **SimulatedDeviceFWUpdate** -console en ook de volgorde gerapporteerd via de gerapporteerde eigenschappen van het apparaat in de **TriggerFWUpdate** console. Houd er rekening mee dat het proces duurt enkele seconden om te voltooien. 
   
    ![Service- en apparaat-app uitvoeren][img-combinedrun]


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een rechtstreekse methode gebruikt voor het activeren van een externe firmware-update op een apparaat en gebruikt de gerapporteerde eigenschappen om te volgen van de voortgang van de firmware-update.

Zie voor informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten roept, de [taken plannen en uitzenden] [ lnk-tutorial-jobs] zelfstudie.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-firmware-update/servicesdknuget.png
[img-clientnuget]: media/iot-hub-csharp-csharp-firmware-update/clientsdknuget.png
[img-createserviceapp]: media/iot-hub-csharp-csharp-firmware-update/createserviceapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-firmware-update/createdeviceapp.png
[img-combinedrun]: media/iot-hub-csharp-csharp-firmware-update/combinedrun.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-csharp-csharp-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-csharp-csharp-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

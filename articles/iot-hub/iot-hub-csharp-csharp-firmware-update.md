---
title: Apparaat firmware-update met Azure IoT Hub (.NET/.NET) | Microsoft Docs
description: Klik hier voor meer informatie over het beheer van apparaten op Azure IoT Hub gebruiken om te zetten van een apparaat firmware-update. U kunt het Azure-IoT-apparaat-SDK voor .NET gebruiken voor het implementeren van een gesimuleerde apparaattoepassing en de Azure IoT service SDK voor .NET voor het implementeren van een service-app waarmee de firmware-update wordt geactiveerd.
services: iot-hub
documentationcenter: .net
author: JimacoMS2
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/19/2017
ms.author: v-jamebr
ms.openlocfilehash: bd0a227861d75dc66af8fb4865a17a3b6d0f70ba
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnet"></a>Apparaatbeheer gebruiken om te zetten van een apparaat firmware-update (.NET/.NET)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Inleiding
In de [aan de slag met Apparaatbeheer] [ lnk-dm-getstarted] zelfstudie, hebt u gezien hoe u de [apparaat twin] [ lnk-devtwin] en [methoden directe] [ lnk-c2dmethod] primitieven op afstand opnieuw opstarten van een apparaat. Deze zelfstudie gebruikt de dezelfde IoT Hub primitieven en ziet u hoe u een end-to-end gesimuleerde firmware-update.  Dit patroon wordt gebruikt in de firmware-update-implementatie voor de [frambozen Pi apparaat implementatie voorbeeld][lnk-rpi-implementation].

In deze handleiding ontdekt u hoe u:

* Maken van een .NET-consoletoepassing roept de **firmwareUpdate** directe methode in de gesimuleerde apparaattoepassing via uw IoT-hub.
* Maakt een gesimuleerd apparaat-app die u implementeert een **firmwareUpdate** directe methode. Deze methode initieert een meerdere fasen die wacht de firmware-installatiekopie te downloaden, de installatiekopie van het firmware downloaden en ten slotte is de installatiekopie van de firmware van toepassing. In elke fase van de update wordt het apparaat de gerapporteerde eigenschappen gebruikt om te rapporteren over de voortgang.

Aan het einde van deze zelfstudie hebt u een .NET (C#)-consoletoepassing apparaat en een back-end-.NET (C#) console-app:

* **SimulatedDeviceFwUpdate**, die verbinding maakt met uw IoT-hub aan de apparaat-id eerder hebt gemaakt, ontvangt de **firmwareUpdate** directe methode, wordt uitgevoerd via een proces met meerdere status om te simuleren een firmware-update inclusief: wachten op voor het downloaden van de installatiekopie, downloaden van de nieuwe installatiekopie en ten slotte de installatiekopie toe te passen.

* **TriggerFWUpdate**, waarbij de SDK-service wordt gebruikt om aan te roepen op afstand de **firmwareUpdate** directe methode voor het gesimuleerde apparaat, wordt het antwoord weergegeven en wordt regelmatig (elke 500ms) geeft de bijgewerkte gerapporteerd Eigenschappen.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

Ga als volgt de [aan de slag met Apparaatbeheer](iot-hub-csharp-csharp-device-management-get-started.md) artikel voor het maken van uw IoT-hub en uw IoT Hub-verbindingsreeks ophalen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Activeren van een externe firmware-update op het apparaat met een directe methode
In deze sectie maakt u een .NET-consoletoepassing (met C#) die een externe firmware-update op een apparaat start. De app gebruikmaakt van een directe methode om de update te initiëren en apparaat twin query's gebruikt om het periodiek de status van de actieve firmware-update niet ophalen.

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

5. Voeg de volgende velden toe aan de klasse **Program**: De meerdere tijdelijke aanduiding voor waarden vervangt door de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de vorige sectie en de ID van uw apparaat.
   
    ```csharp   
    static RegistryManager registryManager;
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static string targetDevice = "{deviceIdForTargetDevice}";
    ```
        
6. Voeg de volgende methode voor de **programma** klasse. Deze methode, controleert of de apparaat-twin om de bijgewerkte status elke 500 milliseconden. Geschreven naar de console alleen wanneer de status is gewijzigd. Voor dit voorbeeld, om te voorkomen dat het verbruik van extra IoT Hub berichten in uw abonnement polling stopt wanneer het apparaat de status van meldt **applyComplete** of een fout.  
   
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

8. Voeg de volgende regels voor de **Main** methode. Dit maakt een register om te lezen van het apparaat twin met, begint de polling-taak op een werkthread en vervolgens de firmware-update wordt geactiveerd.
   
    ```csharp   
    registryManager = RegistryManager.CreateFromConnectionString(connString);

    Task queryTask = Task.Run(() => (QueryTwinFWUpdateReported(DateTime.Now)));

    StartFirmwareUpdate().Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```
        
9. De oplossing bouwen.

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie doet u het volgende:

* Een .NET-consoletoepassing dat met een directe methode is aangeroepen door de cloud overeenkomt maken
* Een firmware-update geactiveerd door een back-endservice via een directe methode simuleren
* U gebruikt de gerapporteerde eigenschappen om apparaatdubbelquery's in te schakelen die de apparaten identificeren en vaststellen wanneer er voor het laatst een firmware-update op deze apparaten is uitgevoerd.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **SimulateDeviceFWUpdate**.
   
    ![Nieuwe Visual C# klassieke Windows-apparaat-app][img-createdeviceapp]
    
2. Klik in Solution Explorer met de rechtermuisknop op de **SimulateDeviceFWUpdate** project en klik vervolgens op **NuGet-pakketten beheren**.
3. In de **NuGet Package Manager** Selecteer **Bladeren** en zoek naar **microsoft.azure.devices.client**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices.Client** Inpakken en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT-device SDK] [ lnk-nuget-client-sdk] NuGet-pakket en de bijbehorende afhankelijkheden.
   
    ![NuGet-Pakketbeheer venster Client-app][img-clientnuget]
4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
    ```csharp   
    using Newtonsoft.Json.Linq;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de verbindingsreeks voor apparaten die u hebt genoteerd in de **maken van een apparaat-id** sectie.
   
    ```csharp   
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. Voeg de volgende methode toe voor statusrapportage terug naar de cloud via de apparaat-twin: 

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

7. Voeg de volgende methode om te simuleren downloaden van de firmware-installatiekopie:
        
    ```csharp   
    static async Task<byte[]> simulateDownloadImage(string imageUrl)
    {
        var image = "[fake image data]";

        Console.WriteLine("Downloading image from " + imageUrl);

        await Task.Delay(4000);

        return Encoding.ASCII.GetBytes(image);
            
    }
    ```

8. De volgende methode om te simuleren de firmware-installatiekopie wordt toegepast op het apparaat toevoegen:
        
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
 
9.  Voeg de volgende methode om te simuleren wachten op de firmware-installatiekopie te downloaden. Status bijwerken naar **wachten** en andere eigenschappen van de firmware bijwerken op de twin wissen. Deze eigenschappen zijn uitgeschakeld om te verwijderen van een bestaande waarden uit eerdere firmware-updates. Dit is nodig omdat gemelde-eigenschappen worden verzonden als een PATCH-bewerking (delta) en niet een PUT-bewerking (een volledige set eigenschappen die alle van de vorige waarden vervangt). Normaal gesproken worden apparaten op de hoogte gesteld van een beschikbare update, waarna een door de beheerder gedefinieerd beleid ervoor zorgt dat het apparaat de update downloadt en toepast. In deze functie moet de logica voor het inschakelen van dat beleid worden uitgevoerd. 
        
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

10. Voeg de volgende methode om uit te voeren van de download. De status om te worden bijgewerkt **downloaden** via de apparaat-twin roept de methode van de download simuleren en rapporteert de status van **downloadComplete** of **downloadFailed** via de twin afhankelijk van de resultaten van de downloadbewerking. 
        
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

11. Voeg de volgende methode om de installatiekopie toe te passen. De status om te worden bijgewerkt **toepassen** via de apparaat-twin aanroepen de simuleren toepassen methode en de status van de updates voor installatiekopie **applyComplete** of **applyFailed** via de Twin afhankelijk van de resultaten van de bewerking toepassen. 
        
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

12. Voeg de volgende methode voor het sequentiëren van de firmware-update-bewerking van het wachten op de installatiekopie via de installatiekopie wordt toegepast op het apparaat te downloaden:
        
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

13. Voeg de volgende methode voor het afhandelen van de **updateFirmware** directe methode vanuit de cloud. De URL naar de firmware-update wordt geëxtraheerd uit de nettolading van het bericht en wordt doorgegeven aan de **doUpdate** taak die op een andere threadpool thread wordt gestart. Vervolgens wordt onmiddellijk het antwoord van de methode op de cloud.
        
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
> Deze methode wordt geactiveerd voor de gesimuleerde update uit te voeren als een **taak** en vervolgens onmiddellijk reageert op de methodeaanroep waarin de service worden geïnformeerd dat de firmware-update is gestart. Updatestatus en voltooiing zal worden verzonden naar de service via de gerapporteerde eigenschappen van het apparaat twin. We reageren op de methodeaanroep bij het starten van de update, in plaats van na de voltooiing omdat:
> * Een echte updateproces is het zeer waarschijnlijk langer duurt dan de time-out van de aanroep van methode.
> * Een echte updateproces is het zeer waarschijnlijk een opnieuw opstarten, die u zou deze app bij opnieuw opstarten vereist de **MetodRequest** object is niet beschikbaar. (Gemelde eigenschappen bijwerken is echter mogelijk zelfs na het opnieuw opstarten.) 

14. Voeg de volgende code naar de **Main** methode voor het openen van de verbinding met uw IoT-hub en de methode-listener initialiseren:
   
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
        
15. De oplossing bouwen.       

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).


## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.
1. Voer de app voor .NET-apparaat **SimulatedDeviceFWUpdate**.  Met de rechtermuisknop op de **SimulatedDeviceFWUpdate** project, selecteer **Debug**, en selecteer vervolgens **nieuw exemplaar gestart**. Deze zou moeten starten luisteren naar methodeaanroepen van uw IoT-Hub: 

2. Zodra het apparaat is verbonden en wachten op methode-aanroepen uitgevoerd .NET **TriggerFWUpdate** app de firmware-update-methode in de gesimuleerde apparaattoepassing aan te roepen. Met de rechtermuisknop op de **TriggerFWUpdate** project, selecteer **Debug**, en selecteer vervolgens **nieuw exemplaar gestart**. U ziet bijwerken sequence geschreven in de **SimulatedDeviceFWUpdate** -console en ook de volgorde gerapporteerd via de gerapporteerde eigenschappen van het apparaat in de **TriggerFWUpdate** console. Houd er rekening mee dat dit enkele seconden duurt worden voltooid. 
   
    ![Service- en app uitvoeren][img-combinedrun]


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een directe methode gebruikt voor het activeren van een externe firmware-update op een apparaat en de gerapporteerde eigenschappen gebruikt voor het bijhouden van de voortgang van de firmware-update.

Zie voor meer informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten aanroepen, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-firmware-update/servicesdknuget.png
[img-clientnuget]: media/iot-hub-csharp-csharp-firmware-update/clientsdknuget.png
[img-createserviceapp]: media/iot-hub-csharp-csharp-firmware-update/createserviceapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-firmware-update/createdeviceapp.png
[img-combinedrun]: media/iot-hub-csharp-csharp-firmware-update/combinedrun.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-csharp-csharp-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-csharp-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
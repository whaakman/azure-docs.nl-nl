---
title: Gebruik Azure IoT Hub apparaat twin eigenschappen (.NET/.NET) | Microsoft Docs
description: Het gebruik van Azure IoT Hub apparaat horende apparaten configureren. U het apparaat met Azure IoT SDK voor .NET gebruiken voor het implementeren van een gesimuleerde apparaattoepassing en de Azure IoT service SDK voor .NET voor het implementeren van een service-app die de apparaatconfiguratie van een met behulp van een apparaat-twin wijzigt.
services: iot-hub
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dkshir
ms.openlocfilehash: 0bb18f1b289dd01866842a5193437dabdb5fd20b
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="use-desired-properties-to-configure-devices"></a>Gewenste eigenschappen gebruiken om apparaten te configureren
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Aan het einde van deze zelfstudie hebt u twee apps voor .NET-console:

* **SimulateDeviceConfiguration**, een gesimuleerde apparaattoepassing die wordt gewacht op een gewenste configuratie-update en rapporteert de status van een gesimuleerde configuratie updateproces.
* **SetDesiredConfigurationAndQuery**, een back-end-app, die de gewenste configuratie ingesteld op een apparaat en het configuratieproces van de update-query's.

> [!NOTE]
> Het artikel [Azure IoT SDK's] [ lnk-hub-sdks] bevat informatie over de Azure IoT SDK's dat u gebruiken kunt om zowel apparaatgegevens als back-end-apps te bouwen.
> 
> 

Voor deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.

Als u hebt gevolgd de [aan de slag met apparaat horende] [ lnk-twin-tutorial] zelfstudie, u hebt al een IoT-hub en een apparaat-id genoemd **myDeviceId**. In dat geval kunt u doorgaan met de [de gesimuleerde apparaattoepassing maken] [ lnk-how-to-configure-createapp] sectie.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>De gesimuleerde apparaattoepassing maken
In deze sectie maakt u een .NET-consoletoepassing die is verbonden met uw hub als **myDeviceId**, wordt gewacht op een gewenste configuratie-update en vervolgens rapporten updates op het updateproces gesimuleerde configuratie.

1. Maak in Visual Studio een nieuw Visual C# Classic Windows Desktop-project met behulp van de **consoletoepassing** projectsjabloon. Noem het project **SimulateDeviceConfiguration**.
   
    ![Nieuwe Visual C# klassieke Windows-apparaat-app][img-createdeviceapp]

1. Klik in Solution Explorer met de rechtermuisknop op de **SimulateDeviceConfiguration** project en klik vervolgens op **NuGet-pakketten beheren...** .
1. In de **NuGet Package Manager** Selecteer **Bladeren** en zoek naar **microsoft.azure.devices.client**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices.Client** Inpakken en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT-device SDK] [ lnk-nuget-client-sdk] NuGet-pakket en de bijbehorende afhankelijkheden.
   
    ![NuGet-Pakketbeheer venster Client-app][img-clientnuget]
1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de verbindingsreeks voor apparaten die u in de vorige sectie hebt genoteerd.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. Voeg de volgende methode toe aan de klasse **Program**:
 
        public static void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting to hub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }
    De **Client** object bevat de methoden die u nodig hebt om te communiceren met horende apparaten van het apparaat. De hierboven weergegeven code initialiseert de **Client** object, en vervolgens haalt de apparaat-twin voor **myDeviceId**.

1. Voeg de volgende methode voor de **programma** klasse. Deze methode stelt de oorspronkelijke waarden van telemetrie op het lokale apparaat en werkt vervolgens de apparaat-twin.

        public static async void InitTelemetry()
        {
            try
            {
                Console.WriteLine("Report initial telemetry config:");
                TwinCollection telemetryConfig = new TwinCollection();
                
                telemetryConfig["configId"] = "0";
                telemetryConfig["sendFrequency"] = "24h";
                reportedProperties["telemetryConfig"] = telemetryConfig;
                Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Voeg de volgende methode voor de **programma** klasse. Dit is een terugbelactie die een wijziging in detecteert *gewenst eigenschappen* in de apparaat-twin.

        private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            try
            {
                Console.WriteLine("Desired property change:");
                Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                var currentTelemetryConfig = reportedProperties["telemetryConfig"];
                var desiredTelemetryConfig = desiredProperties["telemetryConfig"];

                if ((desiredTelemetryConfig != null) && (desiredTelemetryConfig["configId"] != currentTelemetryConfig["configId"]))
                {
                    Console.WriteLine("\nInitiating config change");
                    currentTelemetryConfig["status"] = "Pending";
                    currentTelemetryConfig["pendingConfig"] = desiredTelemetryConfig;

                    await Client.UpdateReportedPropertiesAsync(reportedProperties);

                    CompleteConfigChange();
                }
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    Deze methode werkt de gerapporteerde eigenschappen op het lokale apparaat twin-object met de updateaanvraag configuratie en wordt de status ingesteld op **in behandeling**, werkt u vervolgens de twin apparaat van de service. Nadat het apparaat twin is bijgewerkt, de configuratiewijziging is voltooid door het aanroepen van de methode `CompleteConfigChange` beschreven in het volgende punt.

1. Voeg de volgende methode voor de **programma** klasse. Deze methode wordt gesimuleerd apparaat opnieuw instellen en vervolgens updates de lokale gerapporteerd eigenschappen van de status is ingesteld op **geslaagd** en verwijdert u de **pendingConfig** element. De apparaat-twin op de service wordt vervolgens bijgewerkt. 

        public static async void CompleteConfigChange()
        {
            try
            {
                var currentTelemetryConfig = reportedProperties["telemetryConfig"];

                Console.WriteLine("\nSimulating device reset");
                await Task.Delay(30000); 

                Console.WriteLine("\nCompleting config change");
                currentTelemetryConfig["configId"] = currentTelemetryConfig["pendingConfig"]["configId"];
                currentTelemetryConfig["sendFrequency"] = currentTelemetryConfig["pendingConfig"]["sendFrequency"];
                currentTelemetryConfig["status"] = "Success";
                currentTelemetryConfig["pendingConfig"] = null;

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
                Console.WriteLine("Config change complete \nPress any key to exit.");
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Voeg ten slotte de volgende regels voor de **Main** methode:

        try
        {
            InitClient();
            InitTelemetry();

            Console.WriteLine("Wait for desired telemetry...");
            Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();
            Console.ReadKey();
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }

   > [!NOTE]
   > Deze zelfstudie wordt een gedrag voor updates voor gelijktijdige configuratie niet simuleren. Bepaalde configuratie-update-processen mogelijk wijzigingen van de doelconfiguratie voor terwijl de update wordt uitgevoerd, enkele mogelijk moet u ze in de wachtrij en sommige met een fout opgetreden weigeren kan. Zorg ervoor dat rekening houden met het gewenste gedrag voor uw specifieke configuratie-proces en de juiste logica toevoegen voordat u begint de wijziging in de configuratie.
   > 
   > 
1. De oplossing bouwen en voer de app apparaat vanuit Visual Studio door te klikken op **F5**. U ziet in de uitvoer-console de berichten die aangeeft dat het gesimuleerde apparaat van het apparaat twin ophalen, instellen van de Telemetrie en wachten op gewenste eigenschap wijzigen. De app actief houden.

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
   
    De **register** object bevat de methoden die zijn vereist voor interactie met horende apparaten van de service. Deze code initialiseert de **register** object, haalt de apparaat-twin voor **myDeviceId**, en werkt vervolgens de gewenste eigenschappen met een nieuwe telemetrie configuration-object.
    Daarna wordt deze query's van het apparaat horende opgeslagen in de IoT-hub elke 10 seconden en de gewenste en gerapporteerde telemetrie-configuraties worden afgedrukt. Raadpleeg de [IoT Hub-querytaal] [ lnk-query] voor informatie over het uitgebreide om rapporten te genereren in al uw apparaten.
   
   > [!IMPORTANT]
   > Deze toepassing een IoT Hub query elke 10 seconden ter illustratie. Met query's gebruikersgerichte om rapporten te genereren over verschillende apparaten en niet voor het detecteren van wijzigingen. Als uw oplossing realtime meldingen over apparaatgebeurtenissen vereist, gebruikt u [twin meldingen][lnk-twin-notifications].
   > 
   > 
1. Voeg tot slot de volgende regels toe aan de methode **Main**:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. Open in Solution Explorer de **opstartprojecten instellen...**  en zorg ervoor dat de **actie** voor **SetDesiredConfigurationAndQuery** project **Start**. De oplossing bouwen.
1. Met **SimulateDeviceConfiguration** apparaat app wordt uitgevoerd, de service-app uitvoeren vanuit Visual Studio met **F5**. Ziet u de configuratie van de gerapporteerde gewijzigd van **in behandeling** naar **geslaagd** verzenden met de nieuwe actieve frequentie van vijf minuten in plaats van 24 uur.

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
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/devicesdknuget.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app

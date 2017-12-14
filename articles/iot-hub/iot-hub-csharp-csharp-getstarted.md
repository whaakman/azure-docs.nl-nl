---
title: Aan de slag met Azure IoT Hub (.NET) | Microsoft Docs
description: Informatie over het verzenden van apparaat-naar-cloud-berichten naar Azure IoT Hub met behulp van IoT SDK's voor .NET. U maakt gesimuleerde apparaat- en service-apps om uw apparaat te registreren, berichten te verzenden en berichten uit IoT Hub te lezen.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ec9ff4a866ef8736ed260b4d17aa997433c1ef8d
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-net"></a>Uw apparaat verbinding laten maken met uw IoT Hub met .NET

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Aan het eind van deze zelfstudie beschikt u over drie .NET-consoletoepassingen:

* **CreateDeviceIdentity**: deze toepassing maakt een apparaat-id en de bijbehorende beveiligingssleutel waarmee uw apparaat-app kan worden verbonden.
* **ReadDeviceToCloudMessages**: deze toepassing geeft de telemetrie weer die is verzonden door uw apparaat-app.
* **SimulatedDevice**: deze toepassing koppelt uw IoT-hub aan de apparaat-id die u eerder hebt gemaakt en verzendt iedere seconde een telemetriebericht via het MQTT-protocol.

U kunt de Visual Studio-oplossing downloaden of klonen om zo de beschikking te krijgen over de drie apps van Github.

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> Raadpleeg het artikel [Azure IoT-SDKs][lnk-hub-sdks] voor meer informatie over de verschillende Azure IoT-SDK's die u kunt gebruiken om beide toepassingen zo te maken dat ze zowel op het apparaat als op de back-end van uw oplossing kunnen worden uitgevoerd.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

U hebt nu uw IoT Hub gemaakt en u hebt de hostnaam en de IoT Hub-verbindingsreeks die u nodig hebt voor de rest van deze handleiding.

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten ontvangen
In dit gedeelte maakt u een .NET-consoletoepassing die apparaat-naar-cloud-berichten uit IoT Hub kan lezen. Een IoT-hub toont een [Azure Event Hubs][lnk-event-hubs-overview]-compatibel eindpunt waarmee u apparaat-naar-cloud-berichten kunt lezen. Om de zaken niet nodeloos ingewikkeld te maken, maakt u met deze handleiding een basislezer die niet geschikt is voor hoge doorvoersnelheden. In de zelfstudie [Apparaat-naar-cloud-berichten verwerken][lnk-process-d2c-tutorial] leert u hoe u op grote schaal apparaat-naar-cloud-berichten kunt verwerken. Zie voor meer informatie over het verwerken van Event Hubs-berichten de zelfstudie [Aan de slag met Event Hubs][lnk-eventhubs-tutorial]. (Deze zelfstudie is van toepassing op eindpunten die compatibel zijn met event hubs in IoT Hub.)

> [!NOTE]
> Het met Event Hub compatibele eindpunt voor het lezen van apparaat-naar-cloud-berichten maakt altijd gebruik van het AMQP-protocol.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console App (.NET Framework)**. Zorg ervoor dat de versie van .NET Framework minimaal 4.5.1 is. Noem het project **ReadDeviceToCloudMessages**.

    ![Nieuw Windows Classic Desktop-project in Visual C#][10a]

2. Klik in Solution Explorer met de rechtermuisknop op het project **ReadDeviceToCloudMessages** en klik vervolgens op **Manage NuGet Packages**.

3. In het venster **NuGet Package Manager** zoekt u **WindowsAzure.ServiceBus**. Accepteer de gebruikersvoorwaarden en klik op **Install**. Met deze procedure worden [Azure Service Bus][lnk-servicebus-nuget] en de bijbehorende afhankelijkheden gedownload en geïnstalleerd. Ook worden verwijzingen hiernaar toegevoegd. Met dit pakket kan de toepassing verbinding maken met het eindpunt dat compatibel is met event hubs op uw IoT-hub.

4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke-aanduidingswaarde met de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de sectie Een IoT-hub maken.

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp
    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested) break;
            EventData eventData = await eventHubReceiver.ReceiveAsync();
            if (eventData == null) continue;

            string data = Encoding.UTF8.GetString(eventData.GetBytes());
            Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
        }
    }
    ```

    Deze methode gebruikt een exemplaar van **EventHubReceiver** om berichten te ontvangen van alle apparaat-naar-cloud ontvangstpartities van de IoT-hub. U geeft nu een `DateTime.Now` parameter door bij het maken van het object **EventHubReceiver**, zodat alleen berichten worden ontvangen die zijn verzonden nadat het object is gestart. Dit filter is handig in een testomgeving, omdat u zo de huidige reeks berichten kunt zien. In een productieomgeving moet de code ervoor zorgen dat alle berichten worden verwerkt. Zie voor meer informatie de zelfstudie [IoT Hub-apparaat-naar-cloud-berichten verwerken][lnk-process-d2c-tutorial].

7. Voeg tot slot de volgende regels toe aan de methode **Main**:

    ```csharp
    Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
    eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

    var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

    CancellationTokenSource cts = new CancellationTokenSource();

    System.Console.CancelKeyPress += (s, e) =>
    {
        e.Cancel = true;
        cts.Cancel();
        Console.WriteLine("Exiting...");
    };

    var tasks = new List<Task>();
    foreach (string partition in d2cPartitions)
    {
        tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
    }  
    Task.WaitAll(tasks.ToArray());
    ```

## <a name="create-a-device-app"></a>Een apparaat-app maken

In deze sectie maakt u een .NET-consoletoepassing die een apparaat simuleert dat apparaat-naar-cloud-berichten naar een IoT Hub verzendt.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console App (.NET Framework)**. Zorg ervoor dat de versie van .NET Framework minimaal 4.5.1 is. Noem het project **SimulatedDevice**.

    ![Nieuw Windows Classic Desktop-project in Visual C#][10b]

2. Klik in Solution Explorer met de rechtermuisknop op het project **SimulatedDevice** en klik vervolgens op **Manage NuGet Packages**.

3. Klik in de **NuGet Package Manager** op **Browse** en zoek naar **Microsoft.Azure.Devices.Client**. Accepteer de gebruiksvoorwaarden en klik op **Install** om het **Microsoft.Azure.Devices.Client**-pakket te installeren. Met deze procedure worden het [Azure IoT Device SDK NuGet-pakket][lnk-device-nuget] en de bijbehorende afhankelijkheden gedownload en geïnstalleerd. Ook worden verwijzingen hiernaar toegevoegd.

4. Voeg aan het begin van het bestand **Program.cs** de volgende `using`-instructie toe:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang `{iot hub hostname}` met de hostnaam van de IoT-hub die u in de sectie 'Een IoT-hub maken' hebt opgehaald. Vervang `{device key}` door de apparaatsleutel die u in de sectie 'Een apparaatidentiteit maken' hebt opgehaald.

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp
    private static async void SendDeviceToCloudMessagesAsync()
    {
        double minTemperature = 20;
        double minHumidity = 60;
        int messageId = 1;
        Random rand = new Random();

        while (true)
        {
            double currentTemperature = minTemperature + rand.NextDouble() * 15;
            double currentHumidity = minHumidity + rand.NextDouble() * 20;

            var telemetryDataPoint = new
            {
                messageId = messageId++,
                deviceId = "myFirstDevice",
                temperature = currentTemperature,
                humidity = currentHumidity
            };
            var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
            var message = new Message(Encoding.ASCII.GetBytes(messageString));
            message.Properties.Add("temperatureAlert", (currentTemperature > 30) ? "true" : "false");

            await deviceClient.SendEventAsync(message);
            Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
        }
    }
    ```

    Met deze methode wordt elke seconde een nieuw apparaat-naar-cloud bericht verzonden. Het bericht bevat een JSON-geserialiseerd object met de apparaat-id en willekeurig gegenereerde nummers om een temperatuursensor te simuleren, en een vochtigheidssensor.

7. Voeg tot slot de volgende regels toe aan de methode **Main**:

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);
    deviceClient.ProductInfo = "HappyPath_Simulated-CSharp";
    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    Met de **Create**-methode in een .NET Framework-app maakt u standaard een **DeviceClient**-exemplaar dat het AMQP-protocol gebruikt om te communiceren met IoT Hub. Als u het MQTT- of HTTPS-protocol wilt gebruiken, moet u de **Create**-methode overschrijven. Zo kunt u zelf het protocol bepalen. UWP- en PCL-clients gebruiken standaard het HTTPS-protocol. Als u het HTTPS-protocol gebruikt, moet u ook het NuGet-pakket **Microsoft.AspNet.WebApi.Client** toevoegen om uw project op te nemen in de naamruimte **System.Net.Http.Formatting**.

In deze handleiding doorloopt u de stappen voor het maken van een IoT Hub-apparaat-app. U kunt ook de Visual Studio-extensie [Connected Service for Azure IoT Hub][lnk-connected-service] gebruiken om de benodigde code toe te voegen aan de apparaat-app.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. Klik in Solution Explorer, in Visual Studio, met de rechtermuisknop op uw oplossing en klik vervolgens op **Set StartUp projects**. Klik op **Multiple startup projects** en klik vervolgens op **Start** als de actie voor beide projecten **ProcessDeviceToCloudMessages** en **SimulatedDevice**.

    ![Eigenschappen van opstartprojecten][41]

2. Druk op **F5** om beide apps uit te voeren. De console-uitvoer van de **SimulatedDevice**-app toont de berichten die uw apparaat-app verzendt naar uw IoT-hub. De console-uitvoer van de **ReadDeviceToCloudMessages**-app toont de berichten die uw IoT-hub ontvangt.

    ![Console-uitvoer van apps][42]

3. De tegel **Gebruik** in [Azure Portal][lnk-portal] toont het aantal berichten dat is verzonden naar de IoT-hub:

    ![Tegel Usage in Azure Portal][43]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt deze apparaat-id gebruikt om de apparaat-app in staat te stellen apparaat-naar-cloud-berichten te verzenden naar de IoT-hub. Ook hebt u een app gemaakt die de berichten weergeeft die worden ontvangen door de IoT-hub.

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Verbinding maken met uw apparaat][lnk-connect-device]
* [Aan de slag met apparaatbeheer][lnk-device-management]
* [Aan de slag met IoT Edge][lnk-iot-edge]

Raadpleeg de zelfstudie [Apparaat-naar-cloud-berichten verwerken][lnk-process-d2c-tutorial] voor meer informatie over hoe u uw IoT-oplossing uitbreidt en apparaat-naar-cloud-berichten op schaal verwerkt.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10a]: ./media/iot-hub-csharp-csharp-getstarted/create-receive-csharp1.png
[10b]: ./media/iot-hub-csharp-csharp-getstarted/create-device-csharp1.png


<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

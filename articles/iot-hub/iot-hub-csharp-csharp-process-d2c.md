---
title: Azure IoT Hub apparaat-naar-cloud-berichten met behulp van routes (.Net) | Microsoft Docs
description: Klik hier voor meer informatie over het verwerken van IoT Hub apparaat-naar-cloud-berichten met behulp van regels voor het doorsturen en aangepaste eindpunten verzending van berichten naar andere back-end-services.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 1d2b52ea005ab520bf294efa603512c00a92ee63
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a>Verwerken van Iothub apparaat-naar-cloud-berichten met behulp van routes (.NET)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Deze zelfstudie bouwt voort op de [aan de slag met IoT Hub] zelfstudie. De zelfstudie:

* Laat zien hoe u regels voor doorsturen verzending van apparaat-naar-cloud-berichten in een eenvoudig en gebaseerd op configuratie-hulpprogramma te gebruiken.
* Laat zien hoe isoleren interactieve berichten die directe actie van de back-end oplossing voor verdere verwerking is vereist. Een apparaat kan bijvoorbeeld een waarschuwing weergegeven dat activeert een ticket in een CRM-systeem invoegen verzenden. Daarentegen feed gegevenspunt berichten, zoals temperatuur telemetrie, in een analyse-engine.

Aan het einde van deze zelfstudie, moet u drie console .NET-toepassingen uitvoeren:

* **SimulatedDevice**, een aangepaste versie van de app gemaakt in de [aan de slag met IoT Hub] zelfstudie verzendt gegevenspunt apparaat-naar-cloud-berichten per seconde en interactieve apparaat-naar-cloud met berichten voor elke 10 seconden.
* **ReadDeviceToCloudMessages** waarmee de niet-kritieke telemetrie verzonden door de app op uw apparaat worden weergegeven.
* **ReadCriticalQueue** ongedaan wachtrijen de kritieke berichten is verzonden door uw app voor het apparaat van een Service Bus-wachtrij. Deze wachtrij is gekoppeld aan de IoT-hub.

> [!NOTE]
> IoT Hub heeft ondersteuning voor veel apparaatplatforms en talen, waaronder C, Java en JavaScript SDK. Zie voor meer informatie over het gesimuleerde apparaat in deze zelfstudie vervangen door een fysiek apparaat, de [Azure IoT Developer Center].

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. <br/>Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) binnen een paar minuten.

Er is enige basiskennis van [Azure Storage] en [Azure Service Bus].

## <a name="send-interactive-messages"></a>Interactieve berichten verzenden

Wijzigen van de apparaat-app die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie af en interactieve om berichten te verzenden.

In Visual Studio in de **SimulatedDevice** project, vervangen door de `SendDeviceToCloudMessagesAsync` methode met de volgende code:

```csharp
private static async void SendDeviceToCloudMessagesAsync()
{
    double minTemperature = 20;
    double minHumidity = 60;
    Random rand = new Random();

    while (true)
    {
        double currentTemperature = minTemperature + rand.NextDouble() * 15;
        double currentHumidity = minHumidity + rand.NextDouble() * 20;

        var telemetryDataPoint = new
        {
            deviceId = "myFirstDevice",
            temperature = currentTemperature,
            humidity = currentHumidity
        };
        var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
        string levelValue;

        if (rand.NextDouble() > 0.7)
        {
            messageString = "This is a critical message";
            levelValue = "critical";
        }
        else
        {
            levelValue = "normal";
        }
        
        var message = new Message(Encoding.ASCII.GetBytes(messageString));
        message.Properties.Add("level", levelValue);
        
        await deviceClient.SendEventAsync(message);
        Console.WriteLine("{0} > Sent message: {1}", DateTime.Now, messageString);

        await Task.Delay(1000);
    }
}
```

Deze methode wordt willekeurig toegevoegd voor de eigenschap `"level": "critical"` op berichten die door het apparaat verzonden, die een bericht dat directe actie is vereist door de oplossing voor back-end simuleert. De apparaattoepassing geeft deze informatie in de berichteigenschappen in plaats van in de hoofdtekst van het bericht, zodat deze IoT Hub het bericht naar de juiste bestemming sturen kan.

> [!NOTE]
> U kunt de berichteigenschappen om berichten te routeren voor verschillende scenario's, inclusief de verwerking van koude pad, naast het hot pad voorbeeld dat hier wordt weergegeven.

> [!NOTE]
> Omwille van de eenvoud in deze zelfstudie niet geïmplementeerd voor een beleid voor opnieuw proberen. In productiecode moet u een beleid voor opnieuw proberen zoals exponentieel uitstel, zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout].

## <a name="route-messages-to-a-queue-in-your-iot-hub"></a>Routeren van berichten aan een wachtrij in uw IoT-hub

In deze sectie doet u het volgende:

* Een Service Bus-wachtrij maken.
* Verbindt dit met uw IoT-hub.
* Configureer uw IoT-hub om berichten te verzenden naar de wachtrij op basis van de aanwezigheid van een eigenschap van het bericht.

Zie voor meer informatie over het verwerken van berichten van Service Bus-wachtrijen [aan de slag met wachtrijen][Service Bus queue].

1. Een Service Bus-wachtrij maakt, zoals beschreven in [aan de slag met wachtrijen][Service Bus queue]. De wachtrij moet zich in hetzelfde abonnement en dezelfde regio als uw IoT-hub. Noteer de naam van de naamruimte en de wachtrij.

    > [!NOTE]
    > Service Bus-wachtrijen en onderwerpen die worden gebruikt als IoT-hubeindpunten mag geen **sessies** of **detectie van dubbele** ingeschakeld. Als een van deze opties zijn ingeschakeld, het eindpunt wordt weergegeven als **onbereikbaar** in de Azure portal.

2. Open uw IoT-hub in de Azure-portal en klikt u op **eindpunten**.
    
    ![Eindpunten van IoT-hub][30]

3. In de **eindpunten** blade, klikt u op **toevoegen** boven uw wachtrij toevoegen aan uw IoT-hub. Naam van het eindpunt **CriticalQueue** en selecteer met de vervolgkeuzelijsten **Service Bus-wachtrij**, de Service Bus-naamruimte waarin de wachtrij zich bevindt en de naam van de wachtrij. Wanneer u klaar bent, klikt u op **opslaan** onderaan.
    
    ![Een eindpunt toevoegen][31]
    
4. Klik nu op **Routes** in uw IoT-Hub. Klik op **toevoegen** boven aan de blade voor het maken van een regel voor doorsturen waarmee berichten worden doorgestuurd naar de wachtrij die u zojuist hebt toegevoegd. Selecteer **DeviceTelemetry** als de bron van gegevens. Voer `level="critical"` als de voorwaarde, en kies de wachtrij die u zojuist hebt toegevoegd als een aangepaste eindpunt als de routering eindpunt van de regel. Wanneer u klaar bent, klikt u op **opslaan** onderaan.
    
    ![Toevoegen van een route][32]
    
    Zorg ervoor dat de terugval route is ingesteld op **ON**. Deze waarde is de standaardconfiguratie voor een IoT-hub.
    
    ![Route voor terugval][33]

## <a name="read-from-the-queue-endpoint"></a>Lezen van het eindpunt van de wachtrij

In deze sectie kunt u de berichten van het eindpunt van de wachtrij lezen.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console App (.NET Framework)**. Noem het project **ReadCriticalQueue**.

2. Klik in Solution Explorer met de rechtermuisknop op de **ReadCriticalQueue** project en klik vervolgens op **NuGet-pakketten beheren**. Met deze bewerking wordt de **NuGet Package Manager** venster.

3. Zoeken naar **WindowsAzure.ServiceBus**, klikt u op **installeren**, en accepteer de gebruiksvoorwaarden. Deze bewerking downloadt, installeert en voegt een verwijzing naar de Azure Service Bus, inclusief alle afhankelijkheden ervan.

4. Voeg de volgende **met** instructies aan het begin van de **Program.cs** bestand:
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Voeg de volgende regels voor de **Main** methode. Vervang de verbindingsreeks met **luisteren** machtigingen voor de wachtrij:
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C to exit.\n");
    var connectionString = "{service bus listen string}";
    var queueName = "{queue name}";
    
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
        {
            Stream stream = message.GetBody<Stream>();
            StreamReader reader = new StreamReader(stream, Encoding.ASCII);
            string s = reader.ReadToEnd();
            Console.WriteLine(String.Format("Message body: {0}", s));
        });
        
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. In Visual Studio in Solution Explorer met de rechtermuisknop op uw oplossing en selecteer **Opstartprojecten instellen**. Selecteer **meerdere opstartprojecten**, selecteer daarna **Start** als de actie voor de **ReadDeviceToCloudMessages**, **SimulatedDevice**, en **ReadCriticalQueue** projecten.
2. Druk op **F5** starten die de drie console apps. De **ReadDeviceToCloudMessages** app heeft alleen niet-kritieke berichten van de **SimulatedDevice** toepassing, en de **ReadCriticalQueue** app heeft alleen kritieke berichten.
   
   ![Drie console-apps][50]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe betrouwbaar apparaat-naar-cloud-berichten verzenden met behulp van de berichtroutering van IoT Hub.

De [het verzenden van berichten van de cloud-naar-apparaat met IoT Hub] [ lnk-c2d] ziet u hoe u berichten verzenden naar uw apparaten vanuit de back-end van uw oplossing.

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub [Azure IoT Suite][lnk-suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

Zie voor meer informatie over het routeren van berichten van IoT-Hub, [berichten verzenden en ontvangen met IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/
[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[aan de slag met IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[afhandeling van tijdelijke fout]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

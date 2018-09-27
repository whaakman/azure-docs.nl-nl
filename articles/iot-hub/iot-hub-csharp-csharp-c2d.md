---
title: Cloud-naar-apparaat-berichten met Azure IoT Hub (.NET) | Microsoft Docs
description: Klik hier voor meer informatie over het cloud-naar-apparaat-berichten verzenden naar een apparaat van een Azure IoT-hub met behulp van de Azure IoT SDK's voor .NET. Een apparaat-app voor het ontvangen van berichten van cloud-naar-apparaat en het wijzigen van een back-end-app om de cloud-naar-apparaat-berichten te verzenden kunt u wijzigen.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: 2dca04cadd4c5b904c752ac8a01af8bcb7c6ae33
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220105"
---
# <a name="send-messages-from-the-cloud-to-your-device-with-iot-hub-net"></a>Berichten verzenden vanuit de cloud naar uw apparaat met IoT Hub (.NET)

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Inleiding

Azure IoT Hub is een volledig beheerde service die stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing. [Verzenden van telemetrie vanaf een apparaat naar een IoT-hub... ](quickstart-send-telemetry-dotnet.md) laat zien hoe u een IoT-hub maken, een apparaat-id in het inrichten en code van een apparaat-app dat apparaat-naar-cloud-berichten verzendt.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze zelfstudie bouwt voort op de snelstartgids [telemetrie vanaf een apparaat naar een IoT-hub verzenden... ](quickstart-send-telemetry-dotnet.md). Het laat zien hoe u de volgende stappen uit:

* Vanuit de back-end, cloud-naar-apparaat-berichten naar een enkel apparaat via IoT Hub te verzenden.

* Cloud-naar-apparaat-berichten op een apparaat ontvangen.

* Aanvragen van de back-end, levering bevestiging (*feedback*) voor berichten die worden verzonden naar een apparaat vanuit IoT Hub.

U vindt meer informatie over cloud-naar-apparaat-berichten in [D2C en C2D-berichten met IoT Hub](iot-hub-devguide-messaging.md).

Aan het einde van deze zelfstudie, moet u twee .NET-consoletoepassingen uitvoeren:

* **SimulatedDevice**, een aangepaste versie van de app gemaakt [telemetrie vanaf een apparaat naar een IoT-hub verzenden... ](quickstart-send-telemetry-dotnet.md), die verbinding maakt met uw IoT-hub en cloud-naar-apparaat-berichten worden ontvangen.

* **SendCloudToDevice**, die een cloud-naar-apparaat-bericht naar de app voor Apparaatbeheer via IoT Hub verzendt en ontvangt u vervolgens de bevestiging levering.

> [!NOTE]
> IoT-Hub SDK-ondersteuning voor vele platformen voor apparaten en talen (waaronder C, Java en Javascript) heeft via [Azure IoT device SDK's](iot-hub-devguide-sdks.md). Zie voor stapsgewijze instructies voor het verbinding maken tussen uw apparaat in de code van deze zelfstudie, en in het algemeen voor Azure IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md).
> 

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2017

* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](http://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

## <a name="receive-messages-in-the-device-app"></a>Berichten ontvangen in de apparaat-app

In deze sectie bewerkt u de apparaat-app die u hebt gemaakt in [telemetrie vanaf een apparaat naar een IoT-hub verzenden... ](quickstart-send-telemetry-dotnet.md) cloud-naar-apparaat-berichten ontvangen van de IoT-hub.

1. In Visual Studio in de **SimulatedDevice** project, voegt u de volgende methode naar de **programma** klasse.

   ```csharp   
    private static async void ReceiveC2dAsync()
    {
        Console.WriteLine("\nReceiving cloud to device messages from service");
        while (true)
        {
            Message receivedMessage = await deviceClient.ReceiveAsync();
            if (receivedMessage == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received message: {0}", 
            Encoding.ASCII.GetString(receivedMessage.GetBytes()));
            Console.ResetColor();

            await deviceClient.CompleteAsync(receivedMessage);
        }
    }
   ```

   De `ReceiveAsync` methode als het ontvangen bericht asynchroon resultaat op het moment dat deze is ontvangen door het apparaat. Het resultaat *null* na een specifiable time-outperiode (in dit geval wordt de standaardwaarde van één minuut gebruikt). Wanneer de app ontvangt een *null*, het wachten op nieuwe berichten moet blijven. Deze vereiste is de reden voor de `if (receivedMessage == null) continue` regel.
   
    De aanroep van `CompleteAsync()` IoT-Hub meldt dat het bericht is verwerkt. Het bericht kan veilig worden verwijderd uit de apparaatwachtrij. Als er iets opgetreden waardoor de apparaat-app vanuit het voltooien van de verwerking van het bericht, voorziet IoT-Hub in het opnieuw. Vervolgens is het belangrijk dat bericht verwerking van logica in de apparaat-app is *idempotent*, zodat meerdere keren voor hetzelfde bericht ontvangen hetzelfde resultaat levert. 
    
    Een toepassing kan ook tijdelijk afbreken voor een bericht, hetgeen resulteert in IoT hub behoud van het bericht in de wachtrij voor toekomstig gebruik. Of de toepassing kan een bericht, waarin het bericht permanent verwijderd uit de wachtrij afwijzen. Zie voor meer informatie over de levenscyclus van cloud-naar-apparaat bericht [D2C en C2D met IoT Hub-berichten](iot-hub-devguide-messaging.md).
   
   > [!NOTE]
   > Bij het gebruik van HTTPS in plaats van MQTT- of AMQP als transport, de `ReceiveAsync` methode keert onmiddellijk. De ondersteunde patroon voor cloud-naar-apparaat-berichten met HTTPS is tijdelijk verbonden apparaten te controleren voor minder vaak berichten (minder dan elke 25 minuten). Meer HTTPS uitgeven, ontvangt resultaten in IoT Hub beperking van de aanvragen. Zie voor meer informatie over de verschillen tussen MQTT-, AMQP- en HTTPS-ondersteuning en het beperken van IoT-Hub [D2C en C2D met IoT Hub-berichten](iot-hub-devguide-messaging.md).
   > 
   > 
2. Voeg de volgende methode in de **Main** methode, precies vóór de `Console.ReadLine()` regel:
   
   ``` csharp   
   ReceiveC2dAsync();
   ```

## <a name="send-a-cloud-to-device-message"></a>Een cloud-naar-apparaat-bericht verzenden
In deze sectie schrijft u een .NET-consoletoepassing die cloud-naar-apparaat-berichten naar de apparaat-app verzendt.

1. In de huidige Visual Studio-oplossing, kunt u een Visual C# bureaublad-App-project maken met behulp van de **consoletoepassing** projectsjabloon. Noem het project **SendCloudToDevice**.
   
   ![Nieuw project in Visual Studio](./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png)

2. Klik in Solution Explorer met de rechtermuisknop op de oplossing en klik vervolgens op **NuGet-pakketten beheren voor oplossing...** . 
   
    Deze actie opent u de **NuGet-pakketten beheren** venster.

3. Zoeken naar **Microsoft.Azure.Devices**, klikt u op **installeren**, en accepteer de gebruiksvoorwaarden. 
   
    Deze downloadt, installeert en voegt u een verwijzing naar de [Azure IoT service SDK NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Devices/).

4. Voeg aan het begin van het bestand **Program.cs** de volgende `using`-instructie toe:

   ``` csharp   
   using Microsoft.Azure.Devices;
   ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de IoT hub-verbindingsreeks uit [telemetrie vanaf een apparaat naar een IoT-hub verzenden... ](quickstart-send-telemetry-dotnet.md):

   ``` csharp
   static ServiceClient serviceClient;
   static string connectionString = "{iot hub connection string}";
   ```

6. Voeg de volgende methode toe aan de klasse **Program**:
   
   ``` csharp
   private async static Task SendCloudToDeviceMessageAsync()
   {
        var commandMessage = new 
         Message(Encoding.ASCII.GetBytes("Cloud to device message."));
        await serviceClient.SendAsync("myFirstDevice", commandMessage);
   }
   ```

   Deze methode verzendt u een nieuwe cloud-naar-apparaat-bericht naar het apparaat met de ID `myFirstDevice`. Wijzig deze parameter alleen als u deze gewijzigd vanuit de die werd gebruikt [telemetrie vanaf een apparaat naar een IoT-hub verzenden... ](quickstart-send-telemetry-dotnet.md).

7. Voeg tot slot de volgende regels toe aan de methode **Main**:

   ``` csharp
   Console.WriteLine("Send Cloud-to-Device message\n");
   serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
   Console.WriteLine("Press any key to send a C2D message.");
   Console.ReadLine();
   SendCloudToDeviceMessageAsync().Wait();
   Console.ReadLine();
   ```

8. Vanuit Visual Studio met de rechtermuisknop op uw oplossing en selecteer **Set StartUp projects...** . Selecteer **meerdere opstartprojecten**en selecteer vervolgens de **Start** actie voor **ReadDeviceToCloudMessages**, **SimulatedDevice**, en **SendCloudToDevice**.

9. Druk op **F5**. Alle drie de toepassingen moeten worden gestart. Selecteer de **SendCloudToDevice** windows en drukt u op **Enter**. U ziet het bericht wordt ontvangen door de apparaat-app.
   
   ![Ontvangen bericht in de App](./media/iot-hub-csharp-csharp-c2d/sendc2d1.png)

## <a name="receive-delivery-feedback"></a>Leveringsfeedback ontvangen

Het is mogelijk op aanvraag leveren (of het aflopen) bevestigingen van IoT Hub voor elk cloud-naar-apparaat-bericht. Deze optie kunt de back-end oplossing om eenvoudig logica voor opnieuw proberen of vergoeding aan. Zie voor meer informatie over cloud-naar-apparaat feedback [D2C en C2D-berichten met IoT Hub](iot-hub-devguide-messaging.md).

In deze sectie maakt u wijzigt de **SendCloudToDevice** app feedback aanvragen en ontvangen van de IoT-hub.

1. In Visual Studio in de **SendCloudToDevice** project, voegt u de volgende methode naar de **programma** klasse.

   ```csharp
   private async static void ReceiveFeedbackAsync()
   {
        var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
        Console.WriteLine("\nReceiving c2d feedback from service");
        while (true)
        {
            var feedbackBatch = await feedbackReceiver.ReceiveAsync();
            if (feedbackBatch == null) continue;
   
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received feedback: {0}", 
              string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
            Console.ResetColor();
   
            await feedbackReceiver.CompleteAsync(feedbackBatch);
        }
    }
    ```

    Houd er rekening mee dat dit patroon ontvangen is hetzelfde als gebruikt voor het cloud-naar-apparaat-berichten ontvangen van de apparaat-app.

2. Toevoegen van de volgende methode in de **Main** methode, rechts na de `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` regel:
   
   ``` csharp
   ReceiveFeedbackAsync();
   ```

3. Om aan te vragen feedback voor de levering van uw cloud-naar-apparaat-bericht, u hoeft op te geven van een eigenschap in de **SendCloudToDeviceMessageAsync** methode. Voeg de volgende regel toe direct na de `var commandMessage = new Message(...);` regel:
   
   ``` csharp
   commandMessage.Ack = DeliveryAcknowledgement.Full;
   ```

4. De apps uitvoeren door te drukken **F5**. Hier ziet u alle drie de toepassingen starten. Selecteer de **SendCloudToDevice** windows en drukt u op **Enter**. U ziet het bericht wordt ontvangen door de apparaat-app en na een paar seconden, de Feedbackbericht wordt ontvangen door uw **SendCloudToDevice** toepassing.
   
   ![Ontvangen bericht in de App](./media/iot-hub-csharp-csharp-c2d/sendc2d2.png)

> [!NOTE]
> In deze zelfstudie implementeert voor geeft een beeld van de eenvoud, niet een beleid voor opnieuw proberen. Bij de productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals aangegeven in het artikel implementeren [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults).
> 

## <a name="next-steps"></a>Volgende stappen

In deze instructies hebt u geleerd hoe u cloud-naar-apparaat-berichten verzenden en ontvangen. 

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT-Hub [Azure IoT Remote Monitoring solution accelerator](https://docs.microsoft.com/azure/iot-suite/).

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md).
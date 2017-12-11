---
title: Cloud-naar-apparaat-berichten met Azure IoT Hub (.NET) | Microsoft Docs
description: Het cloud-naar-apparaat om berichten te verzenden naar een apparaat van een Azure IoT hub met de Azure IoT SDK's voor .NET. U kunt een apparaattoepassing cloud-naar-apparaat-berichten ontvangen en wijzigen van een back-end-app voor het verzenden van de cloud-naar-apparaat-berichten wijzigen.
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: a31c05ed-6ec0-40f3-99ab-8fdd28b1a89a
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: df0a319cdeabef8c854d7c60c7b90ef775be3ce1
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="send-messages-from-the-cloud-to-your-device-with-iot-hub-net"></a>Verzenden van berichten vanuit de cloud op uw apparaat met IoT Hub (.NET)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Inleiding
Azure IoT Hub is een volledig beheerde service waarmee stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing. De [aan de slag met IoT Hub] zelfstudie laat zien hoe u een iothub maken, een apparaat-id in het inrichten en code van de app voor een apparaat dat apparaat-naar-cloud-berichten verzendt.

Deze zelfstudie bouwt voort op [aan de slag met IoT Hub]. Hier ziet u hoe aan:

* Van uw back-end oplossing, cloud-naar-apparaat-berichten naar één enkel apparaat via IoT Hub te verzenden.
* Cloud-naar-apparaat-berichten op een apparaat ontvangen.
* Aanvragen van uw back-end oplossing, levering bevestiging (*feedback*) voor berichten die worden verzonden naar een apparaat uit IoT Hub.

U vindt meer informatie over cloud-naar-apparaat-berichten in de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].

Aan het einde van deze zelfstudie, moet u twee apps voor .NET-console uitvoeren:

* **SimulatedDevice**, een aangepaste versie van de app gemaakt in [aan de slag met IoT Hub], die verbinding maakt met uw IoT-hub en cloud-naar-apparaat-berichten worden ontvangen.
* **SendCloudToDevice**, die verzendt een bericht cloud-naar-apparaat naar de apparaattoepassing via IoT Hub en vervolgens ontvangt de bevestiging levering.

> [!NOTE]
> IoT-Hub SDK-ondersteuning voor veel apparaatplatforms en talen (inclusief C, Java en Javascript) via heeft [apparaat Azure IoT SDK's]. Zie voor stapsgewijze instructies voor het koppelen van uw apparaat in deze zelfstudie code en in het algemeen naar Azure IoT Hub de [Ontwikkelaarshandleiding voor IoT Hub].
> 
> 

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

## <a name="receive-messages-in-the-device-app"></a>Berichten ontvangen in de app voor apparaat
In deze sectie bewerkt u de app die u hebt gemaakt in voor het apparaat [aan de slag met IoT Hub] cloud-naar-apparaat om berichten te ontvangen van de IoT-hub.

1. In Visual Studio in de **SimulatedDevice** project, voegt u de volgende methode voor de **programma** klasse.
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();
   
                await deviceClient.CompleteAsync(receivedMessage);
            }
        }
   
    De `ReceiveAsync` methode retourneert asynchroon het ontvangen bericht op het moment dat deze is ontvangen door het apparaat. Deze retourneert *null* na een specifiable time-outperiode (in dit geval wordt de standaardwaarde van één minuut wordt gebruikt). Wanneer de app ontvangt een *null*, het wachten op nieuwe berichten moet blijven. Deze vereiste is de reden voor de `if (receivedMessage == null) continue` regel.
   
    De aanroep van `CompleteAsync()` IoT Hub een melding dat het bericht is verwerkt. Het bericht kan veilig worden verwijderd uit de apparaatwachtrij. Als er iets opgetreden waardoor de apparaat-app voor het voltooien van de verwerking van het bericht, bezorgd IoT-Hub opnieuw. Vervolgens is het belangrijk dat bericht verwerking logica in de apparaat-app wordt *idempotent*, zodat meerdere keren voor hetzelfde bericht ontvangen hetzelfde resultaat geeft. Een toepassing kan een bericht, hetgeen resulteert in het bericht in de wachtrij voor toekomstige consumptie behouden IoT-hub ook tijdelijk afbreken. Of de toepassing kan een bericht dat het bericht permanent verwijderd uit de wachtrij afwijzen. Zie voor meer informatie over de levenscyclus van de cloud-naar-apparaat-bericht, de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].
   
   > [!NOTE]
   > Bij gebruik van HTTPS in plaats van MQTT of AMQP als een transport, de `ReceiveAsync` methode retourneert onmiddellijk. De ondersteunde patroon voor cloud-naar-apparaat-berichten met HTTPS is tijdelijk verbonden apparaten controleren voor berichten zelden (minder dan elke 25 minuten). Uitgifte van meer HTTPS ontvangt resultaten van IoT-Hub beperking van de aanvragen. Zie voor meer informatie over de verschillen tussen MQTT, AMQP- en HTTPS-ondersteuning en Iothub beperking de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].
   > 
   > 
2. Voeg de volgende methode in de **Main** methode, precies vóór de `Console.ReadLine()` regel:
   
        ReceiveC2dAsync();

> [!NOTE]
> Deze zelfstudie implementeert voor de eenvoud mogelijk te houden, niet een beleid voor opnieuw proberen. In productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout].
> 
> 

## <a name="send-a-cloud-to-device-message"></a>Een cloud naar apparaat verzenden
In deze sectie schrijft u een .NET-consoletoepassing dat cloud-naar-apparaat-berichten naar de app apparaat verzendt.

1. Maak in de huidige Visual Studio-oplossing een Visual C# bureaublad-App-project met behulp van de **consoletoepassing** projectsjabloon. Noem het project **SendCloudToDevice**.
   
    ![Nieuw project in Visual Studio][20]
2. Klik in Solution Explorer met de rechtermuisknop op de oplossing en klik vervolgens op **NuGet-pakketten beheren voor oplossing...** . 
   
    Hiermee opent u deze actie de **NuGet-pakketten beheren** venster.
3. Zoeken naar **Microsoft.Azure.Devices**, klikt u op **installeren**, en accepteer de gebruiksvoorwaarden. 
   
    Dit downloadt, installeert en voegt u een verwijzing naar de [Azure IoT service SDK NuGet-pakket].

4. Voeg aan het begin van het bestand **Program.cs** de volgende `using`-instructie toe:
   
        using Microsoft.Azure.Devices;
5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de verbindingsreeks van de IoT-hub uit [aan de slag met IoT Hub]:
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Voeg de volgende methode toe aan de klasse **Program**:
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    Deze methode verzendt u een nieuw bericht in de cloud-naar-apparaat op het apparaat met de ID `myFirstDevice`. Deze parameter alleen wijzigen als u deze uit het abonnement in gebruikt gewijzigd [aan de slag met IoT Hub].
7. Voeg tot slot de volgende regels toe aan de methode **Main**:
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. Vanuit Visual Studio met de rechtermuisknop op uw oplossing en selecteer **opstartprojecten instellen...** . Selecteer **meerdere opstartprojecten**, selecteer vervolgens de **Start** actie voor **ReadDeviceToCloudMessages**, **SimulatedDevice**, en **SendCloudToDevice**.
9. Druk op **F5**. Alle drie de toepassingen moeten worden gestart. Selecteer de **SendCloudToDevice** windows en druk op **Enter**. U ziet het bericht wordt ontvangen door de apparaat-app.
   
   ![App-ontvangende bericht][21]

## <a name="receive-delivery-feedback"></a>Leveringsfeedback ontvangen
Het is mogelijk aan de aanvraag-levering (of verlopen) bevestigingen van IoT Hub voor elk bericht cloud-naar-apparaat. Deze optie kunt de back-end oplossing eenvoudig opnieuw of compensatie logica te informeren. Zie voor meer informatie over de feedback voor cloud-naar-apparaat de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].

In deze sectie, wijzigt u de **SendCloudToDevice** app feedback aanvragen en ontvangen van IoT Hub.

1. In Visual Studio in de **SendCloudToDevice** project, voegt u de volgende methode voor de **programma** klasse.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();
   
                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }
   
    Houd er rekening mee dat dit patroon ontvangen dezelfde is als cloud-naar-apparaat om berichten te ontvangen van de app apparaat gebruikt.
2. Voeg de volgende methode in de **Main** methode rechts na de `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` regel:
   
        ReceiveFeedbackAsync();
3. Om aan te vragen feedback voor de levering van uw cloud-naar-apparaat-bericht, u moet opgeven van een eigenschap in de **SendCloudToDeviceMessageAsync** methode. Voeg de volgende regel, meteen na de `var commandMessage = new Message(...);` regel:
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. De apps worden uitgevoerd door te drukken **F5**. Hier ziet u alle drie de toepassingen starten. Selecteer de **SendCloudToDevice** windows en druk op **Enter**. U ziet het bericht wordt ontvangen door de apparaat-app en na een paar seconden de Feedbackbericht wordt ontvangen door uw **SendCloudToDevice** toepassing.
   
   ![App-ontvangende bericht][22]

> [!NOTE]
> Deze zelfstudie implementeert voor de eenvoud mogelijk te houden, niet een beleid voor opnieuw proberen. In productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout].
> 
> 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe cloud-naar-apparaat-berichten verzenden en ontvangen. 

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub [Azure IoT Suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT service SDK NuGet-pakket]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[afhandeling van tijdelijke fout]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md

[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[aan de slag met IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://docs.microsoft.com/azure/iot-suite/
[apparaat Azure IoT SDK's]: iot-hub-devguide-sdks.md
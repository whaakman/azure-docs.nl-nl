---
title: Uploaden van bestanden van apparaten naar Azure IoT Hub met .NET | Microsoft Docs
description: "Het uploaden van bestanden van een apparaat naar de cloud met Azure IoT-device SDK voor .NET. Geüploade bestanden worden opgeslagen in een Azure storage-blob-container."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2017
ms.author: elioda
ms.openlocfilehash: 4362512121ca426fcae6716c74e1f8effa0986f1
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-using-net"></a>Uploaden van bestanden op uw apparaat naar de cloud met IoT Hub met .NET

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Deze zelfstudie bouwt voort op de code in de [Cloud naar apparaat verzenden met IoT Hub](iot-hub-csharp-csharp-c2d.md) zelfstudie leert u de mogelijkheden voor het uploaden van bestand van IoT Hub gebruiken. Hier ziet u hoe aan:

- Veilig een apparaat voorzien van een Azure blob-URI voor het uploaden van een bestand.
- Gebruik de IoT Hub-bestand uploaden meldingen voor het activeren van het bestand in de back-end van uw app wordt verwerkt.

De [aan de slag met IoT Hub](iot-hub-csharp-csharp-getstarted.md) en [Cloud naar apparaat verzenden met IoT Hub](iot-hub-csharp-csharp-c2d.md) zelfstudies ziet de apparaat-naar-cloud- en cloud-naar-apparaat messaging basisfunctionaliteit van IoT-Hub. De [proces apparaat-naar-cloudberichten](iot-hub-csharp-csharp-process-d2c.md) zelfstudie een manier voor het apparaat-naar-cloud-berichten veilig opslaan in Azure blob-opslag beschreven. Echter, in sommige scenario's kan niet eenvoudig koppelt u de gegevens verzenden van uw apparaten in de relatief klein apparaat-naar-cloud-berichten die IoT Hub accepteert. Bijvoorbeeld:

* Grote bestanden met afbeeldingen
* Video's
* Trillingen gegevens dat met hoge frequentie
* Een vorm van voorverwerkte gegevens

Deze bestanden zijn meestal batch verwerkt in de cloud met hulpprogramma's zoals [Azure Data Factory](../data-factory/introduction.md) of de [Hadoop](../hdinsight/index.md) stack. Als u uploaden van bestanden van een apparaat wilt, kunt u de beveiliging en betrouwbaarheid van IoT Hub.

Aan het einde van deze zelfstudie kunt u twee apps voor .NET-console uitvoeren:

* **SimulatedDevice**, een aangepaste versie van de app gemaakt in de [Cloud naar apparaat verzenden met IoT Hub](iot-hub-csharp-csharp-c2d.md) zelfstudie. Deze app uploadt een bestand naar de opslag met een SAS-URI geleverd door uw IoT-hub.
* **ReadFileUploadNotification**, dat bestand uploaden meldingen ontvangt van uw IoT-hub.

> [!NOTE]
> IoT Hub biedt ondersteuning voor veel apparaatplatforms en talen (inclusief C, Java en Javascript) via Azure IoT-apparaat SDK's. Raadpleeg de [Azure IoT Developer Center] voor stapsgewijze instructies voor het koppelen van uw apparaat met Azure IoT Hub.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Een bestand vanaf een apparaat-app uploaden

In deze sectie maakt u de app die u hebt gemaakt in voor het apparaat wijzigen [Cloud naar apparaat verzenden met IoT Hub](iot-hub-csharp-csharp-c2d.md) cloud-naar-apparaat om berichten te ontvangen van de IoT-hub.

1. In Visual Studio met de rechtermuisknop op de **SimulatedDevice** project, klikt u op **toevoegen**, en klik vervolgens op **bestaand Item**. Navigeer naar een afbeeldingsbestand en deze opnemen in uw project. Deze zelfstudie wordt ervan uitgegaan dat de installatiekopie met de naam `image.jpg`.

1. Met de rechtermuisknop op de installatiekopie en klik vervolgens op **eigenschappen**. Zorg ervoor dat **naar uitvoermap kopiëren** is ingesteld op **altijd kopiëren**.

    ![][1]

1. In de **Program.cs** bestand, voeg de volgende instructies toe aan de bovenkant van het bestand:

    ```csharp
    using System.IO;
    ```

1. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    De `UploadToBlobAsync` methode neemt in de naam en stroom bron van het bestand te uploaden en verwerkt het uploaden naar de opslag. De console-app geeft de tijd die nodig is om het bestand te uploaden.

1. Voeg de volgende methode in de **Main** methode, precies vóór de `Console.ReadLine()` regel:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Deze zelfstudie implementeert voor de eenvoud mogelijk te houden, niet een beleid voor opnieuw proberen. In productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout].

## <a name="receive-a-file-upload-notification"></a>Ontvangt een melding van bestand uploaden

In deze sectie schrijft u een .NET-consoletoepassing die bestand uploaden meldingsberichten uit IoT Hub ontvangt.

1. Maak in de huidige Visual Studio-oplossing een Visual C# Windows-project met behulp van de **consoletoepassing** projectsjabloon. Noem het project **ReadFileUploadNotification**.

    ![Nieuw project in Visual Studio][2]

1. Klik in Solution Explorer met de rechtermuisknop op de **ReadFileUploadNotification** project en klik vervolgens op **NuGet-pakketten beheren...** .

1. In de **NuGet Package Manager** venster, zoekt u **Microsoft.Azure.Devices**, klikt u op **installeren**, en accepteer de gebruiksvoorwaarden.

    Deze actie downloadt, installeert en voegt u een verwijzing naar de [Azure IoT service SDK NuGet-pakket] in de **ReadFileUploadNotification** project.

1. In de **Program.cs** bestand, voeg de volgende instructies toe aan de bovenkant van het bestand:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de verbindingsreeks van de IoT-hub uit [aan de slag met IoT Hub]:

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    Houd er rekening mee dat dit patroon ontvangen dezelfde is als cloud-naar-apparaat om berichten te ontvangen van de app apparaat gebruikt.

1. Voeg tot slot de volgende regels toe aan de methode **Main**:

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

1. Met de rechtermuisknop op uw oplossing in Visual Studio, en selecteer **Set StartUp projects**. Selecteer **meerdere opstartprojecten**, selecteer vervolgens de **Start** actie voor **ReadFileUploadNotification** en **SimulatedDevice**.

1. Druk op **F5**. Beide toepassingen moeten worden gestart. U ziet het uploaden is voltooid in één console-app en het meldingsbericht uploaden is ontvangen door de console-app. U kunt de [Azure-portal] of Visual Studio Server Explorer om te controleren op de aanwezigheid van het geüploade bestand in uw Azure Storage-account.

    ![][50]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe de mogelijkheden voor het uploaden van bestand van IoT Hub gebruiken voor het vereenvoudigen van bestandsuploads van apparaten. U kunt doorgaan met het verkennen van IoT hub functies en scenario's met de volgende artikelen:

* [Een iothub via een programma maken][lnk-create-hub]
* [Inleiding tot C-SDK][lnk-c-sdk]
* [Azure IoT SDK 's][lnk-sdks]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png

<!-- Links -->

[Azure-portal]: https://portal.azure.com/

[Azure IoT Developer Center]: http://azure.microsoft.com/develop/iot

[afhandeling van tijdelijke fout]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure IoT service SDK NuGet-pakket]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md

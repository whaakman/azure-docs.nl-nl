---
title: Uploaden van bestanden vanaf apparaten met Azure IoT Hub met .NET | Microsoft Docs
description: Klik hier voor meer informatie over het uploaden van bestanden vanaf een apparaat in de cloud met Azure IoT device-SDK voor .NET. Geüploade bestanden worden opgeslagen in een Azure storage blob-container.
author: fsautomata
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: elioda
ms.openlocfilehash: c881ead472d07200bdf4284f30bcf097f0efcba4
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223795"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-using-net"></a>Uploaden van bestanden van uw apparaat naar de cloud met IoT Hub met .NET

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

In deze zelfstudie bouwt voort op de code in de [Cloud-naar-apparaat-berichten verzenden met IoT Hub](iot-hub-csharp-csharp-c2d.md) zelfstudie leert u hoe u gebruik van de mogelijkheden voor het uploaden van bestand van IoT-Hub. Hier ziet u hoe aan:

- Veilig een apparaat voorzien van een Azure blob-URI voor het uploaden van een bestand.

- Gebruik de IoT Hub-bestand uploaden meldingen voor het activeren van het bestand in de back-end van uw app wordt verwerkt.

De [verzenden van telemetrie vanaf een apparaat naar een IoT-hub](quickstart-send-telemetry-dotnet.md) en [Cloud-naar-apparaat-berichten verzenden met IoT Hub](iot-hub-csharp-csharp-c2d.md) artikelen tonen de apparaat-naar-cloud en cloud-naar-apparaat berichten basisfunctionaliteit van IoT-Hub. De [berichtroutering configureren met IoT Hub](tutorial-routing.md) zelfstudie een manier voor het opslaan van apparaat-naar-cloud-berichten op betrouwbare wijze in Azure blob-opslag wordt beschreven. Echter, in sommige scenario's kan niet eenvoudig koppelt u de gegevens die uw apparaten verzenden naar de relatief klein aantal apparaat-naar-cloud-berichten die IoT Hub worden geaccepteerd. Bijvoorbeeld:

* Grote bestanden met afbeeldingen
* Video's
* Trillingen gegevens verzameld met hoge frequentie
* Een vorm van voorverwerkte gegevens

Deze bestanden zijn meestal batch verwerkt in de cloud met behulp van hulpprogramma's zoals [Azure Data Factory](../data-factory/introduction.md) of de [Hadoop](../hdinsight/index.yml) stack. Wanneer u nodig hebt voor het uploaden van bestanden vanaf een apparaat, kunt u de beveiliging en betrouwbaarheid van IoT Hub nog steeds gebruiken.

Aan het einde van deze zelfstudie moet u twee .NET-consoletoepassingen uitvoeren:

* **SimulatedDevice**, een aangepaste versie van de app gemaakt de [Cloud-naar-apparaat-berichten verzenden met IoT Hub](iot-hub-csharp-csharp-c2d.md) zelfstudie. Deze app wordt een bestand geüpload naar storage met behulp van een SAS-URI geleverd door uw IoT-hub.

* **ReadFileUploadNotification**, dat bestand uploaden meldingen ontvangt van uw IoT-hub.

> [!NOTE]
> IoT Hub biedt ondersteuning voor vele platformen voor apparaten en talen (waaronder C, Java en Javascript) via Azure IoT device SDK's. Raadpleeg de [Azure IoT-ontwikkelaarscentrum](http://azure.microsoft.com/develop/iot) voor stapsgewijze instructies over hoe u uw apparaat aansluiten op Azure IoT Hub.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2017
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](http://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Upload een bestand van een apparaat-app

In deze sectie maakt u de apparaat-app die u hebt gemaakt in [Cloud-naar-apparaat-berichten verzenden met IoT Hub](iot-hub-csharp-csharp-c2d.md) cloud-naar-apparaat-berichten ontvangen van de IoT-hub.

1. In Visual Studio met de rechtermuisknop op de **SimulatedDevice** project, klikt u op **toevoegen**, en klik vervolgens op **bestaand Item**. Navigeer naar een afbeeldingsbestand en deze opnemen in uw project. In deze zelfstudie wordt ervan uitgegaan dat de installatiekopie met de naam `image.jpg`.

1. Met de rechtermuisknop op de afbeelding, en klik vervolgens op **eigenschappen**. Zorg ervoor dat **naar uitvoermap kopiëren** is ingesteld op **altijd kopiëren**.

    ![Laten zien waar u het bijwerken van de eigenschap image voor naar uitvoermap kopiëren](./media/iot-hub-csharp-csharp-file-upload/image-properties.png)

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

    De `UploadToBlobAsync` methode neemt de bestandsbron voor naam en de stream van het bestand moet worden geüpload en verwerkt voor het uploaden naar opslag. De console-app wordt weergegeven de tijd die nodig zijn om het bestand te uploaden.

1. Voeg de volgende methode in de **Main** methode, precies vóór de `Console.ReadLine()` regel:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> In deze zelfstudie implementeert voor geeft een beeld van de eenvoud, niet een beleid voor opnieuw proberen. Bij de productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals aangegeven in het artikel implementeren [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults).

## <a name="receive-a-file-upload-notification"></a>Een bestand uploaden melding ontvangen

In deze sectie schrijft u een .NET-consoletoepassing die bestand uploaden kennisgeving berichten uit IoT Hub ontvangt.

1. In de huidige Visual Studio-oplossing, kunt u een Visual C# Windows-project maken met behulp van de **consoletoepassing** projectsjabloon. Noem het project **ReadFileUploadNotification**.

    ![Nieuw project in Visual Studio](./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png)

2. Klik in Solution Explorer met de rechtermuisknop op de **ReadFileUploadNotification** project en klik vervolgens op **NuGet-pakketten beheren...** .

3. In de **NuGet Package Manager** venster, zoekt u **Microsoft.Azure.Devices**, klikt u op **installeren**, en accepteer de gebruiksvoorwaarden.

    Met deze actie downloadt, installeert en voegt u een verwijzing naar de [Azure IoT service SDK NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Devices/) in de **ReadFileUploadNotification** project.

4. In de **Program.cs** bestand, voeg de volgende instructies toe aan de bovenkant van het bestand:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de IoT hub-verbindingsreeks uit [verzenden van telemetrie vanaf een apparaat naar een IoT-hub](quickstart-send-telemetry-dotnet.md):

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

6. Voeg de volgende methode toe aan de klasse **Program**:

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
            Console.WriteLine("Received file upload notification: {0}", 
              string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }   
    ```

    Houd er rekening mee dat dit patroon ontvangen is hetzelfde als gebruikt voor het cloud-naar-apparaat-berichten ontvangen van de apparaat-app.

7. Voeg tot slot de volgende regels toe aan de methode **Main**:

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

1. In Visual Studio met de rechtermuisknop op uw oplossing en selecteer **Set StartUp projects**. Selecteer **meerdere opstartprojecten**en selecteer vervolgens de **Start** actie voor **ReadFileUploadNotification** en **SimulatedDevice**.

2. Druk op **F5**. Beide toepassingen moeten worden gestart. U ziet het uploaden is voltooid in één console-app en de upload-melding ontvangen door de console-app. U kunt de [Azure-portal](https://portal.azure.com/) of Visual Studio Server Explorer om te controleren op de aanwezigheid van het geüploade bestand in uw Azure Storage-account.

    ![Schermopname van het uitvoerscherm](./media/iot-hub-csharp-csharp-file-upload/run-apps1.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de mogelijkheden voor het uploaden van bestand van IoT Hub gebruikt voor het vereenvoudigen van het uploaden van bestanden vanaf apparaten. U kunt doorgaan met het verkennen van IoT hub-functies en scenario's met de volgende artikelen:

* [Een IoT hub via een programma maken](iot-hub-rm-template-powershell.md)
* [Inleiding tot C SDK](iot-hub-device-sdk-c-intro.md)
* [SDK's voor Azure IoT](iot-hub-devguide-sdks.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)


---
title: Bestanden van apparaten uploaden naar Azure IoT Hub met .NET | Microsoft Docs
description: Bestanden van een apparaat naar de Cloud uploaden met behulp van Azure IoT Device SDK voor .NET. Geüploade bestanden worden opgeslagen in een Azure Storage-BLOB-container.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: robinsh
ms.openlocfilehash: 6e41b1999033c00b277cd35173b3247a727e9a8a
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668141"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-net"></a>Bestanden van uw apparaat uploaden naar de Cloud met IoT Hub (.NET)

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

In deze zelf studie wordt gebruikgemaakt van de code in de [IOT hub-zelf studie Cloud-naar-apparaat-berichten verzenden](iot-hub-csharp-csharp-c2d.md) om u te laten zien hoe u de mogelijkheden voor het uploaden van bestanden van IOT hub kunt gebruiken. U ziet hoe u het volgende kunt doen:

* Een apparaat veilig voorzien van een Azure Blob-URI voor het uploaden van een bestand.

* Gebruik de IoT Hub bestands upload meldingen om de verwerking van het bestand in de back-end van de app te activeren.

Het [verzenden van telemetrie van een apparaat naar een IOT hub](quickstart-send-telemetry-dotnet.md) Quick Start en het [verzenden van Cloud-naar-apparaat-berichten met IOT hub](iot-hub-csharp-csharp-c2d.md) zelf studie geven de basis functionaliteit van het apparaat-naar-Cloud-en Cloud-naar-apparaat-bericht van IOT hub weer. In de zelf studie [bericht routering configureren met IOT hub](tutorial-routing.md) wordt beschreven hoe u apparaat-naar-Cloud-berichten betrouwbaar kunt opslaan in Azure Blob-opslag. In sommige gevallen kunt u de gegevens die uw apparaten verzenden echter niet eenvoudig toewijzen aan de relatief kleine apparaat-naar-Cloud-berichten die IoT Hub accepteren. Bijvoorbeeld:

* Grote bestanden die installatie kopieën bevatten
* Video's
* Bemonsterde trillings gegevens met hoge frequentie
* Een vorm van voorverwerkte gegevens

Deze bestanden worden meestal batch verwerkt in de Cloud met behulp van hulpprogram ma's als [Azure Data Factory](../data-factory/introduction.md) of de [Hadoop](../hdinsight/index.yml) -stack. Wanneer u bestanden van een apparaat wilt uploaden, kunt u nog steeds gebruikmaken van de beveiliging en betrouw baarheid van IoT Hub.

Aan het einde van deze zelf studie voert u twee .NET-console-apps uit:

* **SimulatedDevice**, een gewijzigde versie van de app die is gemaakt in het [verzenden van Cloud-naar-apparaat-berichten met IOT hub](iot-hub-csharp-csharp-c2d.md) zelf studie. Met deze app wordt een bestand geüpload naar Storage met behulp van een SAS-URI van uw IoT-hub.

* **ReadFileUploadNotification**, die meldingen over het uploaden van bestanden ontvangt van uw IOT-hub.

> [!NOTE]
> IoT Hub ondersteunt veel platformen en talen (waaronder C, Java, python en Java script) via Azure IoT-apparaat-Sdk's. Raadpleeg het [Azure IOT-ontwikkelaars centrum](https://azure.microsoft.com/develop/iot) voor stapsgewijze instructies voor het verbinden van uw apparaat met Azure IOT hub.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio

* Een actief Azure-account. (Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Een bestand uploaden vanuit een apparaat-app

In deze sectie wijzigt u de apparaat-app die u hebt gemaakt in [Cloud-naar-apparaat-berichten verzenden met IOT hub](iot-hub-csharp-csharp-c2d.md) om Cloud-naar-apparaat-berichten van de IOT-hub te ontvangen.

1. Klik in Visual Studio met de rechter muisknop op het project **SimulatedDevice** , klikt u op **toevoegen**, en klik vervolgens op **bestaand item**. Navigeer naar een afbeeldings bestand en voeg dit toe aan uw project. In deze zelf studie wordt ervan uitgegaan `image.jpg`dat de installatie kopie een naam heeft.

1. Klik met de rechter muisknop op de afbeelding en klik vervolgens op **Eigenschappen**. Zorg ervoor dat de **map kopiëren naar uitvoermap** is ingesteld op **kopiëren altijd**.

    ![Weer geven waar de afbeeldings eigenschap moet worden bijgewerkt voor de map kopiëren naar uitvoermap](./media/iot-hub-csharp-csharp-file-upload/image-properties.png)

1. Voeg in het bestand **Program.cs** de volgende instructies toe boven aan het bestand:

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

    De `UploadToBlobAsync` -methode neemt de bestands naam en stroom bron van het bestand dat moet worden geüpload en verwerkt het uploaden naar de opslag. De console-app geeft de tijd weer die nodig is om het bestand te uploaden.

1. Voeg de volgende methode toe in de methode **Main** , rechts voor `Console.ReadLine()` de regel:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> In het omwille van de eenvoud implementeert deze zelf studie geen beleid voor opnieuw proberen. In productie code moet u beleid voor opnieuw proberen implementeren (zoals exponentiële uitstel), zoals wordt voorgesteld in het artikel, [tijdelijke fout afhandeling](/azure/architecture/best-practices/transient-faults).

## <a name="get-the-iot-hub-connection-string"></a>De IoT hub-connection string ophalen

In dit artikel maakt u een back-end-service om meldings berichten over het uploaden van bestanden te ontvangen van de IoT-hub die u hebt gemaakt in telemetrie [van een apparaat naar een IOT-hub verzenden](quickstart-send-telemetry-dotnet.md). Als u meldings berichten over het uploaden van bestanden wilt ontvangen, moet u de service **Connect** -machtiging hebben. Standaard wordt elke IoT Hub gemaakt met een gedeeld toegangs beleid met de naam **service** dat deze machtiging verleent.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="receive-a-file-upload-notification"></a>Een melding over het uploaden van een bestand ontvangen

In deze sectie schrijft u een .NET-console-app die berichten over het uploaden van bestanden van IoT Hub ontvangt.

1. Maak in de huidige Visual Studio-oplossing een Visual C# Windows-project met behulp van de project sjabloon **console toepassing** . Geef het project de naam **ReadFileUploadNotification**.

    ![Nieuw project in Visual Studio](./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png)

2. Klik in Solution Explorer met de rechter muisknop op het project **ReadFileUploadNotification** en klik vervolgens op **NuGet-pakketten beheren...** .

3. In het venster **NuGet package manager** zoekt u naar **micro soft. Azure. devices**, klikt u op **install**en gaat u akkoord met de gebruiks voorwaarden.

    Met deze actie wordt een verwijzing gedownload, geïnstalleerd en toegevoegd aan het [Azure IOT Service SDK NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Devices/) in het **ReadFileUploadNotification** -project.

4. Voeg in het bestand **Program.cs** de volgende instructies toe boven aan het bestand:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de waarde van de tijdelijkeaanduidingdoordeIOThub-ConnectionStringdieueerderhebtgekopieerdindeIOThub-ConnectionStringophalen:`{iot hub connection string}` [](#get-the-iot-hub-connection-string)

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

    Houd er rekening mee dat dit ontvangst patroon hetzelfde is als het ontvangen van Cloud-naar-apparaat-berichten van de apparaat-app.

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

1. Klik in Visual Studio met de rechter muisknop op uw oplossing en selecteer vervolgens **opstart projecten instellen**. Selecteer **meerdere opstart projecten**en selecteer vervolgens de actie **starten** voor **ReadFileUploadNotification** en **SimulatedDevice**.

2. Druk op **F5**. Beide toepassingen moeten worden gestart. U ziet dat de Upload voltooid is in één console-app en het bericht voor het uploaden van meldingen dat is ontvangen door de andere console-app. U kunt de Server Explorer [Azure Portal](https://portal.azure.com/) of Visual Studio gebruiken om te controleren of het geüploade bestand aanwezig is in uw Azure Storage-account.

    ![Scherm opname van het uitvoer scherm](./media/iot-hub-csharp-csharp-file-upload/run-apps1.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u geleerd hoe u de functies voor het uploaden van bestanden van IoT Hub kunt gebruiken om Bestands uploads van apparaten te vereenvoudigen. U kunt IoT hub-functies en-scenario's blijven verkennen met de volgende artikelen:

* [Een IoT-hub maken via een programma](iot-hub-rm-template-powershell.md)

* [Inleiding tot C SDK](iot-hub-device-sdk-c-intro.md)

* [SDK's voor Azure IoT](iot-hub-devguide-sdks.md)

Zie voor meer informatie over de mogelijkheden van IoT Hub:

* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)

---
title: Uploaden van bestanden van apparaten naar Azure IoT Hub met Java | Microsoft Docs
description: "Het uploaden van bestanden van een apparaat naar de cloud met Azure IoT-device SDK voor Java. Geüploade bestanden worden opgeslagen in een Azure storage-blob-container."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: b6e1ef5a5b3f9298134a7c312ac7d1927cf41a7f
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Uploaden van bestanden op uw apparaat naar de cloud met IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Deze zelfstudie bouwt voort op de code in de [Cloud naar apparaat verzenden met IoT Hub](iot-hub-java-java-c2d.md) zelfstudie leert u gebruikt de [bestand uploaden mogelijkheden van IoT Hub](iot-hub-devguide-file-upload.md) voor het uploaden van een bestand naar [Azure blob-opslag](../storage/index.yml). De zelfstudie leert u hoe aan:

- Veilig een apparaat voorzien van een Azure blob-URI voor het uploaden van een bestand.
- Gebruik de IoT Hub-bestand uploaden meldingen voor het activeren van het bestand in de back-end van uw app wordt verwerkt.

De [aan de slag met IoT Hub](iot-hub-java-java-getstarted.md) en [Cloud naar apparaat verzenden met IoT Hub](iot-hub-java-java-c2d.md) zelfstudies ziet de apparaat-naar-cloud- en cloud-naar-apparaat messaging basisfunctionaliteit van IoT-Hub. De [proces apparaat-naar-cloudberichten](iot-hub-java-java-process-d2c.md) zelfstudie een manier voor het apparaat-naar-cloud-berichten veilig opslaan in Azure blob-opslag beschreven. Echter, in sommige scenario's kan niet eenvoudig koppelt u de gegevens verzenden van uw apparaten in de relatief klein apparaat-naar-cloud-berichten die IoT Hub accepteert. Bijvoorbeeld:

* Grote bestanden met afbeeldingen
* Video's
* Trillingen gegevens dat met hoge frequentie
* Een vorm van voorverwerkte gegevens.

Deze bestanden zijn meestal batch verwerkt in de cloud met hulpprogramma's zoals [Azure Data Factory](../data-factory/introduction.md) of de [Hadoop](../hdinsight/index.md) stack. Wanneer u upland bestanden van een apparaat moet, kunt u de beveiliging en betrouwbaarheid van IoT Hub.

Aan het einde van deze zelfstudie kunt u twee Java-apps die console uitvoeren:

* **simulated-device**, een aangepaste versie van de app in de zelfstudie [berichten verzenden Cloud-naar-apparaat met IoT Hub] gemaakt. Deze app uploadt een bestand naar de opslag met een SAS-URI geleverd door uw IoT-hub.
* **Lees-bestand-upload-melding**, dat bestand uploaden meldingen ontvangt van uw IoT-hub.

> [!NOTE]
> IoT Hub biedt ondersteuning voor veel apparaatplatforms en talen (inclusief C, .NET en Javascript) via Azure IoT-apparaat SDK's. Raadpleeg de [Azure IoT Developer Center] voor stapsgewijze instructies voor het koppelen van uw apparaat met Azure IoT Hub.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](http://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Een bestand vanaf een apparaat-app uploaden

In deze sectie maakt u de app die u hebt gemaakt in voor het apparaat wijzigen [Cloud naar apparaat verzenden met IoT Hub](iot-hub-java-java-c2d.md) een bestand te uploaden naar iothub.

1. Een van installatiekopiebestand kopiëren naar de `simulated-device` map en wijzig de naam `myimage.png`.

1. Open een teksteditor de `simulated-device\src\main\java\com\mycompany\app\App.java` bestand.

1. Toevoegen van de variabelen declareren naar de **App** klasse:

    ```java
    private static String fileName = "myimage.png";
    ```

1. Voor het verwerken van bestand uploaden statusberichten callback toevoegen de volgende geneste klasse naar de **App** klasse:

    ```java
    // Define a callback method to print status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to file upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. Om afbeeldingen te uploaden naar IoT Hub, voegt u de volgende methode naar het **App** klasse voor het uploaden van afbeeldingen met IoT Hub:

    ```java
    // Use IoT Hub to upload a file asynchronously to Azure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

1. Wijzig de **belangrijkste** methode aan te roepen de **uploadFile** methode, zoals wordt weergegeven in het volgende fragment:

    ```java
    client.open();

    try
    {
      // Get the filename and start the upload.
      String fullFileName = System.getProperty("user.dir") + File.separator + fileName;
      uploadFile(fullFileName);
      System.out.println("File upload started with success");
    }
    catch (Exception e)
    {
      System.out.println("Exception uploading file: " + e.getCause() + " \nERROR: " + e.getMessage());
    }

    MessageSender sender = new MessageSender();
    ```

1. Gebruik de volgende opdracht om samen te stellen de **simulated-device** app en controleren op fouten:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a>Ontvangt een melding van bestand uploaden

In deze sectie maakt u een Java-consoletoepassing die bestand uploaden meldingsberichten uit IoT Hub ontvangt.

U moet de **iothubowner** verbindingsreeks voor uw IoT-Hub in deze sectie voltooid. U vindt de verbindingsreeks in de [Azure-portal](https://portal.azure.com/) op de **beleid voor gedeelde toegang** blade.

1. Maak een Maven-project aangeroepen **lezen-bestand-upload-melding** met de volgende opdracht achter de opdrachtprompt. Houd rekening met dat deze opdracht is een enkele, lange opdracht:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. Bij de opdrachtprompt, gaat u naar de nieuwe `read-file-upload-notification` map.

1. Open een teksteditor de `pom.xml` bestand de `read-file-upload-notification` map en voeg de volgende afhankelijkheid voor de **afhankelijkheden** knooppunt. De afhankelijkheid toe te voegen, kunt u met de **iothub-java-service-client** pakket in uw toepassing om te communiceren met de service van uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > U kunt controleren voor de meest recente versie van **iot-service-client** met [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Sla op en sluit de `pom.xml` bestand.

1. Open een teksteditor de `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` bestand.

1. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. De volgende klasseniveau variabelen toevoegen aan de **App** klasse:

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. Informatie over het bestand te uploaden naar de console om af te drukken toevoegen de volgende geneste klasse naar de **App** klasse:

    ```java
    // Create a thread to receive file upload notifications.
    private static class ShowFileUploadNotifications implements Runnable {
      public void run() {
        try {
          while (true) {
            System.out.println("Recieve file upload notifications...");
            FileUploadNotification fileUploadNotification = fileUploadNotificationReceiver.receive();
            if (fileUploadNotification != null) {
              System.out.println("File Upload notification received");
              System.out.println("Device Id : " + fileUploadNotification.getDeviceId());
              System.out.println("Blob Uri: " + fileUploadNotification.getBlobUri());
              System.out.println("Blob Name: " + fileUploadNotification.getBlobName());
              System.out.println("Last Updated : " + fileUploadNotification.getLastUpdatedTimeDate());
              System.out.println("Blob Size (Bytes): " + fileUploadNotification.getBlobSizeInBytes());
              System.out.println("Enqueued Time: " + fileUploadNotification.getEnqueuedTimeUtcDate());
            }
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. Voor het starten van de thread die naar bestand uploaden meldingen luistert, voeg de volgende code naar de **belangrijkste** methode:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from the ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start the thread to receive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER to exit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

1. Sla op en sluit de `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` bestand.

1. Gebruik de volgende opdracht om samen te stellen de **lezen-bestand-upload-melding** app en controleren op fouten:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

Bij een opdrachtprompt in de `read-file-upload-notification` map, voer de volgende opdracht:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Bij een opdrachtprompt in de `simulated-device` map, voer de volgende opdracht:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

De volgende schermafbeelding ziet u de uitvoer van de **simulated-device** app:

![De uitvoer van de gesimuleerde apparaattoepassing](media/iot-hub-java-java-upload/simulated-device.png)

De volgende schermafbeelding ziet u de uitvoer van de **lezen-bestand-upload-melding** app:

![De uitvoer van de lezen-bestand-upload-melding app](media/iot-hub-java-java-upload/read-file-upload-notification.png)

U kunt de portal gebruiken om het geüploade bestand in de storage-container die u hebt geconfigureerd:

![Geüploade bestand](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe de mogelijkheden voor het uploaden van bestand van IoT Hub gebruiken voor het vereenvoudigen van bestandsuploads van apparaten. U kunt doorgaan met het verkennen van IoT hub functies en scenario's met de volgende artikelen:

* [Een iothub via een programma maken][lnk-create-hub]
* [Inleiding tot C-SDK][lnk-c-sdk]
* [Azure IoT SDK 's][lnk-sdks]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Een apparaat simuleren met IoT rand][lnk-iotedge]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png
[3]: ./media/iot-hub-csharp-csharp-file-upload/enable-file-notifications.png

<!-- Links -->



[Azure IoT Developer Center]: http://azure.microsoft.com/develop/iot

[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure Storage]:../storage/common/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md



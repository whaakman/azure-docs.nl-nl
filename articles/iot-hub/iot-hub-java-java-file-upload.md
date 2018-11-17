---
title: Uploaden van bestanden vanaf apparaten met Azure IoT Hub met Java | Microsoft Docs
description: Klik hier voor meer informatie over het uploaden van bestanden vanaf een apparaat naar de cloud met Azure IoT device-SDK voor Java. Geüploade bestanden worden opgeslagen in een Azure storage blob-container.
author: dominicbetts
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: 11b283bf3557fd14663e1789ce19af48b9aa9db2
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824061"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Uploaden van bestanden van uw apparaat naar de cloud met IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

In deze zelfstudie bouwt voort op de code in de [Cloud-naar-apparaat-berichten verzenden met IoT Hub](iot-hub-java-java-c2d.md) zelfstudie leert u hoe u gebruik van de [bestand uploaden mogelijkheden van IoT-Hub](iot-hub-devguide-file-upload.md) naar een bestand uploadt naar [Azure blob opslag](../storage/index.yml). In deze zelfstudie leert u het volgende:

* Veilig een apparaat voorzien van een Azure blob-URI voor het uploaden van een bestand.

* Gebruik de IoT Hub-bestand uploaden meldingen voor het activeren van het bestand in de back-end van uw app wordt verwerkt.

De [verzenden van telemetrie naar IoT Hub (Java)](quickstart-send-telemetry-java.md) en [Cloud-naar-apparaat-berichten verzenden met IoT Hub (Java)](iot-hub-java-java-c2d.md) zelfstudies ziet u de apparaat-naar-cloud en cloud-naar-apparaat berichten basisfunctionaliteit van IoT-Hub. De [configureren met IoT Hub-berichtroutering](tutorial-routing.md) zelfstudie een manier voor het opslaan van apparaat-naar-cloud-berichten op betrouwbare wijze in Azure blob-opslag wordt beschreven. Echter, in sommige scenario's kan niet eenvoudig koppelt u de gegevens die uw apparaten verzenden naar de relatief klein aantal apparaat-naar-cloud-berichten die IoT Hub worden geaccepteerd. Bijvoorbeeld:

* Grote bestanden met afbeeldingen
* Video's
* Trillingen gegevens verzameld met hoge frequentie
* Een vorm van voorverwerkte gegevens.

Deze bestanden zijn meestal batch verwerkt in de cloud met behulp van hulpprogramma's zoals [Azure Data Factory](../data-factory/introduction.md) of de [Hadoop](../hdinsight/index.yml) stack. Wanneer u upland bestanden vanaf een apparaat wilt, kunt u de beveiliging en betrouwbaarheid van IoT Hub nog steeds gebruiken.

Aan het einde van deze zelfstudie moet u twee Java-consoletoepassingen uitvoeren:

* **simulated-device**, een aangepaste versie van de app gemaakt in de zelfstudie [berichten verzenden Cloud-naar-apparaat met IoT Hub]. Deze app wordt een bestand geüpload naar storage met behulp van een SAS-URI geleverd door uw IoT-hub.

* **lezen-bestand-upload-melding**, dat bestand uploaden meldingen ontvangt van uw IoT-hub.

> [!NOTE]
> IoT Hub biedt ondersteuning voor vele platformen voor apparaten en talen (waaronder C, .NET en Javascript) via Azure IoT device SDK's. Raadpleeg de [Azure IoT-ontwikkelaarscentrum](https://azure.microsoft.com/develop/iot) voor stapsgewijze instructies over hoe u uw apparaat aansluiten op Azure IoT Hub.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* De meest recente [Java SE Development Kit 8](https://aka.ms/azure-jdks)

* [Maven 3](https://maven.apache.org/install.html)

* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Upload een bestand van een apparaat-app

In deze sectie maakt u de apparaat-app die u hebt gemaakt in [Cloud-naar-apparaat-berichten verzenden met IoT Hub](iot-hub-java-java-c2d.md) voor het uploaden van een bestand met IoT hub.

1. Een van installatiekopiebestand kopiëren naar de `simulated-device` map en wijzig de naam `myimage.png`.

2. Open met behulp van een teksteditor en de `simulated-device\src\main\java\com\mycompany\app\App.java` bestand.

3. Voeg de variabeledeclaratie naar de **App** klasse:

    ```java
    private static String fileName = "myimage.png";
    ```

4. Voor het verwerken van bestand uploaden statusberichten callback toevoegen de volgende geneste klasse om de **App** klasse:

    ```java
    // Define a callback method to print status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to file upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

5. Toevoegen als u wilt afbeeldingen uploaden naar IoT Hub, de volgende methode aan de **App** klasse afbeeldingen uploaden naar IoT Hub:

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

6. Wijzig de **belangrijkste** methode om aan te roepen de **uploadFile** methode zoals wordt weergegeven in het volgende codefragment:

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

7. Gebruik de volgende opdracht uit om te bouwen de **simulated-device** app en op fouten controleren:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a>Een bestand uploaden melding ontvangen

In deze sectie maakt u een Java-consoletoepassing die bestand uploaden kennisgeving berichten uit IoT Hub ontvangt.

U moet de **iothubowner** verbindingsreeks voor uw IoT-Hub in deze sectie voltooid. U vindt de verbindingsreeks in de [Azure-portal](https://portal.azure.com/) op de **gedeeld toegangsbeleid** blade.

1. Maak een Maven-project met de naam **lezen-bestand-upload-melding** met de volgende opdracht in uw opdrachtvenster. Houd rekening met dat deze opdracht wordt een enkele, lange opdracht:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Bij de opdrachtprompt, gaat u naar de nieuwe `read-file-upload-notification` map.

3. Open met behulp van een teksteditor en de `pom.xml` -bestand in de `read-file-upload-notification` map en voeg de volgende afhankelijkheid aan de **afhankelijkheden** knooppunt. De afhankelijkheid toe te voegen, kunt u de **iothub-java-service-client** -pakket in uw toepassing om te communiceren met uw IoT hub-service:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > U kunt controleren voor de meest recente versie van **iot-service-client** met behulp van [Maven zoeken](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

4. Opslaan en sluiten de `pom.xml` bestand.

5. Open met behulp van een teksteditor en de `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` bestand.

6. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

7. Voeg de volgende variabelen op klasseniveau naar de **App** klasse:

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

8. Als u wilt afdrukken van informatie over het bestand te uploaden naar de console, toevoegen de volgende geneste klasse om de **App** klasse:

    ```java
    // Create a thread to receive file upload notifications.
    private static class ShowFileUploadNotifications implements Runnable {
      public void run() {
        try {
          while (true) {
            System.out.println("Receive file upload notifications...");
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

9. Voor het starten van de thread die naar bestand uploaden meldingen luistert, voeg de volgende code aan de **belangrijkste** methode:

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

10. Opslaan en sluiten de `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` bestand.

11. Gebruik de volgende opdracht uit om te bouwen de **lezen-bestand-upload-melding** app en op fouten controleren:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

Bij een opdrachtprompt in de `read-file-upload-notification` map, voer de volgende opdracht uit:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Bij een opdrachtprompt in de `simulated-device` map, voer de volgende opdracht uit:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

De volgende schermafbeelding ziet u de uitvoer van de **simulated-device** app:

![Uitvoer van het gesimuleerde apparaat-app](media/iot-hub-java-java-upload/simulated-device.png)

De volgende schermafbeelding ziet u de uitvoer van de **lezen-bestand-upload-melding** app:

![Uitvoer van de app lezen-bestand-upload-melding](media/iot-hub-java-java-upload/read-file-upload-notification.png)

U kunt de portal gebruiken om het geüploade bestand in de storage-container die u hebt geconfigureerd:

![Het geüploade bestand](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de mogelijkheden voor het uploaden van bestand van IoT Hub gebruikt voor het vereenvoudigen van het uploaden van bestanden vanaf apparaten. U kunt doorgaan met het verkennen van IoT hub-functies en scenario's met de volgende artikelen:

* [Een IoT hub via een programma maken](iot-hub-rm-template-powershell.md)
* [Inleiding tot C SDK](iot-hub-device-sdk-c-intro.md)
* [SDK's voor Azure IoT](iot-hub-devguide-sdks.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Een apparaat simuleren met IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
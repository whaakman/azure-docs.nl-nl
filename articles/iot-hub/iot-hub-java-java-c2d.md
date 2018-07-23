---
title: Cloud-naar-apparaat-berichten met Azure IoT Hub (Java) | Microsoft Docs
description: Klik hier voor meer informatie over het cloud-naar-apparaat-berichten verzenden naar een apparaat van een Azure IoT-hub met behulp van de Azure IoT SDK's voor Java. Een gesimuleerde apparaat-app voor het ontvangen van berichten van cloud-naar-apparaat en het wijzigen van een back-end-app om de cloud-naar-apparaat-berichten te verzenden kunt u wijzigen.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: 853754947b8d89af15a8c773a765f33523721e12
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187879"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a>Cloud-naar-apparaat-berichten verzenden met IoT Hub (Java)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub is een volledig beheerde service die stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing. De [aan de slag met IoT Hub] zelfstudie laat zien hoe u een IoT-hub maken, een apparaat-id in het inrichten en code van een gesimuleerde apparaat-app dat apparaat-naar-cloud-berichten verzendt.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze zelfstudie bouwt voort op [aan de slag met IoT Hub]. Hier ziet u hoe aan:

* Vanuit de back-end, cloud-naar-apparaat-berichten naar een enkel apparaat via IoT Hub te verzenden.
* Cloud-naar-apparaat-berichten op een apparaat ontvangen.
* Aanvragen van de back-end, levering bevestiging (*feedback*) voor berichten die worden verzonden naar een apparaat vanuit IoT Hub.

U vindt meer informatie over cloud-naar-apparaat-berichten in de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].

Aan het einde van deze zelfstudie, moet u twee Java-consoletoepassingen uitvoeren:

* **simulated-device**, een aangepaste versie van de app gemaakt [aan de slag met IoT Hub], die verbinding maakt met uw IoT-hub en cloud-naar-apparaat-berichten worden ontvangen.
* **verzenden-c2d-berichten**, die een cloud-naar-apparaat-bericht naar de gesimuleerde apparaattoepassing via IoT Hub verzendt en ontvangt u vervolgens de bevestiging levering.

> [!NOTE]
> IoT-Hub heeft SDK-ondersteuning voor vele platformen voor apparaten en talen (waaronder C, Java en Javascript) via Azure IoT device-SDK's. Zie voor stapsgewijze instructies voor het verbinding maken tussen uw apparaat in de code van deze zelfstudie, en in het algemeen voor Azure IoT Hub, de [Azure IoT-ontwikkelaarscentrum].

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een volledig werkende versie van de [aan de slag met IoT Hub](quickstart-send-telemetry-java.md) of [Process IoT Hub apparaat-naar-cloud-berichten](tutorial-routing.md) zelfstudie.
* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

## <a name="receive-messages-in-the-simulated-device-app"></a>Berichten ontvangen in het gesimuleerde apparaat-app

In deze sectie maakt u de gesimuleerde apparaat-app die u hebt gemaakt in [aan de slag met IoT Hub] cloud-naar-apparaat-berichten ontvangen van de IoT-hub.

1. Open het bestand simulated-device\src\main\java\com\mycompany\app\App.java met een teksteditor.

2. Voeg de volgende **MessageCallback** klasse als een geneste klasse binnen de **App** klasse. De **uitvoeren** methode wordt aangeroepen wanneer het apparaat een bericht van IoT Hub ontvangt. In dit voorbeeld wordt het apparaat altijd bericht verstuurd aan de IoT-hub die het bericht is voltooid:

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. Wijzigen de **belangrijkste** methode voor het maken van een **AppMessageCallback** exemplaar en aanroep de **setMessageCallback** methode voordat deze wordt geopend de client als volgt:

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > Als u HTTPS in plaats van MQTT- of AMQP als het transport, de **DeviceClient** exemplaar wordt gecontroleerd op berichten uit IoT Hub weinig (minder dan elke 25 minuten). Zie voor meer informatie over de verschillen tussen MQTT-, AMQP- en HTTPS-ondersteuning en het beperken van IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].

4. Als u de app **simulated-device** wilt maken met behulp van Maven, geeft u de volgende opdracht op in het opdrachtvenster in de map simulated-device:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a>Een cloud-naar-apparaat-bericht verzenden

In deze sectie maakt maken u een Java-consoletoepassing die cloud-naar-apparaat-berichten naar de gesimuleerde apparaattoepassing verzendt. U moet de apparaat-ID van het apparaat dat u hebt toegevoegd in de [aan de slag met IoT Hub] zelfstudie. U moet ook de IoT Hub-verbindingsreeks voor uw hub die u kunt vinden in de [Azure Portal].

1. Maak een Maven-project met de naam **verzenden-c2d-berichten** met de volgende opdracht in uw opdrachtvenster. Houd rekening met dat deze opdracht wordt een enkele, lange opdracht:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigeer naar de nieuwe map verzenden-c2d-berichten in uw opdrachtvenster.

3. Met een teksteditor, open het bestand pom.xml in de map verzenden-c2d-berichten en voeg de volgende afhankelijkheid aan de **afhankelijkheden** knooppunt. De afhankelijkheid toe te voegen, kunt u de **iothub-java-service-client** -pakket in uw toepassing om te communiceren met uw IoT hub-service:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > U vindt de meest recente versie van **iot-service-client** met [Maven zoeken][lnk-maven-service-search].

4. Sla het bestand pom.xml op en sluit het af.

5. Met een teksteditor, open het bestand send-c2d-messages\src\main\java\com\mycompany\app\App.java.

6. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Voeg de volgende variabelen op klasseniveau naar de **App** klasse, Vervang **{yourhubconnectionstring}** en **{yourdeviceid}** door de waarden die u hebt genoteerd:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. Vervang de **belangrijkste** methode met de volgende code. Deze code maakt verbinding met uw IoT-hub, verzendt een bericht naar uw apparaat en wacht dan tot een bevestiging dat het apparaat ontvangen en het bericht verwerkt:
   
    ```java
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
   
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver();
        if (feedbackReceiver != null) feedbackReceiver.open();
   
        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
   
        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");
   
        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }
   
        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [!NOTE]
    > In deze zelfstudie implementeert voor geeft een beeld van de eenvoud, niet een beleid voor opnieuw proberen. Bij de productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel implementeren [afhandeling van tijdelijke fouten].


9. Als u de app **simulated-device** wilt maken met behulp van Maven, geeft u de volgende opdracht op in het opdrachtvenster in de map simulated-device:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

1. Voer de volgende opdracht om te beginnen met het verzenden van telemetrie naar uw IoT-hub en om te luisteren naar berichten van cloud-naar-apparaat vanaf uw hub verzonden bij een opdrachtprompt in de map simulated-device:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![De gesimuleerde apparaat-app uitvoeren][img-simulated-device]

2. Voer de volgende opdracht om een cloud-naar-apparaat-bericht verzenden en te wachten op een bevestiging feedback bij een opdrachtprompt in de map verzenden-c2d-berichten:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Voer de opdracht uit om de cloud-naar-apparaat-bericht te verzenden][img-send-command]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u cloud-naar-apparaat-berichten verzenden en ontvangen. 

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT-Hub [Azure IoT-Remote Monitoring solution accelerator].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Aan de slag met IoT Hub]: quickstart-send-telemetry-java.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[Azure IoT-ontwikkelaarscentrum]: http://azure.microsoft.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
[Afhandeling van tijdelijke fouten]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure Portal]: https://portal.azure.com
[Azure IoT-Remote Monitoring solution accelerator]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
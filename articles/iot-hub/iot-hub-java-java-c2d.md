---
title: Cloud-naar-apparaat-berichten met Azure IoT Hub (Java) | Microsoft Docs
description: Het cloud-naar-apparaat om berichten te verzenden naar een apparaat van een Azure IoT hub met behulp van de Azure IoT SDK's voor Java. U kunt een gesimuleerde apparaattoepassing cloud-naar-apparaat-berichten ontvangen en wijzigen van een back-end-app voor het verzenden van de cloud-naar-apparaat-berichten wijzigen.
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7f785ea8-e7c2-40c5-87ef-96525e9b9e1e
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: 0c94b627ad80a1cc32bc0084d3792a917225d9c7
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a>Cloud-naar-apparaat-berichten verzenden met IoT Hub (Java)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub is een volledig beheerde service waarmee stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing. De [aan de slag met IoT Hub] zelfstudie laat zien hoe u een iothub maken, een apparaat-id in het inrichten en code van een gesimuleerde apparaattoepassing dat apparaat-naar-cloud-berichten verzendt.

Deze zelfstudie bouwt voort op [aan de slag met IoT Hub]. Hier ziet u hoe aan:

* Van uw back-end oplossing, cloud-naar-apparaat-berichten naar één enkel apparaat via IoT Hub te verzenden.
* Cloud-naar-apparaat-berichten op een apparaat ontvangen.
* Aanvragen van uw back-end oplossing, levering bevestiging (*feedback*) voor berichten die worden verzonden naar een apparaat uit IoT Hub.

U vindt meer informatie over cloud-naar-apparaat-berichten in de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].

Aan het einde van deze zelfstudie, moet u twee Java-apps die console uitvoeren:

* **simulated-device**, een aangepaste versie van de app gemaakt in [aan de slag met IoT Hub], die verbinding maakt met uw IoT-hub en cloud-naar-apparaat-berichten worden ontvangen.
* **verzenden-c2d-berichten**, die verzendt een bericht cloud-naar-apparaat naar de gesimuleerde apparaattoepassing via IoT Hub en vervolgens ontvangt de bevestiging levering.

> [!NOTE]
> IoT-Hub heeft SDK-ondersteuning voor veel apparaatplatforms en talen (inclusief C, Java en Javascript) via Azure IoT-apparaat-SDK's. Zie voor stapsgewijze instructies voor het koppelen van uw apparaat in deze zelfstudie code en in het algemeen naar Azure IoT Hub de [Azure IoT Developer Center].

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een volledige werkende versie van de [aan de slag met IoT Hub](iot-hub-java-java-getstarted.md) of [proces IoT Hub apparaat-naar-cloud-berichten](iot-hub-java-java-process-d2c.md) zelfstudie.
* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

## <a name="receive-messages-in-the-simulated-device-app"></a>Berichten ontvangen in de gesimuleerde apparaattoepassing

In deze sectie, wijzigt u de gesimuleerde apparaattoepassing die u hebt gemaakt in [aan de slag met IoT Hub] cloud-naar-apparaat om berichten te ontvangen van de IoT-hub.

1. Open het bestand simulated-device\src\main\java\com\mycompany\app\App.java met een teksteditor.

2. Voeg de volgende **MessageCallback** klasse als een geneste klasse binnen de **App** klasse. De **uitvoeren** methode wordt aangeroepen wanneer het apparaat een bericht van IoT-Hub ontvangt. In dit voorbeeld wordt het apparaat altijd een melding iothub dat deze het bericht is voltooid:

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. Wijzig de **belangrijkste** methode om te maken een **AppMessageCallback** exemplaar en de aanroep van de **setMessageCallback** methode voordat deze wordt geopend. de client als volgt:

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > Als u HTTPS in plaats van de protocollen MQTT of AMQP zijn als het transport de **DeviceClient** exemplaar wordt gecontroleerd op berichten uit IoT Hub zelden (minder dan elke 25 minuten). Zie voor meer informatie over de verschillen tussen MQTT, AMQP- en HTTPS-ondersteuning en Iothub beperking de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].

4. Als u de app **simulated-device** wilt maken met behulp van Maven, geeft u de volgende opdracht op in het opdrachtvenster in de map simulated-device:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a>Een cloud naar apparaat verzenden

In deze sectie maakt maken u een Java-consoletoepassing dat cloud-naar-apparaat-berichten naar de gesimuleerde apparaattoepassing verzendt. U moet de apparaat-ID van het apparaat dat u hebt toegevoegd in de [aan de slag met IoT Hub] zelfstudie. U moet ook de IoT Hub-verbindingsreeks voor uw hub die u kunt vinden in de [Azure-portal].

1. Maak een Maven-project aangeroepen **verzenden-c2d-berichten** met de volgende opdracht achter de opdrachtprompt. Houd rekening met dat deze opdracht is een enkele, lange opdracht:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigeer naar de nieuwe map voor de send-c2d-berichten bij de opdrachtprompt.

3. Met een teksteditor, open het bestand pom.xml in de map send-c2d-berichten en voeg de volgende afhankelijkheid voor de **afhankelijkheden** knooppunt. De afhankelijkheid toe te voegen, kunt u met de **iothub-java-service-client** pakket in uw toepassing om te communiceren met de service van uw IoT-hub:

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

7. De volgende klasseniveau variabelen toevoegen aan de **App** klasse, Vervang **{yourhubconnectionstring}** en **{yourdeviceid}** met de waarden hebt genoteerd:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. Vervang de **belangrijkste** methode met de volgende code. Deze code maakt verbinding met uw IoT-hub, verzendt een bericht naar uw apparaat en wordt er gewacht totdat een bevestiging dat het apparaat heeft ontvangen en het bericht verwerkt:
   
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
    > Deze zelfstudie implementeert voor de eenvoud mogelijk te houden, niet een beleid voor opnieuw proberen. In productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout].


9. Als u de app **simulated-device** wilt maken met behulp van Maven, geeft u de volgende opdracht op in het opdrachtvenster in de map simulated-device:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

1. Voer de volgende opdracht om te beginnen met het verzenden van telemetrie naar uw IoT-hub en om te luisteren naar de cloud-naar-apparaat-berichten van uw hub bij een opdrachtprompt in de map simulated-device:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![De gesimuleerde apparaattoepassing uitvoeren][img-simulated-device]

2. Voer de volgende opdracht om een cloud naar apparaat verzenden en te wachten op een bevestiging feedback bij een opdrachtprompt in de map send-c2d-berichten:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Voer de opdracht om de cloud-naar-apparaat-bericht te verzenden][img-send-command]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe cloud-naar-apparaat-berichten verzenden en ontvangen. 

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub [Azure IoT Suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[aan de slag met IoT Hub]: iot-hub-java-java-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://azure.microsoft.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
[afhandeling van tijdelijke fout]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
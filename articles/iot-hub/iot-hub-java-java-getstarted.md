---
title: Aan de slag met Azure IoT Hub (Java) | Microsoft Docs
description: Informatie over het verzenden van apparaat-naar-cloud-berichten naar Azure IoT Hub met behulp van IoT SDK's voor Java. U maakt gesimuleerde apparaat- en service-apps om uw apparaat te registreren, berichten te verzenden en berichten uit IoT Hub te lezen.
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 70dae4a8-0e98-4c53-b5a5-9d6963abb245
ms.service: iot-hub
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f41dd62d3f074eebe0c996a9c14bc7811d5b365
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-java"></a>Uw apparaat verbinding laten maken met uw IoT Hub met Java
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Aan het eind van deze zelfstudie beschikt u over drie Java-consoletoepassingen:

* **create-device-identity**: deze toepassing maakt een apparaat-id en de bijbehorende beveiligingssleutel waarmee uw apparaat-app verbonden kan worden.
* **read-d2c-messages**: deze toepassing geeft de telemetrie weer die is verzonden door uw apparaat-app.
* **simulated-device**: deze toepassing koppelt uw IoT-hub aan de apparaat-id die u eerder hebt gemaakt en verzendt iedere seconde een telemetriebericht via het MQTT-protocol.

> [!NOTE]
> Het artikel [Azure IoT-SDK's][lnk-hub-sdks] bevat informatie over de verschillende Azure IoT-SDK's die u kunt gebruiken om beide apps zo te maken dat ze zowel op het apparaat als op de back-end van uw oplossing kunnen worden uitgevoerd.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Als laatste stap noteert u de waarde van de **primaire sleutel**. Klik vervolgens op **Eindpunten** en op het ingebouwde eindpunt **Gebeurtenissen**. Noteer de **Event Hub-compatibele naam** en het adres van het **Event Hub-compatibele eindpunt** op de blade **Eigenschappen**. U hebt deze drie waarden nodig wanneer u uw **read-d2c-messages**-app maakt.

![Blade IoT Hub-berichten in Azure Portal][6]

U hebt nu uw IoT-hub gemaakt. U beschikt over de hostnaam en verbindingsreeks van de IoT Hub, de primaire sleutel van de IoT Hub, de Event Hub-compatibele naam en het Event Hub-compatibele eindpunt. Deze gegevens hebt u nodig voor de rest van deze zelfstudie.

## <a name="create-a-device-identity"></a>Een apparaat-id maken
In dit gedeelte gaat u een Java-consoletoepassing maken die een apparaat-id kan maken in het identiteitenregister van uw IoT Hub. Een apparaat kan geen verbinding maken met de IoT-hub, tenzij het vermeld staat in het id-register. Zie het gedeelte **Id-register** in de [ontwikkelaarshandleiding voor IoT Hub][lnk-devguide-identity] voor meer informatie. Wanneer u deze consoletoepassing uitvoert, worden er een unieke apparaat-id en sleutel gegenereerd waarmee uw apparaat zichzelf kan identificeren tijdens het verzenden van apparaat-naar-cloud-berichten naar IoT Hub.

1. Maak een lege map genaamd iot-java-get-started. Maak een Maven-project in de map iot-java-get-started en noem dit **create-device-identity**. Gebruik hiervoor de volgende opdracht in uw opdrachtvenster. Let op: dit is één enkele, lange opdracht:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigeer vanuit het opdrachtvenster naar de map create-device-identity.

3. Open het bestand pom.xml in de map create-device-identity met een teksteditor en voeg de volgende afhankelijkheden toe aan de node **dependencies**. Met deze afhankelijkheid kunt u het iot-service-client-pakket gebruiken in uw app:

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

5. Open het bestand create-device-identity\src\main\java\com\mycompany\app\App.java met een teksteditor.

6. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang daarbij **{yourhubconnectionstring}** door de waarde die u eerder hebt genoteerd:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. Wijzig de handtekening van de **main**-methode om de uitzonderingen als volgt op te nemen:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. Voeg de volgende code toe als hoofdtekst van de **belangrijkste** methode. Deze code maakt een apparaat in het identiteitsregister van de IoT Hub dat *javadevice* heet als dit nog niet bestaat. Vervolgens worden de apparaat-id en de sleutel weergegeven. Deze hebt u later nodig:

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Sla het bestand App.java op en sluit het af.

11. Als u de app **create-device-identity** wilt maken met behulp van Maven, geeft u de volgende opdracht op in het opdrachtvenster in de map create-device-identity:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. Als u de app **create-device-identity** wilt uitvoeren met behulp van Maven, geeft u de volgende opdracht op in het opdrachtvenster in de map create-device-identity:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Noteer de **apparaat-id** en de **apparaatsleutel**. U hebt deze waarden later nodig wanneer u een app maakt die verbinding maakt met IoT Hub als apparaat.

> [!NOTE]
> In het id-register van IoT Hub worden alleen apparaat-id's opgeslagen waarmee veilig toegang tot de IoT-hub kan worden verkregen. De apparaat-id’s en sleutels worden opgeslagen en gebruikt als beveiligingsreferenties. Met de vlag voor ingeschakeld/uitgeschakeld kunt u toegang tot een afzonderlijk apparaat uitschakelen. Als uw app andere apparaatspecifieke metagegevens moet opslaan, moet deze een app-specifieke opslagmethode gebruiken. Zie de [ontwikkelaarshandleiding voor IoT Hub][lnk-devguide-identity] voor meer informatie.

## <a name="receive-device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten ontvangen

In dit gedeelte maakt u een Java-consoletoepassing die apparaat-naar-cloud-berichten uit IoT Hub kan lezen. Een IoT-hub toont een [Event Hub][lnk-event-hubs-overview]-compatibel eindpunt waarmee u apparaat-naar-cloud-berichten kunt lezen. Om de zaken niet nodeloos ingewikkeld te maken, maakt u met deze handleiding een basislezer die niet geschikt is voor hoge doorvoersnelheden. In de handleiding [Apparaat-naar-cloud-berichten verwerken][lnk-process-d2c-tutorial] leert u hoe u op grote schaal apparaat-naar-cloud-berichten kunt verwerken. In de handleiding [Aan de slag met Event Hubs][lnk-eventhubs-tutorial] leest u meer over het verwerken van berichten van Event Hubs. Deze handleiding is van toepassing op de Event Hub-compatibele eindpunten van IoT Hub.

> [!NOTE]
> Het met Event Hub compatibele eindpunt voor het lezen van apparaat-naar-cloud-berichten maakt altijd gebruik van het AMQP-protocol.

1. Maak een Maven-project in de map iot-java-get-started die u hebt gemaakt in de sectie *Een apparaat-id maken* en noem dit **read-d2c-messages**. Gebruik hiervoor de volgende opdracht in uw opdrachtvenster. Let op: dit is één enkele, lange opdracht:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigeer vanuit het opdrachtvenster naar de map read-d2c-messages.

3. Open het bestand pom.xml in de map read-d2c-messages met een teksteditor en voeg de volgende afhankelijkheden toe aan de node **dependencies**. Met deze afhankelijkheid kunt u het eventhubs-client-pakket in uw app gebruiken om berichten te lezen van het eindpunt dat compatibel is met Event Hub:

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.15.0</version> 
    </dependency>
    ```

4. Sla het bestand pom.xml op en sluit het af.

5. Open het bestand read-d2c-messages\src\main\java\com\mycompany\app\App.java met een teksteditor.

6. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. Voeg de volgende variabele op klasseniveau toe aan de **App**-klasse. Vervang **{youriothubkey}**, **{youreventhubcompatibleendpoint}** en **{youreventhubcompatiblename}** door de waarden die u eerder hebt genoteerd:

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Voeg de volgende **receiveMessages**-methode toe aan de **App**-klasse. Met deze methode maakt u een **EventHubClient**-exemplaar dat verbinding maakt met het Event Hub-compatibele eindpunt. Vervolgens maakt u asynchroon een **PartitionReceiver**-exemplaar voor het lezen van een Event Hub-partitie. Dit wordt continu uitgevoerd en de berichtgegevens worden afgedrukt totdat de app wordt gesloten.

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        // Create a receiver using the
        // default Event Hubs consumer group
        // that listens for messages from now on.
        client.createReceiver(EventHubClient.DEFAULT_CONSUMER_GROUP_NAME, partitionId, Instant.now())
          .thenAccept(new Consumer<PartitionReceiver>() {
            public void accept(PartitionReceiver receiver) {
              System.out.println("** Created receiver on partition " + partitionId);
              try {
                while (true) {
                  Iterable<EventData> receivedEvents = receiver.receive(100).get();
                  int batchSize = 0;
                  if (receivedEvents != null) {
                    System.out.println("Got some events");
                    for (EventData receivedEvent : receivedEvents) {
                      System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s",
                        receivedEvent.getSystemProperties().getOffset(),
                        receivedEvent.getSystemProperties().getSequenceNumber(),
                        receivedEvent.getSystemProperties().getEnqueuedTime()));
                      System.out.println(String.format("| Device ID: %s",
                        receivedEvent.getSystemProperties().get("iothub-connection-device-id")));
                      System.out.println(String.format("| Message Payload: %s",
                        new String(receivedEvent.getBytes(), Charset.defaultCharset())));
                      batchSize++;
                    }
                  }
                  System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId, batchSize));
                }
              } catch (Exception e) {
                System.out.println("Failed to receive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > Deze methode maakt gebruik van een filter bij het maken van de ontvanger, zodat de ontvanger alleen de berichten leest die naar de IoT Hub zijn verzonden nadat de ontvanger actief is geworden. Deze methode is handig in een testomgeving, omdat u zo de huidige reeks berichten kunt zien. In een productieomgeving moet uw code er echter voor zorgen dat alle berichten worden verwerkt. Zie de zelfstudie [IoT Hub apparaat-naar-cloud-berichten verwerken][lnk-process-d2c-tutorial] voor meer informatie.

9. Wijzig de handtekening van de **main**-methode om de uitzondering als volgt op te nemen:

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. Voeg de volgende code aan de **belangrijkste** methode in de **App**-klasse. Deze code wordt gemaakt van de twee **EventHubClient**- en **PartitionReceiver**-instanties. U kunt de app sluiten als u klaar bent met het verwerken van berichten:

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    } catch (Exception e) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > Voor deze code wordt ervan uitgegaan dat u uw IoT-hub in de (gratis) F1-categorie hebt gemaakt. Een gratis IoT-hub heeft twee partities, genaamd "0" en "1".

11. Sla het bestand App.java op en sluit het af.

12. Als u de app **read-d2c-messages** wilt maken met behulp van Maven, geeft u de volgende opdracht op in het opdrachtvenster in de map read-d2c-messages:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Een apparaat-app maken
In dit gedeelte maakt u een Java-consoletoepassing die een apparaat simuleert dat apparaat-naar-cloud-berichten naar een IoT Hub verzendt.

1. Maak een Maven-project in de map iot-java-get-started die u hebt gemaakt in het gedeelte *Een apparaat-id maken* en noem dit **simulated-device**. Gebruik hiervoor de volgende opdracht in uw opdrachtvenster. Let op: dit is één enkele, lange opdracht:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigeer vanuit het opdrachtvenster naar de map simulated-device.

3. Open het bestand pom.xml in de map simulated-device met een teksteditor en voeg de volgende afhankelijkheden toe aan de node **dependencies**. Met deze afhankelijkheid kunt u het iothub-java-client-pakket in uw app gebruiken om te communiceren met uw IoT-hub en om Java-objecten te serialiseren naar JSON:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

    > [!NOTE]
    > U vindt de meest recente versie van **iot-device-client** met [Maven zoeken][lnk-maven-device-search].

4. Sla het bestand pom.xml op en sluit het af.

5. Open het bestand simulated-device\src\main\java\com\mycompany\app\App.java met een teksteditor.

6. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang **{youriothubname}** door de naam van uw IoT Hub en **{yourdevicekey}** door de waarde van de apparaatsleutel die u hebt gegenereerd in het gedeelte *Een apparaat-id maken*:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    Deze voorbeeld-app gebruikt de **protocol**-variabele bij het maken van een **DeviceClient**-object. U kunt het MQTT-, AMQP- of HTTPS-protocol gebruiken om te communiceren met IoT Hub.

8. Voeg de volgende geneste **TelemetryDataPoint**-klasse toe binnen de **App**-klasse om de telemetriegegevens op te geven die uw apparaat verzendt naar uw IoT-hub:

    ```java
    private static class TelemetryDataPoint {
      public String deviceId;
      public double temperature;
      public double humidity;
   
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```
9. Voeg de volgende geneste **EventCallback**-klasse toe binnen de **App**-klasse om de bevestigingsstatus weer te geven die wordt geretourneerd door de IoT-hub wanneer deze een bericht verwerkt van de apparaat-app. Met deze methode wordt ook een bericht verstuurd aan de hoofdthread in de app wanneer het bericht is verwerkt:
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Voeg de volgende geneste **MessageSender**-klasse toe binnen de **App**-klasse. De **uitvoeren**-methode in deze klasse genereert voorbeeldtelemetrie die u kunt verzenden naar uw IoT-hub en die wacht op een bevestiging voordat het volgende bericht wordt verzonden:

    ```java
    private static class MessageSender implements Runnable {
      public void run()  {
        try {
          double minTemperature = 20;
          double minHumidity = 60;
          Random rand = new Random();
    
          while (true) {
            double currentTemperature = minTemperature + rand.nextDouble() * 15;
            double currentHumidity = minHumidity + rand.nextDouble() * 20;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.temperature = currentTemperature;
            telemetryDataPoint.humidity = currentHumidity;
    
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            msg.setProperty("temperatureAlert", (currentTemperature > 30) ? "true" : "false");
            msg.setMessageId(java.util.UUID.randomUUID().toString()); 
            System.out.println("Sending: " + msgStr);
    
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
    
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Deze methode verzendt één seconde nadat de IoT-hub het vorige bericht erkent een nieuw apparaat-naar-cloud bericht. Het bericht bevat een JSON-geserialiseerd object met de deviceId en willekeurig gegenereerde nummers om een temperatuursensor te simuleren, en een vochtigheidssensor.

11. Vervang de **belangrijkste** methode door de volgende code die een thread maakt om apparaat-naar-cloudberichten te verzenden naar uw IoT-hub:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. Sla het bestand App.java op en sluit het af.

13. Als u de app **simulated-device** wilt maken met behulp van Maven, geeft u de volgende opdracht op in het opdrachtvenster in de map simulated-device:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. Voer bij een opdrachtprompt in de map read-d2c de volgende opdracht uit om de eerste partitie in uw IoT Hub te kunnen volgen:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![De Java-app voor IoT Hub-services voor het bewaken van apparaat-naar-cloud-berichten][7]

2. Voer bij een opdrachtprompt in de map simulated-device de volgende opdracht uit om telemetriegegevens naar uw IoT Hub te verzenden:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![De Java-app voor IoT Hub-apparaten voor het bewaken van apparaat-naar-cloud-berichten][8]

3. De tegel **Gebruik** in [Azure Portal][lnk-portal] toont het aantal berichten dat is verzonden naar de IoT-hub:

    ![De tegel Gebruik in Azure Portal met het aantal berichten dat is verzonden naar IoT Hub][43]

## <a name="next-steps"></a>Volgende stappen
In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt deze apparaat-id gebruikt om de apparaat-app in staat te stellen apparaat-naar-cloud-berichten te verzenden naar de IoT-hub. Ook hebt u een app gemaakt die de berichten weergeeft die worden ontvangen door de IoT-hub.

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Verbinding maken met uw apparaat][lnk-connect-device]
* [Aan de slag met apparaatbeheer][lnk-device-management]
* [Aan de slag met Azure IoT Edge][lnk-iot-edge]

Raadpleeg de zelfstudie [Apparaat-naar-cloud-berichten verwerken][lnk-process-d2c-tutorial] voor meer informatie over hoe u uw IoT-oplossing uitbreidt en apparaat-naar-cloud-berichten op schaal verwerkt.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-eventhubs-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22

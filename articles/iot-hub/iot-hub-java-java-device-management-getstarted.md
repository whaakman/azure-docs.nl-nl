---
title: Aan de slag met Azure IoT Hub Apparaatbeheer (Java) | Microsoft Docs
description: "Het gebruik van Azure IoT Hub Apparaatbeheer initiëren externe apparaten opnieuw worden opgestart. U kunt het Azure-IoT-apparaat-SDK voor Java gebruiken voor het implementeren van een gesimuleerde apparaattoepassing met een directe methode en de service Azure IoT SDK voor Java voor het implementeren van een service-app die de directe methode aanroept."
services: iot-hub
documentationcenter: .java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 7e3837582e2020dc560a2b624352f7326ea87c3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-device-management-java"></a>Aan de slag met Apparaatbeheer (Java)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

In deze handleiding ontdekt u hoe u:

* De Azure portal gebruiken voor het maken van een IoT-Hub en een apparaat-id maakt in uw IoT-hub.
* Maak een gesimuleerde apparaattoepassing die een directe methode opnieuw opstarten van het apparaat implementeert. Rechtstreekse methoden worden aangeroepen vanuit de cloud.
* Maak een app die de directe methode voor opnieuw opstarten in de gesimuleerde apparaattoepassing via uw IoT-hub. Deze app controleert vervolgens de gerapporteerde eigenschappen van het apparaat om te zien wanneer de bewerking opnieuw opstarten voltooid is.

Aan het einde van deze zelfstudie hebt u twee Java-apps die console:

**simulated-device**. Deze app:

* Maakt verbinding met uw IoT-hub aan de apparaat-id eerder hebt gemaakt.
* Een aanroep van de directe methode opnieuw opstarten ontvangt.
* Simuleert fysieke computer opnieuw is opgestart.
* De tijd van de laatste keer opnieuw opstarten via een gerapporteerde eigenschap rapporteert.

**trigger-reboot**. Deze app:

* Een directe methode wordt aangeroepen in de gesimuleerde apparaattoepassing.
* Geeft het antwoord op de directe methode-aanroep verzonden door het gesimuleerde apparaat
* Geeft de bijgewerkte gerapporteerd eigenschappen.

> [!NOTE]
> Zie voor meer informatie over de SDK's die u gebruiken kunt om toepassingen uit te voeren op apparaten en de back-end van uw oplossing te bouwen [Azure IoT SDK's][lnk-hub-sdks].

Voor deze zelfstudie hebt u het volgende nodig:

* Java SE 8. <br/> In het gedeelte [Uw ontwikkelingsomgeving voorbereiden][lnk-dev-setup] staat beschreven hoe u Java voor deze handleiding kunt installeren op Windows of Linux.
* Maven 3.  <br/> In het gedeelte [Uw ontwikkelingsomgeving voorbereiden][lnk-dev-setup] staat beschreven hoe u [Maven][lnk-maven] voor deze handleiding kunt installeren op Windows of Linux.
* [Node.js-versie 0.10.0 of hoger](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Activeren van een extern opnieuw opstarten op het apparaat met een directe methode

In deze sectie maakt u een Java-consoletoepassing maken die:

1. De directe methode van opnieuw opstarten in de gesimuleerde apparaattoepassing aanroept.
1. Geeft het antwoord.
1. Polls de gerapporteerde eigenschappen van het apparaat verzonden om te bepalen wanneer het opnieuw opstarten is voltooid.

Deze consoletoepassing maakt verbinding met uw IoT-Hub aan de directe methode aangeroepen en de gerapporteerde eigenschappen worden gelezen.

1. Maak een lege map dm-get-started.

1. Maak een Maven-project aangeroepen in de map dm-get-started **trigger-reboot** met de volgende opdracht achter de opdrachtprompt. Hieronder ziet u een enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigeer naar de trigger-reboot-map bij de opdrachtprompt.

1. Met een teksteditor, open het bestand pom.xml in de map trigger-opnieuw opstarten en voeg de volgende afhankelijkheid voor de **afhankelijkheden** knooppunt. Deze afhankelijkheid kunt u het pakket iot-service-client gebruiken in uw app om te communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > U vindt de meest recente versie van **iot-service-client** met [Maven zoeken][lnk-maven-service-search].

1. Voeg de volgende **bouwen** knooppunt na de **afhankelijkheden** knooppunt. Deze configuratie wordt Maven Java 1.8 gebruiken om de app te bouwen:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Sla het bestand pom.xml op en sluit het af.

1. Met een teksteditor, open het bronbestand trigger-reboot\src\main\java\com\mycompany\app\App.java.

1. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{youriothubconnectionstring}` met uw IoT hub-verbindingsreeks die u hebt genoteerd in de *een IoT Hub maken* sectie:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. Voor het implementeren van een thread die de gerapporteerde eigenschappen van het apparaat twin elke 10 seconden leest toevoegen de volgende geneste klasse naar de **App** klasse:

    ```java
    private static class ShowReportedProperties implements Runnable {
      public void run() {
        try {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          while (true) {
            System.out.println("Get reported properties from device twin");
            deviceTwins.getTwin(twinDevice);
            System.out.println(twinDevice.reportedPropertiesToString());
            Thread.sleep(10000);
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. Wijzig de handtekening van de **belangrijkste** methode de volgende uitzondering:

    ```java
    public static void main(String[] args) throws IOException
    ```

1. Voeg de volgende code aan om aan te roepen de directe methode van opnieuw opstarten op het gesimuleerde apparaat, de **belangrijkste** methode:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      System.out.println("Invoke reboot direct method");
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, null);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }
      System.out.println("Invoked reboot on device");
      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }
    ```

1. Voor het starten van de thread moet peilen op de gerapporteerde eigenschappen van het gesimuleerde apparaat, voeg de volgende code naar de **belangrijkste** methode:

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

1. Zodat u de app stoppen, voeg de volgende code naar de **belangrijkste** methode:

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

1. Sla en sluit het bestand trigger-reboot\src\main\java\com\mycompany\app\App.java.

1. Bouwen de **trigger-reboot** back-endserver voor Apps en eventuele fouten te corrigeren. Navigeer naar de map trigger-opnieuw opstarten en voer de volgende opdracht achter de opdrachtprompt:

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken

In deze sectie maakt u een Java-consoletoepassing die een apparaat simuleert. De app luistert voor het opnieuw opstarten directe aanroep van methode van uw IoT-hub en onmiddellijk reageert op die aanroep. De app vervolgens modus ingeschakeld gedurende het proces te simuleren opnieuw opstarten voordat deze een gerapporteerde eigenschap gebruikt om te melden bij de **trigger-reboot** back-endserver voor apps of het opnieuw opstarten voltooid is.

1. Maak een Maven-project aangeroepen in de map dm-get-started **simulated-device** met de volgende opdracht achter de opdrachtprompt. Hier volgt een enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigeer vanuit het opdrachtvenster naar de map simulated-device.

1. Met een teksteditor, open het bestand pom.xml in de map simulated-device en voeg de volgende afhankelijkheid voor de **afhankelijkheden** knooppunt. Deze afhankelijkheid kunt u het pakket iot-service-client gebruiken in uw app om te communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > U vindt de meest recente versie van **iot-device-client** met [Maven zoeken][lnk-maven-device-search].

1. Voeg de volgende **bouwen** knooppunt na de **afhankelijkheden** knooppunt. Deze configuratie wordt Maven Java 1.8 gebruiken om de app te bouwen:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Sla het bestand pom.xml op en sluit het af.

1. Met een teksteditor, open het bestand simulated-device\src\main\java\com\mycompany\app\App.java bron.

1. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.time.LocalDateTime;
    import java.util.Scanner;
    import java.util.Set;
    import java.util.HashSet;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{yourdeviceconnectionstring}` met de apparaat-verbindingsreeks die u hebt genoteerd in de *maken van een apparaat-id* sectie:

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

1. Voor het implementeren van een retouraanroep-handler voor directe methode statusgebeurtenissen toevoegen de volgende geneste klasse naar de **App** klasse:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. Voor het implementeren van een retouraanroep-handler voor apparaat twin statusgebeurtenissen toevoegen de volgende geneste klasse naar de **App** klasse:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

1. Voor het implementeren van een retouraanroep-handler voor gebeurtenissen van de eigenschap toevoegen de volgende geneste klasse naar de **App** klasse:

    ```java
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

1. Voor het implementeren van een thread om te simuleren van het apparaat opnieuw opstarten, voeg de volgende geneste klasse om te de **App** klasse. De thread modus ingeschakeld gedurende vijf seconden en vervolgens stelt de **lastReboot** eigenschap gerapporteerd:

    ```java
    protected static class RebootDeviceThread implements Runnable {
      public void run() {
        try {
          System.out.println("Rebooting...");
          Thread.sleep(5000);
          Property property = new Property("lastReboot", LocalDateTime.now());
          Set<Property> properties = new HashSet<Property>();
          properties.add(property);
          client.sendReportedProperties(properties);
          System.out.println("Rebooted");
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

1. Als u wilt implementeren de directe methode op het apparaat, voeg de volgende geneste klasse om te de **App** klasse. Wanneer de gesimuleerde app ontvangt een aanroep van de **opnieuw opstarten** directe methode op deze retourneert een bevestiging naar de aanroeper en een thread voor het verwerken van het opnieuw opstarten wordt gestart:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "reboot" :
          {
            int status = METHOD_SUCCESS;
            System.out.println("Received reboot request");
            deviceMethodData = new DeviceMethodData(status, "Started reboot");
            RebootDeviceThread rebootThread = new RebootDeviceThread();
            Thread t = new Thread(rebootThread);
            t.start();
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

1. Wijzig de handtekening van de **belangrijkste** methode om de volgende uitzonderingen:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. Het instantiëren van een **DeviceClient**, voeg de volgende code naar de **belangrijkste** methode:

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

1. Om te beginnen met luisteren voor directe methode-aanroepen, voegt u de volgende code aan de **belangrijkste** methode:

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed to direct methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Als u wilt de apparaatsimulator wordt afgesloten, voeg de volgende code naar de **belangrijkste** methode:

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. Sla en sluit het bestand simulated-device\src\main\java\com\mycompany\app\App.java.

1. Bouwen de **simulated-device** back-endserver voor Apps en eventuele fouten te corrigeren. Navigeer naar de map simulated-device en voer de volgende opdracht achter de opdrachtprompt:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. Voer bij een opdrachtprompt in de map simulated-device de volgende opdracht om te beginnen met luisteren naar de methodeaanroepen opnieuw opstarten van uw IoT-hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![De gesimuleerde apparaattoepassing Java IoT-Hub te luisteren naar directe methodeaanroepen opnieuw opstarten][1]

1. Bij een opdrachtprompt in de map trigger-opnieuw opstarten, voer de volgende opdracht om de methode opnieuw opstarten op het gesimuleerde apparaat aanroepen vanuit uw IoT-hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Service-app Java IoT-Hub aan te roepen de directe methode voor opnieuw opstarten][2]

1. Het gesimuleerde apparaat reageert op de aanroep van de directe methode opnieuw opstarten:

    ![De gesimuleerde apparaattoepassing IoT-Hub voor Java reageert op de aanroep van directe methode][3]

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[1]: ./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png
[2]: ./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png
[3]: ./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png
<!-- Links -->

[lnk-maven]: https://maven.apache.org/what-is-maven.html

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
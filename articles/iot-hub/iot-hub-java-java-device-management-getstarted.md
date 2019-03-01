---
title: Aan de slag met Apparaatbeheer via Azure IoT Hub (Java) | Microsoft Docs
description: Het gebruik van Apparaatbeheer via Azure IoT Hub om te beginnen met een extern apparaat opnieuw opstarten. U de Azure IoT-device-SDK voor Java gebruiken voor het implementeren van een gesimuleerde apparaat-app met een rechtstreekse methode en de Azure IoT service SDK voor Java voor het implementeren van een service-app die de directe methode aanroept.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: 272f4cfd4d79ef6d4fca3f3c00afb1c251649db8
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010749"
---
# <a name="get-started-with-device-management-java"></a>Aan de slag met Apparaatbeheer (Java)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

In deze zelfstudie ontdekt u hoe u:

* De Azure portal gebruiken voor het maken van een IoT-Hub en een apparaat-id maken in uw IoT-hub.

* Een gesimuleerde apparaat-app die een rechtstreekse methode om het apparaat opnieuw te maken. Directe methoden zijn aangeroepen vanuit de cloud.

* Maak een app die de directe methode die opnieuw worden opgestart in de gesimuleerde apparaattoepassing via uw IoT-hub roept. Deze app controleert vervolgens de gerapporteerde eigenschappen van het apparaat om te zien wanneer de bewerking opnieuw opstarten voltooid is.

Aan het einde van deze zelfstudie, beschikt u over twee Java-consoletoepassingen:

**simulated-device**. Deze app:

* Maakt verbinding met uw IoT-hub aan de apparaat-id die eerder hebt gemaakt.

* Een aanroep van de directe methode opnieuw opstarten ontvangt.

* Simuleert fysieke opnieuw worden opgestart.

* De tijd van de laatste keer opnieuw opstarten via een gerapporteerde eigenschap rapporteert.

**trigger-reboot**. Deze app:

* Een rechtstreekse methode aanroepen in het gesimuleerde apparaat-app.

* Het antwoord op het aanroepen van de directe methode die is verzonden door het gesimuleerde apparaat weergegeven.

* Geeft de bijgewerkte gerapporteerde eigenschappen.

> [!NOTE]
> Zie voor meer informatie over de SDK's die u gebruiken kunt om toepassingen uit te voeren op apparaten en de back-end van uw oplossing te bouwen, [Azure IoT SDK's](iot-hub-devguide-sdks.md).

Voor deze zelfstudie hebt u het volgende nodig:

* Java SE 8. <br/> [Uw ontwikkelomgeving voorbereiden](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md) wordt beschreven hoe u Java voor deze zelfstudie installeren op Windows of Linux.

* Maven 3.  <br/> [Uw ontwikkelomgeving voorbereiden](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md) wordt beschreven hoe u voor het installeren van [Maven](https://maven.apache.org/what-is-maven.html) voor deze zelfstudie op Windows of Linux.

* [Node.js versie 0.10.0 of hoger](http://nodejs.org).

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Verbindingsreeks voor IoT-hub ophalen

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Een extern opnieuw opstarten op het apparaat met een rechtstreekse methode activeren

In deze sectie maakt u een Java-consoletoepassing die:

1. Roept de directe methode van opnieuw opstarten in het gesimuleerde apparaat-app.

2. Het antwoord weergegeven.

3. Controleert de gerapporteerde eigenschappen van het apparaat verzonden om te bepalen wanneer het opnieuw opstarten is voltooid.

Deze console-app maakt verbinding met uw IoT-Hub aan de rechtstreekse methode aanroepen en de gerapporteerde eigenschappen lezen.

1. Maak een lege map genaamd dm-get-started.

2. Maak een Maven-project met de naam in de map dm-get-started **trigger-opnieuw opstarten** met de volgende opdracht in uw opdrachtvenster. Hieronder ziet u een enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

3. Navigeer naar de map van de trigger-opnieuw opstarten in uw opdrachtvenster.

4. Met een teksteditor, open het bestand pom.xml in de map van de trigger-opnieuw opstarten en voeg de volgende afhankelijkheid aan de **afhankelijkheden** knooppunt. Met deze afhankelijkheid kunt u de iot-service-client-pakket in uw app om te communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > U kunt controleren voor de meest recente versie van **iot-service-client** met behulp van [Maven zoeken](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

5. Voeg de volgende **bouwen** knooppunt na de **afhankelijkheden** knooppunt. Deze configuratie geeft Maven Java 1.8 gebruiken om de app te bouwen:

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

6. Sla het bestand pom.xml op en sluit het af.

7. Met een teksteditor, open het bronbestand trigger-reboot\src\main\java\com\mycompany\app\App.java.

8. Voeg de volgende **import**instructies toe aan het bestand:

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

9. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{youriothubconnectionstring}` met uw IoT hub-verbindingsreeks die u hebt genoteerd in de *maken van een IoT-Hub* sectie:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

10. Voor het implementeren van een thread die de gerapporteerde eigenschappen van het dubbele apparaat elke tien seconden leest, toevoegen de volgende geneste klasse om de **App** klasse:

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

11. Wijzig de handtekening van de **belangrijkste** methode voor het gebruik de volgende uitzondering:

    ```java
    public static void main(String[] args) throws IOException
    ```

12. Voor het aanroepen van de directe methode van opnieuw opstarten op het gesimuleerde apparaat, voeg de volgende code aan de **belangrijkste** methode:

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

13. Voor het starten van de thread voor het pollen van de gerapporteerde eigenschappen van het gesimuleerde apparaat, voeg de volgende code aan de **belangrijkste** methode:

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

14. Voeg de volgende code aan zodat u kunt de app stoppen de **belangrijkste** methode:

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

15. Sla op en sluit het bestand trigger-reboot\src\main\java\com\mycompany\app\App.java.

16. Bouw de **trigger-opnieuw opstarten** back-end-app en eventuele fouten te corrigeren. Navigeer naar de map van de trigger-opnieuw opstarten bij de opdrachtprompt en voer de volgende opdracht uit:

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken

In deze sectie maakt u een Java-consoletoepassing die een apparaat simuleert. De app-meldingen voor het opnieuw opstarten direct methodeaanroep van uw IoT-hub en direct reageren op die aanroep. De app en de inactieve modus ingeschakeld gedurende even om te simuleren dat het proces opnieuw opstarten voordat het een gerapporteerde eigenschap gebruikt om u te waarschuwen de **trigger-opnieuw opstarten** back-end-app of het opnieuw opstarten voltooid is.

1. Maak een Maven-project met de naam in de map dm-get-started **simulated-device** met de volgende opdracht in uw opdrachtvenster. Hier volgt een enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

2. Navigeer vanuit het opdrachtvenster naar de map simulated-device.

3. Met een teksteditor, open het bestand pom.xml in de map simulated-device en voeg de volgende afhankelijkheid aan de **afhankelijkheden** knooppunt. Met deze afhankelijkheid kunt u de iot-service-client-pakket in uw app om te communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > U kunt controleren voor de meest recente versie van **iot-device-client** met behulp van [Maven zoeken](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

4. Voeg de volgende **bouwen** knooppunt na de **afhankelijkheden** knooppunt. Deze configuratie geeft Maven Java 1.8 gebruiken om de app te bouwen:

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

5. Sla het bestand pom.xml op en sluit het af.

6. Open met een teksteditor het bestand simulated-device\src\main\java\com\mycompany\app\App.java bron.

7. Voeg de volgende **import**instructies toe aan het bestand:

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

7. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{yourdeviceconnectionstring}` met de apparaatverbindingsreeks die u hebt genoteerd in de *maken van een apparaat-id* sectie:

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

8. Voor het implementeren van een retouraanroep-handler voor directe methode statusgebeurtenissen toevoegen de volgende geneste klasse om de **App** klasse:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

9. Voor het implementeren van een retouraanroep-handler voor apparaatdubbel statusgebeurtenissen toevoegen de volgende geneste klasse om de **App** klasse:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

10. Voor het implementeren van een retouraanroep-handler voor gebeurtenissen van de eigenschap toevoegen de volgende geneste klasse om de **App** klasse:

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

11. Voor het implementeren van een thread voor het simuleren van het apparaat opnieuw opstarten, toevoegen de volgende geneste klasse om de **App** klasse. De thread inactieve modus ingeschakeld gedurende vijf seconden en stelt vervolgens de **lastReboot** gerapporteerde eigenschap:

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

12. Voor het implementeren van de directe methode die op het apparaat, voeg de volgende geneste klasse om de **App** klasse. Wanneer de gesimuleerde app ontvangt een aanroep naar de **opnieuw opstarten** directe methode op deze retourneert een bevestiging voor de oproepende functie en start vervolgens een thread voor het verwerken van het opnieuw opstarten:

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

13. Wijzig de handtekening van de **belangrijkste** methode voor het genereren van de volgende uitzonderingen:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

14. Exemplaar maken van een **DeviceClient**, voeg de volgende code aan de **belangrijkste** methode:

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

15. Als u wilt beginnen met luisteren voor rechtstreekse methodeaanroepen, voeg de volgende code aan de **belangrijkste** methode:

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

16. Als u wilt de apparaatsimulator afsluiten, voeg de volgende code aan de **belangrijkste** methode:

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

17. Opslaan en sluiten van het bestand simulated-device\src\main\java\com\mycompany\app\App.java.

18. Bouw de **simulated-device** back-end-app en eventuele fouten te corrigeren. Bij de opdrachtprompt, gaat u naar de map simulated-device en voer de volgende opdracht uit:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. Bij een opdrachtprompt in de map simulated-device de volgende opdracht uit om te luisteren naar de methodeaanroepen opnieuw opstarten van uw IoT-hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![IoT-Hub voor Java gesimuleerde apparaat-app om te luisteren naar rechtstreekse methodeaanroepen opnieuw opstarten](./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png)

2. Bij een opdrachtprompt in de map van de trigger-opnieuw opstarten, voer de volgende opdracht uit om de methode voor opnieuw opstarten op het gesimuleerde apparaat aanroepen vanuit uw IoT-hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![IoT-Hub voor Java service-app voor het aanroepen van de directe methode voor opnieuw opstarten](./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png)

3. Het gesimuleerde apparaat reageert op de aanroep van de directe methode opnieuw opstarten:

    ![Gesimuleerde apparaat-app voor IoT-Hub voor Java reageert op de aanroep van de directe methode](./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]
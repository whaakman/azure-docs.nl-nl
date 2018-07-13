---
title: Firmware-update van het apparaat met Azure IoT Hub (Java/Java) | Microsoft Docs
description: Het beheer van apparaten op Azure IoT Hub gebruiken om een firmware-update van het apparaat. U gebruikt de Azure IoT-device-SDK voor Java voor het implementeren van een gesimuleerde apparaat-app en voor het implementeren van een service-app die wordt geactiveerd de firmware-update.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: dobett
ms.openlocfilehash: 5991615bca26749e1f138b561260108f8bcf2646
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611324"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-javajava"></a>Apparaatbeheer gebruiken om een apparaat firmware-update (Java/Java)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

In de [aan de slag met Apparaatbeheer] [ lnk-dm-getstarted] zelfstudie, hebt u gezien hoe u de [apparaatdubbel] [ lnk-devtwin] en [directe methoden ] [ lnk-c2dmethod] primitieven aan een apparaat op afstand opnieuw opstarten. In deze zelfstudie maakt gebruik van de dezelfde IoT Hub-primitieven en ziet u hoe u een end-to-end gesimuleerde firmware-update doet.  Dit patroon wordt gebruikt in de firmware-update-implementatie voor de [Raspberry Pi apparaat implementatie voorbeeld][lnk-rpi-implementation].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze handleiding ontdekt u hoe u:

* Een Java-consoletoepassing die roept de **firmwareUpdate** directe methode op het gesimuleerde apparaat-app via uw IoT-hub.
* Een Java-consoletoepassing die overeenkomt met het apparaat en implementeert de **firmwareUpdate** directe methode. Deze methode initieert een proces met meerdere fasen dat wacht om te downloaden van de firmware-installatiekopie, wordt de firmware-installatiekopie gedownload en ten slotte past de firmware-installatiekopie. Tijdens elke fase van de update wordt het apparaat gebruikt van de gerapporteerde eigenschappen om te rapporteren over hun voortgang.

Aan het einde van deze zelfstudie, beschikt u over twee Java-consoletoepassingen:

**firmware-update**, een rechtstreekse methode aanroepen op het gesimuleerde apparaat, wordt het antwoord weergegeven en wordt periodiek gerapporteerde eigenschap updates

**simulated-device**, maakt verbinding met uw IoT-hub aan de eerder gemaakte apparaat-id, ontvangt de aanroep van de directe methode firmwareUpdate en wordt uitgevoerd via een firmware-update-simulatie

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Activeren van een externe firmware-update op het apparaat met een rechtstreekse methode
In deze sectie maakt u een Java-consoletoepassing die een externe firmware-update op een apparaat initieert. De app maakt gebruik van een rechtstreekse methode om de update te initiëren en apparaatdubbel-query's gebruikt om periodiek de status van de actieve firmware-update.

1. Maak een lege map genaamd fw-get-started.

1. Maak een Maven-project met de naam in de map fw-get-started **firmware-update** met de volgende opdracht in uw opdrachtvenster. Let op: dit is één enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=firmware-update -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigeer naar de map-firmware-update in uw opdrachtvenster.

1. Met een teksteditor, open het bestand pom.xml in de map-firmware-update en voeg de volgende afhankelijkheid aan de **afhankelijkheden** knooppunt. Met deze afhankelijkheid kunt u de iot-service-client-pakket in uw app om te communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.5.22</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > U kunt controleren voor de meest recente versie van **iot-service-client** met behulp van [Maven zoeken](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Voeg de volgende **bouwen** knooppunt na de **afhankelijkheden** knooppunt. Deze configuratie geeft Maven Java 1.8 gebruiken om de app te bouwen:

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

1. Met een teksteditor, open het bronbestand firmware-update\src\main\java\com\mycompany\app\App.java.

1. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang **{youriothubconnectionstring}** met uw IoT hub-verbindingsreeks die u hebt genoteerd in de *maken van een IoT-Hub* sectie:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "firmwareUpdate";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. Voor het implementeren van een methode die de gerapporteerde eigenschappen van de apparaatdubbel leest, het volgende toevoegen aan de **App** klasse:

    ```java
    public static void ShowReportedProperties() 
    {
        try 
        {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          
          Boolean firmwareUpdated = false;
          Integer timeoutCycle = 0;
    
          while (!firmwareUpdated)
          {
            if (timeoutCycle > 5)
            {
              System.out.println("Operation timed out");
              break;
            }
    
            Thread.sleep(1000);
              
            deviceTwins.getTwin(twinDevice);
    
            String reportedProperties = twinDevice.reportedPropertiesToString();
              
            if(reportedProperties.contains("status=waiting"))
            {
              System.out.println("Waiting on device...");
            }
            else if(reportedProperties.contains("status=downloadComplete"))
            {
              System.out.println("Download complete, applying firmware...");
            }
            else if (reportedProperties.contains("status=applyComplete"))
            {
              System.out.println("Firmware applied");
              System.out.println("Get reported properties from device twin");
              System.out.println(twinDevice.reportedPropertiesToString());
              firmwareUpdated = true;
            }
            else
            {
              timeoutCycle++;
            }
          }
        } catch (Exception ex) {
            System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
    }
    ```

1. Wijzig de handtekening van de **belangrijkste** methode voor het genereren van de volgende uitzonderingen:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Voor het aanroepen van de directe methode firmwareUpdate op het gesimuleerde apparaat, voeg de volgende code aan de **belangrijkste** methode:

    ```java
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      String payload = "https://someurl";
      
      System.out.println("Invoked firmware update on device.");
      System.out.println("Sent URL: " + payload);
      
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }

      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
      System.out.println(e.getMessage());
    }
    ```

1. Als u wilt laten pollen de gerapporteerde eigenschappen van het gesimuleerde apparaat, voeg de volgende code aan de **belangrijkste** methode:

    ```java
    ShowReportedProperties();
    ```

1. Voeg de volgende code aan zodat u kunt de app stoppen de **belangrijkste** methode:

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    System.out.println("Shutting down sample...");
    ```

1. Sla op en sluit het bestand firmware-update\src\main\java\com\mycompany\app\App.java.

1. Bouw de **firmware-update** back-end-app en eventuele fouten te corrigeren. Bij de opdrachtprompt, gaat u naar de map-firmware-update en voer de volgende opdracht uit:

    `mvn clean package -DskipTests`

## <a name="simulate-a-device-to-handle-direct-method-calls"></a>Een apparaat voor het afhandelen van rechtstreekse methodeaanroepen simuleren
In deze sectie maakt u een Java-console gesimuleerde apparaat-app die de directe methode die firmwareUpdate kan ontvangen. De app wordt uitgevoerd via een proces met meerdere staat voor het simuleren van de firmware-update reportedProperties gebruiken om te communiceren van status.

1. Maak een Maven-project met de naam in de map fw-get-started **simulated-device** met de volgende opdracht in uw opdrachtvenster. Let op: dit is één enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigeer vanuit het opdrachtvenster naar de map simulated-device.

1. Met een teksteditor, open het bestand pom.xml in de map-firmware-update en voeg de volgende afhankelijkheid aan de **afhankelijkheden** knooppunt. Met deze afhankelijkheid kunt u de iot-service-client-pakket in uw app om te communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > U kunt controleren voor de meest recente versie van **iot-device-client** met behulp van [Maven zoeken](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Voeg de volgende **bouwen** knooppunt na de **afhankelijkheden** knooppunt. Deze configuratie geeft Maven Java 1.8 gebruiken om de app te bouwen:

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

1. Open met een teksteditor het bestand simulated-device\src\main\java\com\mycompany\app\App.java bron.

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
    import java.util.HashMap;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang **{yourdeviceconnectionstring}** met de apparaatverbindingsreeks die u hebt genoteerd in de *maken van een apparaat-id* sectie:

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring";
    private static DeviceClient client;

    private static String downloadURL = "unknown";
    ```

1. Voor het implementeren van de directe methode functionaliteit bieden callbacks door toe te voegen van de volgende geneste klassen op de **App** klasse:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "firmwareUpdate" :
          {
            System.out.println("Response to method '" + methodName + "' sent successfully");
    
            downloadURL = new String((byte[])methodData);
    
            int status = METHOD_SUCCESS;
            System.out.println("Starting firmware update");
            deviceMethodData = new DeviceMethodData(status, "Started firmware update");
            FirmwareUpdateThread firmwareUpdateThread = new FirmwareUpdateThread();
            Thread t = new Thread(firmwareUpdateThread);
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

1. Voor het implementeren van de functionaliteit van de dubbele apparaten bieden callbacks door toe te voegen van de volgende geneste klassen op de **App** klasse:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

1. Voor het implementeren van de firmware-update, voeg de volgende geneste klasse om de **App** klasse:

    ```java
    protected static class FirmwareUpdateThread implements Runnable {
      public void run() {
        try {      
          HashMap initialUpdate = new HashMap();
          Property sentProperty = new Property("firmwareUpdate", initialUpdate);
          Set<Property> sentPackage = new HashSet<Property>();
          
          System.out.println("Downloading from " + downloadURL);
    
          initialUpdate.put("status", "waiting");
          initialUpdate.put("fwPackageUri", downloadURL);
          initialUpdate.put("startedWaitingTime", LocalDateTime.now().toString());   
          sentPackage.add(sentProperty);
    
          client.sendReportedProperties(sentPackage);
    
          Thread.sleep(5000);
    
          System.out.println("Download complete");
    
          HashMap downloadUpdate = new HashMap();
          downloadUpdate.put("status","downloadComplete");
          downloadUpdate.put("downloadCompleteTime", LocalDateTime.now().toString());
          downloadUpdate.put("startedApplyingImage", LocalDateTime.now().toString());
          sentProperty.setValue(downloadUpdate);
    
          client.sendReportedProperties(sentPackage);
    
          Thread.sleep(5000);
    
          System.out.println("Apply complete");
    
          HashMap applyUpdate = new HashMap();
          applyUpdate.put("status","applyComplete");
          applyUpdate.put("lastFirmwareUpdate", LocalDateTime.now().toString());
          sentProperty.setValue(applyUpdate);
    
          client.sendReportedProperties(sentPackage);
    
          Thread.sleep(5000);
    
          HashMap resetUpdate = new HashMap();
          applyUpdate.put("status","reset");
          sentProperty.setValue(resetUpdate);
    
          client.sendReportedProperties(sentPackage);
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

1. Wijzig de handtekening van de **belangrijkste** methode voor het genereren van de volgende uitzonderingen:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. Voor het starten van de directe methoden en device twins routine, voeg de volgende code aan de **belangrijkste** methode:

    ```java
    client = new DeviceClient(connString, protocol);

    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Zodat u kunt de app stoppen, kunt u de volgende code toevoegen aan het einde van de **belangrijkste** methode:

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. Opslaan en sluiten van het bestand simulated-device\src\main\java\com\mycompany\app\App.java.

1. Bouw de **simulated-device** app en eventuele fouten te corrigeren. Bij de opdrachtprompt, gaat u naar de map simulated-device en voer de volgende opdracht uit:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij een opdrachtprompt in de **simulated-device** map, de volgende opdracht uit om te luisteren naar de directe methode die firmware-update.
   
    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

1. Bij een opdrachtprompt in de **firmware-update** map, voer de volgende opdracht het aanroepen van de firmware-update en het opvragen van de dubbele apparaten op uw gesimuleerde apparaat bij uw IoT-hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

3. U kunt zien dat het gesimuleerde apparaat reageert op de directe methode die in de console.

    ![Firmware is bijgewerkt][img-fwupdate]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een rechtstreekse methode gebruikt voor het activeren van een externe firmware-update op een apparaat en gebruikt de gerapporteerde eigenschappen om te volgen van de voortgang van de firmware-update.

Zie voor informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten roept, de [taken plannen en uitzenden] [ lnk-tutorial-jobs] zelfstudie.

<!-- images -->
[img-fwupdate]: media/iot-hub-java-java-firmware-update/firmwareUpdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-java-java-device-management-getstarted.md
[lnk-tutorial-jobs]: iot-hub-java-java-schedule-jobs.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
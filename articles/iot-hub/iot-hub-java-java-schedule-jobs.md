---
title: Plannen van taken met Azure IoT Hub (Java) | Microsoft Docs
description: Klik hier voor meer informatie over het plannen van een taak Azure IoT Hub is een directe methode aangeroepen en een gewenste eigenschap instellen op meerdere apparaten. U kunt het Azure-IoT-apparaat-SDK voor Java gebruiken voor het implementeren van het gesimuleerde apparaat-apps en de service Azure IoT SDK voor Java voor het implementeren van een service-app als de taak wilt uitvoeren.
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dobett
ms.openlocfilehash: 54f446f8735bc46b87fe19aaf7845c5fbfce2744
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="schedule-and-broadcast-jobs-java"></a>Planning en broadcast-taken (Java)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub gebruiken om te plannen en bijhouden van taken die miljoenen apparaten worden bijgewerkt. Taken die moeten worden gebruikt:

* Gewenste eigenschappen bijwerken
* Labels bijwerken
* Directe methoden aanroepen

Een taak een van deze acties worden verpakt en volgt de uitvoering op basis van een reeks apparaten. Een apparaat twin query definieert de set van apparaten die op de taak wordt uitgevoerd. Een back-endserver voor apps kunt bijvoorbeeld een taak gebruiken om aan te roepen een directe methode op 10.000 apparaten die de apparaten opnieuw wordt opgestart. U opgeven welke set apparaten met een apparaat twin query en plannen van de taak wilt uitvoeren op een later tijdstip. De voortgang van de taak houdt aangezien aan elk van de apparaten ontvangen en uitvoeren van de directe methode voor opnieuw opstarten.

Zie voor meer informatie over elk van deze mogelijkheden:

* Apparaat-twin en eigenschappen: [aan de slag met horende apparaten](iot-hub-java-java-twin-getstarted.md)
* Directe methoden: [IoT Hub developer guide - rechtstreekse methoden](iot-hub-devguide-direct-methods.md) en [zelfstudie: directe methoden gebruiken](iot-hub-java-java-direct-methods.md)

In deze handleiding ontdekt u hoe u:

* Maakt een apparaat-app die u een directe methode aangeroepen implementeert **lockDoor**. De apparaat-app ontvangt ook gewenste wijzigingen van de back-endserver voor apps.
* Maakt een back-end-app die u maakt een taak aan te roepen de **lockDoor** directe methode op meerdere apparaten. Een andere taak verzendt de gewenste eigenschap updates op meerdere apparaten.

Aan het einde van deze zelfstudie hebt u een java-consoletoepassing apparaat en het hulpprogramma voor het back-end van een java-consoletoepassing:

**simulated-device** die is verbonden met uw IoT-hub implementeert het **lockDoor** directe methode en ingangen gewenst eigenschapswijzigingen.

**taken plannen** die gebruikmaken van taken aan te roepen de **lockDoor** directe methode en het bijwerken van de eigenschappen van de gewenste twin apparaten op meerdere apparaten.

> [!NOTE]
> Het artikel [Azure IoT SDK's](iot-hub-devguide-sdks.md) bevat informatie over de Azure IoT SDK's dat u gebruiken kunt om zowel apparaatgegevens als back-end-apps te bouwen.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](http://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Als u liever de apparaat-id via een programma maken, leest u de bijbehorende sectie in het [uw apparaat aansluit op uw IoT-hub met Java](iot-hub-java-java-getstarted.md#create-a-device-identity) artikel. U kunt ook de [IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) hulpprogramma naar een apparaat toevoegt aan uw IoT-hub.

## <a name="create-the-service-app"></a>De service-app maken

In deze sectie kunt u een Java-consoletoepassing die gebruikmaakt van taken te maken:

* Roep de **lockDoor** directe methode op meerdere apparaten.
* Eigenschappen van de gewenste verzenden naar meerdere apparaten.

Maken van de app:

1. Maak een lege map genaamd op uw ontwikkelcomputer `iot-java-schedule-jobs`.

1. In de `iot-java-schedule-jobs` map, maak een Maven-project aangeroepen **taken plannen** met de volgende opdracht achter de opdrachtprompt. Let op: dit is één enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Ga bij de opdrachtprompt naar de `schedule-jobs` map.

1. Open een teksteditor de `pom.xml` bestand de `schedule-jobs` map en voeg de volgende afhankelijkheid voor de **afhankelijkheden** knooppunt. Deze afhankelijkheid kunt u met de **iot-service-client** pakket in uw app om te communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > U kunt controleren voor de meest recente versie van **iot-service-client** met [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

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

1. Sla op en sluit de `pom.xml` bestand.

1. Open een teksteditor de `schedule-jobs\src\main\java\com\mycompany\app\App.java` bestand.

1. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Pair;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Query;
    import com.microsoft.azure.sdk.iot.service.devicetwin.SqlQuery;
    import com.microsoft.azure.sdk.iot.service.jobs.JobClient;
    import com.microsoft.azure.sdk.iot.service.jobs.JobResult;
    import com.microsoft.azure.sdk.iot.service.jobs.JobStatus;

    import java.util.Date;
    import java.time.Instant;
    import java.util.HashSet;
    import java.util.Set;
    import java.util.UUID;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{youriothubconnectionstring}` met uw IoT hub-verbindingsreeks die u hebt genoteerd in de *een IoT Hub maken* sectie:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long the job is permitted to run without
    // completing its work on the set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. Toevoegen van de volgende methode voor de **App** klasse plannen van een taak voor het bijwerken de **gebouw** en **Floor** gewenst eigenschappen in de apparaat-twin:

    ```java
    private static JobResult scheduleJobSetDesiredProperties(JobClient jobClient, String jobId) {
      DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
      Set<Pair> desiredProperties = new HashSet<Pair>();
      desiredProperties.add(new Pair("Building", 43));
      desiredProperties.add(new Pair("Floor", 3));
      twin.setDesiredProperties(desiredProperties);
      // Optimistic concurrency control
      twin.setETag("*");

      // Schedule the update twin job to run now
      // against a single device
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleUpdateTwin(jobId, 
          "deviceId='" + deviceId + "'",
          twin,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling desired properties job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    }
    ```

1. Een taak aan te roepen plannen de **lockDoor** methode, voegt u de volgende methode voor de **App** klasse:

    ```java
    private static JobResult scheduleJobCallDirectMethod(JobClient jobClient, String jobId) {
      // Schedule a job now to call the lockDoor direct method
      // against a single device. Response and connection
      // timeouts are set to 5 seconds.
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleDeviceMethod(jobId,
          "deviceId='" + deviceId + "'",
          "lockDoor",
          5L, 5L, null,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling direct method job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    };
    ```

1. Voor het bewaken van een taak, voegt u de volgende methode voor de **App** klasse:

    ```java
    private static void monitorJob(JobClient jobClient, String jobId) {
      try {
        JobResult jobResult = jobClient.getJob(jobId);
        if(jobResult == null)
        {
          System.out.println("No JobResult for: " + jobId);
          return;
        }
        // Check the job result until it's completed
        while(jobResult.getJobStatus() != JobStatus.completed)
        {
          Thread.sleep(100);
          jobResult = jobClient.getJob(jobId);
          System.out.println("Status " + jobResult.getJobStatus() + " for job " + jobId);
        }
        System.out.println("Final status " + jobResult.getJobStatus() + " for job " + jobId);
      } catch (Exception e) {
        System.out.println("Exception monitoring job: " + jobId);
        System.out.println(e.getMessage());
        return;
      }
    }
    ```

1. Om te vragen voor de details van de taken die u hebt uitgevoerd, voegt u de volgende methode toe:

    ```java
    private static void queryDeviceJobs(JobClient jobClient, String start) throws Exception {
      System.out.println("\nQuery device jobs since " + start);

      // Create a jobs query using the time the jobs started
      Query deviceJobQuery = jobClient
          .queryDeviceJob(SqlQuery.createSqlQuery("*", SqlQuery.FromType.JOBS, "devices.jobs.startTimeUtc > '" + start + "'", null).getQuery());

      // Iterate over the list of jobs and print the details
      while (jobClient.hasNextJob(deviceJobQuery)) {
        System.out.println(jobClient.getNextJob(deviceJobQuery));
      }
    }
    ```

1. Update de **belangrijkste** methodehandtekening om op te nemen van de volgende `throws` component:

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. Als u wilt uitvoeren en controleren van twee taken opeenvolgend, voeg de volgende code naar de **belangrijkste** methode:

    ```java
    // Record the start time
    String start = Instant.now().toString();

    // Create JobClient
    JobClient jobClient = JobClient.createFromConnectionString(iotHubConnectionString);
    System.out.println("JobClient created with success");

    // Schedule twin job desired properties
    // Maximum concurrent jobs is 1 for Free and S1 tiers
    String desiredPropertiesJobId = "DPCMD" + UUID.randomUUID();
    scheduleJobSetDesiredProperties(jobClient, desiredPropertiesJobId);
    monitorJob(jobClient, desiredPropertiesJobId);

    // Schedule twin job direct method
    String directMethodJobId = "DMCMD" + UUID.randomUUID();
    scheduleJobCallDirectMethod(jobClient, directMethodJobId);
    monitorJob(jobClient, directMethodJobId);

    // Run a query to show the job detail
    queryDeviceJobs(jobClient, start);

    System.out.println("Shutting down schedule-jobs app");
    ```

1. Sla op en sluit de `schedule-jobs\src\main\java\com\mycompany\app\App.java` bestand

1. Bouwen de **taken plannen** app en eventuele fouten te corrigeren. Ga bij de opdrachtprompt naar de `schedule-jobs` map en voer de volgende opdracht:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Een apparaat-app maken

In deze sectie maakt u een Java-consoletoepassing die de gewenste eigenschappen verzonden uit IoT Hub worden verwerkt en implementeert de directe methode-aanroep.

1. In de `iot-java-schedule-jobs` map, maak een Maven-project aangeroepen **simulated-device** met de volgende opdracht achter de opdrachtprompt. Let op: dit is één enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Ga bij de opdrachtprompt naar de `simulated-device` map.

1. Open een teksteditor de `pom.xml` bestand de `simulated-device` map en voeg de volgende afhankelijkheden van de **afhankelijkheden** knooppunt. Deze afhankelijkheid kunt u met de **iot-apparaat-client** pakket in uw app om te communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > U kunt controleren voor de meest recente versie van **iot-apparaat-client** met [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

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

1. Sla op en sluit de `pom.xml` bestand.

1. Open een teksteditor de `simulated-device\src\main\java\com\mycompany\app\App.java` bestand.

1. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervangen van `{youriothubname}` met de naam van uw IoT-hub en `{yourdevicekey}` waarde met sleutel voor het apparaat u hebt gegenereerd tijdens de *maken van een apparaat-id* sectie:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Deze voorbeeld-app gebruikt de **protocol**-variabele bij het maken van een **DeviceClient**-object.

1. Apparaat twin meldingen naar de console om af te drukken toevoegen de volgende geneste klasse naar de **App** klasse:

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    ```

1. Directe methode meldingen naar de console om af te drukken toevoegen de volgende geneste klasse naar de **App** klasse:

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to direct method operation with status " + status.name());
      }
    }
    ```

1. Voor het afhandelen van directe methodeaanroepen van IoT Hub toevoegen de volgende geneste klasse naar de **App** klasse:

    ```java
    // Handler for direct method calls from IoT Hub
    protected static class DirectMethodCallback
        implements DeviceMethodCallback {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context) {
        DeviceMethodData deviceMethodData;
        switch (methodName) {
          case "lockDoor": {
            System.out.println("Executing direct method: " + methodName);
            deviceMethodData = new DeviceMethodData(METHOD_SUCCESS, "Executed direct method " + methodName);
            break;
          }
          default: {
            deviceMethodData = new DeviceMethodData(METHOD_NOT_DEFINED, "Not defined direct method " + methodName);
          }
        }
        // Notify IoT Hub of result
        return deviceMethodData;
      }
    }
    ```

1. Update de **belangrijkste** methodehandtekening om op te nemen van de volgende `throws` component:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. Voeg de volgende code naar de **belangrijkste** methode:
    * Een apparaat-client om te communiceren met IoT Hub maken.
    * Maak een **apparaat** object voor het opslaan van de eigenschappen van de twee apparaten.

    ```java
    // Create a device client
    DeviceClient client = new DeviceClient(connString, protocol);

    // An object to manage device twin desired and reported properties
    Device dataCollector = new Device() {
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context)
      {
        System.out.println("Received desired property change: " + propertyKey + " " + propertyValue);
      }
    };
    ```

1. Voor het starten van de clientservices apparaat, voeg de volgende code naar de **belangrijkste** methode:

    ```java
    try {
      // Open the DeviceClient
      // Start the device twin services
      // Subscribe to direct method calls
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
    } catch (Exception e) {
      System.out.println("Exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

1. Wachten tot de gebruiker te druk op de **Enter** sleutel voordat u afsluit, voeg de volgende code toe aan het einde van de **belangrijkste** methode:

    ```java
    // Close the app
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. Sla op en sluit de `simulated-device\src\main\java\com\mycompany\app\App.java` bestand.

1. Bouwen de **simulated-device** app en eventuele fouten te corrigeren. Ga bij de opdrachtprompt naar de `simulated-device` map en voer de volgende opdracht:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>De apps uitvoeren

U bent nu klaar om uit te voeren van de console-apps.

1. Bij een opdrachtprompt in de `simulated-device` map, voer de volgende opdracht om de apparaat-app luisteren naar de gewenste wijzigingen en directe methodeaanroepen te starten:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![De apparaatclient wordt gestart](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. Bij een opdrachtprompt in de `schedule-jobs` map, voer de volgende opdracht om uit te voeren de **taken plannen** service-app twee taken uitvoeren. De eerste optie stelt u de gewenste eigenschapswaarden, de tweede de directe methode aanroept:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Service-IoT-Hub voor Java-app maakt u twee taken](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. De apparaat-app omgaat met de gewenste wijziging en de aanroep directe methode:

    ![De apparaatclient reageert op wijzigingen](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt gemaakt met een back-end-app om twee taken uitvoeren. De eerste taak gewenste eigenschapswaarden instellen en de tweede taak heeft een directe methode aangeroepen.

Gebruik de volgende bronnen voor meer informatie over hoe:

* Verzenden van telemetrie vanaf apparaten met de [aan de slag met IoT Hub](iot-hub-java-java-getstarted.md) zelfstudie.
* Beheren van apparaten interactief (zoals het inschakelen van een ventilator van een gebruiker beheerde app) met de [direct methoden gebruiken](iot-hub-java-java-direct-methods.md) zelfstudie.

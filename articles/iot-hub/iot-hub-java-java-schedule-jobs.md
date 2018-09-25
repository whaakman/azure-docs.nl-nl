---
title: Plannen van taken met Azure IoT Hub (Java) | Microsoft Docs
description: Klik hier voor meer informatie over het plannen van een Azure IoT Hub-taak voor een rechtstreekse methode aanroepen en een gewenste eigenschap instellen op meerdere apparaten. U de Azure IoT-device-SDK voor Java gebruiken om de gesimuleerde apparaat-apps en de Azure IoT service SDK voor Java voor het implementeren van een service-app voor het uitvoeren van de taak te implementeren.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 07/10/2017
ms.author: dobett
ms.openlocfilehash: 7d41732103bd76e281c2a669a649645c8ff5bc73
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957979"
---
# <a name="schedule-and-broadcast-jobs-java"></a>Taken plannen en uitzenden (Java)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Gebruik Azure IoT Hub voor het plannen en bijhouden van taken door miljoenen apparaten bij te werken. Taken die moeten worden gebruikt:

* Gewenste eigenschappen bijwerken
* Bijwerken van tags
* Directe methoden aanroepen

Een taak een van deze acties wordt verpakt en houdt de uitvoering op basis van een reeks apparaten. Een apparaat dubbele query definieert de set met apparaten die op de taak wordt uitgevoerd. Een back-end-app kunt bijvoorbeeld een taak aan een rechtstreekse methode aanroepen op 10.000 apparaten die de apparaten opnieuw is opgestart. U geeft de set met apparaten met een dubbel apparaatquery en plannen van de taak wordt uitgevoerd op een later tijdstip. De voortgang van de taak worden bijgehouden als elk van de apparaten ontvangen en voer de directe methode die opnieuw worden opgestart.

Zie voor meer informatie over elk van deze mogelijkheden:

* Apparaatdubbel en eigenschappen: [aan de slag met apparaatdubbels](iot-hub-java-java-twin-getstarted.md)
* Directe methoden: [het Ontwikkelaarshandleiding voor IoT Hub - directe methoden](iot-hub-devguide-direct-methods.md) en [zelfstudie: directe methoden gebruiken](quickstart-control-device-java.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze zelfstudie ontdekt u hoe u:

* Maakt een apparaat-app die u een rechtstreekse methode met de naam implementeert **lockDoor**. De apparaat-app ontvangt ook wijzigingen in de gewenste eigenschappen van de back-end-app.
* Maakt een back-end-app die u maakt een taak aan te roepen de **lockDoor** directe methode op meerdere apparaten. Een andere taak verzendt gewenste eigenschap updates voor meerdere apparaten.

Aan het einde van deze zelfstudie hebt u het hulpprogramma voor het apparaat van een java-consoletoepassing en het hulpprogramma voor het back-end van een java-consoletoepassing:

**simulated-device** die is verbonden met uw IoT-hub implementeert het **lockDoor** directe methode en verwerkt de gewenste wijzigingen in de eigenschappen.

**taken plannen** die gebruikmaken van taken om aan te roepen de **lockDoor** directe methode en bijwerken van de gewenste apparaatdubbeleigenschappen op meerdere apparaten.

> [!NOTE]
> Het artikel [Azure IoT SDK's](iot-hub-devguide-sdks.md) bevat informatie over de Azure IoT SDK's die u gebruiken kunt om apparaat- en back-end-apps te bouwen.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](http://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

U kunt ook de [IoT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension) hulpprogramma op een apparaat toevoegt aan uw IoT-hub.

## <a name="create-the-service-app"></a>De service-app maken

In deze sectie maakt maken u een Java-consoletoepassing die gebruikmaakt van taken te:

* Roep de **lockDoor** directe methode op meerdere apparaten.
* Gewenste eigenschappen verzenden naar meerdere apparaten.

De app maken:

1. Maak op uw ontwikkelcomputer een lege map genaamd `iot-java-schedule-jobs`.

1. In de `iot-java-schedule-jobs` map, maak een Maven-project met de naam **taken plannen** met de volgende opdracht in uw opdrachtvenster. Let op: dit is één enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Bij de opdrachtprompt, gaat u naar de `schedule-jobs` map.

1. Open met behulp van een teksteditor en de `pom.xml` -bestand in de `schedule-jobs` map en voeg de volgende afhankelijkheid aan de **afhankelijkheden** knooppunt. Met deze afhankelijkheid kunt u gebruikmaken van de **iot-service-client** -pakket in uw app kan communiceren met uw IoT-hub:

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

1. Opslaan en sluiten de `pom.xml` bestand.

1. Open met behulp van een teksteditor en de `schedule-jobs\src\main\java\com\mycompany\app\App.java` bestand.

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

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{youriothubconnectionstring}` met uw IoT hub-verbindingsreeks die u hebt genoteerd in de *maken van een IoT-Hub* sectie:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long the job is permitted to run without
    // completing its work on the set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. Toevoegen van de volgende methode aan de **App** klasse voor het plannen van een taak om bij te werken de **gebouw** en **Floor** gewenste eigenschappen op het dubbele apparaat:

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

1. Voor het plannen van een taak aan te roepen de **lockDoor** methode, voegt u de volgende methode naar de **App** klasse:

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

1. Voor het controleren van een taak, voegt u de volgende methode naar de **App** klasse:

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

1. Update de **belangrijkste** metody om op te nemen van de volgende `throws` component:

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. Als u wilt uitvoeren en bewaken van twee taken sequentieel worden verwerkt, voeg de volgende code aan de **belangrijkste** methode:

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

1. Opslaan en sluiten de `schedule-jobs\src\main\java\com\mycompany\app\App.java` bestand

1. Bouw de **taken plannen** app en eventuele fouten te corrigeren. Bij de opdrachtprompt, gaat u naar de `schedule-jobs` map en voer de volgende opdracht:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Een apparaat-app maken

In deze sectie maakt u een Java-consoletoepassing die werkt met de gewenste eigenschappen van IoT Hub verzonden, en implementeert de aanroep van de directe methode.

1. In de `iot-java-schedule-jobs` map, maak een Maven-project met de naam **simulated-device** met de volgende opdracht in uw opdrachtvenster. Let op: dit is één enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Bij de opdrachtprompt, gaat u naar de `simulated-device` map.

1. Open met behulp van een teksteditor en de `pom.xml` -bestand in de `simulated-device` map en voeg de volgende afhankelijkheden aan de **afhankelijkheden** knooppunt. Met deze afhankelijkheid kunt u gebruikmaken van de **iot-device-client** -pakket in uw app kan communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
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

1. Opslaan en sluiten de `pom.xml` bestand.

1. Open met behulp van een teksteditor en de `simulated-device\src\main\java\com\mycompany\app\App.java` bestand.

1. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervangen van `{youriothubname}` met de naam van uw IoT-hub en `{yourdevicekey}` door de sleutelwaarde die u hebt gegenereerd in de *maken van een apparaat-id* sectie:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Deze voorbeeld-app gebruikt de **protocol**-variabele bij het maken van een **DeviceClient**-object.

1. Toevoegen als u wilt afdrukken apparaat dubbele meldingen naar de console, de volgende geneste klasse om de **App** klasse:

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    ```

1. Toevoegen als u wilt afdrukken directe methode meldingen naar de console, de volgende geneste klasse om de **App** klasse:

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to direct method operation with status " + status.name());
      }
    }
    ```

1. Voor het afhandelen van rechtstreekse methodeaanroepen van IoT Hub, toevoegen de volgende geneste klasse om de **App** klasse:

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

1. Update de **belangrijkste** metody om op te nemen van de volgende `throws` component:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. Voeg de volgende code aan de **belangrijkste** methode:
    * Een apparaatclient om te communiceren met IoT Hub maken.
    * Maak een **apparaat** object voor het opslaan van de eigenschappen van het dubbele apparaat.

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

1. Voor het starten van de services van de device-client, voeg de volgende code aan de **belangrijkste** methode:

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

1. Na afloop van de gebruiker op de **Enter** sleutel voordat u afsluit, voeg de volgende code toe aan het einde van de **belangrijkste** methode:

    ```java
    // Close the app
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. Opslaan en sluiten de `simulated-device\src\main\java\com\mycompany\app\App.java` bestand.

1. Bouw de **simulated-device** app en eventuele fouten te corrigeren. Bij de opdrachtprompt, gaat u naar de `simulated-device` map en voer de volgende opdracht:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>De apps uitvoeren

U bent nu klaar om uit te voeren van de console-apps.

1. Bij een opdrachtprompt in de `simulated-device` map, voer de volgende opdracht om de apparaat-app die luisteren naar wijzigingen in de gewenste eigenschappen en rechtstreekse methodeaanroepen te starten:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![De apparaatclient wordt gestart](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. Bij een opdrachtprompt in de `schedule-jobs` map, voer de volgende opdracht om uit te voeren de **taken plannen** service-app twee taken uit te voeren. De eerste Hiermee stelt u de gewenste eigenschapswaarden, de tweede de directe methode aanroept:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![IoT-Hub voor Java service-app maakt u twee taken](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. De apparaat-app wordt gebruikt voor het wijzigen van de gewenste eigenschap en aanroepen van de directe methode:

    ![Client van het apparaat reageert op de wijzigingen](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt gemaakt met een back-end-app voor twee taken uitvoeren. De eerste taak gewenste eigenschapswaarden instellen en de tweede taak met de naam een rechtstreekse methode.

Gebruik de volgende bronnen voor meer informatie over het:

* Verzenden van telemetrie van apparaten met de [aan de slag met IoT Hub](quickstart-send-telemetry-java.md) zelfstudie.
* Beheren van apparaten interactief (zoals het inschakelen van een fan, van een gebruiker beheerde app) met de [directe methoden gebruiken](quickstart-control-device-java.md) zelfstudie.

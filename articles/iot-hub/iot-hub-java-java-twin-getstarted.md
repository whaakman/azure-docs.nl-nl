---
title: Aan de slag met Azure IoT Hub apparaat horende (Java) | Microsoft Docs
description: Het gebruik van Azure IoT Hub apparaat horende labels toevoegen en vervolgens met de query voor een IoT Hub. U kunt het Azure-IoT-apparaat-SDK voor Java gebruiken voor het implementeren van de app voor apparaat en de Azure IoT service SDK voor Java voor het implementeren van een service-app die de labels worden toegevoegd en de IoT Hub-query wordt uitgevoerd.
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
ms.date: 07/04/2017
ms.author: dobett
ms.openlocfilehash: 6d306d4742a53789d8e69c80d7fbdfc4e1ade4bf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-device-twins-java"></a>Aan de slag met apparaat horende (Java)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

In deze zelfstudie maakt maken u twee console Java-apps:

* **toevoegen-tags-query**, een Java-back-end-app die labels toegevoegd en wordt opgevraagd horende apparaten.
* **simulated-device**, een Java-apparaat app dat verbinding maakt met uw IoT-hub en rapporteert de connectiviteit van de voorwaarde gebruik van een eigenschap gemeld.

> [!NOTE]
> Het artikel [Azure IoT SDK's](iot-hub-devguide-sdks.md) bevat informatie over de Azure IoT SDK's dat u gebruiken kunt om zowel apparaatgegevens als back-end-apps te bouwen.

Voor deze zelfstudie hebt u het volgende nodig:

* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](http://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Als u liever de apparaat-id via een programma maken, leest u de bijbehorende sectie in het [uw apparaat aansluit op uw IoT-hub met Java](iot-hub-java-java-getstarted.md#create-a-device-identity) artikel.

## <a name="create-the-service-app"></a>De service-app maken

In deze sectie maakt u een Java-app die locatie metagegevens wordt toegevoegd als een label aan de apparaat-twin in IoT-Hub die zijn gekoppeld aan **myDeviceId**. De app eerst een query uit iothub voor apparaten die zich in de Verenigde Staten en voor apparaten die rapporteren van de verbinding van een mobiel netwerk.

1. Maak een lege map genaamd op uw ontwikkelcomputer `iot-java-twin-getstarted`.

1. In de `iot-java-twin-getstarted` map, maak een Maven-project aangeroepen **toevoegen-tags-query** met de volgende opdracht achter de opdrachtprompt. Let op: dit is één enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Ga bij de opdrachtprompt naar de `add-tags-query` map.

1. Open een teksteditor de `pom.xml` bestand de `add-tags-query` map en voeg de volgende afhankelijkheid voor de **afhankelijkheden** knooppunt. Deze afhankelijkheid kunt u met de **iot-service-client** pakket in uw app om te communiceren met uw IoT-hub:

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

1. Open een teksteditor de `add-tags-query\src\main\java\com\mycompany\app\App.java` bestand.

1. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{youriothubconnectionstring}` met uw IoT hub-verbindingsreeks die u hebt genoteerd in de *een IoT Hub maken* sectie:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. Update de **belangrijkste** methodehandtekening om op te nemen van de volgende `throws` component:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Voeg de volgende code naar de **belangrijkste** methode voor het maken van de **DeviceTwin** en **DeviceTwinDevice** objecten. De **DeviceTwin** object verwerkt de communicatie met uw IoT-hub. De **DeviceTwinDevice** object vertegenwoordigt de apparaat-twin met eigenschappen en tags:

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. Voeg de volgende `try/catch` blokkeren tot de **belangrijkste** methode:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. Bijwerken van de **regio** en **plant** apparaat twin labels in uw apparaat twin, voeg de volgende code in de `try` blokkeren:

    ```java
    // Get the device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from the existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create the tags and attach them to the DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update the device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve the device twin with the tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

1. Voeg de volgende code aan om te vragen de horende apparaten in IoT-hub, de `try` blok na de code die u in de vorige stap hebt toegevoegd. De code wordt uitgevoerd twee query's. Elke query retourneert maximaal 100 apparaten:

    ```java
    // Query the device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct the query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run the query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct the query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run the query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

1. Sla op en sluit de `add-tags-query\src\main\java\com\mycompany\app\App.java` bestand

1. Bouwen de **toevoegen-tags-query** app en eventuele fouten te corrigeren. Ga bij de opdrachtprompt naar de `add-tags-query` map en voer de volgende opdracht:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Een apparaat-app maken

In deze sectie maakt maken u een Java-consoletoepassing die stelt u een eigenschapswaarde gemeld dat wordt verzonden naar IoT Hub.

1. In de `iot-java-twin-getstarted` map, maak een Maven-project aangeroepen **simulated-device** met de volgende opdracht achter de opdrachtprompt. Let op: dit is één enkele, lange opdracht:

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
    private static String deviceId = "myDeviceId";
    ```

    Deze voorbeeld-app gebruikt de **protocol**-variabele bij het maken van een **DeviceClient**-object. 

1. Voeg de volgende code naar de **belangrijkste** methode:
    * Een apparaat-client om te communiceren met IoT Hub maken.
    * Maak een **apparaat** object voor het opslaan van de eigenschappen van de twee apparaten.

    ```java
    DeviceClient client = new DeviceClient(connString, protocol);

    // Create a Device object to store the device twin properties
    Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed to " + propertyValue);
      }
    };
    ```

1. Voeg de volgende code naar de **belangrijkste** methode voor het maken van een **connectivityType** eigenschap gemeld en dit te verzenden naar IoT Hub:

    ```java
    try {
      // Open the DeviceClient and start the device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it to your IoT hub.
      dataCollector.setReportedProp(new Property("connectivityType", "cellular"));
      client.sendReportedProperties(dataCollector.getReportedProp());
    }
    catch (Exception e) {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Voeg de volgende code toe aan het einde van de **belangrijkste** methode. Wachten op de **Enter** sleutel kan tijd voor IoT Hub voor het rapporteren van de status van het apparaat twin bewerkingen:

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. Sla op en sluit de `simulated-device\src\main\java\com\mycompany\app\App.java` bestand.

1. Bouwen de **simulated-device** app en eventuele fouten te corrigeren. Ga bij de opdrachtprompt naar de `simulated-device` map en voer de volgende opdracht:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>De apps uitvoeren

U bent nu klaar om uit te voeren van de console-apps.

1. Bij een opdrachtprompt in de `add-tags-query` map, voer de volgende opdracht om uit te voeren de **toevoegen-tags-query** service-app:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![IoT Hub voor Java-service-app bijwerken labelwaarden en apparaten query's uitvoeren](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    U ziet de **plant** en **regio** toegevoegd aan de apparaat-twin labels. De eerste query retourneert het apparaat, maar de tweede niet.

1. Bij een opdrachtprompt in de `simulated-device` map, voer de volgende opdracht om toe te voegen de **connectivityType** eigenschap gerapporteerd aan de apparaat-twin:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![De apparaatclient voegt de ** connectivityType ** eigenschap gerapporteerd](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. Bij een opdrachtprompt in de `add-tags-query` map, voer de volgende opdracht om uit te voeren de **toevoegen-tags-query** service-app een tweede keer:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![IoT Hub voor Java-service-app bijwerken labelwaarden en apparaten query's uitvoeren](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Nu u uw apparaat heeft verzonden de **connectivityType** eigenschap IoT-hub, de tweede query retourneert het apparaat.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U metagegevens van apparaten als labels toegevoegd vanuit een back-end-app en een apparaattoepassing geschreven naar apparaten connectiviteit rapportgegevens in de apparaat-twin. U hebt ook geleerd hoe query uitvoeren op de apparaatgegevens twin met behulp van de SQL-achtige IoT Hub-querytaal.

Gebruik de volgende bronnen voor meer informatie over hoe:

* Verzenden van telemetrie vanaf apparaten met de [aan de slag met IoT Hub](iot-hub-java-java-getstarted.md) zelfstudie.
* Beheren van apparaten interactief (zoals het inschakelen van een ventilator van een gebruiker beheerde app) met de [direct methoden gebruiken](iot-hub-java-java-direct-methods.md) zelfstudie.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md

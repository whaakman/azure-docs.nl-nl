---
title: Aan de slag met apparaatdubbels Azure IoT Hub (Java) | Microsoft Docs
description: Het gebruik van Azure IoT Hub-apparaatdubbels tags toevoegen en vervolgens een IoT Hub-query. U de Azure IoT-device-SDK voor Java gebruiken voor het implementeren van de apparaat-app en de Azure IoT service SDK voor Java voor het implementeren van een service-app die wordt toegevoegd de labels en de IoT Hub-query wordt uitgevoerd.
author: dominicbetts
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: dobett
ms.openlocfilehash: a19eb99f6400e57f93c3a8d18c472ba6324383db
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155521"
---
# <a name="get-started-with-device-twins-java"></a>Aan de slag met apparaatdubbels (Java)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

In deze zelfstudie maakt u twee Java-consoletoepassingen:

* **toevoegen-tags-query**, een Java-back-end-app die wordt toegevoegd tags en apparaatdubbels-query's.
* **simulated-device**, een Java-apparaat-app die is verbonden met uw IoT hub en rapporteert de voorwaarde van de connectiviteit met behulp van een gerapporteerde eigenschap.

> [!NOTE]
> Het artikel [Azure IoT SDK's](iot-hub-devguide-sdks.md) bevat informatie over de Azure IoT SDK's die u gebruiken kunt om apparaat- en back-end-apps te bouwen.

Voor deze zelfstudie hebt u het volgende nodig:

* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

* [Maven 3](https://maven.apache.org/install.html)

* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](http://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-service-app"></a>De service-app maken

In deze sectie maakt u een Java-app die metagegevens van de locatie wordt toegevoegd als een argument naar de apparaatdubbel in IoT-Hub die is gekoppeld aan **myDeviceId**. De app eerst een query uit IoT hub voor apparaten die zich in de Verenigde Staten, en vervolgens voor apparaten die rapporteren van de verbinding van een mobiel netwerk.

1. Maak op uw ontwikkelcomputer een lege map genaamd `iot-java-twin-getstarted`.

2. In de `iot-java-twin-getstarted` map, maak een Maven-project met de naam **toevoegen-tags-query** met de volgende opdracht in uw opdrachtvenster. Let op: dit is één enkele, lange opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

3. Bij de opdrachtprompt, gaat u naar de `add-tags-query` map.

4. Open met behulp van een teksteditor en de `pom.xml` -bestand in de `add-tags-query` map en voeg de volgende afhankelijkheid aan de **afhankelijkheden** knooppunt. Met deze afhankelijkheid kunt u gebruikmaken van de **iot-service-client** -pakket in uw app kan communiceren met uw IoT-hub:

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

6. Opslaan en sluiten de `pom.xml` bestand.

7. Open met behulp van een teksteditor en de `add-tags-query\src\main\java\com\mycompany\app\App.java` bestand.

8. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

9. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{youriothubconnectionstring}` met uw IoT hub-verbindingsreeks die u hebt genoteerd in de *maken van een IoT-Hub* sectie:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

10. Update de **belangrijkste** metody om op te nemen van de volgende `throws` component:

    ```java
    public static void main( String[] args ) throws IOException
    ```

11. Voeg de volgende code aan de **belangrijkste** methode voor het maken van de **DeviceTwin** en **DeviceTwinDevice** objecten. De **DeviceTwin** object verwerkt de communicatie met uw IoT-hub. De **DeviceTwinDevice** object vertegenwoordigt het dubbele apparaat met de eigenschappen en tags:

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

12. Voeg de volgende `try/catch` blokkeren tot de **belangrijkste** methode:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

13. Om bij te werken de **regio** en **fabriek** apparaat dubbele tags in de apparaatdubbel, voeg de volgende code in de `try` blokkeren:

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

14. Om te vragen de dubbele apparaten in IoT hub, voeg de volgende code aan de `try` blokkeren na de code die u in de vorige stap hebt toegevoegd. De code wordt uitgevoerd twee query's. Elke query retourneert een maximum van 100 apparaten:

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

15. Opslaan en sluiten de `add-tags-query\src\main\java\com\mycompany\app\App.java` bestand

16. Bouw de **toevoegen-tags-query** app en eventuele fouten te corrigeren. Bij de opdrachtprompt, gaat u naar de `add-tags-query` map en voer de volgende opdracht:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Een apparaat-app maken

In deze sectie maakt u een Java-consoletoepassing die Hiermee stelt u de waarde van een gerapporteerde eigenschap die wordt verzonden naar IoT Hub.

1. In de `iot-java-twin-getstarted` map, maak een Maven-project met de naam **simulated-device** met de volgende opdracht in uw opdrachtvenster. Let op: dit is één enkele, lange opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Bij de opdrachtprompt, gaat u naar de `simulated-device` map.

3. Open met behulp van een teksteditor en de `pom.xml` -bestand in de `simulated-device` map en voeg de volgende afhankelijkheden aan de **afhankelijkheden** knooppunt. Met deze afhankelijkheid kunt u gebruikmaken van de **iot-device-client** -pakket in uw app kan communiceren met uw IoT-hub:

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

5. Opslaan en sluiten de `pom.xml` bestand.

6. Open met behulp van een teksteditor en de `simulated-device\src\main\java\com\mycompany\app\App.java` bestand.

7. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

8. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervangen van `{youriothubname}` met de naam van uw IoT-hub en `{yourdevicekey}` door de sleutelwaarde die u hebt gegenereerd in de *maken van een apparaat-id* sectie:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    Deze voorbeeld-app gebruikt de **protocol**-variabele bij het maken van een **DeviceClient**-object. 

9. Voeg de volgende code aan de **belangrijkste** methode:
    * Een apparaatclient om te communiceren met IoT Hub maken.
    * Maak een **apparaat** object voor het opslaan van de eigenschappen van het dubbele apparaat.

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

10. Voeg de volgende code aan de **belangrijkste** methode voor het maken van een **connectivityType** gerapporteerde eigenschap en te verzenden naar IoT Hub:

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

11. Voeg de volgende code toe aan het einde van de **belangrijkste** methode. Wachten op de **Enter** sleutel kan tijd voor IoT Hub kunt u de status van de bewerkingen van de dubbele apparaat rapporteren:

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

12. Opslaan en sluiten de `simulated-device\src\main\java\com\mycompany\app\App.java` bestand.

13. Bouw de **simulated-device** app en eventuele fouten te corrigeren. Bij de opdrachtprompt, gaat u naar de `simulated-device` map en voer de volgende opdracht:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-apps"></a>De apps uitvoeren

U bent nu klaar om uit te voeren van de console-apps.

1. Bij een opdrachtprompt in de `add-tags-query` map, voer de volgende opdracht om uit te voeren de **toevoegen-tags-query** service-app:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![IoT-Hub voor Java service-app voor het bijwerken van tagwaarden en apparaat-query's uitvoeren](./media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    U ziet de **fabriek** en **regio** tags toegevoegd aan het dubbele apparaat. De eerste query retourneert het apparaat, maar de tweede niet.

2. Bij een opdrachtprompt in de `simulated-device` map, voer de volgende opdracht uit om toe te voegen de **connectivityType** eigenschap gerapporteerd aan het dubbele apparaat:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![De apparaatclient voegt de ** connectivityType ** gerapporteerde eigenschap](./media/iot-hub-java-java-twin-getstarted/device-app-1.png)

3. Bij een opdrachtprompt in de `add-tags-query` map, voer de volgende opdracht om uit te voeren de **toevoegen-tags-query** service-app een tweede keer:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![IoT-Hub voor Java service-app voor het bijwerken van tagwaarden en apparaat-query's uitvoeren](./media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Nu uw apparaat heeft verzonden de **connectivityType** eigenschap naar IoT Hub, de tweede query retourneert het apparaat.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U metagegevens van apparaten als labels toegevoegd vanuit een back-end-app en een apparaat-app geschreven naar apparaatgegevens met connectiviteit van rapport in de apparaatdubbel. U hebt ook geleerd hoe u query's van de dubbele apparaatgegevens met behulp van de SQL-achtige IoT Hub-querytaal.

Gebruik de volgende bronnen voor meer informatie over het:

* Verzenden van telemetrie van apparaten met de [aan de slag met IoT Hub](quickstart-send-telemetry-java.md) zelfstudie.

* Beheren van apparaten interactief (zoals het inschakelen van een fan, van een gebruiker beheerde app) met de [directe methoden gebruiken](quickstart-control-device-java.md) zelfstudie.
---
title: Aan de slag met Azure IoT Hub Device apparaatdubbels (Java) | Microsoft Docs
description: Azure IoT Hub Device apparaatdubbels gebruiken om labels toe te voegen en vervolgens een IoT Hub query te gebruiken. U gebruikt de Azure IoT Device SDK voor Java voor het implementeren van de apparaat-app en de Azure IoT Service SDK voor Java om een service-app te implementeren waarmee de tags worden toegevoegd en de IoT Hub-query wordt uitgevoerd.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 07/04/2017
ms.openlocfilehash: 6d2d0540786f1aa4bec35cf4bec26212cb7df7ae
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68404212"
---
# <a name="get-started-with-device-twins-java"></a>Aan de slag met Device apparaatdubbels (Java)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

In deze zelf studie maakt u twee Java Console-apps:

* **Add-Tags-query**, een Java-back-end-app waarmee Tags en apparaatdubbels worden toegevoegd.
* **gesimuleerd: apparaat**, een Java-apparaat-app die verbinding maakt met uw IOT-hub en de verbindings voorwaarde rapporteert met behulp van een gerapporteerde eigenschap.

> [!NOTE]
> Het artikel [Azure IOT sdk's](iot-hub-devguide-sdks.md) bevat informatie over de Azure IOT-sdk's die u kunt gebruiken om zowel apparaat-als back-end-apps te bouwen.

Voor deze zelfstudie hebt u het volgende nodig:

* De meest recente [Java SE Development Kit 8](https://aka.ms/azure-jdks)

* [Maven 3](https://maven.apache.org/install.html)

* Een actief Azure-account. (Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>De IoT hub-connection string ophalen

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>De service-app maken

In deze sectie maakt u een Java-app waarmee de meta gegevens van een locatie worden toegevoegd als een tag aan het apparaat dubbele in IoT Hub gekoppeld aan **myDeviceId**. De app vraagt eerst IoT hub op voor apparaten die zich in de VS bevinden en vervolgens naar apparaten die een mobiele netwerk verbinding rapporteren.

1. Maak op de ontwikkel computer een lege map met de `iot-java-twin-getstarted`naam.

2. Maak in `iot-java-twin-getstarted` de map een Maven-project met de naam **Add-Tags-query** met behulp van de volgende opdracht achter de opdracht prompt. Let op: dit is één enkele, lange opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

3. Ga bij de opdracht prompt naar de `add-tags-query` map.

4. Open het `pom.xml` bestand in de `add-tags-query` map met een tekst editor en voeg de volgende afhankelijkheden toe aan het knoop punt **afhankelijkheden** . Met deze afhankelijkheid kunt u het **IOT-service-client-** pakket in uw app gebruiken om te communiceren met uw IOT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > U kunt de meest recente versie van **IOT-service-client** controleren met behulp van [maven Search](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

5. Voeg het volgende **Build** -knoop punt  toe na het knoop punt afhankelijkheden. Deze configuratie zorgt ervoor dat maven Java 1,8 wordt gebruikt om de app te bouwen:

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

6. Sla het bestand op `pom.xml` en sluit het.

7. Open het `add-tags-query\src\main\java\com\mycompany\app\App.java` bestand met behulp van een tekst editor.

8. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

9. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{youriothubconnectionstring}` door de IOT hub-Connection String die u eerder hebt gekopieerd in [de IOT hub-Connection String ophalen](#get-the-iot-hub-connection-string):

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

10. Werk de hand tekening van de **hoofd** methode bij `throws` zodat deze de volgende component bevat:

    ```java
    public static void main( String[] args ) throws IOException
    ```

11. Voeg de volgende code toe aan de methode **Main** om de **DeviceTwin** -en **DeviceTwinDevice** -objecten te maken. Het **DeviceTwin** -object verwerkt de communicatie met uw IOT-hub. Het **DeviceTwinDevice** -object vertegenwoordigt het apparaat dubbele met de eigenschappen en Tags:

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

12. Voeg het volgende `try/catch` blok toe aan de methode **Main** :

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

13. Voeg de volgende code toe aan het blok om de dubbele labels van de **regio** en het **centrale** apparaat in `try` het apparaat te wijzigen:

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

14. Als u een query wilt uitvoeren voor het apparaat apparaatdubbels in IOT hub, voegt `try` u de volgende code toe aan de blok kering na de code die u in de vorige stap hebt toegevoegd. De code voert twee query's uit. Elke query retourneert een maximum van 100 apparaten:

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

15. Sla het bestand op `add-tags-query\src\main\java\com\mycompany\app\App.java` en sluit het

16. Bouw de app **Add-Tags-query** op en corrigeer eventuele fouten. Ga bij de opdracht prompt naar de `add-tags-query` map en voer de volgende opdracht uit:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Een apparaat-app maken

In deze sectie maakt u een Java-Console-app waarmee een gerapporteerde eigenschaps waarde wordt ingesteld die wordt verzonden naar IoT Hub.

1. Maak in `iot-java-twin-getstarted` de map een Maven-project met de naam gesimuleerd **apparaat** met behulp van de volgende opdracht bij de opdracht prompt. Let op: dit is één enkele, lange opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Ga bij de opdracht prompt naar de `simulated-device` map.

3. Open met een tekst editor het `pom.xml` bestand in de `simulated-device` map en voeg de volgende afhankelijkheden toe aan het knoop punt **afhankelijkheden** . Met deze afhankelijkheid kunt u het **IOT-apparaat-client-** pakket in uw app gebruiken om te communiceren met uw IOT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.14.2</version>
    </dependency>
    ```

    > [!NOTE]
    > U kunt de meest recente versie van **IOT-Device-client** controleren met behulp van [maven Search](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

4. Voeg het volgende **Build** -knoop punt  toe na het knoop punt afhankelijkheden. Deze configuratie zorgt ervoor dat maven Java 1,8 wordt gebruikt om de app te bouwen:

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

5. Sla het bestand op `pom.xml` en sluit het.

6. Open het `simulated-device\src\main\java\com\mycompany\app\App.java` bestand met behulp van een tekst editor.

7. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

8. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{youriothubname}` door de naam van uw IOT- `{yourdevicekey}` hub en met de waarde voor de apparaatwaarde die u hebt gegenereerd in het gedeelte *een apparaat-id maken* :

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    Deze voorbeeld-app gebruikt de **protocol**-variabele bij het maken van een **DeviceClient**-object. 

1. Voeg de volgende methode toe aan de **app** -klasse om informatie over dubbele updates af te drukken:

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
        @Override
        public void execute(IotHubStatusCode status, Object context) {
          System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
      }
    ```

9. Voeg de volgende code toe aan de methode **Main** om:
    * Maak een apparaatclient om te communiceren met IoT Hub.
    * Maak een **apparaatobject** om de dubbele eigenschappen van het apparaat op te slaan.

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

10. Voeg de volgende code toe aan de methode **Main** om een **connectivityType** -gerapporteerde eigenschap te maken en deze te verzenden naar IOT hub:

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
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

11. Voeg de volgende code toe aan het einde van de methode **Main** . Als u op de **Enter** -toets wacht, kan de IOT hub de status van de dubbele bewerkingen van het apparaat rapporteren:

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. Wijzig de handtekening van de **main**-methode om de uitzonderingen als volgt op te nemen:

     ```java
     public static void main(String[] args) throws URISyntaxException, IOException
     ```

1. Sla het bestand op `simulated-device\src\main\java\com\mycompany\app\App.java` en sluit het.

13. Bouw de **gesimuleerde apparaat-** app op en corrigeer eventuele fouten. Ga bij de opdracht prompt naar de `simulated-device` map en voer de volgende opdracht uit:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-apps"></a>De apps uitvoeren

U bent nu klaar om de console-apps uit te voeren.

1. Voer bij een opdracht prompt in `add-tags-query` de map de volgende opdracht uit om de **Add-Tags-query service-** app uit te voeren:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Service-app van Java IoT Hub om label waarden bij te werken en query's uit te voeren](./media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    U kunt de **fabrieks** -en **regio** Tags zien die zijn toegevoegd aan het apparaat dubbele. De eerste query retourneert uw apparaat, maar de tweede niet.

2. Voer bij een opdracht prompt in `simulated-device` de map de volgende opdracht uit om de gerapporteerde eigenschap **connectivityType** toe te voegen aan het apparaat dubbele:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![De apparaatclient voegt de gerapporteerde eigenschap * * connectivityType * * toe](./media/iot-hub-java-java-twin-getstarted/device-app-1.png)

3. Voer bij een opdracht prompt in `add-tags-query` de map de volgende opdracht uit om de app **Add-Tags-query** service een tweede keer uit te voeren:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Service-app van Java IoT Hub om label waarden bij te werken en query's uit te voeren](./media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Uw apparaat heeft nu de eigenschap **connectivityType** naar IOT hub verzonden, met de tweede query wordt uw apparaat geretourneerd.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt meta gegevens van apparaten toegevoegd als tags van een back-end-app en u hebt een apparaat-app geschreven om connectiviteits gegevens van apparaten te rapporteren in het dubbele apparaat. U hebt ook geleerd hoe u een query kunt uitvoeren op het apparaat dubbele informatie met behulp van de SQL-achtige IoT Hub query taal.

Gebruik de volgende bronnen voor meer informatie over:

* Verzend telemetrie van apparaten met de zelf studie [aan de slag met IOT hub](quickstart-send-telemetry-java.md) .

* Apparaten interactief beheren (bijvoorbeeld door een ventilator in te scha kelen vanuit een door de gebruiker beheerde app) met de zelf studie [directe methoden gebruiken](quickstart-control-device-java.md) .
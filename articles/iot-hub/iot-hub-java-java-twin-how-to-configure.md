---
title: Gebruik Azure IoT Hub apparaat twin eigenschappen (Java) | Microsoft Docs
description: Het gebruik van Azure IoT Hub apparaat horende apparaten configureren. U de Azure-IoT-apparaat-SDK voor Java gebruiken voor het implementeren van een gesimuleerde apparaattoepassing en de Azure IoT service SDK voor Java voor het implementeren van een service-app die de apparaatconfiguratie van een met behulp van een apparaat-twin wijzigt.
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
ms.date: 09/12/2017
ms.author: dobett
ms.openlocfilehash: 72b57a2495d1a03a57923fb171ee17c0f870ddc7
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="use-desired-properties-to-configure-devices"></a>Gewenste eigenschappen gebruiken om apparaten te configureren

[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Aan het einde van deze zelfstudie hebt u twee Java-apps die console:

* **simulated-device**, een gesimuleerde apparaattoepassing die wordt gewacht op een gewenste configuratie-update en meldt de status van een gesimuleerde configuratie updateproces.
* **set-configuratie**, een back-end-app, die de gewenste configuratie ingesteld op een apparaat en het configuratieproces van de update-query's.

> [!NOTE]
> Het artikel [Azure IoT SDK's] [ lnk-hub-sdks] bevat informatie over de Azure IoT SDK's dat u gebruiken kunt om zowel apparaatgegevens als back-end-apps te bouwen.
> 
> 

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

Als u hebt gevolgd de [aan de slag met apparaat horende] [ lnk-twin-tutorial] zelfstudie, u hebt al een IoT-hub en een apparaat-id genoemd **myDeviceId**. In dat geval kunt u doorgaan met de [de gesimuleerde apparaattoepassing maken] [ lnk-how-to-configure-createapp] sectie.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>De gesimuleerde apparaattoepassing maken
In deze sectie maakt u een Java-consoletoepassing die is verbonden met uw hub als **myDeviceId**, wordt gewacht op een gewenste configuratie-update en vervolgens verslag uitbrengt updates op het updateproces gesimuleerde configuratie.

1. Maak een lege map dt-get-started.

1. Maak een Maven-project aangeroepen in de map dt-get-started **simulated-device** met de volgende opdracht achter de opdrachtprompt. Let op: dit is één enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigeer vanuit het opdrachtvenster naar de map simulated-device.

1. Met een teksteditor, open het bestand pom.xml in de map simulated-device en voeg de volgende afhankelijkheid voor de **afhankelijkheden** knooppunt. Deze afhankelijkheid kunt u het pakket iot-service-client gebruiken in uw app om te communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.30</version>
    </dependency>
    ```

    > [!NOTE]
    > U kunt controleren voor de meest recente versie van **iot-apparaat-client** [Maven zoekactie] [lnk-maven-apparaat-search].

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
    import java.util.Scanner;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang **{yourdeviceconnectionstring}** met de apparaat-verbindingsreeks die u hebt genoteerd in de *maken van een apparaat-id* sectie:

    ```java
    private static String deviceId = "myDeviceId";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    private static TelemetryConfig telemetryConfig;
    ```

1. Voor het implementeren van een retouraanroep-handler voor apparaat twin statusgebeurtenissen (ten behoeve van foutopsporing), voeg de volgende geneste klasse om te de **App** klasse:

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback 
    {
        public void execute(IotHubStatusCode status, Object context) 
        {
            //System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

1. Voeg de volgende geneste klasse naar de **App** klasse:

    ```java
    private static class TelemetryConfig extends Device 
    {
        private String configId = "0000";
        private String sendFrequency = "45m";

        private Boolean initialRun = true;
        
        private Property telemetryConfig = new Property("telemetryConfig", "{configId=" + configId + ", sendFrequency=" + sendFrequency + "}");

        public void InitTelemetry() throws IOException 
        {
            System.out.println("Report initial telemetry config:");
            System.out.println(this.telemetryConfig);

            this.setReportedProp(this.telemetryConfig);

            client.sendReportedProperties(this.getReportedProp());
        }

        private void UpdateReportedConfiguration() 
        {
            try {
                System.out.println("Initiating config change");
                    
                this.setReportedProp(this.telemetryConfig);
                client.sendReportedProperties(this.getReportedProp());
                    
                System.out.println("Simulating device reset");
                
                Thread.sleep(10000);
                
                System.out.println("Completing config change");
                System.out.println("Config change complete");
            } catch (Exception e) {
                System.out.println("Exception \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
            }
        }

        @Override
        public void PropertyCall(String propertyKey, Object propertyValue, Object context) 
        {
            if (propertyKey.equals("telemetryConfig")) {
                if (!(initialRun)) {
                    System.out.println("Desired property change:");
                    System.out.println(propertyKey + ":" + propertyValue);

                    telemetryConfig.setValue(propertyValue);

                    UpdateReportedConfiguration();
                } else {
                    initialRun = false;
                }
            } 
        }
    }
    ```

    > [!NOTE]
    > De klasse TelemetryConfig breidt de [apparaatklasse] [ lnk-java-device-class] toegang te krijgen tot de gewenste eigenschappen callbacks.

1. Wijzig de handtekening van de **belangrijkste** methode om de volgende uitzonderingen:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. Voeg de volgende code naar de **belangrijkste** methode instantiëren van een **DeviceClient** en **TelemetryConfig**:

    ```java
    client = new DeviceClient(connString, protocol);

    telemetryConfig = new TelemetryConfig();
    ```

1. Voeg de volgende code naar de **belangrijkste** methode apparaat twin services starten:

    ```java
    try 
    {
        System.out.println("Connecting to hub");
        client.open();
        client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, telemetryConfig, null);

        telemetryConfig.InitTelemetry();

        System.out.println("Wait for desired telemetry...");
        client.subscribeToDesiredProperties(telemetryConfig.getDesiredProp());
    }
    catch (Exception e) 
    {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
        telemetryConfig.clean();
        client.close();
        System.out.println("Shutting down...");
    }
    ```

1. Voeg de volgende code naar de **belangrijkste** methode afsluiten van de apparaatsimulator wanneer dit nodig:

    ```java
    System.out.println("Press enter to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    telemetryConfig.clean();
    client.close();
    ```

1. Sla en sluit het bestand simulated-device\src\main\java\com\mycompany\app\App.java.

1. Bouwen de **simulated-device** back-endserver voor Apps en eventuele fouten te corrigeren. Navigeer naar de map simulated-device en voer de volgende opdracht achter de opdrachtprompt:

    `mvn clean package -DskipTests`

   > [!NOTE]
   > Deze zelfstudie wordt een gedrag voor updates voor gelijktijdige configuratie niet simuleren. Bepaalde configuratie-update-processen mogelijk wijzigingen van de doelconfiguratie voor terwijl de update wordt uitgevoerd, enkele mogelijk moet u ze in de wachtrij en sommige met een fout opgetreden weigeren kan. Zorg ervoor dat rekening houden met het gewenste gedrag voor uw specifieke configuratie-proces en de juiste logica toevoegen voordat u begint de wijziging in de configuratie.
   > 
   > 

## <a name="create-the-service-app"></a>De service-app maken
In deze sectie maakt u een Java-consoletoepassing die updates maken het *gewenst eigenschappen* op het apparaat twin gekoppeld **myDeviceId** met een nieuwe telemetrie configuration-object. Vervolgens zoekt de horende apparaat is opgeslagen in de IoT-hub en ziet u het verschil tussen de gewenste en gerapporteerde configuraties van het apparaat.

1. Maak een Maven-project aangeroepen in de map dt-get-started **set configuratie** met de volgende opdracht achter de opdrachtprompt. Let op: dit is één enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=set-configuration -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigeer naar de configuratiemap op de set bij de opdrachtprompt.

1. Met een teksteditor, open het bestand pom.xml in de map trigger-opnieuw opstarten en voeg de volgende afhankelijkheid voor de **afhankelijkheden** knooppunt. Deze afhankelijkheid kunt u het pakket iot-service-client gebruiken in uw app om te communiceren met uw IoT-hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.5.22</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > U kunt controleren voor de meest recente versie van **iot-service-client** [Maven zoekactie] [lnk-maven-service-search].

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

1. Met een teksteditor, open het bronbestand set-configuration\src\main\java\com\mycompany\app\App.java.

1. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang **{youriothubconnectionstring}** met uw IoT hub-verbindingsreeks die u hebt genoteerd in de *een IoT Hub maken* sectie:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";
    ```

1. Voeg de volgende code aan om te vragen en horende apparaten op het gesimuleerde apparaat bijwerken, de **belangrijkste** methode:

    ```java
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);

    String sendFrequency= "12h";

    try {
        twinClient.getTwin(device);

        // make sure a differing value exists before calling the updateTwin() method, else a null device exception will be thrown
        // this allows the program to be run multiple times for demonstration purposes
        String desiredProperties = device.desiredPropertiesToString();
        if (desiredProperties.contains("sendFrequency=" + sendFrequency))
        {
            sendFrequency = "8h";
        }
            
        Set<Pair> tags = new HashSet<Pair>();
        tags.add(new Pair("telemetryConfig", "{configId=0001, sendFrequency=" + sendFrequency + "}"));

        twinClient.getTwin(device);
        device.setDesiredProperties(tags);

        System.out.println("Config report for: " + deviceId);   
        System.out.println(device);

        twinClient.updateTwin(device);

        String reportedProperties = device.reportedPropertiesToString();
        Boolean waitFlag = true;

        while (waitFlag) {
            if (!reportedProperties.contains("sendFrequency=" + sendFrequency)) {
                Thread.sleep(10000);
            }
            else 
            {
                waitFlag = false;
            }

            twinClient.getTwin(device);
            reportedProperties = device.reportedPropertiesToString();
        }
            
        SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "properties.reported.telemetryConfig='{configId=0001, sendFrequency=" + sendFrequency + "}'", null);

        Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
        while (twinClient.hasNextDeviceTwin(twinQuery)) {
            DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
            System.out.println(d.getDeviceId() + " found with changed telemetryConfig");
        }

        System.out.println("Config report for: " + deviceId);
        twinClient.getTwin(device);
        System.out.println(device);

        } catch (IotHubException e) {
            System.out.println(e.getMessage());
        } catch (IOException e) {
            System.out.println(e.getMessage());
        } catch (InterruptedException e) {
            System.out.println(e.getMessage());
        }
    ```

1. Sla en sluit het bestand set-desired-configuration\src\main\java\com\mycompany\app\App.java.

1. Bouwen de **set configuratie** back-endserver voor Apps en eventuele fouten te corrigeren. Navigeer naar de map set-configuratie en voer de volgende opdracht achter de opdrachtprompt:

    `mvn clean package -DskipTests`
   
    Deze code haalt de apparaat-twin voor **myDeviceId**, en werkt vervolgens de gewenste eigenschappen met een nieuwe telemetrie configuration-object.
    Daarna wordt deze query's van het apparaat horende opgeslagen in de IoT-hub elke 10 seconden en de gewenste en gerapporteerde telemetrie-configuraties worden afgedrukt. Raadpleeg de [IoT Hub-querytaal] [ lnk-query] voor informatie over het uitgebreide om rapporten te genereren in al uw apparaten.
   
   > [!IMPORTANT]
   > IoT Hub van deze toepassing vraagt om de tien seconden ter illustratie totdat het apparaat is bijgewerkt. Met query's gebruikersgerichte om rapporten te genereren over verschillende apparaten en niet voor het detecteren van wijzigingen. Als uw oplossing realtime meldingen over apparaatgebeurtenissen vereist, gebruikt u [twin meldingen][lnk-twin-notifications].
   > 

   > [!IMPORTANT]
   > Er is een vertraging van maximaal een minuut tussen het apparaat rapport opnieuw en het queryresultaat. Dit is het inschakelen van de query-infrastructuur om te werken op zeer grote schaal.  Voor het ophalen van consistente weergaven van een enkel apparaat twin gebruiken de **getDeviceTwin** methode in de **DeviceTwin** klasse.
   > 
   > 

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. Voer bij een opdrachtprompt in de map simulated-device de volgende opdracht om te beginnen met luisteren naar apparaat twin aanroepen vanuit uw IoT-hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

1. Voer de volgende opdracht kunnen opvragen en bijwerken van de horende apparaten op het gesimuleerde apparaat uit uw IoT-hub bij een opdrachtprompt in de map set-configuratie:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

1. Het gesimuleerde apparaat reageert op de aanroep van de directe methode opnieuw opstarten:

    ![De gesimuleerde apparaattoepassing IoT-Hub voor Java reageert op het apparaat twin aanroepen][img-deviceconfigured]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie stelt u een gewenste configuratie als *gewenst eigenschappen* back-end van de oplossing en een app voor het apparaat om te detecteren die wijziging en een updateproces van meerdere stappen de status ervan via de gerapporteerde eigenschappen reporting simuleren geschreven.

Gebruik de volgende bronnen voor meer informatie over hoe:

* verzenden van telemetrie vanaf apparaten met de [aan de slag met IoT Hub] [ lnk-iothub-getstarted] zelfstudie
* schema of bewerkingen uitvoeren op grote sets van apparaten, Zie de [planning en broadcast taken] [ lnk-schedule-jobs] zelfstudie.
* beheren van apparaten interactief (zoals het inschakelen van een ventilator van een gebruiker beheerde app), met de [direct methoden gebruiken] [ lnk-methods-tutorial] zelfstudie.

<!-- images -->
[img-deviceconfigured]: media/iot-hub-java-java-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-java-device-class]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._device_twin._device

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app

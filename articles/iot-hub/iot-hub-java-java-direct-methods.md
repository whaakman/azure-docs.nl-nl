---
title: Gebruik Azure IoT Hub direct methoden (Java) | Microsoft Docs
description: Het gebruik van Azure IoT Hub rechtstreekse methoden. U kunt het Azure-IoT-apparaat-SDK voor Java gebruiken voor het implementeren van een gesimuleerde apparaattoepassing met een directe methode en de service Azure IoT SDK voor Java voor het implementeren van een service-app die de directe methode aanroept.
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 4fb759ecd7767c126bc22165494652039ba1caa4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-java"></a>Directe methoden (Java) gebruiken

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

In deze zelfstudie maakt maken u twee console Java-apps:

* **Invoke-direct-method**, een Java-back-end-app waarmee een methode wordt aangeroepen in de gesimuleerde apparaattoepassing en het antwoord weergegeven.
* **simulated-device**, een Java-app die een apparaat verbinding te maken met uw IoT-hub aan de apparaat-id die u maakt simuleert. Deze app reageert op het direct aangeroepen vanuit de back-end.

> [!NOTE]
> Zie voor meer informatie over de SDK's die u gebruiken kunt om toepassingen uit te voeren op apparaten en de back-end van uw oplossing te bouwen [Azure IoT SDK's][lnk-hub-sdks].

Voor deze zelfstudie hebt u het volgende nodig:

* Java SE 8. <br/> In het gedeelte [Uw ontwikkelingsomgeving voorbereiden][lnk-dev-setup] staat beschreven hoe u Java voor deze handleiding kunt installeren op Windows of Linux.
* Maven 3.  <br/> In het gedeelte [Uw ontwikkelingsomgeving voorbereiden][lnk-dev-setup] staat beschreven hoe u [Maven][lnk-maven] voor deze handleiding kunt installeren op Windows of Linux.
* [Node.js-versie 0.10.0 of hoger](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken

In deze sectie maakt u een Java-consoletoepassing dat met een methode aangeroepen door de oplossing voor back-end overeenkomt.

1. Maak een lege map genaamd iot-java-direct-methode.

1. In de map iot-java-direct-methode maakt u een Maven-project aangeroepen **simulated-device** met de volgende opdracht achter de opdrachtprompt. De volgende opdracht wordt een enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigeer vanuit het opdrachtvenster naar de map simulated-device.

1. Open het bestand pom.xml in de map simulated-device met een teksteditor en voeg de volgende afhankelijkheden toe aan de node **dependencies**. Deze afhankelijkheid kunt u het pakket iot-apparaat-client gebruiken in uw app om te communiceren met uw IoT-hub:

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

1. Open het bestand simulated-device\src\main\java\com\mycompany\app\App.java met een teksteditor.

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
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Deze voorbeeld-app gebruikt de **protocol**-variabele bij het maken van een **DeviceClient**-object. 

1. Als u wilt een statuscode terugkeren naar uw IoT-hub, voeg de volgende geneste klasse om te de **App** klasse:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. Voor het afhandelen van de directe methode-aanroepen vanuit de back-end oplossing toevoegen de volgende geneste klasse naar de **App** klasse:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "writeLine" :
          {
            int status = METHOD_SUCCESS;
            System.out.println(new String((byte[])methodData));
            deviceMethodData = new DeviceMethodData(status, "Executed direct method " + methodName);
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

1. Maken van een **DeviceClient** en luisteren naar directe methode aanroepen, het toevoegen van een **belangrijkste** methode de **App** klasse:

    ```java
    public static void main(String[] args)
      throws IOException, URISyntaxException
    {
      System.out.println("Starting device sample...");

      DeviceClient client = new DeviceClient(connString, protocol);

      try
      {
        client.open();
        client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
        System.out.println("Subscribed to direct methods. Waiting...");
      }
      catch (Exception e)
      {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
        client.close();
        System.out.println("Shutting down...");
      }

      System.out.println("Press any key to exit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();
      scanner.close();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Opslaan en sluiten van het bestand simulated-device\src\main\java\com\mycompany\app\App.java

1. Bouwen de **simulated-device** app en eventuele fouten te corrigeren. Navigeer naar de map simulated-device en voer de volgende opdracht achter de opdrachtprompt:

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a>Een directe methode is aangeroepen voor een apparaat

In deze sectie maakt u een Java-consoletoepassing die een directe methode wordt aangeroepen en wordt vervolgens het antwoord. Deze app console maakt verbinding met uw IoT-Hub aan te roepen de directe methode.

1. In de map iot-java-direct-methode maakt u een Maven-project aangeroepen **aanroepen directe methode** met de volgende opdracht achter de opdrachtprompt. De volgende opdracht wordt een enkele, lange opdracht:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigeer naar de map invoke-direct-methode bij de opdrachtprompt.

1. Met een teksteditor, open het bestand pom.xml in de map invoke-direct-methode en voeg de volgende afhankelijkheid voor de **afhankelijkheden** knooppunt. Deze afhankelijkheid kunt u het pakket iot-service-client gebruiken in uw app om te communiceren met uw IoT-hub:

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

1. Met een teksteditor, open het bestand invoke-direct-method\src\main\java\com\mycompany\app\App.java.

1. Voeg de volgende **import**instructies toe aan het bestand:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Voeg de volgende variabelen op klasseniveau toe aan de **App**-klasse. Vervang `{youriothubconnectionstring}` met uw IoT hub-verbindingsreeks die u hebt genoteerd in de *een IoT Hub maken* sectie:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line to be written";
    ```

1. Voeg de volgende code aan om aan te roepen bij de methode voor het gesimuleerde apparaat, de **belangrijkste** methode:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
        System.out.println("Invoke direct method");
        MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

        if(result == null)
        {
            throw new IOException("Direct method invoke returns null");
        }
        System.out.println("Status=" + result.getStatus());
        System.out.println("Payload=" + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }

    System.out.println("Shutting down sample...");
    ```

1. Opslaan en sluiten van het bestand invoke-direct-method\src\main\java\com\mycompany\app\App.java

1. Bouwen de **aanroepen directe methode** app en eventuele fouten te corrigeren. Navigeer naar de map invoke-direct-methode en voer de volgende opdracht achter de opdrachtprompt:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>De apps uitvoeren

U bent nu klaar om uit te voeren van de console-apps.

1. Voer bij een opdrachtprompt in de map simulated-device de volgende opdracht om te beginnen met luisteren naar methodeaanroepen van uw IoT-hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![De gesimuleerde apparaattoepassing Java IoT-Hub te luisteren naar directe methodeaanroepen][8]

1. Voer de volgende opdracht om een methode niet aanroepen voor het gesimuleerde apparaat uit uw IoT-hub bij een opdrachtprompt in de map invoke-direct-methode:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![IoT Hub voor Java-service-app een directe methode niet aanroepen][7]

1. Het gesimuleerde apparaat reageert op de aanroep van directe methode:

    ![De gesimuleerde apparaattoepassing IoT-Hub voor Java reageert op de aanroep van directe methode][9]

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt deze apparaat-id gebruikt om in te schakelen van de gesimuleerde apparaattoepassing om te reageren op de methoden die worden aangeroepen door de cloud. Hebt u ook een app roept methoden op het apparaat en de reactie van het apparaat wordt gemaakt.

Als u wilt verkennen andere IoT-scenario's, Zie [plannen van taken op meerdere apparaten][lnk-devguide-jobs].

Zie voor meer informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten aanroepen, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-direct-methods/invoke-method.png
[8]: ./media/iot-hub-java-java-direct-methods/device-listen.png
[9]: ./media/iot-hub-java-java-direct-methods/device-respond.png

<!-- Links -->
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md
[lnk-maven]: https://maven.apache.org/what-is-maven.html
[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22

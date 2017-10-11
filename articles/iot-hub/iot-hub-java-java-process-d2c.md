---
title: Azure IoT Hub apparaat-naar-cloud-berichten (Java) | Microsoft Docs
description: Klik hier voor meer informatie over het verwerken van IoT Hub apparaat-naar-cloud-berichten met behulp van regels voor het doorsturen en aangepaste eindpunten verzending van berichten naar andere back-end-services.
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.openlocfilehash: d1aca8f39e305105d4ec9f63fbe7bee95487e294
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-java"></a>Verwerken van Iothub apparaat-naar-cloud-berichten (Java)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Azure IoT Hub is een volledig beheerde service waarmee betrouwbare en veilige tweerichtingscommunicatie tussen miljoenen apparaten en een oplossing voor back-end. Andere zelfstudies ([aan de slag met IoT Hub] en [cloud naar apparaat verzenden met IoT Hub][lnk-c2d]) hoe u het basic apparaat-naar-cloud- en cloud-naar-apparaat gebruiken Messaging-functionaliteit van IoT Hub.

Deze zelfstudie bouwt voort op de code die wordt weergegeven de [aan de slag met IoT Hub] zelfstudie, en hoe u berichtroutering gebruiken voor het verwerken van apparaat-naar-cloud-berichten in een schaalbare manier. De zelfstudie laat zien hoe berichten waarvoor onmiddellijke actie van de back-end oplossing te verwerken. Een apparaat kan bijvoorbeeld een waarschuwing weergegeven dat activeert een ticket in een CRM-systeem invoegen verzenden. Gegevenspunt berichten feed daarentegen alleen in een analyse-engine. Telemetrie van de temperatuur van een apparaat dat moet worden opgeslagen voor latere analyse is bijvoorbeeld een gegevenspunt bericht.

Aan het einde van deze zelfstudie, moet u drie Java-apps die console uitvoeren:

* **simulated-device**, een aangepaste versie van de app gemaakt in de [aan de slag met IoT Hub] zelfstudie verzendt gegevenspunt apparaat-naar-cloud-berichten per seconde en interactieve apparaat-naar-cloud met berichten voor elke 10 seconden . Deze app gebruikt het AMQP-protocol om te communiceren met IoT Hub.
* **Read-d2c-messages** geeft de telemetrie die is verzonden door de app op uw apparaat weer.
* **alleen kritieke wachtrij** ongedaan de kritieke berichten uit de Service Bus-wachtrij gekoppeld aan de IoT-hub in de wachtrij geplaatst.

> [!NOTE]
> IoT Hub heeft ondersteuning voor veel apparaatplatforms en talen, waaronder C, Java en JavaScript SDK. Zie voor instructies over hoe het apparaat in deze zelfstudie vervangen door een fysiek apparaat en hoe apparaten verbinden met een IoT-Hub, de [Azure IoT Developer Center].

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een volledige werkende versie van de [aan de slag met IoT Hub] zelfstudie.
* De meest recente [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account] [lnk-free-trial] binnen een paar minuten.)

Er is enige basiskennis van [Azure Storage] en [Azure Service Bus].

## <a name="send-interactive-messages-from-a-device-app"></a>Interactieve berichten verzenden vanuit een apparaat-app
In deze sectie die u wijzigt de apparaat-app die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie af en toe om berichten te verzenden waarvoor onmiddellijke verwerking.

1. Gebruik een teksteditor te openen van het bestand simulated-device\src\main\java\com\mycompany\app\App.java. Dit bestand bevat de code voor de **simulated-device** app die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie.

2. Vervang de **MessageSender** klasse met de volgende code:

    ```java
    private static class MessageSender implements Runnable {

        public void run()  {
            try {
                double minTemperature = 20;
                double minHumidity = 60;
                Random rand = new Random();

                while (true) {
                    String msgStr;
                    Message msg;
                    if (new Random().nextDouble() > 0.7) {
                        msgStr = "This is a critical message.";
                        msg = new Message(msgStr);
                        msg.setProperty("level", "critical");
                    } else {
                        double currentTemperature = minTemperature + rand.nextDouble() * 15;
                        double currentHumidity = minHumidity + rand.nextDouble() * 20; 
                        TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
                        telemetryDataPoint.deviceId = deviceId;
                        telemetryDataPoint.temperature = currentTemperature;
                        telemetryDataPoint.humidity = currentHumidity;

                        msgStr = telemetryDataPoint.serialize();
                        msg = new Message(msgStr);
                    }
                    
                    System.out.println("Sending: " + msgStr);

                    Object lockobj = new Object();
                    EventCallback callback = new EventCallback();
                    client.sendEventAsync(msg, callback, lockobj);

                    synchronized (lockobj) {
                        lockobj.wait();
                    }
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                System.out.println("Finished.");
            }
        }
    }
    ```
   
    Deze methode wordt willekeurig toegevoegd voor de eigenschap `"level": "critical"` op berichten die door het apparaat verzonden, die een bericht dat directe actie is vereist door de toepassing van de back-end simuleert. De toepassing geeft deze informatie in de berichteigenschappen in plaats van in de hoofdtekst van het bericht, zodat deze IoT Hub het bericht naar de juiste bestemming sturen kan.
   
   > [!NOTE]
   > U kunt de berichteigenschappen om berichten te routeren voor verschillende scenario's, inclusief de verwerking van koude pad, naast het hot pad voorbeeld dat hier wordt weergegeven.

2. Sla en sluit het bestand simulated-device\src\main\java\com\mycompany\app\App.java.

    > [!NOTE]
    > Omwille van de eenvoud in deze zelfstudie niet geïmplementeerd voor een beleid voor opnieuw proberen. In productiecode moet u een beleid voor opnieuw proberen zoals exponentieel uitstel, zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout].

3. Als u de app **simulated-device** wilt maken met behulp van Maven, geeft u de volgende opdracht op in het opdrachtvenster in de map simulated-device:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-to-your-iot-hub-and-route-messages-to-it"></a>Een wachtrij toevoegen aan uw IoT hub en route-berichten naar het

In deze sectie maakt een Service Bus-wachtrij, verbinden met uw IoT-hub en configureren van uw IoT-hub om berichten te verzenden naar de wachtrij op basis van de aanwezigheid van een eigenschap van het bericht. Zie voor meer informatie over het verwerken van berichten van Service Bus-wachtrijen [aan de slag met wachtrijen][lnk-sb-queues-java].

1. Een Service Bus-wachtrij maakt, zoals beschreven in [aan de slag met wachtrijen][lnk-sb-queues-java]. Noteer de naam van de naamruimte en de wachtrij.

2. Open uw IoT-hub in de Azure-portal en klikt u op **eindpunten**.

    ![Eindpunten van IoT-hub][30]

3. In de **eindpunten** blade, klikt u op **toevoegen** boven uw wachtrij toevoegen aan uw IoT-hub. Naam van het eindpunt **CriticalQueue** en selecteer met de vervolgkeuzelijsten **Service Bus-wachtrij**, de Service Bus-naamruimte waarin de wachtrij zich bevindt en de naam van de wachtrij. Wanneer u klaar bent, klikt u op **opslaan** onderaan.

    ![Een eindpunt toevoegen][31]

4. Klik nu op **Routes** in uw IoT-Hub. Klik op **toevoegen** boven aan de blade voor het maken van een regel voor doorsturen waarmee berichten worden doorgestuurd naar de wachtrij die u zojuist hebt toegevoegd. Selecteer **DeviceTelemetry** als de bron van gegevens. Voer `level="critical"` als de voorwaarde, en kies de wachtrij die u zojuist hebt toegevoegd als een aangepaste eindpunt als de routering eindpunt van de regel. Wanneer u klaar bent, klikt u op **opslaan** onderaan.

    ![Toevoegen van een route][32]

    Zorg ervoor dat de terugval route is ingesteld op **ON**. Deze instelling is de standaardconfiguratie van een IoT-hub.

    ![Route voor terugval][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(Optioneel) Lezen van het eindpunt van de wachtrij

U kunt eventueel de berichten van het eindpunt van de wachtrij lezen door de instructies in [aan de slag met wachtrijen][lnk-sb-queues-java]. Naam van de app **lezen kritieke wachtrij**.

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren van de drie toepassingen.

1. Om uit te voeren de **read-d2c-messages** toepassing in een opdrachtprompt of een shell Navigeer naar de map read-d2c en voer de volgende opdracht:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Read-d2c-messages uitvoeren][readd2c]

2. Om uit te voeren de **lezen kritieke wachtrij** toepassing in een opdrachtprompt of een shell Navigeer naar de map lezen kritieke wachtrij en de volgende opdracht:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Alleen kritieke berichten uitgevoerd][readqueue]

3. Om uit te voeren de **simulated-device** app in een opdrachtprompt of een shell gaat u naar de map simulated-device en de volgende opdracht:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Simulated-device uitvoeren][simulateddevice]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe betrouwbaar apparaat-naar-cloud-berichten verzenden met behulp van de berichtroutering van IoT Hub.

De [het verzenden van berichten van de cloud-naar-apparaat met IoT Hub] [ lnk-c2d] ziet u hoe u berichten verzenden naar uw apparaten vanuit de back-end van uw oplossing.

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub [Azure IoT Suite][lnk-suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

Zie voor meer informatie over het routeren van berichten van IoT-Hub, [berichten verzenden en ontvangen met IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-java-java-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-java-java-process-d2c/route-creation.png
[33]: ./media/iot-hub-java-java-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-java]: ../service-bus-messaging/service-bus-java-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[aan de slag met IoT Hub]: iot-hub-java-java-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[afhandeling van tijdelijke fout]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Afhandeling van tijdelijke fout]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-java-java-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

---
title: Cloud-naar-apparaat-berichten met Azure IoT Hub (iOS) | Microsoft Docs
description: Het cloud-naar-apparaat om berichten te verzenden naar een apparaat van een Azure IoT hub met de Azure IoT SDK's voor iOS.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2018
ms.author: kgremban
ms.openlocfilehash: 23dbd1f359f947b8e87ab4115887120dfd55907a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="send-cloud-to-device-messages-with-iot-hub-ios"></a>Cloud-naar-apparaat-berichten verzenden met IoT Hub (iOS)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]


Azure IoT Hub is een volledig beheerde service waarmee stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing. De [verzenden van telemetrie vanaf een apparaat naar een IoT-hub] artikel laat zien hoe u een iothub maken, een apparaat-id in het inrichten en code van een gesimuleerde apparaattoepassing dat apparaat-naar-cloud-berichten verzendt.

Dit artikel ziet u hoe aan:

* Van uw back-end oplossing, cloud-naar-apparaat-berichten naar één enkel apparaat via IoT Hub te verzenden.
* Cloud-naar-apparaat-berichten op een apparaat ontvangen.
* Aanvragen van uw back-end oplossing, levering bevestiging (*feedback*) voor berichten die worden verzonden naar een apparaat uit IoT Hub.

U vindt meer informatie over cloud-naar-apparaat-berichten in de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].

Aan het einde van dit artikel uitvoeren u twee Swift iOS projecten:

* **voorbeeld apparaat**, gemaakt in dezelfde app [verzenden van telemetrie vanaf een apparaat naar een IoT-hub], die verbinding maakt met uw IoT-hub en cloud-naar-apparaat-berichten worden ontvangen.
* **voorbeeld-service**, die verzendt een bericht cloud-naar-apparaat naar de gesimuleerde apparaattoepassing via IoT Hub en vervolgens ontvangt de bevestiging levering.

> [!NOTE]
> IoT-Hub heeft SDK-ondersteuning voor veel apparaatplatforms en talen (inclusief C, Java en Javascript) via Azure IoT-apparaat-SDK's. Zie voor stapsgewijze instructies voor het koppelen van uw apparaat in deze zelfstudie code en in het algemeen naar Azure IoT Hub de [Azure IoT Developer Center].

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

- Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)
- Een actieve iothub in Azure. 
- De voorbeeldcode van [Azure-voorbeelden](https://github.com/Azure-Samples/azure-iot-samples-ios/archive/master.zip) .
- De nieuwste versie van [XCode](https://developer.apple.com/xcode/), met de nieuwste versie van de iOS-SDK. Deze snelstart is getest met XCode 9.3 en iOS 11.3.
- De nieuwste versie van [CocoaPods](https://guides.cocoapods.org/using/getting-started.html).


## <a name="simulate-an-iot-device"></a>Een IoT-apparaat te simuleren
In deze sectie maakt simuleren u een iOS-apparaat met een Swift toepassing cloud-naar-apparaat om berichten te ontvangen van de IoT-hub. 

Dit is het voorbeeld voorbeeld apparaat die u in het artikel maakt [verzenden van telemetrie vanaf een apparaat naar een IoT-hub]. Als u al hebt dat wordt uitgevoerd, kunt u deze sectie overslaan.

### <a name="install-cocoapods"></a>CocoaPods installeren

CocoaPods beheren afhankelijkheden voor iOS-projecten die gebruikmaken van bibliotheken van derden.

Navigeer in een terminalvenster naar de Azure-IoT-Samples-iOS-map die u hebt gedownload bij de vereisten. Navigeer vervolgens naar het voorbeeldproject:

```sh
cd quickstart/sample-device
```

Zorg ervoor dat XCode is gesloten en voer vervolgens de volgende opdracht uit om de CocoaPods te installeren die worden gedeclareerd in het **podfile**-bestand:

```sh
pod install
```

Naast het installeren van de pods die nodig zijn voor uw project, heeft de installatieopdracht ook een XCode-werkruimtebestand gemaakt dat al geconfigureerd is om de pods te gebruiken voor afhankelijkheden. 

### <a name="run-the-sample-device-application"></a>De voorbeeldtoepassing apparaat uitvoeren 

1. De verbindingsreeks voor uw apparaat worden opgehaald. U kunt deze tekenreeks van de Azure-portal op de blade van de details apparaat kopiëren of deze ophalen met de volgende CLI-opdracht: 

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id {YourDeviceID} --output table
    ```

1. Open de voorbeeldwerkruimte in XCode.

   ```sh
   open "MQTT Client Sample.xcworkspace"
   ```

2. Vouw de **MQTT clientvoorbeeld** project en vervolgens de map met dezelfde naam.  
3. Open **ViewController.swift** om het te bewerken in XCode. 
4. Zoeken naar de **connectionString** variabele en werk de waarde met de verbinding van de tekenreeks die u in de eerste stap hebt gekopieerd.
5. Sla uw wijzigingen op. 
6. Voer het project in de apparaatemulator uit met de knop **Build and run** of de toetscombinatie **command+r**. 

   ![Het project uitvoeren](media/quickstart-send-telemetry-ios/run-sample.png)


## <a name="simulate-a-service-device"></a>Een serviceapparaat te simuleren

In deze sectie maakt simuleren u een tweede iOS-apparaat met een Swift app dat cloud-naar-apparaat-berichten via iothub verzendt. Dit is handig voor IoT-scenario's waarbij er een iPhone of iPad functioneert als een domeincontroller voor andere iOS-apparaten moet worden verbonden met een IoT-hub. 

### <a name="install-cocoapods"></a>CocoaPods installeren

CocoaPods beheren afhankelijkheden voor iOS-projecten die gebruikmaken van bibliotheken van derden.

Navigeer naar de Azure IoT iOS voorbeelden map die u hebt gedownload in de vereisten. Navigeer naar het voorbeeldproject service:

```sh
cd quickstart/sample-service
```

Zorg ervoor dat XCode is gesloten en voer vervolgens de volgende opdracht uit om de CocoaPods te installeren die worden gedeclareerd in het **podfile**-bestand:

```sh
pod install
```

Naast het installeren van de pods die nodig zijn voor uw project, heeft de installatieopdracht ook een XCode-werkruimtebestand gemaakt dat al geconfigureerd is om de pods te gebruiken voor afhankelijkheden.

### <a name="run-the-sample-service-application"></a>De voorbeeldtoepassing voor de service uitgevoerd

1. Ophalen van de verbindingsreeks voor de service voor uw IoT-hub. U kunt deze tekenreeks kopiëren vanuit de Azure-portal van de **iothubowner** beleid in de **gedeeld toegangsbeleid** blade of deze ophalen met de volgende CLI-opdracht:  

    ```azurecli-interactive
    az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
    ```

2. Open de voorbeeldwerkruimte in XCode.

   ```sh
   open AzureIoTServiceSample.xcworkspace
   ```

3. Vouw de **AzureIoTServiceSample** project en vouw vervolgens de map met dezelfde naam.  
4. Open **ViewController.swift** om het te bewerken in XCode. 
5. Zoeken naar de **connectionString** variabele en werk de waarde met de verbindingsreeks van de service die u eerder hebt gekopieerd.
6. Sla uw wijzigingen op. 
7. In Xcode, wijzigt u de emulator-instellingen op een andere iOS-apparaat dan u hebt gebruikt voor het uitvoeren van de IoT-apparaat. Meerdere emulators van hetzelfde type kan niet worden uitgevoerd in XCode. 

   ![Wijzigen van de emulator](media/iot-hub-ios-swift-c2d/change-device.png)

8. Voer het project in de apparaatemulator met de **bouwen en uitvoeren van** knop of de sleutel keuzelijst met invoervak **opdracht + r**. 

   ![Het project uitvoeren](media/iot-hub-ios-swift-c2d/run-app.png)


## <a name="send-a-cloud-to-device-message"></a>Een cloud naar apparaat verzenden
U bent nu klaar voor gebruik van de twee toepassingen naar de cloud-naar-apparaat-berichten verzenden en ontvangen.

1. In de **iOS-App voorbeeld** app uitgevoerd op het gesimuleerde IoT-apparaat, klikt u op **Start**. De toepassing wordt gestart apparaat-naar-cloud-berichten verzenden, maar ook begint met luisteren voor cloud-naar-apparaat-berichten. 

   ![Voorbeeld van IoT-apparaat app weergeven](media/iot-hub-ios-swift-c2d/view-d2c.png)

2. In de **IoTHub-Service clientvoorbeeld** app uitgevoerd op het gesimuleerde apparaat, geef de ID voor de IoT-apparaat dat u wilt een een bericht te verzenden. 
3. Schrijf een bericht als leesbare tekst en klik vervolgens op **verzenden**. 

Verschillende acties gebeuren als u op verzenden klikt. Het voorbeeld service verzendt het bericht naar uw IoT hub, die de app heeft toegang tot vanwege de verbinding met de tekenreeks die u hebt opgegeven. Uw IoT-hub controleert de apparaat-ID, zendt het bericht naar het doelapparaat en een bevestiging van ontvangst verzendt naar de Bronapparaat. De app actief is op het gesimuleerde IoT-apparaat controleert op berichten uit IoT Hub en de tekst van de meest recente versie van het scherm wordt afgedrukt.

Uw uitvoer moet eruitzien als in het volgende voorbeeld:

   ![Cloud-naar-apparaat-berichten weergeven](media/iot-hub-ios-swift-c2d/view-c2d.png)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe cloud-naar-apparaat-berichten verzenden en ontvangen. 

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub [oplossingsverbetering Azure IoT externe controle].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

<!-- Images -->
[img-simulated-device]: media/iot-hub-python-python-c2d/simulated-device.png
[img-send-command]:  media/iot-hub-python-python-c2d/send-command.png
[img-message-recieved]: media/iot-hub-python-python-c2d/message-recieved.png

<!-- Links -->
[verzenden van telemetrie vanaf een apparaat naar een IoT-hub]: quickstart-send-telemetry-ios.md

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[oplossingsverbetering Azure IoT externe controle]: https://azure.microsoft.com/documentation/suites/iot-suite/

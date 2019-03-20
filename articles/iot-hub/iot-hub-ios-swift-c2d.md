---
title: Cloud-naar-apparaat-berichten met Azure IoT Hub (iOS) | Microsoft Docs
description: Klik hier voor meer informatie over het cloud-naar-apparaat-berichten verzenden naar een apparaat van een Azure IoT-hub met behulp van de Azure IoT SDK's voor iOS.
author: kgremban
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/19/2018
ms.author: kgremban
ms.openlocfilehash: e7e8d12af92a566753d8f3d7baf5019bae44de2c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58163514"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-ios"></a>Cloud-naar-apparaat-berichten verzenden met IoT Hub (iOS)

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub is een volledig beheerde service die stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing. De [verzenden van telemetrie vanaf een apparaat naar een IoT-hub](quickstart-send-telemetry-ios.md) artikel wordt beschreven hoe u een IoT-hub maken, een apparaat-id in het inrichten en code van een gesimuleerde apparaat-app dat apparaat-naar-cloud-berichten verzendt.

In dit artikel ziet u hoe u naar:

* Vanuit de back-end, cloud-naar-apparaat-berichten naar een enkel apparaat via IoT Hub te verzenden.

* Cloud-naar-apparaat-berichten op een apparaat ontvangen.

* Aanvragen van de back-end, levering bevestiging (*feedback*) voor berichten die worden verzonden naar een apparaat vanuit IoT Hub.

U vindt meer informatie over cloud-naar-apparaat-berichten in de [sectie van de Ontwikkelaarshandleiding voor IoT Hub-berichten](iot-hub-devguide-messaging.md).

Aan het einde van dit artikel u twee Swift iOS projecten uitvoeren:

* **voorbeeld-apparaat**, gemaakt in dezelfde app [verzenden van telemetrie vanaf een apparaat naar een IoT-hub](quickstart-send-telemetry-ios.md), die verbinding maakt met uw IoT-hub en cloud-naar-apparaat-berichten worden ontvangen.

* **voorbeeld-service**, die een cloud-naar-apparaat-bericht naar de gesimuleerde apparaattoepassing via IoT Hub verzendt en ontvangt u vervolgens de bevestiging levering.

> [!NOTE]
> IoT-Hub heeft SDK-ondersteuning voor vele platformen voor apparaten en talen (waaronder C, Java en Javascript) via Azure IoT device-SDK's. Zie voor stapsgewijze instructies voor het verbinding maken tussen uw apparaat in de code van deze zelfstudie, en in het algemeen voor Azure IoT Hub, de [Azure IoT-ontwikkelaarscentrum](https://www.azure.com/develop/iot).

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

- Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

- Een actieve IoT-hub in Azure. 

- Het codevoorbeeld van [Azure-voorbeelden](https://github.com/Azure-Samples/azure-iot-samples-ios/archive/master.zip) .

- De nieuwste versie van [XCode](https://developer.apple.com/xcode/), met de nieuwste versie van de iOS-SDK. Deze snelstart is getest met XCode 9.3 en iOS 11.3.

- De nieuwste versie van [CocoaPods](https://guides.cocoapods.org/using/getting-started.html).

## <a name="simulate-an-iot-device"></a>Een IoT-apparaat simuleren

In deze sectie maakt simuleren u een iOS-apparaat met een Swift toepassing cloud-naar-apparaat-berichten ontvangen van de IoT-hub. 

Dit is de voorbeeld-apparaat die u in het artikel maakt [verzenden van telemetrie vanaf een apparaat naar een IoT-hub](quickstart-send-telemetry-ios.md). Als u al dat deze worden uitgevoerd, kunt u deze sectie overslaan.

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

### <a name="run-the-sample-device-application"></a>De voorbeeldtoepassing voor apparaat uitvoeren 

1. Haal de verbindingsreeks voor uw apparaat. Kunt u deze tekenreeks van de [Azure-portal](https://portal.azure.com) in de blade Apparaatdetails, haalt u deze met de volgende CLI-opdracht: 

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id {YourDeviceID} --output table
    ```

1. Open de voorbeeldwerkruimte in XCode.

   ```sh
   open "MQTT Client Sample.xcworkspace"
   ```

2. Vouw de **MQTT Client Sample** project en klikt u vervolgens de map met dezelfde naam.  

3. Open **ViewController.swift** om het te bewerken in XCode. 

4. Zoek de **connectionString** variabele en werk de waarde bij met de apparaatverbinding de tekenreeks die u in de eerste stap hebt gekopieerd.

5. Sla uw wijzigingen op. 

6. Voer het project in de apparaatemulator uit met de knop **Build and run** of de toetscombinatie **command+r**. 

   ![Het project uitvoeren](media/iot-hub-ios-swift-c2d/run-sample.png)

## <a name="simulate-a-service-device"></a>Een serviceapparaat simuleren

In deze sectie maakt simuleren u een tweede iOS-apparaat met een Swift-app dat cloud-naar-apparaat-berichten via de IoT hub verzendt. Dit is handig voor IoT-scenario's waarbij er een iPhone of iPad functioneert als een domeincontroller voor andere iOS-apparaten die zijn verbonden met een IoT-hub. 

### <a name="install-cocoapods"></a>CocoaPods installeren

CocoaPods beheren afhankelijkheden voor iOS-projecten die gebruikmaken van bibliotheken van derden.

Navigeer naar de map met Azure IoT iOS voorbeelden die u in de vereisten hebt gedownload. Navigeer vervolgens naar het voorbeeldproject van de service:

```sh
cd quickstart/sample-service
```

Zorg ervoor dat XCode is gesloten en voer vervolgens de volgende opdracht uit om de CocoaPods te installeren die worden gedeclareerd in het **podfile**-bestand:

```sh
pod install
```

Naast het installeren van de pods die nodig zijn voor uw project, heeft de installatieopdracht ook een XCode-werkruimtebestand gemaakt dat al geconfigureerd is om de pods te gebruiken voor afhankelijkheden.

### <a name="run-the-sample-service-application"></a>De voorbeeldtoepassing voor service uitvoeren

1. Haal de serviceverbindingsreeks voor uw IoT-hub. Kunt u deze tekenreeks van de [Azure-portal](https://portal.azure.com) uit de **iothubowner** beleid in de **beleid voor gedeelde toegang** blade, haalt u deze met de volgende CLI-opdracht:  

    ```azurecli-interactive
    az iot hub show-connection-string --name {YourIoTHubName} --output table
    ```

2. Open de voorbeeldwerkruimte in XCode.

   ```sh
   open AzureIoTServiceSample.xcworkspace
   ```

3. Vouw de **AzureIoTServiceSample** project en vouw vervolgens de map met dezelfde naam.  

4. Open **ViewController.swift** om het te bewerken in XCode. 

5. Zoek de **connectionString** variabele en werk de waarde bij met de service-verbindingsreeks die u eerder hebt gekopieerd.

6. Sla uw wijzigingen op. 

7. In Xcode, door de emulator-instellingen te wijzigen naar een ander iOS-apparaat dan u hebt gebruikt voor het uitvoeren van de IoT-apparaat. Meerdere emulators van hetzelfde type kan niet worden uitgevoerd in XCode. 

   ![Wijzigen van de emulator](media/iot-hub-ios-swift-c2d/change-device.png)

8. Voer het project in de apparaatemulator uit met de **bouwen en uitvoeren** knop of de toetscombinatie **Command + r**. 

   ![Het project uitvoeren](media/iot-hub-ios-swift-c2d/run-app.png)

## <a name="send-a-cloud-to-device-message"></a>Een cloud-naar-apparaat-bericht verzenden

U bent nu klaar voor gebruik van de twee toepassingen naar de cloud-naar-apparaat-berichten verzenden en ontvangen.

1. In de **voorbeeld-App voor iOS** app die wordt uitgevoerd op het gesimuleerde IoT-apparaat, klikt u op **Start**. De toepassing wordt gestart voor het verzenden van apparaat-naar-cloud-berichten, maar ook begint luistert naar berichten van cloud-naar-apparaat. 

   ![Voorbeeld-app voor IoT-apparaat weergeven](media/iot-hub-ios-swift-c2d/view-d2c.png)

2. In de **IoTHub-Service-clientvoorbeeld** app die wordt uitgevoerd op het gesimuleerde apparaat, voer de ID voor de IoT-apparaat die u wilt een bericht te verzenden. 

3. Schrijf een bericht als tekst zonder opmaak en klik vervolgens op **verzenden**. 

    Meerdere acties gebeuren als u op verzenden klikt. Voorbeeld van de service wordt het bericht verzonden naar uw IoT hub die de app heeft toegang tot het vanwege de verbinding met de tekenreeks die u hebt opgegeven. Uw IoT-hub controleert de apparaat-ID, wordt het bericht verzonden naar het doelapparaat en verzendt een bevestiging van ontvangst naar het bronapparaat. De app in uw gesimuleerde IoT-apparaat controleert op berichten uit IoT Hub en de tekst van de meest recente versie van het scherm wordt afgedrukt.

    Uw uitvoer moet eruitzien als in het volgende voorbeeld:

   ![Cloud-naar-apparaat-berichten weergeven](media/iot-hub-ios-swift-c2d/view-c2d.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u cloud-naar-apparaat-berichten verzenden en ontvangen. 

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub, de [Azure IoT-oplossingsversnellers](https://azure.microsoft.com/documentation/suites/iot-suite/) documentatie.

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md).

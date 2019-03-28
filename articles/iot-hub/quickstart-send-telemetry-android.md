---
title: Telemetrie verzenden naar Azure IoT Hub (Android) | Microsoft Docs
description: In deze quickstart voert u een Android-voorbeeldtoepassing uit om gesimuleerde telemetrie te verzenden naar een IoT-hub, en telemetrie van de IoT-hub te lezen voor verwerking in de cloud.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/15/2019
ms.author: wesmc
ms.openlocfilehash: 63e1f6a6779cf2689a7cbffad06447d272a413d7
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58516923"
---
# <a name="quickstart-send-iot-telemetry-from-an-android-device"></a>Snelstart: IoT-telemetrie verzenden vanaf een Android-apparaat

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub is een Azure-service waarmee grote hoeveelheden telemetrie van uw IoT-apparaten naar de cloud kunt opnemen voor opslag of verwerking. In deze quickstart verzendt u telemetrie naar een IoT-hub vanaf een Android-toepassing die wordt uitgevoerd op een fysiek of gesimuleerd apparaat.

In deze quickstart wordt een vooraf geschreven Android-toepassing gebruikt om de telemetrie te verzenden. De telemetrie wordt gelezen vanuit de IoT-hub met behulp van Azure Cloud Shell. Voordat u de toepassing kunt uitvoeren, moet u een IoT-hub maken en een apparaat registreren bij de hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Android Studio vanaf https://developer.android.com/studio/. Zie [Android-installatie](https://developer.android.com/studio/install) voor meer informatie over de installatie van Android Studio. 

* Voor het voorbeeld in dit artikel wordt Android SDK 27 gebruikt. 

* De [Android-voorbeeldtoepassing](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/device/AndroidSample) die u uitvoert in deze snelstartgids, maakt deel uit van de opslagplaats azure-iot-samples-java in GitHub. Download of kloon de opslagplaats [azure-iot-samples-java](https://github.com/Azure-Samples/azure-iot-samples-java).



## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstart gebruikt u Azure Cloud Shell om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdrachten uit in Azure Cloud Shell om de IoT Hub CLI-extensie toe te voegen en de apparaat-id te maken. 

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

   **MyAndroidDevice**: MyAndroidDevice is de naam van het geregistreerde apparaat. Gebruik MyAndroidDevice zoals wordt weergegeven. Als u een andere naam voor het apparaat kiest, moet u deze naam ook in de rest van dit artikel gebruiken, en moet u de apparaatnaam bijwerken in de voorbeeldtoepassingen voordat u ze uitvoert.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyAndroidDevice
    ```

2. Voer de volgende opdrachten uit in Azure Cloud Shell om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u zojuist hebt geregistreerd:

    **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyAndroidDevice --output table
    ```

    Noteer de apparaatverbindingsreeks. Deze ziet er ongeveer als volgt uit:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}`

    U gebruikt deze waarde later in deze quickstart om telemetrie te verzenden.

## <a name="send-telemetry"></a>Telemetrie verzenden

1. Open het GitHub-voorbeeldproject voor Android in Android Studio. Het project bevindt zich in de volgende map met uw gekloonde of gedownloade kopie van de opslagplaats [azure-iot-sample-java](https://github.com/Azure-Samples/azure-iot-samples-java).

        \azure-iot-samples-java\iot-hub\Samples\device\AndroidSample

2. Open in Android Studio *gradle.properties* voor het voorbeeldproject en vervang de tijdelijke plaatsaanduiding **Device_Connection_String** door de apparaatverbindingsreeks die u eerder hebt genoteerd.

    ```
    DeviceConnectionString=HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}
    ```

3. Klik in Android Studio op **Bestand** > **Project synchroniseren met Gradle-bestanden**. Controleer of de build is voltooid.

   > [!NOTE]
   > Als de project-synchronisatie is mislukt, kan het zijn voor een van de volgende redenen:
   >
   > * De versies van de Android Gradle-invoegtoepassing en Gradle waarnaar wordt verwezen in het project zijn verouderd voor uw versie van Android Studio. Ga als volgt [deze instructies](https://developer.android.com/studio/releases/gradle-plugin) om te verwijzen naar en installeren van de juiste versie van de invoegtoepassing en gradle zijn voor uw installatie.
   > * De gebruiksrechtovereenkomst voor de Android SDK is niet ondertekend. Volg de instructies in de uitvoer van de Build ondertekenen van de gebruiksrechtovereenkomst en downloadt u de SDK.

4. Zodra de build is voltooid, klikt u op **Uitvoeren** > **App uitvoeren**. Configureer de app om te worden uitgevoerd op een fysiek Android-apparaat of een Android-emulator. Zie [Uw app uitvoeren](https://developer.android.com/training/basics/firstapp/running-app) voor meer informatie over het uitvoeren van een Android-app op een fysiek apparaat of een emulator.

5. Zodra de app wordt geladen, klikt u op de knop **Start** om telemetrie te verzenden naar de IoT-hub:

    ![Toepassing](media/quickstart-send-telemetry-android/sample-screenshot.png)


## <a name="read-the-telemetry-from-your-hub"></a>De telemetrie van uw hub lezen

In deze sectie gebruikt u Azure Cloud Shell met de [IoT-extensie](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) voor het bewaken van de apparaatberichten die worden verzonden met het Android-apparaat.

1. Voer met de Azure Cloud Shell de volgende opdracht uit om te verbinden en berichten te lezen uit uw IoT-hub:

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```
    In de volgende schermafbeelding ziet u de uitvoer op het moment dat de IoT-hub de telemetrie ontvangt die is verzonden met het Android-apparaat:

      ![Lees de apparaatberichten met de Azure CLI](media/quickstart-send-telemetry-android/read-data.png)


## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een IoT-hub geconfigureerd, een apparaat geregistreerd, gesimuleerde telemetrie verzonden naar de hub met behulp van een Android-toepassing, en de telemetrie van de hub gelezen met behulp van Azure Cloud Shell.

Ga verder met de volgende snelstartgids als u wilt weten hoe u een gesimuleerd apparaat beheert vanuit een back-endtoepassing.

> [!div class="nextstepaction"]
> [Snelstart: Een apparaat beheren dat is verbonden met een IoT-hub](quickstart-control-device-android.md)


---
title: Ontwikkelen voor Android dingen platform met Azure IoT SDK's | Microsoft Docs
description: "Developer guide: meer informatie over het ontwikkelen van Android dingen met Azure IoT Hub SDK's."
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 1/30/2019
ms.author: yizhon
ms.openlocfilehash: 0bfba7f923ca394aa29dd907db1b8b1284a605d8
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55981669"
---
# <a name="develop-for-android-things-platform-using-azure-iot-sdks"></a>Ontwikkelen voor Android dingen platform met Azure IoT SDK 's
[Azure IoT Hub SDK's](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) eerste laag ondersteuning bieden voor populaire platforms zoals Windows, Linux, OSX, MBED en mobiele platforms, zoals Android en iOS.  Als onderdeel van onze inzet om in te schakelen meer keuze en flexibiliteit in IoT-implementaties, de Java-SDK biedt ook ondersteuning voor [Android dingen](https://developer.android.com/things/) platform.  Ontwikkelaars kunnen gebruikmaken van de voordelen van het besturingssysteem Android dingen aan het apparaat tijdens het gebruik van [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/about-iot-hub) als het bericht centrale hub die kan worden geschaald naar miljoenen gelijktijdig verbonden apparaten. 

In deze zelfstudie bevat de stappen voor het bouwen van een toepassing op apparaten op Android dingen met de Azure IoT-Java-SDK.

## <a name="prerequisites"></a>Vereisten
* Een Android dingen ondersteunde hardware met Android dingen besturingssysteem.  U kunt volgen [Android dingen documentatie](https://developer.android.com/things/get-started/kits#flash-at) over hoe u Android dingen OS flash.  Zorg ervoor dat uw zaken op Android-apparaat is verbonden met internet met essentiële randapparatuur, zoals toetsenbord, weergeven en muis aangesloten.  In deze zelfstudie maakt gebruik van Raspberry Pi 3.
* Meest recente versie van [Android Studio](https://developer.android.com/studio/)
* Meest recente versie van [Git](https://git-scm.com/)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstart gebruikt u Azure Cloud Shell om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdrachten uit in Azure Cloud Shell om de IoT Hub CLI-extensie toe te voegen en de apparaat-id te maken. 

   **YourIoTHubName** : vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

   **MyAndroidThingsDevice** : dit is de naam van het geregistreerde apparaat. MyAndroidThingsDevice gebruiken zoals wordt weergegeven. Als u een andere naam voor het apparaat kiest, moet u deze naam ook in de rest van dit artikel gebruiken, en moet u de apparaatnaam bijwerken in de voorbeeldtoepassingen voordat u ze uitvoert.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyAndroidThingsDevice
    ```

2. Voer de volgende opdrachten uit in Azure Cloud Shell om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u zojuist hebt geregistreerd:  **YourIoTHubName** : vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyAndroidThingsDevice --output table
    ```

    Noteer de apparaatverbindingsreeks. Deze ziet er ongeveer als volgt uit:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidThingsDevice;SharedAccessKey={YourSharedAccessKey}`

    U gebruikt deze waarde verderop in de snelstartgids.

## <a name="building-an-android-things-application"></a>Het bouwen van een toepassing Android dingen
1.  De eerste stap bij het bouwen van een Android dingen-toepassing verbinding maakt met uw dingen op Android-apparaten.  Uw apparaat Android dingen koppelen aan een weer te geven en verbinding te maken met het internet.  Android dingen bieden [documentatie](https://developer.android.com/things/get-started/kits) op verbinding maken met Wi-Fi.  Nadat u hebt verbonden met internet, let u op het IP-adres dat is vermeld in netwerken.
2.  Gebruik de [adb](https://developer.android.com/studio/command-line/adb) hulpprogramma verbinding maken met uw dingen op Android-apparaat met het IP-adres hierboven vermeld.  Controleer de verbinding met deze opdracht in uw terminal.  U ziet de apparaten die worden vermeld als 'verbonden'
    ```
    adb devices
    ```
3.  Ons voorbeeld downloaden voor Android/Android dingen uit deze [opslagplaats](https://github.com/Azure-Samples/azure-iot-samples-java) of gebruik Git.
    ```
    git clone https://github.com/Azure-Samples/azure-iot-samples-java.git
    ```
4.  Open in '\azure-iot-samples-java\iot-hub\Samples\device\AndroidSample' bevindt zich in de Android-Project in Android Studio.
5.  Open gradle.properties bestand en vervang 'Device_connection_string' met de apparaatverbindingsreeks die u eerder hebt genoteerd.
    ![Schermafbeelding van de master-vertakking van de opslagplaats](./media/how-to-android-things/connection-string.png)
6.  Klik op uitvoeren - fouten opsporen en selecteer uw apparaat voor het implementeren van deze code naar uw zaken op Android-apparaten.
7.  Wanneer de toepassing wordt gestart, ziet u een toepassing die wordt uitgevoerd op uw apparaat Android dingen.  Deze voorbeeldtoepassing wordt verzonden, serverlogs, willekeurig gegenereerde temperatuur.

## <a name="read-the-telemetry-from-your-hub"></a>De telemetrie van uw hub lezen

U kunt de gegevens via uw IoT-hub bekijken terwijl ze worden ontvangen. De IoT Hub CLI-extensie kan verbinding maken met het **Gebeurtenissen**-eindpunt aan de servicezijde van uw IoT Hub. De extensie ontvangt de berichten die van het gesimuleerde apparaat naar de cloud worden verzonden. Een back-endtoepassing van IoT Hub wordt meestal uitgevoerd in de cloud om berichten van apparaat naar cloud te ontvangen en verwerken.

Voer de volgende opdrachten uit in Azure Cloud Shell. Vervang daarbij `YourIoTHubName` door de naam van uw IoT-hub:

```
azurecli-interactive
az iot hub monitor-events --device-id MyAndroidThingsDevice --hub-name YourIoTHubName
```

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [over het beheren van verbindingen en betrouwbare berichten](iot-hub-reliability-features-in-sdks.md) met behulp van de IoT Hub SDK's.
* Meer informatie over het [ontwikkelen voor mobiele platforms](iot-hub-how-to-develop-for-mobile-devices.md) , zoals iOS en Android.
* [Ondersteuning voor het platform van Azure IoT-SDK](iot-hub-device-sdk-platform-support.md)

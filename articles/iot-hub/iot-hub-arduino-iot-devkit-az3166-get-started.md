---
title: IoT DevKit naar de cloud--IoT DevKit AZ3166 verbindt met Azure IoT Hub | Microsoft Docs
description: In deze zelfstudie leert u hoe u instelt en verbindt IoT DevKit AZ3166 met Azure IoT Hub, zodat het verzenden van gegevens naar de Azure-cloud-platform.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/17/2019
ms.author: wesmc
ms.openlocfilehash: 2f86b74299b5d47a87ed0b8e89a992f0f91a84be
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64924635"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub"></a>IoT DevKit AZ3166 verbinden met Azure IoT Hub

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

U kunt de [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) te ontwikkelen en prototype Internet of Things (IoT)-oplossingen die van Microsoft Azure-services profiteren. Het bevat een bord Arduino-compatibel met uitgebreide randapparatuur en sensoren, een open-source-bord-pakket en een uitgebreide [Voorbeeldgalerie](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-learn"></a>Wat u leert

* Over het maken van een IoT-hub en een apparaat registreren voor de MXChip IoT DevKit.
* Hoe u de IoT DevKit verbinden met Wi-Fi en de IoT Hub-verbindingsreeks configureren.
* Over het verzenden van de DevKit sensor-telemetriegegevens naar uw IoT-hub.
* Hoe u de ontwikkelomgeving voorbereiden en ontwikkelen van toepassingen voor de IoT DevKit.

Heb je nog een DevKit? Probeer de [DevKit simulator](https://azure-samples.github.io/iot-devkit-web-simulator/) of [aanschaffen van een DevKit](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-need"></a>Wat u nodig hebt

* Een bord MXChip IoT DevKit met een Micro-USB-kabel. [Nu downloaden](https://aka.ms/iot-devkit-purchase).
* Een computer met Windows 10, macOS 10.10 + of Ubuntu 18.04 +.
* Een actief Azure-abonnement. [Activeren van een gratis 30-daagse proefversie Microsoft Azure-account](https://azureinfo.microsoft.com/us-freetrial.html).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]
  
## <a name="prepare-your-hardware"></a>Bereid uw hardware

De volgende hardware op de computer koppelen:

* DevKit bord
* Micro-USB-kabel

![Vereiste hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

Als u wilt de DevKit verbinden met uw computer, de volgende stappen uit:

1. Het einde van de USB-verbinding naar uw computer.

2. Het einde Micro-USB verbinden met de DevKit.

3. De groene LED voor energiebeheer wordt bevestigd dat de verbinding.

   ![Hardwareverbindingen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="quickstart-send-telemetry-from-devkit-to-an-iot-hub"></a>Quickstart: Verzenden van telemetrie uit DevKit naar een IoT-Hub

De Snelstartgids wordt vooraf gecompileerde DevKit firmware gebruikt voor het verzenden van de telemetrie naar de IoT Hub. Voordat u deze uitvoert, moet u een IoT-hub maken en registreren van een apparaat met de hub.

### <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstart gebruikt u Azure Cloud Shell om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdracht in Azure Cloud Shell te maken van de apparaat-id.

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

   **MyNodeDevice**: De naam van het apparaat dat u gaat registreren. Gebruik **MyNodeDevice**, zoals weergegeven. Als u een andere naam voor het apparaat kiest, moet u deze naam in de rest van dit artikel gebruiken, en moet u de apparaatnaam bijwerken in de voorbeeldtoepassingen voordat u ze uitvoert.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyNodeDevice
    ```
   > [!NOTE]
   > Als er een fout optreedt bij het uitvoeren van `device-identity`, installeert de [Azure IOT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension/blob/dev/README.md) voor meer informatie.
  
1. Voer de volgende opdrachten uit in Azure Cloud Shell om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u zojuist hebt geregistreerd:

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyNodeDevice --output table
    ```

    Noteer de apparaatverbindingsreeks. Deze ziet er ongeveer als volgt uit:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    U gebruikt deze waarde verderop in de snelstartgids.

### <a name="send-devkit-telemetry"></a>DevKit telemetrie verzenden

De DevKit maakt verbinding met een apparaatspecifiek eindpunt op uw IoT-hub en temperatuur en vochtigheid telemetrie verzendt.

1. Download de nieuwste versie van [GetStarted firmware](https://aka.ms/devkit/prod/getstarted/latest) voor IoT DevKit.

1. Zorg ervoor dat IoT DevKit verbinden met uw computer via USB. Open File Explorer er een USB-massaopslag-apparaat met de naam is **AZ3166**.
    ![Open Windows Explorer](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/az3166-usb.png)

1. Slepen en neerzetten de firmware zojuist hebt gedownload naar het apparaat voor massaopslag en deze wordt automatisch flash.
    ![Firmware kopiëren](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/copy-firmware.png)

1. Op de DevKit, houdt u ingedrukt **B**, push en laat de **opnieuw** knoppen, en vervolgens release **B**. Uw DevKit in de Azië en Stille Oceaan modus geplaatst. Het scherm weergegeven om te bevestigen, de serviceset-id (SSID) van de DevKit en de IP-adres van de configuratie-portal.
    ![Knop, knop B en SSID opnieuw instellen](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/wifi-ap.jpg)

    ![Modus instellen voor Azië en Stille Oceaan](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/set-ap-mode.gif)

1. Gebruik een webbrowser op een andere Wi-Fi ingeschakeld apparaat (computer of mobiele telefoon) verbinding maken met de IoT DevKit SSID weergegeven in de vorige stap. Als het om een wachtwoord wordt gevraagd, laat deze leeg.
    ![Verbinding maken met de SSID](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/connect-ssid.png)

1. Open **192.168.0.1** in de browser. Selecteer het Wi-Fi die u wilt dat de IoT DevKit verbinden als u wilt, typt u het Wi-Fi-wachtwoord en plak de verbindingsreeks die u hebt aangebracht, houd er rekening mee van eerder. Klik vervolgens op Save.
    ![Configuratie-UI](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/configuration-ui.png)

    > [!NOTE]
    > De IoT DevKit biedt alleen ondersteuning voor network 2,4 GHz. Controleer [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#wi-fi-configuration) voor meer informatie.

1. De verbindingsreeks van Wi-Fi informatie en het apparaat worden opgeslagen in de IoT DevKit wanneer u de pagina ziet.
    ![Resultaat van de configuratie](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/configuration-ui-result.png)

    > [!NOTE]
    > Nadat Wi-Fi is geconfigureerd, wordt uw referenties op het apparaat voor die verbinding behouden, zelfs als het apparaat losgekoppeld wordt.

1. De IoT DevKit opnieuw wordt opgestart in een paar seconden. Op het scherm DevKit ziet u het IP-adres voor de DevKit wordt gevolgd door de telemetriegegevens met inbegrip van temperatuur en vochtigheid-waarde met het aantal berichten verzenden naar Azure IoT Hub.
    ![WiFi IP](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/wifi-ip.jpg)

    ![Verzenden van gegevens](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/sending-data.jpg)

## <a name="prepare-the-development-environment"></a>De ontwikkelomgeving voorbereiden

Volg deze stappen voor de ontwikkelomgeving voorbereiden voor de DevKit:

### <a name="install-visual-studio-code-with-azure-iot-tools-extension-package"></a>Visual Studio Code installeren met hulpprogramma's voor Azure IoT-uitbreidingspakket

1. Installeer [Arduino IDE](https://www.arduino.cc/en/Main/Software). Het biedt de benodigde hulpprogrammaketen voor compileren en Arduino code uploaden.
    * **Windows**: Gebruik Windows Installer-versie. Niet installeert vanuit de App Store.
    * **macOS**: Sleep en zet de uitgepakte **Arduino.app** in `/Applications` map.
    * **Ubuntu**: Zoals in de map uitpakken `$HOME/Downloads/arduino-1.8.8`

2. Installeer [Visual Studio Code](https://code.visualstudio.com/), een platformoverschrijdende code-editor met krachtige intellisense source code is voltooid en ondersteuning, evenals een uitgebreide extensies foutopsporing kan worden geïnstalleerd vanaf marketplace.

3. VS Code starten, gaat u naar **Arduino** in de marketplace extensie en installeer deze. Deze uitbreiding biedt verbeterde ervaring voor het ontwikkelen van op Arduino-platform.
    ![Arduino installeren](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-arduino.png)

4. Zoek naar [hulpprogramma's voor Azure IoT](https://aka.ms/azure-iot-tools) in de marketplace extensie en installeer deze.
    ![Azure IoT-hulpprogramma's installeren](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-azure-iot-tools.png)

    Of gebruik deze directe koppeling:
    > [!div class="nextstepaction"]
    > [Hulpprogramma's voor Azure IoT-extensie pack installeren](vscode:extension/vsciot-vscode.azure-iot-tools)

    > [!NOTE]
    > De hulpprogramma's voor Azure IoT-extensie-pack bevat de [Azure IoT-apparaat Workbench](https://aka.ms/iot-workbench) die wordt gebruikt om te ontwikkelen en fouten opsporen op verschillende IoT devkit-apparaten. De [Azure IoT Hub Toolkit](https://aka.ms/iot-toolkit), ook worden opgenomen met het hulpprogramma's voor Azure IoT-extensie-pack, wordt gebruikt voor het beheren van en interactie met Azure IoT Hubs.

5. VS Code met Arduino-instellingen configureren.

    Klik in Visual Studio Code, op **bestand > Voorkeuren > instellingen**. Klik vervolgens op de **...**  en **settings.json Open**.
    ![Azure IoT-hulpprogramma's installeren](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/user-settings-arduino.png)
    
    Voeg de volgende regels configureren Arduino afhankelijk van uw platform: 

    * **Windows**:
      
        ```json
        "arduino.path": "C:\\Program Files (x86)\\Arduino",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

    * **macOS**:

        ```json
        "arduino.path": "/Applications",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

    * **Ubuntu**:
    
        Vervang de **{username}** tijdelijke aanduiding voor hieronder door uw gebruikersnaam.

        ```json
        "arduino.path": "/home/{username}/Downloads/arduino-1.8.8",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

6. Klik op `F1` om te openen de command palette, typ en selecteer **Arduino: Bord Manager**. Zoeken naar **AZ3166** en installeer de nieuwste versie.
    ![DevKit SDK installeren](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-az3166-sdk.png)

### <a name="install-st-link-drivers"></a>ST-koppeling stuurprogramma's installeren

[ST-koppeling/V2](https://www.st.com/en/development-tools/st-link-v2.html) is de USB-interface die IoT DevKit gebruikt om te communiceren met uw ontwikkelcomputer. U moet ze deze willen installeren op Windows naar de apparaatcode gecompileerde naar de DevKit flash. De specifieke stappen voor de machine toegang tot uw apparaat.

* **Windows**: Download en installeer USB-stuurprogramma van [STMicroelectronics website](https://www.st.com/en/development-tools/stsw-link009.html) of [directe koppeling](https://aka.ms/stlink-v2-windows).
* **macOS**: Er zijn geen stuurprogramma is vereist voor macOS.
* **Ubuntu**: Voer de opdrachten in de terminal en meld u af en meld u aan voor de groepswijziging door te voeren:
    ```bash
    # Copy the default rules. This grants permission to the group 'plugdev'
    sudo cp ~/.arduino15/packages/AZ3166/tools/openocd/0.10.0/linux/contrib/60-openocd.rules /etc/udev/rules.d/
    sudo udevadm control --reload-rules

    # Add yourself to the group 'plugdev'
    # Logout and log back in for the group to take effect
    sudo usermod -a -G plugdev $(whoami)
    ```

Nu zijn u klaar met het voorbereiden en configureren uw ontwikkelomgeving. Laat het ons bouw het GetStarted-voorbeeld dat u zojuist hebt uitgevoerd.

## <a name="build-your-first-project"></a>Bouw uw eerste project

### <a name="open-sample-code-from-sample-gallery"></a>Open voorbeeldcode van de galerie met voorbeelden

De IoT DevKit bevat een uitgebreide galerie met voorbeelden die u gebruiken kunt voor meer informatie over verbinding maken met de DevKit tot verschillende Azure-services.

1. Zorg ervoor dat uw IoT DevKit is **niet verbonden** op uw computer. VS Code eerst start, en vervolgens de DevKit verbinden met uw computer.

1. Klik op `F1` om te openen de command palette, typ en selecteer **Azure IoT-apparaat Workbench: Voorbeelden openen...** . Selecteer vervolgens **IoT DevKit** als bord.

1. Zoek in de pagina IoT Workbench voorbeelden **aan de slag** en klikt u op **Open voorbeeld**. Vervolgens selecteert het standaardpad voor het downloaden van de voorbeeldcode.
    ![Open voorbeeld](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/open-sample.png)

### <a name="provision-azure-iot-hub-and-device"></a>Richt Azure IoT Hub en apparaat

In plaats van de inrichting van Azure IoT Hub en apparaat vanuit de Azure-portal, kunt u dit doen in de VS Code zonder de ontwikkelomgeving.

1. Klik in het nieuwe venster geopende project op `F1` om te openen de command palette, typ en selecteer **Azure IoT-apparaat Workbench: Azure-Services inrichten...** . Volg de stapsgewijze handleiding voor het voltooien van uw Azure-IoT-Hub inrichten en het maken van de IoT Hub-apparaat.
    ![Opdracht inrichten](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/provision.png)

    > [!NOTE]
    > Als u geen Azure bent aangemeld. Volg de pop-upmelding voor het aanmelden.

1. Selecteer het abonnement dat u wilt gebruiken.
    ![Sub selecteren](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-subscription.png)

1. Selecteer of maak een nieuwe [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#terminology).
    ![Resourcegroep selecteren](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-resource-group.png)

1. In de resourcegroep die u hebt opgegeven, volgt u de handleiding om te selecteren of een nieuwe Azure-IoT-Hub maken.
    ![Stappen voor IoT-Hub selecteren](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-provision.png)

    ![IoT-Hub selecteren](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-iot-hub.png)

    ![Geselecteerde IoT Hub](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-selected.png)

1. In het uitvoervenster weergegeven, ziet u de Azure IoT-Hub ingericht ![ingericht voor IoT Hub](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-provisioned.png)

1. Selecteer of maak een nieuw apparaat in Azure IoT Hub die u hebt ingericht.
    ![Selecteer stappen van de IoT-apparaat](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-device-provision.png)

    ![Selecteer de IoT-apparaat is ingericht](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-iot-device.png)

1. U hebt nu Azure IoT Hub is ingericht en het apparaat is gemaakt in het. De verbindingsreeks van het apparaat wordt ook in VS Code voor het configureren van de IoT DevKit later worden opgeslagen.
    ![Inrichten is voltooid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/provision-done.png)

### <a name="configure-and-compile-device-code"></a>Configureren en apparaatcode compileren

1. Schakel in de statusbalk rechtsonder, de **MXCHIP AZ3166** wordt weergegeven als de geselecteerde bord en seriële poort met **STMicroelectronics** wordt gebruikt.
    ![Bord- en COM selecteren](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-com.png)

1. Klik op `F1` om te openen de command palette, typ en selecteer **Azure IoT-apparaat Workbench: Apparaatinstellingen configureren...** en selecteer vervolgens **Config Apparaatverbindingsreeks > Selecteer IoT Hub apparaat Connection String**.

1. Op DevKit, houdt u **een knop**, push en laat de **opnieuw** knop en loslaten **een knop**. Uw DevKit krijgt de configuratiemodus en slaat de verbindingsreeks.
    ![Verbindingsreeks](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connection-string.png)

1. Klik op `F1` Typ wederom en selecteer **Azure IoT-apparaat Workbench: Uploaden apparaatcode**. Het compileren wordt gestart en de code uploaden naar DevKit.
    ![Arduino uploaden](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/arduino-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

> [!NOTE]
> Als er fouten of onderbrekingen, kunt u altijd herstellen door de opdracht nogmaals uit te voeren.

## <a name="test-the-project"></a>Het project testen

### <a name="view-the-telemetry-sent-to-azure-iot-hub"></a>De telemetrie die verzonden naar Azure IoT Hub bekijken

Klik op het pictogram voor energiebeheer plug op de statusbalk te openen van de seriële Monitor: ![Seriële monitor](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/serial-monitor.png)

De voorbeeldtoepassing wordt uitgevoerd wanneer u de volgende resultaten ziet:

* De seriële Monitor geeft het bericht verzonden naar de IoT-Hub.
* De LED op de MXChip IoT DevKit knippert.

![Seriële monitor-uitvoer](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/result-serial-output.png)

### <a name="view-the-telemetry-received-by-azure-iot-hub"></a>De telemetrie ontvangen via Azure IoT Hub bekijken

U kunt [hulpprogramma's voor Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) voor het bewaken van apparaat-naar-cloud (D2C) berichten in IoT Hub.

1. Meld u aan [Azure-portal](https://portal.azure.com/), vinden de IoT-Hub die u hebt gemaakt.
    ![Azure Portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-hub-portal.png)

1. In de **beleid voor gedeelde toegang** deelvenster, klikt u op de **iothubowner beleid**, en noteer de verbindingsreeks van uw IoT-hub.
    ![Azure IoT Hub-verbindingsreeks](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-portal-conn-string.png)

1. In VS Code, klikt u op `F1`, typt en selecteer **Azure IoT Hub: IoT Hub-verbindingsreeks instellen**. Kopieer de verbindingsreeks naar het.
    ![Azure IoT Hub-verbindingsreeks instellen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/set-iothub-connection-string.png)

1. Vouw de **AZURE IOT HUB-apparaten** deelvenster aan de rechterkant, klik met de rechtermuisknop op de naam van het apparaat u hebt gemaakt en selecteer **Start Monitoring D2C Message**.
    ![Monitor D2C Message](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/monitor-d2c.png)

1. In **uitvoer** deelvenster ziet u de binnenkomende D2C-berichten naar de IoT Hub.
    ![D2C message](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/d2c-output.png)

## <a name="review-the-code"></a>De code bekijken

De `GetStarted.ino` is het belangrijkste Arduino schets-bestand.

![D2C message](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/code.png)

Als u wilt zien hoe de telemetrie van apparaten naar de Azure IoT Hub wordt verzonden, opent u de `utility.cpp` bestand in dezelfde map. Weergave [API-verwijzing](https://microsoft.github.io/azure-iot-developer-kit/docs/apis/arduino-language-reference/) voor meer informatie over het gebruik van sensoren en randapparatuur op IoT DevKit.

De `DevKitMQTTClient` gebruikt, is een wrapper van de **iothub_client** uit de [Microsoft Azure IoT SDK's en bibliotheken voor C](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client) om te communiceren met Azure IoT Hub.

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, kunt u controleren voor een oplossing in de [IoT DevKit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of contact opnemen met ons [Gitter](https://gitter.im/Microsoft/azure-iot-developer-kit). U kunt ons ook feedback geven door een opmerking op deze pagina verlaten.

## <a name="next-steps"></a>Volgende stappen

U hebt een MXChip IoT DevKit is verbonden met uw IoT-hub en u hebt de vastgelegde gegevens verzonden naar uw IoT-hub.

[!INCLUDE [iot-hub-get-started-az3166-next-steps](../../includes/iot-hub-get-started-az3166-next-steps.md)]

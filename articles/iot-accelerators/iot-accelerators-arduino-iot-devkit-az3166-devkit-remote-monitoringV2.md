---
title: IoT DevKit naar de cloud--verbinden met IoT DevKit AZ3166 oplossingsverbetering voor externe controle IoT | Microsoft Docs
description: In deze zelfstudie leert u hoe u de status van sensoren op IoT DevKit AZ3166 verzendt naar externe controle IoT-oplossingsversnellers voor het controleren en visualisatie.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: isacabe
ms.openlocfilehash: 32742b2a680370f443051e2d86f90d94e8632850
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720579"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>MXChip IoT DevKit AZ3166 verbinden met de externe controle IoT-oplossingsversnellers

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

U leert hoe u een voorbeeld-app op uw IoT DevKit om sensorgegevens te verzenden naar uw oplossingenaccelerator uitvoert.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een compatibele alles-in-een Arduino-bord met uitgebreide randapparatuur en sensoren. U kunt ontwikkelen via [Azure IoT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) in Visual Studio Code. En wordt geleverd met een groeiend [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) om u te begeleiden prototype Internet of Things (IoT)-oplossingen die van Microsoft Azure-services profiteren.

## <a name="what-you-need"></a>Wat u nodig hebt

Doorloop de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) en **voltooien van de volgende secties alleen**:

* Bereid uw hardware
* Wi-Fi configureren
* Start met behulp van de DevKit
* De ontwikkelomgeving voorbereiden

## <a name="open-the-remote-monitoring-sample-in-vs-code"></a>Het voorbeeld bewaking op afstand openen in VS Code

1. Zorg ervoor dat uw IoT DevKit is **niet verbonden** op uw computer. VS Code eerst start, en vervolgens de DevKit verbinden met uw computer.

2. Klik op `F1` openen het opdrachtenpalet, type en selecteer **IoT Workbench: voorbeelden**. Selecteer vervolgens **IoT DevKit** als bord.

3. Zoek **bewaking op afstand** en klikt u op **Open voorbeeld**. Een nieuwe VS Code-venster wordt het geopend met de map in het project.
  ![IoT-Workbench, selecteer Remote Monitoring-voorbeeld](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-example.png)

## <a name="configure-iot-hub-device-connection-string"></a>Verbindingsreeks voor IoT Hub-apparaat configureren

1. Overschakelen van de IoT DevKit in **configuratiemodus**. Dit doet u als volgt:
   * Houd **A**.
   * Push en laat de **opnieuw** knop.

2. Het scherm wordt weergegeven de DevKit-ID en 'Configuration'.
   
  ![IoT DevKit configuratiemodus](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/devkit-configuration-mode.png)

3. Klik op `F1` openen het opdrachtenpalet, type en selecteer **IoT Workbench: apparaat > configuratie-instellingen voor apparaten**.

4. Plak de verbindingsreeks die u zojuist hebt gekopieerd, klikt u op `Enter` te configureren.

## <a name="build-and-upload-the-device-code"></a>Bouwen en de apparaatcode uploaden

1. Klik op `F1` openen het opdrachtenpalet, type en selecteer **IoT Workbench: apparaat > apparaat uploaden**.
  ![IoT-Workbench: Apparaat - > uploaden](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-upload.png)

1. VS Code wordt vervolgens gestart compileren en de code uploaden naar uw DevKit.
  ![IoT-Workbench: Apparaat - > geüpload](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-uploaded.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

## <a name="test-the-project"></a>Het project testen

### <a name="view-the-telemetry-sent-to-remote-monitoring-solution"></a>De telemetrie die verzonden naar de oplossing voor externe controle weergeven

Wanneer de voorbeeld-app wordt uitgevoerd, verzendt DevKit sensorgegevens via Wi-Fi naar uw oplossing voor externe controle. Het resultaat wilt weergeven, de volgende stappen uit:

1. Ga naar het dashboard van uw oplossing en klik **apparaten**.

2. Klik op de naam van het apparaat, klikt u op het tabblad rechts ziet u de status van de sensor op DevKit in realtime.
  ![Sensorgegevens in Azure IoT Suite](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard.png)

### <a name="send-a-c2d-message"></a>Een C2D-bericht verzenden

Oplossing voor externe controle kunt u externe methode op het apparaat aanroepen. De code sxample publiceert drie methoden die u kunt zien in de **methode** sectie als de sensor is geselecteerd.

![IoT DevKit methoden](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-methods.png)

Laat het ons probeert de kleur van een van de DevKit-LED's met behulp van de methode 'LedColor' wijzigen.

1. Selecteer de naam van het apparaat uit de lijst met apparaten en klik op de **taken**.

  ![Een taak maken](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-job.png)

2. Configureren van de taken zoals hieronder en klik op **toepassen**.
  * Selecteer taak: **methode uitvoeren**
  * Methodenaam: **LedColor**
  * Taaknaam: **ChangeLedColor**
  
  ![Taakinstellingen](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-suite-change-color.png)

In enkele seconden, moet uw DevKit de kleur van de RGB-LED beheren (onder de knop A) wijzigen.

![IoT DevKit red geleid](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-devkit-led.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent verder te gaan met de zelfstudies, laat u de oplossingsverbetering voor externe controle geïmplementeerd.

Als u de oplossingsversneller niet meer nodig hebt, verwijderen van de pagina van de oplossing ingericht door te selecteren en vervolgens te klikken op de oplossing verwijderen:

![Oplossing verwijderen](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [de veelgestelde vragen over de IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of contact opnemen met ons opnemen met behulp van de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een apparaat DevKit verbinden met uw externe controle van Azure IoT-oplossingsversnellers en de sensorgegevens visualiseren, vindt hier u de voorgestelde volgende stappen:

* [Overzicht oplossingsversnellers Azure IoT](https://docs.microsoft.com/azure/iot-suite/)
* [De gebruikersinterface aanpassen](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md)
* [IoT DevKit verbinden met uw Azure IoT Central-toepassing](../iot-central/howto-connect-devkit.md)
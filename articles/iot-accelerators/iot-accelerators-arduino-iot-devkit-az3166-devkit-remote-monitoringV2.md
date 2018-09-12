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
ms.openlocfilehash: 35ef6ef02e5ae8a4b9231121615f44e0dc00ad15
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378734"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>MXChip IoT DevKit AZ3166 verbinden met de externe controle IoT-oplossingsversnellers

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

In deze zelfstudie leert u hoe u een voorbeeld-app op uw DevKit om sensorgegevens te verzenden naar uw oplossingenaccelerator uitvoert.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een compatibele alles-in-een Arduino-bord met uitgebreide randapparatuur en sensoren. U kunt ontwikkelen via [Azure IoT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) in Visual Studio Code. En wordt geleverd met een groeiend [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) om u te begeleiden prototype Internet of Things (IoT)-oplossingen die van Microsoft Azure-services profiteren.

## <a name="what-you-need"></a>Wat u nodig hebt

Doorloop de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) en **voltooien van de volgende secties alleen**:

* Bereid uw hardware
* Wi-Fi configureren
* Start met behulp van de DevKit
* De ontwikkelomgeving voorbereiden


## <a name="create-an-azure-iot-remote-monitoring-solution-accelerator"></a>Maken van een externe controle van Azure IoT-oplossingsversnellers

Het AZ3166-apparaat dat in deze zelfstudie u maakt verzendt gegevens naar een exemplaar van de oplossingsverbetering voor externe controle. Als u dit nog niet hebt nog een exemplaar in uw Azure-account ingericht, volgt u de [snelstartgids](https://docs.microsoft.com/azure/iot-accelerators/quickstart-remote-monitoring-deploy) handleiding om dit te doen.

Wanneer de implementatie voor de oplossing voor externe controle is voltooid, opent u het dashboard van de oplossing.

![Dashboard van de oplossing](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard-info.png)

## <a name="add-a-device-to-the-remote-monitoring-solution"></a>Een apparaat toevoegt aan de oplossing voor externe controle

1. In het dashboard, gaat u naar **apparaten** sectie en klikt u op de **nieuw apparaat**.
   ![Een nieuw apparaat toevoegen](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-add-device.png)

2. Het apparaat configureren met behulp van onderstaande informatie en klik op **toepassen**.
  * Apparaattype: **fysieke**
  * Apparaat-ID: **AZ3166**
  * Verificatietype: **symmetrische sleutel**
  * Verificatiesleutel: **automatisch sleutels genereren**
  
  ![De vorm van een nieuw apparaat toe te voegen](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-add-new-device-form.png)

3. Nadat het nieuwe apparaat is gemaakt, kopieert u de **verbindingsreeks-primaire sleutel**. Dit is het apparaat dat zojuist is gemaakt in Azure IoT Hub onder.
  
  ![Apparaat-verbindingsreeks](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-new-device-connstring.png)

## <a name="open-the-remote-monitoring-sample-in-vs-code"></a>Het voorbeeld bewaking op afstand openen in VS Code

1. Zorg ervoor dat uw IoT DevKit is **niet verbonden** op uw computer. VS Code eerst start, en vervolgens de DevKit verbinden met uw computer.

1. Klik op `F1` openen het opdrachtenpalet, type en selecteer **IoT Workbench: voorbeelden**. Selecteer vervolgens **IoT DevKit** als bord.

1. Zoek **bewaking op afstand** en klikt u op **Open voorbeeld**. Een nieuwe VS Code-venster wordt het geopend met de map in het project.
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

1. Klik op de naam van het apparaat (AZ3166) er wordt een tabblad geopend aan de rechterkant van het dashboard, waar u de status van de sensor op DevKit in realtime zien kunt.
  ![Sensorgegevens in Azure IoT Suite](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard.png)

### <a name="send-a-c2d-message"></a>Een C2D-bericht verzenden

Oplossing voor externe controle kunt u externe methode op het apparaat aanroepen. De code sxample publiceert drie methoden die u kunt zien in de **methode** sectie als de sensor is geselecteerd.

![IoT DevKit methoden](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-methods.png)

Laat het ons probeert de kleur van een van de DevKit-LED's met behulp van de methode 'LedColor' wijzigen.

1. Selecteer de **AZ3166** uit de lijst met apparaten en klik op de **taken**.

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
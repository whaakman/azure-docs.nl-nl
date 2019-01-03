---
title: IoT DevKit verbinden met externe controle oplossingsverbetering - Azure | Microsoft Docs
description: In deze handleiding leert u hoe u voor het verzenden van telemetrie van de sensoren op IoT DevKit AZ3166 apparaat de aan de oplossingsverbetering voor externe controle voor controle en visualisatie.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: isacabe
ms.openlocfilehash: b14a36e79488f586173a6f4c8b81a24d8ce24806
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53727493"
---
# <a name="connect-an-iot-devkit-device-to-the-remote-monitoring-solution-accelerator"></a>Een apparaat IoT DevKit verbinden met de oplossingsverbetering voor externe controle

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

In deze gebruiksaanwijzing laat zien hoe u een voorbeeld van toepassing op uw apparaat IoT DevKit uitvoeren. De voorbeeldcode wordt telemetrie van de sensoren op het apparaat DevKit verzendt naar uw oplossingenaccelerator.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een compatibele alles-in-een Arduino-bord met uitgebreide randapparatuur en sensoren. U kunt ontwikkelen voor met behulp van [Azure IoT-apparaat Workbench](https://aka.ms/iot-workbench) of [hulpprogramma's voor Azure IoT](https://aka.ms/azure-iot-tools) -uitbreidingspakket in Visual Studio Code. De [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) bevat voorbeeldtoepassingen om u te helpen prototype IoT-oplossingen.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt de stappen in deze zelfstudie hebt voltooid, moet u eerst de volgende taken uitvoeren:

* Voorbereiden van uw DevKit met de volgende stappen in [IoT DevKit AZ3166 verbinding maken met Azure IoT Hub in de cloud](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).

## <a name="open-sample-project"></a>Open voorbeeldproject

Het voorbeeld bewaking op afstand openen in VS Code:

1. Zorg ervoor dat uw IoT DevKit is niet op uw computer. VS Code eerst start, en vervolgens de DevKit verbinden met uw computer.

1. Klik op `F1` om te openen de command palette, typ en selecteer **Azure IoT-apparaat Workbench: Voorbeelden openen...** . Selecteer vervolgens **IoT DevKit** als bord.

1. Zoek **bewaking op afstand** en klikt u op **Open voorbeeld**. Een nieuwe VS Code-venster wordt geopend met de projectmap:

  ![IoT-Workbench, selecteer Remote Monitoring-voorbeeld](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-example.png)

## <a name="configure-the-device"></a>Het apparaat configureren

IoT Hub apparaat-verbindingsreeks op uw apparaat DevKit configureren:

1. Overschakelen van de IoT DevKit in **configuratiemodus**:

    * Houd **A**.
    * Push en laat de **opnieuw** knop.

1. Het scherm wordt weergegeven de DevKit-ID en `Configuration`.

    ![IoT DevKit configuratiemodus](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/devkit-configuration-mode.png)

1. Druk op **F1** om te openen de command palette, typ en selecteer **Azure IoT-apparaat Workbench: Configureren van apparaatinstellingen... > Config Apparaatverbindingsreeks**.

1. Plak de verbindingsreeks die u eerder hebt gekopieerd en druk op **Enter** het apparaat te configureren.

## <a name="build-the-code"></a>De code bouwen

Om te bouwen en de apparaatcode uploaden:

1. Druk op `F1` om te openen de command palette, typ en selecteer **Azure IoT-apparaat Workbench: Uploaden apparaatcode**:

1. VS Code kan worden gecompileerd en wordt de code geüpload naar uw apparaat DevKit:

    ![IoT-Workbench: Apparaat - > geüpload](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-uploaded.png)

1. Het apparaat DevKit opnieuw wordt opgestart en wordt uitgevoerd op de code die u hebt geüpload.

## <a name="test-the-sample"></a>Het voorbeeld testen

Om te controleren of de voorbeeldtoepassing die u hebt geüpload naar het apparaat DevKit werkt, voert u de volgende stappen uit:

### <a name="view-the-telemetry-sent-to-remote-monitoring-solution"></a>De telemetrie die verzonden naar de oplossing voor externe controle weergeven

Wanneer de voorbeeld-app wordt uitgevoerd, verzendt het apparaat DevKit telemetrie van de gegevens van sensoren via Wi-Fi naar uw oplossingenaccelerator. De telemetrie bekijken:

1. Ga naar het dashboard van uw oplossing en klik **apparaten**.

1. Klik op de naam van het apparaat van uw apparaat DevKit. u kunt de telemetrie van de DevKit in realtime zien op het tabblad rechter:

    ![Sensorgegevens in Azure IoT Suite](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard.png)

### <a name="control-the-devkit-device"></a>Het apparaat DevKit beheren

De oplossingsverbetering voor externe controle kunt u uw apparaat op afstand beheren. De voorbeeldcode implementeert drie methoden die u kunt zien in de **methode** sectie wanneer u het apparaat selecteert op de **apparaten** pagina:

![IoT DevKit methoden](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-methods.png)

U kunt de kleur van een van de DevKit-LED's wijzigen met de **LedColor** methode:

1. Selecteer de naam van het apparaat uit de lijst met apparaten en klik op de **taken**:

    ![Een taak maken](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-job.png)

1. Configureren van de taken die met behulp van de volgende waarden en klik op **toepassen**:

    * Selecteer de taak: **Methode uitvoeren**
    * Methodenaam: **LedColor**
    * Taaknaam: **ChangeLedColor**

    ![Taakinstellingen](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-suite-change-color.png)

1. Na een paar seconden verandert de kleur van de RGB-LED (onder de knop A) op uw DevKit:

    ![IoT DevKit red geleid](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-devkit-led.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent verder te gaan met de zelfstudies, laat u de oplossingsverbetering voor externe controle geïmplementeerd.

Als u de oplossingsversneller niet meer nodig hebt, verwijderen van de pagina van de oplossing ingericht door te selecteren en vervolgens te klikken op de oplossing verwijderen:

![Oplossing verwijderen](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [de veelgestelde vragen over de IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of contact opnemen met ons opnemen met behulp van de volgende kanalen:

* [Gitter.IM](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een apparaat DevKit verbinden met uw oplossingsverbetering voor externe controle, worden hier enkele Voorgestelde volgende stappen:

* [Overzicht oplossingsversnellers Azure IoT](https://docs.microsoft.com/azure/iot-accelerators/)
* [De gebruikersinterface aanpassen](iot-accelerators-remote-monitoring-customize.md)
* [IoT DevKit verbinden met uw Azure IoT Central-toepassing](../iot-central/howto-connect-devkit.md)
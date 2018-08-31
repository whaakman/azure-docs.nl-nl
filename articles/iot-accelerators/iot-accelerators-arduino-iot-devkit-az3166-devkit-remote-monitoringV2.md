---
title: IoT DevKit naar de cloud--verbinden met IoT DevKit AZ3166 oplossingsverbetering voor externe controle IoT | Microsoft Docs
description: In deze zelfstudie leert u hoe u de status van sensoren op IoT DevKit AZ3166 verzendt naar externe controle IoT-oplossingsversnellers voor het controleren en visualisatie.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: isacabe
ms.openlocfilehash: 92ce85a3cc94702468a13348d3a41667498c68f5
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43187388"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>MXChip IoT DevKit AZ3166 verbinden met de externe controle IoT-oplossingsversnellers


[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

In deze zelfstudie leert u hoe u een voorbeeld-app op uw DevKit om sensorgegevens te verzenden naar uw oplossingenaccelerator uitvoert.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een compatibele alles-in-een Arduino-bord met uitgebreide randapparatuur en sensoren. U kunt ontwikkelen via [Visual Studio Code-extensie voor Arduino](https://aka.ms/arduino). En wordt geleverd met een groeiend [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) om u te begeleiden prototype Internet of Things (IoT)-oplossingen die van Microsoft Azure-services profiteren.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) aan:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden


## <a name="open-the-remotemonitoring-sample"></a>Open het voorbeeld RemoteMonitoring

1. De DevKit loskoppelt van de computer, als deze is verbonden.

2. Start VS Code.

3. De DevKit verbinden met uw computer. VS Code uw DevKit detecteert automatisch en opent de volgende pagina's:
  * De introductiepagina DevKit.
  * Voorbeelden van Arduino: Praktische voorbeelden aan de slag met DevKit.

4. Vouw linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **RemoteMonitoringv2**. Een nieuwe VS Code-venster wordt het geopend met een projectmap daarin.

  ![Externe controle project openen](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-arduino-examples.png)


  > [!NOTE]
  > Als u het deelvenster sluiten, kunt u deze opnieuw openen. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) om te openen de command palette, typt u **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

## <a name="add-a-new-physical-device"></a>Een nieuwe fysiek apparaat toevoegen

In de portal, gaat u naar **apparaten** sectie en klik de **+ nieuwe apparaat** knop. 

![Een nieuw apparaat toevoegen](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device.png)

De *nieuw apparaat formulier* moeten worden ingevuld.
1. Klik op **fysieke** in de *apparaattype* sectie.
2. Uw eigen apparaat-ID definiëren (bijvoorbeeld *MXChip* of *AZ3166*).
3. Kies **automatisch sleutels genereren** in de *verificatiesleutel* sectie.
4. Klik op *toepassen* knop.

![De vorm van een nieuw apparaat toe te voegen](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-new-device-form.png)

Wacht totdat de portal is voltooid de inrichting van het nieuwe apparaat.

![Een nieuw apparaat inrichten ](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device-provisioning.png)


Vervolgens wordt de configuratie van het nieuwe apparaat worden weergegeven.
Kopieer de **Connection String** gegenereerd.

![Apparaat-verbindingsreeks](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-new-device-connstring.png)


Deze verbindingsreeks wordt gebruikt in de volgende sectie.





## <a name="build-and-upload-the-device-code"></a>Bouwen en de apparaatcode uploaden

Ga terug naar de Visual Studio-Code: 

1. Gebruik `Ctrl+P` (Mac OS: `Cmd + P`) en het type **config apparaatverbinding taak**.

  ![Kies uw Azure-abonnement en uw IoT-Hub](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion.png)

2. De terminal wordt u gevraagd of u wilt gebruiken de verbindingsreeks van de IoT-apparaat dat u wilt gebruiken. Selecteer *Maak een nieuwe*, en nu te plakken.

  ![Plak de verbindingsreeks](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion-choose-iot-hub-press-button-A.png)

3. De terminal soms wordt u gevraagd om in te voeren van de configuratiemodus. Om dit te doen, houdt u ingedrukt A, en vervolgens push- en release van de knop opnieuw instellen en loslaten op de knop A. Het scherm wordt weergegeven de DevKit-ID en 'Configuration'.

  ![Apparaatscherm DevKit](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-screen.png)

  > [!NOTE]
  > De verbindingsreeks moet worden opgeslagen in het Klembord als u de laatste sectie van deze zelfstudie hebt gevolgd. Als u moet niet zo is, gaat u naar de Azure-portal en zoek naar de IoT-Hub van uw resourcegroep externe controle. Daar kunt u zien dat de IoT-Hub verbonden apparaten en kopieer de verbindingsreeks van het apparaat.

  ![Zoek naar de connection string](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-connection-string-of-a-device.png)


U kunt nu uw nieuwe fysiek apparaat in de sectie VS Code 'Azure IoT Hub-apparaten' zien:

![U ziet dat de nieuwe IoT Hub-apparaat](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-new-iot-hub-device.png)

## <a name="test-the-project"></a>Het project testen

Wanneer de voorbeeld-app wordt uitgevoerd, verzendt DevKit sensorgegevens via Wi-Fi naar uw IoT-oplossingsversnellers. Het resultaat wilt weergeven, de volgende stappen uit:

1. Ga naar uw IoT-oplossingsversnellers en klikt u op **DASHBOARD**.

2. Op de IoT-oplossing accelerator-console ziet u de status van uw DevKit sensor. 

![Sensorgegevens in IoT-oplossingsversnellers](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-dashboard.png)

Als u op de sensornaam van de (AZ3166 klikt) wordt een tabblad geopend aan de rechterkant van het dashboard, waar u de grafiek MX-Chip sensoren in realtime kunt zien.


## <a name="send-a-c2d-message"></a>Een C2D-bericht verzenden
Externe bewaking v2 kunt u externe methode op het apparaat aanroepen.
De voorbeeldcode MX-Chip publiceert drie methoden die u in de sectie methode zien kunt wanneer de sensor is geselecteerd.

![Methoden MX-Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-methods.png)

U kunt de kleur van een van de MX-Chip-LED's met behulp van de methode 'LedColor' wijzigen. Om dit te doen, selecteert u het selectievakje in van het apparaat en klik op de knop planning. 

![Methoden MX-Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-schedule.png)

Kies de methode ChangeColor aangeroepen in de vervolgkeuzelijst waar alle methoden worden weergegeven, een naam in te schrijven en toepassen.

![Vervolgkeuzelijst MX-Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-change-color.png)

In enkele seconden uw fysieke MX-Chip de kleur van de RGB geleid moet wijzigen (hieronder van de knop)

![Leidde MX-Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-led.png)

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of contact met ons opnemen uit de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een apparaat DevKit verbinden met uw IoT-oplossingsversnellers en de sensorgegevens visualiseren, vindt hier u de voorgestelde volgende stappen:

* [Overzicht oplossingsversnellers IoT](https://docs.microsoft.com/azure/iot-suite/)
* [Een apparaat MXChip IoT DevKit verbinden met uw Microsoft IoT Central-toepassing](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
* [IoT developer kit](https://microsoft.github.io/azure-iot-developer-kit/)

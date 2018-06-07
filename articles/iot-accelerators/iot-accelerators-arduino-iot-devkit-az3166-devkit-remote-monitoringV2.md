---
title: 'IoT DevKit naar cloud: verbinding maken met IoT DevKit AZ3166 voor externe controle IoT oplossingsverbetering | Microsoft Docs'
description: Informatie over het verzenden van status van sensoren op IoT DevKit AZ3166 naar externe controle IoT oplossingsverbetering voor bewaking en visualisatie in deze zelfstudie.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: isacabe
ms.openlocfilehash: 8aa4d660fbf785a4caf99bcdeddc86aa9929c50d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627736"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>MXChip IoT DevKit AZ3166 verbinding met de externe controle IoT oplossingsverbetering


[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

In deze zelfstudie leert u hoe u een voorbeeld-app op uw DevKit sensorgegevens verzenden naar uw oplossingsverbetering uitvoert.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een Arduino alles in één mededelingenbord compatibel is met de randapparaten uitgebreide en sensoren. U kunt ontwikkelen via [Visual Studio Code-uitbreiding voor Arduino](https://aka.ms/arduino). En beschikt u over een groeiende [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) om u te begeleiden prototype Internet der dingen (IoT) oplossingen die van Microsoft Azure-services gebruikmaken.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) naar:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden


## <a name="open-the-remotemonitoring-sample"></a>Open het voorbeeld RemoteMonitoring

1. Verbreek de verbinding met de DevKit van uw computer, als deze is verbonden.

2. Start VS-Code.

3. De DevKit aansluiten op uw computer. VS Code uw DevKit detecteert automatisch en Hiermee opent u de volgende pagina's:
  * De introductiepagina DevKit.
  * Voorbeelden van Arduino: Praktische voorbeelden aan de slag met DevKit.

4. Vouw de linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **RemoteMonitoringv2**. Er wordt een nieuw venster van de VS Code geopend met een projectmap in het.

  ![Externe controle project openen](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-arduino-examples.png)


  > [!NOTE]
  > Als u per ongeluk het venster sluit, kunt u deze opnieuw openen. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) typt u de opdracht om palet te openen, **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

## <a name="add-a-new-physical-device"></a>Een nieuwe fysiek apparaat toevoegen

In de portal, gaat u naar **apparaten** sectie en klik de **+ nieuw apparaat** knop. 

![Een nieuw apparaat toe te voegen](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device.png)

De *nieuw apparaat formulier* moeten worden ingevuld.
1. Klik op **fysieke** in de *apparaattype* sectie.
2. Definieer uw eigen apparaat-ID (bijvoorbeeld *MXChip* of *AZ3166*).
3. Kies **automatisch genereren van sleutels** in de *verificatiesleutel* sectie.
4. Klik op *toepassen* knop.

![De vorm van een nieuw apparaat toe te voegen](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-new-device-form.png)

Wacht totdat de portal is voltooid de inrichting van het nieuwe apparaat.

![Een nieuw apparaat inrichten ](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device-provisioning.png)


Vervolgens wordt de configuratie van het nieuwe apparaat worden weergegeven.
Kopieer de **verbindingsreeks** gegenereerd.

![Apparaat-verbindingsreeks](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-new-device-connstring.png)


Deze verbindingsreeks wordt gebruikt in de volgende sectie.





## <a name="build-and-upload-the-device-code"></a>Maken en uploaden van de apparaatcode

Ga terug naar de Visual Studio Code: 

1. Gebruik `Ctrl+P` (Mac OS: `Cmd + P`) en type **config apparaatverbinding taak**.

  ![Kies uw Azure-abonnement en uw IoT-Hub](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion.png)

2. De terminal wordt u gevraagd of u wilt gebruiken verbindingsreeks van IoT-apparaat dat u wilt gebruiken. Selecteer *maken van nieuwe*, en plak deze nu.

  ![Plak de verbindingsreeks](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion-choose-iot-hub-press-button-A.png)

3. De terminal soms wordt u gevraagd om de configuratie activeren. Om dit te doen, houd A, en vervolgens push en release van de knop herstellen en vervolgens de loslaat A. Het scherm wordt weergegeven voor de DevKit-ID en 'Configuration'.

  ![Het scherm DevKit apparaat](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-screen.png)

  > [!NOTE]
  > De verbindingsreeks moet worden opgeslagen in het Klembord als u de laatste sectie van deze zelfstudie hebt gevolgd. Als u moet niet zo is, gaat u naar de Azure-portal en zoek naar de IoT-Hub van de resourcegroep voor externe controle. Daar ziet u de IoT-Hub verbonden apparaten en kopieer de verbindingsreeks van het apparaat.

  ![zoekt u de verbindingsreeks](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-connection-string-of-a-device.png)


Nu ziet u het nieuwe fysieke apparaat in de sectie met de Code van de VS 'Azure IoT Hub-apparaten':

![Let op het nieuwe IoT Hub-apparaat](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-new-iot-hub-device.png)

## <a name="test-the-project"></a>Het project testen

Wanneer de voorbeeld-app wordt uitgevoerd, verzendt DevKit sensorgegevens via Wi-Fi naar uw IoT-oplossing accelerators. Het resultaat wilt weergeven, de volgende stappen uit:

1. Ga naar uw IoT-oplossingsverbetering en klik **DASHBOARD**.

2. In de IoT-oplossing accelerator-console ziet u de status van de sensor DevKit. 

![Sensorgegevens in IoT-oplossing accelerators](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-dashboard.png)

Als u klikt u op de sensornaam van de (AZ3166) wordt een tabblad geopend aan de rechterkant van het dashboard, waar u de grafiek MX-Chip sensoren in realtime kunt zien.


## <a name="send-a-c2d-message"></a>Een C2D-bericht verzenden
Externe controle v2 kunt u externe methode op het apparaat aan te roepen.
De voorbeeldcode MX-Chip publiceert drie methoden die u in de sectie methode zien kunt wanneer de sensor is geselecteerd.

![Methoden MX-Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-methods.png)

U kunt de kleur van een van de MX-Chip LED's via de methode 'LedColor' wijzigen. Om dit te doen, schakel het selectievakje in van het apparaat en klik op de knop planning. 

![Methoden MX-Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-schedule.png)

Kies de methode ChangeColor aangeroepen in de vervolgkeuzelijst waar alle methoden die worden weergegeven, schrijft een naam en toepassen.

![Vervolgkeuzelijst MX-Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-change-color.png)

In enkele seconden uw fysieke MX-Chip de kleur van de RGB geleid moet wijzigen (onder van de knop)

![Leidde MX-Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-led.png)


## <a name="change-device-id"></a>Apparaat-ID wijzigen

U kunt de apparaat-ID van IoT-Hub wijzigen door de volgende [in deze handleiding](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).


## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of bereiken aan ons van de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een DevKit apparaat aansluit op uw IoT-oplossing accelerators en visualiseren van de sensorgegevens, vindt hier u de voorgestelde volgende stappen uit:

* [Overzicht van IoT-oplossing accelerators](https://docs.microsoft.com/azure/iot-suite/)
* [Sluit een apparaat MXChip IoT DevKit aan uw toepassing Microsoft IoT centrale](https://docs.microsoft.com/en-us/microsoft-iot-central/howto-connect-devkit)

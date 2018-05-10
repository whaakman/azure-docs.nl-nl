---
title: 'IoT DevKit naar cloud: IoT MXChip DevKit verbinding maken met Azure IoT Hub | Microsoft Docs'
description: Informatie over het verzenden van status van sensoren op IoT DevKit AZ3166 naar Azure IoT Remote Monitoring solution accelerator in deze zelfstudie.
services: iot-hub
documentationcenter: ''
author: liydu
manager: timlt
tags: ''
keywords: ''
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2018
ms.author: liydu
ms.openlocfilehash: d3de8241b42aff63faaf04477a58c5564407ac8b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-remote-monitoring-solution-accelerator"></a>Verbinding maken met MXChip IoT DevKit oplossingsverbetering Azure IoT externe controle

In deze zelfstudie leert u het uitvoeren van een voorbeeld-app op uw DevKit sensorgegevens verzenden naar uw oplossingsverbetering Azure IoT externe controle.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een Arduino alles in één mededelingenbord compatibel is met de randapparaten uitgebreide en sensoren. U kunt ontwikkelen via [Visual Studio Code-uitbreiding voor Arduino](https://aka.ms/arduino). En beschikt u over een groeiende [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) om u te begeleiden prototype Internet der dingen (IoT) oplossingen die van Microsoft Azure-services gebruikmaken.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) naar:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden

Een actief Azure-abonnement. Als u geen abonnement hebt, kunt u via een van deze twee manieren registreren:

* Activeren van een [gratis 30-daagse evaluatieversie Microsoft Azure-account](https://azure.microsoft.com/free/)
* Claim uw [Azure-tegoeden](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) als u MSDN of Visual Studio-abonnee bent

## <a name="create-an-azure-iot-remote-monitoring-solution-accelerator"></a>Maken van een oplossing voor externe controle van Azure IoT accelerator

1. Ga naar [Azure IoT-oplossing acclerators site](https://www.azureiotsuite.com/) en klik op **maken van een nieuwe oplossing**.
  ![Azure IoT-oplossing accelerator type selecteren](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-solution-types.png)
  > [!WARNING]
  > Dit voorbeeld maakt standaard een IoT-Hub S2 na het maken van een externe controle IoT oplossingsverbetering. Als deze iothub niet met het grote aantal apparaten gebruikt is, raden we u het downgraden van S2 naar S1 en verwijderen van de accelerator externe controle IoT-oplossing, zodat gerelateerde IoT Hub kan ook worden verwijderd, wanneer u deze niet meer nodig. 

2. Selecteer **externe controle**.

3. Voer de naam van een oplossing, selecteert u een abonnement en een regio en klik vervolgens op **oplossing maken**. De oplossing kan even duren om te worden ingericht.
  ![Oplossing maken](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution.png)

4. Nadat het inrichten is voltooid, klikt u op **starten**. Sommige gesimuleerde apparaten worden gemaakt voor de oplossing tijdens het inrichten. Klik op **apparaten** ze uitchecken. ![Dashboard](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution-created.png)
  ![Console](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-console.png)

5. Klik op **een apparaat TOEVOEGT**.

6. Klik op **nieuwe toevoegen** voor **aangepast apparaat**.
  ![Nieuwe apparaat toevoegen](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-add-new-device.png)

7. Klik op **laat mij mijn eigen apparaat-ID definiëren**, voer `AZ3166`, en klik vervolgens op **maken**.
  ![Apparaat met de ID maken](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-device-configuration.png)

8. Maak een notitie van **IoT Hub-hostnaam**, en klik op **gedaan**.

## <a name="open-the-remotemonitoring-sample"></a>Open het voorbeeld RemoteMonitoring

1. Verbreek de verbinding met de DevKit van uw computer, als deze is verbonden.

2. Start VS-Code.

3. De DevKit aansluiten op uw computer. VS Code uw DevKit detecteert automatisch en Hiermee opent u de volgende pagina's:
  * De introductiepagina DevKit.
  * Voorbeelden van Arduino: Praktische voorbeelden aan de slag met DevKit.

4. Vouw de linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **RemoteMonitoring**. Er wordt een nieuw venster van de VS Code geopend met een projectmap in het.
  > [!NOTE]
  > Als u per ongeluk het venster sluit, kunt u deze opnieuw openen. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) typt u de opdracht om palet te openen, **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

## <a name="provision-required-azure-services"></a>Vereist Azure services inrichten

In het oplossingsvenster kunt u uw taak uitvoeren via `Ctrl+P` (Mac OS: `Cmd+P`) door te voeren `task cloud-provision` in het tekstvak opgegeven:

In de terminal VS-Code in leert een interactieve opdrachtregel u de vereiste Azure-services inrichten:

![Azure-resources inrichten](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/provision.png)

## <a name="build-and-upload-the-device-code"></a>Maken en uploaden van de apparaatcode

1. Gebruik `Ctrl+P` (Mac OS: `Cmd + P`) en type **config apparaatverbinding taak**.

2. De terminal wordt u gevraagd of u wilt gebruiken van de verbindingsreeks die wordt opgehaald uit `task cloud-provision` stap. Er kan ook uw eigen apparaat verbindingsreeks door te klikken op 'Nieuw' ingevoerd

3. De terminal vraagt u om de configuratie activeren. Om dit te doen, houdt u A, en vervolgens push en release van de knop herstellen. Het scherm wordt weergegeven voor de DevKit-ID en 'Configuration'.
  ![Invoer-verbindingsreeks](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/config-device-connection.png)

4. Na `task config-device-connection` is voltooid, klikt u op `F1` tegenover codeopdrachten laden en selecteer `Arduino: Upload`, VS-Code wordt gestart controleren en werk de Arduino uploaden: ![controle en het uploaden van het schema Arduino](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/arduino-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

## <a name="test-the-project"></a>Het project testen

Wanneer de voorbeeld-app wordt uitgevoerd, verzendt DevKit sensorgegevens via Wi-Fi naar uw oplossingsverbetering Azure IoT externe controle. Het resultaat wilt weergeven, de volgende stappen uit:

1. Ga naar uw oplossingsverbetering Azure IoT externe controle en klik **DASHBOARD**.

2. Op de oplossing voor externe controle console ziet u de status van de sensor DevKit.

![Sensorgegevens in Azure IoT externe controle oplossingsverbetering](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/sensor-status.png)

## <a name="change-device-id"></a>Apparaat-ID wijzigen

U kunt de apparaat-ID van IoT-Hub wijzigen door de volgende [in deze handleiding](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/). En als u wilt wijzigen van de hardcoded **AZ3166** aangepast aan de apparaat-ID in de code. [Hier](https://github.com/Microsoft/devkit-sdk/blob/master/AZ3166/src/libraries/AzureIoT/examples/RemoteMonitoring/RemoteMonitoring.ino#L23) is de regel code die u kunt wijzigen.

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of bereiken aan ons van de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een DevKit apparaat aansluit op uw oplossingsverbetering Azure IoT externe controle en de sensorgegevens visualiseren, vindt hier u de voorgestelde volgende stappen uit:

* [Overzicht van Azure IoT-oplossing accelerators](https://docs.microsoft.com/azure/iot-suite/)
* [Sluit een apparaat MXChip IoT DevKit aan uw Azure IoT centrale toepassing](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)

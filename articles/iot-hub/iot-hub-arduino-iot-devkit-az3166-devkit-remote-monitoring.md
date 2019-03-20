---
title: IoT DevKit naar de cloud--IoT MXChip DevKit verbinden met Azure IoT Hub | Microsoft Docs
description: In deze zelfstudie leert u hoe u de status van sensoren op IoT DevKit AZ3166 verzendt naar de oplossingsverbetering voor externe controle van Azure IoT.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/02/2018
ms.author: liydu
ms.openlocfilehash: ae8dc263e08528c6e3b3bae8c779162c96d51f43
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58100782"
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-remote-monitoring-solution-accelerator"></a>MXChip IoT DevKit verbinden met Azure IoT Remote Monitoring solution accelerator

In deze zelfstudie leert u hoe u een voorbeeld-app op uw DevKit om sensorgegevens te verzenden naar uw externe controle van Azure IoT-oplossingsversnellers uitvoert.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een compatibele alles-in-een Arduino-bord met uitgebreide randapparatuur en sensoren. U kunt ontwikkelen via [Visual Studio Code-extensie voor Arduino](https://aka.ms/arduino). En wordt geleverd met een groeiend [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) om u te begeleiden prototype Internet of Things (IoT)-oplossingen die van Microsoft Azure-services profiteren.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) aan:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden

Een actief Azure-abonnement. Als u een hebt, kunt u registreren via een van deze twee methoden:

* Activeer een [gratis 30-daagse proefversie Microsoft Azure-account](https://azure.microsoft.com/free/)

* Claim uw [Azure-tegoed](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) bent u MSDN of Visual Studio-abonnee

## <a name="create-an-azure-iot-remote-monitoring-solution-accelerator"></a>Maken van een externe controle van Azure IoT-oplossingsversnellers

1. Ga naar [Azure IoT-oplossing accelerators site](https://www.azureiotsolutions.com/) en klikt u op **een nieuwe oplossing maken**.

   ![Azure IoT-oplossing accelerator-type selecteren](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-solution-types.png)

   > [!WARNING]
   > In dit voorbeeld maakt standaard een IoT-Hub S2 na het maken van een oplossingsverbetering voor externe controle IoT. Als deze IoT hub niet met een groot aantal apparaten gebruikt wordt, ten zeerste aangeraden het downgraden van S2 naar S1, en de oplossingsverbetering voor externe controle IoT verwijderen zodat de gerelateerde IoT-Hub kan ook worden verwijderd, wanneer u ze niet meer nodig hebt. 

2. Selecteer **bewaking op afstand**.

3. Voer de naam van een oplossing, selecteer een abonnement en een regio en klik vervolgens op **oplossing maken**. De oplossing kan even om te worden ingericht.
  
   ![Oplossing maken](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution.png)

4. Na het inrichten is voltooid, klikt u op **starten**. Sommige gesimuleerde apparaten worden tijdens het inrichtingsproces voor de oplossing gemaakt. Klik op **apparaten** en controleer of ze uit.

   ![Dashboard](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution-created.png)
  
   ![Console](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-console.png)

5. Klik op **toevoegen van een apparaat**.

6. Klik op **nieuwe toevoegen** voor **aangepast apparaat**.
  
   ![Een nieuw apparaat toevoegen](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-add-new-device.png)

7. Klik op **laat mij mijn eigen apparaat-ID definiëren**, voer `AZ3166`, en klik vervolgens op **maken**.
  
   ![Apparaat maken met de ID](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-device-configuration.png)

8. Maak een notitie van **IoT Hub-hostnaam**, en klikt u op **gedaan**.

## <a name="open-the-remotemonitoring-sample"></a>Open het voorbeeld RemoteMonitoring

1. De DevKit loskoppelt van de computer, als deze is verbonden.

2. Start VS Code.

3. De DevKit verbinden met uw computer. VS Code uw DevKit detecteert automatisch en opent de volgende pagina's:

   * De introductiepagina DevKit.
   * Arduino-voorbeelden: Praktische voorbeelden aan de slag met DevKit.

4. Vouw linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **RemoteMonitoring**. Een nieuwe VS Code-venster wordt het geopend met een projectmap daarin.

   > [!NOTE]
   > Als u het deelvenster sluiten, kunt u deze opnieuw openen. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) om te openen de command palette, typt u **Arduino**, en zoek en selecteer **Arduino: Voorbeelden**.

## <a name="provision-required-azure-services"></a>Vereiste Azure-services inrichten

In het venster van de oplossing, kunt u uw taak uitvoeren via `Ctrl+P` (Mac OS: `Cmd+P`) door in te voeren `task cloud-provision` in het tekstvak opgegeven.

In de terminal van VS Code en helpt een interactieve vanaf de opdrachtregel u de vereiste Azure-services voor het inrichten.

![Azure-resources inrichten](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/provision.png)

## <a name="build-and-upload-the-device-code"></a>Bouwen en de apparaatcode uploaden

1. Gebruik `Ctrl+P` (Mac OS: `Cmd + P`) en het type **config apparaatverbinding taak**.

2. De terminal wordt gevraagd of u gebruiken van een verbindingsreeks waarmee deze worden opgehaald wilt uit de `task cloud-provision` stap. U kunt ook de verbindingsreeks van uw eigen apparaat invoer door te klikken op 'Nieuw'

3. De terminal vraagt u om in te voeren van de configuratiemodus. Om dit te doen, houdt u ingedrukt A, en vervolgens push- en release van de knop opnieuw instellen. Het scherm wordt weergegeven de DevKit-ID en 'Configuration'.

   ![Invoer verbindingsreeks](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/config-device-connection.png)

4. Na `task config-device-connection` is voltooid, klikt u op `F1` laden van VS Code-opdrachten en selecteer `Arduino: Upload`. VS Code begint te controleren en de schets Arduino uploaden.
  
   ![Verificatie en uploaden van de schets Arduino](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/arduino-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

## <a name="test-the-project"></a>Het project testen

Wanneer de voorbeeld-app wordt uitgevoerd, verzendt DevKit sensorgegevens via Wi-Fi naar uw oplossingsverbetering voor externe controle van Azure IoT. Het resultaat wilt weergeven, de volgende stappen uit:

1. Ga naar uw externe controle van Azure IoT-oplossingsversnellers en klikt u op **DASHBOARD**.

2. Op de oplossing voor externe controle-console ziet u de status van uw DevKit sensor.

   ![Sensorgegevens in Azure IoT Remote Monitoring solution accelerator](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/sensor-status.png)

## <a name="change-device-id"></a>Apparaat-ID wijzigen

Als u wilt wijzigen van de vastgelegde **AZ3166** naar een aangepaste apparaat-ID in de code, wijzigt u de regel code weergegeven in de [externe bewaking voorbeeld](https://github.com/Microsoft/devkit-sdk/blob/master/AZ3166/src/libraries/AzureIoT/examples/RemoteMonitoring/RemoteMonitoring.ino#L23).

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [IoT developer kit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of contact opnemen met ons opnemen met behulp van de volgende kanalen:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een apparaat DevKit verbinden met uw externe controle van Azure IoT-oplossingsversnellers en de sensorgegevens visualiseren, vindt hier u de voorgestelde volgende stappen:

* [Overzicht oplossingsversnellers Azure IoT](https://docs.microsoft.com/azure/iot-suite/)

* [Een apparaat MXChip IoT DevKit verbinden met uw Azure IoT Central-toepassing](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)

* [IoT developer kit](https://microsoft.github.io/azure-iot-developer-kit/) 

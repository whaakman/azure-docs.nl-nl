---
title: Azure-apparaatdubbels gebruiken voor het beheren van MXChip IoT DevKit gebruiker LED | Microsoft Docs
description: In deze zelfstudie leert u hoe u DevKit statussen bewaken en beheren van de gebruiker LED met dubbele voor Azure IoT Hub-apparaten.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/04/2018
ms.author: liydu
ms.openlocfilehash: 6bc1255c5bbb9cf74c97b88600f34e7fcd90ae4f
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343144"
---
# <a name="mxchip-iot-devkit"></a>MXChip IoT DevKit

U kunt in dit voorbeeld gebruiken voor het bewaken van de MXChip IoT DevKit Wi-Fi-informatie en de sensor-status en de kleur van de gebruiker LED beheren met behulp van Azure IoT Hub-apparaatdubbels.

## <a name="what-you-learn"></a>Wat u leert

- Klik hier voor meer informatie over het bewaken van de sensor MXChip IoT DevKit Staten.

- Het gebruik van Azure-apparaatdubbels voor het beheren van de kleur van de de DevKit RGB-LED.

## <a name="what-you-need"></a>Wat u nodig hebt

- Uw ontwikkelomgeving instellen door de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).

- In uw terminalvenster GitBash (of een andere Git command-line interface), typ de volgende opdrachten:

   ```bash
   git clone https://github.com/DevKitExamples/DevKitState.git
   cd DevKitState
   code .
   ```

## <a name="provision-azure-services"></a>Azure-Services inrichten

1. Klik op de **taken** vervolgkeuzelijst in Visual Studio Code en selecteer **taak uitvoeren...**   -  **cloud inrichten**.

2. De voortgang wordt weergegeven onder de **TERMINAL** tabblad van de **Welkom** deelvenster.

3. Wanneer u wordt gevraagd de foutmelding *welk abonnement dat u wilt kiezen*, selecteer een abonnement.

4. Selecteer of kies een resourcegroep. 
 
   > [!NOTE]
   > Als u al een gratis IoT-Hub hebt, kunt u deze stap overslaan.

5. Wanneer u wordt gevraagd de foutmelding *welke IoT-hub wilt u kiest*selecteert of maakt u een IoT-Hub.

6. Ongeveer als in *functie-app: functie-appnaam: xxx*, wordt weergegeven. Noteer de naam van de functie-app; Deze wordt gebruikt in een latere stap.

7. Wacht tot de Azure Resource Manager sjabloonimplementatie is voltooid, die wordt aangegeven wanneer het bericht *sjabloonimplementatie van Resource Manager-: gedaan* wordt weergegeven.

## <a name="deploy-function-app"></a>Functie-App implementeren

1. Klik op de **taken** vervolgkeuzelijst in Visual Studio Code en selecteer **taak uitvoeren...**   -  **cloud implementeren**.

2. Wachten op functie-app-code proces is voltooid; uploaden het bericht *functie-app wordt geïmplementeerd: gedaan* wordt weergegeven.

## <a name="configure-iot-hub-device-connection-string-in-devkit"></a>IoT Hub apparaat-verbindingsreeks in DevKit configureren

1. Uw MXChip IoT DevKit verbinden met uw computer.

2. Klik op de **taken** vervolgkeuzelijst in Visual Studio Code en selecteer **taak uitvoeren...**   -  **config-apparaat-verbinding**

3. MXChip IoT DevKit, drukt u op en wacht knop **A**, drukt u op de **opnieuw** knoppen, en vervolgens release **A** om de configuratie van de modus DekKit.

4. Wachten op verbinding tekenreeks configuratieproces worden uitgevoerd.

## <a name="upload-arduino-code-to-devkit"></a>Arduino Code uploaden naar DevKit

Verbonden met uw MXChip IoT DevKit met uw computer:

1. Klik op de **taken** vervolgkeuzelijst in Visual Studio Code en selecteer **bouwen taak uitvoeren...** De schets Arduino worden gebundeld en geüpload naar de DevKit.

2. Wanneer het schema is geüpload, een *Build & uploaden schetsen: geslaagd* bericht wordt weergegeven.

## <a name="monitor-devkit-state-in-browser"></a>DevKit Monitorstatus in Browser

1. Open in een webbrowser, de `DevKitState\web\index.html` -bestand dat is gemaakt tijdens de [wat u nodig hebt](#whatyouneed) stap.

2. De volgende webpagina wordt weergegeven:![Geef de naam van de functie-app.](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state-function-app-name.png)

3. Voer de functie-appnaam die u eerder hebt genoteerd.

4. Klik op de **Connect** knop

5. Binnen enkele seconden, de pagina wordt vernieuwd en status van de DevKit Wi-Fi-verbinding en de status van elk van de ingebouwde sensoren worden weergegeven.

## <a name="control-the-devkits-user-led"></a>Gebruiker van de DevKit LED beheren

1. Klik op de gebruiker LED afbeelding op de webpagina-afbeelding.

2. Binnen enkele seconden, wordt het scherm wordt vernieuwd en ziet u de huidige kleurstatus van de gebruiker LED.

3. Wijzig de kleurwaarde van de RGB-LED beheren door te klikken op verschillende locaties op de besturingselementen van de RGB-schuifregelaar.

## <a name="example-operation"></a>Voorbeeld van de bewerking

![Procedure voor het testen van voorbeeld](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state.gif)

> [!NOTE]
> U kunt zien onbewerkte gegevens van het dubbele apparaat in Azure portal: IoT Hub -\> IoT-apparaten -\> *\<uw apparaat\>*  - \> Apparaatdubbel.

## <a name="next-steps"></a>Volgende stappen

Hebt u geleerd hoe u:
- Een apparaat MXChip IoT DevKit verbinden met uw oplossingsverbetering voor externe controle van Azure IoT.
- Gebruik de functie van Azure IoT device twins detecteren en de kleur van de de DevKit RGB-LED beheren.

Hier volgen de voorgestelde volgende stappen:

* [Overzicht van Azure accelerator voor externe controle IoT-oplossing](https://docs.microsoft.com/azure/iot-suite/)
* [Een apparaat MXChip IoT DevKit verbinden met uw Azure IoT Central-toepassing](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)

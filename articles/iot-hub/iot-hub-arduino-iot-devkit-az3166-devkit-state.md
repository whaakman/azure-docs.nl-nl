---
title: Gebruik Azure apparaat horende voor het beheren van MXChip IoT DevKit gebruiker LED | Microsoft Docs
description: Informatie over het DevKit statussen bewaken en beheren van de gebruiker LED met Azure IoT Hub apparaat horende in deze zelfstudie.
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
ms.date: 04/04/2018
ms.author: liydu
ms.openlocfilehash: ac013cf234fdc7249d4d71992435adcc27fcc64e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="mxchip-iot-devkit"></a>MXChip IoT DevKit

U kunt in dit voorbeeld gebruiken voor het bewaken van de MXChip IoT DevKit Wi-Fi-informatie en sensor status te bepalen en de kleur van de gebruiker LED met behulp van Azure IoT Hub apparaat horende.

## <a name="what-you-learn"></a>Wat u leert

- Het controleren van de sensor MXChip IoT DevKit statussen.
- Het gebruik van Azure apparaat horende de kleur van de DevKit RGB-LED.

## <a name="what-you-need"></a>Wat u nodig hebt

- Uw ontwikkelomgeving instellen door de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
- Typ de volgende opdrachten vanuit uw terminalvenster GitBash (of andere opdrachtregelinterface Git):
    - `git clone https://github.com/DevKitExamples/DevKitState.git`
    - `cd DevKitState`
    - `code .`

## <a name="provision-azure-services"></a>Azure-Services inrichten

1. Klik op de **taken** vervolgkeuzelijst in Visual Studio Code en selecteer **taak uitvoeren...**   -  **cloud-provision**.
2. Uw voortgang wordt weergegeven onder de **TERMINAL** tabblad van de **Welkom** Configuratiescherm.
3. Wanneer het bericht *welk abonnement dat u wilt kiezen*, selecteer een abonnement.
4. Selecteer of kies een resourcegroep. 
 
    > [!NOTE]
    > Als u al een gratis IoT-Hub, wordt deze stap overgeslagen.

5. Wanneer het bericht *welke IoT-hub wilt u Kies*selecteert of maakt u een IoT-Hub.
6. Iets dergelijks naar *functie-app: de functie app-naam: xxx*, wordt weergegeven. Noteer de naam van de functie-app; Deze wordt gebruikt in een later stadium.
7. Wacht tot de Azure Resource Manager sjabloonimplementatie voltooid die wordt aangegeven wanneer het bericht *sjabloonimplementatie van Resource Manager-: gedaan* wordt weergegeven.

## <a name="deploy-function-app"></a>Functie-App implementeren

1. Klik op de **taken** vervolgkeuzelijst in Visual Studio Code en selecteer **taak uitvoeren...**   -  **cloud implementeren**.
2. Wacht tot de functie app-code voor het uploaden van proces is voltooid; het bericht *functie-app wordt geïmplementeerd: gedaan* wordt weergegeven.

## <a name="configure-iot-hub-device-connection-string-in-devkit"></a>IoT Hub apparaat-verbindingsreeks in DevKit configureren

1. Verbinding maken met uw IoT MXChip DevKit op uw computer.
2. Klik op de **taken** vervolgkeuzelijst in Visual Studio Code en selecteer **taak uitvoeren...**   -  **config apparaatverbinding**
3. Op de MXChip IoT DevKit, drukt u op en wacht knop **A**, drukt u op de **opnieuw instellen** knop en vervolgens op de knop release **A** waarmee de DekKit configuratiemodus invoeren.
4. Wachten op verbinding tekenreeks proces voor de configuratie te voltooien.

## <a name="upload-arduino-code-to-devkit"></a>Arduino Code uploaden naar DevKit

Verbonden met uw IoT MXChip DevKit op uw computer:
1. Klik op de **taken** vervolgkeuzelijst in Visual Studio Code en selecteer **bouwen taak uitvoeren...** Het schema Arduino gecompileerd en geüpload naar de DevKit.
2. Wanneer het schema is geüpload, een *Build & werk uploaden: geslaagd* bericht wordt weergegeven.

## <a name="monitor-devkit-state-in-browser"></a>Status van de monitor DevKit in Browser

1. Open in een webbrowser, de `DevKitState\web\index.html` -bestand dat is gemaakt tijdens de [wat u moet](#whatyouneed) stap.
2. De volgende webpagina wordt weergegeven:![Geef de naam van de functie-app.](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state-function-app-name.png)
1. Voer de naam van de functie-app die u eerder hebt genoteerd.
2. Klik op de **Connect** knop
3. Binnen enkele seconden, de pagina wordt vernieuwd en de DevKit Wi-Fi-verbindingsstatus en de status van elk van de ingebouwde sensoren worden weergegeven.

## <a name="control-the-devkits-user-led"></a>De DevKit gebruiker LED beheren

1. Klik op de afbeelding van de gebruiker LED op de webpagina-afbeelding.
2. Binnen enkele seconden, wordt het scherm wordt vernieuwd en geeft de huidige kleurstatus van de gebruiker LED.
3. Wijzig de kleurwaarde van de RGB-LED door te klikken op verschillende locaties op de schuifregelaar RGB-besturingselementen.

## <a name="example-operation"></a>Voorbeeld van de bewerking

![Test voorbeeldprocedure](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state.gif)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe:
- Een apparaat MXChip IoT DevKit aansluiten op uw oplossingsverbetering Azure IoT externe controle.
- Gebruik de functie van Azure IoT-apparaat horende detecteren en beheren van de kleur van de DevKit RGB-LED.

Hier volgen de mogelijke volgende stappen:

* [Overzicht van Azure accelerator voor externe controle IoT-oplossing](https://docs.microsoft.com/azure/iot-suite/)
* [Sluit een apparaat MXChip IoT DevKit aan uw toepassing Microsoft IoT centrale](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)

---
title: Visual Studio Code gebruiken om op te sporen een C#-module met Azure IoT rand | Microsoft Docs
description: Fouten opsporen in een C#-module met Azure IoT rand in de Visual Studio Code.
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: eecbc10b5e030f67382d72a7b702e441a2e5492c
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="use-visual-studio-code-to-debug-a-c-module-with-azure-iot-edge"></a>Gebruik Visual Studio Code fouten opsporen in een C#-module met Azure IoT rand
In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio Code](https://code.visualstudio.com/) als de belangrijkste ontwikkelprogramma fouten opsporen in uw Azure-IoT-Edge-modules.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT-randapparaat kan een andere fysieke apparaat, of kunt u uw IoT-randapparaat simuleren op uw ontwikkelcomputer.

> [!NOTE]
> U kunt alleen fouten opsporen in C#-module in linux amd64-containers.

Voer de stappen in voordat u de instructies in dit artikel, [ontwikkelen van een rand van de IoT-oplossing met meerdere modules in Visual Studio Code](tutorial-multiple-modules-in-vscode.md). Daarna moet u de volgende items gereed hebt:
- Een lokale Docker-register uitgevoerd op uw ontwikkelcomputer. Aangeraden wordt een lokale Docker-register voor prototype- en testdoeleinden gebruiken. U kunt in het register van de container bijwerken de `module.json` bestand in de modulemap voor elke.
- Een rand van de IoT-oplossing project-werkruimte met een C#-module-submap van deze.
- De `Program.cs` bestand met de meest recente code van de module.
- Een Edge-runtime is uitgevoerd op uw ontwikkelcomputer.

## <a name="build-your-iot-edge-c-module-for-debugging"></a>Uw IoT rand C#-module voor het opsporen van maken
1. Voor foutopsporing starten, moet u de **Dockerfile.amd64.debug** naar uw docker-installatiekopie bouwen en implementeren van uw oplossing opnieuw. Navigeer in VS-Code Verkenner naar `deployment.template.json` bestand. De afbeeldings-URL van de functie bijwerken door toe te voegen een `.debug` in het einde.

2. Uw oplossing opnieuw worden opgebouwd. Typ in de VS Code opdracht palet en voer de opdracht **rand: bouwen IoT oplossing**.

3. In Azure IoT Hub-apparaten explorer met de rechtermuisknop op een apparaat-ID van IoT-rand en selecteer vervolgens **implementatie creëert voor randapparaat**. Selecteer de `deployment.json` onder `config` map. Vervolgens ziet u dat de implementatie is gemaakt met een implementatie-ID in VS-Code wordt geïntegreerd terminal.

> [!NOTE]
> U kunt de status van de container in de VS Code Docker Verkenner of voer controleren de `docker images` opdracht in de terminal.

## <a name="start-debugging-c-module-in-vs-code"></a>C#-module in de VS Code foutopsporing starten
1. VS Code houdt foutopsporing configuratie-informatie in een `launch.json` bestand zich in een `.vscode` map in uw werkruimte. Dit `launch.json` bestand is gegenereerd tijdens het maken van een nieuwe rand van de IoT-oplossing. En deze worden bijgewerkt wanneer die u een nieuwe module die ondersteuning bieden voor foutopsporing toevoegt. Navigeer naar de weergave voor foutopsporing en selecteer het bijbehorende configuratiebestand voor foutopsporing.
    ![Selecteer Foutopsporing configuratie](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navigeer naar `program.cs`. Een onderbrekingspunt in dit bestand toevoegen.

3. Klik op knop foutopsporing starten of druk op **F5**, en selecteert u het proces om aan te koppelen.

4. VS Code fouten opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 

> [!NOTE]
> Het vorige voorbeeld laat zien hoe .NET Core IoT Edge-modules voor containers voor foutopsporing. Deze gebaseerd op de foutopsporingsversie van de `Dockerfile.debug`, waaronder VSDBG (het opdrachtregelprogramma .NET Core-foutopsporingsprogramma) in uw installatiekopie container tijdens het opbouwen van het. Nadat u foutopsporing van uw C#-modules, raden we u rechtstreeks gebruiken of aanpassen `Dockerfile` zonder VSDBG voor modules die gereed is voor productie IoT rand.

## <a name="next-steps"></a>Volgende stappen

[Visual Studio Code gebruiken om op te sporen Azure werkt in combinatie met Azure IoT rand](how-to-vscode-debug-azure-function.md)


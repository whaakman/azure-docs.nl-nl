---
title: Fouten opsporen in modules van de functies voor Azure IoT rand | Microsoft Docs
description: Gebruik Visual Studio Code fouten opsporen in C# Azure werkt in combinatie met Azure IoT rand
author: shizn
manager: ''
ms.author: xshi
ms.date: 03/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 52c5afbd37459267181cfc1d8d8d2620aff37913
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34630779"
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Visual Studio Code gebruiken om op te sporen Azure werkt in combinatie met Azure IoT rand

In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio (VS) Code](https://code.visualstudio.com/) fouten opsporen in uw Azure-functies op IoT rand.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT-randapparaat kan een andere fysieke apparaat of kunt u uw IoT-randapparaat simuleren op uw ontwikkelcomputer.

> [!NOTE]
> U kunt alleen fouten opsporen in C#-functies in linux amd64 containers.

Voer de stappen in voordat u de instructies in dit artikel, [ontwikkelen van een rand van de IoT-oplossing met meerdere modules in Visual Studio Code](tutorial-multiple-modules-in-vscode.md). Daarna moet u de volgende items gereed hebt:
- Een lokale Docker-register uitgevoerd op uw ontwikkelcomputer. Aangeraden wordt een lokale Docker-register voor prototype- en testdoeleinden gebruiken. U kunt in het register van de container bijwerken de `module.json` bestand in de modulemap voor elke.
- Een rand van de IoT-oplossing project-werkruimte met een submap van de module Azure-functie in het.
- De `run.csx` bestand met de functiecode.
- Een Edge-runtime is uitgevoerd op uw ontwikkelcomputer.

## <a name="build-your-iot-edge-function-module-for-debugging-purpose"></a>Uw IoT rand functie-module voor het opsporen van doel maken
1. Voor foutopsporing starten, moet u gebruiken **Dockerfile.amd64.debug** naar uw docker-installatiekopie bouwen en implementeren van uw oplossing opnieuw. Navigeer in VS-Code Verkenner naar `deployment.template.json` bestand. De afbeeldings-URL van de functie bijwerken door toe te voegen een `.debug` in het einde.

    ![Foutopsporing installatiekopie maken](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Uw oplossing opnieuw worden opgebouwd. Typ in de VS Code opdracht palet en voer de opdracht **rand: bouwen IoT oplossing**.
3. In Azure IoT Hub-apparaten explorer met de rechtermuisknop op een apparaat-ID van IoT-rand en selecteer vervolgens **implementatie creëert voor randapparaat**. Selecteer de `deployment.json` bestand onder de `config` map. Vervolgens ziet u dat de implementatie is gemaakt met een implementatie-ID in VS-Code wordt geïntegreerd terminal.

U kunt de status van de container in de VS Code Docker explorer of door te voeren controleren de `docker images` opdracht in de terminal.

## <a name="start-debugging-c-function-in-vs-code"></a>C#-functie in VS-Code foutopsporing starten
1. VS Code houdt foutopsporing configuratie-informatie in een `launch.json` bestand zich in een `.vscode` map in uw werkruimte. Dit `launch.json` bestand is gegenereerd tijdens het maken van een nieuwe rand van de IoT-oplossing. Telkens wanneer u een nieuwe module die ondersteuning biedt voor foutopsporing toevoegt worden bijgewerkt. Navigeer naar de weergave voor foutopsporing en selecteer het bijbehorende configuratiebestand voor foutopsporing.
    ![Selecteer Foutopsporing configuratie](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navigeer naar `run.csx`. Een onderbrekingspunt in de functie toevoegen.
3. Klik op de **foutopsporing starten** of drukt u op **F5**, en selecteert u het proces om aan te koppelen.
4. VS Code fouten opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 


> [!NOTE]
> Hierboven voorbeeld ziet u hoe u foutopsporing .net Core IoT Edge-functie op containers. Deze gebaseerd op de foutopsporingsversie van de `Dockerfile.amd64.debug`, waaronder VSDBG (het opdrachtregelprogramma .NET Core-foutopsporingsprogramma) in uw installatiekopie container tijdens het opbouwen van het. We raden u rechtstreeks gebruiken of aanpassen aan de `Dockerfile` zonder VSDBG voor gereed is voor productie IoT rand functie als u klaar bent met foutopsporing van uw C#-functie.

## <a name="next-steps"></a>Volgende stappen


[Gebruik Visual Studio Code fouten opsporen in een C#-module met Azure IoT rand](how-to-vscode-debug-csharp-module.md)


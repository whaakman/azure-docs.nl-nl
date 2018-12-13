---
title: Fouten opsporen in meerdere modules in Visual Studio Code - Azure IoT Edge | Microsoft Docs
description: Visual Studio Code gebruiken om op te sporen meerdere modules met Azure IoT Edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 0c421eb1532536a3d11013073f0474f5ac37311b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100432"
---
# <a name="use-visual-studio-code-to-debug-multiple-modules-with-azure-iot-edge"></a>Visual Studio Code gebruiken om op te sporen meerdere modules met Azure IoT Edge
In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio (VS) Code](https://code.visualstudio.com/) fouten opsporen in meerdere modules op IoT Edge.

## <a name="prerequisites"></a>Vereisten
Voltooi de zelfstudie [een IoT Edge-oplossing met meerdere modules in Visual Studio Code ontwikkelen](tutorial-multiple-modules-in-vscode.md) en zorg ervoor dat u hebt ten minste twee modules die worden uitgevoerd op uw IoT Edge-apparaat.

## <a name="multi-target-and-remote-debugging-in-vs-code"></a>Meerdere doel en op afstand opsporen in VS Code
Met VS Code en Azure IoT Edge-extensie kunt u het proces van de module in een container, koppelen of de container wordt uitgevoerd op uw ontwikkelcomputer of in een externe fysieke IoT Edge-apparaat. Debug meerdere modules die worden uitgevoerd in containers is meer dan één proces in afzonderlijke containers daadwerkelijk koppelen. VS Code [meerdere doel foutopsporing](https://code.visualstudio.com/docs/editor/debugging#_multitarget-debugging) kunnen worden gebruikt bij het opsporen van fouten in meerdere modules.

   > [!TIP]
   > Als de module-container wordt uitgevoerd in een externe fysieke IoT Edge-apparaat, moet u mogelijk tot het instellen van [Docker Machine](https://docs.docker.com/machine/overview/) zodat de Docker-engine op uw ontwikkelcomputer contact met de externe Docker-hosts opnemen kunt.

### <a name="build-your-iot-edge-modules-for-debugging-purpose"></a>Uw IoT-Edge-modules build voor foutopsporing doel
1. Als u wilt meerdere module foutopsporing starten, moet u gebruiken **Dockerfile.amd64.debug** opnieuw opbouwen van uw docker-installatiekopieën en opnieuw implementeren van uw Edge-oplossing. Navigeer in de VS Code explorer naar `deployment.template.json` bestand. De URL van uw installatiekopie bijwerken door toe te voegen een `.debug` in het einde. U moet twee module-installatiekopieën met `.debug` ten minste. Als u bezig bent de oplossing uit de vorige zelfstudie, hebt u een C# functies module en een C# module. Deze twee afbeelding-URL's bijwerken door toe te voegen een `.debug` in het einde en sla dit bestand. 
2. Uw oplossing opnieuw maakt. Typ in het opdrachtenpalet van VS Code, en voer de opdracht **Azure IoT Edge: Build IoT Edge-oplossing**.
3. In de Verkenner van Azure IoT Hub-apparaten met de rechtermuisknop op een IoT Edge-apparaat-ID en selecteer vervolgens **implementatie voor Edge-apparaat maken**. Selecteer de `deployment.json` bestand onder de `config` map. Vervolgens ziet u dat de implementatie is gemaakt met een implementatie-ID in VS Code geïntegreerde terminal.

U kunt de containerstatus van uw in Docker van VS Code explorer of door te voeren controleren de `docker ps` opdracht in de terminal.

### <a name="start-debugging-c-function-in-vs-code"></a>Foutopsporing starten C# functie in VS Code
1. VS Code houdt opsporen van fouten in configuratie-informatie in een `launch.json` bestand zich bevindt in een `.vscode` map in uw werkruimte. Dit `launch.json` bestand is gegenereerd tijdens het maken van een nieuwe IoT Edge-oplossing. Bijgewerkt telkens wanneer u een nieuwe module die ondersteuning biedt voor foutopsporing toevoegen. Navigeer naar de weergave voor foutopsporing en selecteer het bijbehorende configuratiebestand voor de foutopsporing voor C# functies module foutopsporing op afstand.
2. Navigeer naar `run.csx`. Voeg een onderbrekingspunt toe in de functie.
3. Klik op de **Start Debugging** of drukt u op **F5**, en selecteert u het proces om aan te koppelen.
4. VS-Code opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 

### <a name="start-debugging-c-module-at-the-same-time-in-vs-code"></a>Foutopsporing starten C# module op hetzelfde moment in VS Code
1. Typ in het opdrachtenpalet van VS Code, en voer de opdracht 'Werkruimte: dubbele werkruimte in nieuw venster'. Een nieuwe VS Code-venster wordt gestart met dezelfde werkruimte.
2. Navigeer naar de weergave voor foutopsporing en selecteer het bijbehorende configuratiebestand voor de foutopsporing voor C# foutopsporing op afstand module.
3. Navigeer naar `program.cs`. Voeg een onderbrekingspunt in de C# module.
4. Klik op de **Start Debugging** of drukt u op **F5**, en selecteert u het proces om aan te koppelen.
5. VS-Code opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 

### <a name="see-variables-in-multiple-debugging-windows"></a>Zie variabelen in meerdere vensters voor foutopsporing
1. Nu hebt u ten minste twee foutopsporingssessie uitvoeren in twee VS Code-venster. Er moet een van de onderbrekingspunt bereikt.
2. Druk op `F10` of klik op de knop stap Over op de **werkbalk Foutopsporing**.
3. Het onderbrekingspunt in een ander VS Code-venster moet worden bereikt. 
4. Doorgaan met bovenstaande twee stappen ziet u variabelen uit bevat meerdere modules in meerdere VS-Code opsporen van fouten in windows.

> [!NOTE]
> Bovenstaande voorbeeld ziet u hoe opsporen van fouten in meerdere modules met Azure IoT Edge. Deze gebaseerd op de foutopsporingsversie van de `Dockerfile.amd64.debug`, waaronder VSDBG (de .NET Core voor foutopsporing) in uw containerinstallatiekopie tijdens het maken van deze. We raden u rechtstreeks gebruiken of aanpassen aan de `Dockerfile` zonder VSDBG voor gereed is voor productie IoT Edge-functie als u klaar bent met het opsporen van fouten in uw C# functie.

## <a name="next-steps"></a>Volgende stappen

Zodra u uw module die is gebouwd hebt, informatie over hoe u [implementeren Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md) 0

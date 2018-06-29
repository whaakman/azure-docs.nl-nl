---
title: Fouten opsporen in meerdere modules voor Azure IoT rand in de VS-Code | Microsoft Docs
description: Visual Studio Code gebruiken om op te sporen meerdere modules met Azure IoT rand
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 38c66129ac8244d18b0f94c1485c785015e29ab4
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049588"
---
# <a name="use-visual-studio-code-to-debug-multiple-modules-with-azure-iot-edge"></a>Visual Studio Code gebruiken om op te sporen meerdere modules met Azure IoT rand
In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio (VS) Code](https://code.visualstudio.com/) fouten opsporen in meerdere modules op IoT rand.

## <a name="prerequisites"></a>Vereisten
Voltooi de zelfstudie [ontwikkelen van een rand van de IoT-oplossing met meerdere modules in Visual Studio Code](tutorial-multiple-modules-in-vscode.md) en zorg ervoor dat er ten minste twee modules dat wordt uitgevoerd op uw apparaat IoT rand.

## <a name="multi-target-and-remote-debugging-in-vs-code"></a>Meerdere doel en externe foutopsporing in VS-Code
Met de extensie tegenover Code en Azure IoT Edge, kunt u het proces van de module in een container koppelen of de container op uw ontwikkelcomputer of in een externe fysieke IoT randapparaat wordt uitgevoerd. Foutopsporing mutiple modules dat wordt uitgevoerd in containers is meer dan een proces in afzonderlijke containers daadwerkelijk koppelen. VS-Code [meerdere doel foutopsporing](https://code.visualstudio.com/docs/editor/debugging#_multitarget-debugging) kan worden gebruikt als u fouten opspoort mutiple modules.

   > [!TIP]
   > Als uw module-container in een externe fysieke IoT randapparaat wordt uitgevoerd, moet u mogelijk Setup [Docker-Machine](https://docs.docker.com/machine/overview/) zodat de Docker-engine op uw ontwikkelcomputer contact met de externe Docker-hosts opnemen kunt.

### <a name="build-your-iot-edge-modules-for-debugging-purpose"></a>Uw IoT-rand modules build voor foutopsporing doel
1. Als u wilt meerdere modules foutopsporing starten, moet u gebruiken **Dockerfile.amd64.debug** om te bouwen van uw docker-installatiekopieën en implementeer uw oplossing opnieuw. Navigeer in VS-Code Verkenner naar `deployment.template.json` bestand. De URL van uw installatiekopie bijwerken door het toevoegen van een `.debug` in het einde. U moet twee module afbeeldingen met `.debug` ten minste. Als u bezig bent met de oplossing van vorige zelfstudie, hebt u een module C#-functies en een C#-module. Deze twee afbeelding-URL's bijwerken door het toevoegen van een `.debug` in het einde en sla dit bestand. 
2. Uw oplossing opnieuw worden opgebouwd. Typ in de VS Code opdracht palet en voer de opdracht **Azure IoT rand: bouwen IoT oplossing**.
3. In Azure IoT Hub-apparaten explorer met de rechtermuisknop op een apparaat-ID van IoT-rand en selecteer vervolgens **implementatie creëert voor randapparaat**. Selecteer de `deployment.json` bestand onder de `config` map. Vervolgens ziet u dat de implementatie is gemaakt met een implementatie-ID in VS-Code wordt geïntegreerd terminal.

U kunt de status van de container in de VS Code Docker explorer of door te voeren controleren de `docker ps` opdracht in de terminal.

### <a name="start-debugging-c-function-in-vs-code"></a>C#-functie in VS-Code foutopsporing starten
1. VS Code houdt foutopsporing configuratie-informatie in een `launch.json` bestand zich in een `.vscode` map in uw werkruimte. Dit `launch.json` bestand is gegenereerd tijdens het maken van een nieuwe rand van de IoT-oplossing. Telkens wanneer u een nieuwe module die ondersteuning biedt voor foutopsporing toevoegt worden bijgewerkt. Navigeer naar de weergave voor foutopsporing en selecteer het bijbehorende configuratiebestand voor de foutopsporing voor C# functies module foutopsporing op afstand.
2. Navigeer naar `run.csx`. Een onderbrekingspunt in de functie toevoegen.
3. Klik op de **foutopsporing starten** of drukt u op **F5**, en selecteert u het proces om aan te koppelen.
4. VS Code fouten opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 

### <a name="start-debugging-c-module-at-the-same-time-in-vs-code"></a>C#-module op hetzelfde moment in de VS Code foutopsporing starten
1. Typ in VS-Code opdracht palet en voer de opdracht 'Werkruimte: dubbele werkruimte in nieuw venster'. Een nieuw venster van de VS Code begint met dezelfde werkruimte.
2. Navigeer naar de weergave voor foutopsporing en selecteer het bijbehorende configuratiebestand voor de foutopsporing voor C# module foutopsporing op afstand.
3. Navigeer naar `program.cs`. Een onderbrekingspunt in de C#-module toevoegen.
4. Klik op de **foutopsporing starten** of drukt u op **F5**, en selecteert u het proces om aan te koppelen.
5. VS Code fouten opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 

### <a name="see-variables-in-multiple-debugging-windows"></a>Zie variabelen in meerdere vensters voor foutopsporing
1. U hebt nu ten minste twee foutopsporingssessie in twee tegenover Code-venster. Er moet een van de onderbrekingspunt bereikt.
2. Druk op `F10` of klik op de knop stap Over in de **werkbalk Foutopsporing**.
3. Het onderbrekingspunt in een ander venster van de VS-Code moet worden bereikt. 
4. Blijven hierboven twee stappen ziet u variabelen van bevat meerdere modules in meerdere tegenover Code foutopsporing van windows.

> [!NOTE]
> Hierboven voorbeeld ziet u hoe aan meerdere modules foutopsporing met Azure IoT rand. Deze gebaseerd op de foutopsporingsversie van de `Dockerfile.amd64.debug`, waaronder VSDBG (het opdrachtregelprogramma .NET Core-foutopsporingsprogramma) in uw installatiekopie container tijdens het opbouwen van het. We raden u rechtstreeks gebruiken of aanpassen aan de `Dockerfile` zonder VSDBG voor gereed is voor productie IoT rand functie als u klaar bent met foutopsporing van uw C#-functie.

## <a name="next-steps"></a>Volgende stappen

Zodra u uw module die is gebouwd hebt, informatie over hoe [modules Azure IoT-rand implementeert vanuit Visual Studio Code](how-to-deploy-modules-vscode.md) 0

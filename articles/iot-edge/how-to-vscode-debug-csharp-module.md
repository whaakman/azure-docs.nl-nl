---
title: Visual Studio Code gebruiken om op te sporen een C#-module met Azure IoT rand | Microsoft Docs
description: Fouten opsporen in een C#-module met Azure IoT rand in de VS-Code
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 1ab67cd8aaf59cde3157fcb877ce13f10cb432bb
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="use-visual-studio-code-to-debug-c-module-with-azure-iot-edge"></a>Visual Studio Code naar foutopsporing C#-module gebruiken met Azure IoT rand
In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio Code](https://code.visualstudio.com/) als de belangrijkste ontwikkelprogramma fouten opsporen in uw IoT-Edge-modules.

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT-randapparaat kan een andere fysieke apparaat of kunt u uw IoT-randapparaat simuleren op uw ontwikkelcomputer.

Zorg ervoor dat u hebt voltooid volgende zelfstudies voordat u deze richtlijnen.
- [Visual Studio Code gebruiken voor het ontwikkelen van C#-module met Azure IoT rand](how-to-vscode-develop-csharp-module.md)

Na het voltooien de voorgaande zelfstudie, moet u de volgende items gereed is, hebben
- Een lokale Docker-register uitgevoerd op uw ontwikkelcomputer. Aangeraden wordt een lokale Docker-register voor prototype- en testdoeleinden gebruiken.
- De `Program.cs` bestand met de meest recente filter module code.
- Een bijgewerkte `deployment.json` -bestand voor uw sensor-module en filtermodule.
- Een Edge-runtime is uitgevoerd op uw ontwikkelcomputer.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Uw IoT-Edge-module voor het opsporen van doel maken
1. Voor foutopsporing starten, moet u de **dockerfile.debug** naar uw docker-installatiekopie bouwen en implementeren van uw oplossing opnieuw. Klik op de Docker-map om deze te openen in VS-Code explorer. Klik vervolgens op de `linux-x64` map met de rechtermuisknop op de **Dockerfile.debug**, en klik op **bouwen IoT rand module Docker installatiekopie**.

    ![Foutopsporing installatiekopie maken](./media/how-to-debug-csharp-module/build-debug-image.png)

3. In de **map selecteren** venster ofwel om te zoeken of invoeren `./bin/Debug/netcoreapp2.0/publish`. Klik op **map selecteren als EXE_DIR**.
4. Typ in het pop-tekst boven aan het venster tegenover Code, naam van de installatiekopie. Bijvoorbeeld: `<your container registry address>/filtermodule:latest`. Als u naar het lokale register implementeert, moet dit worden `localhost:5000/filtermodule:latest`.
5. De installatiekopie naar uw opslagplaats Docker forceren. Gebruik de **rand: Push IoT rand module Docker installatiekopie** opdracht en de afbeeldings-URL in het pop-tekst boven aan het venster VS-Code invoeren. Gebruik dezelfde afbeeldings-URL die u in bovenstaande stap gebruikt.
6. U kunt opnieuw gebruiken de `deployment.json` te implementeren. Typ in de opdracht palet, en selecteer **rand: opnieuw opstarten rand** ophalen van de module expressiefilter uitgevoerd met de foutopsporingsversie.

## <a name="start-debugging-in-vs-code"></a>Foutopsporing in VS-Code starten
1. Ga naar tegenover Code foutopsporingsvenster. Druk op **F5** en selecteer **IoT Edge(.Net Core)**

    ![Druk op F5](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. In `launch.json`, gaat u naar **fouten opsporen in IoT rand aangepaste Module (.NET Core)** sectie en vul de `<container_name>`onder `pipeArgs`. Deze moet `filtermodule` in deze zelfstudie.

    ![PipeArgs wijzigen](./media/how-to-debug-csharp-module/f5-debug-option.png)

3. Ga naar Program.cs. Toevoegen van een onderbrekingspunt in de `method static async Task<MessageResponse> FilterModule(Message message, object userContext)`.
4. Druk op **F5** opnieuw. En selecteer het proces om aan te koppelen. In deze zelfstudie maakt u de procesnaam moet`FilterModule.dll`

    ![Attach-proces](./media/how-to-debug-csharp-module/attach-process.png)

5. In Code fouten opsporen in VS-venster ziet u de variabelen in het linkerdeelvenster. 

> [!NOTE]
> Hierboven voorbeeld ziet u hoe u foutopsporing .net Core IoT Edge-modules voor containers. Deze gebaseerd op de foutopsporingsversie van de `Dockerfile.debug`, waaronder VSDBG (het opdrachtregelprogramma .NET Core-foutopsporingsprogramma) in uw installatiekopie container tijdens het opbouwen van het. We raden u rechtstreeks gebruiken of aanpassen aan de `Dockerfile` zonder VSDBG voor gereed is voor productie IoT rand modules nadat u foutopsporing van uw C#-modules.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie een IoT-Edge-module gemaakt en geïmplementeerd voor foutopsporing doel en fouten kunt opsporen in VS-Code is gestart. U kunt door te gaan naar een van de volgende zelfstudies voor meer informatie over andere scenario's bij het ontwikkelen van Azure IoT rand in de VS-Code. 

> [!div class="nextstepaction"]
> [Ontwikkelen en implementeren van C#-module in VS-Code](how-to-vscode-develop-csharp-module.md)

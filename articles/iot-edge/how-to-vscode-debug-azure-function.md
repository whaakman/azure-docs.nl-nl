---
title: Visual Studio Code gebruiken om op te sporen Azure werkt in combinatie met Azure IoT rand | Microsoft Docs
description: Fouten opsporen in C# Azure werkt in combinatie met Azure IoT rand in de VS-Code
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/20/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: db86a08a19e97f8f415849aa060fe87d77cccf68
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Visual Studio Code gebruiken om op te sporen Azure werkt in combinatie met Azure IoT rand

In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio Code](https://code.visualstudio.com/) als de belangrijkste ontwikkelprogramma fouten opsporen in uw Azure-functies op IoT rand.

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT-randapparaat kan een andere fysieke apparaat of kunt u uw IoT-randapparaat simuleren op uw ontwikkelcomputer.

Zorg ervoor dat u hebt voltooid volgende zelfstudies voordat u deze richtlijnen.
- [Visual Studio Code te ontwikkelen en implementeren van Azure Functions voor Azure IoT Edge gebruiken](how-to-vscode-develop-azure-function.md)

Na het voltooien de voorgaande zelfstudie, moet u de volgende items gereed is, hebben
- Een lokale Docker-register uitgevoerd op uw ontwikkelcomputer. Aangeraden wordt een lokale Docker-register voor prototype- en testdoeleinden gebruiken.
- De `run.csx` bestand met de meest recente filter functiecode.
- Een bijgewerkte `deployment.json` -bestand voor uw sensor-module en filter functiemodule.
- Een Edge-runtime is uitgevoerd op uw ontwikkelcomputer.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Uw IoT-Edge-module voor het opsporen van doel maken
1. Voor foutopsporing starten, moet u de **dockerfile.debug** naar uw docker-installatiekopie bouwen en implementeren van uw oplossing opnieuw. Klik op de Docker-map om deze te openen in VS-Code explorer. Klik vervolgens op de `linux-x64` map met de rechtermuisknop op de **Dockerfile.debug**, en klik op **bouwen IoT rand module Docker installatiekopie**.

    ![Foutopsporing installatiekopie maken](./media/how-to-debug-csharp-function/build-debug-image.png)

2. In de **map selecteren** venster, navigeer naar de **FilterFunction** project en klik op **map selecteren als EXE_DIR**.
3. Typ in het pop-tekst boven aan het venster tegenover Code, naam van de installatiekopie. Bijvoorbeeld: `<your container registry address>/filterfunction:latest`. Als u naar het lokale register implementeert, moet dit worden `localhost:5000/filterfunction:latest`.

    ![Push-afbeelding](./media/how-to-debug-csharp-function/push-image.png)

4. De installatiekopie naar uw opslagplaats Docker forceren. Gebruik de **rand: Push IoT rand module Docker installatiekopie** opdracht en de afbeeldings-URL in het pop-tekst boven aan het venster VS-Code invoeren. Gebruik dezelfde afbeeldings-URL die u in bovenstaande stap gebruikt.
5. U kunt opnieuw gebruiken de `deployment.json` te implementeren. Typ in de opdracht palet, en selecteer **rand: opnieuw opstarten rand** ophalen van uw filterfunctie uitgevoerd met de foutopsporingsversie.

## <a name="start-debugging-in-vs-code"></a>Foutopsporing in VS-Code starten
1. Ga naar tegenover Code foutopsporingsvenster. Druk op **F5** en selecteer **IoT Edge(.Net Core)**

    ![Druk op F5](./media/how-to-debug-csharp-function/f5-debug-option.png)

2. In `launch.json`, gaat u naar **IoT Edge-functie (.NET Core) Debug** sectie en vul de `<container_name>`onder `pipeArgs`. Deze moet `filterfunction` in deze zelfstudie.

    ![Launch.json bijwerken](./media/how-to-debug-csharp-function/update-launch-json.png)

3. Ga naar run.csx. Een onderbrekingspunt in de functie toevoegen.
4. Navigeer naar venster Foutopsporing (Ctrl + Shift + D), kiest u **IoT Edge-functie (.NET Core) Debug** uit de vervolgkeuzelijst. 

    ![Selecteer de foutopsporingsmodus](./media/how-to-debug-csharp-function/choose-debug-mode.png)

5. Klik op knop foutopsporing starten of druk op **F5**, en selecteert u het proces om aan te koppelen.

    ![De functie proces koppelen](./media/how-to-debug-csharp-function/attach-function-process.png)

6. In Code fouten opsporen in VS-venster ziet u de variabelen in het linkerdeelvenster. 

> [!NOTE]
> Hierboven voorbeeld ziet u hoe u foutopsporing .net Core IoT Edge-functie op containers. Deze gebaseerd op de foutopsporingsversie van de `Dockerfile.debug`, waaronder VSDBG (het opdrachtregelprogramma .NET Core-foutopsporingsprogramma) in uw installatiekopie container tijdens het opbouwen van het. We raden u rechtstreeks gebruiken of aanpassen aan de `Dockerfile` zonder VSDBG voor gereed is voor productie IoT rand functie als u klaar bent met foutopsporing van uw C#-functie.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie een Azure-functie gemaakt en geïmplementeerd bij rand IoT voor foutopsporing doel en fouten kunt opsporen in VS-Code is gestart. U kunt door te gaan naar een van de volgende zelfstudies voor meer informatie over andere scenario's bij het ontwikkelen van Azure IoT rand in de VS-Code. 

> [!div class="nextstepaction"]
> [Ontwikkelen en implementeren van C#-module in VS-Code](how-to-vscode-develop-csharp-module.md)


---
title: Visual Studio Code gebruiken om op te sporen een C#-module met Azure IoT rand | Microsoft Docs
description: Fouten opsporen in een C#-module met Azure IoT rand in de Visual Studio Code.
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5ed517cf8d70cd279a55b79ad448709116cf511b
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="use-visual-studio-code-to-debug-a-c-module-with-azure-iot-edge"></a>Gebruik Visual Studio Code fouten opsporen in een C#-module met Azure IoT rand
In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio Code](https://code.visualstudio.com/) als de belangrijkste ontwikkelprogramma fouten opsporen in uw Azure-IoT-Edge-modules.

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT-randapparaat kan een andere fysieke apparaat, of kunt u uw IoT-randapparaat simuleren op uw ontwikkelcomputer.

Voordat u deze richtlijnen, moet u de volgende zelfstudie voltooien:
- [Visual Studio Code gebruiken voor het ontwikkelen van C#-module met Azure IoT rand](how-to-vscode-develop-csharp-module.md)

Nadat u de voorgaande zelfstudie hebt voltooid, hebt u de volgende items gereed:
- Een lokale Docker-register uitgevoerd op uw ontwikkelcomputer. Dit is voor het maken van een prototype- en testdoeleinden.
- De `Program.cs` bestand met de meest recente filter module code.
- Een bijgewerkte `deployment.json` -bestand voor uw modules sensor en filteren.
- Een IoT-Edge-runtime die wordt uitgevoerd op uw ontwikkelcomputer.

## <a name="build-your-iot-edge-module-for-debugging"></a>Uw IoT-Edge-module voor het opsporen van maken
1. Gebruiken om foutopsporing starten **dockerfile.debug** uw Docker-installatiekopie bouwen en de rand van de IoT-oplossing opnieuw implementeren. Selecteer de map Docker om dit te openen in Visual Studio Code Explorer. Selecteer vervolgens de **linux x64** map met de rechtermuisknop op **Dockerfile.debug**, en selecteer **bouwen IoT rand module Docker installatiekopie**.

    ![Schermafbeelding van de VS Code Explorer](./media/how-to-debug-csharp-module/build-debug-image.png)

3. In de **map selecteren** venster ofwel om te zoeken of invoeren **./bin/Debug/netcoreapp2.0/publish**. Selecteer vervolgens **map selecteren als EXE_DIR**.
4. Typ in het pop-tekst boven aan het venster tegenover Code, naam van de installatiekopie. Bijvoorbeeld: `<your container registry address>/filtermodule:latest`. Als u naar het lokale register implementeert, moet dit worden: `localhost:5000/filtermodule:latest`.
5. De installatiekopie naar uw opslagplaats Docker forceren. Gebruik de **rand: Push IoT rand module Docker installatiekopie** opdracht en de afbeeldings-URL in het pop-tekst boven aan het venster VS-Code invoeren. Gebruik dezelfde afbeeldings-URL die u hebt gebruikt in de vorige stap.
6. U kunt opnieuw gebruiken de `deployment.json` te implementeren. Typ in het palet opdracht en selecteer **rand: opnieuw opstarten rand** ophalen van de module expressiefilter uitgevoerd met de foutopsporingsversie.

## <a name="start-debugging-in-vs-code"></a>Foutopsporing in VS-Code starten
1. Ga naar het venster van de foutopsporing VS-Code. Druk op **F5**, en selecteer **IoT Edge(.NET Core)**.

    ![Schermafbeelding van de Code van de VS foutopsporingsvenster](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. In `launch.json`, blader naar de **fouten opsporen in IoT rand aangepaste Module (.NET Core)** sectie. Onder **pipeArgs**, vult u de `<container_name>`. Deze moet `filtermodule` in deze zelfstudie.

    ![Schermafbeelding van de VS Code launch.json](./media/how-to-debug-csharp-module/add-container-name.png)

3. Blader naar **Program.cs**. Toevoegen van een onderbrekingspunt in de `method static async Task<MessageResponse> FilterModule(Message message, object userContext)`.
4. Druk op **F5** opnieuw, en selecteert u het proces om aan te koppelen. In deze zelfstudie maakt u de procesnaam moet `FilterModule.dll`.

    ![Schermafbeelding van de Code van de VS foutopsporingsvenster](./media/how-to-debug-csharp-module/attach-process.png)

5. In het venster van de foutopsporing tegenover Code ziet u de variabelen in het linkerdeelvenster. 

> [!NOTE]
> Het vorige voorbeeld laat zien hoe .NET Core IoT Edge-modules voor containers voor foutopsporing. Deze gebaseerd op de foutopsporingsversie van de `Dockerfile.debug`, waaronder VSDBG (het opdrachtregelprogramma .NET Core-foutopsporingsprogramma) in uw installatiekopie container tijdens het opbouwen van het. Nadat u foutopsporing van uw C#-modules, raden we u rechtstreeks gebruiken of aanpassen `Dockerfile` zonder VSDBG voor modules die gereed is voor productie IoT rand.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie een IoT-Edge-module gemaakt en geïmplementeerd voor foutopsporing. U fouten kunt opsporen in VS-Code hebt gestart. Voor meer informatie over andere scenario's wanneer u Azure IoT rand in de VS Code ontwikkelt, Zie: 

> [!div class="nextstepaction"]
> [Ontwikkelen en implementeren van C#-module in VS-Code](how-to-vscode-develop-csharp-module.md)

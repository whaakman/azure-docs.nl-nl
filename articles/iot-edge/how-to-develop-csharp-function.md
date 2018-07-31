---
title: Fouten opsporen in Azure-functie-modules voor Azure IoT Edge | Microsoft Docs
description: Gebruik Visual Studio Code fouten opsporen in C# Azure functions met Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: bf2d1af66cc3ecc35dafe3bcd43bf10399d71641
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346712"
---
# <a name="use-visual-studio-code-to-develop-and-debug-azure-functions-for-azure-iot-edge"></a>Visual Studio Code gebruiken om te ontwikkelen en fouten opsporen in Azure functions voor Azure IoT Edge

In dit artikel leest u hoe u [Visual Studio Code (VS-Code)](https://code.visualstudio.com/) fouten opsporen in uw Azure-functies in Azure IoT Edge.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT Edge-apparaat mag een andere fysieke apparaat. Of u kunt uw IoT Edge-apparaat simuleren op uw ontwikkelcomputer.

> [!NOTE]
> In dit artikel foutopsporing ziet u hoe u een proces in een container module koppelen en foutopsporing kunt uitvoeren met VS Code. U kunt alleen fouten opsporen C#-functies in Linux amd64-containers. Als u niet bekend bent met de mogelijkheden voor foutopsporing van Visual Studio Code, meer informatie over [foutopsporing](https://code.visualstudio.com/Docs/editor/debugging). 

In dit artikel wordt Visual Studio Code als de belangrijkste ontwikkeltool. Visual Studio Code installeren. Voeg vervolgens de nodige uitbreidingen toe: 

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C#-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker-extensie](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Voor het maken van een module, moet u .NET voor het bouwen van de projectmap, Docker om de installatiekopie van de module en een containerregister voor de module-installatiekopie te bouwen:
* [.NET core SDK 2.1](https://www.microsoft.com/net/download)
* [Docker Community Edition](https://docs.docker.com/install/) op uw ontwikkelcomputer 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > U kunt een lokale Docker-register voor prototype en in plaats van een cloud-register voor testdoeleinden gebruiken. 

Als u wilt testen van uw module op een apparaat, moet u een actieve IoT-hub met ten minste één IoT Edge-apparaat. Voor het gebruik van de computer als een IoT Edge-apparaat, volg de stappen in de quickstart voor [Windows](quickstart.md) of [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Een nieuwe oplossingssjabloon maken

Volg deze stappen voor het maken van een IoT Edge-oplossing met een C#-functiemodule. Elke oplossing kan meerdere modules hebben.

1. Selecteer in Visual Studio Code, **weergave** > **geïntegreerde Terminal**.
3. Selecteer **weergave** > **Command Palette**.
4. Voer in het opdrachtenpalet en voer de opdracht **Azure IoT Edge: nieuwe IoT Edge-oplossing**. 

   ![Nieuwe IoT Edge-oplossing uitvoeren](./media/how-to-develop-csharp-module/new-solution.png)

5. Blader naar de map waar u de nieuwe oplossing maken. Kies **map selecteren**. 
6. Voer een naam voor uw oplossing. 
7. Kies **Azure Functions - C#** als de sjabloon voor de eerste module in de oplossing.
8. Voer een naam voor uw module. Kies een naam die uniek is binnen uw containerregister. 
9. Geef de opslagplaats voor installatiekopieën voor de module. VS Code autopopulates de module met naam **localhost:5000**. Vervang deze door uw eigen gegevens. Als u een lokale Docker-register voor het testen, klikt u vervolgens **localhost** functioneert. Als u Azure Container Registry gebruikt, gebruikt u de aanmeldingsserver van de instellingen van uw register. De aanmeldingsserver ziet eruit als  **\<registernaam\>. azurecr.io**.

VS Code wordt de informatie die u verstrekt, maakt een IoT Edge-oplossing met een Azure Functions-project en laadt deze in een nieuw venster.

Er zijn vier items in de oplossing: 

* Een **.vscode** map bevat foutopsporing configuraties.
* Een **modules** map bevat submappen voor elke module. Op dit moment, hoeft u slechts één. Maar u meer kunt toevoegen via het opdrachtenpalet met de opdracht **Azure IoT Edge: IoT Edge-Module toevoegen**.
* Een **.env** bestand geeft een lijst van de omgevingsvariabelen. Als u Azure Container Registry is uw register, hebt u een Azure Container Registry-gebruikersnaam en wachtwoord in het. 

   >[!NOTE]
   >Het omgevingsbestand wordt alleen gemaakt als u een opslagplaats voor installatiekopieën voor de module opgeven. Als u de standaardinstellingen localhost om te testen en fouten opsporen in lokaal hebt geaccepteerd, moet u geen omgevingsvariabelen declareren. 

* Een **deployment.template.json** bestand geeft een lijst van de nieuwe module samen met een voorbeeld van een **tempSensor** module die gegevens simuleert u voor testdoeleinden kunt gebruiken. Zie voor meer informatie over hoe implementatie werk manifesten [informatie over het gebruik van implementatie manifesten implementeren modules en routes tot stand brengen](module-composition.md).

## <a name="develop-your-module"></a>Uw-module ontwikkelen

De standaardcode voor het Azure-functie die wordt geleverd met de oplossing bevindt zich in **modules** > **\<de modulenaam van uw\>**   >   **EdgeHubTrigger-Csharp** > **run.csx**. De module en het bestand deployment.template.json zijn ingesteld zodat u kunt de oplossing te bouwen, deze naar het containerregister pushen en implementeren op een apparaat om te testen zonder code aan te starten. De module is gebouwd om te gewoon nemen invoer van een bron (in dit geval de tempSensor-module die gegevens simuleert) en doorgeven naar IoT Hub. 

Wanneer u klaar bent om de sjabloon van de Azure-functie met uw eigen code aanpassen, gebruikt u de [Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md) modules bouwen dat adres de sleutel die nodig zijn voor IoT-oplossingen, zoals beveiliging, beheer van apparaten en betrouwbaarheid. 

## <a name="build-your-module-for-debugging"></a>De module voor foutopsporing maken
1. Gebruiken voor het starten van foutopsporing, **Dockerfile.amd64.debug** opnieuw opbouwen van uw docker-installatiekopie en opnieuw implementeren van uw Edge-oplossing. Navigeer in de VS Code explorer naar de `deployment.template.json` bestand. De URL van uw functie-installatiekopie bijwerken door toe te voegen `.debug` aan het einde.

    ![Debug-installatiekopie bouwen](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Uw oplossing opnieuw maakt. Voer in het opdrachtenpalet VS Code en voer de opdracht **Azure IoT Edge: Build IoT Edge-oplossing**.
3. In de Verkenner van Azure IoT Hub-apparaten met de rechtermuisknop op een IoT Edge-apparaat-ID en selecteer vervolgens **implementatie voor Edge-apparaat maken**. Selecteer de `deployment.json` -bestand in de `config` map. Hier ziet u de implementatie is gemaakt met een implementatie-ID in een VS Code geïntegreerde terminal.

Controleer de containerstatus van uw in Docker van VS Code explorer of door het uitvoeren van de `docker images` opdracht in de terminal.

## <a name="start-debugging-c-functions-in-vs-code"></a>C#-functies in VS Code voor de foutopsporing starten
1. VS Code houdt opsporen van fouten in configuratie-informatie in een `launch.json` bestand zich bevindt in een `.vscode` map in uw werkruimte. Dit `launch.json` bestand is gegenereerd tijdens het maken van een nieuwe IoT Edge-oplossing. Bijgewerkt telkens wanneer u een nieuwe module die ondersteuning biedt voor foutopsporing toevoegen. Navigeer naar de weergave voor foutopsporing. Selecteer het bijbehorende configuratiebestand voor foutopsporing. De naam van de optie foutopsporing zijn vergelijkbaar met **ModuleName externe foutopsporing (.NET Core)**.

   ![Selecteer debug-configuratie](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navigeer naar `run.csx`. Voeg een onderbrekingspunt toe in de functie.
3. Selecteer **Start Debugging** of selecteer **F5**. Selecteer het proces om aan te koppelen.
4. In de weergave van VS Code-debug ziet u de variabelen in het linkerdeelvenster. 


> [!NOTE]
> In dit voorbeeld laat zien hoe u fouten opsporen in .net Core IoT Edge-functies voor containers. Deze gebaseerd op de foutopsporingsversie van de `Dockerfile.amd64.debug`, waaronder het opdrachtregelprogramma foutopsporingsprogramma van .NET Core VSDBG in uw containerinstallatiekopie tijdens het maken van deze. We raden aan dat u rechtstreeks gebruiken of aanpassen van de `Dockerfile` zonder VSDBG voor gereed is voor productie IoT Edge werkt nadat u fouten opsporen in uw C#-functies.

## <a name="next-steps"></a>Volgende stappen

Nadat u uw module hebt gebouwd, informatie over hoe u [implementeren Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md).

Voor het ontwikkelen van modules voor uw IoT Edge-apparaten, [begrijpen en gebruiken Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md).

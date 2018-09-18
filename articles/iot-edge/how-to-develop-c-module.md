---
title: Fouten opsporen in C-modules voor Azure IoT Edge | Microsoft Docs
description: Gebruik Visual Studio Code ontwikkelen, bouwen en fouten opsporen in een C-module voor Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/13/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 3ec7f6043c1d2e8e8f090ffc60822768ab9bc9d9
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983998"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Visual Studio Code gebruiken om te ontwikkelen en fouten opsporen in C-modules voor Azure IoT Edge

U kunt uw bedrijfslogica in modules inschakelen voor Azure IoT Edge. Dit artikel ziet u hoe u Visual Studio Code (VS-Code) gebruikt als het belangrijkste instrument ontwikkelen en fouten opsporen C-modules.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. En u uw IoT Edge-apparaat simuleren op een ontwikkelcomputer met IoT Edge security daemon.

> [!NOTE]
> In dit artikel foutopsporing ziet u hoe u een proces in een container module koppelen en foutopsporing kunt uitvoeren met VS Code. U kunt alleen fouten opsporen C-modules in Linux amd64-containers. Als u niet bekend bent met de mogelijkheden voor foutopsporing van Visual Studio Code, meer informatie over [foutopsporing](https://code.visualstudio.com/Docs/editor/debugging).

Omdat in dit artikel Visual Studio Code als de belangrijkste ontwikkeltool wordt, Visual Studio Code installeren. Voeg vervolgens de nodige uitbreidingen toe:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C/C++-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) voor Visual Studio Code (Engelstalig).
* [Docker-extensie](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Voor het maken van een module, moet u Docker om de installatiekopie van de module en een containerregister voor de module-installatiekopie te bouwen:
* [Docker Community Edition](https://docs.docker.com/install/) op uw ontwikkelcomputer. 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * U kunt een lokale Docker-register voor prototype en in plaats van een cloud-register voor testdoeleinden gebruiken. 

Als u wilt testen van uw module op een apparaat, moet u een actieve IoT-hub met ten minste één IoT Edge-apparaat. Voor het gebruik van de computer als een IoT Edge-apparaat, volg de stappen in de quickstart voor [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Een nieuwe oplossingssjabloon maken

Volg deze stappen voor het maken van een IoT Edge-module op basis van Azure IoT C SDK met behulp van Visual Studio Code en de Azure IoT Edge-extensie. U maakt eerst een oplossing en vervolgens het genereren van de eerste module in die oplossing. Elke oplossing kan meer dan één module bevatten. 

1. Selecteer in Visual Studio Code, **weergave** > **geïntegreerde Terminal**.

2. Selecteer **weergave** > **Command Palette**. 

3. Voer in het opdrachtenpalet en voer de opdracht **Azure IoT Edge: nieuwe IoT Edge-oplossing**.

   ![Nieuwe IoT Edge-oplossing uitvoeren](./media/how-to-develop-csharp-module/new-solution.png)

4. Blader naar de map waar u de nieuwe oplossing maken. Kies **map selecteren**. 

5. Voer een naam voor uw oplossing. 

6. Selecteer **C Module** als de sjabloon voor de eerste module in de oplossing.

7. Voer een naam voor uw module. Kies een naam die uniek is binnen uw containerregister. 

8. Geef de naam van de opslagplaats voor installatiekopieën van de module. VS Code autopopulates de module met naam **localhost:5000**. Vervang deze door uw eigen gegevens. Als u een lokale Docker-register voor het testen, klikt u vervolgens **localhost** functioneert. Als u Azure Container Registry gebruikt, gebruikt u de aanmeldingsserver van de instellingen van uw register. De aanmeldingsserver ziet eruit als  **\<registernaam\>. azurecr.io**. Vervang alleen het onderdeel localhost van de tekenreeks, verwijder niet de naam van de module. 

   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/how-to-develop-c-module/repository.png)

VS Code haalt de gegevens die u verstrekt, maakt u een IoT Edge-oplossing, en laadt deze in een nieuw venster.

   ![IoT Edge-oplossing weergeven](./media/how-to-develop-c-module/view-solution.png)

Er zijn vier items in de oplossing: 
* Een **.vscode** map bevat foutopsporing configuraties.
* Een **modules** map bevat submappen voor elke module. Op dit moment, hoeft u slechts één. Maar u kunt meer toevoegen in het opdrachtenpalet met de opdracht **Azure IoT Edge: IoT Edge-Module toevoegen**. 
* Een **.env** bestand geeft een lijst van de omgevingsvariabelen. Als u Azure Container Registry is uw register, hebt u een Azure Container Registry-gebruikersnaam en wachtwoord in het. 

   > [!NOTE]
   > Het omgevingsbestand wordt alleen gemaakt als u een opslagplaats voor installatiekopieën voor de module opgeven. Als u de standaardinstellingen localhost om te testen en fouten opsporen in lokaal hebt geaccepteerd, moet u geen omgevingsvariabelen declareren. 

* Een **deployment.template.json** bestand geeft een lijst van de nieuwe module samen met een voorbeeld van een **tempSensor** module die gegevens simuleert u voor testdoeleinden kunt gebruiken. Zie voor meer informatie over hoe implementatie werk manifesten [informatie over het gebruik van implementatie manifesten implementeren modules en routes tot stand brengen](module-composition.md). 

## <a name="develop-your-module"></a>Uw-module ontwikkelen

De standaard C-module-code die wordt geleverd met de oplossing bevindt zich in **modules** > [Modulenaam van uw] > **main.c**. De module en het bestand deployment.template.json zijn ingesteld zodat u kunt de oplossing te bouwen, deze naar het containerregister pushen en implementeren op een apparaat om te testen zonder code aan te starten. De module is gebouwd om te gewoon nemen invoer van een bron (in dit geval de tempSensor-module die gegevens simuleert) en doorgeven naar IoT Hub. 

Wanneer u klaar bent om de sjabloon C met uw eigen code aanpassen, gebruikt u de [Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md) modules bouwen dat adres de sleutel die nodig zijn voor IoT-oplossingen, zoals beveiliging, beheer van apparaten en betrouwbaarheid. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Bouw en implementeer uw module voor foutopsporing

In elke modulemap zijn er verschillende Docker-bestanden voor andere containertypen. Gebruik een van deze bestanden die met de extensie eindigen **.debug** uw-module voor het testen van maken. Op dit moment ondersteunen C modules foutopsporing alleen in Linux amd64-containers.

1. In VS Code, gaat u naar de `deployment.template.json` bestand. De afbeeldings-URL van uw module bijwerken door toe te voegen **.debug** aan het einde.

    ![Voeg *** .debug op de installatiekopienaam van uw](./media/how-to-develop-c-module/image-debug.png)

2. Vervang de C-module createOptions in **deployment.template.json** met onderstaande inhoud en sla dit bestand: 
    
    ```json
    "createOptions": "{\"HostConfig\": {\"Privileged\": true}}"
    ```

2. Voer in het opdrachtenpalet VS Code en voer de opdracht **Azure IoT Edge: Build and Push IoT Edge-oplossing**.
3. Selecteer de `deployment.template.json` -bestand voor uw oplossing van de command palette. 
4. In Azure IoT Hub Device Explorer met de rechtermuisknop op een IoT Edge-apparaat-ID. Selecteer vervolgens **implementatie voor één apparaat maken**. 
5. Openen van uw oplossing **config** map. Selecteer vervolgens de `deployment.json` bestand. Kies **Selecteer Edge-implementatie Manifest**. 

Hier ziet u de implementatie is gemaakt met een implementatie-ID in een VS Code geïntegreerde terminal.

Controleer de containerstatus van uw in Docker van VS Code explorer of door het uitvoeren van de `docker ps` opdracht in de terminal.

## <a name="start-debugging-c-module-in-vs-code"></a>C-module in de VS Code voor de foutopsporing starten
VS Code houdt opsporen van fouten in configuratie-informatie in een `launch.json` bestand zich bevindt in een `.vscode` map in uw werkruimte. Dit `launch.json` bestand is gegenereerd tijdens het maken van een nieuwe IoT Edge-oplossing. Bijgewerkt telkens wanneer u een nieuwe module die ondersteuning biedt voor foutopsporing toevoegen. 

1. Navigeer naar de weergave voor foutopsporing van VS Code. Selecteer het configuratiebestand voor de foutopsporing voor de module. De naam van de optie foutopsporing zijn vergelijkbaar met **ModuleName Remote Debug (C)**

   ![Selecteer debug-configuratie](./media/how-to-develop-c-module/debug-config.png)

2. Navigeer naar `main.c`. Voeg een onderbrekingspunt toe in dit bestand.

3. Selecteer **Start Debugging** of selecteer **F5**. Selecteer het proces om aan te koppelen.

4. VS-Code opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 

Het vorige voorbeeld laat zien hoe fouten opsporen in C IoT Edge-modules voor containers. Deze blootgestelde poorten in uw container module createOptions toegevoegd. Nadat u klaar bent met het opsporen van fouten in uw C-modules, wordt u aangeraden verwijderen van deze blootgestelde poorten voor de IoT Edge-modules gereed is voor productie.

## <a name="next-steps"></a>Volgende stappen

Nadat u uw module hebt gebouwd, informatie over hoe u [implementeren van Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md).

Voor het ontwikkelen van modules voor uw IoT Edge-apparaten, [begrijpen en gebruiken Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md).

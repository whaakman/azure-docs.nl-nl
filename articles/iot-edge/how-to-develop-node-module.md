---
title: Fouten opsporen in Node.js-modules voor Azure IoT Edge | Microsoft Docs
description: Visual Studio Code gebruiken om te ontwikkelen en fouten opsporen in Node.js-modules voor Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: a5ab49beed79a8ea3a7ded0848c09acad27a5fb1
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39390534"
---
# <a name="develop-and-debug-nodejs-modules-with-azure-iot-edge-for-visual-studio-code"></a>Ontwikkelen en fouten opsporen in Node.js-modules met Azure IoT Edge voor Visual Studio Code

U kunt uw bedrijfslogica op de edge werken door te schakelen in modules voor Azure IoT Edge kunt verzenden. In dit artikel vindt u gedetailleerde instructies voor het gebruik van Visual Studio Code (VS-Code) als de belangrijkste ontwikkelingsprogramma voor het ontwikkelen van C#-modules.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT Edge-apparaat kan een andere fysieke apparaat, of kunt u uw IoT Edge-apparaat simuleren op uw ontwikkelcomputer.

> [!NOTE]
> In deze zelfstudie foutopsporing wordt beschreven hoe u een proces in een container module koppelen en foutopsporing kunt uitvoeren met VS Code. U kunt fouten opsporen in Node.js-modules in linux-amd64, windows en arm32 containers. Als u niet bekend bent met de mogelijkheden voor foutopsporing van Visual Studio Code, meer informatie over [foutopsporing](https://code.visualstudio.com/Docs/editor/debugging). 

Omdat in dit artikel Visual Studio Code als de belangrijkste ontwikkeltool wordt, Visual Studio Code installeren en vervolgens de nodige uitbreidingen toevoegen:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Docker-extensie](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Voor het maken van een module, moet u Node.js waaronder npm voor het bouwen van de projectmap, Docker om de installatiekopie van de module en een containerregister voor de module-installatiekopie te bouwen:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   >[!TIP]
   >U kunt een lokale Docker-register voor prototype- en testdoeleinden, in plaats van een cloud-register. 

Als u wilt testen van uw module op een apparaat, moet u een actieve IoT-hub met ten minste één IoT Edge-apparaat. Als u wilt dat uw computer te gebruiken als een IoT Edge-apparaat, kunt u doen met de volgende stappen in de zelfstudies voor [Windows](quickstart.md) of [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Een nieuwe oplossingssjabloon maken

De volgende stappen laten zien hoe u een IoT Edge-module op basis van .NET Core 2.0 maken met behulp van Visual Studio Code en de Azure IoT Edge-extensie. Begint u met het maken van een oplossing en genereert vervolgens de eerste module in die oplossing. Elke oplossing kan meerdere modules bevatten. 

1. Selecteer in Visual Studio Code, **weergave** > **geïntegreerde Terminal**.
2. Voer de volgende opdracht om te installeren (of bijwerken) op de meest recente versie van de sjabloon voor Azure IoT Edge-module voor Node.js in de geïntegreerde terminal:

   ```cmd/sh
   npm install -g yo generator-azure-iot-edge-module
   ```
3. Selecteer **View** > **Command Palette** in Visual Studio Code. 
4. Typ in het opdrachtenpalet en voer de opdracht **Azure IoT Edge: nieuwe IoT Edge-oplossing**.

   ![Nieuwe IoT Edge-oplossing uitvoeren](./media/how-to-develop-csharp-module/new-solution.png)

5. Blader naar de map waar u de nieuwe oplossing maken en klikt u op **map selecteren**. 
6. Geef een naam voor uw oplossing. 
7. Kies **Node.js-Module** als de sjabloon voor de eerste module in de oplossing.
8. Geef een naam voor uw module. Kies een naam die uniek is binnen uw containerregister. 
9. Geef de opslagplaats voor installatiekopieën voor de module. VS Code autopopulates de module-naam, dus u hoeft te vervangen **localhost:5000** met uw eigen gegevens. Als u een lokale Docker-register voor het testen, klikt u vervolgens is ' localhost ' geen probleem. Als u Azure Container Registry gebruikt, gebruikt u de aanmeldingsserver van de instellingen van uw register. De aanmeldingsserver ziet eruit als  **\<registernaam\>. azurecr.io**.

VS Code haalt de gegevens opgegeven, maakt u een IoT Edge-oplossing en laadt deze in een nieuw venster.

In de oplossing hebt u drie items: 
* Een **.vscode** map bevat foutopsporing configuraties.
* Een **modules** map bevat submappen voor elke module. Op dit moment u slechts één hebt, maar u kunt meer toevoegen in het opdrachtenpalet met de opdracht **Azure IoT Edge: IoT Edge-Module toevoegen**. 
* Een **.env** bestand geeft een lijst van de omgevingsvariabelen. Als u ACR als het register, rechts nu kunt u ACR-gebruikersnaam en wachtwoord in het. 

   >[!NOTE]
   >Het omgevingsbestand wordt alleen gemaakt als u een opslagplaats voor installatiekopieën voor de module opgeven. Als u de standaardinstellingen localhost om te testen en fouten opsporen in lokaal hebt geaccepteerd, moet u geen omgevingsvariabelen declareren. 

* Een **deployment.template.json** bestand geeft een lijst van de nieuwe module samen met een voorbeeld van een **tempSensor** module die gegevens die u gebruiken voor het testen van kunt simuleert. Zie voor meer informatie over hoe implementatie werk manifesten [te begrijpen hoe IoT Edge-modules kunnen worden gebruikt, geconfigureerd en opnieuw gebruikt](module-composition.md).

## <a name="devlop-your-module"></a>Devlop uw module

De standaardcode voor het Azure-functie die wordt geleverd met de oplossing bevindt zich in **modules** > **\<de modulenaam van uw\>**   >   **App.js**. De module en het bestand deployment.template.json zijn ingesteld zodat u kunt de oplossing te bouwen, deze naar het containerregister pushen en implementeren op een apparaat om te testen zonder code aan te starten. De module is gebouwd om te gewoon nemen invoer van een bron (in dit geval de tempSensor-module die gegevens simuleert) en doorgeven naar IoT Hub. 

Wanneer u klaar bent om de Node.js-sjabloon met uw eigen code aanpassen, gebruikt u de [Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md) modules bouwen dat adres de sleutel die nodig zijn voor IoT-oplossingen, zoals beveiliging, beheer van apparaten en betrouwbaarheid. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Bouw en implementeer uw module voor foutopsporing

Er zijn meerdere Docker-bestanden voor typen die andere container in elke modulemap. U kunt een van deze bestanden die met de extensie eindigen **.debug** uw-module voor het testen van maken. Op dit moment ondersteuning C#-modules alleen voor foutopsporing in linux-amd64-containers.

1. In VS Code, gaat u naar de `deployment.template.json` bestand. De afbeeldings-URL van uw module bijwerken door toe te voegen **.debug** aan het einde.
2. Vervang de Node.js-module createOptions in **deployment.template.json** met onderstaande inhoud en sla dit bestand: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

2. Typ in het opdrachtenpalet VS Code en voer de opdracht **Azure IoT Edge: Build IoT Edge-oplossing**.
3. Selecteer de `deployment.template.json` -bestand voor uw oplossing van de command palette. 
4. In de Verkenner van Azure IoT Hub-apparaten met de rechtermuisknop op een IoT Edge-apparaat-ID en selecteer vervolgens **implementatie voor IoT Edge-apparaat maken**. 
5. Open de **config** map van uw oplossing, selecteer vervolgens de `deployment.json` bestand. Klik op **Edge-distributiemanifest selecteren**. 

Vervolgens ziet u dat de implementatie is gemaakt met een implementatie-ID in VS Code geïntegreerde terminal.

U kunt de containerstatus van uw in Docker van VS Code explorer of door uitvoering controleren de `docker ps` opdracht in de terminal.

## <a name="start-debugging-nodejs-module-in-vs-code"></a>Node.Js-module in VS Code voor de foutopsporing starten

VS Code houdt opsporen van fouten in configuratie-informatie in een `launch.json` bestand zich bevindt in een `.vscode` map in uw werkruimte. Dit `launch.json` bestand is gegenereerd tijdens het maken van een nieuwe IoT Edge-oplossing. Bijgewerkt telkens wanneer u een nieuwe module die ondersteuning biedt voor foutopsporing toevoegen. 

1. Navigeer naar de weergave voor foutopsporing van VS Code en selecteer het configuratiebestand voor de foutopsporing voor de module.

2. Navigeer naar `app.js`. Voeg een onderbrekingspunt toe in dit bestand.

3. Klik op de **Start Debugging** of drukt u op **F5**, en selecteert u het proces om aan te koppelen.

4. VS-Code opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 

Het vorige voorbeeld laat zien hoe fouten opsporen in Node.js IoT Edge-modules voor containers. Deze blootgestelde poorten in uw container module createOptions toegevoegd. Nadat u klaar bent met het opsporen van fouten in uw Node.js-modules, wordt u aangeraden verwijderen van deze blootgestelde poorten voor de IoT Edge-modules gereed is voor productie.

## <a name="next-steps"></a>Volgende stappen

Zodra u uw module die is gebouwd hebt, informatie over hoe u [implementeren Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md)

Voor het ontwikkelen van modules voor uw IoT Edge-apparaten, [begrijpen en gebruiken Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md).

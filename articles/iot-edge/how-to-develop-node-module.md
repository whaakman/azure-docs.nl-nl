---
title: Fouten opsporen in Node.js-modules voor Azure IoT rand | Microsoft Docs
description: Visual Studio Code te ontwikkelen en testen van Node.js-modules voor Azure IoT Edge gebruiken
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 6b217690b88f303268f5abe66abb7868711d3125
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045089"
---
# <a name="develop-and-debug-nodejs-modules-with-azure-iot-edge-for-visual-studio-code"></a>Ontwikkelen en testen van Node.js-modules met Azure IoT rand voor Visual Studio Code

U kunt uw bedrijfslogica op de rand werken door in te schakelen deze in modules voor Azure IoT rand verzenden. In dit artikel vindt u gedetailleerde instructies voor het gebruik van Visual Studio Code (VS) weergegeven als de belangrijkste ontwikkelprogramma voor het ontwikkelen van C#-modules.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT-randapparaat kan een andere fysieke apparaat, of kunt u uw IoT-randapparaat simuleren op uw ontwikkelcomputer.

> [!NOTE]
> Deze zelfstudie foutopsporing wordt beschreven hoe koppelen van een proces in een module-container op te sporen met VS-Code. U kunt fouten opsporen Node.js-modules in linux amd64, windows en arm32 containers. Als u niet bekend bent met de mogelijkheden voor foutopsporing van Visual Studio Code, leest u over [foutopsporing](https://code.visualstudio.com/Docs/editor/debugging). 

Sinds dit artikel wordt Visual Studio Code als de belangrijkste ontwikkelprogramma gebruikt, VS-Code installeren en vervolgens de nodige uitbreidingen toevoegen:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Docker-uitbreiding](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

U moet Node.js waaronder npm om samen te stellen de projectmap, Docker voor het bouwen van de installatiekopie van de module en een container register voor de installatiekopie van de module voor het maken van een module:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Azure-Container register](https://docs.microsoft.com/azure/container-registry/) of [Docker-Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   >[!TIP]
   >U kunt een lokale Docker-register gebruiken voor prototype- en testdoeleinden, in plaats van een cloud-register. 

Als u wilt testen uw module op een apparaat, moet u een actieve iothub met ten minste één rand van de IoT-apparaat. Als u wilt uw computer te gebruiken als een Edge van de IoT-apparaat, kunt u doen door de stappen in de zelfstudies voor [Windows](quickstart.md) of [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Maak een nieuwe oplossingssjabloon

De volgende stappen ziet u hoe u een IoT-Edge-module op basis van .NET Core 2.0 maakt met behulp van Visual Studio Code en de extensie Azure IoT rand. Begint u met een oplossing te maken en vervolgens het genereren van de eerste module in deze oplossing. Elke oplossing kan meerdere modules bevatten. 

1. Selecteer in Visual Studio Code **weergave** > **geïntegreerde Terminal**.
2. Voer de volgende opdracht om de meest recente versie van de sjabloon voor Azure IoT Edge-module installeren (of werk) voor Node.js in de geïntegreerde terminal:

   ```cmd/sh
   npm install -g yo generator-azure-iot-edge-module
   ```
3. Selecteer **View** > **Command Palette** in Visual Studio Code. 
4. Typ in het palet opdracht en voer de opdracht **Azure IoT rand: nieuwe IoT-oplossing voor rand**.

   ![Nieuwe Edge IoT-oplossing](./media/how-to-develop-csharp-module/new-solution.png)

5. Blader naar de map waar u de nieuwe oplossing maken en klikt u op **Selecteer map**. 
6. Geef een naam voor uw oplossing. 
7. Kies **Node.js Module** als de sjabloon voor de eerste module in de oplossing.
8. Geef een naam voor uw module. Kies een naam die uniek is binnen het register van de container. 
9. Geef de opslagplaats voor installatiekopieën voor de module. VS Code autopopulates de module-naam, dus u hoeft alleen te vervangen **localhost:5000** met uw eigen gegevens. Als u een lokale Docker-register gebruikt voor het testen, klikt u vervolgens functioneert localhost. Als u Azure Container register gebruikt, gebruikt u login-server uit het register-instellingen. De aanmeldingsserver ziet eruit als  **\<naam van routeringsregister\>. azurecr.io**.

VS-Code wordt de informatie die u verstrekt, maakt u een Edge van de IoT-oplossing en laadt in een nieuw venster.

Binnen de oplossing hebt u drie items: 
* Een **.vscode** map bevat foutopsporing configuraties.
* Een **modules** map bevat submappen voor elke module. Nu u slechts één hebt, maar u kunt meer toevoegen in het palet opdracht met de opdracht rechtermuisknop **Azure IoT rand: IoT rand Module toevoegen**. 
* Een **.env** bestand bevat de omgevingsvariabelen. Als u ACR als het register, rechts nu hebt u ACR-gebruikersnaam en wachtwoord in het. 
* Een **deployment.template.json** bestand bevat de nieuwe module samen met een steekproef **tempSensor** module dat gegevens die u gebruiken voor het testen van kunt simuleert. Zie voor meer informatie over hoe implementatie werk manifesten [begrijpen hoe IoT rand modules kunnen worden gebruikt, geconfigureerd en hergebruikt](module-composition.md).

## <a name="build-and-deploy-your-module-for-debugging"></a>Bouw en implementeer uw module voor foutopsporing

Er zijn meerdere Docker-bestanden voor andere containertypen in de modulemap van elke. U kunt een van deze bestanden die op de extensie eindigen **.debug** uw module voor het testen van maken. Op dit moment ondersteuning C#-modules alleen voor foutopsporing in linux amd64-containers.

1. Navigeer in VS-Code naar de `deployment.template.json` bestand. Vervang de Node.js-module createOptions in **deployment.template.json** met onderstaande inhoud en sla dit bestand: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

2. Typ in het palet van de opdracht tegenover Code en voer de opdracht **Azure IoT rand: bouwen IoT oplossing**.
3. Selecteer de `deployment.template.json` -bestand voor uw oplossing in het palet opdracht. 
4. In Azure IoT Hub-apparaten explorer met de rechtermuisknop op een apparaat-ID van IoT-rand en selecteer vervolgens **implementatie creëert voor IoT randapparaat**. 
5. Open de **config** map van uw oplossing, selecteer vervolgens de `deployment.json` bestand. Klik op **rand Deployment Manifest Selecteer**. 

Vervolgens ziet u dat de implementatie is gemaakt met een implementatie-ID in VS-Code wordt geïntegreerd terminal.

U kunt de status van de container in de VS Code Docker Verkenner of voer controleren de `docker ps` opdracht in de terminal.

## <a name="start-debugging-nodejs-module-in-vs-code"></a>Node.Js-module in de VS Code foutopsporing starten

VS Code houdt foutopsporing configuratie-informatie in een `launch.json` bestand zich in een `.vscode` map in uw werkruimte. Dit `launch.json` bestand is gegenereerd tijdens het maken van een nieuwe rand van de IoT-oplossing. Telkens wanneer u een nieuwe module die ondersteuning biedt voor foutopsporing toevoegt worden bijgewerkt. 

1. Navigeer naar de Code van de VS-weergave voor foutopsporing en selecteer het configuratiebestand van de foutopsporing voor de module.

2. Navigeer naar `app.js`. Een onderbrekingspunt in dit bestand toevoegen.

3. Klik op de **foutopsporing starten** of drukt u op **F5**, en selecteert u het proces om aan te koppelen.

4. VS Code fouten opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 

Het vorige voorbeeld laat zien hoe Node.js IoT Edge-modules voor containers voor foutopsporing. Deze poorten weergegeven in de module container createOptions toegevoegd. Nadat u foutopsporing van uw Node.js-modules, wordt u aangeraden dat u verwijdert deze poorten blootgestelde voor modules die gereed is voor productie IoT rand.

## <a name="next-steps"></a>Volgende stappen

Zodra u uw module die is gebouwd hebt, informatie over hoe [modules Azure IoT-rand implementeert vanuit Visual Studio Code](how-to-deploy-modules-vscode.md)


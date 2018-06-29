---
title: Fouten opsporen in modules van de functies voor Azure IoT rand | Microsoft Docs
description: Gebruik Visual Studio Code fouten opsporen in C# Azure werkt in combinatie met Azure IoT rand
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5ee5d368bde0f176442a01ba266791f8a9c2cbc3
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052824"
---
# <a name="use-visual-studio-code-to-develop-and-debug-azure-functions-for-azure-iot-edge"></a>Visual Studio Code te ontwikkelen en testen van Azure Functions voor Azure IoT Edge gebruiken

In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio (VS) Code](https://code.visualstudio.com/) fouten opsporen in uw Azure-functies op IoT rand.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT-randapparaat kan een andere fysieke apparaat of kunt u uw IoT-randapparaat simuleren op uw ontwikkelcomputer.

> [!NOTE]
> Deze zelfstudie foutopsporing wordt beschreven hoe koppelen van een proces in een module-container op te sporen met VS-Code. U kunt alleen fouten opsporen in C#-modules in linux amd64-containers. Als u niet bekend bent met de mogelijkheden voor foutopsporing van Visual Studio Code, leest u over [foutopsporing](https://code.visualstudio.com/Docs/editor/debugging). 

Dit artikel wordt Visual Studio Code gebruikt als de belangrijkste ontwikkelprogramma. VS-Code installeren en voer vervolgens de nodige uitbreidingen toevoegen: 

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C#-uitbreiding](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker-uitbreiding](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

U moet .NET voor het bouwen van de projectmap, Docker voor het bouwen van de installatiekopie van de module en een container register voor de installatiekopie van de module voor het maken van een module:
* [.NET core SDK 2.1](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/) op uw ontwikkelcomputer. 
* [Azure-Container register](https://docs.microsoft.com/azure/container-registry/) of [Docker-Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > U kunt een lokale Docker-register gebruiken voor prototype- en testdoeleinden, in plaats van een cloud-register. 

Als u wilt testen uw module op een apparaat, moet u een actieve iothub met ten minste één rand van de IoT-apparaat. Als u wilt uw computer te gebruiken als een Edge van de IoT-apparaat, kunt u doen door de stappen in de zelfstudies voor [Windows](quickstart.md) of [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Maak een nieuwe oplossingssjabloon

De volgende stappen laten zien hoe een rand van de IoT-oplossing met een C#-functiemodule maken. Elke oplossing kan meerdere modules bevatten.

1. Selecteer in Visual Studio Code **weergave** > **geïntegreerde Terminal**.
3. Selecteer **weergave** > **palet opdracht**.
4. Typ in het palet opdracht en voer de opdracht **Azure IoT rand: nieuwe IoT-oplossing voor rand**. 

   ![Nieuwe Edge IoT-oplossing](./media/how-to-develop-csharp-module/new-solution.png)

5. Blader naar de map waar u de nieuwe oplossing maken en klikt u op **Selecteer map**. 
6. Geef een naam voor uw oplossing. 
7. Kies **Azure Functions - C#** als de sjabloon voor de eerste module in de oplossing.
8. Geef een naam voor uw module. Kies een naam die uniek is binnen het register van de container. 
9. Geef de opslagplaats voor installatiekopieën voor de module. VS Code autopopulates de module-naam, dus u hoeft alleen te vervangen **localhost:5000** met uw eigen gegevens. Als u een lokale Docker-register gebruikt voor het testen, klikt u vervolgens functioneert localhost. Als u Azure Container register gebruikt, gebruikt u login-server uit het register-instellingen. De aanmeldingsserver ziet eruit als  **\<naam van routeringsregister\>. azurecr.io**.

VS-Code wordt de informatie die u verstrekt, maakt u een Edge van de IoT-oplossing met een functie-project en laadt in een nieuw venster.

Binnen de oplossing hebt u drie items: 

* Een **.vscode** map met foutopsporing configuraties.
* Een **modules** map submappen voor elke module bevat. Nu u slechts één hebt, maar u kunt meer toevoegen via het palet opdracht met de opdracht rechtermuisknop **Azure IoT rand: IoT rand Module toevoegen**.
* Een **.env** bestand bevat de omgevingsvariabelen. Als u ACR als het register, rechts nu hebt u ACR-gebruikersnaam en wachtwoord in het. 
* Een **deployment.template.json** bestand bevat de nieuwe module samen met een steekproef **tempSensor** module waarmee gegevens die u gebruiken voor het testen van kunt simuleert. Zie voor meer informatie over hoe implementatie werk manifesten [begrijpen hoe IoT rand modules kunnen worden gebruikt, geconfigureerd en hergebruikt](module-composition.md).

## <a name="build-your-iot-edge-function-module-for-debugging-purpose"></a>Uw IoT rand functie-module voor het opsporen van doel maken
1. Voor foutopsporing starten, moet u gebruiken **Dockerfile.amd64.debug** naar uw docker-installatiekopie bouwen en implementeren van uw oplossing opnieuw. Navigeer in VS-Code Verkenner naar `deployment.template.json` bestand. De afbeeldings-URL van de functie bijwerken door toe te voegen een `.debug` in het einde.

    ![Foutopsporing installatiekopie maken](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Uw oplossing opnieuw worden opgebouwd. Typ in de VS Code opdracht palet en voer de opdracht **Azure IoT rand: bouwen IoT oplossing**.
3. In Azure IoT Hub-apparaten explorer met de rechtermuisknop op een apparaat-ID van IoT-rand en selecteer vervolgens **implementatie creëert voor randapparaat**. Selecteer de `deployment.json` bestand onder de `config` map. Vervolgens ziet u dat de implementatie is gemaakt met een implementatie-ID in VS-Code wordt geïntegreerd terminal.

U kunt de status van de container in de VS Code Docker explorer of door te voeren controleren de `docker images` opdracht in de terminal.

## <a name="start-debugging-c-function-in-vs-code"></a>C#-functie in VS-Code foutopsporing starten
1. VS Code houdt foutopsporing configuratie-informatie in een `launch.json` bestand zich in een `.vscode` map in uw werkruimte. Dit `launch.json` bestand is gegenereerd tijdens het maken van een nieuwe rand van de IoT-oplossing. Telkens wanneer u een nieuwe module die ondersteuning biedt voor foutopsporing toevoegt worden bijgewerkt. Navigeer naar de weergave voor foutopsporing en selecteer het bijbehorende configuratiebestand voor foutopsporing. De naam van de optie foutopsporing moet zoals **ModuleName Remote Debug (.NET Core)** ![Selecteer Foutopsporing configuratie](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navigeer naar `run.csx`. Een onderbrekingspunt in de functie toevoegen.
3. Klik op de **foutopsporing starten** of drukt u op **F5**, en selecteert u het proces om aan te koppelen.
4. VS Code fouten opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 


> [!NOTE]
> Het bovenstaande voorbeeld wordt getoond hoe u foutopsporing .net Core IoT Edge-functie op containers. Deze gebaseerd op de foutopsporingsversie van de `Dockerfile.amd64.debug`, waaronder VSDBG (het opdrachtregelprogramma .NET Core-foutopsporingsprogramma) in uw installatiekopie container tijdens het opbouwen van het. We raden u rechtstreeks gebruiken of aanpassen aan de `Dockerfile` zonder VSDBG voor gereed is voor productie IoT rand functie als u klaar bent met foutopsporing van uw C#-functie.

## <a name="next-steps"></a>Volgende stappen

Zodra u uw module die is gebouwd hebt, informatie over hoe [modules Azure IoT-rand implementeert vanuit Visual Studio Code](how-to-deploy-modules-vscode.md)

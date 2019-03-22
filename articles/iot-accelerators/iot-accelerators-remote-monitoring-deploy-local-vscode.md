---
title: Implementeren van de oplossing voor externe controle lokaal (Visual Studio Code) - Azure | Microsoft Docs
description: In deze gebruiksaanwijzing laat zien hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer met behulp van Visual Studio Code voor het testen en ontwikkeling.
author: avneet723
manager: hegate
ms.author: avneet723
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2019
ms.topic: conceptual
ms.openlocfilehash: 946f815cecea4cc172fac35c0b260d795317e6e1
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316215"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---visual-studio-code"></a>De bewaking op afstand oplossingsversnellers implementeren lokaal - Visual Studio Code

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

In dit artikel leest u hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer voor het testen en ontwikkeling. Leert u hoe u kunt de microservices in Visual Studio Code uitvoeren. Een lokale microservices-implementatie maakt gebruik van de volgende cloudservices: IoT-Hub, Cosmos DB, Azure stream Analytics en Azure Time Series Insights.

## <a name="prerequisites"></a>Vereisten

Voor het implementeren van de Azure-services die worden gebruikt door de oplossingsverbetering voor externe controle, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

### <a name="machine-setup"></a>Machine-instellingen

Voor het voltooien van de lokale implementatie, moet u de volgende hulpprogramma's geïnstalleerd op uw lokale ontwikkelcomputer:

* [Git](https://git-scm.com/)
* [.NET Core](https://dotnet.microsoft.com/download)
* [Docker](https://www.docker.com)
* [Nginx](https://nginx.org/en/download.html)
* [Visual Studio Code](https://code.visualstudio.com/)
* [VS-Code C# extensie](https://code.visualstudio.com/docs/languages/csharp)
* [Node.js v8](https://nodejs.org/) -deze software is een vereiste voor de CLI PCS die de scripts gebruiken voor het maken van Azure-resources. Gebruik geen Node.js v10

> [!NOTE]
> Visual Studio Code is beschikbaar voor Windows, Mac- en Ubuntu.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices"></a>Voer de microservices

In deze sectie maakt uitvoeren u de microservices bewaking op afstand. U hebt de web-UI systeemeigen uitgevoerd, de Apparaatsimulatie-service in Docker en de microservices in Visual Studio Code.

### <a name="build-the-code"></a>De code bouwen

Navigeer naar azure-iot-pcs-remote-monitoring-dotnet\services uit achter de opdrachtprompt en voer de volgende opdrachten om de code te bouwen.

```cmd
dotnet restore
dotnet build -c Release
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Implementatie van alle andere microservices op lokale computer

De volgende stappen laten zien hoe u de microservices bewaking op afstand uitvoeren in Visual Studio 2017:

1. Start Visual Studio Code.
1. Open de **azure-iot-pcs-remote-monitoring-dotnet** module op basis van de lokale kopie in VS Code.
1. Kopieer de bestanden **launch.json** en **tasks.json** van scripts\local\launch\idesettings\vscode\. Maak een nieuwe map **azure-iot-pcs-remote-monitoring-dotnet\.vscode** en plak er de bestanden.
1. Open het deelvenster Foutopsporing in VS Code en voer de **alle microservices worden uitgevoerd** configuratie. Deze configuratie wordt uitgevoerd de simulatie apparaat microservices in Docker en voert de andere microservices in het foutopsporingsprogramma.

Een voorbeeld: de uitvoer voor **Auth** service in de **Foutopsporingsconsole** er als volgt uit:

[![Deploy-Local-Auth-Service](./media/deploy-locally-vscode/auth-debug-results-inline.png)](./media/deploy-locally-vscode/auth-debug-results-expanded.png#lightbox)

### <a name="run-the-web-ui"></a>Uitvoeren van de web-UI

In deze stap start u de web-UI. Navigeer naar **azure-iot-pcs-remote-monitoring-dotnet\webui** map in uw lokale kopiëren en voer de volgende opdrachten uit:

```cmd
npm install
npm start
```

Wanneer het begin voltooid is, wordt de pagina in uw browser weergegeven **http:\//localhost:3000 / dashboard**. De fouten op deze pagina worden verwacht. Als u de toepassing zonder fouten, voer de volgende stap.

### <a name="configure-and-run-nginx"></a>Configureren en uitvoeren van NGINX

Instellen van een reverse proxy-server om de web-App en microservices die worden uitgevoerd op uw lokale computer te koppelen:

* Kopieer de **nginx.conf** -bestand uit de **webui\scripts\localhost** map die u wilt de **nginx\conf** directory installeren.
* Voer **nginx**.

Voor meer informatie over het uitvoeren **nginx**, Zie [nginx voor Windows](https://nginx.org/en/docs/windows.html).

### <a name="connect-to-the-dashboard"></a>Verbinding maken met het dashboard

Voor toegang tot het oplossingsdashboard voor externe controle, gaat u naar http:\//localhost:9000 in uw browser.

## <a name="clean-up"></a>Opruimen

Om te voorkomen dat onnodige verwijderen kosten, wanneer u klaar bent met de test cloudservices uit uw Azure-abonnement. Als u wilt verwijderen van de services, gaat u naar de [Azure-portal](https://ms.portal.azure.com) en verwijder de resource-groep die de **start.cmd** script dat is gemaakt.

U kunt ook de lokale kopie van de bewaking op afstand opslagplaats gemaakt wanneer u de broncode van GitHub gekloond verwijderen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het oplossingsdashboard](quickstart-remote-monitoring-deploy.md).

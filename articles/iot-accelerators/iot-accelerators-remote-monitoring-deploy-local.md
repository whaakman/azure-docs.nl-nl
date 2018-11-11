---
title: Implementeer de oplossing voor externe controle lokaal (via Visual Studio IDE) - Azure | Microsoft Docs
description: In deze gebruiksaanwijzing laat zien hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer met behulp van Visual Studio voor het testen en ontwikkeling.
author: avneet723
manager: hegate
ms.author: avneet723
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: 5068f0277726b7c468aa24d0629c4350b60b78b5
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51287605"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---visual-studio"></a>De bewaking op afstand oplossingsversnellers implementeren lokaal - Visual Studio

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

In dit artikel leest u hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer voor het testen en ontwikkeling. U informatie over het uitvoeren van de microservices in Visual Studio. Een lokale microservices-implementatie maakt gebruik van de volgende cloudservices: IoT Hub, Cosmos DB, Azure stream Analytics en Azure Time Series Insights-services in de cloud.

Als u wilt dat de oplossingsverbetering voor externe controle uitvoeren in Docker op uw lokale computer, raadpleegt u [Remote Monitoring solution accelerator lokaal - Docker implementeren](iot-accelerators-remote-monitoring-deploy-local-docker.md).

## <a name="prerequisites"></a>Vereisten

Voor het implementeren van de Azure-services die worden gebruikt door de oplossingsverbetering voor externe controle, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

### <a name="machine-setup"></a>Machine-instellingen

Voor het voltooien van de lokale implementatie, moet u de volgende hulpprogramma's geïnstalleerd op uw lokale ontwikkelcomputer:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Visual Studio](https://visualstudio.microsoft.com/)
* [Nginx](http://nginx.org/en/download.html)
* [Node.js v8](https://nodejs.org/) -deze software is een vereiste voor de CLI PCS die de scripts gebruiken voor het maken van Azure-resources. Gebruik geen Node.js v10.

> [!NOTE]
> Visual Studio is beschikbaar voor Windows en Mac.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices"></a>Voer de microservices

In deze sectie maakt uitvoeren u de microservices bewaking op afstand. U hebt de web-UI systeemeigen uitgevoerd, de Apparaatsimulatie-service in Docker en de microservices in Visual Studio.

### <a name="run-the-web-ui"></a>Uitvoeren van de web-UI

In deze stap start u de web-UI. Navigeer naar de **webinterface** map in uw lokale exemplaar van de opslagplaats en voer de volgende opdrachten uit:

```cmd
npm install
npm start
```

### <a name="run-the-device-simulation-service"></a>Uitvoeren van de service van de simulatie van apparaat

Voer de volgende opdracht voor het starten van de Docker-container voor de service van de simulatie apparaat. De service simuleert apparaten voor de oplossing voor externe controle.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Implementatie van alle andere microservices op lokale computer

De volgende stappen laten zien hoe u de microservices bewaking op afstand uitvoeren in Visual Studio 2017:

1. Start Visual Studio 2017
1. Open de **extern monitoring.sln** oplossing in de **services** map in de lokale kopie van de opslagplaats.
1. In **Solution Explorer**, met de rechtermuisknop op de oplossing en klik op **eigenschappen**.
1. Selecteer **algemene eigenschappen > Startup Project**.
1. Selecteer **meerdere opstartprojecten** en stel **actie** naar **Start** voor de volgende projecten:
    * WebService (asa-manager\WebService)
    * WebService (auth\WebService)
    * WebService (config\WebService)
    * WebService (apparaat-telemetry\WebService)
    * WebService (iothub-manager\WebService)
    * WebService (opslag-adapter\WebService)
1. Klik op **OK** om op te slaan, uw keuzes.
1. Klik op **fouten opsporen > Foutopsporing starten** wilt bouwen en uitvoeren van de webservices op de lokale computer.

Elke webservice wordt een opdrachtprompt en web-browservenster geopend. Bij de opdrachtprompt, ziet u uitvoer van de service en het browservenster kunt u de status controleren. Sluit niet de opdracht prompts of webpagina's, met deze actie wordt de web-service gestopt.

### <a name="start-the-stream-analytics-job"></a>De Stream Analytics-taak starten

Volg deze stappen voor het starten van de Stream Analytics-taak:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
1. Navigeer naar de **resourcegroep** gemaakt voor uw oplossing. De naam van de resourcegroep is de naam die u voor uw oplossing hebt gekozen bij het uitvoeren van de **start.cmd** script **.
1. Klik op de **Stream Analytics-taak** in de lijst met resources.
1. Op de Stream Analytics-taak **overzicht** pagina, klikt u op de **Start** knop. Klik vervolgens op **Start** de taak om nu te starten.

### <a name="configure-and-run-nginx"></a>Configureren en uitvoeren van NGINX

Instellen van een reverse proxy-server om de web-App en microservices die worden uitgevoerd op uw lokale computer te koppelen:

* Kopieer de **nginx.conf** -bestand uit de **webui\scripts\localhost** map die u wilt de **nginx\conf** directory installeren.
* Voer **nginx**.

Voor meer informatie over het uitvoeren **nginx**, Zie [nginx voor Windows](http://nginx.org/en/docs/windows.html).

### <a name="connect-to-the-dashboard"></a>Verbinding maken met het dashboard

Voor toegang tot het oplossingsdashboard voor externe controle, gaat u naar [ http://localhost:9000 ](http://localhost:9000) in uw browser.

## <a name="clean-up"></a>Opruimen

Om te voorkomen dat onnodige verwijderen kosten, wanneer u klaar bent met de test cloudservices uit uw Azure-abonnement. Als u wilt verwijderen van de services, gaat u naar de [Azure-portal](https://ms.portal.azure.com) en verwijder de resource-groep die de **start.cmd** script dat is gemaakt.

U kunt ook de lokale kopie van de bewaking op afstand opslagplaats gemaakt wanneer u de broncode van GitHub gekloond verwijderen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het oplossingsdashboard](quickstart-remote-monitoring-deploy.md).

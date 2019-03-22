---
title: Implementeer de oplossing voor externe controle lokaal (via IntelliJ IDE) - Azure | Microsoft Docs
description: In deze gebruiksaanwijzing laat zien hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer met behulp van IntelliJ voor testen en ontwikkeling.
author: v-krghan
manager: dominicbetts
ms.author: v-krghan
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/24/2019
ms.topic: conceptual
ms.openlocfilehash: ccdb3e16ea60cf85ae28e533e3b2d9f473cc90c8
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316385"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---intellij"></a>De bewaking op afstand oplossingsversnellers implementeren lokaal - IntelliJ

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

In dit artikel leest u hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer voor het testen en ontwikkeling. Leert u hoe u de microservices in IntelliJ uitvoert. Een lokale microservices-implementatie maakt gebruik van de volgende cloudservices: IoT Hub, Cosmos DB, Azure stream Analytics en Azure Time Series Insights-services in de cloud.

Als u wilt dat de oplossingsverbetering voor externe controle uitvoeren in Docker op uw lokale computer, raadpleegt u [Remote Monitoring solution accelerator lokaal - Docker implementeren](iot-accelerators-remote-monitoring-deploy-local-docker.md).

## <a name="prerequisites"></a>Vereisten

Voor het implementeren van de Azure-services die worden gebruikt door de oplossingsverbetering voor externe controle, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

### <a name="machine-setup"></a>Machine-instellingen

Voor het voltooien van de lokale implementatie, moet u de volgende hulpprogramma's geïnstalleerd op uw lokale ontwikkelcomputer:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [IntelliJ Community-editie](https://www.jetbrains.com/idea/download/)
* [IntelliJ Plugin Scala](https://plugins.jetbrains.com/plugin/1347-scala)
* [IntelliJ Plugin SBT](https://plugins.jetbrains.com/plugin/5007-sbt)
* [Invoegtoepassing voor IntelliJ SBT Executor](https://plugins.jetbrains.com/plugin/7247-sbt-executor)
* [Nginx](https://nginx.org/en/download.html)
* [Node.js v8](https://nodejs.org/) -deze software is een vereiste voor de CLI PCS die de scripts gebruiken voor het maken van Azure-resources. Gebruik geen Node.js v10.

> [!NOTE]
> IntelliJ IDE is beschikbaar voor Windows en Mac.

[!INCLUDE [iot-accelerators-local-setup-java](../../includes/iot-accelerators-local-setup-java.md)]

## <a name="run-the-microservices"></a>Voer de microservices

In deze sectie maakt uitvoeren u de microservices bewaking op afstand. U hebt de web-UI systeemeigen uitgevoerd, de Apparaatsimulatie Auth en ASA Manager-service in Docker en de microservices in IntelliJ.

### <a name="run-the-device-simulation-service"></a>Uitvoeren van de service van de simulatie van apparaat

Open een nieuw opdrachtpromptvenster om er zeker van te zijn dat u toegang hebt tot de omgevingsvariabelen ingesteld door de **start.cmd** script in de vorige sectie.

Voer de volgende opdracht voor het starten van de Docker-container voor de service van de simulatie apparaat. De service simuleert apparaten voor de oplossing voor externe controle.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="run-the-auth-service"></a>Uitvoeren van de service Auth

Open een nieuwe opdrachtprompt-venster en voer de volgende opdracht om te starten van de Docker-container voor de Authentication service. De service kunnen voor het beheren van de gebruikers die zijn gemachtigd voor toegang tot Azure IoT-oplossingen.

```cmd
<path_to_cloned_repository>\services\auth\scripts\docker\run.cmd
```

### <a name="run-the-asa-manager-service"></a>De ASA-Manager-service wordt uitgevoerd

Open een nieuwe opdrachtprompt-venster en voer de volgende opdracht om te starten van de Docker-container voor de ASA-Manager-service. De service kunt het beheer van Azure Stream Analytics (ASA)-taken, met inbegrip van de configuratie van de instelling en starten, stoppen en hun status controleren.

```cmd
<path_to_cloned_repository>\services\asa-manager\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Implementatie van alle andere microservices op lokale computer

De volgende stappen laten zien hoe u de bewaking op afstand microservices in IntelliJ uitvoeren:

#### <a name="import-project"></a>Project importeren

1. IntelliJ IDE starten
1. Selecteer **Project importeren** en kies **azure-iot-pcs-remote-monitoring-java\services\build.sbt**

#### <a name="create-run-configurations"></a>Voer configuraties maken

1. Selecteer **uitvoeren > configuraties bewerken**
1. Selecteer **nieuwe configuratie toevoegen > sbt taak** 
1. Voer **naam** en voer **taken** uitvoeren 
1. Selecteer de **werkmap** op basis van de service die u wilt uitvoeren
1. Klik op **toepassen > Ok** om op te slaan, uw keuzes.
1. Maak uitvoeren configuraties voor de volgende services:
    * WebService (services\config)
    * WebService (services\device-telemetrie)
    * WebService (services\iothub-manager)
    * WebService (services\storage-adapter)

Als voorbeeld de volgende afbeelding toont de configuratie voor een service toe te voegen:

[![Add-Configuration](./media/deploy-locally-intelliJ/run-configurations.png)](./media/deploy-locally-intelliJ/run-configurations.png#lightbox)


#### <a name="create-compound-configuration"></a>Samengestelde configuratie maken

1. Als u wilt alle Services worden uitgevoerd, selecteert u samen **nieuwe configuratie toevoegen > samengestelde**
1. Voer de **naam** en **sbt taken toevoegen**
1. Klik op **toepassen > Ok** om op te slaan, uw keuzes.

Als u bijvoorbeeld de volgende afbeelding ziet u alle sbt taken toe te voegen aan één configuratie:


[![Add-All-Services](./media/deploy-locally-intelliJ/all-services.png)](./media/deploy-locally-intelliJ/all-services.png#lightbox)



1. Klik op **uitvoeren** wilt bouwen en uitvoeren van de webservices op de lokale computer.

Elke webservice wordt een opdrachtprompt en web-browservenster geopend. Bij de opdrachtprompt, ziet u uitvoer van de service en het browservenster kunt u de status controleren. Sluit niet de opdracht prompts of webpagina's, met deze actie wordt de web-service gestopt.


Voor toegang tot de status van de services, kunt u navigeren naar de volgende URL's:
* IoT-Hub Manager [http://localhost:9002/v1/status](http://localhost:9002/v1/status)
* Telemetrie van apparaten  [http://localhost:9004/v1/status](http://localhost:9004/v1/status)
* configuratie [http://localhost:9005/v1/status](http://localhost:9005/v1/status)
* opslag-adapter [http://localhost:9022/v1/status](http://localhost:9022/v1/status)


### <a name="start-the-stream-analytics-job"></a>De Stream Analytics-taak starten

Volg deze stappen voor het starten van de Stream Analytics-taak:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
1. Navigeer naar de **resourcegroep** gemaakt voor uw oplossing. De naam van de resourcegroep is de naam die u voor uw oplossing hebt gekozen bij het uitvoeren van de **start.cmd** script **.
1. Klik op de **Stream Analytics-taak** in de lijst met resources.
1. Op de Stream Analytics-taak **overzicht** pagina, klikt u op de **Start** knop. Klik vervolgens op **Start** de taak om nu te starten.

### <a name="run-the-web-ui"></a>Uitvoeren van de web-UI

In deze stap start u de web-UI. Open een nieuw opdrachtpromptvenster om er zeker van te zijn dat u toegang hebt tot de omgevingsvariabelen ingesteld door de **start.cmd** script. Navigeer naar de **webinterface** map in uw lokale exemplaar van de opslagplaats en voer de volgende opdrachten uit:

```cmd
npm install
npm start
```

Wanneer het begin voltooid is, wordt de pagina in uw browser weergegeven **http:\//localhost:3000 / dashboard**. De fouten op deze pagina worden verwacht. Als u de toepassing zonder fouten, voer de volgende stap.

### <a name="configure-and-run-nginx"></a>Configureren en uitvoeren van NGINX

Instellen van een reverse proxy-server om de web-App en microservices die worden uitgevoerd op uw lokale computer te koppelen:

* Kopieer de **nginx.conf** -bestand uit de **webui\scripts\localhost** map in de lokale kopie van de opslagplaats naar de **nginx\conf** directory installeren.
* Voer **nginx**.

Voor meer informatie over het uitvoeren **nginx**, Zie [nginx voor Windows](https://nginx.org/en/docs/windows.html).

### <a name="connect-to-the-dashboard"></a>Verbinding maken met het dashboard

Voor toegang tot het oplossingsdashboard voor externe controle, gaat u naar http:\//localhost:9000 in uw browser.

## <a name="clean-up"></a>Opruimen

Om te voorkomen dat onnodige verwijderen kosten, wanneer u klaar bent met de test cloudservices uit uw Azure-abonnement. Als u wilt verwijderen van de services, gaat u naar de [Azure-portal](https://ms.portal.azure.com) en verwijder de resource-groep die de **start.cmd** script dat is gemaakt.

U kunt ook de lokale kopie van de bewaking op afstand opslagplaats gemaakt wanneer u de broncode van GitHub gekloond verwijderen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het oplossingsdashboard](quickstart-remote-monitoring-deploy.md).

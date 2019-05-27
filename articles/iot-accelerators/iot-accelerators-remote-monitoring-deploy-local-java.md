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
ms.openlocfilehash: 2b55fea69fe1affb6cab5d360f1e8355c3bb720d
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "66015433"
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

## <a name="download-the-source-code"></a>De broncode downloaden

De bewaking op afstand broncodeopslagplaatsen bevatten de broncode en de Docker-configuratiebestanden die u nodig hebt voor het uitvoeren van de microservices Docker-installatiekopieën.

Klonen en maken van een lokale versie van de opslagplaats, uw opdrachtregel omgeving te gebruiken om te navigeren naar een geschikte map op uw lokale computer. Voer vervolgens een van de volgende sets opdrachten om de java-opslagplaats te klonen:

Voor het downloaden van de meest recente versie van de java-microservices-implementaties, voert u de volgende uit:


```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Deze opdrachten download de broncode voor de microservices naast de scripts die u kunt de microservices lokaal uitvoeren. Hoewel u niet de broncode voor het uitvoeren van de microservices in Docker nodig hebt, is de broncode is handig als u later wilt wijzigen van de oplossingsversnellers en uw wijzigingen lokaal testen.

## <a name="deploy-the-azure-services"></a>De Azure-services implementeren

Hoewel dit artikel u hoe u de microservices lokaal uitvoert leest, worden ze vertrouwen op Azure-services die worden uitgevoerd in de cloud. Het volgende script gebruiken voor het implementeren van de Azure-services. Het volgende scriptvoorbeelden wordt ervan uitgegaan dat u de java-opslagplaats op een Windows-machine. Als u in een andere omgeving werkt, aanpassen op de juiste wijze de paden, bestandsextensies en padscheidingstekens.

### <a name="create-new-azure-resources"></a>Nieuwe Azure-resources maken

Als u de vereiste Azure-resources is nog niet hebt gemaakt, volgt u deze stappen:

1. In de opdrachtregel-omgeving, gaat u naar de **\services\scripts\local\launch** map in de gekloonde kopie van de opslagplaats.

1. Voer de volgende opdrachten voor het installeren van de **pc's** CLI-hulpprogramma en meld u aan bij uw Azure-account:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Voer de **start.cmd** script. Het script vraagt u om de volgende informatie:
   * De naam van een oplossing.
   * Het te gebruiken Azure-abonnement.
   * De locatie van het Azure-datacenter te gebruiken.

     Het script wordt de resourcegroep gemaakt in Azure met de naam van uw oplossing. Deze resourcegroep bevat de Azure-resources die maakt gebruik van de solution accelerator. Nadat u de bijbehorende resources niet meer nodig hebt, kunt u deze resourcegroep verwijderen.

     Het script wordt ook een set van omgevingsvariabelen met een voorvoegsel toegevoegd **pc's** naar uw lokale computer. Deze omgevingsvariabelen Geef de details voor externe bewaking om u te kunnen worden gelezen uit een Azure Key Vault-resource. Deze Key Vault-resource is waar bewaking op afstand de configuratiewaarden van wordt gelezen.

     > [!TIP]
     > Wanneer het script is voltooid, ook wordt opgeslagen in de omgevingsvariabelen van een bestand met de naam  **\<uw basismap\>\\.pcs\\\<oplossingsnaam\>.env** . U kunt deze gebruiken voor toekomstige solution accelerator-implementaties. Houd er rekening mee dat omgevingsvariabelen instellen op uw lokale computer, overschrijven de waarden in de **services\\scripts\\lokale\\.env** tijdens het uitvoeren van het bestand **docker-compose**.

1. Afsluiten van uw opdrachtregelomgeving.

### <a name="use-existing-azure-resources"></a>Bestaande Azure-bronnen

Als u de vereiste Azure-resources al hebt gemaakt, maakt u de bijbehorende omgevingsvariabelen op uw lokale computer.
Stel de omgevingsvariabelen voor het volgende:
* **PCS_KEYVAULT_NAME** -naam van de Azure Key Vault-resource
* **PCS_AAD_APPID** -de AAD-toepassing-ID
* **PCS_AAD_APPSECRET** -geheim van de AAD-toepassing

Configuratiewaarden worden gelezen uit de Azure Key Vault-resource. Deze omgevingsvariabelen kunnen worden opgeslagen de  **\<uw basismap\>\\.pcs\\\<oplossingsnaam\>.env** -bestand van de implementatie. Houd er rekening mee dat omgevingsvariabelen instellen op uw lokale computer, overschrijven de waarden in de **services\\scripts\\lokale\\.env** tijdens het uitvoeren van het bestand **docker-compose**.

Bepaalde onderdelen van de configuratie die nodig zijn voor de microservices is opgeslagen in een exemplaar van **Key Vault** die is gemaakt op de eerste implementatie. De bijbehorende variabelen in Key Vault moeten worden gewijzigd, indien nodig.

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

Als u bijvoorbeeld de volgende afbeelding toont de configuratie voor een service toevoegen:

[![Add-Configuration](./media/deploy-locally-intellij/run-configurations.png)](./media/deploy-locally-intellij/run-configurations.png#lightbox)


#### <a name="create-compound-configuration"></a>Samengestelde configuratie maken

1. Als u wilt alle Services worden uitgevoerd, selecteert u samen **nieuwe configuratie toevoegen > samengestelde**
1. Voer de **naam** en **sbt taken toevoegen**
1. Klik op **toepassen > Ok** om op te slaan, uw keuzes.

Als u bijvoorbeeld de volgende afbeelding toont alle sbt taken toe te voegen aan één configuratie:

[![Add-All-Services](./media/deploy-locally-intellij/all-services.png)](./media/deploy-locally-intellij/all-services.png#lightbox)

Klik op **uitvoeren** wilt bouwen en uitvoeren van de webservices op de lokale computer.

Elke webservice wordt een opdrachtprompt en web-browservenster geopend. Bij de opdrachtprompt, ziet u uitvoer van de service en het browservenster kunt u de status controleren. Sluit niet de opdracht prompts of webpagina's, met deze actie wordt de web-service gestopt.


Voor toegang tot de status van de services, kunt u navigeren naar de volgende URL's:
* IoT-Hub Manager [http://localhost:9002/v1/status](http://localhost:9002/v1/status)
* Telemetrie van apparaten  [http://localhost:9004/v1/status](http://localhost:9004/v1/status)
* configuratie [http://localhost:9005/v1/status](http://localhost:9005/v1/status)
* opslag-adapter [http://localhost:9022/v1/status](http://localhost:9022/v1/status)


### <a name="start-the-stream-analytics-job"></a>De Stream Analytics-taak starten

Volg deze stappen voor het starten van de Stream Analytics-taak:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
1. Navigeer naar de **resourcegroep** gemaakt voor uw oplossing. De naam van de resourcegroep is de naam die u voor uw oplossing hebt gekozen bij het uitvoeren van de **start.cmd** script.
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

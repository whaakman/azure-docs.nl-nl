---
title: Azure IoT Edge continue integratie en continue implementatie | Microsoft Docs
description: Overzicht van de continue integratie en continue implementatie voor Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 11/12/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 06dec64a55aaece4cd67ebf0485e34aa206a8936
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633730"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Continue integratie en continue implementatie voor Azure IoT Edge

U kunt eenvoudig inzetten van DevOps met uw Azure IoT Edge-toepassingen met [Azure IoT Edge voor Azure pijplijnen](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) of [Azure IoT Edge-invoegtoepassing voor Jenkins](https://plugins.jenkins.io/azure-iot-edge). In dit artikel laat zien hoe u kunt de continue integratie en continue implementatiefuncties van Azure-pijplijnen en Microsoft Team Foundation Server (TFS) om te bouwen, testen en implementeren van toepassingen snel en efficiënt aan uw Azure IoT Edge. 

In dit artikel leert u hoe u:
* Maken en controleren in een voorbeeld van een IoT Edge-oplossing.
* Azure IoT Edge-extensie voor uw Azure DevOps installeren.
* Configureer continue integratie (CI) om de oplossing te bouwen.
* Configureer continue implementatie (CD) voor het implementeren van de oplossing en antwoorden weer.

Het duurt 30 minuten om de stappen in dit artikel te voltooien.

![CI en CD](./media/how-to-ci-cd/cd.png)


## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Een voorbeeld van Azure IoT Edge-oplossing met behulp van Visual Studio Code maken

In deze sectie maakt u een voorbeeld van een IoT Edge oplossing met eenheidstests dat kan worden uitgevoerd als onderdeel van het bouwproces. Voordat u de instructies in deze sectie, voer de stappen in [een IoT Edge-oplossing met meerdere modules in Visual Studio Code ontwikkelen](tutorial-multiple-modules-in-vscode.md).

1. Typ in het opdrachtenpalet van VS Code, en voer de opdracht **Azure IoT Edge: nieuwe IoT-Edge-oplossing**. Selecteer de werkruimtemap van uw, geeft u de oplossingsnaam (de standaardnaam is **EdgeSolution**), en maakt u een C#-Module (**FilterModule**) als de eerste gebruikersmodule in deze oplossing. U moet ook de opslagplaats voor Dockerinstallatiekopieën opgeven voor de eerste module. De standaard-opslagplaats voor installatiekopieën is gebaseerd op een lokale Docker-register (`localhost:5000/filtermodule`). U moet dit wijzigen in Azure Container Registry (`<your container registry address>/filtermodule`) of Docker-Hub voor verdere continue integratie.

    ![ACR instellen](./media/how-to-ci-cd/acr.png)

2. Het venster VS Code, de werkruimte van uw IoT Edge-oplossing wordt geladen. U kunt eventueel typt en uitvoeren **Azure IoT Edge: toevoegen aan IoT Edge module** om toe te voegen meer modules. Er is een `modules` map, een `.vscode` map en een manifest-sjabloonbestand van de implementatie in de hoofdmap. Alle gebruiker module codes zijn submappen onder de map `modules`. De `deployment.template.json` is het manifest sjabloon voor de implementatie. Sommige van de parameters in dit bestand wordt geparseerd uit de `module.json`, waar zich bevindt in de modulemap voor elke.

3. Uw voorbeeld IoT Edge-oplossing is nu gereed. De standaard C#-module fungeert als een pipe bericht-module. In de `deployment.template.json`, ziet u deze oplossing bevat twee modules. Het bericht wordt gegenereerd op basis van de `tempSensor` -module, en wordt rechtstreeks kan worden doorgesluisd `FilterModule`, vervolgens naar uw IoT-hub zijn verzonden.

4. Sla deze projecten en bekijk het in de opslagplaats van uw Azure-opslagplaatsen of TFS.
    
> [!NOTE]
> Zie voor meer informatie over het gebruik van Azure-opslagplaatsen [deel van uw code met Visual Studio en Azure-opslagplaatsen](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-azure-pipelines-for-continuous-integration"></a>Azure-pijplijnen voor continue integratie configureren
In deze sectie maakt u een build-pijplijn die is geconfigureerd voor het automatisch uitgevoerd wanneer u op wijzigingen in het voorbeeld IoT Edge-oplossing controleren en deze build-Logboeken in Azure pijplijnen weergegeven.

1. Meld u aan bij uw organisatie Azure DevOps ( **https://dev.azure.com/{your organisatie} /**) en open het project waarin u dit selectievakje is ingeschakeld in de voorbeeld-app.

    ![De code is ingecheckt](./media/how-to-ci-cd/init-project.png)

1. Ga naar [Azure IoT Edge voor Azure pijplijnen](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) op Azure DevOps-Marketplace. Klik op **gratis downloaden** en volg de wizard voor het installeren van deze extensie voor uw Azure DevOps-organisatie of downloaden naar uw TFS.

    ![De extensie installeren](./media/how-to-ci-cd/install-extension.png)

1. Open in uw Azure-pijplijnen, de **Build & Release** hub en klik in de **bouwt** tabblad **+ nieuwe pijplijn**. Of, als u bouwen van pijplijnen al hebt, kiest u de **+ nieuw** knop.

    ![Nieuwe pijplijn](./media/how-to-ci-cd/add-new-build.png)

1. Als u hierom wordt gevraagd, selecteert u **Git** het brontype. Selecteer vervolgens het project, de opslagplaats en de vertakking waar uw code zich bevindt. Kies **blijven**.

    ![Git selecteren](./media/how-to-ci-cd/select-vsts-git.png)

    In **selecteert u een sjabloon** venster, kiest u **beginnen met een lege proces**.

    ![Selecteer een sjabloon](./media/how-to-ci-cd/start-with-empty.png)

1. Kies in de pijplijneditor de agent-pool. 
    
    * Als u maken van de modules in platform amd64 voor Linux-containers wilt, kiest u **Ubuntu 1604 die worden gehost**
    * Als u maken van de modules in platform amd64 voor Windows-containers wilt, kiest u **VS2017 die worden gehost** 
    * Als u uw modules in platform arm32v7 voor Linux-containers maken wilt, moet u instellen van uw eigen bouwagent door te klikken op de **beheren** knop.
    
    ![Build-agent configureren](./media/how-to-ci-cd/configure-env.png)

1. In de Agent-taak, klikt u op '+' om toe te voegen twee taken in de build-pijplijn. De eerste die afkomstig is van **Azure IoT Edge**. En de tweede waarde is van **Build-artefacten publiceren**
    
    ![Taken toevoegen](./media/how-to-ci-cd/add-tasks.png)

1. In de eerste **Azure IoT Edge** taak, bij te werken de **weergavenaam** naar **Module bouwen en Push**, en klik in de **actie** vervolgkeuzelijst **Maken en pushen van**. In de **Module.json bestand** tekstvak onder het pad aan toe te voegen. Kies vervolgens **Type Containerregister**, zorg ervoor dat u configureert en selecteert u de dezelfde register in uw code(module.json). Deze taak bouwt en pushen van alle modules in de oplossing en publiceren naar het containerregister dat u hebt opgegeven. Als uw modules naar verschillende registers gepusht, kunt u meerdere hebben **Module bouwen en Push** taken. In het geval dat de IoT Edge-oplossing is niet onder de hoofdmap van de codeopslagplaats van uw, kunt u **pad van de Edge-oplossing hoofdmap** in build-definitie.
    
    ![Maken en pushen](./media/how-to-ci-cd/build-and-push.png)

1. In **Build-artefacten publiceren** taak, geeft u de implementatiebestand dat is gegenereerd door de build-taak. Stelt de **pad om te publiceren** naar 'config/deployment.json'. Als u instelt **pad van de Edge-oplossing hoofdmap** in de laatste taak hebt u voor deelname aan het hoofdpad hier. Bijvoorbeeld, als het pad van de basis van de Edge-oplossing is ". / edgesolution ', dan zal de **pad om te publiceren** moet". / edgesolution/config/deployment.json '. De `deployment.json` -bestand is gegenereerd tijdens de build-tijd, dus het is veilig om op te negeren van de rode foutregels in het tekstvak. 

    ![Artefacten publiceren](./media/how-to-ci-cd/publish-build-artifacts.png)

1. Open de **Triggers** tabblad en schakelt u de **continue integratie** trigger. Zorg ervoor dat de vertakking met uw code is opgenomen.

    ![Trigger configureren](./media/how-to-ci-cd/configure-trigger.png)

    Sla de nieuwe build-pijplijn. Klik op de knop **Opslaan**.


## <a name="configure-azure-pipelines-for-continuous-deployment"></a>Azure-pijplijnen voor continue implementatie configureren
In deze sectie maakt u een release-pijplijn die is geconfigureerd voor het automatisch uitgevoerd zodra uw build-pijplijn artefacten komt en implementatie-Logboeken in Azure-pijplijnen worden weergegeven.

1. In de **Releases** tabblad **+ nieuwe pijplijn**. Of, als u release-pijplijnen al hebt, kiest u de **+ nieuw** knop.  

    ![Release-pijplijn toevoegen](./media/how-to-ci-cd/add-release-pipeline.png)

    In **selecteert u een sjabloon** venster, kiest u **beginnen met een leeg project.**

    ![Beginnen met een leeg project](./media/how-to-ci-cd/start-with-empty-job.png)

2. Klik de release-pijplijn met één fase zou initialiseren: **fase 1**. Wijzig de naam van de **fase 1** naar **QA** en behandelen als een testomgeving. Deze doorgaans meerdere fasen bestaat, maakt u meer op basis van uw DevOps-praktijken in een pijplijn typische continue implementatie.

    ![Fase maken](./media/how-to-ci-cd/QA-env.png)

3. Koppel de release aan de build-artefacten. Klik op **toevoegen** in het gebied van artefacten.

    ![Artefacten toevoegen](./media/how-to-ci-cd/add-artifacts.png)  
    
    In **toevoegen van een pagina artefact**, brontype kiezen **bouwen**. Selecteer vervolgens het project en de build-pijplijn die u hebt gemaakt. Klik vervolgens op **toevoegen**.

    ![Een artefact toevoegen](./media/how-to-ci-cd/add-an-artifact.png)

    Open de trigger voor continue implementatie zodat nieuwe release wordt telkens wanneer die een nieuwe build beschikbaar is worden gemaakt.

    ![Trigger configureren](./media/how-to-ci-cd/add-a-trigger.png)

4. Navigeer naar **QA fase** en configureert u de taken in deze fase.

    ![QA-taken configureren](./media/how-to-ci-cd/view-stage-tasks.png)

   Implementatietaak is platform ongevoelig, wat betekent dat u kunt een **VS2017 gehost** of **gehost Ubuntu 1604** in de **agentpool** (of een andere agent worden beheerd door zelf). Klik op '+' en een taak wordt toegevoegd.

    ![Taken toevoegen voor QA](./media/how-to-ci-cd/add-task-qa.png)

5. In de Azure IoT Edge-taak, gaat u naar de **actie** vervolgkeuzelijst **implementeren in IoT Edge-apparaat**. Selecteer uw **Azure-abonnement** en input uw **IoT Hub-naam**. Kunt u een IoT-Edge **implementatie-ID** en de implementatie **prioriteit**. U kunt ook kiezen voor implementatie op één of meerdere apparaten. Als u naar implementeert **meerdere apparaten**, moet u het apparaat opgeven **voorwaarde als doel**. De doelvoorwaarde is een filter op dat moet overeenkomen met een set van Edge-apparaten in IoT Hub. Als u wilt de apparaat-labels gebruiken als de voorwaarde, moet u voor het bijwerken van de bijbehorende apparaten Tags met dubbele voor IoT Hub-apparaat. Stel dat u hebt verschillende IoT Edge apparaten zijn gelabeld als 'qa', en vervolgens de taakconfiguratie van de zoals in de volgende schermafbeelding moet. 

    ![Implementatie naar QA](./media/how-to-ci-cd/deploy-to-qa.png)

    Sla de nieuwe release-pijplijn. Klik op de knop **Opslaan**. En klik vervolgens op **pijplijn** terugkeren naar de pijplijn.

6. De tweede fase is voor uw productieomgeving. Om toe te voegen een nieuwe fase 'PROD', kunt u gewoon de fase 'QA' klonen en wijzig de naam gekloonde fase **PROD**,

    ![Kloon-fase](./media/how-to-ci-cd/clone-stage.png)

7. Configureer de taken voor uw productieomgeving. Stel dat u verschillende IoT Edge apparaten zijn gelabeld als 'prod', in de configuraties van de taak, de Doelvoorwaarde 'prod', en het instellen van de implementatie-id als 'implementeren prod' update hebt. Klik op de knop **Opslaan**. En klik vervolgens op **pijplijn** terugkeren naar de pijplijn.
    
    ![Implementeren naar productie](./media/how-to-ci-cd/deploy-to-prod.png)

7. Op dit moment onze build-artefact wordt geactiveerd continu op **QA** fase en vervolgens **PROD** fase. Maar de meeste van de tijden die u wilt integreren soms test op de apparaten QA en handmatig goedkeuren de bits. De bits wordt later opnieuw worden geïmplementeerd op de productie-omgeving. Een goedkeuring in de fase PROD als het volgende instellen.

    1. Open **vóór de implementatie voorwaarden** instelling deelvenster.

        ![Open vóór de implementatie-voorwaarden](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Stel **ingeschakeld** in **vóór de implementatie goedkeuringen**. En vult u de **goedkeurders** invoer. Klik vervolgens op **Opslaan**.
    
        ![Voorwaarden instellen](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


8. Nu uw release-pijplijn is ingesteld als volgt.

    ![Release-pijplijn](./media/how-to-ci-cd/release-pipeline.png)

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Controleer of IoT Edge CI/CD met de build en pipelines vrijgeven

In deze sectie maakt activeren u een build zodat de werken CI/CD-pijplijn. Controleer vervolgens of dat de implementatie is geslaagd.

1. Voor het activeren van een build-taak, kunt u een wijziging naar de opslagplaats broncode pushen of deze handmatig te activeren. U kunt een taak build activeren in de build-pijplijn door te klikken op de **wachtrij** knop zoals in de volgende schermafbeelding.

    ![Handmatige trigger](./media/how-to-ci-cd/manual-trigger.png)

2. Als de build-pijplijn met succes is voltooid produceert een release te **QA** fase. Ga voor het bouwen van Logboeken van de pijplijn en u ziet het volgende.

    ![Buildlogboeken](./media/how-to-ci-cd/build-logs.png)

3. De implementatie is voltooid op **QA** fase activeren een melding naar de fiatteur. Ga voor release-pijplijn, kunt u het volgende zien. 

    ![Wachten op goedkeuring](./media/how-to-ci-cd/pending-approval.png)


4. Nadat de fiatteur deze wijziging hebt goedgekeurd, kan deze worden geïmplementeerd voor **PROD**.

    ![Als u wilt prod implementeren](./media/how-to-ci-cd/approve-and-deploy-to-prod.png)


## <a name="next-steps"></a>Volgende stappen

* Inzicht in de IoT Edge-implementatie in [inzicht in IoT Edge-implementaties voor individuele apparaten of op schaal](module-deployment-monitoring.md)
* Doorloop de stappen voor het maken, bijwerken of verwijderen van een implementatie in [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).

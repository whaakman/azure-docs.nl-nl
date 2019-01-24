---
title: Continue integratie en continue implementatie - Azure IoT Edge | Microsoft Docs
description: Instellen van continue integratie en continue implementatie - Azure IoT Edge met Azure DevOps, Azure-pijplijnen
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: ca638f1374c2e15c3978cfe711295f0ebe249d68
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54818163"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Continue integratie en continue implementatie voor Azure IoT Edge

U kunt eenvoudig inzetten van DevOps met uw Azure IoT Edge-toepassingen met de ingebouwde Azure IoT Edge-taken in Azure-pijplijnen. In dit artikel laat zien hoe u de continue integratie en continue implementatie-functies van Azure-pijplijnen te bouwen, testen en implementeren van toepassingen snel en efficiënt aan uw Azure IoT Edge kunt gebruiken. 

In dit artikel leert u hoe u twee pijplijnen voor uw IoT Edge-oplossing maken met de ingebouwde Azure IoT Edge-taken voor het Azure-pijplijnen. De eerste uw code en uw module-installatiekopieën pushen naar uw containerregister en het maken van een manifest van de implementatie van de oplossing bouwt. De tweede implementeert uw modules naar de betreffende IoT Edge-apparaten.  

![Diagram - CI en CD vertakkingen voor ontwikkeling en productie](./media/how-to-ci-cd/cd.png)


## <a name="prerequisites"></a>Vereisten

* De opslagplaats van een Azure-opslagplaatsen. Als u niet hebt, kunt u [maken van een nieuwe Git-opslagplaats in uw project](https://docs.microsoft.com/azure/devops/repos/git/create-new-repo?view=vsts&tabs=new-nav).
* Een IoT Edge-oplossing doorgevoerd en gepusht naar uw opslagplaats. Als u maken van een nieuwe oplossing wilt voor het testen van dit artikel, volgt u de stappen in [modules ontwikkelen en fouten opsporen in Visual Studio Code](how-to-vs-code-develop-module.md) of [ontwikkelen en fouten opsporen C# modules in Visual Studio](how-to-visual-studio-develop-csharp-module.md).
   * In dit artikel is alles wat u hoeft de map van de oplossing die zijn gemaakt door de IoT Edge-sjablonen in Visual Studio Code of Visual Studio. U hoeft te bouwen, pushen, implementeren of fouten opsporen in deze code voordat u doorgaat. U zult deze processen instellen in Azure-pijplijnen. 
   * Als u een nieuwe oplossing maakt, eerst uw opslagplaats lokaal klonen. Vervolgens bij het maken van de oplossing kunt u deze rechtstreeks in de map voor de opslagplaats te maken. U kunt eenvoudig doorvoeren en pushen van de nieuwe bestanden van daaruit. 
* Een containerregister waar u de module installatiekopieën kunt pushen. U kunt [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) of een register van derden. 
* Een actief [IoT-hub](../iot-hub/iot-hub-create-through-portal.md) met ten minste IoT Edge-apparaten voor het testen van de afzonderlijke fasen voor testen en productie-implementatie. U kunt de artikelen voor het maken van een IoT Edge-apparaat op volgen [Linux](quickstart-linux.md) of [Windows](quickstart.md)


Zie voor meer informatie over het gebruik van Azure-opslagplaatsen [deel van uw code met Visual Studio en Azure-opslagplaatsen](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts)

## <a name="configure-continuous-integration"></a>Continue integratie configureren
In deze sectie maakt u een nieuwe build-pijplijn. Configureren van de pijplijn worden automatisch uitgevoerd wanneer u wijzigingen in het voorbeeld IoT Edge-oplossing inchecken en publiceren van build-Logboeken.

>[!NOTE]
>In dit artikel wordt gebruikgemaakt van de visuele ontwerpfunctie Azure DevOps. Voordat u de stappen in deze sectie gebruikt u de uitschakelen van de preview-functie voor de nieuwe ervaring voor het maken van het YAML-pijplijn. 
>1. In Azure DevOps, selecteert u het pictogram van uw profiel vervolgens **Preview-functies**.
>2. Schakel **interface voor het maken van nieuwe YAML-pijplijn** uitschakelen. 
>
>Zie voor meer informatie, [maken van een build-pijplijn](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav#create-a-build-pipeline).

1. Meld u aan bij uw organisatie Azure DevOps ( **https://dev.azure.com/{your organisatie} /**) en open het project met de opslagplaats van uw IoT Edge-oplossing.

   In dit artikel hebben we een opslagplaats met de naam gemaakt **IoTEdgeRepo**. Deze opslagplaats bevat **IoTEdgeSolution** die is de code voor een module met de naam **filtermodule**. 

   ![Uw DevOps-project openen](./media/how-to-ci-cd/init-project.png)

2. Navigeer naar de Azure-pijplijnen in uw project. Open de **bouwt** tabblad en selecteer **nieuwe pijplijn**. Of, als u bouwen van pijplijnen al hebt, selecteert u de **nieuw** knop. Kies vervolgens **nieuwe build-pijplijn**.

    ![Een nieuwe build-pipeline maken](./media/how-to-ci-cd/add-new-build.png)

3. Volg de aanwijzingen om de pijplijn te maken. 

   1. Geef de broninformatie voor uw nieuwe build-pijplijn. Selecteer **Azure opslagplaatsen Git** als de bron, selecteer vervolgens de project, de opslagplaats en de vertakking waar de code van uw IoT Edge-oplossing zich bevindt. Selecteer **doorgaan**. 

      ![Selecteer de pijplijnbron van uw](./media/how-to-ci-cd/pipeline-source.png)

   2. Selecteer **leeg taak** in plaats van een sjabloon. 

      ![Beginnen met een lege proces](./media/how-to-ci-cd/start-with-empty.png)

4. Zodra uw pijplijn is gemaakt, gaat u naar de de pijplijn-editor. Kies in de beschrijving van uw pijplijn, de juiste agentpool op basis van uw doelplatform: 
    
    * Als u maken van de modules in platform amd64 voor Linux-containers wilt, kiest u **Ubuntu 1604 die worden gehost**

    * Als u uw modules in platform amd64 voor Windows 1809 containers maken wilt, moet u [instellen van de zelf-hostend-agent op Windows](https://docs.microsoft.com/azure/devops/pipelines/agents/v2-windows?view=vsts).

    * Als u uw modules in platform arm32v7 voor Linux-containers maken wilt, moet u [instellen van de zelf-hostend-agent op Linux](https://blogs.msdn.microsoft.com/iotdev/2018/11/13/setup-azure-iot-edge-ci-cd-pipeline-with-arm-agent/).
    
    ![Build-agentpool configureren](./media/how-to-ci-cd/configure-env.png)

5. De pijplijn is vooraf geconfigureerd met een taak met de naam **Agent-taak 1**. Selecteer het plusteken (**+**) drie taken toevoegen aan de taak: **Azure IoT Edge** twee keer, en **Build-artefacten publiceren** zodra. (Beweeg de muisaanwijzer over de naam van de afzonderlijke taken om te zien de **toevoegen** knop.)

   ![Azure IoT Edge-taak toevoegen](./media/how-to-ci-cd/add-iot-edge-task.png)

   Wanneer alle drie de taken worden toegevoegd, lijkt uw Agent-taak in het volgende voorbeeld:
    
   ![Drie taken in de build-pijplijn](./media/how-to-ci-cd/add-tasks.png)

6. Selecteer de eerste **Azure IoT Edge** taak om deze te bewerken. Deze taak bouwt alle modules in de oplossing met het doelplatform dat u opgeeft, wordt ook gegenereerd de **deployment.json** -bestand dat uw IoT Edge-apparaten legt uit hoe het configureren van de implementatie.

   * **Weergavenaam**: Accepteer de standaardwaarde **Azure IoT Edge - Build-module installatiekopieën**.
   * **Actie**: Accepteer de standaardwaarde **maken van installatiekopieën van de module**. 
   * **. template.json-bestand**: Selecteer het weglatingsteken (**...** ) en navigeer naar de **deployment.template.json** bestand in de opslagplaats met uw IoT Edge-oplossing. 
   * **Standaard platform**: Selecteer het juiste platform voor uw modules op basis van de gewenste IoT Edge-apparaat. 
   * **Variabelen voor de uitvoer**: De uitvoervariabelen bevatten een naam die u gebruiken kunt voor het configureren van het bestandspad waar uw bestand deployment.json wordt gegenereerd. De naam van verwijzing ingesteld op iets gemakkelijk te onthouden, zoals **edge**. 

7. Selecteer het tweede **Azure IoT Edge** taak om deze te bewerken. Deze taak worden alle installatiekopieën van de module naar het containerregister dat u selecteert. Ook wordt toegevoegd de referenties van uw container-register op de **deployment.json** bestand, zodat uw IoT Edge-apparaat toegang de module-installatiekopieën tot hebben. 

   * **Weergavenaam**: De weergavenaam wordt automatisch bijgewerkt wanneer het actieveld wordt gewijzigd. 
   * **Actie**: Gebruik de vervolgkeuzelijst om te selecteren **module installatiekopieën pushen**. 
   * **Type containerregister**: Selecteer het type van het containerregister dat u gebruiken voor het opslaan van uw module-installatiekopieën. Afhankelijk van welke register dat u kiest, het formulier verandert. Als u ervoor kiest **Azure Container Registry**, gebruik de vervolgkeuzelijsten om te selecteren van het Azure-abonnement en de naam van het containerregister. Als u ervoor kiest **algemene Containerregister**, selecteer **nieuw** een register-serviceverbinding te maken. 
   * **. template.json-bestand**: Selecteer het weglatingsteken (**...** ) en navigeer naar de **deployment.template.json** bestand in de opslagplaats met uw IoT Edge-oplossing. 
   * **Standaard platform**: Selecteer het platform dat dezelfde als de ingebouwde module afbeeldingen.

   Als u meerdere containerregisters voor het hosten van uw installatiekopieën module hebt, moet u het dupliceren van deze taak, selecteert u andere container registry en gebruiken **overslaan van modules die zijn** in de geavanceerde instellingen om de installatiekopieën die niet voor dit over te slaan specifieke register.

8. Selecteer de **Build-artefacten publiceren** taak om deze te bewerken. Geef het pad naar het implementatiebestand dat is gegenereerd door de build-taak. Stel de **pad om te publiceren** waarde zodat deze overeenkomen met de uitvoervariabele die u in de module opbouwtaak instelt. Bijvoorbeeld `$(edge.DEPLOYMENT_FILE_PATH)`. Laat de andere waarden als standaardwaarden. 

9. Open de **Triggers** tabblad en schakel het selectievakje aan **inschakelen van continue integratie**. Zorg ervoor dat de vertakking met uw code is opgenomen.

    ![Trigger continue integratie inschakelen](./media/how-to-ci-cd/configure-trigger.png)

10. Sla de nieuwe build-pijplijn met **opslaan** knop.

Deze pijplijn is nu geconfigureerd voor het automatisch uitgevoerd wanneer u nieuwe code naar uw opslagplaats pushen. De laatste taak, het publiceren van de artefacten pijplijn activeert een release-pijplijn. Doorgaan naar de volgende sectie voor de release-pijplijn bouwen. 

## <a name="configure-continuous-deployment"></a>Continue implementatie inschakelen
In deze sectie maakt u een release-pijplijn die is geconfigureerd voor het automatisch uitgevoerd zodra uw build-pijplijn artefacten komt en implementatielogboeken wordt het weergegeven in de Azure-pijplijnen.

In deze sectie maakt u twee verschillende stadia, één voor testimplementaties en één voor productie-implementaties. 

### <a name="create-test-stage"></a>Test fase maken

Een nieuwe pijplijn maken en configureren van de eerste fase voor kwaliteit assurance (QA) implementaties. 

1. In de **Releases** tabblad **+ nieuwe pijplijn**. Of, als u release-pijplijnen al hebt, kiest u de **+ nieuw** en selecteer **+ nieuw release-pijplijn**.  

    ![Release-pijplijn toevoegen](./media/how-to-ci-cd/add-release-pipeline.png)

2. Wanneer u hierom wordt gevraagd om een sjabloon te selecteren, kiest u beginnen met een **leeg taak**.

    ![Beginnen met een leeg project](./media/how-to-ci-cd/start-with-empty-job.png)

3. Uw nieuwe release-pijplijn met één fase, met de naam initialiseert **fase 1**. Wijzig de naam van fase 1 van **QA** en behandelen als een testomgeving. Pijplijnen voor continue implementatie hebben meestal meerdere fasen. U kunt meer op basis van uw DevOps-praktijken. Sluit het venster Details over van de fase nadat deze gewijzigd. 

    ![Fase voor test-omgeving maken](./media/how-to-ci-cd/QA-env.png)

4. De release aan de build-artefacten die zijn gepubliceerd door de build-pijplijn koppelen. Klik op **toevoegen** in het gebied van artefacten.

   ![Artefacten toevoegen](./media/how-to-ci-cd/add-artifacts.png)  
    
5. In **toevoegen van een pagina artefact**, selecteert u het brontype **bouwen**. Selecteer vervolgens het project en de build-pijplijn die u hebt gemaakt. Selecteer vervolgens **Toevoegen**.

   ![Een build-artefact toevoegen](./media/how-to-ci-cd/add-an-artifact.png)

6. Open de artefact-triggers en selecteert u de wisselknop om in te schakelen van de trigger voor continue implementatie. Een nieuwe versie wordt nu, telkens wanneer die een nieuwe build beschikbaar is gemaakt.

   ![Trigger voor continue implementatie configureren](./media/how-to-ci-cd/add-a-trigger.png)

7. De **QA** fase vooraf is geconfigureerd met één taak en geen taken. Selecteer in het menu pijplijn **taken** en kies vervolgens de **QA** fase.  Selecteer het aantal taak en het configureren van de taken in deze fase.

    ![QA-taken configureren](./media/how-to-ci-cd/view-stage-tasks.png)

8. In de fase QA ziet u een standaard **Agent-taak**. U meer informatie over de agent-taak kunt configureren, maar de Implementatietaak is het platform ongevoelig zodat u een kunt **gehost VS2017** of **gehost Ubuntu 1604** in de **agentpool**(of een andere agent worden beheerd door uzelf). 

9. Selecteer het plusteken (**+**) een taak wordt toegevoegd. Zoeken en toevoegen **Azure IoT Edge**. 

    ![Taken toevoegen voor QA](./media/how-to-ci-cd/add-task-qa.png)

10. Selecteer de nieuwe Azure IoT Edge-taak en deze configureren met de volgende waarden:

   * **Weergavenaam**: De weergavenaam wordt automatisch bijgewerkt wanneer het actieveld wordt gewijzigd. 
   * **Actie**: Gebruik de vervolgkeuzelijst om te selecteren **implementeren in IoT Edge-apparaat**. De weergavenaam van de taak zodat deze overeenkomen met wijzigen van de actiewaarde wordt ook bijgewerkt.
   * **Azure-abonnement**: Selecteer het abonnement met uw IoT-Hub.
   * **De naam van IoT Hub**: Selecteer uw IoT-hub. 
   * **Eén/meerdere apparaat kiezen**: Kies of u de release-pijplijn te implementeren op een of meer apparaten. 
      * Als u op één apparaat implementeert, voert u de **IoT Edge-apparaat-ID**. 
      * Als u op meerdere apparaten implementeert, geeft u het apparaat **voorwaarde als doel**. De doelvoorwaarde is een filter op dat moet overeenkomen met een set van Edge-apparaten in IoT Hub. Als u wilt de apparaat-labels gebruiken als de voorwaarde, moet u voor het bijwerken van de bijbehorende apparaten Tags met dubbele voor IoT Hub-apparaat. Update de **IoT Edge-implementatie-ID** en **IoT Edge-implementatie prioriteit** in de geavanceerde instellingen. Zie voor meer informatie over het maken van een implementatie voor meerdere apparaten [automatisch informatie over IoT Edge-implementaties](module-deployment-monitoring.md).

11. Selecteer **opslaan** uw wijzigingen opslaan in de nieuwe release-pijplijn. Ga terug naar de pijplijnweergave door te selecteren **pijplijn** in het menu. 

### <a name="create-production-stage"></a>Productiefase maken

Maak een tweede fase in de release-pijplijn voor productie-implementatie. 

1. Maak een tweede fase voor productie door het klonen van de QA-fase. Beweeg de muisaanwijzer de cursor over de QA-fase en selecteer vervolgens de knop klonen. 

    ![Kloon-fase](./media/how-to-ci-cd/clone-stage.png)

2. Selecteer de nieuwe fase wordt met de naam **kopie van QA**, de eigenschappen te openen. Wijzig de fasenaam in **PROD**, voor productie. Sluit het venster van de eigenschappen van fase. 

3. Als u de taken van de fase PROD, schakelt **taken** Kies in de pijplijn-menu de **PROD** fase. 

4. Selecteer de Azure IoT Edge-taak configureren als voor uw productieomgeving. De implementatie-instellingen zijn waarschijnlijk hetzelfde voor QA en productie, behalve dat u wilt een ander apparaat of een set met apparaten in productie. De apparaat-ID-veld of de voorwaarde en implementatie-ID doelvelden voor uw productieapparaten bijwerken. 

5. Sla het met de **opslaan** knop. En selecteer vervolgens **pijplijn** om terug te gaan naar de pijplijnweergave.
    
6. De manier waarop deze release-pijplijn die momenteel is geconfigureerd, build-artefact wordt geactiveerd. de **QA** fase en vervolgens **PROD** fase telkens wanneer een nieuwe build is voltooid. Echter meestal willen soms test op de apparaten QA integreren en handmatig goedkeuren van de implementatie voor productie. Gebruik de volgende stappen uit om een goedkeuringsvoorwaarde voor de productie-fase te maken:

    1. Open de **vóór de implementatie voorwaarden** instellingenvenster.

        ![Open vóór de implementatie-voorwaarden](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. In-/ uitschakelen de **vóór de implementatie goedkeuringen** voorwaarde aan **ingeschakeld**. Voeg een of meer gebruikers of groepen in de **goedkeurders** veld en eventuele andere goedkeuring beleidsregels die u wilt aanpassen. Om uw wijzigingen hebt opgeslagen, sluit u het deelvenster voorwaarden vóór de implementatie.
    
       ![Voorwaarden instellen](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


7. Sla uw release-pijplijn met de **opslaan** knop. 

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Controleer of IoT Edge CI/CD met de build en pipelines vrijgeven

Voor het activeren van een build-taak, kunt u een wijziging naar de opslagplaats broncode pushen of deze handmatig te activeren. In deze sectie activeert u handmatig de CI/CD-pijplijn om te controleren of het werkt. Controleer vervolgens of de implementatie is geslaagd.

1. Navigeer naar de build-pijplijn die u aan het begin van dit artikel hebt gemaakt. 

2. U kunt een taak build activeren in de build-pijplijn door te selecteren de **wachtrij** knop zoals in de volgende schermafbeelding.

    ![Handmatige trigger](./media/how-to-ci-cd/manual-trigger.png)

3. Selecteer de build-taak voor het bekijken van de voortgang. Als de build-pijplijn met succes is voltooid, activeert dit een release te **QA** fase. 

    ![Buildlogboeken](./media/how-to-ci-cd/build-logs.png)

4. De implementatie is voltooid op **QA** fase wordt een melding naar de fiatteur geactiveerd. Controleer of de modules is geïmplementeerd op het apparaat of apparaten waarop u met de fase QA van toepassing. Vervolgens navigeert u release-pijplijn en goedkeuring voor de release te gaan naar de productie-fase door te selecteren geeft de **PROD** knop en selecteer vervolgens **goedkeuren**. 

    ![Wachten op goedkeuring](./media/how-to-ci-cd/pending-approval.png)

5. Nadat de fiatteur deze wijziging hebt goedgekeurd, kan deze worden geïmplementeerd voor **PROD**.

## <a name="next-steps"></a>Volgende stappen

* Inzicht in de IoT Edge-implementatie in [inzicht in IoT Edge-implementaties voor individuele apparaten of op schaal](module-deployment-monitoring.md)
* Doorloop de stappen voor het maken, bijwerken of verwijderen van een implementatie in [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).

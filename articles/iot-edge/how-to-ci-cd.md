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
ms.openlocfilehash: 659a6f5acaac848084ed1e9590a414191542b54a
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414625"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Continue integratie en continue implementatie voor Azure IoT Edge

U kunt eenvoudig DevOps met uw Azure IoT Edge-toepassingen met de ingebouwde Azure IoT Edge taken in azure-pijp lijnen. In dit artikel wordt beschreven hoe u de functies continue integratie en continue implementatie van Azure-pijp lijnen kunt gebruiken om toepassingen snel en efficiënt te bouwen, te testen en te implementeren op uw Azure IoT Edge. 

In dit artikel leert u hoe u de ingebouwde Azure IoT Edge taken voor Azure-pijp lijnen kunt gebruiken om twee pijp lijnen te maken voor uw IoT Edge oplossing. De eerste zet uw code en bouwt de oplossing op, waarbij uw module-installatie kopieën naar het container register worden gepusht en een implementatie manifest wordt gemaakt. De tweede implementeert uw modules om IoT Edge apparaten te richten.  

![Diagram - CI en CD vertakkingen voor ontwikkeling en productie](./media/how-to-ci-cd/cd.png)


## <a name="prerequisites"></a>Vereisten

* Een Azure opslag plaatsen-opslag plaats. Als u er nog geen hebt, kunt u [een nieuwe Git-opslag plaats in uw project maken](https://docs.microsoft.com/azure/devops/repos/git/create-new-repo?view=vsts&tabs=new-nav).
* Een IoT Edge oplossing die is doorgevoerd en gepusht naar uw opslag plaats. Als u een nieuwe voorbeeld oplossing wilt maken voor het testen van dit artikel, volgt u de stappen in [modules ontwikkelen en fouten opsporen in Visual Studio code](how-to-vs-code-develop-module.md) of [modules ontwikkelen en fouten opsporen C# in Visual Studio](how-to-visual-studio-develop-csharp-module.md).
   * Voor dit artikel hoeft u alleen de map Solution te maken die is gemaakt door de IoT Edge sjablonen in Visual Studio code of Visual Studio. U hoeft deze code niet te bouwen, te pushen, te implementeren of te debuggen voordat u doorgaat. U stelt deze processen in op Azure-pijp lijnen. 
   * Als u een nieuwe oplossing wilt maken, moet u uw opslag plaats lokaal eerst klonen. Wanneer u de oplossing maakt, kunt u ervoor kiezen om deze rechtstreeks in de map opslag plaats te maken. U kunt de nieuwe bestanden eenvoudig van daaruit door voeren en pushen. 
* Een container register waar u module installatie kopieën kunt pushen. U kunt [Azure container Registry](https://docs.microsoft.com/azure/container-registry/) of een REGI ster van derden gebruiken. 
* Een actieve [IOT-hub](../iot-hub/iot-hub-create-through-portal.md) met ten minste IOT edge apparaten voor het testen van de afzonderlijke test-en productie-implementatie fasen. U kunt de Quick Start-artikelen volgen om een IoT Edge apparaat te maken in [Linux](quickstart-linux.md) of [Windows](quickstart.md)


Zie [uw code delen met Visual Studio en Azure opslag plaatsen](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts) voor meer informatie over het gebruik van Azure opslag plaatsen

## <a name="configure-continuous-integration"></a>Continue integratie configureren
In deze sectie maakt u een nieuwe build-pijp lijn. Configureer de pijp lijn zo dat deze automatisch wordt uitgevoerd wanneer u wijzigingen aanbrengt in de voor beeld-IoT Edge oplossing en publiceer build-Logboeken.

>[!NOTE]
>In dit artikel wordt gebruikgemaakt van de Visual Designer van Azure DevOps. Voordat u de stappen in deze sectie volgt, schakelt u de preview-functie uit voor het maken van de nieuwe YAML pipeline-ervaring. 
>1. Selecteer in azure DevOps uw profiel pictogram en selecteer vervolgens **Preview-functies**.
>2. Schakel de functie voor het **maken van nieuwe YAML-pijp lijnen** uit. 
>
>Zie [een build-pijp lijn maken](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav#create-a-build-pipeline)voor meer informatie.

1. Meld u aan bij uw Azure DevOps-organisatie (**https:\//dev.Azure.com/{your organization}/** ) en open het project met uw IOT Edge Solution-opslag plaats.

   Voor dit artikel hebt u een opslag plaats gemaakt met de naam **IoTEdgeRepo**. Deze opslag plaats bevat **IoTEdgeSolution** die de code voor een module met de naam **filtermodule**heeft. 

   ![Open uw DevOps-project](./media/how-to-ci-cd/init-project.png)

2. Navigeer naar Azure-pijp lijnen in uw project. Open het  tabblad builds en selecteer **nieuwe pijp lijn**. Als u al een pijp lijn hebt gemaakt, selecteert u de knop **Nieuw** . Kies vervolgens **nieuwe build-pijp lijn**.

    ![Een nieuwe build-pipeline maken](./media/how-to-ci-cd/add-new-build.png)

3. Volg de aanwijzingen om uw pijp lijn te maken. 

   1. Geef de bron gegevens voor de nieuwe build-pijp lijn op. Selecteer **Azure opslag plaatsen Git** als bron en selecteer vervolgens het project, de opslag plaats en de vertakking waar uw IOT Edge oplossings code zich bevindt. Selecteer vervolgens **door gaan**. 

      ![De pijplijn bron selecteren](./media/how-to-ci-cd/pipeline-source.png)

   2. Selecteer **lege taak** in plaats van een sjabloon. 

      ![Beginnen met een lege proces](./media/how-to-ci-cd/start-with-empty.png)

4. Zodra de pijp lijn is gemaakt, wordt u naar de pijplijn editor geleid. Kies in de pijplijn beschrijving de juiste agent pool op basis van uw doel platform: 
    
   * Als u maken van de modules in platform amd64 voor Linux-containers wilt, kiest u **Ubuntu 1604 die worden gehost**

   * Als u uw modules in platform amd64 voor Windows 1809-containers wilt maken, moet u [zelf-hostende agent instellen in Windows](https://docs.microsoft.com/azure/devops/pipelines/agents/v2-windows?view=vsts).

   * Als u uw modules in platform arm32v7 of arm64 voor Linux-containers wilt maken, moet u [zelf-hostende agent instellen in Linux](https://blogs.msdn.microsoft.com/iotdev/2018/11/13/setup-azure-iot-edge-ci-cd-pipeline-with-arm-agent/).
    
     ![Build-agentpool configureren](./media/how-to-ci-cd/configure-env.png)

5. Uw pijp lijn wordt vooraf geconfigureerd met een taak genaamd **Agent taak 1**. Selecteer het plus teken ( **+** ) om drie taken toe te voegen aan de taak: **Azure IOT Edge** twee keer op en **Publiceer build** -artefacten eenmaal. (Beweeg de muis aanwijzer over de naam van elke taak om de knop **toevoegen** te zien.)

   ![Azure IoT Edge taak toevoegen](./media/how-to-ci-cd/add-iot-edge-task.png)

   Wanneer alle drie de taken worden toegevoegd, ziet uw agent-taak eruit als in het volgende voor beeld:
    
   ![Drie taken in de build-pijp lijn](./media/how-to-ci-cd/add-tasks.png)

6. Selecteer de eerste **Azure IOT Edge** taak om deze te bewerken. Met deze taak worden alle modules in de oplossing op basis van het door u opgegeven doel platform gegenereerd. het bestand implementeert de **implementatie. json** dat uw IOT edge-apparaten vertelt hoe de implementatie moet worden geconfigureerd.

   * **Weergave naam**: Accepteer de standaard **installatie kopieën van de module Azure IOT Edge-build**.
   * **Actie**: Accepteer de standaard **installatie kopieën**voor de build-module. 
   * **. bestand template. json**: Selecteer het beletsel teken ( **...** ) en navigeer naar het bestand **Deployment. template. json** in de opslag plaats met uw IOT EDGE-oplossing. 
   * **Standaard platform**: Selecteer het juiste platform voor uw modules op basis van uw doel IoT Edge apparaat. 
   * **Uitvoer variabelen**: De uitvoer variabelen bevatten een referentie naam die u kunt gebruiken om het bestandspad te configureren waar uw implementatie. JSON-bestand wordt gegenereerd. Stel de naam van de verwijzing in op een onthouden soort, net als een **rand**. 

7. Selecteer de tweede **Azure IOT Edge** taak om deze te bewerken. Met deze taak worden alle module-installatie kopieën gepusht naar het container register dat u selecteert. Ook worden uw container register referenties toegevoegd aan het bestand **implement. json** , zodat het IOT edge apparaat toegang kan krijgen tot de module-installatie kopieën. 

   * **Weergave naam**: De weergave naam wordt automatisch bijgewerkt wanneer het actie veld wordt gewijzigd. 
   * **Actie**: Gebruik de vervolg keuzelijst om **installatie kopieën van push module**te selecteren. 
   * **Container register type**: Selecteer het type container register dat u gebruikt om de module installatie kopieën op te slaan. Afhankelijk van het register type dat u kiest, verandert het formulier. Als u **Azure container Registry**selecteert, gebruikt u de vervolg keuzelijst om het Azure-abonnement en de naam van uw container register te selecteren. Als u **algemene container Registry**kiest, selecteert u **Nieuw** om een register service verbinding te maken. 
   * **. bestand template. json**: Selecteer het beletsel teken ( **...** ) en navigeer naar het bestand **Deployment. template. json** in de opslag plaats met uw IOT EDGE-oplossing. 
   * **Standaard platform**: Selecteer hetzelfde platform als uw ingebouwde module-installatie kopieën.

   Als u meerdere containerregisters voor het hosten van uw installatiekopieën module hebt, moet u het dupliceren van deze taak, selecteert u andere container registry en gebruiken **overslaan van modules die zijn** in de geavanceerde instellingen om de installatiekopieën die niet voor dit over te slaan specifieke register.

8. Selecteer de taak **Build-artefacten publiceren** om deze te bewerken. Geef het bestandspad op naar het implementatie bestand dat is gegenereerd door de taak bouwen. Stel het **pad naar de publicatie** waarde in die overeenkomt met de uitvoer variabele die u in de opbouw module taak hebt ingesteld. Bijvoorbeeld `$(edge.DEPLOYMENT_FILE_PATH)`. Zorg ervoor dat de andere waarden worden gebruikt. 

9. Open het tabblad **Triggers** en schakel het selectie vakje in om **continue integratie in te scha kelen**. Zorg ervoor dat de vertakking met uw code is opgenomen.

    ![Trigger continue integratie inschakelen](./media/how-to-ci-cd/configure-trigger.png)

10. Sla de nieuwe build-pijp lijn op met de knop **Opslaan** .

Deze pijp lijn is nu geconfigureerd om automatisch te worden uitgevoerd wanneer u nieuwe code naar uw opslag plaats pusht. De laatste taak, die pijp lijn artefacten publiceert, een release pijplijn wordt geactiveerd. Ga door naar de volgende sectie om de release pijplijn te bouwen. 

## <a name="configure-continuous-deployment"></a>Continue implementatie inschakelen
In deze sectie maakt u een release pijplijn die is geconfigureerd om automatisch te worden uitgevoerd wanneer uw build-pijp lijn artefacten overneemt en de implementatie Logboeken in azure-pijp lijnen worden weer gegeven.

In deze sectie maakt u twee verschillende fasen: één voor test implementaties en één voor productie-implementaties. 

### <a name="create-test-stage"></a>Test fase maken

Maak een nieuwe pijp lijn en configureer de eerste fase voor implementaties van Quality Assurance (QA). 

1. In de **Releases** tabblad **+ nieuwe pijplijn**. Als u al release pijplijnen hebt, kiest u de knop **+ Nieuw** en selecteert u **+ nieuwe release pijplijn**.  

    ![Release-pijplijn toevoegen](./media/how-to-ci-cd/add-release-pipeline.png)

2. Wanneer u wordt gevraagd om een sjabloon te selecteren, kiest u om te beginnen met een **lege taak**.

    ![Beginnen met een leeg project](./media/how-to-ci-cd/start-with-empty-job.png)

3. Uw nieuwe release pijplijn wordt geïnitialiseerd met één fase, de naam **fase 1**. Wijzig de naam fase 1 in **QA** en behandel dit als een test omgeving. Doorgaans hebben doorlopende implementatie pijplijnen meerdere fasen. U kunt meer maken op basis van uw DevOps-oefening. Sluit het venster met fase details zodra de naam ervan is gewijzigd. 

    ![Fase voor test-omgeving maken](./media/how-to-ci-cd/QA-env.png)

4. Koppel de release aan de constructie-artefacten die worden gepubliceerd door de build-pijp lijn. Klik op **toevoegen** in het gebied van artefacten.

   ![Artefacten toevoegen](./media/how-to-ci-cd/add-artifacts.png)  
    
5. Selecteer op **de pagina een artefact toevoegen de**optie bron type **Build**. Selecteer vervolgens het project en de build-pijp lijn die u hebt gemaakt. Selecteer vervolgens **Toevoegen**.

   ![Een build-artefact toevoegen](./media/how-to-ci-cd/add-an-artifact.png)

6. Open de artefact triggers en selecteer de schakel optie om de continue implementatie trigger in te scha kelen. Er wordt nu een nieuwe release gemaakt wanneer een nieuwe build beschikbaar is.

   ![Trigger voor continue implementatie configureren](./media/how-to-ci-cd/add-a-trigger.png)

7. De **QA** -fase is vooraf geconfigureerd met één taak en nul taken. Selecteer in het menu pijp lijn **taken** en kies vervolgens de **QA** -fase.  Selecteer de taak en het aantal taken voor het configureren van de taken in deze fase.

    ![QA-taken configureren](./media/how-to-ci-cd/view-stage-tasks.png)

8. In de QA-fase ziet u een standaard **Agent taak**. U kunt details over de agent taak configureren, maar de implementatie taak is niet van het platform, zodat u een **gehoste VS2017** of gehoste **Ubuntu 1604** in de **agent groep** (of een andere door uzelf beheerde agent) kunt gebruiken. 

9. Selecteer het plus teken ( **+** ) om één taak toe te voegen. Zoek en voeg **Azure IOT Edge**toe. 

    ![Taken toevoegen voor QA](./media/how-to-ci-cd/add-task-qa.png)

10. Selecteer de nieuwe Azure IoT Edge taak en configureer deze met de volgende waarden:

    * **Weergave naam**: De weergave naam wordt automatisch bijgewerkt wanneer het actie veld wordt gewijzigd. 
    * **Actie**: Gebruik de vervolg keuzelijst om **implementeren naar IOT edge apparaat**te selecteren. Als u de actie waarde wijzigt, wordt ook de weergave naam van de taak bijgewerkt zodat deze overeenkomt.
    * **Azure-abonnement**: Selecteer het abonnement dat uw IoT Hub bevat.
    * **IOT hub naam**: Selecteer uw IoT-hub. 
    * **Kies één of meerdere apparaten**: Kies of u wilt dat de release pijplijn op één apparaat of op meerdere apparaten wordt geïmplementeerd. 
      * Als u op één apparaat implementeert, voert u de **IOT edge apparaat-id**in. 
      * Als u implementeert op meerdere apparaten, geeft u de **voor waarde voor**het apparaat doel op. De doelvoorwaarde is een filter op dat moet overeenkomen met een set van Edge-apparaten in IoT Hub. Als u wilt de apparaat-labels gebruiken als de voorwaarde, moet u voor het bijwerken van de bijbehorende apparaten Tags met dubbele voor IoT Hub-apparaat. Werk de **IOT Edge implementatie-id** en **IOT Edge implementatie prioriteit** bij in de geavanceerde instellingen. Zie [inzicht IOT Edge automatische implementaties](module-deployment-monitoring.md)voor meer informatie over het maken van een implementatie voor meerdere apparaten.

11. Selecteer **Opslaan** om uw wijzigingen op te slaan in de nieuwe release pijplijn. Ga terug naar de pijplijn weergave door **pijp lijn** te selecteren in het menu. 

### <a name="create-production-stage"></a>Productie fase maken

Maak een tweede fase in uw release pijplijn voor productie-implementatie. 

1. Maak een tweede fase voor de productie door de QA-fase te klonen. Beweeg de cursor over de QA-fase en selecteer vervolgens de knop klonen. 

    ![Kloon-fase](./media/how-to-ci-cd/clone-stage.png)

2. Selecteer de nieuwe fase **copy of QA**, waarmee u de eigenschappen ervan kunt openen. Wijzig de naam van het stadium in **Prod**voor productie. Sluit het venster met de eigenschappen van de fase. 

3. Als u de taken voor het taak venster PROD-fase wilt openen, selecteert u **taken** in het menu pijp lijn en kiest u vervolgens de fase **Prod** . 

4. Selecteer de Azure IoT Edge taak om in te stellen als voor uw productie omgeving. De implementatie-instellingen zijn waarschijnlijk hetzelfde voor QA en voor PROD, behalve dat u een ander apparaat of een set apparaten in productie wilt richten. Werk het veld apparaat-ID of de velden doel waarde en implementatie-ID voor uw productie apparaten bij. 

5. Sla het bestand op met de knop **Opslaan** . En selecteer vervolgens **pijp lijn** om terug te gaan naar de pijplijn weergave.
    
6. De manier waarop deze release pijplijn op dit moment is geconfigureerd, wordt door het maken van artefacten het **kwaliteits** stadium geactiveerd **en vervolgens elke** keer dat een nieuwe build wordt voltooid. Normaal gesp roken wilt u echter bepaalde test cases op de QA-apparaten integreren en de implementatie voor productie hand matig goed keuren. Gebruik de volgende stappen om een goedkeurings voorwaarde te maken voor de fase PROD:

    1. Open het paneel instellingen **voor waarden voorafgaand aan implementatie** .

        ![Open vóór de implementatie-voorwaarden](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Schakel de voor waarde voor **goed keuring vooraf** in op **ingeschakeld**. Voeg een of meer gebruikers of groepen toe in het veld **goed keurders** en pas alle andere goedkeurings beleidsregels aan die u wilt aanpassen. Als u uw wijzigingen wilt opslaan, sluit u het paneel voor waarden voor voor bereiding.
    
       ![Voorwaarden instellen](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


7. Sla uw release pijplijn op met behulp van de knop **Opslaan** . 

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Controleer of IoT Edge CI/CD met de build en pipelines vrijgeven

Voor het activeren van een build-taak, kunt u een wijziging naar de opslagplaats broncode pushen of deze handmatig te activeren. In deze sectie kunt u hand matig de CI/CD-pijp lijn activeren om te testen dat deze werkt. Controleer vervolgens of de implementatie slaagt.

1. Ga naar de build-pijp lijn die u aan het begin van dit artikel hebt gemaakt. 

2. U kunt een build-taak in uw build-pijp lijn activeren door de knop **Queue** te selecteren als de volgende scherm afbeelding.

    ![Handmatige trigger](./media/how-to-ci-cd/manual-trigger.png)

3. Selecteer de taak maken om de voortgang te bekijken. Als de build-pijp lijn met succes is voltooid, wordt een release-naar- **QA** -fase geactiveerd. 

    ![Buildlogboeken](./media/how-to-ci-cd/build-logs.png)

4. Met de geslaagde implementatie naar **QA** -fase wordt een melding naar de goed keurder geactiveerd. Controleer of de modules zijn geïmplementeerd op het apparaat of de apparaten waarop u het kwaliteits stadium hebt gericht. Ga vervolgens naar de release-pijp lijn en geef goed keuring voor de release op door de knop **productie order** te selecteren en vervolgens **goed keuren**te selecteren. 

    ![Wachten op goedkeuring](./media/how-to-ci-cd/pending-approval.png)

5. Nadat de fiatteur deze wijziging hebt goedgekeurd, kan deze worden geïmplementeerd voor **PROD**.

## <a name="next-steps"></a>Volgende stappen

* Inzicht in de IoT Edge-implementatie in [inzicht in IoT Edge-implementaties voor individuele apparaten of op schaal](module-deployment-monitoring.md)
* Doorloop de stappen voor het maken, bijwerken of verwijderen van een implementatie in [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).

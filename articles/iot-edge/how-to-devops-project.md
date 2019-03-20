---
title: CI/CD-pijplijn met Azure DevOps Projects - Azure IoT Edge | Microsoft Docs
description: Azure DevOps Projects zorgt ervoor dat u eenvoudig aan de slag kunt met Azure. Hiermee kunt u een Azure IoT Edge-app van uw keuze in enkele snelle stappen starten.
author: shizn
manager: ''
ms.author: xshi
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 02977032c5975de4098600ddbebccfcbb9b0fafd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58122889"
---
# <a name="create-a-cicd-pipeline-for-iot-edge-with-azure-devops-projects-preview"></a>Een CI/CD-pijplijn maken voor IoT Edge met Azure DevOps Projects (Preview)

Continue integratie (CI) en continue levering (CD) voor uw IoT Edge-toepassing configureren met DevOps-projecten. Azure DevOps Projects vereenvoudigt de eerste configuratie van een build- en release-pijplijn in Azure Pipelines.

Als u nog geen actief abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

DevOps Projects maakt een CI/CD-pijplijn in Azure DevOps. U kunt een nieuwe Azure DevOps-organisatie maken of een bestaande organisatie gebruiken. Met DevOps Projects worden ook Azure-resources gemaakt in het Azure-abonnement van uw keuze.

1. Meld u aan bij de [Microsoft Azure Portal](https://portal.azure.com).

1. Selecteer in het linkerdeelvenster de optie **Een resource maken**, en zoek vervolgens naar **DevOps Projects**.  

1.  Selecteer **Maken**.

## <a name="create-a-new-application-pipeline"></a>Een nieuwe toepassing-pijplijn 

1. Uw Azure IoT Edge-modules kunnen worden geschreven in [ C# ](tutorial-csharp-module.md), [Node.js](tutorial-node-module.md), [Python](tutorial-python-module.md), [C](tutorial-c-module.md) en [Java](tutorial-java-module.md). Selecteer de taal van uw voorkeur om een nieuwe toepassing te starten: **.NET**, **Node.js**, **Python**, **C**, of **Java**. Selecteer **Volgende** om door te gaan.

   ![Selecteer de taal om een nieuwe toepassing te maken](./media/how-to-devops-project/select-language.png)

2. Selecteer **eenvoudige IoT (Preview)** als uw toepassingsframework, en selecteer vervolgens **volgende**.

   ![Eenvoudige IoT framework selecteren](media/how-to-devops-project/select-iot.png)

3. Selecteer **IoT Edge** als de Azure-service die uw toepassing wordt geïmplementeerd, en selecteer vervolgens **volgende**.

   ![IoT Edge-service selecteren](media/how-to-devops-project/select-iot-edge.png)

4. Maak gratis een nieuwe Azure DevOps-organisatie of kies een bestaande organisatie.

   1. Geef een naam voor uw project. 

   2. Selecteer uw Azure DevOps-organisatie. Als u een bestaande organisatie hebt, selecteert u **extra instellingen** naar een nieuwe maken. 

   3. Selecteer uw Azure-abonnement.

   4. Gebruik de naam van de IoT-Hub die is gegenereerd door de projectnaam van uw, of geef uw eigen.

   5. Selecteer **extra instellingen** het configureren van de Azure-resources die DevOps Projects namens u worden gemaakt.

   6. Selecteer **gedaan** voltooid-project te maken. 

   ![Geef de naam en -toepassing maken](media/how-to-devops-project/select-devops.png)

Na enkele minuten wordt het DevOps Projects-dashboard weergegeven in de Azure-portal. Selecteer de naam van uw project de voortgang wilt bekijken. Mogelijk moet u de pagina vernieuwd. Een voorbeeld van een IoT Edge-toepassing kan worden gedaan in een opslagplaats in uw organisatie Azure DevOps, een build wordt uitgevoerd en uw toepassing is geïmplementeerd op het IoT Edge-apparaat. Dit dashboard biedt meer inzicht in uw codeopslagplaats, CI/CD-pijplijn en toepassing in Azure.

   ![De toepassing weergeven in de DevOps-portal](./media/how-to-devops-project/devops-portal.png)


## <a name="commit-code-changes-and-execute-cicd"></a>Codewijzigingen doorvoeren en CI/CD uitvoeren

DevOps Projects gemaakt een Git-opslagplaats voor uw project in Azure-opslagplaatsen. In deze sectie maakt u weergeven van de opslagplaats en codewijzigingen aanbrengen in uw toepassing.

1. Ga naar de opslagplaats voor uw project gemaakt, selecteert u **opslagplaatsen** in het menu van het projectdashboard van uw.  

   ![Weergave-opslagplaats die is gegenereerd in Azure-opslagplaatsen](./media/how-to-devops-project/view-repositories.png)

2. De volgende stappen maakt via de webbrowser gebruiken voor het aanbrengen van wijzigingen in de code. Als u uw opslagplaats lokaal in plaats daarvan klonen wilt, selecteert u **kloon** vanaf de bovenkant van het venster. De opgegeven URL gebruiken voor het klonen van de Git-opslagplaats in Visual Studio Code of uw favoriete ontwikkelprogramma. 

3. De opslagplaats bevat al de code voor een module met de naam **SampleModule** op basis van de taal van de toepassing die u hebt gekozen tijdens het proces. Open de **modules/SampleModule/module.json** bestand.

   ![Open module.json-bestand in Azure-opslagplaatsen](./media/how-to-devops-project/open-module-json.png)

4. Selecteer **bewerken**, en breng een wijziging in `"version"` onder de `"tag"`. Bijvoorbeeld, kunt u deze bijwerken naar `"version": "${BUILD_BUILDID}"` gebruiken [Azure DevOps bouwen variabelen](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=vsts#build-variables) als onderdeel van de afbeeldingscode van uw Azure IoT Edge-module.

   ![Versie voor het accepteren van build variabelen bewerken](media/how-to-devops-project/update-module-json.png)

5. Selecteer **Doorvoeren** en sla de wijzigingen op.

6. In uw browser, Ga terug naar uw dashboard DevOps-projecten in Azure portal. Als het goed is, ziet u nu dat er een build wordt gemaakt. De aangebrachte wijzigingen worden automatisch gebouwd en geïmplementeerd via een CI/CD-pijplijn.

    ![De status in uitvoering weergeven](media/how-to-devops-project/ci-cd-in-progress.png)

## <a name="examine-the-cicd-pipeline"></a>De CI/CD-pijplijn onderzoeken

In de vorige secties, Azure DevOps Projects automatisch geconfigureerd voor een volledige CI/CD-pijplijn voor uw IoT Edge-toepassing. Vervolgens u die build-pijplijn getest door het doorvoeren van wijzigingen aan een van de bestanden. Nu verkennen en aanpassen van de pijplijn, indien nodig. Ga als volgt te werk om vertrouwd te raken met de build- en release-pijplijnen van Azure DevOps.

1. Als u wilt weergeven van het bouwen van pijplijnen in uw DevOps-project, selecteer **pijplijnen bouwen** in het menu van het projectdashboard van uw. Met deze koppeling worden een browsertabblad en de Azure DevOps build-pijplijn van Azure DevOps voor het nieuwe project geopend.

   ![Weergave pijplijnen in Azure pijplijnen bouwen](./media/how-to-devops-project/view-build-pipelines.png)

2. Selecteer **Bewerken**.

    ![Build-pijplijn bewerken](media/how-to-devops-project/click-edit-button.png)

3. In het deelvenster dat wordt geopend, kunt u de taken die zich voordoen wanneer uw build-pijplijnuitvoeringen controleren. De build-pijplijn voert verschillende taken, zoals het ophalen van de bronnen van de Git-opslagplaats, het maken van IoT Edge module-installatiekopieën, IoT Edge-modules naar een containerregister pushen en het publicatieproces weergeeft die worden gebruikt voor implementaties. Zie voor meer informatie over Azure IoT Edge-taken in Azure DevOps, [Azure-pijplijnen voor continue integratie configureren](how-to-ci-cd.md#configure-continuous-integration).

4. Selecteer de **pijplijn** header aan de bovenkant van de build-pijplijn in de details van de pijplijn openen. Wijzig de naam van uw build-pijplijn in iets meer beschrijvende.

   ![De details van de pijplijn bewerken](./media/how-to-devops-project/edit-build-pipeline.png)

5. Selecteer **opslaan en in de wachtrij**, en selecteer vervolgens **opslaan**.

6. Selecteer in het menu van uw build-pijplijn **Triggers** in het menu. In DevOps Projects is automatisch een CI-trigger gemaakt en met elke doorvoering naar de opslagplaats wordt een nieuwe build gestart.  U kunt desgewenst kiezen of u vertakkingen van het CI-proces wilt opnemen of uitsluiten.

7. Selecteer **Retentie**. Afhankelijk van het scenario kunt u beleidsregels opgeven om een bepaald aantal builds te behouden of te verwijderen.

8. Selecteer **geschiedenis**. Het deelvenster van de geschiedenis bevat een audittrail van recente wijzigingen in de build. Azure Pipelines houdt alle wijzigingen in de build-pijplijn bij en biedt de mogelijkheid om versies te vergelijken.

9. Wanneer u klaar bent voor het verkennen van de build-pijplijn, gaat u naar de bijbehorende release-pijplijn. Selecteer **Releases** onder **pijplijnen**en selecteer vervolgens **bewerken** om de details van de pijplijn weer te geven.

    ![Weergave release-pijplijn](media/how-to-devops-project/release-pipeline.png)

10. Onder **Artefacten** selecteert u **Neerzetten**. De bron die dit artefact bewaakt, is de uitvoer van de build-pijplijn die u in de vorige stappen hebt onderzocht. 

11. Naast de **neerzetten** pictogram en selecteer de **trigger voor continue implementatie** dat ziet eruit als een lightning-bolt. Deze release-pijplijn kan de trigger, die een implementatie wordt uitgevoerd telkens als er is een nieuwe build-artefact beschikbaar is ingeschakeld. U kunt de trigger eventueel uitschakelen zodat de implementaties handmatig moeten worden uitgevoerd.  

12. Selecteer in het menu voor uw release-pijplijn **taken** en kies vervolgens de **dev** fase in de vervolgkeuzelijst. DevOps Projects een release-fase voor u gemaakt dat een IoT-hub maakt, maakt u een IoT Edge-apparaat in de hub, de voorbeeldmodule op basis van de build-pijplijn implementeert en richt een virtuele machine uit te voeren als uw IoT Edge-apparaat. Zie voor meer informatie over Azure IoT Edge-taken voor CD, [configureren Azure pijplijnen voor continue implementatie](how-to-ci-cd.md#configure-continuous-deployment).

    ![Continue implementatie-taken weergeven](media/how-to-devops-project/dev-release.png)

13. Selecteer aan de rechterkant **Releases weergeven**. In deze weergave wordt een versiesgeschiedenis weergegeven.

14. Selecteer de naam van een release voor meer informatie hierover.


## <a name="clean-up-resources"></a>Resources opschonen

U kunt Azure App Service en gerelateerde resources die u hebt gemaakt, verwijderen wanneer u ze niet meer nodig hebt. Gebruik de functionaliteit **Verwijderen** op het DevOps Projects-dashboard.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de taken voor Azure IoT Edge op Azure DevOps in [continue integratie en continue implementatie voor Azure IoT Edge](how-to-ci-cd.md)
* Inzicht in de IoT Edge-implementatie in [inzicht in IoT Edge-implementaties voor individuele apparaten of op schaal](module-deployment-monitoring.md)
* Doorloop de stappen voor het maken, bijwerken of verwijderen van een implementatie in [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).

---
title: CI/CD-pijplijn maken voor IoT Edge met Azure DevOps Projects (Preview) | Microsoft Docs
description: Azure DevOps Projects zorgt ervoor dat u eenvoudig aan de slag kunt met Azure. Hiermee kunt u een Azure IoT Edge-app van uw keuze in enkele snelle stappen starten.
author: shizn
manager: ''
ms.author: xshi
ms.date: 12/04/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a394951de833ee8c01bb4a385c5ebb126ebd3534
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52882591"
---
# <a name="create-a-cicd-pipeline-for-iot-edge-with-azure-devops-projects-preview"></a>Een CI/CD-pijplijn maken voor IoT Edge met Azure DevOps Projects (Preview)

Continue integratie (CI) en continue levering (CD) voor uw IoT Edge-toepassing configureren met DevOps-projecten. Azure DevOps Projects vereenvoudigt de eerste configuratie van een build- en release-pijplijn in Azure Pipelines.

Als u nog geen actief abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

DevOps Projects maakt een CI/CD-pijplijn in Azure DevOps. U kunt een nieuwe Azure DevOps-organisatie maken of een bestaande organisatie gebruiken. Met DevOps Projects worden ook Azure-resources gemaakt in het Azure-abonnement van uw keuze.

1. Meld u aan bij de [Microsoft Azure Portal](https://portal.azure.com).

1. Kies in het linkerdeelvenster het pictogram **Een resource maken** in de linkernavigatiebalk en zoek naar **DevOps Projects**.  

1.  Selecteer **Maken**.

## <a name="select-a-sample-application-and-azure-service"></a>Een voorbeeldtoepassing en Azure-service selecteren

1. Uw Azure IoT Edge-modules kunnen worden geschreven in [ C# ](tutorial-csharp-module.md), [Node.js](tutorial-node-module.md), [Python](tutorial-python-module.md), [C](tutorial-c-module.md) en [Java](tutorial-java-module.md). Selecteer de taal van uw voorkeur om een nieuwe toepassing te starten. Dienovereenkomstig, kunt u **.NET**, **Node.js**, **Python**, **C**, of **Java**, en klik vervolgens op **Volgende**.

    ![Taal selecteren](./media/how-to-devops-project/select-language.png)

2. Selecteer **eenvoudige IoT (Preview)**, en klik vervolgens op **volgende**.

    ![Selecteer IoT](media/how-to-devops-project/select-iot.png)

3. Selecteer **IoT Edge**, en klik vervolgens op **volgende**.

    ![Selecteer IoT Edge](media/how-to-devops-project/select-iot-edge.png)

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps en een Azure-abonnement configureren

1. Maak gratis een nieuwe Azure DevOps-organisatie of kies een bestaande organisatie.

    a. Kies een naam voor het project. 

    b. Selecteer uw Azure-abonnement en locatie, kies een naam voor de toepassing en selecteer vervolgens **Gereed**.  

    ![Selecteer devops](media/how-to-devops-project/select-devops.png)

1. Na enkele minuten wordt het DevOps Projects-dashboard weergegeven in de Azure-portal. Een voorbeeld van een IoT Edge-toepassing kan worden gedaan in een opslagplaats in uw organisatie Azure DevOps, een build wordt uitgevoerd en uw toepassing is geïmplementeerd op het IoT Edge-apparaat. Dit dashboard biedt meer inzicht in uw codeopslagplaats, CI/CD-pijplijn en toepassing in Azure.

    ![DevOps-portal](./media/how-to-devops-project/devops-portal.png)


## <a name="commit-code-changes-and-execute-cicd"></a>Codewijzigingen doorvoeren en CI/CD uitvoeren

Met DevOps Projects is een Git-opslagplaats gemaakt in Azure Repos of in GitHub. Als u wilt weergeven van de opslagplaats en codewijzigingen aanbrengen in uw toepassing, moet u de volgende stappen uitvoeren:

1. Selecteer aan de linkerkant van het DevOps Projects-dashboard de koppeling voor uw **master** branch.  
Met deze koppeling opent u een weergave in de zojuist gemaakte Git-opslagplaats.

1. Als u de kloon-URL van de opslagplaats wilt weergeven, selecteert u **Klonen** in de rechterbovenhoek van de browser. U kunt uw Git-opslagplaats in VS Code of andere u favoriete hulpprogramma's kunt klonen. In de volgende stappen u de webbrowser gebruiken om u te maken en commit-code rechtstreeks naar de master-vertakking wordt gewijzigd.

    ![Klonen](media/how-to-devops-project/clone.png)

1. Aan de linkerkant van de browser, Ga naar de **modules/FilterModule/module.json** bestand.

1. Selecteer **bewerken**, en breng een wijziging in `"version"` onder de `"tag"`. Bijvoorbeeld, kunt u deze bijwerken naar `"version": "${BUILD_BUILDID}"` gebruiken [Azure DevOps bouwen variabelen](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=vsts#build-variables) als onderdeel van de afbeeldingscode van uw Azure IoT Edge-module.

    ![Bewerken](media/how-to-devops-project/update-module-json.png)

1. Selecteer **Doorvoeren** en sla de wijzigingen op.

1. Ga in uw browser naar het Azure DevOps-projectdashboard.  Als het goed is, ziet u nu dat er een build wordt gemaakt. De aangebrachte wijzigingen worden automatisch gebouwd en geïmplementeerd via een CI/CD-pijplijn.

    ![Wordt uitgevoerd](media/how-to-devops-project/ci-cd-in-progress.png)

## <a name="examine-the-cicd-pipeline"></a>De CI/CD-pijplijn onderzoeken

In de vorige stap, Azure DevOps Projects automatisch geconfigureerd voor een volledige CI/CD-pijplijn voor uw IoT Edge-toepassing. U kunt de pijplijn verkennen en zo nodig aanpassen. Ga als volgt te werk om vertrouwd te raken met de build- en release-pijplijnen van Azure DevOps.

1. Selecteer boven in het DevOps Projects-dashboard de optie **Build-pijplijnen**.  
Met deze koppeling worden een browsertabblad en de Azure DevOps build-pijplijn van Azure DevOps voor het nieuwe project geopend.

1. Selecteer **Bewerken**.

    ![Bewerken](media/how-to-devops-project/click-edit-button.png)

1. In dit deelvenster kunt u de verschillende taken voor uw build-pijplijn onderzoeken. De build voert verschillende taken, zoals het ophalen van de bronnen van de Git-opslagplaats, het maken van IoT Edge module-installatiekopieën, IoT Edge-modules pushen en publiceren van uitvoer gebruikt die worden gebruikt voor implementaties. Voor meer informatie over Azure IoT Edge-taken voor CI, vindt u [Azure-pijplijnen voor continue integratie configureren](https://docs.microsoft.com/azure/iot-edge/how-to-ci-cd#configure-azure-pipelines-for-continuous-integration).

    ![CI-taken](media/how-to-devops-project/ci.png)

1. Selecteer bovenaan de build-pijplijn de naam van de build-pijplijn.

1. Wijzig de naam van de build-pijplijn in een gebruiksvriendelijkere naam. Selecteer **Opslaan en wachtrij** en selecteer **Opslaan**.

1. Selecteer onder de naam van de build-pijplijn de optie **Geschiedenis**.   
In het deelvenster **Geschiedenis** ziet u een audittrail van recente wijzigingen voor de build.  Azure Pipelines houdt alle wijzigingen in de build-pijplijn bij en biedt de mogelijkheid om versies te vergelijken.

1. Selecteer **Triggers**. In DevOps Projects is automatisch een CI-trigger gemaakt en met elke doorvoering naar de opslagplaats wordt een nieuwe build gestart.  U kunt desgewenst kiezen of u vertakkingen van het CI-proces wilt opnemen of uitsluiten.

1. Selecteer **Retentie**. Afhankelijk van het scenario kunt u beleidsregels opgeven om een bepaald aantal builds te behouden of te verwijderen.

1. Selecteer **Release** onder **pijplijnen**. DevOps Projects maakt een release-pijplijn voor het beheren van implementaties voor Azure IoT Edge.

    ![Release-pijplijn](media/how-to-devops-project/release-pipeline.png)

1. Selecteer **Bewerken**. De release-pijplijn bevat een pijplijn die het releaseproces definieert.  

1. Onder **Artefacten** selecteert u **Neerzetten**. De build-pijplijn die u in de vorige stappen hebt onderzocht, produceert de uitvoer die wordt gebruikt voor het artefact. 

1. Selecteer naast het pictogram **Neerzetten** de optie **Continue implementatietrigger**.  
Deze release-pijplijn heeft een ingeschakelde CD-trigger op basis waarvan een implementatie wordt uitgevoerd telkens wanneer een nieuw build-artefact beschikbaar is. U kunt de trigger eventueel uitschakelen zodat de implementaties handmatig moeten worden uitgevoerd.  

1. Selecteer aan de linkerkant **Taken**. De taken zijn de acties die tijdens het implementatieproces worden uitgevoerd. In dit voorbeeld is een taak gemaakt voor het implementeren van uw module-installatiekopieën op Azure IoT Edge. Voor meer informatie over Azure IoT Edge-taken voor CD, vindt u [configureren Azure pijplijnen voor continue implementatie](https://docs.microsoft.com/azure/iot-edge/how-to-ci-cd#configure-azure-pipelines-for-continuous-deployment).

    ![CD](media/how-to-devops-project/dev-release.png)

1. Selecteer aan de rechterkant **Releases weergeven**. In deze weergave wordt een versiesgeschiedenis weergegeven.

1. Selecteer het beletselteken (...) naast een van de releases en selecteer vervolgens **Openen**.  
U kunt verschillende menu's verkennen, zoals een releaseoverzicht, gekoppelde werkitems en tests.

1. Selecteer **Doorvoeringen**. In deze weergave worden de codedoorvoeringen weergegeven die zijn gekoppeld aan deze implementatie. 

1. Selecteer **Logboeken**. De logboeken bevatten nuttige informatie over het implementatieproces. Ze kunnen worden weergegeven tijdens en na de implementaties.


## <a name="clean-up-resources"></a>Resources opschonen

U kunt Azure App Service en gerelateerde resources die u hebt gemaakt, verwijderen wanneer u ze niet meer nodig hebt. Gebruik de functionaliteit **Verwijderen** op het DevOps Projects-dashboard.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de taken voor Azure IoT Edge op Azure DevOps in [continue integratie en continue implementatie voor Azure IoT Edge](how-to-ci-cd.md)
* Inzicht in de IoT Edge-implementatie in [inzicht in IoT Edge-implementaties voor individuele apparaten of op schaal](module-deployment-monitoring.md)
* Doorloop de stappen voor het maken, bijwerken of verwijderen van een implementatie in [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).

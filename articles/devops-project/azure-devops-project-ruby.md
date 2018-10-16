---
title: Een CI/CD-pijplijn voor Ruby on Rails maken met het Azure DevOps-project | Snelstart
description: Met het DevOps-project kunt u eenvoudig aan de slag in Azure. Hiermee kunt u een web-app in Ruby voor een Azure-service in slechts enkele stappen starten.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: ebc36aed11ebef56e04067616a7d65cf94367665
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405077"
---
# <a name="create-a-cicd-pipeline-for-ruby-on-rails-with-the-azure-devops-project"></a>Een CI/CD-pijplijn voor Ruby on Rails maken met het Azure DevOps-project

Configureer continue integratie (CI) en continue levering (CD) voor uw Ruby on Rails-toepassing met het **Azure DevOps-project**.  Het Azure DevOps-project vereenvoudigt de eerste configuratie van een Azure DevOps Services-build en -release-pipeline.

Als u geen Azure-abonnement hebt, kunt u er gratis een krijgen via [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Het Azure DevOps-project maakt een CI-/CD-pijplijn in Azure DevOps Services.  U kunt een **nieuwe Azure DevOps Services**-organisatie maken of een **bestaande organisatie** gebruiken.  Het Azure DevOps-project maakt ook **Azure-resources** in het **Azure-abonnement** van uw keuze.

1. Meld u aan bij [Microsoft Azure Portal](https://portal.azure.com).

1. Kies het pictogram **Een resource maken** in de linkernavigatiebalk en zoek naar **DevOps-project**.  Kies **Maken**.

    ![Continue levering starten](_img/azure-devops-project-ruby/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Een voorbeeldtoepassing en Azure-service selecteren

1. Selecteer de **Ruby**-voorbeeldtoepassing.

1. Selecteer het **Ruby on Rails**-toepassingsframework.  Als u klaar bent, kiest u **Volgende**.

1. **Web App on Linux** is het standaardimplementatiedoel.  Desgewenst kunt u ook kiezen voor Web App for Containers.  Het toepassingsframework dat u in de vorige stappen hebt gekozen bepaalt welk type implementatiedoel hier beschikbaar is voor de Azure-service.  Selecteer de **doelservice** van uw keuze.  Als u klaar bent, kiest u **Volgende**.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Azure DevOps Services en een Azure-abonnement configureren 

1. Maak een **nieuwe** gratis Azure DevOps Services-organisatie of kies een **bestaande** organisatie.  Kies een **naam** voor uw Azure DevOps-project.  Selecteer uw **Azure-abonnement** en **locatie** en kies een **naam** voor uw toepassing.  Als u klaar bent, kiest u **Gereed**.

1. In slechts enkele minuten wordt het **Azure DevOps-projectdashboard** in de Azure Portal geladen.  Er wordt een voorbeeldtoepassing in een opslagplaats in uw Azure DevOps Services-organisatie ingesteld, er wordt een build uitgevoerd en uw toepassing wordt geïmplementeerd in Azure.  Dit dashboard biedt meer inzicht in uw **codeopslagplaats**, **Azure-CI/CD-pijplijn** en uw **toepassing in Azure**.  Aan de rechterkant van het dashboard selecteert u **Bladeren** om de lopende toepassing weer te geven.

    ![Dashboardweergave](_img/azure-devops-project-ruby/dashboardnopreview.png) 

## <a name="commit-code-changes-and-execute-cicd"></a>Codewijzigingen doorvoeren en CI/CD uitvoeren

Het Azure DevOps-project heeft een Git-opslagplaats gemaakt in uw Azure DevOps Services-organisatie of GitHub-account.  Volg de onderstaande stappen om de opslagplaats weer te geven en codewijzigingen in uw toepassing aan te brengen.

1. Selecteer aan de linkerkant van het DevOps-projectdashboard de koppeling voor uw **master** branch.  Met deze koppeling opent u een weergave in de zojuist gemaakte Git-opslagplaats.

1. Als u de kloon-URL van de opslagplaats wilt weergeven, selecteert u **Klonen** in de rechterbovenhoek van de browser. U kunt uw Git-opslagplaats klonen in uw favoriete IDE.  In de volgende stappen kunt u de webbrowser gebruiken om codewijzigingen rechtstreeks aan te brengen en door te voeren in de master branch.

1. Aan de linkerkant van de browser navigeert u naar het bestand **app/views/pages/home.html.erb**.

1. Selecteer **Bewerken** en breng een wijziging aan in de tekst.  Wijzig bijvoorbeeld de tekst in een van de div-tags.

1. Kies **Doorvoeren** en sla uw wijzigingen op.

1. Navigeer in uw browser naar het **Azure DevOps-projectdashboard**.  Als het goed is, ziet u nu dat er een build wordt gemaakt.  De zojuist aangebrachte wijzigingen worden automatisch in een build verwerkt en geïmplementeerd via een Azure-CI/CD-pijplijn.

## <a name="examine-the-azure-cicd-pipeline"></a>De Azure-CI/CD-pijplijn onderzoeken

Het Azure DevOps-project heeft automatisch een volledige Azure-CI/CD-pijplijn geconfigureerd in uw Azure DevOps Services-organisatie.  U kunt de pijplijn verkennen en zo nodig aanpassen.  Volg de onderstaande stappen om vertrouwd te raken met de Azure DevOps Services-build- en -release-pipelines.

1. Selecteer **Build-pipelines** **bovenin** het Azure DevOps-projectdashboard.  Met deze koppeling opent u een tabblad in de browser met de Azure DevOps Services-build-pipeline voor het nieuwe project.

1. Selecteer het **weglatingsteken**.  Met deze actie wordt een menu geopend waarin u diverse acties kunt starten, zoals een nieuwe build in de wachtrij plaatsen, een build onderbreken en de build-pipeline bewerken.

1. Selecteer **Bewerken**.

1. In deze weergave **onderzoekt u de verschillende taken** voor uw build-pipeline.  De build voert verschillende taken uit, zoals het ophalen van bronnen uit de Git-opslagplaats, het herstellen van afhankelijkheden en het publiceren van uitvoer die wordt gebruikt voor implementaties.

1. Selecteer bovenaan de build-pipeline de **naam van de build-pipeline**.

1. Wijzig de **naam** van de build-pipeline in een gebruiksvriendelijkere naam.  Selecteer **Opslaan en wachtrij** en selecteer vervolgens **Opslaan**.

1. Selecteer onder de naam van de build-pipeline de optie **Geschiedenis**.  U ziet een audittrail van recente wijzigingen voor de build.  Azure DevOps Services houdt alle wijzigingen in de build-pipeline bij en biedt de mogelijkheid om versies te vergelijken.

1. Selecteer **Triggers**.  Het Azure DevOps-project maakt automatisch een CI-trigger en met elke doorvoering naar de opslagplaats wordt een nieuwe build gestart.  U kunt desgewenst kiezen of u vertakkingen van het CI-proces wilt opnemen of uitsluiten.

1. Selecteer **Retentie**.  Op basis van uw scenario kunt u beleidsregels opgeven om een bepaald aantal builds te behouden of te verwijderen.

1. Selecteer **Build en release** en kies **Releases**.  Het Azure DevOps-project heeft een Azure DevOps Services-release-pipeline gemaakt om implementaties naar Azure te beheren.

1. Selecteer links van de browser het **beletselteken** naast uw release-pipeline en kies **Bewerken**.

1. De release-pipeline bevat een **pijplijn** die het releaseproces definieert.  Onder **Artefacten** selecteert u **Neerzetten**.  De build-pipeline die u in de vorige stappen hebt onderzocht, produceert de uitvoer die wordt gebruikt voor het artefact. 

1. Selecteer rechts van het pictogram **Neerzetten** het pictogram **Continue implementatietrigger**.  Deze release-pipeline heeft een ingeschakelde CD-trigger die een implementatie uitvoert telkens wanneer er een nieuw build-artefact beschikbaar is.  U kunt de trigger desgewenst uitschakelen, zodat uw implementaties handmatig moeten worden uitgevoerd. 

1. Selecteer aan de linkerkant van de browser de optie **Taken**.  De taken zijn de acties die in het implementatieproces worden uitgevoerd.  In dit voorbeeld is een taak gemaakt om te implementeren naar **Azure App Service**.

1. Selecteer aan de rechterkant van de browser de optie **Releases weergeven**.  In deze weergave wordt een versiesgeschiedenis weergegeven.

1. Selecteer het **weglatingsteken** naast een van uw versies en kies **Openen**.  Er zijn verschillende menu's die u in deze weergave kunt verkennen, zoals een versieoverzicht, gekoppelde werkitems en tests.

1. Selecteer **Doorvoeringen**.  In deze weergave worden de codedoorvoeringen getoond die zijn gekoppeld aan de specifieke implementatie. 

1. Selecteer **Logboeken**.  De logboeken bevatten nuttige informatie over het implementatieproces.  Ze kunnen worden weergegeven tijdens en na de implementaties.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de Azure App Service en relevante resources die in deze snelstart zijn gemaakt niet meer nodig hebt, kunt u ze verwijderen met de functie **Verwijderen** in het Azure DevOps-projectdashboard.

## <a name="next-steps"></a>Volgende stappen

Zie deze zelfstudie voor meer informatie over het wijzigen van de build- en release-pipelines in overeenstemming met de behoeften van uw team:

> [!div class="nextstepaction"]
> [CD-proces aanpassen](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

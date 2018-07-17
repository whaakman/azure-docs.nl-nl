---
title: Uw ASP.NET Core-app implementeren naar AKS (Azure Kubernetes Service) met het Azure DevOps-project | VSTS-zelfstudie
description: Met het DevOps-project kunt u eenvoudig aan de slag in Azure. Met het Azure DevOps-project kunt u een ASP.NET-Core app eenvoudig in slechts enkele stappen implementeren naar AKS (Azure Kubernetes Service).
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 98c585cc0c6136496fe190cd3eceb27fd856af21
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967331"
---
# <a name="tutorial--deploy-your-aspnet-core-app-to-azure-kubernetes-service-aks-with-the-azure-devops-project"></a>Zelfstudie: Uw ASP.NET Core-app implementeren naar AKS (Azure Kubernetes Service) met het Azure DevOps-project

Het Azure DevOps-project geeft een vereenvoudigde ervaring waar u uw bestaande code en Git-opslagplaats gebruikt of een van de voorbeeldtoepassingen kiest voor het maken van een CI (Continue integratie) en CD-pijplijn (Continue levering) naar Azure.  Het DevOps-project maakt automatisch Azure-resources zoals AKS, maakt en configureert een versiepijplijn in VSTS met een builddefinitie voor CI, stelt een versiedefinitie in voor CD en maakt vervolgens een Azure Application Insights-resource voor bewaking.

U gaat het volgende doen:

> [!div class="checklist"]
> * Een Azure DevOps-project maken voor een ASP.NET Core-app en AKS
> * VSTS en een Azure-abonnement configureren 
> * Het AKS-cluster bestuderen
> * De definitie van de build van VSTS-CI bestuderen
> * De definitie van het Release Management van VSTS-CD bestuderen
> * Wijzigingen doorvoeren in VSTS en automatisch implementeren naar Azure
> * Resources opschonen

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. U kunt er een gratis krijgen via [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-core-app-and-aks"></a>Een Azure DevOps-project maken voor een ASP.NET Core-app en AKS

Het Azure DevOps-project maakt een CI-/CD-pijplijn in VSTS.  U kunt een **nieuw VSTS**-account maken of een **bestaand account** gebruiken.  Het Azure DevOps-project maakt ook **Azure-resources**, zoals een AKS-cluster, in het **Azure-abonnement** van uw keuze.

1. Meld u aan bij [Microsoft Azure Portal](https://portal.azure.com).

1. Kies het pictogram **Een resource maken** in de linkernavigatiebalk en zoek naar **DevOps-project**.  Kies **Maken**.

    ![Continue levering starten](_img/azure-devops-project-aks/fullbrowser.png)

1. Selecteer **.NET** en kies **Volgende**.

1. Bij **Een toepassingsframework kiezen** selecteert u **ASP.NET Core**. Selecteer daarna **Volgende**.

1. Selecteer **Kubernetes-service** en kies **Volgende**.  

## <a name="configure-vsts-and-an-azure-subscription"></a>VSTS en een Azure-abonnement configureren

1. Maak een **nieuw** VSTS-account of kies een **bestaand** account.  Kies een **naam** voor uw VSTS-project.  

1. Selecteer uw **Azure-abonnement**.

1. Selecteer de koppeling **Wijziging** om aanvullende Azure-configuratie-instellingen te zien, en zoek het **aantal knooppunten** voor het **Kubernetes-cluster**.  Hier vindt u verschillende opties voor het configureren van het type en de locatie van de Azure-services.
 
1. Verlaat het Azure-configuratiegebied en kies **Gereed**.

1. Het duurt enkele minuten voor het proces is voltooid.  Een voorbeeld-ASP.NET Core-toepassing wordt in een opslagplaats in uw VSTS-account ingesteld; een AKS-cluster wordt gemaakt, een build en versie worden uitgevoerd en uw toepassing wordt geïmplementeerd naar Azure.  

    Wanneer u klaar bent, wordt het **dashboard** voor het Azure DevOps-project geladen in Azure Portal.  U kunt ook rechtstreeks vanuit **Alle resources** in **Azure Portal** naar het **dashboard van het Azure DevOps-project** navigeren.  

    Dit dashboard biedt meer inzicht in uw VSTS-**codeopslagplaats**, **VSTS-CI/CD-pijplijn** en uw **AKS-cluster**.  U kunt verder aanvullende opties configureren in VSTS.  Aan de rechterkant van het dashboard selecteert u **Bladeren** om de lopende toepassing weer te geven.

## <a name="examine-the-aks-cluster"></a>Het AKS-cluster bestuderen

Een AKS-cluster wordt automatisch geconfigureerd door het Azure DevOps-project.  U kunt het cluster verkennen en aanpassen.  Volg de stappen hieronder om vertrouwd te raken met AKS.

1. Navigeer naar het dashboard van het **Azure DevOps-project**.

1. Aan de rechterkant van het projectdashboard van DevOps selecteert u de **Kubernetes-service**.

1. Er wordt een blade voor het AKS-cluster geopend.  In deze weergave kunt u verschillende acties uitvoeren, zoals **de containerstatus bewaken**, **zoeken in logboeken** en het **Kubernetes-dashboard** openen.

1. Selecteer aan de rechterkant van het scherm de optie **Kubernetes-dashboard weergeven**.  Volg desgewenst de stappen om het Kubernetes-dashboard te openen.

## <a name="examine-the-vsts-ci-build-definition"></a>De definitie van de build van VSTS-CI bestuderen

Het Azure DevOps-Project configureert automatisch een volledige VSTS-CI-/CD-pijplijn in uw VSTS-account.  U kunt de pijplijn verkennen en aanpassen.  Volg de stappen hieronder om vertrouwd te raken met de definitie van de VSTS-build.

1. Navigeer naar het dashboard van het **Azure DevOps-project**.

1. Selecteer **Pijplijnen bouwen** **bovenin** het **Azure DevOps-projectdashboard**.  Met deze koppeling opent u een tabblad in de browser met de definitie van de VSTS-build voor het nieuwe project.

1. Beweeg de muisaanwijzer naar de rechterkant van de builddefinitie naast het veld **Status**. Selecteer het **weglatingsteken** dat wordt weergegeven.  Met deze actie opent u een menu waar u diverse acties kunt uitvoeren zoals **Een nieuwe build in de wachtrij plaatsen**, **Een build onderbreken** en **De builddefinitie bewerken**.

1. Selecteer **Bewerken**.

1. In deze weergave **onderzoekt u de verschillende taken** voor uw builddefinitie.  De build voert verschillende taken uit, zoals het ophalen van bronnen uit de VSTS Git-opslagplaats, het herstellen van afhankelijkheden en het publiceren van uitvoer die wordt gebruikt voor implementaties.

1. Selecteer boven de builddefinitie de **naam van de builddefinitie**.

1. Wijzig de **naam** van uw builddefinitie in een gebruiksvriendelijkere naam.  Selecteer **Opslaan en wachtrij** en selecteer vervolgens **Opslaan**.

1. Selecteer onder de naam van uw builddefinitie de optie **Geschiedenis**.  U ziet een audittrail van recente wijzigingen voor de build.  VSTS houdt alle wijzigingen in de builddefinitie bij en biedt de mogelijkheid om versies te vergelijken.

1. Selecteer **Triggers**.  Het Azure DevOps-project maakt automatisch een CI-trigger en met elke doorvoering naar de opslagplaats wordt een nieuwe build gestart.  U kunt desgewenst kiezen of u vertakkingen van het CI-proces wilt opnemen of uitsluiten.

1. Selecteer **Retentie**.  Op basis van uw scenario kunt u beleidsregels opgeven om een bepaald aantal builds te behouden of te verwijderen.

## <a name="examine-the-vsts-cd-release-management-definition"></a>De definitie van het Release Management van VSTS-CD bestuderen

Het Azure DevOps-project maakt en configureert automatisch de benodigde stappen om vanuit uw VSTS-account te implementeren naar uw Azure-abonnement.  Deze stappen omvatten het configureren van een Azure-serviceverbinding om VSTS te verifiëren bij uw Azure-abonnement.  Er wordt ook automatisch een VSTS-versiedefinitie gemaakt; deze verstrekt de CD naar de Azure-VM.  Volg de onderstaande stappen voor meer informatie over de VSTS-releasedefinitie.

1. Selecteer **Build en release** en kies **Releases**.  Het Azure DevOps-project heeft een VSTS-versiedefinitie gemaakt om implementaties naar Azure te beheren.

1. Selecteer links van de browser het **weglatingsteken** naast uw releasedefinitie en kies **Bewerken**.

1. De releasedefinitie bevat een **pijplijn** die het versieproces definieert.  Onder **Artefacten** selecteert u **Neerzetten**.  De builddefinitie die u in de vorige stappen hebt onderzocht, produceert de uitvoer die wordt gebruikt voor het artefact. 

1. Selecteer rechts van het pictogram **Neerzetten** het **pictogram** **Continue implementatietrigger** (zie eruit als een bliksemflits.)  Deze versiedefinitie heeft een ingeschakelde CD-trigger.  Telkens wanneer er een nieuwe build-artefact beschikbaar is, maakt de trigger een implementatie.  U kunt de trigger desgewenst uitschakelen, zodat uw implementaties handmatig moeten worden uitgevoerd. 

1. Selecteer aan de rechterkant van de browser de optie **Releases weergeven**.  In deze weergave wordt een versiesgeschiedenis weergegeven.

1. Selecteer het **weglatingsteken** naast een van uw versies en kies **Openen**.  Er zijn verschillende menu's die u in deze weergave kunt verkennen, zoals een **versieoverzicht**, **gekoppelde werkitems** en **tests**.

1. Selecteer **Doorvoeringen**.  In deze weergave worden de codedoorvoeringen getoond die zijn gekoppeld aan de specifieke implementatie. U kunt versies vergelijken om de doorvoerverschillen tussen implementaties weer te geven.

1. Selecteer **Logboeken**.  De logboeken bevatten nuttige informatie over het implementatieproces.  Ze kunnen worden weergegeven tijdens en na de implementaties.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>Wijzigingen doorvoeren in VSTS en automatisch implementeren naar Azure 

 > [!NOTE]
 > Met de volgende stappen test u de CI/CD-pijplijn met een eenvoudige tekstwijziging in uw web-app.

U bent nu klaar om met een team samen te werken aan uw app met een CI/CD-proces dat automatisch uw meest recente werk aan uw website implementeert.  Elke wijziging in de VSTS git-opslagplaats start een build in VSTS, en met een VSTS-versiebeheerdefinitie worden uw wijzigingen in Azure geïmplementeerd.  Volg de onderstaande stappen of gebruik andere technieken om wijzigingen door te voeren in uw opslagplaats.  U kunt bijvoorbeeld de Git-opslagplaats in uw favoriete hulpprogramma of IDE **klonen** en wijzigingen naar deze opslagplaats pushen.

1. Selecteer **Code** en vervolgens **Bestanden** in het VSTS-menu en navigeer naar uw opslagplaats.
1. Navigeer naar de map **Views\Home**, selecteer het **weglatingsteken** naast het bestand **Index.cshtml** en kies **Bewerken**.

1. Breng een wijziging aan in het bestand; voeg bijvoorbeeld wat tekst toe in een van de **div-tags**.  Selecteer in de rechterbovenhoek **Doorvoeren**.  Selecteer nogmaals **Doorvoeren** om de wijziging te pushen. 

1. In een paar seconden wordt er in **VSTS een build gestart**, waarna er een versie wordt uitgevoerd om de wijzigingen te implementeren.  U kunt de **buildstatus** bewaken met het dashboard van het Azure DevOps-project of in de browser met uw VSTS-account.

1. Wanneer de versie is voltooid, **vernieuwt u de toepassing** in de browser om te controleren of u uw wijzigingen kunt zien.

## <a name="clean-up-resources"></a>Resources opschonen

 > [!NOTE]
 > Met de onderstaande stappen worden resources permanent verwijderd.  Gebruik deze functionaliteit alleen nadat u de prompts zorgvuldig hebt gelezen.

Als u aan het testen bent, kunt u resources opschonen om te voorkomen dat kosten oplopen.  Wanneer u het Azure Kubernetes-cluster en verwante resources die in deze zelfstudie zijn gemaakt, niet meer nodig hebt, kunt u ze verwijderen met de functionaliteit **Verwijderen** op het dashboard van het Azure DevOps-project.  **Wees voorzichtig**, want met de functionaliteit Verwijderen verwijdert u de gegevens die door het Azure DevOps-project zijn gemaakt in zowel Azure als VSTS. Als ze zijn verwijderd, kunt u ze niet meer terughalen.

1. Navigeer vanuit **Azure Portal** naar het **Azure DevOps-project**.
2. Selecteer **rechtsboven** in het dashboard de optie **Verwijderen**.  Na het lezen van de prompt selecteert u **Ja** om de resources **definitief te verwijderen**.

## <a name="next-steps"></a>Volgende stappen

U kunt deze build en versiedefinities desgewenst wijzigen om te voldoen aan de behoeften van uw team. U kunt dit CI/CD-patroon ook als een sjabloon voor uw andere projecten gebruiken.  U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Azure DevOps-project maken voor een ASP.NET Core-app en AKS
> * VSTS en een Azure-abonnement configureren 
> * Het AKS-cluster bestuderen
> * De definitie van de build van VSTS-CI bestuderen
> * De definitie van het Release Management van VSTS-CD bestuderen
> * Wijzigingen doorvoeren in VSTS en automatisch implementeren naar Azure
> * Resources opschonen

Meer informatie over het gebruik van het Kubernetes-dashboard vindt u hieronder:

> [!div class="nextstepaction"]
> [Het Kubernetes-dashboard gebruiken](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)

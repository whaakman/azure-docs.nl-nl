---
title: 'Zelfstudie: ASP.NET-apps implementeren op Azure Functions met Azure DevOps Projects'
description: Azure DevOps Projects zorgt ervoor dat u eenvoudig aan de slag kunt met Azure. Met DevOps Projects, kunt u uw ASP.NET-app implementeren op Azure Functions in een paar snelle stappen.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 06/20/2019
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 6894f9bc6c803e2692afb54d7459adf6b0e6dbd6
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828090"
---
# <a name="continuously-deploy-to-azure-functions-with-devops-projects"></a>Continu implementeren naar Azure Functions met DevOps Projects

Azure DevOps Projects biedt een vereenvoudigde ervaring waar u uw bestaande code en Git-opslagplaats gebruikt of een voorbeeldtoepassing kiest voor het maken van een CI- (Continue integratie) en CD-pijplijn (Continue levering) naar Azure.

In DevOps Projects gebeurt ook het volgende:

* Azure-resources, zoals Azure Functions wordt automatisch gemaakt

* Maakt en configureert een release-pijplijn in Azure DevOps voor CI/CD

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
>* DevOps Projects gebruiken voor het implementeren van een ASP.NET-app naar Azure-functie
>* Azure DevOps en een Azure-abonnement configureren
>* Bekijk de Azure-functie
>* De CI-pijplijn onderzoeken
>* De CD-pijplijn onderzoeken
>* Wijzigingen doorvoeren in Git en automatisch implementeren naar Azure
>* Resources opschonen

De ondersteunde runtimes voor functies zijn momenteel **.NET** en **Node.js**. We gebruiken. NET-runtime voor deze zelfstudie om te implementeren op Azure Functions. 

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. U krijgt een gratis tot [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/)

## <a name="use-devops-projects-to-deploy-an-aspnet-app-to-azure-functions"></a>DevOps Projects gebruiken voor het implementeren van een ASP.NET-app naar Azure Functions

In DevOps Projects wordt een CI/CD-pijplijn gemaakt in Azure Pipelines. U kunt een nieuwe Azure DevOps-organisatie maken of een bestaande organisatie gebruiken. DevOps Projects maakt ook Azure-resources, zoals een IoTHub, in het Azure-abonnement van uw keuze.

1. Meld u aan bij [Azure Portal](https://portal.azure.com)

1. Selecteer **Een resource maken** in het linkerdeelvenster.

1. Typ in het zoekvak **DevOps Projects**, en klik vervolgens op **toevoegen**.

   ![DevOps Projects](_img/azure-devops-project-functions/devops-project.png)

1. Selecteer **.NET** en selecteer vervolgens **Volgende**. Onder **een toepassingsframework kiezen**, selecteer **ASP.NET** en klikt u op **volgende**.

1. Selecteer **functie-App** en selecteer vervolgens **volgende**.

## <a name="configure-azure-devops-and-azure-subscription"></a>Azure DevOps en Azure-abonnement configureren

1. Voer een naam in voor uw Azure DevOps-project.

1. Maak een nieuwe Azure DevOps-organisatie of selecteer een bestaande organisatie.

1. Selecteer uw Azure-abonnement.

1. Aanvullende Azure-configuratie-instellingen weergeven en voor het identificeren van de prijzen laag en de locatie, klikt u op de overige instellingen op. Dit deelvenster geeft de verschillende opties voor het configureren van de prijzen laag en de locatie van de Azure-services.

1. Sluit het gebied van Azure-configuratie en selecteer gereed.

1. Na enkele minuten is het proces voltooid. Een voorbeeld-ASP.NET-app is ingesteld in een Git-opslagplaats in uw organisatie Azure DevOps, een functie-App en Application Insights wordt gemaakt, een CI/CD-pijplijn wordt uitgevoerd en uw app is geïmplementeerd in Azure.

   Nadat u dit alles hebt voltooid, wordt het dashboard van Azure DevOps Projects in de Azure-portal weergegeven. U kunt ook rechtstreeks vanuit **Alle resources** in de Azure-portal naar het dashboard van DevOps Projects gaan.

   Dit dashboard biedt meer inzicht in uw Azure DevOps-codeopslagplaats, uw CI/CD-pijplijn en uw Azure-functie. U kunt meer CI/CD-opties configureren in uw Azure DevOps-pijplijn. Selecteer aan de rechterkant **functie-App** om weer te geven.

## <a name="examine-the-function-app"></a>De functie-App onderzoeken

DevOps Projects configureert automatisch de functie-app die u kunt verkennen en aanpassen. Als u wilt meer te weten over de functie-app, het volgende doen:

1. Ga naar het DevOps Projects-dashboard.

    ![DevOps-projecten Dashboard](_img/azure-devops-project-functions/devops-projects-dashboard.png)

1. Selecteer de functie-app aan de rechterkant. Er wordt een deelvenster geopend voor de functie-app. In deze weergave kunt u verschillende acties, zoals operations bewaking, zoeken in Logboeken uitvoeren.

    ![Functie-app](_img/azure-devops-project-functions/function-app.png)

## <a name="examine-the-ci-pipeline"></a>De CI-pijplijn onderzoeken

DevOps Projects configureert automatisch een Azure-CI/CD-pijplijn in uw Azure DevOps-organisatie. U kunt de pijplijn verkennen en aanpassen. Ga als volgt te werk om vertrouwd te raken met de pijplijn:

1. Ga naar het DevOps Projects-dashboard.

1. Klik op de hyperlink onder **bouwen**. Op een tabblad in de browser wordt de build-pijplijn voor het nieuwe project weergegeven.

    ![Ontwikkelen](_img/azure-devops-project-functions/build.png)

1. Selecteer **Bewerken**. In dit deelvenster kunt u de verschillende taken voor uw build-pijplijn onderzoeken. De build voert verschillende taken, zoals het ophalen van de broncode van de Git-opslagplaats, het bouwen van de toepassing, eenheidstests uitgevoerd en publiceren van uitvoer die worden gebruikt voor implementaties.

1. Selecteer **Triggers**. In DevOps Projects wordt automatisch een CI-trigger gemaakt en met elke doorvoering naar de opslagplaats wordt een nieuwe build gestart. U kunt eventueel kiezen of u branches van het CI-proces wilt opnemen of uitsluiten.

1. Selecteer **Retentie**. Afhankelijk van het scenario kunt u beleidsregels opgeven om een bepaald aantal builds te behouden of te verwijderen.

1. Selecteer bovenaan de build-pijplijn de naam van de build-pijplijn.

1. Wijzig de naam van uw build-pijplijn in iets meer beschrijvende en selecteer vervolgens **opslaan** uit de **opslaan en in de wachtrij** vervolgkeuzelijst.

1. Selecteer onder de naam van de build-pijplijn de optie **Geschiedenis**. In dit deelvenster ziet u een audittrail van recente wijzigingen voor de build. Azure DevOps bijhouden van wijzigingen in de build-pijplijn en Hiermee kunt u versies vergelijken.

## <a name="examine-the-cd-release-pipeline"></a>De CD-release-pijplijn onderzoeken

In DevOps Projects worden automatisch de benodigde stappen gemaakt en geconfigureerd om vanuit uw Azure DevOps-organisatie te implementeren naar uw Azure-abonnement. Deze stappen omvatten het configureren van een Azure-serviceverbinding om Azure DevOps te verifiëren bij uw Azure-abonnement. Er wordt ook automatisch ook een release-pijplijn gemaakt, die de CD levert aan Azure. Voor meer informatie over de release-pijplijn doet u het volgende:

1. Navigeer naar de **pijplijnen | Releases**.

1. Klik op **Edit**.

1. Onder **Artefacten** selecteert u **Neerzetten**. Met de build-pijplijn die u in de vorige stappen hebt onderzocht, wordt de uitvoer geproduceerd die wordt gebruikt voor het artefact.

1. Selecteer **Continue implementatietrigger** rechts van het pictogram **Neerzetten**. Deze release-pijplijn heeft een ingeschakelde CD-trigger die een implementatie uitvoert telkens wanneer een nieuw build-artefact beschikbaar is. U kunt de trigger eventueel uitschakelen zodat de implementaties handmatig moeten worden uitgevoerd.

1. Selecteer aan de rechterkant **Versies weergeven** om een versiegeschiedenis weer te geven.

1. Klik op de release, waarmee de pijplijn wordt weergegeven. Klik op een omgeving om te controleren of de release **samenvatting, doorvoeringen**, gekoppeld **werkitems**.

1. Selecteer **Doorvoeringen**. In deze weergave worden de codedoorvoeringen weergegeven die zijn gekoppeld aan deze implementatie. Vergelijk versies om de doorvoerverschillen tussen implementaties weer te geven.

1. Selecteer **logboeken bekijken**. De logboeken bevatten nuttige informatie over het implementatieproces. U kunt beide weergeven tijdens en na de implementaties.

## <a name="commit-code-changes-and-execute-cicd"></a>Codewijzigingen doorvoeren en CI/CD uitvoeren

> [!NOTE]
> Met de volgende procedure wordt de CI/CD-pijplijn getest door een eenvoudige tekstwijziging aan te brengen.

U bent nu klaar om samen te werken met een team van uw app met behulp van een CI/CD-proces waarmee automatisch de meest recente werk in uw Azure-functie wordt geïmplementeerd. Bij elke wijziging in de Git-opslagplaats wordt een build gestart in Azure DevOps, en met een CD-pijplijn wordt een implementatie uitgevoerd in Azure. Volg de procedure in deze sectie of gebruik een andere methode om wijzigingen in de opslagplaats door te voeren. U kunt bijvoorbeeld de Git-opslagplaats in uw favoriete hulpprogramma of IDE klonen en wijzigingen vervolgens naar deze opslagplaats pushen.

1. Selecteer in het menu Azure DevOps **opslagplaatsen | Bestanden**, en ga vervolgens naar uw opslagplaats.

1. De opslagplaats bevat al code met de naam **SampleFunctionApp** op basis van de taal van de toepassing die u hebt gekozen tijdens het proces. Open de **Application/SampleFunctionApp/Function1.cs** bestand.

1. Selecteer **bewerken**, en breng een wijziging in **regelnummer 31** . Bijvoorbeeld, kunt u deze bijwerken naar **er Hallo! Welkom bij Azure Functions met behulp van DevOps Projects**

1. Selecteer in de rechterbovenhoek **Doorvoeren** en selecteer vervolgens nogmaals **Doorvoeren** om de wijziging te pushen.

1. Open de **Application/SampleFunctionApp.Test/Function1TestRunner.cs** bestand. 

1. Selecteer **bewerken**, en breng een wijziging in **regelnummer 21**. Bijvoorbeeld, kunt u deze bijwerken naar **er Hallo! Welkom bij Azure Functions met behulp van Azure DevOps Projects**.

     Na een paar seconden wordt er in Azure DevOps een build gestart en wordt er een versie uitgevoerd om de wijzigingen te implementeren. Bewaak de buildstatus via het DevOps Projects-dashboard of in de browser met uw Azure DevOps-organisatie.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de gerelateerde resources die u hebt gemaakt toen u ze niet meer nodig verwijderen. Gebruik de functionaliteit **Verwijderen** op het DevOps Projects-dashboard.

## <a name="next-steps"></a>Volgende stappen

U kunt de build- en release-pijplijn desgewenst wijzigen in overeenstemming met de behoeften van uw team. U kunt dit CI/CD-patroon ook als een sjabloon voor uw andere pijplijnen gebruiken. In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * DevOps Projects gebruiken voor het implementeren van een ASP.NET Core-app naar Azure-functie
> * Azure DevOps en een Azure-abonnement configureren 
> * Bekijk de Azure-functie
> * De CI-pijplijn onderzoeken
> * De CD-pijplijn onderzoeken
> * Wijzigingen doorvoeren in Git en automatisch implementeren naar Azure
> * Resources opschonen


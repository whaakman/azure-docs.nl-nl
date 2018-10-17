---
title: Uw ASP.NET-app implementeren naar Azure Virtual Machines met het Azure DevOps-project | Azure DevOps Services-zelfstudie
description: Met het DevOps-project kunt u eenvoudig aan de slag in Azure. Met het Azure DevOps-project kunt u een ASP.NET-app eenvoudig in slechts enkele stappen implementeren naar Azure Virtual Machines.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b05e2c2c46aa9bfa8c92d3d3c5c83d018c547b9f
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299131"
---
# <a name="tutorial--deploy-your-aspnet-app-to-azure-virtual-machines-with-the-azure-devops-project"></a>Zelfstudie: Uw ASP.NET-app implementeren naar Azure Virtual Machines met het Azure DevOps-project

Het Azure DevOps-project geeft een vereenvoudigde ervaring waar u uw bestaande code en Git-opslagplaats gebruikt of een van de voorbeeldtoepassingen kiest voor het maken van een CI (Continue integratie) en CD-pijplijn (Continue levering) naar Azure.  Het DevOps-project maakt automatisch Azure-resources, zoals een nieuwe Azure-VM, maakt en configureert een release-pijplijn in Azure DevOps die een build-pijplijn voor CI bevat, stelt een release-pijplijn in voor CD en maakt vervolgens een Application Insights-resource voor bewaking.

U gaat het volgende doen:

> [!div class="checklist"]
> * Een Azure DevOps-project maken voor een ASP.NET-app
> * Azure DevOps Services en een Azure-abonnement configureren 
> * De Azure DevOps Services CI-pijplijn onderzoeken
> * De Azure DevOps Services CD-pijplijn onderzoeken
> * Wijzigingen doorvoeren in Azure DevOps Services en automatisch implementeren naar Azure
> * Azure Application Insights-bewaking configureren
> * Resources opschonen

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. U kunt er een gratis krijgen via [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-app"></a>Een Azure DevOps-project maken voor een ASP.NET-app

Het Azure DevOps-project maakt een CI-/CD-pijplijn in Azure.  U kunt een **nieuwe Azure DevOps Services**-organisatie maken of een **bestaande organisatie** gebruiken.  Het Azure DevOps-project maakt ook **Azure-resources**, zoals virtuele machines, in het **Azure-abonnement** van uw keuze.

1. Meld u aan bij de [Microsoft Azure-portal](https://portal.azure.com).

1. Kies het pictogram **+ Nieuw** in de linkernavigatiebalk en zoek naar **DevOps-project**.  Kies **Maken**.

    ![Continue levering starten](_img/azure-devops-project-github/fullbrowser.png)

1. Selecteer **.NET** en kies **Volgende**.

1. Bij **Een toepassingsframework kiezen** selecteert u **ASP.NET**. Kies daarna **Volgende**. 

1. Het toepassingsframework dat u in de vorige stappen hebt gekozen bepaalt welk type implementatiedoel hier beschikbaar is voor de Azure-service.  Selecteer de **virtuele machine** en kies **Volgende**.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Azure DevOps Services en een Azure-abonnement configureren

1. Maak een **nieuwe** Azure DevOps Services-organisatie of kies een **bestaande** organisatie.  Kies een **naam** voor uw Azure DevOps-project.  

1. Selecteer uw **Azure-abonnement**.  U kunt ook de koppeling **Wijzigen** selecteren en meer configuratiegegevens invoeren. U kunt bijvoorbeeld de locatie van de Azure-resources wijzigen.
 
1. Voer een **virtuele-machinenaam**, **gebruikersnaam** en **wachtwoord** voor uw nieuwe Azure-VM-resource in, en kies **Gereed**.

1. Het duurt enkele minuten voor de virtuele machine van Azure klaar is.  Een voorbeeld-ASP.NET-toepassing wordt in een opslagplaats in uw Azure DevOps Services-organisatie ingesteld; een build en release worden uitgevoerd en uw toepassing wordt geïmplementeerd naar de zojuist gemaakte Azure-VM.  

    Wanneer u klaar bent, wordt het **dashboard** voor het Azure DevOps-project geladen in de Azure-portal.  U kunt ook rechtstreeks vanuit **Alle resources** in de **Azure-portal** naar het **dashboard van het Azure DevOps-project** navigeren.  

    Dit dashboard biedt inzicht in uw Azure DevOps Services-**codeopslagplaats**, **Azure-CI/CD-pijplijn** en uw actieve **toepassing in Azure**.    

    ![Dashboardweergave](_img/azure-devops-project-vms/dashboardnopreview.png)

1. Het Azure DevOps-project configureert automatisch een CI-build- en release-trigger die automatisch eventuele codewijzigingen in uw opslagplaats implementeert.  U kunt verder aanvullende opties configureren in Azure DevOps.  Aan de rechterkant van het dashboard selecteert u **Bladeren** om de lopende toepassing weer te geven.
    
## <a name="examine-the-azure-devops-services-ci-pipeline"></a>De Azure DevOps Services CI-pijplijn onderzoeken
 
Het Azure DevOps-project configureert automatisch een volledige Azure-CI/CD-pijplijn in uw Azure DevOps Services-organisatie.  U kunt de pijplijn verkennen en aanpassen.  Volg de onderstaande stappen om vertrouwd te raken met de Azure DevOps Services-build-pijplijn.

1. Selecteer **Pijplijnen bouwen** **boven in** het **Azure DevOps-projectdashboard**.  Met deze koppeling opent u een tabblad in de browser met de Azure DevOps Services-build-pijplijn voor het nieuwe project.

1. Beweeg de muisaanwijzer naar de rechterkant van de build-pijplijn naast het veld **Status**. Selecteer het **weglatingsteken** dat wordt weergegeven.  Met deze actie opent u een menu waar u diverse acties kunt uitvoeren zoals **een nieuwe build in de wachtrij plaatsen**, **een build onderbreken** en **de build-pijplijn bewerken**.

1. Selecteer **Bewerken**.

1. In deze weergave **onderzoekt u de verschillende taken** voor uw build-pijplijn.  De build voert verschillende taken uit, zoals het ophalen van bronnen uit de Git-opslagplaats van Azure DevOps Services, het herstellen van afhankelijkheden en het publiceren van uitvoer die wordt gebruikt voor implementaties.

1. Selecteer bovenaan de build-pijplijn de **naam van de build-pijplijn**.

1. Wijzig de **naam** van de build-pijplijn in een gebruiksvriendelijkere naam.  Selecteer **Opslaan en wachtrij** en selecteer vervolgens **Opslaan**.

1. Selecteer onder de naam van de build-pijplijn de optie **Geschiedenis**.  U ziet een audittrail van recente wijzigingen voor de build.  Azure DevOps Services houdt alle wijzigingen in de build-pijplijn bij en biedt de mogelijkheid om versies te vergelijken.

1. Selecteer **Triggers**.  Het Azure DevOps-project maakt automatisch een CI-trigger en met elke doorvoering naar de opslagplaats wordt een nieuwe build gestart.  Kies desgewenst of u vertakkingen van het CI-proces wilt opnemen of uitsluiten.

1. Selecteer **Retentie**.  Op basis van uw scenario kunt u beleidsregels opgeven om een bepaald aantal builds te behouden of te verwijderen.

## <a name="examine-the-azure-devops-services-cd-pipeline"></a>De Azure DevOps Services CD-pijplijn onderzoeken

Het Azure DevOps-project maakt en configureert automatisch de benodigde stappen om vanuit uw Azure DevOps Services-organisatie te implementeren naar uw Azure-abonnement.  Deze stappen omvatten het configureren van een Azure-serviceverbinding om Azure DevOps Services te verifiëren bij uw Azure-abonnement.  De automatisering maakt ook een Azure DevOps Services-CD-pijplijn en deze levert de CD aan de Azure-VM.  Volg de stappen hieronder om meer te weten te komen over de Azure DevOps Services-CD-pijplijn.

1. Selecteer **Build en release** en kies **Releases**.  Het Azure DevOps-project heeft een Azure DevOps Services-release-pijplijn gemaakt om implementaties naar Azure te beheren.

1. Selecteer links van de browser het **beletselteken** naast uw release-pijplijn en kies **Bewerken**.

1. De release-pijplijn bevat een **pijplijn** die het releaseproces definieert.  Onder **Artefacten** selecteert u **Neerzetten**.  De build-pijplijn die u in de vorige stappen hebt onderzocht, produceert de uitvoer die wordt gebruikt voor het artefact. 

1. Selecteer rechts van het pictogram **Neerzetten** het **pictogram** **Continue implementatietrigger** (zie eruit als een bliksemflits.)  Deze release-pijplijn heeft een ingeschakelde CD-trigger.  Telkens wanneer er een nieuw build-artefact beschikbaar is, start de trigger een implementatie.  U kunt de trigger desgewenst uitschakelen, zodat uw implementaties handmatig moeten worden uitgevoerd. 

1. Selecteer aan de linkerkant van de browser de optie **Taken** en kies uw **omgeving**.  

1. De taken zijn de activiteiten die door het implementatieproces worden uitgevoerd en ze zijn gegroepeerd in **fasen**.  Er zijn twee **fasen** voor deze release-pijplijn.  De eerste fase bevat een **Azure Resource Group Deployment**-taak die de virtuele machine configureert voor implementatie en de nieuwe virtuele machine toevoegt aan een **Azure DevOps-implementatiegroep**.  Met de VM-implementatiegroep in Azure DevOps kunt u logische groepen van **implementatiedoel**machines beheren.

1. In deze tweede fase is een **IIS-web-app-beheer**taak gemaakt om een IIS-website te maken op de virtuele machine.  Een tweede **IIS-web-app-implementatie**taak is gemaakt om de site te implementeren.

1. Selecteer aan de rechterkant van de browser de optie **Releases weergeven**.  In deze weergave wordt een versiesgeschiedenis weergegeven.

1. Selecteer het **weglatingsteken** naast een van uw versies en kies **Openen**.  Er zijn verschillende menu's die u in deze weergave kunt verkennen, zoals een **versieoverzicht**, **gekoppelde werkitems** en **tests**.

1. Selecteer **Doorvoeringen**.  In deze weergave worden de codedoorvoeringen getoond die zijn gekoppeld aan de specifieke implementatie. Vergelijk versies om de doorvoerverschillen tussen implementaties weer te geven.

1. Selecteer **Logboeken**.  De logboeken bevatten nuttige informatie over het implementatieproces.  Ze kunnen worden weergegeven tijdens en na de implementaties.

## <a name="commit-changes-to-azure-devops-services-and-automatically-deploy-to-azure"></a>Wijzigingen doorvoeren in Azure DevOps Services en automatisch implementeren naar Azure 

U bent nu klaar om met een team samen te werken aan uw app met een CI/CD-proces dat automatisch uw meest recente werk aan uw website implementeert.  Bij elke wijziging in de Git-opslagplaats van Azure DevOps Services wordt er een build gestart in Azure DevOps Services en worden uw wijzigingen naar de virtuele machine van Azure geïmplementeerd via een Azure DevOps Services-CD-pijplijn.  Volg de onderstaande stappen of gebruik andere technieken om wijzigingen door te voeren in uw opslagplaats.  De codewijzigingen starten het CI/CD-proces en implementeren uw nieuwe wijzigingen automatisch op de IIS-website op de virtuele machine van Azure.

1. Selecteer **Code** in het menu van Azure DevOps Services en navigeer naar uw opslagplaats.

1. Navigeer naar de map **Views\Home**, selecteer het **weglatingsteken** naast het bestand **Index.cshtml** en kies **Bewerken**.

1. Breng een wijziging aan in het bestand; voeg bijvoorbeeld wat tekst toe in een van de **div-tags**.  Selecteer in de rechterbovenhoek **Doorvoeren**.  Selecteer nogmaals **Doorvoeren** om de wijziging te pushen. 

1. In een paar seconden wordt er in **Azure DevOps Services een build gestart**, waarna er een versie wordt uitgevoerd om de wijzigingen te implementeren.  Bewaak de **buildstatus** met het DevOps-projectdashboard of in de browser met uw Azure DevOps Services-organisatie.

1. Wanneer de versie is voltooid, **vernieuwt u de toepassing** in de browser om te controleren of u uw wijzigingen kunt zien.

## <a name="configure-azure-application-insights-monitoring"></a>Azure Application Insights-bewaking configureren

Met Azure Application Insights kunt u eenvoudig de prestaties en het gebruik van een webtoepassing controleren.  Het Azure DevOps-project heeft automatisch een Application Insights-resource voor uw toepassing geconfigureerd.  U kunt verder diverse waarschuwingen en bewakingsopties configureren, indien nodig.

1. Navigeer naar het dashboard van het **Azure DevOps-project** in de **Azure-portal**.  Kies rechtsonder in het dashboard de koppeling **Application Insights** voor uw app.

1. De blade **Application Insights** wordt geopend in de Azure-portal.  Deze weergave bevat controlegegevens over gebruik, prestaties en beschikbaarheid voor uw app.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Selecteer **Tijdsbereik** en kies **Afgelopen uur**.  Selecteer **Bijwerken** om de resultaten te filteren.  U ziet nu alle activiteiten van de afgelopen 60 minuten.  Selecteer de **x** om het tijdsbereik af te sluiten.

1. U vindt **Waarschuwingen** en verschillende andere nuttige koppelingen aan de bovenkant van het dashboard.  Selecteer **Waarschuwingen** en vervolgens **+ Metrische waarschuwing toevoegen**.

1. Voer een **naam** in voor de waarschuwing.

1. De standaardwaarschuwing is voor een **serverreactietijd langer dan 1 seconde**.  Selecteer de vervolgkeuzelijst **Metrische gegevens** om de verschillende metrische waarschuwingsgegevens te verkennen.  U kunt bijvoorbeeld **Uitvoertijd van aanvraag voor ASP.NET** configureren voor een ASP.NET-app.  U kunt eenvoudig tal van waarschuwingen configureren om de controlemogelijkheden van uw app te verbeteren.

1. Schakel het selectievakje in voor **Eigenaars, bijdragers en lezers op de hoogte brengen via e-mail**.  Desgewenst kunt u aanvullende acties uitvoeren wanneer een waarschuwing wordt geactiveerd, door een logische app van Azure uit te voeren.

1. Kies **OK** om de waarschuwing te maken.  Binnen enkele ogenblikken wordt de waarschuwing als actief weergegeven op het dashboard.  **Verlaat** het gebied Waarschuwingen en ga terug naar de blade **Application Insights**.

1. Selecteer **Beschikbaarheid** en selecteer **+ Test toevoegen**. 

1. Voer een **testnaam** in en kies **Maken**.  Hiermee maakt u eenvoudige ping-test om de beschikbaarheid van uw toepassing te controleren.  Na een paar minuten zijn de resultaten beschikbaar en toont het Application Insights-dashboard een beschikbaarheidsstatus.

## <a name="clean-up-resources"></a>Resources opschonen

 > [!NOTE]
 > Met de onderstaande stappen worden resources permanent verwijderd.  Gebruik deze functionaliteit alleen nadat u de prompts zorgvuldig hebt gelezen.

Als u aan het testen bent, kunt u resources opschonen om te voorkomen dat kosten oplopen.  Wanneer u de virtuele machine van Azure en verwante resources die in deze zelfstudie zijn gemaakt, niet meer nodig hebt, kunt u ze verwijderen met de functionaliteit **Verwijderen** op het dashboard van het Azure DevOps-project.  **Wees voorzichtig**, want met de functionaliteit Verwijderen verwijdert u de gegevens die door het Azure DevOps-project zijn gemaakt in zowel Azure als Azure DevOps. Als ze zijn verwijderd, kunt u ze niet meer terughalen.

1. Navigeer vanuit de **Azure-portal** naar het **Azure DevOps-project**.
2. Selecteer **rechtsboven** in het dashboard de optie **Verwijderen**.  Na het lezen van de prompt selecteert u **Ja** om de resources **definitief te verwijderen**.

## <a name="next-steps"></a>Volgende stappen

U kunt de build- en release-pijplijn desgewenst wijzigen in overeenstemming met de behoeften van uw team. U kunt dit CI/CD-patroon ook als een sjabloon voor uw andere projecten gebruiken.  U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Azure DevOps-project maken voor een ASP.NET-app
> * Azure DevOps Services en een Azure-abonnement configureren 
> * De Azure DevOps Services CI-pijplijn onderzoeken
> * De Azure DevOps Services CD-pijplijn onderzoeken
> * Wijzigingen doorvoeren in Azure DevOps Services en automatisch implementeren naar Azure
> * Azure Application Insights-bewaking configureren
> * Resources opschonen

Raadpleeg deze zelfstudie voor meer informatie over de Azure-CI/CD-pijplijn:

> [!div class="nextstepaction"]
> [CD-proces aanpassen](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

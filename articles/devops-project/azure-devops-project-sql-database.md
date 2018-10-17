---
title: Uw ASP.NET-app en Azure SQL Database implementeren met het Azure DevOps-project | Azure DevOps Services-zelfstudie
description: Met het DevOps-project kunt u eenvoudig aan de slag in Azure. Met het Azure DevOps-project kunt u uw ASP.NET eenvoudig in slechts enkele stappen implementeren met Azure SQL Database.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: d9c7c94e344daee5af87ce40ddf4dcb686696ded
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297330"
---
# <a name="tutorial--deploy-your-aspnet-app-and-azure-sql-database-with-the-azure-devops-project"></a>Zelfstudie: uw ASP.NET-app en Azure SQL Database implementeren met het Azure DevOps-project

Het Azure DevOps-project geeft een vereenvoudigde ervaring waar u uw bestaande code en Git-opslagplaats gebruikt of een van de voorbeeldtoepassingen kiest voor het maken van een CI (Continue integratie) en CD-pijplijn (Continue levering) naar Azure.  Het DevOps-project maakt automatisch Azure-resources, zoals een Azure SQL Database-database, maakt en configureert een release-pijplijn in Azure DevOps die een build-pijplijn voor CI bevat, stelt een release-pijplijn in voor CD en maakt vervolgens een Azure Application Insights-resource voor bewaking.

U gaat het volgende doen:

> [!div class="checklist"]
> * Een Azure DevOps-project maken voor een ASP.NET-app en Azure SQL Database
> * Azure DevOps Services en een Azure-abonnement configureren 
> * De Azure DevOps Services CI-pijplijn onderzoeken
> * De Azure DevOps Services CD-pijplijn onderzoeken
> * Wijzigingen doorvoeren in Azure DevOps Services en automatisch implementeren naar Azure
> * Verbinding maken met SQL Server Database 
> * Resources opschonen

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. U kunt er een gratis krijgen via [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-app-and-azure-sql-database"></a>Een Azure DevOps-project maken voor een ASP.NET-app en Azure SQL Database

Het Azure DevOps-project maakt een CI-/CD-pijplijn in Azure.  U kunt een **nieuwe Azure DevOps Services**-organisatie maken of een **bestaande organisatie** gebruiken.  Het Azure DevOps-project maakt ook **Azure-resources**, zoals een Azure SQL Database, in het **Azure-abonnement** van uw keuze.

1. Meld u aan bij de [Microsoft Azure-portal](https://portal.azure.com).

1. Kies het pictogram **Een resource maken** in de linkernavigatiebalk en zoek naar **DevOps-project**.  Kies **Maken**.

    ![Continue levering starten](_img/azure-devops-project-github/fullbrowser.png)

1. Selecteer **.NET** en kies **Volgende**.

1. Bij **Een toepassingsframework kiezen** selecteert u **ASP.NET**.

1. Selecteer **Een database toevoegen** en kies vervolgens **Volgende**.  

1. Het toepassingsframework dat u in de vorige stappen hebt gekozen bepaalt welk type implementatiedoel hier beschikbaar is voor de Azure-service.  Selecteer **Volgende**.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Azure DevOps Services en een Azure-abonnement configureren

1. Maak een **nieuwe** Azure DevOps Services-organisatie of kies een **bestaande** organisatie.  Kies een **naam** voor uw Azure DevOps-project.  

1. Selecteer uw **Azure-abonnement**.

1. U kunt desgewenst ook de koppeling **Wijzigen** selecteren om aanvullende configuratie-instellingen van Azure te bekijken en om de **gebruikersnaam** in de sectie **Informatie over de aanmeldingsgegevens voor de databaseserver** te identificeren.  **Bewaar** de **gebruikersnaam** voor toekomstige stappen in deze zelfstudie.
 
1. Als u de bovenstaande stap hebt uitgevoerd, sluit u het configuratiegebied in Azure en kiest u **Gereed**.  Selecteer anders alleen **Gereed**.

1. Het duurt enkele minuten voor het proces is voltooid.  Wanneer u klaar bent, wordt het **dashboard** voor het Azure DevOps-project geladen in de Azure-portal.  U kunt ook rechtstreeks vanuit **Alle resources** in de **Azure-portal** naar het **dashboard van het Azure DevOps-project** navigeren.  Aan de rechterkant van het dashboard selecteert u **Bladeren** om de lopende toepassing weer te geven.
    
## <a name="examine-the-azure-devops-services-ci-pipeline"></a>De Azure DevOps Services CI-pijplijn onderzoeken

Het Azure DevOps-project configureert automatisch een volledige Azure-CI/CD-pijplijn in uw Azure DevOps Services-organisatie.  U kunt de pijplijn verkennen en aanpassen.  Volg de onderstaande stappen om vertrouwd te raken met de Azure DevOps Services-build-pijplijn.

1. Navigeer naar het dashboard van het **Azure DevOps-project**.

1. Selecteer **Pijplijnen bouwen** **boven in** het **Azure DevOps-projectdashboard**.  Met deze koppeling opent u een tabblad in de browser met de Azure DevOps Services-build-pijplijn voor het nieuwe project.

1. Beweeg de muisaanwijzer naar de rechterkant van de build-pijplijn naast het veld **Status**. Selecteer het **weglatingsteken** dat wordt weergegeven.  Met deze actie opent u een menu waar u diverse acties kunt uitvoeren zoals **een nieuwe build in de wachtrij plaatsen**, **een build onderbreken** en **de build-pijplijn bewerken**.

1. Selecteer **Bewerken**.

1. In deze weergave **onderzoekt u de verschillende taken** voor uw build-pijplijn.  De build voert verschillende taken uit, zoals het ophalen van bronnen uit de Git-opslagplaats van Azure DevOps Services, het herstellen van afhankelijkheden en het publiceren van uitvoer die wordt gebruikt voor implementaties.

1. Selecteer bovenaan de build-pijplijn de **naam van de build-pijplijn**.

1. Wijzig de **naam** van de build-pijplijn in een gebruiksvriendelijkere naam.  Selecteer **Opslaan en wachtrij** en selecteer vervolgens **Opslaan**.

1. Selecteer onder de naam van de build-pijplijn de optie **Geschiedenis**.  U ziet een audittrail van recente wijzigingen voor de build.  Azure DevOps Services houdt alle wijzigingen in de build-pijplijn bij en biedt de mogelijkheid om versies te vergelijken.

1. Selecteer **Triggers**.  Het Azure DevOps-project maakt automatisch een CI-trigger en met elke doorvoering naar de opslagplaats wordt een nieuwe build gestart.  U kunt desgewenst kiezen of u vertakkingen van het CI-proces wilt opnemen of uitsluiten.

1. Selecteer **Retentie**.  Op basis van uw scenario kunt u beleidsregels opgeven om een bepaald aantal builds te behouden of te verwijderen.

## <a name="examine-the-azure-devops-services-cd-pipeline"></a>De Azure DevOps Services CD-pijplijn onderzoeken

Het Azure DevOps-project maakt en configureert automatisch de benodigde stappen om vanuit uw Azure DevOps Services-organisatie te implementeren naar uw Azure-abonnement.  Deze stappen omvatten het configureren van een Azure-serviceverbinding om Azure DevOps Services te verifiëren bij uw Azure-abonnement.  De automatisering maakt ook een Azure DevOps Services-releasedefinitie en deze release verstrekt de CD naar de virtuele machine van Azure.  Volg de onderstaande stappen voor meer informatie over de Azure DevOps Services-releasedefinitie.

1. Selecteer **Build en release** en kies **Releases**.  Het Azure DevOps-project heeft een Azure DevOps Services-release-pijplijn gemaakt om implementaties naar Azure te beheren.

1. Selecteer links van de browser het **beletselteken** naast uw release-pijplijn en kies **Bewerken**.

1. De release-pijplijn bevat een **pijplijn** die het releaseproces definieert.  Onder **Artefacten** selecteert u **Neerzetten**.  De build-pijplijn die u in de vorige stappen hebt onderzocht, produceert de uitvoer die wordt gebruikt voor het artefact. 

1. Selecteer rechts van het pictogram **Neerzetten** het **pictogram** **Continue implementatietrigger** (zie eruit als een bliksemflits.)  Deze release-pijplijn heeft een ingeschakelde CD-trigger.  Telkens wanneer er een nieuw build-artefact beschikbaar is, start de trigger een implementatie.  U kunt de trigger desgewenst uitschakelen, zodat uw implementaties handmatig moeten worden uitgevoerd. 

1. Het Azure DevOps-project heeft een willekeurig SQL-wachtwoord ingesteld en dit wachtwoord gebruikt voor de releasedefinitie.  Selecteer aan de linkerkant van de browser de optie **Variabelen**. 

1. **Voer deze stap alleen uit als u het SQL Server-wachtwoord hebt gewijzigd.**  Er is één **Wachtwoord**variabele.  Aan de rechterkant van het tekstvak **Waarde** selecteert u het **hangslot**pictogram.  **Voer** het nieuwe wachtwoord in en selecteer vervolgens **Opslaan**.

1. Selecteer aan de linkerkant van de browser de optie **Taken** en kies uw **omgeving**.  

1. De taken zijn de activiteiten die door het implementatieproces worden uitgevoerd en zijn gegroepeerd in **Fasen**.  Er is een enkele fase voor deze release-pijplijn.  De fase bevat een taak voor een **Azure App Service-implementatie** en een voor een **Azure SQL Database-implementatie**.

1. Selecteer de taak **Azure SQL uitvoeren** en controleer de verschillende eigenschappen die gebruikt worden voor de SQL-implementatie.  Onder **Implementatiepakket** ziet u dat de taak gebruikmaakt van een **SQL DACPAC-bestand**.

1. Selecteer aan de rechterkant van de browser de optie **Releases weergeven**.  In deze weergave wordt een versiesgeschiedenis weergegeven.

1. Selecteer het **weglatingsteken** naast een van uw versies en kies **Openen**.  Er zijn verschillende menu's die u in deze weergave kunt verkennen, zoals een **versieoverzicht**, **gekoppelde werkitems** en **tests**.

1. Selecteer **Doorvoeringen**.  In deze weergave worden de codedoorvoeringen getoond die zijn gekoppeld aan de specifieke implementatie. U kunt versies vergelijken om de doorvoerverschillen tussen implementaties weer te geven.

1. Selecteer **Logboeken**.  De logboeken bevatten nuttige informatie over het implementatieproces.  Ze kunnen worden weergegeven tijdens en na de implementaties.

## <a name="commit-changes-to-azure-devops-services-and-automatically-deploy-to-azure"></a>Wijzigingen doorvoeren in Azure DevOps Services en automatisch implementeren naar Azure 

 > [!NOTE]
 > In de volgende stappen test u de CI/CD-pijplijn met een eenvoudige tekstwijziging.  U kunt ook een SQL Server Schema-wijziging aanbrengen in het tabel om het SQL-implementatieproces te testen.

U bent nu klaar om met een team samen te werken aan uw app met een CI/CD-proces dat automatisch uw meest recente werk aan uw website implementeert.  Bij elke wijziging in de Git-opslagplaats van Azure DevOps Services wordt er een build gestart in Azure DevOps Services en worden uw wijzigingen naar Azure geïmplementeerd via een Azure DevOps Services-CD-pijplijn.  Volg de onderstaande stappen of gebruik andere technieken om wijzigingen door te voeren in uw opslagplaats.  De codewijzigingen beginnen het CI/CD-proces en implementeren uw nieuwe wijzigingen automatisch in Azure.

1. Selecteer **Code** in het menu van Azure DevOps Services en navigeer naar uw opslagplaats.

1. Navigeer naar de map **SampleWebApplication\Views\Home**, selecteer het **weglatingsteken** naast het bestand **Index.cshtml** en kies **Bewerken**.

1. Breng een wijziging aan in het bestand; voeg bijvoorbeeld wat tekst toe in een van de **div-tags**.  Selecteer in de rechterbovenhoek **Doorvoeren**.  Selecteer nogmaals **Doorvoeren** om de wijziging te pushen. 

1. In een paar seconden wordt er in **Azure DevOps Services een build gestart**, waarna er een versie wordt uitgevoerd om de wijzigingen te implementeren.  U kunt de **buildstatus** bewaken met het DevOps-projectdashboard of in de browser met uw Azure DevOps Services-organisatie.

1. Wanneer de versie is voltooid, **vernieuwt u de toepassing** in de browser om te controleren of u uw wijzigingen kunt zien.

## <a name="connect-to-the-azure-sql-server-database"></a>Verbinding maken met SQL Server Database

U hebt de juiste machtigingen nodig om verbinding te kunnen maken met de Azure SQL Database.

1. Selecteer de **SQL Database** in het Azure DevOps-projectdashboard om naar de beheerpagina voor de database te navigeren.
   
1. Selecteer **Serverfirewall instellen** en vervolgens **+ IP-adres van client toevoegen**.  

1. Selecteer **Opslaan**.  Uw client-IP-adres is nu gemachtigd voor toegang tot de **SQL Server Azure-resource**.

1. Ga terug naar de blade **SQL Database**. 

1. Aan de rechterkant van het scherm selecteert u de **servernaam** om naar de configuratiepagina voor de **SQL Server** te navigeren.

1. Selecteer **Wachtwoord opnieuw instellen**, geef een wachtwoord op voor de **SQL Server-adminreferenties** en selecteer vervolgens **Opslaan**.  **Bewaar** dit wachtwoord voor toekomstige stappen in deze zelfstudie.

1. U kunt nu desgewenst gebruikmaken van clienthulpprogramma's zoals **SQL Server Management Studio** of **Visual Studio** om verbinding te maken met de Azure SQL Server en Database.  Gebruik de eigenschap **Servernaam** om verbinding te maken.

   Als u de DB-gebruikersnaam niet hebt gewijzigd bij de eerste configuratie van het DevOps-project, is uw gebruikersnaam het lokale gedeelte van uw e-mailadres.  Als uw e-mailadres bijvoorbeeld johndoe@microsoft.com is, is de gebruikersnaam johndoe.

 > [!NOTE]
 > Als u uw wachtwoord voor de SQL-aanmelding wijzigt, moet u het wachtwoord in de variabele van de Azure DevOps Services-releasedefinitie wijzigen zoals staat beschreven in de sectie **De Azure DevOps Services-CD-pijplijn onderzoeken**

## <a name="clean-up-resources"></a>Resources opschonen

 > [!NOTE]
 > Met de onderstaande stappen worden resources permanent verwijderd.  Gebruik deze functionaliteit alleen nadat u de prompts zorgvuldig hebt gelezen.

Als u aan het testen bent, kunt u resources opschonen om te voorkomen dat kosten oplopen.  Wanneer u de Azure SQL Database en verwante resources die in deze zelfstudie zijn gemaakt, niet meer nodig hebt, kunt u ze verwijderen met de functionaliteit **Verwijderen** in het Azure DevOps-projectdashboard.  **Wees voorzichtig**, want met de functionaliteit Verwijderen verwijdert u de gegevens die door het Azure DevOps-project zijn gemaakt in zowel Azure als Azure DevOps Services. Als ze zijn verwijderd, kunt u ze niet meer terughalen.

1. Navigeer vanuit de **Azure-portal** naar het **Azure DevOps-project**.
2. Selecteer **rechtsboven** in het dashboard de optie **Verwijderen**.  Na het lezen van de prompt selecteert u **Ja** om de resources **definitief te verwijderen**.

## <a name="next-steps"></a>Volgende stappen

U kunt de build- en release-pijplijn desgewenst wijzigen in overeenstemming met de behoeften van uw team. U kunt dit CI/CD-patroon ook als een sjabloon voor uw andere projecten gebruiken.  U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Azure DevOps-project maken voor een ASP.NET-app en Azure SQL Database
> * Azure DevOps Services en een Azure-abonnement configureren 
> * De Azure DevOps Services CI-pijplijn onderzoeken
> * De Azure DevOps Services CD-pijplijn onderzoeken
> * Wijzigingen doorvoeren in Azure DevOps Services en automatisch implementeren naar Azure
> * Verbinding maken met SQL Server Database 
> * Resources opschonen

Raadpleeg deze zelfstudie voor meer informatie over de Azure-pijplijn:

> [!div class="nextstepaction"]
> [CD-proces aanpassen](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Video's

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3308/player]
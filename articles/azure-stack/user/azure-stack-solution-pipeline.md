---
title: Uw app implementeren in Azure en Azure Stack | Microsoft Docs
description: Informatie over het implementeren van apps op Azure en Azure Stack met een hybride CI/CD-pipeline.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 49a80805c976e5584bb158965583a03eda68cc46
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-deploy-apps-to-azure-and-azure-stack"></a>Zelfstudie: Apps implementeren op Azure en Azure Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Een hybride-pijplijn voor continue integratie/continue levering (CI/CD) kunt u bouwen, testen en een app implementeren in meerdere clouds.  In deze zelfstudie maakt u een Voorbeeldomgeving:
 
> [!div class="checklist"]
> * Start een nieuwe build op basis van code doorvoeracties naar uw opslagplaats Visual Studio Team Services (VSTS).
> * Automatisch implementeren uw nieuw gebouwde code voor globale Azure voor gebruikersacceptatietests.
> * Wanneer uw code is verstreken testen, automatisch geïmplementeerd op Azure-Stack.

### <a name="about-the-hybrid-delivery-build-pipe"></a>Over de hybride levering bouwen pipe

Continuïteit van de implementatie van toepassing, beveiliging en betrouwbaarheid is essentieel voor uw organisatie en essentieel is voor uw ontwikkelteam. Met een hybride CI/CD-pijplijn, kunt u uw pijplijnen consolideren in uw on-premises omgeving en de openbare cloud. U kunt de locatie wijzigen zonder het overschakelen van uw toepassing.

Deze aanpak kunt u een consistente set ontwikkelingsprogramma's onderhouden. Consistente hulpprogramma's in de openbare Azure-cloud en uw on-premises omgeving voor Azure-Stack betekent dat het is veel eenvoudiger voor u een CI/CD dev practice implementeren. Apps en services die zijn geïmplementeerd in Azure of Azure-Stack zijn uitwisselbaar en de dezelfde code kunt uitvoeren in beide locaties profiteren van de on-premises en openbare cloud-functies en mogelijkheden.

Meer informatie over:
 - [Wat is continue integratie?](https://www.visualstudio.com/learn/what-is-continuous-integration/)
 - [Wat is de levering van continue?](https://www.visualstudio.com/learn/what-is-continuous-delivery/)


## <a name="prerequisites"></a>Vereisten

U moet beschikken over een aantal onderdelen voor het bouwen van een hybride CI/CD-pijplijn. Ze kunnen even duren om voor te bereiden.
 
 - Een partner Azure OEM/hardware een productie-Azure-Stack kan implementeren en alle gebruikers kunnen een Azure Stack Development Kit (ASDK) implementeren. 
 - Een Azure-Stack-Operator moet ook de App Service implementeren, plannen en aanbiedingen maken, een tenantabonnement maken en de Windows Server 2016-installatiekopie toe te voegen.

Als u al beschikt over sommige van deze onderdelen, zorg er dan voor dat ze voldoen aan de vereisten voor het begin.

In dit onderwerp wordt ervan uitgegaan dat u enige kennis van Azure en Azure-Stack hebben. Als u weten voordat u doorgaat wilt, moet beginnen met de volgende onderwerpen:


Deze zelfstudie wordt ervan uitgegaan dat u enige kennis van Azure en Azure-Stack hebben. 

Als u weten voordat u doorgaat wilt, kunt u starten met de volgende onderwerpen:
 - [Inleiding tot Azure](https://azure.microsoft.com/overview/what-is-azure/)
 - [Belangrijkste concepten van Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure

 - Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

 - Maak een [Web-App](https://docs.microsoft.com/azure/app-service/app-service-web-overview) in Azure. Noteer de nieuwe Web-App-URL, zoals het wordt later gebruikt.

Azure Stack
 - Een Azure-Stack geïntegreerd systeem gebruiken of implementeren van Azure Stack Development Kit (ASDK) gekoppeld hieronder:
    - U vindt gedetailleerde instructies over het implementeren van de ASDK op '[zelfstudie: implementeer de ASDK met behulp van het installatieprogramma](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy)'
    - U kunt een aantal stappen na de implementatie van de ASDK met het volgende PowerShell-script automatiseren [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ).

    > [!note]  
    > De installatie van de ASDK duurt een zeven uur moet zijn voltooid, dus dienovereenkomstig plan.

 - Implementeer [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS-services naar Azure-Stack. 
 - Maak [Plan/aanbiedingen](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) binnen de Azure-Stack-omgeving. 
 - Maak [tenant abonnement](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) binnen de Azure-Stack-omgeving. 
 - Een Web-App in de tenantabonnement maken. Noteer de URL van de nieuwe App voor later gebruik.
 - Implementeer VSTS virtuele Machine, nog steeds binnen het tenantabonnement.
 - Windows Server 2016-VM met .NET 3.5 is vereist. Deze virtuele machine wordt als de persoonlijke build-agent op uw Azure-Stack gebouwd. 

### <a name="developer-tools"></a>Hulpprogramma's voor ontwikkelaars

 - Maak een [VSTS werkruimte](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services). Het aanmeldingsproces maakt een project met de naam **MyFirstProject**.
 - [Installeer Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) en [aanmelden bij VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).
 - Verbinding maken met het project en [klonen lokaal](https://www.visualstudio.com/docs/git/gitquickstart).
 
 > [!note]  
 > Azure-Stack moet u met de juiste installatiekopieën extern gepubliceerd als u wilt uitvoeren (Windows Server en SQL) en App-Service geïmplementeerd.
 
## <a name="prepare-the-private-build-and-release-agent-for-visual-studio-team-services-integration"></a>Voorbereiden van de persoonlijke build en de release-agent voor Visual Studio Team Services-integratie

### <a name="prerequisites"></a>Vereisten

Visual Studio Team Services (VSTS) wordt geverifieerd op basis van Azure Resource Manager met behulp van een Service-Principal. Hiervoor moeten Inzenderstatus voor VSTS kunnen inrichten resources in een Azure-Stack-abonnement.

Hier volgen de hoofdstappen die moeten worden geconfigureerd als deze verificatie wilt inschakelen:

1. Service-Principal moet worden gemaakt of een bestaande kan worden gebruikt.
2. Verificatiesleutels moeten worden gemaakt voor de Service-Principal.
3. Azure-Stack-abonnement moet worden gevalideerd via toegangsbeheer op basis van rollen om toe te staan de SPN deel uitmaken van de rol van de Inzender.
4. Een nieuwe Service-definitie in VSTS moet worden gemaakt met de Azure-Stack eindpunten, evenals de SPN-informatie.

### <a name="service-principal-creation"></a>Maken van de service-principal

Raadpleeg de [Service-Principal maken](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) instructies voor het maken van een service-principal en Web-App/API voor het Type toepassing kiezen.

### <a name="access-key-creation"></a>Toegang maken

Een Service-Principal vereist een sleutel voor verificatie, volg de stappen in deze sectie om een sleutel te genereren.


1. Selecteer uw toepassing bij **App-registraties** in Azure Active Directory.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_01.png)

2.  Noteer de waarde van **toepassings-ID**. U gebruikt deze waarde bij het configureren van het service-eindpunt in VSTS.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_02.png)

3. Selecteer **Instellingen** om een verificatiesleutel te genereren.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_03.png)

4. Selecteer **Sleutels** om een verificatiesleutel te genereren.
 
    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_04.png)

5. Geef een beschrijving van de sleutel en geef de duur van de sleutel op. Selecteer **Opslaan** wanneer u klaar bent.
 
    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_05.png)

    Na het opslaan van de sleutel wordt de waarde van de sleutel weergegeven. Kopieer deze waarde, want u kunt de sleutel later niet meer ophalen. U bieden de **sleutelwaarde** met de toepassings-ID aan te melden als de toepassing. Sla de sleutelwaarde op waar uw toepassing deze kan ophalen.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_06.png)

### <a name="get-tenant-id"></a>Tenant-id ophalen

Als onderdeel van de configuratie van de service-eindpunt VSTS vereist de **Tenant-ID** die overeenkomt met de AAD-Directory dat uw Azure-Stack stempel op is geïmplementeerd. Volg de stappen in deze sectie voor het verzamelen van de Tenant-id.

1. Selecteer **Azure Active Directory**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_07.png)

2. Haal de tenant-id op door **Eigenschappen** voor uw Azure AD-tenant te selecteren.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_08.png)
 
3. Kopieer de **Directory-id**. Deze waarde is uw tenant-id.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>De service-principal rechten voor het implementeren van resources in de Stack van Azure-abonnement toewijzen 

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol vertegenwoordigt de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van het bereik. Bijvoorbeeld, een toepassing met de rol Lezer voor een resourcegroep toe te voegen dat kan de resourcegroep en alle resources die deze bevat gelezen.

1. Ga naar het niveau van het bereik dat u wilt toewijzen van de toepassing. Selecteer bijvoorbeeld als een rol bij het abonnementsbereik wilt toewijzen, **abonnementen**. U kunt in plaats daarvan een resourcegroep of resource selecteren.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_10.png)

2. Selecteer de **abonnement** (resourcegroep of resource) de toepassing toewijzen aan.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_11.png)

3. Selecteer **toegangsbeheer (IAM)**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_12.png)

4. Selecteer **Toevoegen**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_13.png)

5. Selecteer de rol die u wilt toewijzen aan de toepassing. De volgende afbeelding toont de **eigenaar** rol.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_14.png)

6. Azure Active Directory-toepassingen worden niet standaard weergegeven in de beschikbare opties. Als u wilt zoeken in uw toepassing, moet u **geeft u de naam** in het zoekveld. Selecteer deze.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_16.png)

7. Selecteer **opslaan** voltooid de rol toe te wijzen. Ziet u uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

### <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Azure op rollen gebaseerde toegangsbeheer (RBAC) kunt Geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u alleen de toegangsrechten aan gebruikers verlenen die ze nodig hebben om hun taken uit te voeren. Zie voor meer informatie over toegangsbeheer op basis van rollen [toegang beheren tot Azure-Abonnementresources](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

### <a name="vsts-agent-pools"></a>VSTS Agent groepen

Elke agent plaats afzonderlijk te beheren, kunt u agents indelen in groepen van de agent. Een agent-groep definieert de delen grens voor alle agents in die groep. In VSTS, zijn agent pools gericht op het account VSTS; Zo kunt u een agent toepassingen delen in teamprojecten. Zie voor meer informatie en een zelfstudie over het maken van de agent VSTS pools [Agent groepen maken en wachtrijen](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts).

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>Toevoegen van een persoonlijk toegangstoken (PAT) voor Azure-Stack

1. Aanmelden bij uw account VSTS en selecteer de naam van uw profiel.
2. Selecteer **beveiliging beheren** naar de pagina voor toegang tot het token maken.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_17.png)

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_18.png)

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_18a.png)

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_18b.png)

3. Kopieer het token.
    
    > [!note]  
    > De tokeninformatie verkrijgen. Dit wordt niet opnieuw na het verlaten van dit scherm worden weergegeven. 
    
    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_19.png)
    

### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Installeer die de VSTS agent op de Stack Azure bouwen gehoste Server bouwen

1.  Verbinding maken met uw Build-Server die u hebt geïmplementeerd op de Stack van Azure-host.

2.  Downloaden en implementeren van de build-agent als een service met behulp van uw persoonlijke toegangstoken (PAT) en wordt uitgevoerd als de VM-beheeraccount.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\010_downloadagent.png)

3. Ga naar de map van de uitgepakte build-agent. Voer de **run.cmd** bestand vanaf een opdrachtprompt met verhoogde bevoegdheid. 

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_20.png)

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\000_21.png)

4.  Nadat de run.cmd voltooid wordt de map met de geëxtraheerde inhoud moet eruitzien als in het volgende:

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\009_token_file.png)

    U ziet nu de agent in VSTS map.

## <a name="endpoint-creation-permissions"></a>Machtigingen voor het eindpunt maken

Gebruikers kunnen eindpunten maken zodat VSTO builds Azure Service-apps naar de stack implementeren kunnen. VSTS verbindt met de build-agent, die vervolgens verbinding met de Azure-Stack maakt. 

![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\012_securityendpoints.png)

1. Op de **instellingen** selecteert u **beveiliging**.
2. In de **VSTS groepen** lijst aan de linkerkant, selecteer **eindpunt auteurs**. 

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\013_endpoint_creators.png)

3. Op de **leden** tabblad **+ toevoegen**. 

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\014_members_tab.png)

4. Typ een gebruikersnaam en selecteert u die gebruiker in de lijst.
5. Klik op **wijzigingen opslaan**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\015_save_endpoint.png)

6. In de **VSTS groepen** lijst aan de linkerkant, selecteer **eindpunt beheerders**.
7. Op de **leden** tabblad **+ toevoegen**.
8. Typ een gebruikersnaam en selecteert u die gebruiker in de lijst.
9. Klik op **wijzigingen opslaan**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\016_save_changes.png)

    De build-agent in Azure-Stack krijgt instructies uit VSTS die daarna informatie over endpoint voor communicatie met de Azure-Stack wordt. 
    
    VSTS naar Azure Stack-verbinding is nu klaar.

## <a name="develop-your-application"></a>Uw toepassing ontwikkelen

Hybride CI/CD Web-App implementeren naar Azure en Azure-Stack en automatische push-wijzigingen in beide clouds instellen.

> [!note]  
> Azure-Stack moet u met de juiste installatiekopieën extern gepubliceerd als u wilt uitvoeren (Windows Server en SQL) en App-Service geïmplementeerd. Controleer de App Service-documentatie sectie 'Vereisten' voor Azure Stack Operator vereisten.

### <a name="add-code-to-vsts-project"></a>Voeg code toe aan VSTS project

1. Meld u aan Visual Studio met een account met rechten voor het project maken op Azure-Stack.

    Hybride CI/CD kunt toepassen op zowel de toepassingscode en de code van de infrastructuur. Gebruik [Azure Resource Manager-sjablonen, zoals web ](https://azure.microsoft.com/resources/templates/) app-code van VSTS tot beide clouds.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\017_connect_to_project.png)

2. **De opslagplaats klonen** door het maken en openen van de standaard web-app.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Maken van zelfstandige web app-implementatie voor App-Services in beide clouds

1. Bewerken de **WebApplication.csproj** bestand: Selecteer **Runtimeidentifier** en toe te voegen `win10-x64.` voor meer informatie Zie [zelfstandige implementatie](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) documentatie .

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\019_runtimeidentifer.png)

2. Controleer de code in VSTS Team Verkenner.

3. Bevestig dat de toepassingscode in Visual Studio Team Services is gecontroleerd. 

### <a name="create-the-build-definition"></a>De definitie van de build maken

1. Aanmelden bij VSTS om te bevestigen dat build-definities maken.

2. Voeg **win10 - r-x64** code. Dit is nodig voor het activeren van een zelfstandige implementatie met .net Core. 

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\020_publish_additions.png)

3. De build worden uitgevoerd. De [zelfstandige implementatie build](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) proces artefacten die kunnen worden uitgevoerd op Azure en Azure Stack zal publiceren.

### <a name="using-an-azure-hosted-agent"></a>Met behulp van een Azure gehoste Agent

Met behulp van een gehoste-agent in VSTS is een handige manier om te bouwen en implementeren van web-apps. Onderhoud en upgrades worden automatisch uitgevoerd door Microsoft Azure, waardoor continu, ononderbroken ontwikkelen, testen en implementatie.

### <a name="manage-and-configure-the-continuous-deployment-cd-process"></a>Beheren en configureren van het proces van continue implementatie (CD)

Visual Studio Team Services (VSTS) en Team Foundation Server (TFS) bieden een pijplijn maximaal configureerbare en beheerbare voor omgevingen met meerdere zoals ontwikkeling, releases fasering, QA en productieomgevingen; inclusief goedkeuringen vereisen specifieke stadia.

### <a name="create-release-definition"></a>Release-definitie maken

![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\021a_releasedef.png)

1. Selecteer de  **\[ +]** toevoegen van een nieuwe Release onder de **Releases tabblad** op de pagina Build en Release van VSO.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\102.png)

2. Van toepassing de **Azure App Service-implementatie** sjabloon.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\103.png)

3. Onder toevoegen artefact vervolgkeuzelijst, **toevoegen van het artefact** voor de Azure-Cloud-build-app.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\104.png)

4. Pipeline-tabblad, selecteer de **fase**, **taak** koppelen van de omgeving en de Azure-cloud Omgevingswaarden instellen.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\105.png)

5. Stel de **omgevingsnaam** en selecteer Azure **abonnement** voor het eindpunt van de Azure-Cloud.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\106.png)

6. Onder de naam van de omgeving, stelt u de vereiste **Azure app service-naam**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\107.png)

7. Voer **VS2017 gehost** onder Agent wachtrij voor Azure in de cloud gehoste omgeving.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\108.png)

8. Selecteer het geldige in Azure App Service implementeren menu **pakket of de map** voor de omgeving. Selecteer **OK** naar **maplocatie**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\109.png)

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\110.png)

9. Alle wijzigingen opslaan en terug te keren naar **release pijplijn**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\111.png)

10. Voeg een **nieuwe artefact** selecteren van de build voor de Azure-Stack-app.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\112.png)

11. Voeg één meer omgeving toepassen van de **Azure App Service-implementatie**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\113.png)

12. Naam van de nieuwe omgeving **Azure Stack**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\114.png)

13. Zoeken naar de Stack van Azure-omgeving onder **taak** tabblad.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\115.png)

14. Selecteer de **abonnement** voor het eindpunt van de Azure-Stack.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\116.png)

15. Instellen van de Azure-Stack web-app-naam als de **App service-naam**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\117.png)

16. Selecteer de **Azure Stack agent**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\118.png)

17. Selecteer sectie onder het implementeren van Azure App Service het geldige **pakket of de map** voor de omgeving. Selecteer OK om door te **maplocatie**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\119.png)

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\120.png)

18. Voeg onder variabele tabblad een variabele met de naam **VSTS_ARM_REST_IGNORE_SSL_ERRORS**, stel de waarde als **waar**, en bereik voor **Azure Stack**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\121.png)

19. Selecteer de **doorlopend** implementatie pictogram in beide artefacten activeren en de implementatie gaat door met het trigger inschakelen.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\122.png)

20. Selecteer de **vóór implementatie** voorwaarden pictogram in de azure omgeving stapelen en stelt u de trigger op **na release**.

21. Alle wijzigingen worden opgeslagen.

> [!note]  
> Sommige instellingen voor de taken kunnen automatisch gedefinieerd als [omgevingsvariabelen](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) tijdens het maken van een release-definitie van een sjabloon. Deze instellingen kunnen niet worden gewijzigd in de taakinstellingen; in plaats daarvan moet u het item bovenliggende omgeving om deze instellingen te bewerken.

## <a name="create-a-release"></a>Een releaserecord maken

Nu dat u de wijzigingen in de definitie van de release hebt voltooid, is het tijd om de implementatie start. Om dit te doen, moet u een versie van de release-definitie maken. Een releaserecord kan worden automatisch gemaakt. bijvoorbeeld is de trigger doorlopende implementatie ingesteld in de release-definitie. Dit betekent dat het wijzigen van de broncode van een nieuwe build Start en die een nieuwe release. Echter, in deze sectie maakt u een nieuwe release handmatig.

1. Open de **Release** vervolgkeuzelijst lijst en kies **release gemaakt**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\200.png)
 
2. Voer een beschrijving voor de release, controleert u de juiste artefacten zijn geselecteerd en kies vervolgens **maken**. Na enkele ogenblikken een banner wordt weergegeven dat aangeeft dat de nieuwe versie is gemaakt. Kies de koppeling (de naam van de release).

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\201.png)
 
3. De release-overzichtspagina wordt geopend met details van de release. In de **omgevingen** gedeelte, ziet u de implementatiestatus voor de 'QA'-omgeving gewijzigd van 'IN bewerking' naar 'Geslaagd', en op dat moment een banner wordt weergegeven dat aangeeft dat de versie nu op goedkeuring wachten is. Wanneer een implementatie met een omgeving in behandeling is of is mislukt, een blauwe (i) wordt het informatiepictogram dat weergegeven. Wijs aan dit om te zien van een pop-upvenster met de reden.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\202.png)

Andere weergaven, zoals de lijst met releases, ook weer een pictogram dat goedkeuring aangeeft is in behandeling. Het pictogram ziet u een pop-upvenster met de naam van de omgeving en meer details wanneer u aanwijst. Dit maakt het eenvoudig voor een beheerder om te zien welke versies wachten op goedkeuring, evenals de algemene voortgang van alle versies.

### <a name="monitor-and-track-deployments"></a>Bewaken en bijhouden implementaties

In deze sectie ziet u hoe u kunt bewaken en bijhouden van implementaties - in dit voorbeeld twee Azure App Services websites - van de release die u in de vorige sectie hebt gemaakt.

1. Kies in de release overzichtspagina de **logboeken** koppeling. Tijdens de implementatie, ziet deze pagina het live logboek van de agent en, in het linkerdeelvenster, een indicatie van de status van elke bewerking in het implementatieproces voor elke omgeving.

    Kies het pictogram in de **actie** kolom voor een goedkeuring vóór de implementatie of na de implementatie van details van die goedgekeurd (of afgewezen) de implementatie en het bericht die gebruiker is opgegeven.

2. Nadat de implementatie voltooid is, wordt het hele logboekbestand in het rechterdeelvenster weergegeven. Selecteer een van de **processtappen** in het linkerdeelvenster om weer te geven alleen het logboek bestandsinhoud voor deze stap. Dit maakt het gemakkelijker te traceren en fouten opsporen in afzonderlijke onderdelen van de algehele implementatie. U kunt ook downloaden de afzonderlijke logboekbestanden of een zip van alle logboekbestanden van de pictogrammen en koppelingen in de pagina.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\203.png)
 
3. Open de **samenvatting** tabblad voor een overzicht van de algehele details van de release. Deze worden details weergegeven van de build en de omgeving die is geïmplementeerd om - samen met de implementatiestatus en andere informatie over de release.

4. Selecteer de **omgeving koppelingen** voor meer informatie over bestaande en in afwachting van implementaties voor die specifieke omgeving. U kunt deze pagina's gebruiken om te controleren dat dezelfde build voor beide omgevingen is geïmplementeerd.

5. Open de **productie-app geïmplementeerd** in uw bladeren. Bijvoorbeeld: voor een Azure App Services-website van de URL `http://[your-app-name].azurewebsites.net`.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure-Cloud-patronen, [ontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).

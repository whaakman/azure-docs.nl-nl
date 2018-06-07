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
ms.date: 05/15/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 41e6f64ada7c95674cc2573048eef8afc83e4385
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604349"
---
# <a name="tutorial-deploy-apps-to-azure-and-azure-stack"></a>Zelfstudie: apps implementeren op Azure en Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Informatie over het implementeren van een toepassing op Azure en Azure-Stack met behulp van een hybride-pijplijn voor continue integratie/continue levering (CI/CD).

In deze zelfstudie maakt u een Voorbeeldomgeving:

> [!div class="checklist"]
> * Start een nieuwe build op basis van code doorvoeracties naar uw opslagplaats Visual Studio Team Services (VSTS).
> * Uw app automatisch implementeren op globale Azure voor gebruikersacceptatietests.
> * Wanneer uw code testen verstrijkt, moet u automatisch de app implementeren op Azure-Stack.

## <a name="benefits-of-the-hybrid-delivery-build-pipe"></a>Voordelen van de hybride levering bouwen pipe

Bedrijfscontinuïteit, beveiliging en betrouwbaarheid zijn de belangrijkste elementen van de toepassingsimplementatie. Deze elementen zijn essentieel voor uw organisatie en essentieel is voor uw ontwikkelteam. Een hybride CI/CD-pijplijn, kunt u uw build-pipes consolideren in uw on-premises omgeving en de openbare cloud. Een hybride levering modellen kunt u ook implementatie locaties wijzigen zonder uw toepassing worden gewijzigd.

Andere voordelen voor het gebruik van de hybride-methode zijn:

* U kunt een consistente set ontwikkelingsprogramma's onderhouden tussen uw on-premises Azure Stack-omgeving en de openbare Azure-cloud.  Een gemeenschappelijke hulpprogrammaset kunt gemakkelijker CI/CD patterns and practice implementeren.
* Apps en services die zijn geïmplementeerd in Azure of Azure-Stack zijn uitwisselbaar en de dezelfde code kan worden uitgevoerd op een locatie. U kunt profiteren van de on-premises en openbare cloud-functies en mogelijkheden.

Voor meer informatie over CI en CD:

* [Wat is continue integratie?](https://www.visualstudio.com/learn/what-is-continuous-integration/)
* [Wat is de levering van continue?](https://www.visualstudio.com/learn/what-is-continuous-delivery/)

## <a name="prerequisites"></a>Vereisten

U moet beschikken over onderdelen voor het bouwen van een hybride CI/CD-pijplijn. De volgende onderdelen kunnen even duren om voor te bereiden:

* Een Azure OEM/hardware-partner kunt implementeren een productie-Azure-Stack. Alle gebruikers kunnen implementeren Azure Stack Development Kit (ASDK).
* Een Azure-Stack-Operator moet ook: implementeer de App Service, plannen en aanbiedingen maken, een tenantabonnement maken en de Windows Server 2016-installatiekopie toe te voegen.

>[!NOTE]
>Als u al beschikt over sommige van deze onderdelen die zijn geïmplementeerd, zorg er dan voor dat ze voldoen aan de alle vereisten voordat u deze zelfstudie.

Deze zelfstudie wordt ervan uitgegaan dat u enige basiskennis hebben van Azure en Azure-Stack. Voor meer informatie voordat u de zelfstudie begint, leest u de volgende artikelen:

* [Inleiding tot Azure](https://azure.microsoft.com/overview/what-is-azure/)
* [Belangrijkste concepten van Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure-requirements"></a>Azure-vereisten

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
* Maak een [Web-App](https://docs.microsoft.com/azure/app-service/app-service-web-overview) in Azure. Houd er rekening mee van de Web-App-URL, moet u moet worden gebruikt in de zelfstudie.

### <a name="azure-stack-requirements"></a>Azure Stack-vereisten

* Gebruik een Azure-Stack geïntegreerd systeem of Azure Stack Development Kit (ASDK) implementeren. De ASDK implementeren:
    * De [zelfstudie: implementeer de ASDK met behulp van het installatieprogramma](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy) biedt gedetailleerde implementatie-instructies.
    * Gebruik de [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ) PowerShell-script ASDK na de implementatie stappen automatiseren.

    > [!Note]
    > De installatie van de ASDK duurt ongeveer 7 uur moet zijn voltooid, dus plan dienovereenkomstig.

 * Implementeer [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS-services naar Azure-Stack.
 * Maak [Plan/aanbiedingen](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) in Azure-Stack.
 * Maak een [tenant abonnement](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) in Azure-Stack.
 * Een Web-App maken in de tenant-abonnement. Noteer de URL van de nieuwe App voor later gebruik.
 * VSTS virtuele Machine implementeren in de tenantabonnement.
* Geef een installatiekopie van Windows Server 2016 met .NET 3.5 voor een virtuele machine (VM). Deze virtuele machine wordt als een persoonlijke build-agent op uw Azure-Stack gebouwd.

### <a name="developer-tool-requirements"></a>Eisen voor ontwikkelaars

* Maak een [VSTS werkruimte](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services). Het aanmeldingsproces maakt een project met de naam **MyFirstProject**.
* [Installeer Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) en [aanmelden bij VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).
* Verbinding maken met uw project en [lokaal klonen](https://www.visualstudio.com/docs/git/gitquickstart).

 > [!Note]
 > Uw Azure-Stack-omgeving moet de juiste installatiekopieën extern gepubliceerd om te worden uitgevoerd van Windows Server en SQL Server. Ook moet het App Service geïmplementeerd.

## <a name="prepare-the-private-build-and-release-agent-for-visual-studio-team-services-integration"></a>Voorbereiden van de persoonlijke build en de release-agent voor Visual Studio Team Services-integratie

### <a name="prerequisites"></a>Vereisten

Visual Studio Team Services (VSTS) wordt geverifieerd op basis van Azure Resource Manager met behulp van een Service-Principal. VSTS moet hebben de **Inzender** rol inrichten resources in een Azure-Stack-abonnement.

De volgende stappen wordt beschreven wat is vereist om verificatie te configureren:

1. Maken van een Service-Principal of gebruik een bestaande Service-Principal.
2. Verificatie-sleutels voor de Service-Principal maken.
3. Het Azure-Stack-abonnement via toegangsbeheer op basis van rollen om toe te staan van de Service SPN (Principal Name) moet deel uitmaken van de rol van de inzender gevalideerd.
4. De definitie van een nieuwe Service maken in VSTS met behulp van de Azure-Stack-eindpunten en de SPN-informatie.

### <a name="create-a-service-principal"></a>Een Service-Principal maken

Raadpleeg de [Service-Principal maken](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) instructies voor het maken van een service-principal en kies vervolgens **Web-App/API** voor het toepassingstype.

### <a name="create-an-access-key"></a>Een toegangssleutel maken

Een Service-Principal vereist een sleutel voor verificatie. Gebruik de volgende stappen om een sleutel te genereren.

1. Selecteer uw toepassing bij **App-registraties** in Azure Active Directory.

    ![Selecteer de toepassing](media\azure-stack-solution-hybrid-pipeline\000_01.png)

2. Noteer de waarde van **toepassings-ID**. U gebruikt deze waarde bij het configureren van het service-eindpunt in VSTS.

    ![Toepassings-id](media\azure-stack-solution-hybrid-pipeline\000_02.png)

3. Selecteer **Instellingen** om een verificatiesleutel te genereren.

    ![App-instellingen bewerken](media\azure-stack-solution-hybrid-pipeline\000_03.png)

4. Selecteer **Sleutels** om een verificatiesleutel te genereren.

    ![Belangrijke instellingen configureren](media\azure-stack-solution-hybrid-pipeline\000_04.png)

5. Geef een beschrijving op voor de sleutel en de duur van de sleutel instellen. Selecteer **Opslaan** wanneer u klaar bent.

    ![Beschrijving van belangrijke en duur](media\azure-stack-solution-hybrid-pipeline\000_05.png)

    Nadat u de sleutel opslaan **waarde** wordt weergegeven. Deze waarde niet kopiëren omdat u deze waarde hoger kan niet ophalen. U bieden de **sleutelwaarde** met de toepassings-ID aan te melden als de toepassing. Sla de sleutelwaarde op waar uw toepassing deze kan ophalen.

    ![Sleutel waarde](media\azure-stack-solution-hybrid-pipeline\000_06.png)

### <a name="get-the-tenant-id"></a>De tenant-ID niet ophalen

Als onderdeel van de configuratie van de service-eindpunt VSTS vereist de **Tenant-ID** die overeenkomt met de AAD-Directory dat is geïmplementeerd voor uw Azure-Stack stempel. Gebruik de volgende stappen om op te halen van de Tenant-ID.

1. Selecteer **Azure Active Directory**.

    ![Azure Active Directory voor tenant](media\azure-stack-solution-hybrid-pipeline\000_07.png)

2. Haal de tenant-id op door **Eigenschappen** voor uw Azure AD-tenant te selecteren.

    ![Eigenschappen van de tenant weergeven](media\azure-stack-solution-hybrid-pipeline\000_08.png)

3. Kopieer de **Directory-id**. Deze waarde is uw tenant-id.

    ![Map-id](media\azure-stack-solution-hybrid-pipeline\000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>De service-principal rechten voor het implementeren van resources in de Stack van Azure-abonnement toewijzen

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol vertegenwoordigt de beste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van het bereik. Bijvoorbeeld, een toepassing met de rol Lezer voor een resourcegroep toe te voegen dat kan de resourcegroep en alle bijbehorende resources gelezen.

1. Ga naar het niveau van het bereik dat u wilt toewijzen van de toepassing. Selecteer bijvoorbeeld als een rol bij het abonnementsbereik wilt toewijzen, **abonnementen**.

    ![Selecteer abonnementen](media\azure-stack-solution-hybrid-pipeline\000_10.png)

2. In **abonnement**, selecteert u Visual Studio Enterprise.

    ![Visual Studio Enterprise](media\azure-stack-solution-hybrid-pipeline\000_11.png)

3. Selecteer in Visual Studio Enterprise **Access Control (IAM)**.

    ![Toegangsbeheer (IAM)](media\azure-stack-solution-hybrid-pipeline\000_12.png)

4. Selecteer **Toevoegen**.

    ![Toevoegen](media\azure-stack-solution-hybrid-pipeline\000_13.png)

5. In **machtigingen toevoegen**, selecteer de rol die u wilt toewijzen aan de toepassing. In dit voorbeeld wordt de **eigenaar** rol.

    ![Rol van eigenaar](media\azure-stack-solution-hybrid-pipeline\000_14.png)

6. Azure Active Directory-toepassingen worden niet standaard weergegeven in de beschikbare opties. Als u wilt zoeken in uw toepassing, moet u de naam ervan in opgeven de **Selecteer** veld dat u wilt zoeken. Selecteer de app.

    ![App-zoekresultaat](media\azure-stack-solution-hybrid-pipeline\000_16.png)

7. Selecteer **opslaan** voltooid de rol toe te wijzen. Ziet u uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

### <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Azure op rollen gebaseerde toegangsbeheer (RBAC) biedt Geavanceerd toegangsbeheer voor Azure. U kunt met RBAC kunt bepalen het niveau van toegang die gebruikers nodig hebben om hun werk te doen. Zie voor meer informatie over toegangsbeheer op basis van rollen [toegang beheren tot Azure-Abonnementresources](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

### <a name="vsts-agent-pools"></a>VSTS Agent groepen

In plaats van elke agent afzonderlijk beheert, kunt u agents indelen in groepen van de agent. Een agent-groep definieert de delen grens voor alle agents in die groep. In VSTS, zijn agent pools gericht op het account VSTS, wat betekent dat u een agent van toepassingen via teamprojecten kunt delen. Zie voor meer informatie over agent-groepen, [Agent groepen maken en wachtrijen](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts).

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>Toevoegen van een persoonlijk toegangstoken (PAT) voor Azure Stack

Maak een Personal Access Token voor toegang tot VSTS.

1. Aanmelden bij uw account VSTS en selecteer de naam van uw profiel.
2. Selecteer **beveiliging beheren** naar de pagina voor toegang tot het token maken.

    ![Gebruiker aanmelden](media\azure-stack-solution-hybrid-pipeline\000_17.png)

    ![Selecteer teamproject](media\azure-stack-solution-hybrid-pipeline\000_18.png)

    ![Persoonlijke toegangstoken toevoegen](media\azure-stack-solution-hybrid-pipeline\000_18a.png)

    ![Token aanmaken](media\azure-stack-solution-hybrid-pipeline\000_18b.png)

3. Kopieer het token.

    > [!Note]
    > De token gegevens opslaan. Deze informatie wordt niet opgeslagen en worden niet meer weergegeven wanneer u de webpagina laten.

    ![Persoonlijke toegangstoken](media\azure-stack-solution-hybrid-pipeline\000_19.png)

### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Installeer die de VSTS agent op de Stack Azure bouwen gehoste Server bouwen

1. Verbinding maken met uw Build-Server die u hebt geïmplementeerd op de Stack van Azure-host.
2. Downloaden en implementeren van de build-agent als een service met behulp van uw persoonlijke toegangstoken (PAT) en wordt uitgevoerd als de VM-beheeraccount.

    ![Build-agent downloaden](media\azure-stack-solution-hybrid-pipeline\010_downloadagent.png)

3. Navigeer naar de map voor de uitgepakte build-agent. Voer de **run.cmd** bestand vanaf een opdrachtprompt met verhoogde bevoegdheid.

    ![Uitgepakte build-agent](media\azure-stack-solution-hybrid-pipeline\000_20.png)

    ![Build-agent registreren](media\azure-stack-solution-hybrid-pipeline\000_21.png)

4. Wanneer de run.cmd is voltooid, wordt de map van de build-agent wordt bijgewerkt met extra bestanden. De map met de geëxtraheerde inhoud moet eruitzien als in het volgende:

    ![Agent map update bouwen](media\azure-stack-solution-hybrid-pipeline\009_token_file.png)

    U kunt de agent in de map VSTS zien.

## <a name="endpoint-creation-permissions"></a>Machtigingen voor het eindpunt maken

Eindpunten maken, kan een Visual Studio Online VSTO ()-build Azure Service-apps implementeren naar Azure-Stack. VSTS verbindt met de build-agent, die verbinding met Azure-Stack maakt.

![Voorbeeld-app in VSTO NorthwindCloud](media\azure-stack-solution-hybrid-pipeline\012_securityendpoints.png)

1. Aanmelden bij VSTO en navigeer naar de pagina app-instellingen.
2. Op **instellingen**, selecteer **beveiliging**.
3. In **VSTS groepen**, selecteer **eindpunt auteurs**.

    ![NorthwindCloud-eindpunt maken](media\azure-stack-solution-hybrid-pipeline\013_endpoint_creators.png)

4. Op de **leden** tabblad **toevoegen**.

    ![Een lid toevoegen](media\azure-stack-solution-hybrid-pipeline\014_members_tab.png)

5. In **gebruikers en groepen toevoegen**, voer een gebruikersnaam en selecteert u die gebruiker uit de lijst met gebruikers.
6. Selecteer **wijzigingen opslaan**.
7. In de **VSTS groepen** selecteert **eindpunt beheerders**.

    ![NorthwindCloud Endpoint-beheerders](media\azure-stack-solution-hybrid-pipeline\015_save_endpoint.png)

8. Op de **leden** tabblad **toevoegen**.
9. In **gebruikers en groepen toevoegen**, voer een gebruikersnaam en selecteert u die gebruiker uit de lijst met gebruikers.
10. Selecteer **wijzigingen opslaan**.

Nu de informatie over endpoint bestaat, de VSTS naar Azure Stack-verbinding is gereed om te gebruiken. De build-agent in Azure-Stack ontvangt instructies uit VSTS en daarna de agent wordt informatie over endpoint voor communicatie met de Azure-Stack.

![Agent bouwen](media\azure-stack-solution-hybrid-pipeline\016_save_changes.png)

## <a name="develop-your-application-build"></a>Het bouwen van uw toepassing ontwikkelen

In dit gedeelte van de zelfstudie hebt u de:

* Voeg code toe aan een project VSTS.
* Maken van zelfstandige web app-implementatie.
* Het continue implementatie configureren

> [!Note]
 > Uw Azure-Stack-omgeving moet de juiste installatiekopieën extern gepubliceerd om te worden uitgevoerd van Windows Server en SQL Server. Ook moet het App Service geïmplementeerd. Controleer de App Service-documentatie sectie 'Vereisten' voor Azure Stack Operator vereisten.

Hybride CI/CD kunt toepassen op zowel de toepassingscode en de code van de infrastructuur. Gebruik [Azure Resource Manager-sjablonen, zoals web ](https://azure.microsoft.com/resources/templates/) app-code van VSTS om te implementeren op beide clouds.

### <a name="add-code-to-a-vsts-project"></a>Voeg code toe aan een project VSTS

1. Aanmelden bij VSTS met een account met rechten voor het project maken op Azure-Stack. De volgende schermafbeelding ziet u hoe verbinding maken met het HybridCICD-project.

    ![Verbinding maken met een Project](media\azure-stack-solution-hybrid-pipeline\017_connect_to_project.png)

2. **De opslagplaats klonen** door het maken en openen van de standaard web-app.

    ![De opslagplaats klonen](media\azure-stack-solution-hybrid-pipeline\018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Maken van zelfstandige web app-implementatie voor App-Services in beide clouds

1. Bewerken de **WebApplication.csproj** bestand: Selecteer **Runtimeidentifier** en voeg vervolgens `win10-x64.` voor meer informatie Zie [zelfstandige implementatie](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) documentatie.

    ![Runtimeidentifier configureren](media\azure-stack-solution-hybrid-pipeline\019_runtimeidentifer.png)

2. Team Explorer gebruiken om te controleren van de code in VSTS.

3. Bevestig dat de toepassingscode in Visual Studio Team Services is ingeschakeld.

### <a name="create-the-build-definition"></a>De definitie van de build maken

1. Aanmelden bij VSTS met een account dat de definitie van een build kunt maken.
2. Navigeer naar de **bouwen Web Applicaiton** pagina voor het project.

3. In **argumenten**, Voeg **win10 - r-x64** code. Dit is vereist voor het activeren van een zelfstandige implementatie met .net Core.

    ![Argument build definitie toevoegen](media\azure-stack-solution-hybrid-pipeline\020_publish_additions.png)

4. De build worden uitgevoerd. De [zelfstandige implementatie build](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) proces artefacten die kunnen worden uitgevoerd op Azure en Azure Stack zal publiceren.

### <a name="use-an-azure-hosted-build-agent"></a>Gebruik een Azure gehoste build-agent

Met behulp van een gehoste build-agent in VSTS is een geschikte optie voor het bouwen en implementeren van web-apps. Agentonderhoud en upgrades worden automatisch uitgevoerd door Microsoft Azure, waardoor een continue en ononderbroken ontwikkelingscyclus.

### <a name="configure-the-continuous-deployment-cd-process"></a>Het continue implementatie (CD) configureren

Visual Studio Team Services (VSTS) en Team Foundation Server (TFS) voorzien in een pijplijn maximaal configureerbare en beheerbare releases in meerdere omgevingen zoals ontwikkeling, tijdelijke opslag, kwaliteit assurance (QA) en productie. Dit proces kunt opnemen goedkeuringen vereisen specifieke stadia van de levenscyclus van de toepassing.

### <a name="create-release-definition"></a>Release-definitie maken

Maken van de definitie van een release is de laatste stap in uw toepassing bouwen proces. Deze release-definitie wordt een releaserecord maken en implementeren van een build gebruikt.

1. Aanmelden bij VSTS en navigeer naar **bouwen en de vrijgave van** voor uw project.
2. Op de **Releases** tabblad  **\[ +]** en selecteer vervolgens **maken release definitie**.

   ![Release-definitie maken](media\azure-stack-solution-hybrid-pipeline\021a_releasedef.png)

3. Op **Selecteer een sjabloon**, kies **Azure App Service-implementatie**, en selecteer vervolgens **toepassen**.

    ![Sjabloon toepassen](media\azure-stack-solution-hybrid-pipeline\102.png)

4. Op **toevoegen artefact**, uit de **bron (Build-definitie)** vervolgkeuzelijst, selecteer de Azure-Cloud-build-app.

    ![Artefact toevoegen](media\azure-stack-solution-hybrid-pipeline\103.png)

5. Op de **pijplijn** tabblad de **fase 1**, **1 taak** koppelen aan **omgevingstaken weergeven**.

    ![Taken voor pipeline-weergeven](media\azure-stack-solution-hybrid-pipeline\104.png)

6. Op de **taken** tabblad, voert Azure als de **omgevingsnaam** en selecteer de AzureCloud Traders Web EP uit de **Azure-abonnement** vervolgkeuzelijst.

    ![Omgevingsvariabelen instellen](media\azure-stack-solution-hybrid-pipeline\105.png)

7. Voer de **Azure app service-naam**, namelijk 'Noordenwind' in de volgende schermopname.

    ![App service-naam](media\azure-stack-solution-hybrid-pipeline\106.png)

8. Selecteer voor de fase Agent **gehost VS2017** van de **Agent wachtrij** vervolgkeuzelijst.

    ![Gehoste agent](media\azure-stack-solution-hybrid-pipeline\107.png)

9. In **Azure App Service implementeren**, selecteert u het geldige **pakket of de map** voor de omgeving.

    ![Selecteer het pakket of de map](media\azure-stack-solution-hybrid-pipeline\108.png)

10. In **Selecteer bestand of map**, selecteer **OK** naar **locatie**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\109.png)

11. Alle wijzigingen opslaan en terug te keren naar **pijplijn**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\110.png)

12. Op de **pijplijn** tabblad **toevoegen artefact**, en kies de **NorthwindCloud Traders-vat** van de **bron (definitie bouwen)** vervolgkeuzelijst.

    ![Nieuwe artefacten toevoegen](media\azure-stack-solution-hybrid-pipeline\111.png)

13. Op **Selecteer een sjabloon**, toevoegen van een andere omgeving. Kies **Azure App Service-implementatie** en selecteer vervolgens **toepassen**.

    ![Sjabloon selecteren](media\azure-stack-solution-hybrid-pipeline\112.png)

14. Voer 'Azure Stack' als de **omgevingsnaam**.

    ![Naam van de omgeving](media\azure-stack-solution-hybrid-pipeline\113.png)

15. Op de **taken** tabblad, zoeken en selecteert u Azure-Stack.

    ![Azure Stack-omgeving](media\azure-stack-solution-hybrid-pipeline\114.png)

16. Van de **Azure-abonnement** vervolgkeuzelijst "AzureStack Traders vat EP" Selecteer voor het eindpunt van de Azure-Stack.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\115.png)

17. Voer de Azure-Stack web app-naam als de **App service-naam**.

    ![App service-naam](media\azure-stack-solution-hybrid-pipeline\116.png)

18. Onder **Agent selectie**, kiest u 'AzureStack - bDouglas Fir' uit de **Agent wachtrij** vervolgkeuzelijst.

    ![Kies agent](media\azure-stack-solution-hybrid-pipeline\117.png)

19. Voor **Azure App Service implementeren**, selecteert u het geldige **pakket of de map** voor de omgeving. Op **bestand of map selecteren**, selecteer **OK** voor de map **locatie**.

    ![Kies pakket of map](media\azure-stack-solution-hybrid-pipeline\118.png)

    ![Locatie goedkeuren](media\azure-stack-solution-hybrid-pipeline\119.png)

20. Op de **variabele** tabblad, zoeken de variabele met de naam **VSTS_ARM_REST_IGNORE_SSL_ERRORS**. De variabele waarde ingesteld op **true**, en stelt u het bereik op **Azure Stack**.

    ![Variabele configureren](media\azure-stack-solution-hybrid-pipeline\120.png)

21. Op de **pijplijn** tabblad de **continue implementatie trigger** pictogram voor de NorthwindCloud Traders-Web-artefacten en stel de **continue implementatie trigger** naar **Ingeschakeld**.  Doe hetzelfde voor het artefact ' NorthwindCloud Traders-vat '.

    ![Set continue implementatie trigger](media\azure-stack-solution-hybrid-pipeline\121.png)

22. Voor de Azure-Stack-omgeving, selecteert u de **vóór implementatie voorwaarden** pictogram de trigger ingesteld op **na release**.

    ![Set voorwaarden vóór implementatie trigger](media\azure-stack-solution-hybrid-pipeline\122.png)

23. Sla al uw wijzigingen op.

> [!Note]
> Sommige instellingen voor release taken kunnen automatisch gedefinieerd als [omgevingsvariabelen](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) tijdens het maken van een release-definitie van een sjabloon. Deze instellingen worden niet gewijzigd in de taakinstellingen. U kunt echter deze instellingen in de omgeving bovenliggende items bewerken.

## <a name="create-a-release"></a>Een releaserecord maken

Nu dat u de wijzigingen in de definitie van de release hebt voltooid, is het tijd om de implementatie start. Om dit te doen, moet u een versie van de release-definitie maken. Een releaserecord kan worden automatisch gemaakt. bijvoorbeeld is de trigger doorlopende implementatie ingesteld in de release-definitie. Dit betekent dat het wijzigen van de broncode van een nieuwe build Start en die een nieuwe release. Echter, in deze sectie maakt u een nieuwe release handmatig.

1. Op de **pijplijn** tabblad en open de **Release** vervolgkeuzelijst lijst en kies **release gemaakt**.

    ![Een releaserecord maken](media\azure-stack-solution-hybrid-pipeline\200.png)

2. Voer een beschrijving voor de release, controleert u dat de juiste artefacten zijn geselecteerd en kies **maken**. Na enkele ogenblikken een banner wordt weergegeven dat aangeeft dat de nieuwe versie is gemaakt en de release-naam wordt weergegeven als een koppeling. Kies de koppeling om te zien, de overzichtspagina release.

    ![Release maken banner](media\azure-stack-solution-hybrid-pipeline\201.png)

3. De overzichtspagina voor de release voor meer informatie over de versie bevat. In de volgende schermopname voor 'Release 2', de **omgevingen** sectie toont de **Implementatiestatus** voor de Azure als 'IN bewerking' en de status voor Azure-Stack 'voltooid'. Als de implementatiestatus voor de Azure-omgeving verandert in 'Geslaagd', een banner wordt weergegeven dat aangeeft dat de release gereed voor goedkeuring is. Wanneer een implementatie is in behandeling of is mislukt, een blauwe **(i)** het informatiepictogram dat wordt weergegeven. Beweeg de muisaanwijzer over het pictogram om te zien van een pop-upvenster met de reden voor de vertraging of mislukken.

    ![Overzichtspagina van release](media\azure-stack-solution-hybrid-pipeline\202.png)

Andere weergaven, zoals de lijst met releases, toont ook een pictogram dat aangeeft goedkeuring is in behandeling. Het pop-upvenster voor dit pictogram ziet u de omgevingsnaam van de en meer informatie over de implementatie. Het is gemakkelijk voor een beheerder raadpleegt u de algemene voortgang van releases en zien welke versies op goedkeuring wachten.

### <a name="monitor-and-track-deployments"></a>Bewaken en bijhouden implementaties

Deze sectie wordt beschreven hoe u kunt bewaken en bijhouden van alle uw implementaties. De release voor het implementeren van de twee websites met Azure App Services biedt een goed voorbeeld.

1. Selecteer op de overzichtspagina 'Release 2' **logboeken**. Tijdens een implementatie tonen deze pagina de live logboek van de agent. Het linkerdeelvenster ziet de status van elke bewerking in de implementatie voor elke omgeving.

    U kunt een persoon-pictogram in de **actie** kolom voor een goedkeuring vóór de implementatie of na de implementatie heeft goedgekeurd (of afgewezen) op de implementatie, en het bericht die zijn opgegeven.

2. Nadat de implementatie is voltooid, wordt het hele logboekbestand in het rechterdeelvenster weergegeven. U kunt kiezen **stap** in het linkerdeelvenster om te zien van het logboekbestand voor een één stap, zoals "Job initialiseren". De mogelijkheid om te zien van afzonderlijke logboeken gemakkelijker traceren en fouten opsporen in onderdelen van de algehele implementatie. U kunt ook **opslaan** het logboekbestand voor een stap of **alle logboeken downloaden als zip**.

    ![Release-Logboeken](media\azure-stack-solution-hybrid-pipeline\203.png)

3. Open de **samenvatting** tabblad voor algemene informatie over de release. Deze weergave toont informatie over de build, de omgevingen die is geïmplementeerd naar, Implementatiestatus en andere informatie over de release.

4. Selecteer de koppeling van een omgeving (**Azure** of **Azure Stack**) voor informatie over bestaande en in afwachting van implementaties met een specifieke omgeving. U kunt deze weergaven gebruiken als een snelle manier om te controleren dat dezelfde build voor beide omgevingen is geïmplementeerd.

5. Open de **productie-app geïmplementeerd** in uw browser. Bijvoorbeeld: open de URL voor de website Azure App Services `http://[your-app-name].azurewebsites.net`.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over Azure-Cloud-patronen, [ontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).

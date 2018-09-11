---
title: Uw app implementeren in Azure en Azure Stack | Microsoft Docs
description: Leer hoe u apps implementeren op Azure en Azure Stack met een hybride CI/CD-pijplijn.
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
ms.date: 09/04/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 773acd3a22244403548ef4ce35164291f5c0be7d
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44300832"
---
# <a name="tutorial-deploy-apps-to-azure-and-azure-stack"></a>Zelfstudie: apps implementeren in Azure en Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Leer hoe u een toepassing implementeren in Azure en Azure Stack met behulp van een hybride-pijplijn voor continue integratie/continue levering (CI/CD).

In deze zelfstudie maakt u een Voorbeeldomgeving:

> [!div class="checklist"]
> * Start een nieuwe build op basis van de doorvoeringen code naar uw opslagplaats Azure DevOps-Services.
> * Uw app automatisch implementeren op de globale Azure voor het testen van de acceptatie van de gebruiker.
> * Wanneer uw code testen verstrijkt, moet u automatisch de app implementeren op Azure Stack.

## <a name="benefits-of-the-hybrid-delivery-build-pipe"></a>Voordelen van de hybride levering bouwen pipe

Bedrijfscontinuïteit te waarborgen, beveiliging en betrouwbaarheid zijn de belangrijkste elementen van de implementatie van toepassing. Deze elementen zijn essentieel voor uw organisatie en essentieel is voor uw ontwikkelteam. Een hybride CI/CD-pijplijn kunt u uw build-pipes consolideren in uw on-premises omgeving en de openbare cloud. Een hybride levering-model kunt u ook implementatie locaties wijzigen zonder uw toepassing worden gewijzigd.

Andere voordelen voor het gebruik van de hybride-benadering zijn:

* U kunt een consistente set ontwikkelhulpprogramma's onderhouden via uw on-premises Azure Stack-omgeving en de openbare cloud van Azure.  Een algemene set hulpprogramma's maakt het gemakkelijker te implementeren, CI/CD-patronen en procedures.
* Apps en services die zijn geïmplementeerd in Azure of Azure Stack zijn verwisselbaar en dezelfde code kunt uitvoeren in één van beide locaties. U kunt profiteren van de on-premises en openbare cloud-functies en mogelijkheden.

Voor meer informatie over CI en CD:

* [Wat is continue integratie?](https://www.visualstudio.com/learn/what-is-continuous-integration/)
* [Wat is continue levering?](https://www.visualstudio.com/learn/what-is-continuous-delivery/)

## <a name="prerequisites"></a>Vereisten

U moet hebben van onderdelen voor het bouwen van een hybride CI/CD-pijplijn. De volgende onderdelen duurt lang om voor te bereiden:

* Een Azure-OEM/hardware-partner kunt een productie Azure Stack implementeren. Alle gebruikers kunnen de Azure Stack Development Kit (ASDK) implementeren.
* Een Azure Stack-Operator moet ook: de App Service implementeren, plannen en aanbiedingen maken, maakt u een tenantabonnement en de installatiekopie van Windows Server 2016 kunt toevoegen.

>[!NOTE]
>Als u nog enkele van deze onderdelen die zijn geïmplementeerd, zorg er dan voor dat ze voldoen aan de alle vereisten voordat u begint met deze zelfstudie.

In deze zelfstudie wordt ervan uitgegaan dat u enige basiskennis van Azure en Azure Stack hebt. Lees de volgende artikelen voor meer informatie voordat u de zelfstudie begint:

* [Inleiding tot Azure](https://azure.microsoft.com/overview/what-is-azure/)
* [Azure Stack-basisbegrippen](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure-requirements"></a>Azure-vereisten

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
* Maak een [Web-App](https://docs.microsoft.com/azure/app-service/app-service-web-overview) in Azure. Noteer de URL van de Web-App, moet u deze gebruiken in de zelfstudie.

### <a name="azure-stack-requirements"></a>Vereisten voor Azure Stack

* Een geïntegreerde Azure Stack-systeem gebruiken of de Azure Stack Development Kit (ASDK) implementeren. De ASDK implementeren:
    * De [zelfstudie: implementeert de ASDK met behulp van het installatieprogramma](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy) bevat gedetailleerde implementatie-instructies.
    * Gebruik de [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ) PowerShell-script voor het automatiseren van de stappen na de implementatie ASDK.

    > [!Note]
    > De installatie van de ASDK duurt ongeveer 7 uur om te voltooien, dus dienovereenkomstig te plannen.

 * Implementeer [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS-services met Azure Stack.
 * Maak [-Plan/aanbiedingen](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) in Azure Stack.
 * Maak een [tenant abonnement](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) in Azure Stack.
 * Een Web-App maken in de tenantabonnement. Noteer de URL van de nieuwe Web-App voor later gebruik.
 * Virtuele Machine van Azure DevOps Services implementeren in de tenantabonnement.
* Geef een installatiekopie van Windows Server 2016 met .NET 3.5 voor een virtuele machine (VM). Deze virtuele machine worden in uw Azure Stack samengesteld als een persoonlijke bouwagent.

### <a name="developer-tool-requirements"></a>Vereisten voor Developer-hulpprogramma

* Maak een [Azure DevOps-Services-werkruimte](https://docs.microsoft.com/azure/devops/repos/tfvc/create-work-workspaces). Het aanmeldingsproces maakt een project met de naam **MyFirstProject**.
* [Installeer Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) en [aanmelden bij Azure DevOps Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).
* Verbinding maken met uw project en [deze lokaal klonen](https://www.visualstudio.com/docs/git/gitquickstart).

 > [!Note]
 > Uw Azure Stack-omgeving, moet de juiste afbeeldingen publiceren voor het uitvoeren van Windows Server en SQL Server. Ook moet het Appservice is geïmplementeerd.

## <a name="prepare-the-private-azure-pipelines-agent-for-azure-devops-services-integration"></a>Voorbereiden van de persoonlijke Azure-pijplijnen-agent voor integratie met Azure DevOps-Services

### <a name="prerequisites"></a>Vereisten

Azure DevOps-Services worden geverifieerd op basis van Azure Resource Manager met behulp van een Service-Principal. Azure DevOps-Services moet hebben de **Inzender** rol inrichting van resources in een Azure Stack-abonnement.

De volgende stappen wordt beschreven wat is vereist om verificatie te configureren:

1. Maken van een Service-Principal of gebruik een bestaande Service-Principal.
2. Verificatiesleutels voor de Service-Principal maken.
3. Valideren van de Azure Stack-abonnement via toegangsbeheer op basis van rollen om toe te staan van de Service SPN (Principal Name) moet deel uitmaken van de rol van de Inzender.
4. De definitie van een nieuwe Service maken in Azure DevOps-Services met behulp van de Azure Stack-eindpunten en de SPN-informatie.

### <a name="create-a-service-principal"></a>Een Service-Principal maken

Raadpleeg de [Service-Principal maken](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) instructies voor het maken van een service-principal. Kies **Web-App/API** voor het toepassingstype of [het PowerShell-script gebruiken](https://github.com/Microsoft/vsts-rm-extensions/blob/master/TaskModules/powershell/Azure/SPNCreation.ps1#L5) , zoals wordt beschreven in het artikel [Maak een Azure Resource Manager serviceverbinding met een bestaande service Principal ](https://docs.microsoft.com/vsts/pipelines/library/connect-to-azure?view=vsts#create-an-azure-resource-manager-service-connection-with-an-existing-service-principal).

 > [!Note]  
 > Als u het script gebruiken om een Azure Stack Azure Resource Manager-eindpunt te maken, moet u om door te geven de **- azureStackManagementURL** parameter en **- environmentName** parameter. Bijvoorbeeld:  
> `-azureStackManagementURL https://management.local.azurestack.external -environmentName AzureStack`

### <a name="create-an-access-key"></a>Maak een toegangstoets

Een Service-Principal is een sleutel voor verificatie vereist. Gebruik de volgende stappen uit om een sleutel te genereren.

1. Selecteer uw toepassing bij **App-registraties** in Azure Active Directory.

    ![Selecteer de toepassing](media\azure-stack-solution-hybrid-pipeline\000_01.png)

2. Noteer de waarde van **toepassings-ID**. U gebruikt deze waarde bij het configureren van het service-eindpunt in Azure DevOps-Services.

    ![Toepassings-id](media\azure-stack-solution-hybrid-pipeline\000_02.png)

3. Selecteer **Instellingen** om een verificatiesleutel te genereren.

    ![App-instellingen bewerken](media\azure-stack-solution-hybrid-pipeline\000_03.png)

4. Selecteer **Sleutels** om een verificatiesleutel te genereren.

    ![Belangrijke instellingen configureren](media\azure-stack-solution-hybrid-pipeline\000_04.png)

5. Geef een beschrijving voor de sleutel en de duur van de sleutel instellen. Selecteer **Opslaan** wanneer u klaar bent.

    ![Sleutelbeschrijving en duur](media\azure-stack-solution-hybrid-pipeline\000_05.png)

    Nadat u de sleutel opgeslagen **waarde** wordt weergegeven. Deze waarde niet kopiëren omdat u niet kunt deze waarde later ophalen. U opgeven de **sleutelwaarde** met toepassings-ID aan te melden als de toepassing. Bewaar de sleutelwaarde op een locatie waar de toepassing deze kan ophalen.

    ![Sleutel-waarde](media\azure-stack-solution-hybrid-pipeline\000_06.png)

### <a name="get-the-tenant-id"></a>Haal de tenant-ID

Als onderdeel van de configuratie van de service-eindpunt, Azure DevOps-Services is vereist de **Tenant-ID** die overeenkomt met de AAD-Directory dat is geïmplementeerd voor uw Azure Stack-stempel. Gebruik de volgende stappen om op te halen van de Tenant-ID.

1. Selecteer **Azure Active Directory**.

    ![Azure Active Directory voor tenant](media\azure-stack-solution-hybrid-pipeline\000_07.png)

2. Haal de tenant-id op door **Eigenschappen** voor uw Azure AD-tenant te selecteren.

    ![Eigenschappen van de tenant weergeven](media\azure-stack-solution-hybrid-pipeline\000_08.png)

3. Kopieer de **Map-id**. Deze waarde is uw tenant-id.

    ![Map-id](media\azure-stack-solution-hybrid-pipeline\000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>De service-principal-rechten voor het implementeren van resources in de Azure Stack-abonnement verlenen

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol staat voor de beste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van bereik. Bijvoorbeeld een toepassing toevoegt aan de rol van lezer voor een resourcegroep betekent dit dat het de resourcegroep en alle bijbehorende resources kunt lezen.

1. Ga naar het niveau van het bereik dat u wilt toewijzen van de toepassing. Selecteer bijvoorbeeld het volgende om te wijzen aan een rol op het abonnementsbereik, **abonnementen**.

    ![Abonnementen selecteren](media\azure-stack-solution-hybrid-pipeline\000_10.png)

2. In **abonnement**, selecteert u Visual Studio Enterprise.

    ![Visual Studio Enterprise](media\azure-stack-solution-hybrid-pipeline\000_11.png)

3. Selecteer in Visual Studio Enterprise, **Access Control (IAM)**.

    ![Access Control (IAM)](media\azure-stack-solution-hybrid-pipeline\000_12.png)

4. Selecteer **Toevoegen**.

    ![Toevoegen](media\azure-stack-solution-hybrid-pipeline\000_13.png)

5. In **machtigingen toevoegen**, selecteer de rol die u wilt toewijzen aan de toepassing. In dit voorbeeld wordt de **eigenaar** rol.

    ![Rol van eigenaar](media\azure-stack-solution-hybrid-pipeline\000_14.png)

6. Azure Active Directory-toepassingen worden niet standaard weergegeven in de beschikbare opties. Als u uw toepassing zoekt, moet u de naam ervan in de **Selecteer** veld om te zoeken naar deze. Selecteer de app.

    ![App-zoekresultaat](media\azure-stack-solution-hybrid-pipeline\000_16.png)

7. Selecteer **opslaan** voltooien van de rol toe te wijzen. U ziet uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

### <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Azure Role-Based Access Control (RBAC) biedt verfijnd toegangsbeheer voor Azure. Met RBAC kunt kunt u het niveau van toegang die gebruikers nodig hebben om hun werk te doen beheren. Zie voor meer informatie over Role-Based Access Control [toegang beheren tot de Resources voor Azure-abonnement](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

### <a name="azure-devops-services-agent-pools"></a>Agentpools van Azure DevOps-Services

In plaats van elke agent afzonderlijk beheren, kunt u agents indelen in groepen van de agent. Een agentpool definieert de delen grens voor alle agents in die groep. Agentpools zijn in Azure DevOps-Services gericht op de organisatie Azure DevOps-Services, wat betekent dat u een agentpool projecten kunt delen. Zie voor meer informatie over agentpools [Agentpools maken en wachtrijen](https://docs.microsoft.com/azure/devops/pipelines/agents/pools-queues?view=vsts).

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>Toevoegen van een persoonlijk toegangstoken (PAT) voor Azure Stack

Maak een persoonlijk toegangstoken voor toegang tot Azure DevOps-Services.

1. Aanmelden bij uw organisatie Azure DevOps-Services en selecteert u de naam van uw organisatie-profiel.

2. Selecteer **beveiliging beheren** access token maken-pagina.

    ![Gebruikersaanmelding](media\azure-stack-solution-hybrid-pipeline\000_17.png)

    ![Selecteer een project](media\azure-stack-solution-hybrid-pipeline\000_18.png)

    ![Persoonlijk toegangstoken toevoegen](media\azure-stack-solution-hybrid-pipeline\000_18a.png)

    ![Token maken](media\azure-stack-solution-hybrid-pipeline\000_18b.png)

3. Kopieer het token.

    > [!Note]
    > De gegevens van de token niet opslaan. Deze informatie wordt niet opgeslagen en worden niet meer weergegeven als het verlaten van de webpagina wordt weergegeven.

    ![Persoonlijk toegangstoken](media\azure-stack-solution-hybrid-pipeline\000_19.png)

### <a name="install-the-azure-devops-services-build-agent-on-the-azure-stack-hosted-build-server"></a>Installeer die de Azure DevOps-Services agent in de Azure Stack bouwen gehoste Server bouwen

1. Verbinding maken met uw Build-Server die u hebt geïmplementeerd op de Azure Stack-host.
2. Downloaden en implementeren van de build-agent als een service met behulp van uw persoonlijke toegangstoken (PAT) en worden uitgevoerd als het account van de VM-beheer.

    ![Build-agent downloaden](media\azure-stack-solution-hybrid-pipeline\010_downloadagent.png)

3. Navigeer naar de map voor de uitgepakte bouwagent. Voer de **config.cmd** bestand vanaf een opdrachtprompt met verhoogde bevoegdheid.

    ![Geëxtraheerde bouwagent](media\azure-stack-solution-hybrid-pipeline\000_20.png)

    ![Build-agent registreren](media\azure-stack-solution-hybrid-pipeline\000_21.png)

4. Wanneer de config.cmd is voltooid, wordt de map van de build-agent wordt bijgewerkt met extra bestanden. De map met de uitgepakte inhoud ziet er als volgt uit:

    ![Bouw agent map update](media\azure-stack-solution-hybrid-pipeline\009_token_file.png)

    Hier ziet u de agent in de map Azure DevOps-Services.

## <a name="endpoint-creation-permissions"></a>Machtigingen voor het eindpunt maken

Met het maken van eindpunten, implementeren een build van het Visual Studio Online (VSTO) Azure Service-apps met Azure Stack. Azure DevOps-Services verbinding met de build-agent, die verbinding met Azure Stack maakt.

![Voorbeeld-app in VSTO NorthwindCloud](media\azure-stack-solution-hybrid-pipeline\012_securityendpoints.png)

1. Aanmelden bij VSTO en navigeer naar de pagina app-instellingen.
2. Op **instellingen**, selecteer **Security**.
3. In **Azure DevOps-Services groepen**, selecteer **makers van eindpunt**.

    ![Makers van NorthwindCloud eindpunt](media\azure-stack-solution-hybrid-pipeline\013_endpoint_creators.png)

4. Op de **leden** tabblad **toevoegen**.

    ![Een lid toevoegen](media\azure-stack-solution-hybrid-pipeline\014_members_tab.png)

5. In **gebruikers en groepen toevoegen**, voer een gebruikersnaam en selecteert u die gebruiker in de lijst van gebruikers.
6. Selecteer **wijzigingen opslaan**.
7. In de **Azure DevOps-Services groepen** in de lijst met **eindpunt beheerders**.

    ![NorthwindCloud Endpoint-beheerders](media\azure-stack-solution-hybrid-pipeline\015_save_endpoint.png)

8. Op de **leden** tabblad **toevoegen**.
9. In **gebruikers en groepen toevoegen**, voer een gebruikersnaam en selecteert u die gebruiker in de lijst van gebruikers.
10. Selecteer **wijzigingen opslaan**.

Nu informatie over het eindpunt bestaat, het Azure DevOps-Services op Azure Stack-verbinding is klaar voor gebruik. De build-agent in Azure Stack haalt de instructies van Azure DevOps-Services en vervolgens de agent voor de communicatie met Azure Stack-eindpuntgegevens overbrengen.
## <a name="create-an-azure-stack-endpoint"></a>Een Azure Stack-eindpunt maken

U kunt de instructies in [een Azure Resource Manager-serviceverbinding met een bestaande service-principal maken ](https://docs.microsoft.com/vsts/pipelines/library/connect-to-azure?view=vsts#create-an-azure-resource-manager-service-connection-with-an-existing-service-principal) artikel een serviceverbinding met een bestaande service-principal maken en gebruiken van de volgende toewijzing:

- Omgeving: AzureStack
- URL van de omgeving: Er ongeveer als `https://management.local.azurestack.external`
- Abonnements-ID: Gebruiker abonnements-ID van Azure Stack
- De naam van abonnement: naam van gebruikersabonnement van Azure Stack
- Service-Principal-client-ID: de principal-ID van [dit](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-pipeline#create-a-service-principal) sectie in dit artikel.
- Sleutel van de Service-Principal: de sleutel van hetzelfde artikel (of het wachtwoord als u het script gebruikt).
- Tenant-ID: De tenant-ID u ophalen na de instructie op [ophalen van de tenant-ID](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-pipeline#get-the-tenant-id).

Nu het eindpunt is gemaakt, de VSTS aan Azure Stack-verbinding is klaar voor gebruik. De build-agent in Azure Stack haalt de instructies van VSTS en vervolgens de agent voor de communicatie met Azure Stack-eindpuntgegevens overbrengen.

![Agent bouwen](media\azure-stack-solution-hybrid-pipeline\016_save_changes.png)

## <a name="develop-your-application-build"></a>Het bouwen van uw toepassing ontwikkelen

In dit deel van de zelfstudie ziet u de:

* Voeg code toe aan een Azure-Services voor DevOps-project.
* Maken van zelfstandige web app-implementatie.
* Het continue implementatie configureren

> [!Note]
 > Uw Azure Stack-omgeving, moet de juiste afbeeldingen publiceren voor het uitvoeren van Windows Server en SQL Server. Ook moet het Appservice is geïmplementeerd. Raadpleeg de documentatie van de App Service sectie 'Vereisten' voor de vereisten voor Azure Stack-Operator.

Hybride CI/CD kunt toepassen op zowel de toepassingscode als de infrastructuurcode. Gebruik [Azure Resource Manager-sjablonen, zoals web ](https://azure.microsoft.com/resources/templates/) app-code van Azure DevOps-Services om te implementeren voor beide clouds.

### <a name="add-code-to-an-azure-devops-services-project"></a>Voeg code toe aan een Azure-Services voor DevOps-project

1. Meld u met Azure DevOps-Services met een organisatie met rechten voor het project maken in Azure Stack. De volgende schermopname ziet hoe u verbinding maken met de HybridCICD-project.

    ![Verbinding maken met een Project](media\azure-stack-solution-hybrid-pipeline\017_connect_to_project.png)

2. **Kloon de opslagplaats** door het maken en de standaard web-app te openen.

    ![De opslagplaats klonen](media\azure-stack-solution-hybrid-pipeline\018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Implementatie van zelfstandige web-app voor App Services in beide clouds maken

1. Bewerken de **WebApplication.csproj** bestand: Selecteer **Runtimeidentifier** en voeg deze `win10-x64.` voor meer informatie, Zie [onafhankelijke implementatie](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) documentatie.

    ![Runtimeidentifier configureren](media\azure-stack-solution-hybrid-pipeline\019_runtimeidentifer.png)

2. Team Explorer gebruiken om te controleren of de code in Azure DevOps-Services.

3. Bevestig dat de code van de toepassing in Azure DevOps-Services is ingeschakeld.

### <a name="create-the-build-pipeline"></a>De build-pijplijn maken

1. Meld u met Azure DevOps-Services met een organisatie die een build-pijplijn kunt maken.

2. Navigeer naar de **bouwen Web Applicaiton** pagina voor het project.

3. In **argumenten**, toevoegen **win10 - r-x64** code. Dit is vereist voor het activeren van een onafhankelijke implementatie met .net Core.

    ![Argument build-pijplijn toevoegen](media\azure-stack-solution-hybrid-pipeline\020_publish_additions.png)

4. De build worden uitgevoerd. De [onafhankelijke implementatie build](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) proces artefacten die kunnen worden uitgevoerd op Azure en Azure Stack zal publiceren.

### <a name="use-an-azure-hosted-build-agent"></a>Gebruik een Azure gehost bouwagent

Met behulp van een gehoste bouwagent in Azure DevOps-Services is een handige optie voor het bouwen en implementeren van web-apps. Automatisch worden onderhouden van de agent en upgrades uitgevoerd door Microsoft Azure, waarmee een doorlopende en ononderbroken ontwikkelingscyclus.

### <a name="configure-the-continuous-deployment-cd-process"></a>Het continue implementatie (CD) configureren

Azure DevOps-Services en Team Foundation Server (TFS) bieden een pijplijn maximaal kunnen worden geconfigureerd en beheerd voor releases tot meerdere omgevingen zoals ontwikkeling, fasering, kwaliteit te waarborgen (QA) en productie. Dit proces kunt opnemen goedkeuringen vereisen specifieke stadia van de levenscyclus van toepassingen.

### <a name="create-release-pipeline"></a>Release-pijplijn maken

Het maken van een release-pijplijn, is de laatste stap in uw toepassing bouwproces. Deze release-pijplijn wordt gebruikt om een releaserecord maken en implementeren van een build.

1. Aanmelden bij Azure DevOps-Services en navigeren naar **Azure pijplijnen** voor uw project.
2. Op de **Releases** tabblad  **\[ +]** en kies dan **release-definitie maken**.

   ![Release-pijplijn maken](media\azure-stack-solution-hybrid-pipeline\021a_releasedef.png)

3. Op **selecteert u een sjabloon**, kiest u **Azure App Service-implementatie**, en selecteer vervolgens **toepassen**.

    ![Sjabloon toepassen](media\azure-stack-solution-hybrid-pipeline\102.png)

4. Op **toevoegen artefact**, uit de **Source (Build definition)** vervolgkeuzelijst, selecteer de Azure-Cloud-build-app.

    ![Artefact toevoegen](media\azure-stack-solution-hybrid-pipeline\103.png)

5. Op de **pijplijn** tabblad de **1 fase**, **1 taak** koppelen aan **omgevingstaken weergeven**.

    ![Taken voor pijplijn weergeven](media\azure-stack-solution-hybrid-pipeline\104.png)

6. Op de **taken** tabblad, voert Azure als de **omgevingsnaam** en selecteer de AzureCloud Traders Web EP uit de **Azure-abonnement** vervolgkeuzelijst.

    ![Omgevingsvariabelen instellen](media\azure-stack-solution-hybrid-pipeline\105.png)

7. Voer de **Azure app service-naam**, namelijk 'Noordenwind' in de volgende schermopname.

    ![App service-naam](media\azure-stack-solution-hybrid-pipeline\106.png)

8. Voor de fase van de Agent, selecteert u **gehost VS2017** uit de **Fronta agenta** vervolgkeuzelijst.

    ![Gehoste agent](media\azure-stack-solution-hybrid-pipeline\107.png)

9. In **Azure App Service implementeren**, selecteert u de geldige **pakket of de map** voor de omgeving.

    ![Pakket of de map selecteren](media\azure-stack-solution-hybrid-pipeline\108.png)

10. In **Selecteer bestand of map**, selecteer **OK** naar **locatie**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\109.png)

11. Sla alle wijzigingen op en gaat u terug naar **pijplijn**.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\110.png)

12. Op de **pijplijn** tabblad **toevoegen artefact**, en kies de **NorthwindCloud Traders-vat** uit de **Source (Build Definition)** vervolgkeuzelijst.

    ![Nieuwe artefact toevoegen](media\azure-stack-solution-hybrid-pipeline\111.png)

13. Op **selecteert u een sjabloon**, toevoegen van een andere omgeving. Kies **Azure App Service-implementatie** en selecteer vervolgens **toepassen**.

    ![Sjabloon selecteren](media\azure-stack-solution-hybrid-pipeline\112.png)

14. Voer 'Azure Stack' als de **omgevingsnaam**.

    ![Naam van de omgeving](media\azure-stack-solution-hybrid-pipeline\113.png)

15. Op de **taken** tabblad, zoeken en selecteert u Azure Stack.

    ![Azure Stack-omgeving](media\azure-stack-solution-hybrid-pipeline\114.png)

16. Uit de **Azure-abonnement** vervolgkeuzelijst, selecteert u 'AzureStack Traders vat EP' voor het eindpunt van de Azure Stack.

    ![Alternatieve tekst](media\azure-stack-solution-hybrid-pipeline\115.png)

17. Voer de naam van Azure Stack-web-app als de **App service-naam**.

    ![App service-naam](media\azure-stack-solution-hybrid-pipeline\116.png)

18. Onder **selectie van Clientagent**, "AzureStack - bDouglas bestandsnaamgedeelte" Kies uit meer dan de **Fronta agenta** vervolgkeuzelijst.

    ![Agent kiezen](media\azure-stack-solution-hybrid-pipeline\117.png)

19. Voor **Azure App Service implementeren**, selecteert u de geldige **pakket of de map** voor de omgeving. Op **bestand of map selecteren**, selecteer **OK** voor de map **locatie**.

    ![Pakket- of map kiezen](media\azure-stack-solution-hybrid-pipeline\118.png)

    ![Locatie goedkeuren](media\azure-stack-solution-hybrid-pipeline\119.png)

20. Op de **variabele** tabblad, zoek de variabele met de naam **VSTS_ARM_REST_IGNORE_SSL_ERRORS**. Stel de variabele waarde op **waar**, en het bereik ingesteld op **Azure Stack**.

    ![Variabele configureren](media\azure-stack-solution-hybrid-pipeline\120.png)

21. Op de **pijplijn** tabblad de **trigger voor continue implementatie** pictogram voor het artefact NorthwindCloud Traders-Web- en stel de **trigger voor continue implementatie** naar **Ingeschakeld**.  Doe hetzelfde voor het artefact ' NorthwindCloud Traders-vat '.

    ![Trigger voor continue implementatie instellen](media\azure-stack-solution-hybrid-pipeline\121.png)

22. Voor de Azure Stack-omgeving, selecteert u de **vóór de implementatie voorwaarden** pictogram van de trigger ingesteld op **na de release**.

    ![Set voorwaarden voor de pre-trigger](media\azure-stack-solution-hybrid-pipeline\122.png)

23. Sla al uw wijzigingen op.

> [!Note]
> Sommige instellingen voor release taken kunnen automatisch gedefinieerd als [omgevingsvariabelen](https://docs.microsoft.com/azure/devops/pipelines/release/variables?view=vsts#custom-variables) tijdens het maken van een release-pijplijn met een sjabloon. Deze instellingen kunnen niet worden gewijzigd in de taakinstellingen van de. U kunt echter deze instellingen in de omgeving bovenliggende items bewerken.

## <a name="create-a-release"></a>Een releaserecord maken

Nu u de wijzigingen in de release-pijplijn hebt voltooid, is het tijd om de implementatie te starten. Om dit te doen, maakt u een versie van de release-pijplijn. Een release kan worden automatisch gemaakt. bijvoorbeeld, wordt de trigger voor continue implementatie instellen in de release-pijplijn. Dit betekent dat het wijzigen van de broncode van een nieuwe build wordt gestart en die een nieuwe versie. Echter, in deze sectie maakt u een nieuwe versie handmatig.

1. Op de **pijplijn** tabblad, open de **Release** vervolgkeuzelijst lijst en kies **maken release**.

    ![Een releaserecord maken](media\azure-stack-solution-hybrid-pipeline\200.png)

2. Voer een beschrijving voor de release, controleert u dat de juiste artefacten zijn geselecteerd en kies vervolgens **maken**. Na enkele ogenblikken wordt een banner weergegeven dat aangeeft dat de nieuwe versie is gemaakt en de versienaam van de wordt weergegeven als een koppeling. Kies de koppeling om te zien van de release-overzichtspagina.

    ![Banner voor het maken van release](media\azure-stack-solution-hybrid-pipeline\201.png)

3. De overzichtspagina voor de release voor meer informatie over de release weergeven In de volgende schermafbeelding voor 'Release-2', de **omgevingen** sectie bevat de **Implementatiestatus** voor de Azure als 'IN uitvoering' en de status voor Azure Stack 'geslaagd'. Wanneer de status van de implementatie voor de Azure-omgeving wordt gewijzigd naar 'Geslaagd', wordt een banner weergegeven dat aangeeft dat de release klaar zijn voor goedkeuring. Wanneer een implementatie is in behandeling of is mislukt, een blauwe **(i)** het informatiepictogram dat wordt weergegeven. Beweeg de muisaanwijzer over het pictogram om te zien van een pop-upvenster met de reden voor het vertragen of nalaten.

    ![Release-overzichtspagina](media\azure-stack-solution-hybrid-pipeline\202.png)

Andere weergaven, zoals de lijst worden vrijgegeven, wordt ook weergegeven voor een pictogram dat aangeeft goedkeuring in behandeling is. Het pop-upvenster voor dit pictogram ziet u de omgevingsnaam van de en meer details met betrekking tot de implementatie. Het is eenvoudig voor een beheerder de algemene voortgang van releases en zien aan welke versies op goedkeuring wachten.

### <a name="monitor-and-track-deployments"></a>Monitor en traceren implementaties

Deze sectie wordt beschreven hoe u kunt controleren en bijhouden van al uw implementaties. De release voor het implementeren van de twee websites van Azure App Services biedt een goed voorbeeld.

1. Selecteer op de pagina overzicht "Release-2" **logboeken**. Tijdens de implementatie van een toont deze pagina het live logboek van de agent. In het linkerdeelvenster ziet de status van elke bewerking in de implementatie voor elke omgeving.

    U kunt het pictogram van een persoon in de **actie** kolom voor het goedkeuren van een vóór de implementatie of na de implementatie Zie heeft goedgekeurd (of afgewezen) op de implementatie en het bericht die worden geleverd.

2. Nadat de implementatie is voltooid, wordt het hele bestand weergegeven in het rechter deelvenster. U kunt een selecteren **stap** in het linkerdeelvenster om te zien van het logboekbestand voor één stap, zoals "Job initialiseren". De mogelijkheid om te zien van afzonderlijke logboeken is het gemakkelijker te traceren en foutopsporing uitvoeren onderdelen van de algehele implementatie. U kunt ook **opslaan** het logboekbestand voor een stap of **alle logboeken downloaden als zip**.

    ![Release-Logboeken](media\azure-stack-solution-hybrid-pipeline\203.png)

3. Open de **samenvatting** tab om algemene informatie over de versie weergegeven. Deze weergave toont informatie over de build, deze is geïmplementeerd voor omgevingen, Implementatiestatus en andere informatie over de release.

4. Selecteer de koppeling van een omgeving (**Azure** of **Azure Stack**) voor informatie over bestaande en in afwachting zijnde implementaties met een specifieke omgeving. Deze weergaven kunt u een snelle manier om te controleren dat de dezelfde build in beide omgevingen is geïmplementeerd.

5. Open de **productie-app geïmplementeerd** in uw browser. Bijvoorbeeld, de URL voor de website van Azure App Services openen `http://[your-app-name].azurewebsites.net`.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over Azure Cloud-patronen, [Cloudontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).

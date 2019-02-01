---
title: Cross-cloud vergroten/verkleinen oplossingen maken met Azure | Microsoft Docs
description: Leer hoe u binnen de cloud vergroten/verkleinen om oplossingen te maken met Azure.
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
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: a8c4ef5df586c87862ea8e1634e9a72356401d0b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247422"
---
# <a name="tutorial-create-cross-cloud-scaling-solutions-with-azure"></a>Zelfstudie: Cross-cloud vergroten/verkleinen oplossingen maken met Azure

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Informatie over het maken van een cross-cloud-oplossing voor het bieden van een handmatig geactiveerde proces bij het overstappen van een Azure Stack web-app die wordt gehost, naar een Azure gehoste web-app met automatisch schalen via traffic manager, ervoor te zorgen dat flexibele en schaalbare cloud-hulpprogramma met de belasting.

Met dit patroon wordt uw tenant mogelijk niet gereed voor het uitvoeren van uw toepassing in de openbare cloud. Echter deze mogelijk niet geschikt is voor het bedrijf te houden van de capaciteit is vereist in hun on-premises-omgeving voor het afhandelen van pieken in de vraag naar de app. Uw tenant kan duren voordat de elasticiteit van de openbare cloud met hun on-premises oplossing gebruiken.

In deze zelfstudie bouwt u een Voorbeeldomgeving:

> [!div class="checklist"]
> - Maak een webtoepassing met meerdere knooppunten.
> - Configureren en beheren van het proces voor continue implementatie (CD).
> - De web-app publiceren naar Azure Stack.
> - Maak een release.
> - Informatie over het controleren en bijhouden van uw implementaties.

> [!Tip]  
> ![hybride-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack is een uitbreiding van Azure. Azure Stack brengt de flexibiliteit en innovatie van cloud computing naar uw on-premises omgeving en de enige hybride cloud waarmee u waar dan ook hybride apps bouwen en implementeren in te schakelen.  
> 
> Het technische document [ontwerpoverwegingen voor hybride toepassingen](https://aka.ms/hybrid-cloud-applications-pillars) beoordelingen pijlers van softwarekwaliteit (plaatsing, schaalbaarheid, beschikbaarheid, tolerantie, beheerbaarheid en beveiliging) voor het ontwerpen, implementeren en besturingssysteem hybride toepassingen. De overwegingen bij het ontwerpen helpen bij het optimaliseren van hybride toepassingsontwerp, uitdagingen in een productieomgeving te minimaliseren.

## <a name="prerequisites"></a>Vereisten

-   Azure-abonnement. Maak indien nodig een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

- Een geïntegreerde Azure Stack-systeem of de implementatie van Azure Stack Development Kit.
    - U vindt instructies voor het installeren van Azure Stack op [installeren van de Azure Stack Development Kit](../asdk/asdk-install.md).
    - [https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) Deze installatie is mogelijk een paar uur te voltooien.

-   Implementeer [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS-services met Azure Stack.

-   [Plan/aanbiedingen maken](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) binnen de Azure Stack-omgeving.

-   [Maken van tenantabonnement](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) binnen de Azure Stack-omgeving.

-   Een Web-App binnen de tenantabonnement maken. Noteer de URL van de nieuwe Web-App voor later gebruik.

-   Implementeer pijplijnen virtuele Azure-Machine binnen de tenantabonnement.

-   Windows Server 2016-VM met .NET 3.5 is vereist. Deze virtuele machine worden in de tenantabonnement op Azure Stack als de agent van de privéversie samengesteld.

-   [Windows Server 2016 met VM-installatiekopie voor SQL-2017](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-vm-image#add-a-vm-image-through-the-portal) is beschikbaar in de Azure Stack Marketplace. Als deze afbeelding niet is beschikbaar is, werken met een Azure Stack-Operator om te controleren of dat deze wordt toegevoegd aan de omgeving.

## <a name="issues-and-considerations"></a>Problemen en overwegingen

### <a name="scalability-considerations"></a>Schaalbaarheidsoverwegingen

Het belangrijkste onderdeel van cross-cloud schaal is de mogelijkheid om te leveren direct, op aanvraag schalen tussen openbare en on-premises cloudinfrastructuur, consistente, betrouwbare service aan te tonen zoals voorgeschreven door de vraag.

### <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Controleer of lokaal geïmplementeerde apps zijn geconfigureerd voor hoge beschikbaarheid via on-premises hardware-configuratie en software-implementatie.

### <a name="manageability-considerations"></a>Beheerbaarheidsoverwegingen

De cross-cloud-oplossing zorgt voor naadloos beheer en vertrouwde-interface tussen omgevingen. PowerShell wordt aanbevolen voor het beheer van meerdere platforms.

## <a name="cross-cloud-scaling"></a>Cross-cloud schalen

### <a name="obtain-a-custom-domain-and-configure-dns"></a>Verkrijgen van een aangepast domein en DNS configureren

Werk de DNS-zonebestand voor het domein. Azure AD wordt eigenaar van de aangepaste domeinnaam niet verifiëren. Gebruik [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) Azure/Office 365/externe DNS-records in Azure, of Voeg de DNS-vermelding op [een andere DNS-registratieservice](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

1.  Een aangepast domein met een openbare Registrar registreren.

2.  Meld u aan bij de domeinnaamregistrar voor het domein. Een goedgekeurde beheerder kan vereist dat u DNS-updates. 

3.  De DNS-zonebestand voor het domein bijwerken door de DNS-vermelding die is geleverd door Azure AD toe te voegen. (De DNS-vermelding heeft geen invloed op mailroutering of webhosting gedrag.) 

### <a name="create-a-default-multi-node-web-app-in-azure-stack"></a>Een standaard meerdere knooppunten web-app maken in Azure Stack

Hybride continue integratie en continue implementatie (CI/CD) instellen voor Web-App implementeren in Azure en Azure Stack en breng wijzigingen over naar beide clouds automatisch.

> [!Note]  
> Azure Stack met de juiste installatiekopieën publiceren naar uitvoeren (Windows Server en SQL) en App Service-implementatie zijn vereist. Raadpleeg de documentatie van App Service "[voordat u aan de slag met App Service in Azure Stack](../azure-stack-app-service-before-you-get-started.md)" sectie voor Azure Stack-operators.

### <a name="add-code-to-azure-repos"></a>Voeg Code toe aan Azure-opslagplaatsen

Azure Repos

1. Meld u aan Azure-opslagplaatsen met een account met rechten voor het project maken op Azure-opslagplaatsen.

    Hybride CI/CD kunt toepassen op zowel de toepassingscode als de infrastructuurcode. Gebruik [Azure Resource Manager-sjablonen](https://azure.microsoft.com/resources/templates/) voor beide particuliere en gehoste cloudontwikkeling.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image1.JPG)

2. **Kloon de opslagplaats** door het maken en de standaard web-app te openen.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image2.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Implementatie van zelfstandige web-app voor App Services in beide clouds maken

1.  Bewerk de **WebApplication.csproj** bestand. Selecteer **Runtimeidentifier** en toe te voegen **win10 x64**. (Zie [Self-contained implementatie](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) documentatie.) 

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image3.png)

2.  Controleer in de code voor het Azure-opslagplaatsen met Team Explorer.

3.  Bevestig dat de code van de toepassing is gecontroleerd in Azure-opslagplaatsen.

## <a name="create-the-build-definition"></a>De build-definitie maken

1. Meld u aan bij Azure pijplijnen om te bevestigen van de mogelijkheid om te maken, bouwen definities.

2. Voeg **win10 - r-x64** code. Dit is nodig voor het activeren van een onafhankelijke implementatie met .net Core.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image4.png)

3. De build worden uitgevoerd. De [onafhankelijke implementatie build](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) proces artefacten die kunnen worden uitgevoerd op Azure en Azure Stack zal publiceren.

## <a name="use-an-azure-hosted-agent"></a>Gebruik een Azure gehost agent

Gebruik een gehoste-agent in pijplijnen van Azure is een handige optie voor het bouwen en implementeren van web-apps. Automatisch worden onderhoud en upgrades uitgevoerd door Microsoft Azure, waardoor continu en ononderbroken ontwikkelen, testen en implementeren.

### <a name="manage-and-configure-the-cd-process"></a>Beheren en configureren van het CD-proces

Azure pijplijnen en Azure DevOps-Server bieden een pijplijn maximaal kunnen worden geconfigureerd en beheerd voor releases tot meerdere omgevingen zoals ontwikkeling, fasering, QA- en productieomgevingen; waaronder het verplichte gebruik goedkeuringen op specifieke tijdstippen.

## <a name="create-release-definition"></a>Release-definitie maken

![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image5.png)

1.  Selecteer de **plus** om toe te voegen een nieuwe versie onder de **Releases tabblad** op de pagina bouwen en lanceren van VSO.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image6.png)

2. De implementatie van Azure App Service-sjabloon toepassen.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image7.png)

3. Toevoegen onder artefact toevoegen, de artefact voor de Azure-Cloud-app bouwen.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image8.png)

4. Selecteer onder pijplijntabblad, de **fase, taak** koppelen van de omgeving en de Azure-cloud Omgevingswaarden instellen.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image9.png)

5. Stel de **omgevingsnaam** en selecteer Azure **abonnement** voor het Azure-Cloud-eindpunt.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image10.png)

6. Onder de naam van de omgeving, stelt u de vereiste **Azure app service-naam**.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image11.png)

7. Voer **VS2017 gehost** onder wachtrij van de Agent voor Azure in de cloud gehoste omgeving.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image12.png)

8. Selecteer in Azure App Service implementeren in het menu geldig **pakket of de map** voor de omgeving. Selecteer **OK** naar **maplocatie**.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image13.png)

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image14.png)

9. Sla alle wijzigingen op en gaat u terug naar **release-pijplijn**.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image15.png)

10. Voeg een nieuwe artefact selecteren van de build voor de Azure Stack-app.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image16.png)

11. Voeg een meer omgeving toepassen van de Azure App Service-implementatie.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image17.png)

12. De naam van de nieuwe Azure Stack-omgeving.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image18.png)

13. Zoek de Azure Stack-omgeving onder **taak** tabblad.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image19.png)

14. Selecteer het abonnement voor het eindpunt van de Azure Stack.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image20.png)

15. De naam van de Azure Stack web-app instellen als de naam van de App-service.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image21.png)

16. Selecteer het Azure Stack-agent.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image22.png)

17. Selecteer sectie onder de implementatie van Azure App Service de geldige **pakket of de map** voor de omgeving. Selecteer **OK** naar maplocatie.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image23.png)

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image24.png)

18. Variabele tabblad toevoegen een variabele met de naam `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS`, stel de waarde als **waar**, en bereik op Azure Stack.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image25.png)

19. Selecteer de **doorlopend** pictogram van de implementatie-trigger in zowel artefacten en schakelt u de **gaat door met het** trigger voor implementatie.

    ![Alternatieve tekst](media/azure-stack-solution-cloud-burst/image26.png)

20. Selecteer de **vóór de implementatie** voorwaarden-pictogram in de Azure Stack-omgeving en stelt u de trigger op **na de release.**

21. Sla alle wijzigingen op.

> [!Note]  
> Sommige instellingen voor de taken kunnen automatisch gedefinieerd als [omgevingsvariabelen](https://docs.microsoft.com/azure/devops/pipelines/release/variables?view=vsts&tabs=batch#custom-variables) bij het maken van een release-definitie van een sjabloon. Deze instellingen kunnen niet worden gewijzigd in de taakinstellingen. in plaats daarvan moet het hoofdartikel voor de omgeving worden geselecteerd om deze instellingen te bewerken

## <a name="publish-to-azure-stack-via-visual-studio"></a>Publiceren naar Azure Stack via Visual Studio

Met het maken van eindpunten, implementeren een build van het Visual Studio Online (VSTO) Azure Service-apps met Azure Stack. Azure pijplijnen verbindt met de build-agent, die verbinding maakt met Azure Stack.

1.  Aanmelden bij VSTO en navigeer naar de pagina app-instellingen.

2.  Op **instellingen**, selecteer **Security**.

3.  In **VSTS groepen**, selecteer **makers van eindpunt**.

4.  Op de **leden** tabblad **toevoegen**.

5.  In **gebruikers en groepen toevoegen**, voer een gebruikersnaam en selecteert u die gebruiker in de lijst van gebruikers.

6.  Selecteer **Save changes**.

7.  In de **VSTS groepen** in de lijst met **eindpunt beheerders**.

8.  Op de **leden** tabblad **toevoegen**.

9.  In **gebruikers en groepen toevoegen**, voer een gebruikersnaam en selecteert u die gebruiker in de lijst van gebruikers.

10. Selecteer **Save changes**.

Nu informatie over het eindpunt bestaat, de Azure-pijplijnen op Azure Stack-verbinding is klaar voor gebruik. De build-agent in Azure Stack haalt de instructies van Azure-pijplijnen en vervolgens de agent voor de communicatie met Azure Stack-eindpuntgegevens overbrengen.

## <a name="develop-the-application-build"></a>De build toepassing ontwikkelen

> [!Note]  
> Azure Stack met de juiste installatiekopieën publiceren naar uitvoeren (Windows Server en SQL) en App Service-implementatie zijn vereist. Raadpleeg de documentatie van App Service "[voordat u aan de slag met App Service in Azure Stack](../azure-stack-app-service-before-you-get-started.md)" sectie voor Azure Stack-operators.

Gebruik [Azure Resource Manager-sjablonen, zoals web](https://azure.microsoft.com/resources/templates/) app-code uit Azure-opslagplaatsen om te implementeren voor beide clouds.

### <a name="add-code-to-a-azure-repos-project"></a>Voeg code toe aan een Azure-opslagplaatsen-project

1.  Meld u aan Azure-opslagplaatsen met een account met rechten voor het project maken in Azure Stack. De volgende schermopname ziet hoe u verbinding maken met de HybridCICD-project.

2.  **Kloon de opslagplaats** door het maken en de standaard web-app te openen.

#### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Implementatie van zelfstandige web-app voor App Services in beide clouds maken

1.  Bewerk de **WebApplication.csproj** bestand: Selecteer **Runtimeidentifier** en voeg deze win10 x64. Zie voor meer informatie, [onafhankelijke implementatie](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) documentatie.

2.  Team Explorer gebruiken om te controleren of de code in Azure-opslagplaatsen.

3.  Bevestig dat de code van de toepassing is ingeschakeld in de Azure-opslagplaatsen.

### <a name="create-the-build-definition"></a>De build-definitie maken

1.  Meld u aan Azure-pijplijnen met een account dat een build-definitie kunt maken.

2.  Navigeer naar de **webtoepassing bouwen** pagina voor het project.

3.  In **argumenten**, toevoegen **win10 - r-x64** code. Dit is vereist voor het activeren van een onafhankelijke implementatie met .net Core.

4.  De build worden uitgevoerd. De [onafhankelijke implementatie build](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) proces artefacten die kunnen worden uitgevoerd op Azure en Azure Stack zal publiceren.

#### <a name="use-an-azure-hosted-build-agent"></a>Gebruik een Azure gehost bouwagent

Met behulp van een gehoste bouwagent in pijplijnen van Azure is een handige optie voor het bouwen en implementeren van web-apps. Automatisch worden onderhouden van de agent en upgrades uitgevoerd door Microsoft Azure, waarmee een doorlopende en ononderbroken ontwikkelingscyclus.

### <a name="configure-the-continuous-deployment-cd-process"></a>Het continue implementatie (CD) configureren

Pijplijnen en Azure DevOps-Server van Azure bieden een pijplijn maximaal kunnen worden geconfigureerd en beheerd voor releases tot meerdere omgevingen zoals ontwikkeling, fasering, kwaliteit te waarborgen (QA) en productie. Dit proces kunt opnemen goedkeuringen vereisen specifieke stadia van de levenscyclus van toepassingen.

#### <a name="create-release-definition"></a>Release-definitie maken

Het maken van een release-definitie is het bouwproces voor de laatste stap in de toepassing. Deze release-definitie wordt gebruikt om een releaserecord maken en implementeren van een build.

1.  Meld u aan Azure-pijplijnen en navigeer naar **Build and Release** voor het project.

2.  Op de **Releases** tabblad **[+]** en kies dan **release-definitie maken**.

3.  Op **selecteert u een sjabloon**, kiest u **Azure App Service-implementatie**, en selecteer vervolgens **toepassen**.

4.  Op **toevoegen artefact**, uit de **Source (Build definition)** selecteert u de Azure-Cloud-app bouwen.

5.  Op de **pijplijn** tabblad de **1 fase**, **1 taak** koppelen aan **omgevingstaken weergeven**.

6.  Op de **taken** tabblad, voert Azure als de **omgevingsnaam** en selecteer de AzureCloud Traders Web EP uit de **Azure-abonnement** lijst.

7.  Voer de **Azure app service-naam**, die is `northwindtraders` in de volgende schermopname.

8.  Selecteer voor de fase Agent **VS2017 die worden gehost** van de **Fronta agenta** lijst.

9.  In **Azure App Service implementeren**, selecteert u de geldige **pakket of de map** voor de omgeving.

10. In **Selecteer bestand of map**, selecteer **OK** naar **locatie**.

11. Sla alle wijzigingen op en gaat u terug naar **pijplijn**.

12. Op de **pijplijn** tabblad **toevoegen artefact**, en kies de **NorthwindCloud Traders-vat** uit de **Source (Build Definition)** lijst.

13. Op **selecteert u een sjabloon**, toevoegen van een andere omgeving. Kies **Azure App Service-implementatie** en selecteer vervolgens **toepassen**.

14. Voer `Azure Stack` als de **omgevingsnaam**.

15. Op de **taken** tabblad, zoeken en selecteert u Azure Stack.

16. Uit de **Azure-abonnement** in de lijst met **AzureStack Traders vat EP** voor het eindpunt van de Azure Stack.

17. Voer de naam van Azure Stack-web-app als de **App service-naam**.

18. Onder **selectie van Clientagent**, kies **AzureStack -b Douglas bestandsnaamgedeelte** uit de **Fronta agenta** lijst.

19. Voor **Azure App Service implementeren**, selecteert u de geldige **pakket of de map** voor de omgeving. Op **bestand of map selecteren**, selecteer **OK** voor de map **locatie**.

20. Op de **variabele** tabblad, zoek de variabele met de naam `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS`. Stel de variabele waarde op **waar**, en het bereik ingesteld op **Azure Stack**.

21. Op de **pijplijn** tabblad de **trigger voor continue implementatie** pictogram voor het artefact NorthwindCloud Traders-Web- en stel de **trigger voor continue implementatie** naar **Ingeschakeld**. Doe hetzelfde voor de **NorthwindCloud Traders-vat** artefact.

22. Voor de Azure Stack-omgeving, selecteert u de **vóór de implementatie voorwaarden** pictogram van de trigger ingesteld op **na de release**.

23. Sla alle wijzigingen op.

> [!Note]  
> Sommige instellingen voor release taken automatisch worden gedefinieerd als [omgevingsvariabelen](https://docs.microsoft.com/azure/devops/pipelines/release/variables?view=vsts&tabs=batch#custom-variables) bij het maken van een release-definitie van een sjabloon. Deze instellingen in de taakinstellingen van de kunnen niet worden gewijzigd, maar kunnen worden gewijzigd in de omgeving bovenliggende items.

## <a name="create-a-release"></a>Een releaserecord maken

1.  Op de **pijplijn** tabblad, open de **Release** lijst en kies **maken release**.

2.  Voer een beschrijving voor de release, controleert u dat de juiste artefacten zijn geselecteerd en kies vervolgens **maken**. Na enkele ogenblikken wordt een banner weergegeven dat aangeeft dat de nieuwe versie is gemaakt en de versienaam van de wordt weergegeven als een koppeling. Kies de koppeling om te zien van de release-overzichtspagina.

3.  De overzichtspagina voor de release voor meer informatie over de release weergeven In de volgende schermafbeelding voor 'Release-2', de **omgevingen** sectie bevat de **Implementatiestatus** voor de Azure als 'IN uitvoering' en de status voor Azure Stack 'geslaagd'. Wanneer de status van de implementatie voor de Azure-omgeving wordt gewijzigd naar 'Geslaagd', wordt een banner weergegeven dat aangeeft dat de release klaar zijn voor goedkeuring. Wanneer een implementatie is in behandeling of is mislukt, een blauwe **(i)** het informatiepictogram dat wordt weergegeven. Beweeg de muisaanwijzer over het pictogram om te zien van een pop-upvenster met de reden voor het vertragen of nalaten.

4.  Andere weergaven, zoals de lijst met versies van het ook weer een pictogram dat goedkeuring aangeeft in behandeling is. Het pop-upvenster voor dit pictogram ziet u de omgevingsnaam van de en meer details met betrekking tot de implementatie. Het is eenvoudig voor een beheerder de algemene voortgang van releases en zien aan welke versies op goedkeuring wachten.

## <a name="monitor-and-track-deployments"></a>Monitor en traceren implementaties

1.  Op de **Release 2** samenvatting weergeeft, schakelt **logboeken**. Tijdens de implementatie van een toont deze pagina het live logboek van de agent. In het linkerdeelvenster ziet de status van elke bewerking in de implementatie voor elke omgeving.

2.  Kies het pictogram van een persoon in de **actie** kolom voor het goedkeuren van een vóór de implementatie of na de implementatie Zie heeft goedgekeurd (of afgewezen) op de implementatie en het bericht die worden geleverd.

3.  Nadat de implementatie is voltooid, wordt het hele bestand weergegeven in het rechter deelvenster. Selecteer een **stap** in het linkerdeelvenster om te zien van het logboekbestand voor één stap, zoals **taak initialiseren**. De mogelijkheid om te zien van afzonderlijke logboeken is het gemakkelijker te traceren en foutopsporing uitvoeren onderdelen van de algehele implementatie. **Sla** het logboekbestand voor een stap of **alle logboeken downloaden als zip**.

4.  Open de **samenvatting** tab om algemene informatie over de versie weergegeven. Deze weergave toont informatie over de build, deze is geïmplementeerd voor omgevingen, Implementatiestatus en andere informatie over de release.

5.  Selecteer de koppeling van een omgeving (**Azure** of **Azure Stack**) voor informatie over bestaande en in afwachting zijnde implementaties met een specifieke omgeving. Deze weergaven worden gebruikt als een snelle manier om te verifiëren dat de dezelfde build in beide omgevingen is geïmplementeerd.

6.  Open de **productie-app geïmplementeerd** in de browser. Bijvoorbeeld, de URL voor de website van Azure App Services openen [http://[uw app-naam\]. azurewebsites.net](http:// [your-app-name].azurewebsites.net).

**Integratie van Azure en Azure Stack biedt een schaalbare oplossing voor cross-cloud**

Een flexibele en krachtige multi-cloudservice biedt gegevensbeveiliging, terug omhoog en redundantie, beschikbaarheid voor consistente en snelle, schaalbare opslag en distributie en geo-compatibele routering. Dit proces handmatig geactiveerde zorgt ervoor dat betrouwbaar en efficiënt load schakelen tussen gehoste Web-apps, ervoor te zorgen dat de directe beschikbaarheid van essentiële gegevens. 

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over Azure Cloud-patronen, [Cloudontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).

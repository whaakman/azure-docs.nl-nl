---
title: Een geografisch gedistribueerde app-oplossing maken met Azure en Azure Stack | Microsoft Docs
description: Informatie over het maken van een geografisch gedistribueerde app-oplossing met Azure en Azure Stack.
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
ms.date: 09/24/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 19e7506dac82e4d12d5aecbdb5ae1c14fb944c29
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961532"
---
# <a name="tutorial-create-a-geo-distributed-app-solution-with-azure-and-azure-stack"></a>Zelfstudie: Een geografisch gedistribueerde app-oplossing maken met Azure en Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Leer hoe u verkeer naar specifieke eindpunten op basis van verschillende metrische gegevens met behulp van het patroon geografisch gedistribueerde apps. Het maken van een Traffic Manager-profiel met geografische gebaseerde routering en het eindpunt configuratie zorgt ervoor dat informatie wordt doorgestuurd naar eindpunten op basis van regionale vereisten, zakelijke en internationale regelgeving en uw behoeften voor gegevens.

In deze zelfstudie bouwt u een Voorbeeldomgeving:

> [!div class="checklist"]
> - Maak een geografisch gedistribueerde App.
> - Gebruik Traffic Manager om uw app.

## <a name="use-the-geo-distributed-apps-pattern"></a>Het patroon geografisch gedistribueerde apps gebruiken

Met het patroon geografisch gedistribueerde, kan uw app in regio's omvat. U kunt de standaardwaarde voor de openbare cloud, maar sommige gebruikers mogelijk vereist dat hun gegevens in de regio blijven. U kunt instellen dat gebruikers naar de meest geschikte cloud op basis van hun behoeften.

### <a name="issues-and-considerations"></a>Problemen en overwegingen

#### <a name="scalability-considerations"></a>Schaalbaarheidsoverwegingen

De oplossing u met deze zelfstudie bouwt is niet geschikt voor schaalbaarheid. Echter als gebruikt in combinatie met andere Azure en On-Premises technologieën en oplossingen kunt u gebruikmaken van de vereisten voor schaalbaarheid. Zie voor meer informatie over het maken van een oplossing hyrbid met automatisch schalen via traffic manager [vergroten/verkleinen cross-cloudoplossingen te maken met Azure](azure-stack-solution-cloud-burst.md).

#### <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Net als met aandachtspunten voor schaalbaarheid, verband geen beschikbaarheid rechtstreeks met deze oplossing. Echter, ook net als bij onze schaalbaarheidsoverwegingen, Azure, en on-premises technologieën en oplossingen kunnen worden geïmplementeerd in deze oplossing om te hoge beschikbaarheid voor alle onderdelen die betrokken zijn.

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken

- Uw organisatie heeft internationale vertakkingen waarvoor aangepaste regionale beveiliging en beleid voor distributie.

- Elk kantoor organisaties haalt medewerker-, bedrijfs- en faciliteit gegevens, waarvoor is vereist voor het melden van activiteit per lokale voorschriften en dezelfde tijdzone.

- Grote schaalvereisten kan worden voldaan door apps, horizontaal uitschalen met meerdere app-implementaties binnen één regio, evenals in regio's worden ingediend bij het verwerken van de vereisten voor het extreme laden.

### <a name="planning-the-topology"></a>Planning van de topologie

Voordat het opzetten van een distributed app footprint kunt zo u het volgende beschikken:

-   **Het aangepaste domein voor de app:** wat is de naam van het aangepaste domein dat klanten toegang tot de app wordt gebruikt? Voor de voorbeeld-app de aangepaste domeinnaam is *www.scalableasedemo.com.*

-   **Traffic Manager-domein:** een domeinnaam moet worden gekozen bij het maken van een [Azure Traffic Manager-profiel](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles). Deze naam wordt gecombineerd met de *trafficmanager.net* achtervoegsel voor het registreren van een domein-item dat wordt beheerd door Traffic Manager. Voor de voorbeeld-app, is het de naam van de gekozen *schaalbare-as-omgeving-demo*. Als gevolg hiervan de volledige domeinnaam die wordt beheerd door Traffic Manager is *schaalbare-as-omgeving-demo.trafficmanager.net*.

-   **Strategie voor het schalen van de app-voetafdruk:** wordt de voetafdruk van de toepassing worden verdeeld over meerdere App Service-omgevingen in één regio? Meerdere regio's? Een combinatie van beide methoden? De beslissing moet worden gebaseerd op de verwachtingen van waaruit klantenverkeer wordt uitgevoerd, evenals hoe goed de rest van de back-endinfrastructuur voor ondersteuning van van een app kunt schalen. Bijvoorbeeld, met een 100% staatloze toepassingen, kan een app worden zeer geschaald met behulp van een combinatie van meerdere App Service-omgevingen per Azure-regio, vermenigvuldigd met App Service-omgevingen in meerdere Azure-regio's. Klanten met 15 + globale Azure-regio's beschikbaar om de verkeersbelasting, kunnen echt een footprint wereldwijd grootschalige toepassingen bouwen. Voor de voorbeeldapp die wordt gebruikt voor dit artikel, zijn drie App Service-omgevingen gemaakt in één Azure-regio (Zuid-centraal VS).

-   **Naamgevingsregels voor de App Service-omgevingen:** voor elke App Service-omgeving moet een unieke naam. Meer dan één of twee App Service-omgevingen is het handig om een naamconventie gebruikt voor het identificeren van elke App Service-omgeving. Een eenvoudige naamconventie is voor de voorbeeld-app gebruikt. De namen van de drie App Service-omgevingen zijn *fe1ase*, *fe2ase*, en *fe3ase*.

-   **Naamgevingsregels voor de apps:** omdat meerdere exemplaren van de app worden geïmplementeerd, een naam is vereist voor elk exemplaar van de geïmplementeerde app. Met App Service-omgevingen kan dezelfde app-naam worden gebruikt voor meerdere App Service-omgevingen. Omdat elke App Service-omgeving een unieke domeinachtervoegsel heeft, kunt ontwikkelaars exact dezelfde naam van de app in elke omgeving opnieuw gebruiken. Een ontwikkelaar kan bijvoorbeeld apps met de naam als volgt: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*, enzovoort. Voor de app in dit scenario heeft elke app-exemplaar een unieke naam. De namen van de app-exemplaar gebruikt *webfrontend1*, *webfrontend2*, en *webfrontend3*.

> [!Tip]  
> ![hybride-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack is een uitbreiding van Azure. Azure Stack brengt de flexibiliteit en innovatie van cloud computing naar uw on-premises omgeving en de enige hybride cloud waarmee u waar dan ook hybride apps bouwen en implementeren in te schakelen.  
> 
> Het technische document [ontwerpoverwegingen voor hybride toepassingen](https://aka.ms/hybrid-cloud-applications-pillars) beoordelingen pijlers van softwarekwaliteit (plaatsing, schaalbaarheid, beschikbaarheid, tolerantie, beheerbaarheid en beveiliging) voor het ontwerpen, implementeren en besturingssysteem hybride toepassingen. De overwegingen bij het ontwerpen helpen bij het optimaliseren van hybride toepassingsontwerp, uitdagingen in een productieomgeving te minimaliseren.

## <a name="part-1-create-a-geo-distributed-app"></a>Deel 1: Een geografisch gedistribueerde app maken

In dit gedeelte maakt u een web-app.

> [!div class="checklist"]
> - Web-apps maken en publiceren
> - Voeg Code toe aan VSTS-Project
> - De app-build verwijzen naar meerdere doelen van de cloud.
> - Beheren en configureren van het CD-proces

### <a name="prerequisites"></a>Vereisten

Een Azure-abonnement en Azure Stack-installatie zijn vereist.

### <a name="geo-distributed-app-steps"></a>Geografisch gedistribueerde app stappen

### <a name="obtain-a-custom-domain-and-configure-dns"></a>Verkrijgen van een aangepast domein en DNS configureren

Werk de DNS-zone bestand foCreate web-apps en publishr het domein. Azure AD kan vervolgens eigendom van de aangepaste domeinnaam te verifiëren. Gebruik [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) Azure/Office 365/externe DNS-records in Azure, of Voeg de DNS-vermelding op [een andere DNS-registratieservice](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

1. Een aangepast domein met een openbare Registrar registreren.

2. Meld u aan bij de domeinnaamregistrar voor het domein. Een goedgekeurde beheerder kan vereist dat u de DNS-updates.

3. De DNS-zonebestand voor het domein bijwerken door de DNS-vermelding die is geleverd door Azure AD toe te voegen. De DNS-vermelding verandert het gedrag van bijvoorbeeld mailroutering of webhosting niet.

### <a name="create-web-apps-and-publish"></a>Web-apps maken en publiceren

Hybride CI/CD naar Web-App implementeren in Azure en Azure Stack en breng wijzigingen over naar beide clouds automatisch instellen.

> [!Note]  
> Azure Stack met de juiste installatiekopieën publiceren naar uitvoeren (Windows Server en SQL) en App Service-implementatie zijn vereist. Raadpleeg de documentatie van App Service "[voordat u aan de slag met App Service in Azure Stack](/articles/azure-stack/azure-stack-app-service-before-you-get-started)" sectie voor Azure Stack-operators.

#### <a name="add-code-to-vsts-project"></a>Voeg Code toe aan VSTS-Project

1. Aanmelden bij Visual Studio met een **account waaraan rechten voor het maken van project** op VSTS.

    Hybride continue integratie/continue levering (CI/CD) kunt toepassen op zowel de toepassingscode als de infrastructuurcode. Gebruik [Azure Resource Manager-sjablonen](https://azure.microsoft.com/resources/templates/) voor beide particuliere en gehoste cloudontwikkeling.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image1.JPG)

2. **Kloon de opslagplaats** door het maken en de standaard web-app te openen.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image2.png)

### <a name="create-web-app-deployment-in-both-clouds"></a>Implementatie van web-app maken in beide clouds

1.  Bewerk de **WebApplication.csproj** bestand: Selecteer **Runtimeidentifier** en toe te voegen **win10 x64**. (Zie [Self-contained implementatie](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) documentatie.)

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image3.png)

1.  **Controleer in de code voor het VSTS** met Team Explorer.

2.  Bevestig dat de **toepassingscode** in Visual Studio Team Services is gecontroleerd.

### <a name="create-the-build-definition"></a>De build-definitie maken

1. **Meld u aan bij VSTS** om te bevestigen builddefinities kan maken.

2. Voeg **win10 - r-x64** code. Dit is nodig voor het activeren van een onafhankelijke implementatie met .net Core.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image4.png)

3. **Uitvoeren van de build**. De [onafhankelijke implementatie build](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) proces artefacten die kunnen worden uitgevoerd op Azure en Azure Stack zal publiceren.

**Met behulp van een Azure gehoste Agent**

Met behulp van een gehoste agent in VSTS is een handige optie om te bouwen en implementeren van web-apps. Automatisch worden onderhoud en upgrades uitgevoerd door Microsoft Azure, waardoor continu en ononderbroken ontwikkelen, testen en implementeren.

### <a name="manage-and-configure-the-cd-process"></a>Beheren en configureren van het CD-proces

Visual Studio Team Services (VSTS) en Team Foundation Server (TFS) bieden een pijplijn maximaal kunnen worden geconfigureerd en beheerd voor releases tot meerdere omgevingen zoals ontwikkeling, fasering, QA- en productieomgevingen; waaronder het verplichte gebruik goedkeuringen op specifieke tijdstippen.

#### <a name="create-release-definition"></a>Release-definitie maken


![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image5.png)

1.  Selecteer de **plus** om toe te voegen een nieuwe versie onder de **Releases tabblad** op de pagina Build en versie van Visual Studio Online (VSO).

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image6.png)

2. Van toepassing de **Azure App Service-implementatie** sjabloon.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image7.png)

3. Onder toevoegen artefact vervolgkeuzelijst, **toevoegen van het artefact** voor de Azure-Cloud-app bouwen.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image8.png)

4. Selecteer onder pijplijntabblad, de **fase, taak** koppelen van de omgeving en de Azure-cloud Omgevingswaarden instellen.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image9.png)

5. Stel de **omgevingsnaam** en selecteer Azure **abonnement** voor het Azure-Cloud-eindpunt.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image10.png)

6. Onder de naam van de omgeving, stelt u de vereiste **Azure app service-naam**.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image11.png)

7. Voer **VS2017 gehost** onder wachtrij van de Agent voor Azure in de cloud gehoste omgeving.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image12.png)

8. Selecteer in Azure App Service implementeren in het menu geldig **pakket of de map** voor de omgeving. Klik op OK om te **maplocatie**.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image13.png)

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image14.png)

9. Sla alle wijzigingen op en gaat u terug naar **release-pijplijn**.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image15.png)

10. Voeg een **nieuwe artefact** selecteren van de build voor de Azure Stack-app.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image16.png)

11. Toevoegen van een meer omgeving toepassen van de **Azure App Service-implementatie.**

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image17.png)

12. Naam van de nieuwe omgeving **Azure Stack.**

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image18.png)

13. Zoek de Azure Stack-omgeving onder **taak** tabblad.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image19.png)

14. Selecteer de **abonnement** voor het eindpunt van de Azure Stack.

  ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image20.png)

15. Stel de naam van Azure Stack-web-app als de **App service-naam**.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image21.png)

16. Selecteer de **Azure Stack-agent**.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image22.png)

17. Selecteer sectie onder de implementatie van Azure App Service de geldige **pakket of de map** voor de omgeving. Klik op OK om te **maplocatie**.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image23.png)

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image24.png)

18. Onder **variabele** tabblad toevoegen in een variabele met de naam `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS`, stel de waarde als `true`, en het bereik instellen op `Azure Stack`.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image25.png)

19. Selecteer de **doorlopend** pictogram van de implementatie-trigger in zowel artefacten en schakelt u de **gaat door met het** trigger voor implementatie.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image26.png)

20. Selecteer de **vóór de implementatie** voorwaarden-pictogram in de Azure Stack-omgeving en stelt u de trigger op **na de release.**

21. Sla alle wijzigingen op.

> [!Note]  
>  Sommige instellingen voor de taken kunnen automatisch gedefinieerd als [omgevingsvariabelen](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) tijdens het maken van een release-definitie van een sjabloon. Deze instellingen kunnen niet worden gewijzigd in de taakinstellingen. in plaats daarvan moet u het hoofdartikel voor de omgeving om deze instellingen te bewerken.

## <a name="part-2-update-web-app-options"></a>Deel 2: Opties voor web-app het bijwerken

[Azure Web Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. 

![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image27.png)

> [!div class="checklist"]
> - Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps
> - Gebruik een ** CNAME recorder een **een record** naar een aangepaste DNS-naam toewijzen aan App Service.

### <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a>Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps

> [!Note]  
>  Gebruik een CNAME voor alle aangepaste DNS-namen, behalve een hoofddomein (voor example,northwind.com).

Zie voor het migreren van een live site en de DNS-domeinnaam naar App Service, [Een actieve DNS-naam migreren naar Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-custom-domain-name-migrate).

### <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

-   [Een App Service-app maken](https://docs.microsoft.com/azure/app-service/), of gebruik een app voor een andere zelfstudie hebt gemaakt.

-   Een domeinnaam aanschaffen en zorg ervoor dat de toegang tot de DNS-register voor de domeinprovider.

Werk de DNS-zonebestand voor het domein. Azure AD wordt eigenaar van de aangepaste domeinnaam niet verifiëren. Gebruik [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) Azure/Office 365/externe DNS-records in Azure, of Voeg de DNS-vermelding op [een andere DNS-registratieservice](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

-   Een aangepast domein met een openbare Registrar registreren.

-   Meld u aan bij de domeinnaamregistrar voor het domein. (Een goedgekeurde beheerder mogelijk moet maken van DNS-updates.)

-   De DNS-zonebestand voor het domein bijwerken door de DNS-vermelding die is geleverd door Azure AD toe te voegen.

Bijvoorbeeld, om toe te voegen DNS-vermeldingen fornorthwindcloud.comand www.northwindcloud.com, DNS-instellingen voor het hoofddomein thenorthwindcloud.com configureren.

> [!Note]  
>  De naam van een domein kan worden gekocht met de [Azure-portal](https://docs.microsoft.com/azure/app-service/custom-dns-web-site-buydomains-web-app).  
> Om een aangepaste DNS-naam toe te wijzen aan een web-app, moet het [App Service-plan](https://azure.microsoft.com/pricing/details/app-service/) van de web-app een betaalde categorie zijn (**Shared**, **Basic**, **Standard** of  **Premium**).



### <a name="create-and-map-cname-and-a-records"></a>Maken en toewijzen van CNAME- en A-records

#### <a name="access-dns-records-with-domain-provider"></a>Toegang tot DNS-records via domeinprovider

> [!Note]  
>  Azure DNS gebruiken voor het configureren van een aangepaste DNS-naam voor Azure Web Apps. Zie [Use Azure DNS to provide custom domain settings for an Azure service](https://docs.microsoft.com/azure/dns/dns-custom-domain) (Azure DNS gebruiken om aangepaste domeininstellingen te verstrekken voor een Azure-service) voor meer informatie.

1.  Meld u aan bij de website van de belangrijkste provider.

2.  Ga naar de pagina voor het beheren van DNS-records. Elke domeinprovider heeft zijn eigen interface van DNS-records. Doorgaans heeft het sitegedeelte waar u moet zijn, een naam als **Domain Name**, **DNS** of **Name Server Management**.

Pagina DNS-records kan worden weergegeven in **My domains**. De koppeling met de naam **zonebestand**, **DNS-Records**, of **geavanceerde configuratie**.

In de schermafbeelding hieronder wordt een voorbeeld van een pagina met DNS-records weergegeven:

![Voorbeeld van een pagina met DNS-records](media\azure-stack-solution-geo-distributed\image28.png)

1.  Selecteer in de Domeinnaamregistrar, **toevoegen of maken** om een record te maken. Sommige providers hebben afzonderlijke links voor verschillende typen records. Raadpleeg de documentatie van de provider.

2.  Voeg een CNAME-record voor een subdomein toewijzen aan de standaardhostnaam van de app.

  Voor het voorbeeld www.northwindcloud.comdomain toevoegen een CNAME-record die wordt toegewezen, de namewwwto < app\_naam >. azurewebsites.net.

Nadat de CNAME zijn toegevoegd, lijkt de pagina met DNS-records in het volgende voorbeeld:

![Navigatie naar Azure-app in de portal](media\azure-stack-solution-geo-distributed\image29.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a>De toewijzing van het CNAME-record in Azure inschakelen

1.  In een nieuw tabblad moet u zich aanmelden bij de Azure-portal

2.  Navigeer naar de App-Services.

3.  Selecteer de web-app.

4.  Selecteer in het linkernavigatievenster van de app-pagina in de Azure portal **Aangepaste domeinen**.

5.  Selecteer het pictogram **+** naast **Hostnaam toevoegen**.

1.  Typ de volledig gekwalificeerde domeinnaam, zoals `www.northwindcloud.com`.

2.  Selecteer **Valideren**.

3.  Indien aangegeven, toevoegen van extra records van andere typen (`A` of `TXT`) in het domein naam registrars DNS-records. Azure biedt de waarden en typen van deze records:

    a.  Een **A**-record toewijzen aan het IP-adres van de app.

    b.  Een **TXT** record toewijzen aan de standaardhostnaam van de app < app_name >. azurewebsites.net. App Service gebruikt dit record alleen tijdens de configuratie, om te verifiëren dat dit aangepaste domein. Na de verificatie door de TXT-record te verwijderen.

4.  Voltooien van deze taak op het tabblad van de registrar domein en de validatie opnieuw totdat de **hostnaam toevoegen** knop is geactiveerd.

5.  Zorg ervoor dat ** hostnaam recordtype is ingesteld op **CNAME (www.example.com of elk subdomein)**.

6.  Selecteer **Hostnaam toevoegen**.

7.  Typ de volledig gekwalificeerde domeinnaam, zoals `northwindcloud.com`.

8.  Selecteer **Valideren**.

9.  De **toevoegen** is geactiveerd.

10. Zorg ervoor dat ** hostnaam recordtype is ingesteld op **een record (voorbeeld.com)**.

11. **Hostnaam toevoegen**.

  Het duurt even voordat de nieuwe hostnamen worden weergegeven in de app **aangepaste domeinen** pagina. Vernieuw de browser voor om de gegevens bij te werken.
  
  ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image31.png) 
  
  In het geval van een fout wordt een verificatie-foutmelding aan de onderkant van de pagina weergegeven. ![Verificatiefout](media\azure-stack-solution-geo-distributed\image32.png)

> [!Note]  
>  De bovenstaande stappen mogen worden herhaald om toe te wijzen een wildcard-domein (\*. northwindcloud.com)... Hiermee wordt het toevoegen van extra subdomeinen toe die aan deze appservice zonder te hoeven maken van een afzonderlijke CNAME-record voor elk criterium. De registrar instructies om deze instelling te configureren.

#### <a name="test-in-a-browser"></a>Testen in een browser

Blader naar de DNS-namen die eerder hebt geconfigureerd (bijvoorbeeld `northwindcloud.com`, www.northwindcloud.com.

## <a name="part-3-bind-a-custom-ssl-cert"></a>Deel 3: Afhankelijk van een aangepaste SSL-certificaat

In dit gedeelte:

> [!div class="checklist"]
> - Het aangepaste SSL-certificaat binden aan App Service
> - HTTPS afdwingen voor de app.
> - Binden van SSL-certificaat automatiseren met scripts

> [!Note]  
> Indien nodig, verkrijgen van een klant in Azure portal het SSL-certificaat en bindt dit aan de web-app. Volg de [zelfstudie voor App Service Certificates](https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site).

### <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

-   [Een App Service-app maken](https://docs.microsoft.com/azure/app-service/)
-   [Een aangepaste DNS-naam aan uw web-app toewijzen](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain)
-   Een SSL-certificaat van een vertrouwde certificeringsinstantie verkrijgen en gebruiken van de sleutel voor het ondertekenen van de aanvraag

### <a name="requirements-for-your-ssl-certificate"></a>Vereisten voor uw SSL-certificaat

Als u een certificaat in App Service wilt gebruiken, moet het certificaat aan de volgende vereisten voldoen:

-   Ondertekend door een vertrouwde certificeringsinstantie

-   Geëxporteerd als een PFX-bestand met wachtwoordbeveiliging

-   Bevat een persoonlijke sleutel van minstens 2048 bits

-   Bevat alle tussenliggende certificaten in de certificaatketen

> [!Note]  
>  **Elliptic Curve Cryptography (ECC)-certificaten** werken met App Service, maar niet zijn opgenomen in deze handleiding. Raadpleeg een certificeringsinstantie voor hulp bij het maken van ECC-certificaten. 

#### <a name="prepare-the-web-app"></a>De web-app voorbereiden

Een aangepast SSL-certificaat binden aan de web-app, de [App Service-plan](https://azure.microsoft.com/pricing/details/app-service/) moet zich in de **Basic**, **Standard**, of **Premium** laag.

#### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

1.  Open de [Azure-portal](https://portal.azure.com/) en navigeer naar de web-app.

2.  Selecteer in het menu links **App Services**, en selecteer vervolgens de naam van de web-app.

![Selecteer de web-app](media\azure-stack-solution-geo-distributed\image33.png)

#### <a name="check-the-pricing-tier"></a>Controleer de prijscategorie

1.  Schuif in de navigatiebalk links van de web-app-pagina naar de **instellingen** sectie en selecteer **opschalen (App Service-plan)**.

    ![Menu Opschalen](media\azure-stack-solution-geo-distributed\image34.png)

1.  Zorg ervoor dat de web-app zich niet in de **gratis** of **gedeelde** laag. De huidige laag van de web-app wordt gemarkeerd in een donkerblauw vak.

    ![Controleer prijscategorie](media\azure-stack-solution-geo-distributed\image35.png)

Aangepaste SSL wordt niet ondersteund in de categorie **Gratis** of **Gedeeld**. Zo, volg de stappen in de volgende sectie, of **uw prijscategorie kiezen** pagina en gaat u naar [uploaden en uw SSL-certificaat binden](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl).

#### <a name="scale-up-your-app-service-plan"></a>Uw App Service-plan omhoog schalen

1.  Selecteer de prijscategorie **Basic**, **Standard** of **Premium**.

2.  Selecteer **Selecteer**.

![Prijscategorie kiezen](media\azure-stack-solution-geo-distributed\image36.png)

De schaalbewerking is voltooid wanneer de melding wordt weergegeven.

![Melding voor omhoog schalen](media\azure-stack-solution-geo-distributed\image37.png)

#### <a name="bind-your-ssl-certificate-and-merge-intermediate-certificates"></a>Uw SSL-certificaat binden en tussenliggende certificaten samenvoegen

Meerdere certificaten in de keten worden samengevoegd.

1. **Open elk certificaat** u hebt ontvangen in een teksteditor.

2. Maak een bestand voor het samengevoegde certificaat met de naam *mergedcertificate.crt*. Kopieer de inhoud van elk certificaat in dit bestand in een teksteditor. De volgorde van uw certificaten moet de volgorde in de certificaatketen volgen, beginnend met uw certificaat en eindigend met het hoofdcertificaat. Het lijkt op het volgende voorbeeld:

    ```Text

    -----BEGIN CERTIFICATE-----

    <your entire Base64 encoded SSL certificate>

    -----END CERTIFICATE-----

    -----BEGIN CERTIFICATE-----

    <The entire Base64 encoded intermediate certificate 1>

    -----END CERTIFICATE-----

    -----BEGIN CERTIFICATE-----

    <The entire Base64 encoded intermediate certificate 2>

    -----END CERTIFICATE-----

    -----BEGIN CERTIFICATE-----

    <The entire Base64 encoded root certificate>

    -----END CERTIFICATE-----
    ```

#### <a name="export-certificate-to-pfx"></a>Certificaat naar PFX exporteren

Samengevoegde SSL-certificaat exporteren met de persoonlijke sleutel die is gegenereerd door het certificaat.

Een bestand met persoonlijke sleutel is gemaakt via OpenSSL. Als u wilt het certificaat naar PFX exporteren, voer de volgende opdracht, de tijdelijke aanduidingen vervangt *< persoonlijke-sleutel-file >* en *< samengevoegd-certificate-file >* met persoonlijke sleutel paden en samengevoegde bestand voor verificatiecertificaat.

```PowerShell
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>
```

Wanneer u hierom wordt gevraagd, definieert u een wachtwoord voor export voor het uploaden van uw SSL-certificaat naar App Service later opnieuw.

Als IIS of **Certreq.exe** worden gebruikt voor het genereren van de certificaataanvraag, installeert u het certificaat naar een lokale computer en vervolgens [Exporteer het certificaat naar PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

#### <a name="upload-the-ssl-certificate"></a>Het SSL-certificaat uploaden

1.  Selecteer **SSL-instellingen** in het linkernavigatievenster van de web-app.

2.  Selecteer **-certificaat uploaden**.

3.  In **PFX-certificaatbestand**optie PFX-bestand.

4.  4. In **certificaatwachtwoord**, typt u het wachtwoord dat is gemaakt tijdens het exporteren van het PFX-bestand.

5.  Selecteer **Uploaden**.

![Certificaat uploaden](media\azure-stack-solution-geo-distributed\image38.png)

Wanneer App Service klaar is met uploaden van het certificaat, wordt deze weergegeven de **SSL-instellingen** pagina.

![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image39.png)

#### <a name="bind-your-ssl-certificate"></a>Uw SSL-certificaat binden

1.  In de **SSL-bindingen** sectie, selecteer **binding toevoegen**.

    > [!Note]  
    >  Als het certificaat is geüpload, maar niet wordt weergegeven in de domeinnamen in de **hostnaam** vervolgkeuzelijst, vernieuw de browserpagina.

1.  In de **SSL-Binding toevoegen** pagina, gebruikt u de vervolgkeuzelijsten om te selecteren voor het beveiligen van de naam van het domein en het certificaat te gebruiken.

2.  In **SSL-Type**, selecteert u of u wilt gebruiken [ **Server Name Indication (SNI)**](http://en.wikipedia.org/wiki/Server_Name_Indication)of IP-gebaseerd SSL.

-   **Op basis van SNI SSL**-op basis van meerdere SNI SSL-bindingen kunnen worden toegevoegd. Met deze optie kunnen meerdere SSL-certificaten verschillende domeinen beveiligen op hetzelfde IP-adres. De meeste moderne browsers (waaronder Internet Explorer, Chrome, Firefox en Opera) ondersteunen SNI. Ga voor uitgebreidere informatie over browserondersteuning naar [Servernaamindicatie](http://wikipedia.org/wiki/Server_Name_Indication).

-   **IP-gebaseerd SSL**-slechts één IP-gebaseerd SSL-binding worden toegevoegd. Met deze optie kan slechts één SSL-certificaat een specifiek openbaar IP-adres beveiligen. Als u wilt beveiligen in meerdere domeinen, beveiligen ze alle met behulp van het SSL-certificaat. Dit is de traditionele optie voor SSL-binding.

    1.  Selecteer **Binding toevoegen**.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image40.png)

Wanneer App Service klaar is met uploaden van het certificaat, wordt deze weergegeven de **SSL-bindingen** secties.

![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image41.png)

#### <a name="remap-the-a-record-for-ip-ssl"></a>Opnieuw toewijzen van de A-record voor IP SSL

Als het IP-gebaseerd SSL wordt niet gebruikt in de web-app, gaat u naar [Test HTTPS voor uw aangepaste domein](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl).

De web-app maakt standaard gebruik van een gedeelde, openbare IP-adres. Wanneer het certificaat is gekoppeld met IP-gebaseerd SSL, wordt een nieuwe en toegewezen IP-adres voor de web-app gemaakt in App Service.

Wanneer u een A-record is toegewezen aan de web-app, moet het domeinregister worden bijgewerkt met het toegewezen IP-adres.

De **aangepast domein** pagina wordt bijgewerkt met de nieuwe, specifieke IP-adres. Kopieer deze [IP-adres](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain), wijst u vervolgens de [een record](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain) naar deze nieuwe IP-adres.

#### <a name="test-https"></a>HTTPS testen

Blader naar https://<your.custom.domain>to in verschillende browsers, zorg ervoor dat het web Azië en Stille Oceaan wordt geleverd.

![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image42.png)

> [!Note]  
> Als er validatie certificaatfouten optreden, een zelfondertekend certificaat mogelijk de oorzaak of tussenliggende certificaten mogelijk hebben is afgebroken bij het exporteren van het PFX-bestand.

#### <a name="enforce-https"></a>HTTPS afdwingen

Standaard kan iedereen toegang tot de web-app met behulp van HTTP. alle HTTP-verzoeken aan de HTTPS-poort kunnen worden omgeleid.

Selecteer in de web-app-pagina **SL-instellingen**. Klik op **Alleen HTTPS** en selecteer **Aan**.

![HTTPS afdwingen](media\azure-stack-solution-geo-distributed\image43.png)

Wanneer de bewerking is voltooid, gaat u naar een van de HTTP-URL's die naar de app verwijzen. Bijvoorbeeld:

-   http://<APP_NAME>.azurewebsites.NET
-   http://northwindcloud.com
-   <http://www.northwindcloud.com>

#### <a name="enforce-tls-1112"></a>TLS 1.1/1.2 afdwingen

De app kunnen [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.0 standaard wordt niet meer als veilig beschouwd door industriële standaarden, zoals [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). Als u hogere TLS-versies wilt afdwingen, volgt u deze stappen:

1.  Selecteer in de web-app-pagina in het linkernavigatievenster **SSL-instellingen**.

2.  In **TLS-versie**, selecteert u de minimale TLS-versie.

![TLS 1.1 of 1.2 afdwingen](media\azure-stack-solution-geo-distributed\image44.png)

### <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

1.  Selecteer **een resource maken** > **netwerken** > **Traffic Manager-profiel** > **maken**.

2.  Vul het volgende in bij **Traffic Manager-profiel maken**:

    1.  In **naam**, Geef een naam op voor het profiel. Deze naam moet uniek zijn binnen het verkeer manager.net zone en de resultaten in de DNS-naam, verkeer manager.net, die wordt gebruikt voor toegang tot de Traffic Manager-profiel.

    2.  In **routeringsmethode**, selecteer de **Geographicrouting methode**.

    3.  In **abonnement**, selecteer het abonnement waarmee u dit profiel te maken.

    4.  In **Resourcegroep** maakt u een nieuwe resourcegroep om dit profiel voor te maken.

    5.  In **Locatie van de resourcegroep** selecteert u de locatie van de resourcegroep. Deze instelling verwijst naar de locatie van de resourcegroep en heeft geen invloed op het Traffic Manager-profiel dat wereldwijd wordt geïmplementeerd.

    6.  Selecteer **Maken**.

    7.  Wanneer de algemene implementatie van Traffic Manager-profiel voltooid is, wordt deze weergegeven in de bijbehorende resourcegroep als een van de resources.

    ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image45.png)

### <a name="add-traffic-manager-endpoints"></a>Traffic Manager-eindpunten toevoegen

1.  Zoek in de zoekbalk portals de ** Traffic Manager-profiel ** naam gemaakt in de vorige sectie en selecteer de traffic manager-profiel in de resultaten die de weergegeven.

2.  In **Traffic Manager-profiel**, in de **instellingen** sectie, selecteer **eindpunten**.

3.  Selecteer **Toevoegen**.

4.  De Azure Stack-eindpunt toevoegen.

5.  Voor **Type**, selecteer **Extern eindpunt**.

6.  Geef een **naam** voor dit eindpunt, in het ideale geval de naam van de Azure Stack.

7.  Voor de volledig gekwalificeerde domeinnaam (**FQDN**), de externe URL gebruiken voor de Azure Stack Web-App.

8.  Selecteer een regio/continent waar de resource zich bevindt, bijvoorbeeld in het Geo-toewijzing, **Europa.**

9.  Selecteer onder de land/regio vervolgkeuzelijst die wordt weergegeven, het land dat wordt toegepast op dit eindpunt, bijvoorbeeld **Duitsland**.

10. Laat **Toevoegen als uitgeschakeld** uit staan.

11. Selecteer **OK**.

12. Het Azure-eindpunt toevoegen:

    1.  Voor **Type**, selecteer **Azure-eindpunt**.

    2.  Geef een **naam** voor dit eindpunt.

    3.  Voor **Doelresourcetype**, selecteer **App Service**.

    4.  Voor **Doelresource**, selecteer **kiest u een appservice** om de lijst met de Web-Apps onder hetzelfde abonnement weer te geven. In **Resource**, het gebruik van de App service als het eerste eindpunt kiezen.

13. Selecteer een regio/continent waar de resource zich bevindt, bijvoorbeeld in het Geo-toewijzing **Noord-Amerika/Centraal-Amerika/Caribisch gebied.**

14. Onder de land/regio vervolgkeuzelijst die wordt weergegeven, laat dit leeg om alle van de bovenstaande regionale groepering te selecteren.

15. Laat **Toevoegen als uitgeschakeld** uit staan.

16. Selecteer **OK**

  > [!Note]  
  >  Maak ten minste één eindpunt met een geografische bereik van alle (wereld) om te fungeren als het standaardeindpunt voor de resource.

1.  Als beide eindpunten zijn toegevoegd, worden ze weergegeven in **Traffic Manager-profiel**, samen met de controlestatus **Online**.

  ![Alternatieve tekst](media\azure-stack-solution-geo-distributed\image46.png)

**Wereldwijd opererende onderneming is afhankelijk van de mogelijkheden van Azure Geo-distributie**

Routeren van gegevensverkeer via Azure Traffic Manager- en eindpunten Geografie-specifieke, kunt wereldwijd opererende ondernemingen om te voldoen aan de regionale regels en houd gegevens voldoen aan het beleid en veilig essentieel voor het succes van lokale bedrijven en op externe locaties.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure Cloud-patronen, [Cloudontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).
---
title: Een hybride cloud met Azure en Azure Stack implementeren | Microsoft Docs
description: Informatie over het implementeren van een hybride cloud met Azure en Azure Stack.
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
ms.date: 10/18/2018
ms.author: mabrigg
ms.reviewer: anajod
ms.openlocfilehash: 1629c4b62fb04e057c38261a33fd3bc759b279c1
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54267399"
---
# <a name="tutorial-deploy-a-hybrid-cloud-solution-with-azure-and-azure-stack"></a>Zelfstudie: Een oplossing voor hybride cloudopslag met Azure en Azure Stack implementeren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Deze zelfstudie leert u hoe u een hybride cloud-oplossing die gebruikmaakt van de openbare cloud van Azure en de Azure Stack-privécloud implementeert.

U kunt met behulp van een oplossing voor hybride cloudopslag, de voordelen van de naleving van een privécloud combineren met de schaalbaarheid van de openbare cloud. Uw ontwikkelaars kunnen bovendien profiteren van het Microsoft developer-ecosysteem en hun vaardigheden toepassen op de cloud en on-premises-omgevingen.

## <a name="overview-and-assumptions"></a>Overzicht en aannames

In deze zelfstudie voor het instellen van een werkstroom waarmee ontwikkelaars een identieke webtoepassing op een openbare cloud en een privécloud implementeren, kunt u volgen. Deze toepassing zich toegang tot een niet-Internet routeerbare netwerk die wordt gehost op de privécloud. Deze webtoepassingen worden bewaakt en wanneer er een piek in het verkeer, een programma Hiermee wijzigt u de DNS-records voor het omleiden van verkeer naar de openbare cloud. Wanneer verkeer aan het niveau van voordat de piek komt, wordt verkeer doorgestuurd naar de privécloud.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> - Een hybride verbinding SQL Server-database-server implementeren.
> - Een web-app in de globale Azure verbinden met een hybride netwerk.
> - DNS configureren voor het schalen van cross-cloud.
> - Configureer SSL-certificaten voor het schalen van cross-cloud.
> - Configureren en implementeren van de web-App.
> - Een Traffic Manager-profiel maken en configureer deze voor het schalen van cross-cloud.
> - Instellen van Application Insights voor bewaking en waarschuwingen voor het toegenomen verkeer.
> - Configureer Automatische verkeer schakelen tussen de globale Azure en Azure Stack.

### <a name="assumptions"></a>Veronderstellingen

In deze zelfstudie wordt ervan uitgegaan dat u een elementaire kennis hebben van global Azure en Azure Stack. Als u meer informatie wilt voordat u de zelfstudie begint, controleert u deze artikelen:

 - [Inleiding tot Azure](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure Stack-basisbegrippen](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

In deze zelfstudie ook van uitgegaan dat u een Azure-abonnement hebt. Als u geen abonnement hebt, kunt u [Maak een gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, zorg er dan voor dat u kunt voldoen aan de volgende vereisten:

- Een Azure Stack Development Kit (ASDK) of een abonnement op een geïntegreerd Azure Stack-systeem. Volg de instructies in voor het implementeren van een Azure Stack Development Kit, [implementeren de ASDK met behulp van het installatieprogramma](../asdk/asdk-deploy.md).
- Uw Azure Stack-installatie moet het volgende zijn geïnstalleerd:
  - De Azure App Service. Werken met uw Azure Stack-operators om te implementeren en configureren van de Azure App Service in uw omgeving. Deze zelfstudie vereist de App-Service beschikt over ten minste één (1) beschikbaar toegewezen werkrol.
  - Een installatiekopie van Windows Server 2016
  - Een Windows Server 2016 met een Microsoft SQL Server-installatiekopie
  - De juiste plannen en aanbiedingen
 - Een domeinnaam voor uw webtoepassing. Als u een domeinnaam hebt, kunt u een van de domeinprovider van een, zoals GoDaddy, Bluehost en InMotion kunt kopen.
- Een SSL-certificaat voor uw domein van een vertrouwde certificeringsinstantie zoals LetsEncrypt.
- Een web-App die communiceert met een SQL Server-database, en biedt ondersteuning voor Application Insights. U kunt downloaden de [dotnetcore-sqldb-tutorial](https://github.com/Azure-Samples/dotnetcore-sqldb-tutorial) voorbeeld-app vanuit GitHub.
- Een hybride netwerk tussen een Azure-netwerk en het virtuele netwerk van Azure Stack. Zie voor gedetailleerde instructies [hybride cloud-connectiviteit configureren met Azure en Azure Stack](azure-stack-solution-hybrid-connectivity.md).

- Een hybride continue integratie/continue implementatie (CI/CD)-pijplijn met een persoonlijke bouwagent in Azure Stack. Zie voor gedetailleerde instructies [hybride cloud-identiteit met Azure en Azure Stack-toepassingen configureren](azure-stack-solution-hybrid-identity.md)

## <a name="deploy-a-hybrid-connected-sql-server-database-server"></a>Een hybride verbinding SQL Server-database-server implementeren

1. Meld u aan bij de gebruikersportal van Azure Stack.

2. Op de **Dashboard**, selecteer **Marketplace**.

    ![Azure Stack Marketplace](media/azure-stack-solution-hybrid-cloud/image1.png)

3. In **Marketplace**, selecteer **Compute**, en kies vervolgens **meer**. Onder **meer**, selecteer de **gratis licentie voor SQL Server: SQL Server 2017 Developer op Windows Server** installatiekopie.

    ![Selecteer de installatiekopie van een virtuele machine](media/azure-stack-solution-hybrid-cloud/image2.png)

4. Op **gratis licentie voor SQL Server: SQL Server 2017 Developer op Windows Server** Selecteer **maken**.

5. Op **basisbeginselen > basisinstellingen configureren**, bieden een **naam** voor de virtuele machine (VM), een **gebruikersnaam** voor de SQL Server-SA, en een **wachtwoord** voor de SA.  Uit de **abonnement** vervolgkeuzelijst, selecteert u het abonnement dat u implementeert. Voor **resourcegroep**, gebruikt u **Kies bestaande** en plaats de virtuele machine in dezelfde resourcegroep bevinden als uw Azure Stack web-app.

    ![Basisinstellingen voor virtuele machine configureren](media/azure-stack-solution-hybrid-cloud/image3.png)

6. Onder **grootte**, kies een grootte voor uw virtuele machine. Voor deze zelfstudie raden wij A2_Standard of een DS2_V2_Standard.

7. Onder **instellingen > optionele functies configureren**, configureer de volgende instellingen:

    - **Storage-account**. Maak een nieuw account als u nodig hebt.
    - **Virtueel netwerk**

      > [!Important]  
      > Zorg ervoor dat uw SQL Server-VM is geïmplementeerd op hetzelfde virtuele netwerk bevinden als de VPN-gateways.

    - **Openbaar IP-adres**. U kunt de standaardinstellingen kunt gebruiken.
    - **Netwerkbeveiligingsgroep** (NSG). Maak een nieuwe NSG.
    - **Extensies en bewaking**. Behoud de standaardinstellingen.
    - **Opslagaccount voor diagnostische gegevens**. Maak een nieuw account als u nodig hebt.
    - Selecteer **OK** aan uw configuratie op te slaan.

    ![Optionele kenmerken configureren](media/azure-stack-solution-hybrid-cloud/image4.png)

1. Onder **SQL Server-instellingen**, configureer de volgende instellingen:
   - Voor **SQL-verbinding**optie voor het **openbaar (Internet)**.
   - Voor **poort**, gebruik de standaardwaarde, **1433**.
   - Voor **SQL-verificatie**, selecteer **inschakelen**.

     > [!Note]  
     > Wanneer u SQL-verificatie inschakelt, deze moet automatisch invullen met de 'SQLAdmin'-informatie die u hebt geconfigureerd in **basisbeginselen**.

   - Behoud de standaardinstellingen voor de rest van de instellingen. Selecteer **OK**.

    ![SQL Server-instellingen configureren](media/azure-stack-solution-hybrid-cloud/image5.png)

9. Op **samenvatting**, Controleer de configuratie van de virtuele machine en selecteer vervolgens **OK** implementatie te starten.

    ![Overzicht van de configuratie](media/azure-stack-solution-hybrid-cloud/image6.png)

10. Het duurt enige tijd om de nieuwe VM te maken. U ziet de STATUS van uw VM's in **virtuele machines**.

    ![Virtuele machines](media/azure-stack-solution-hybrid-cloud/image7.png)

## <a name="create-web-apps-in-azure-and-azure-stack"></a>Web-apps maken in Azure en Azure Stack

De Azure App Service vereenvoudigt het uitvoeren en beheren van een web-App. Omdat Azure Stack consistent zijn met Azure is, wordt de App Service kunt uitvoeren in beide omgevingen. U gebruikt de App Service om uw toepassing te hosten.

### <a name="create-web-apps"></a>Web-apps maken

1. Een web-app maken in Azure door de instructies in [beheren van een App Service-plan in Azure](https://docs.microsoft.com/azure/app-service/app-service-plan-manage#create-an-app-service-plan). Zorg ervoor dat u de web-app in hetzelfde abonnement en dezelfde resourcegroep als uw hybride netwerk plaatsen.

2. Herhaal de vorige stap (1) in Azure Stack.

### <a name="add-route-for-azure-stack"></a>Route toevoegen voor Azure Stack

De App Service in Azure Stack moet routeerbaar zijn op het openbare Internet zodat gebruikers kunnen toegang tot uw toepassing. Als uw Azure Stack vanop het Internet is, moet u een notitie van het openbare IP-adres of de URL voor de Azure Stack web-app.

Als u een ASDK gebruikt, kunt u [configureren van een statische NAT-toewijzing](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-vpn-connection-one-node#configure-the-nat-virtual-machine-on-each-azure-stack-development-kit-for-gateway-traversal) blootstellen van App Service buiten de virtuele omgeving.

### <a name="connect-a-web-app-in-azure-to-a-hybrid-network"></a>Een web-app in Azure verbinden met een hybride netwerk

Voor connectiviteit tussen de web-front-end in Azure en de SQL Server-database in Azure Stack, de web-app worden verbonden met het netwerk hybride tussen Azure en Azure Stack. Als connectiviteit wilt inschakelen, hebt u naar:

- Punt-naar-site-connectiviteit configureren
- De web-app configureren
- Wijzigen van de lokale netwerkgateway in Azure Stack.

### <a name="configure-the-azure-virtual-network-for-point-to-site-connectivity"></a>Het Azure-netwerk voor punt-naar-site-connectiviteit configureren

De gateway van het virtuele netwerk in de Azure-zijde van de hybride netwerk moet punt-naar-site-verbindingen om te integreren met Azure App Service toestaan.

1. Navigeer in Azure, naar de pagina virtual network gateway. Onder **instellingen**, selecteer **punt-naar-site-configuratie**.

    ![Punt-naar-site-optie](media/azure-stack-solution-hybrid-cloud/image8.png)

2. Selecteer **nu configureren** punt-naar-site configureren.

    ![Start punt-naar-site-configuratie](media/azure-stack-solution-hybrid-cloud/image9.png)

3. Op de **punt-naar-site** configuratie pagina, voer het privé IP-adresbereik dat u gebruiken wilt **-adresgroep**.

   > [!Note]  
   > Zorg ervoor dat het bereik dat u opgeeft niet met een van de adresbereiken die al wordt gebruikt door de subnetten in de globale Azure of Azure Stack-onderdelen van het hybride netwerk overlappen.

   Onder **tunneltype**, schakel het selectievakje de **IKEv2 VPN**. Selecteer **opslaan** voor het configureren van punt-naar-site is voltooid.

   ![Punt-naar-site-instellingen](media/azure-stack-solution-hybrid-cloud/image10.png)

### <a name="integrate-the-azure-app-service-application-with-the-hybrid-network"></a>De Azure App Service-toepassing integreren met de hybride netwerk

1. Volg de instructies in voor de toepassing naar de Azure-VNet verbinding, [VNet-integratie inschakelen](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet#enabling-vnet-integration).

2. Navigeer naar **instellingen** voor de App Service-plan die als host fungeert voor de web-App. In **instellingen**, selecteer **netwerken**.

    ![Configureren van netwerken](media/azure-stack-solution-hybrid-cloud/image11.png)

3. In **VNET-integratie**, selecteer **Klik hier om te beheren**.

    ![Beheren van VNET-integratie](media/azure-stack-solution-hybrid-cloud/image12.png)

4. Selecteer het VNET waarmee u wilt configureren. Onder **IP-ADRESSEN doorgestuurd naar VNET**, voert u het IP-adresbereik voor het Azure-VNet, het Azure Stack-VNet en de punt-naar-site-adresruimten. Selecteer **opslaan** om te valideren en sla deze instellingen.

    ![IP-adresbereiken voor het routeren van](media/azure-stack-solution-hybrid-cloud/image13.png)

Zie voor meer informatie over hoe App Service kan worden geïntegreerd met Azure vnet's, [uw app integreren met een Azure Virtual Network](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet).

### <a name="configure-the-azure-stack-virtual-network"></a>Configureren van het virtuele netwerk van Azure Stack

De lokale netwerkgateway in het virtuele netwerk van Azure Stack moet worden geconfigureerd om verkeer te routeren uit het adresbereik van App Service-punt-naar-site.

1. Navigeer in Azure Stack, naar **lokale netwerkgateway**. Selecteer bij **Instellingen** de optie **Configuratie**.

    ![Gateway-configuratie-optie](media/azure-stack-solution-hybrid-cloud/image14.png)

2. In **adresruimte**, voer het adresbereik van punt-naar-site voor de virtuele netwerkgateway in Azure.l selecteren **opslaan** om te valideren en deze configuratie op te slaan.

    ![Punt-naar-site-adresruimte](media/azure-stack-solution-hybrid-cloud/image15.png)

## <a name="configure-dns-for-cross-cloud-scaling"></a>DNS configureren voor het schalen van cross-cloud

Door het correct configureren van DNS voor cross-cloud-toepassingen, kunnen gebruikers toegang tot de globale Azure en Azure Stack-exemplaren van uw web-app. De DNS-configuratie voor deze zelfstudie kunt ook Azure Traffic Manager verkeer wanneer de belasting toeneemt of afneemt.

In deze zelfstudie wordt Azure DNS voor het beheren van de DNS-server. (App Service-domeinen werkt niet.)

### <a name="create-subdomains"></a>Om subdomeinen te maken

Omdat Traffic Manager, is afhankelijk van CNAME-DNS-records, is een subdomein goed om verkeer te routeren naar eindpunten nodig. Zie voor meer informatie over DNS-records en domeintoewijzing [domeinen toewijzen met Traffic Manager](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager-custom-domain-name)

Voor de Azure eindpunt maakt u een subdomein die gebruikers gebruiken voor toegang tot uw web-app. Voor deze zelfstudie kunt gebruiken **app.northwind.com**, maar u moet deze waarde op basis van uw eigen domein aanpassen.

U moet ook een subdomein met een A-record voor het Azure Stack-eindpunt maken. U kunt **azurestack.northwind.com**.

### <a name="configure-a-custom-domain-in-azure"></a>Een aangepast domein configureren in Azure

1. Voeg de **app.northwind.com** hostnaam naar de Azure-web-app door [toewijzen van een CNAME in Azure App Service](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record).

### <a name="configure-custom-domains-in-azure-stack"></a>Aangepaste domeinen configureren in Azure Stack

1. Voeg de **azurestack.northwind.com** hostnaam naar de Azure Stack web-app door [een A-record toewijzen aan Azure App Service](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain#map-an-a-record). Gebruik het Internet routeerbare IP-adres voor de App Service-toepassing.

2. Voeg de **app.northwind.com** hostnaam naar de Azure Stack web-app door [toewijzen van een CNAME in Azure App Service](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record). Gebruik de hostnaam die u hebt geconfigureerd in de vorige stap (1) als het doel voor de CNAME.

## <a name="configure-ssl-certificates-for-cross-cloud-scaling"></a>Configureren van SSL-certificaten voor het schalen van cross-cloud

U moet ervoor zorgen dat gevoelige gegevens die zijn verzameld door de web-App beveiligen in doorvoer op, en in rust op de SQL-database.

Configureert u uw webtoepassingen voor Azure en Azure Stack SSL-certificaten te gebruiken voor al het binnenkomende verkeer.

### <a name="add-ssl-to-azure-and-azure-stack"></a>SSL toevoegen aan Azure en Azure Stack

SSL toevoegen aan Azure:

1. Zorg ervoor dat het u SSL-certificaat is geldig voor het subdomein dat u hebt gemaakt. (Dit klopt jokertekencertificaten te gebruiken.)

2. In Azure, volg de instructies in de **voorbereiden van uw web-app** en **uw SSL-certificaat binden** secties van de [een bestaand aangepast SSL-certificaat binden aan Azure Web Apps](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-ssl) de artikelen. Selecteer **SNI gebaseerde SSL** als de **SSL-Type**.

3. Alle verkeer omleiden naar de HTTPS-poort. Volg de instructies in de **HTTPS afdwingen** sectie van de [een bestaand aangepast SSL-certificaat binden aan Azure Web Apps](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-ssl) artikel.

SSL toevoegen aan Azure Stack:

- Herhaal stappen 1-3 die u hebt gebruikt voor Azure.

## <a name="configure-and-deploy-the-web-application"></a>Configureer en implementeer de web-App

U moet de toepassingscode configureren voor rapport telemetrie naar de juiste Application Insights-exemplaar en de web-apps configureren met de juiste verbindingsreeksen. Zie voor meer informatie over Application Insights, [wat is Application Insights?](https://docs.microsoft.com/azure/application-insights/app-insights-overview)

### <a name="add-application-insights"></a>Application Insights toevoegen

1. Open uw webtoepassing in Microsoft Visual Studio.

2. [Application Insights toevoegen](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core#add-application-insights-telemetry) aan uw project voor het verzenden van de telemetrie die gebruikmaakt van Application Insights om waarschuwingen te maken bij het webverkeer vergroot of verkleint.

### <a name="configure-dynamic-connection-strings"></a>Dynamische verbindingsreeksen configureren

Elk exemplaar van de web-App wordt een andere methode gebruiken om met de SQL-database. De privé IP-adres van de SQL Server virtuele machine (VM) maakt gebruik van de toepassing in Azure en de toepassing in Azure Stack maakt gebruik van het openbare IP-adres van de SQL Server-VM.

> [!Note]  
> Op een Azure Stack geïntegreerd systeem mag het openbare IP-adres geen Internet routeerbare. Op een Azure Stack Development Kit (ASDK), het openbare IP-adres is niet routeerbaar is buiten de ASDK.

Variabelen voor App Service-omgeving kunt u een andere verbindingsreeks doorgeven aan elk exemplaar van de toepassing.

1. Open de toepassing in Visual Studio.

2. Open Startup.cs en zoek het volgende codeblok:

    ```C#
    services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
    ```

3. Vervang het vorige codeblok door de volgende code, die gebruikmaakt van een verbindingsreeks die is gedefinieerd in het bestand appsettings.json:

    ```C#
    services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
     // Automatically perform database migration
     services.BuildServiceProvider().GetService<MyDatabaseContext>().Database.Migrate();
    ```

### <a name="configure-app-service-application-settings"></a>App Service-toepassing-instellingen configureren

1. Maak tekenreeksen voor databaseverbindingen voor Azure en Azure Stack. De tekenreeksen moeten hetzelfde, met uitzondering van de IP-adressen die worden gebruikt.

2. In Azure en Azure Stack, de juiste verbindingsreeks toevoegen [als een toepassingsinstelling](https://docs.microsoft.com/azure/app-service/web-sites-configure) in de web-App met behulp van `SQLCONNSTR\_` als een voorvoegsel in de naam.

3. **Sla** de instellingen voor web-app en start de toepassing opnieuw.

## <a name="enable-automatic-scaling-in-global-azure"></a>Automatisch schalen in de globale Azure inschakelen

Wanneer u uw web-app maken in een App Service-omgeving die wordt gestart met één exemplaar. U kunt automatisch uitschalen naar exemplaren om te bieden dat meer rekenbronnen voor uw app toe te voegen. Op deze manier kunt u automatisch schalen in en verminder het aantal exemplaren van de behoeften van uw app.

> [!Note]  
> U moet hebben een App Service-Plan om scale-out en inschalen te configureren. Als u een abonnement hebt, maakt u een voordat u begint met de volgende stappen.

### <a name="enable-automatic-scale-out"></a>Automatische scale-out inschakelen

1. Zoeken in Azure, de App Service-plan voor de sites die u wilt uitschalen, en selecteer vervolgens **Scale-out (App Service-plan)**.

    ![Uitschalen](media/azure-stack-solution-hybrid-cloud/image16.png)

2. Selecteer **automatisch schalen inschakelen**.

    ![Automatisch schalen inschakelen](media/azure-stack-solution-hybrid-cloud/image17.png)

3. Voer een naam in voor **naam van de instelling voor automatisch schalen**. Voor de **standaard** automatisch schalen regel, selecteer **schalen op basis van een metrische waarde**. Stel de **Instantielimieten** naar **Minimum: 1**, **Maximum: 10**, en **standaard: 1**.

    ![Automatisch schalen configureren](media/azure-stack-solution-hybrid-cloud/image18.png)

4. Selecteer **+ toevoegen van een regel**.

5. In **metriek bron**, selecteer **huidige Resource**. Gebruik de volgende Criteria en acties voor de regel.

**Criteria**

1. Onder **tijdverzameling,** Selecteer **gemiddelde**.

2. Onder **metriek naam**, selecteer **CPU-Percentage**.

3. Onder **Operator**, selecteer **groter is dan**.

   - Stel de **drempelwaarde** naar **50**.
   - Stel de **duur** naar **10**.

**Actie**

1. Onder **bewerking**, selecteer **aantal verhogen met**.

2. Stel de **exemplaren** naar **2**.

3. Stel de **Cooldown** naar **5**.

4. Selecteer **Toevoegen**.

5. Selecteer de **+ toevoegen van een regel**.

6. In **metriek bron**, selecteer **huidige Resource.**

   > [!Note]  
   > De huidige resource bevat uw App Service-plan naam/GUID, en de **resourcetype** en **Resource** vervolgkeuzelijsten worden lichter gekleurd weergegeven.

### <a name="enable-automatic-scale-in"></a>Automatisch schalen in te schakelen

Wanneer netwerkverkeer afneemt, kan de Azure-web-App automatisch het aantal actieve instanties om kosten te verlagen verminderen. Deze actie is om het aantal van de impact op gebruikers van de toepassing minder agressief is dan de scale-out.

1. Navigeer naar de **standaard** scale-out voorwaarde, selecteer **+ toevoegen van een regel**. Gebruik de volgende Criteria en acties voor de regel.

**Criteria**

1. Onder **tijdverzameling,** Selecteer **gemiddelde**.

2. Onder **metriek naam**, selecteer **CPU-Percentage**.

3. Onder **Operator**, selecteer **minder dan**.

   - Stel de **drempelwaarde** naar **30**.
   - Stel de **duur** naar **10**.

**Actie**

1. Onder **bewerking**, selecteer **aantal verlagen met**.

   - Stel de **exemplaren** naar **1**.
   - Stel de **Cooldown** naar **5**.

2. Selecteer **Toevoegen**.

## <a name="create-a-traffic-manager-profile-and-configure-cross-cloud-scaling"></a>Een Traffic Manager-profiel maken en schalen binnen de cloud configureren

U moet een Traffic Manager-profiel maken in Azure en configureer vervolgens eindpunten om in te schakelen schalen binnen de cloud.

### <a name="create-traffic-manager-profile"></a>Traffic Manager-profiel maken

1. Selecteer **een resource maken**
2. Selecteer **netwerken**
3. Selecteer **Traffic Manager-profiel** en configureer het volgende:

   - In **naam**, voer een naam in voor uw profiel. Deze naam **moet** uniek zijn in de zone trafficmanager.net en wordt gebruikt voor het maken van een nieuwe DNS-naam (bijvoorbeeld northwindstore.trafficmanager.net).
   - Voor **routeringsmethode**, selecteer de **gewogen**.
   - Voor **abonnement**, selecteer het abonnement dat u wilt maken van dit profiel in.
   - In **resourcegroep**, maak een nieuwe resourcegroep op voor dit profiel.
   - In **Locatie van de resourcegroep** selecteert u de locatie van de resourcegroep. Deze instelling verwijst naar de locatie van de resourcegroep en heeft geen invloed op het Traffic Manager-profiel dat wereldwijd wordt geïmplementeerd.

4. Selecteer **Maken**.

    ![Traffic Manager-profiel maken](media/azure-stack-solution-hybrid-cloud/image19.png)

 Wanneer de algemene implementatie van uw Traffic Manager-profiel voltooid is, wordt deze weergegeven in de lijst met resources voor de resourcegroep die u hebt gemaakt onder.

### <a name="add-traffic-manager-endpoints"></a>Traffic Manager-eindpunten toevoegen

1. Zoeken naar de Traffic Manager-profiel dat u hebt gemaakt. (Als u de resourcegroep voor het profiel genavigeerd, selecteert u het profiel.)

2. In **Traffic Manager-profiel**onder **instellingen**, selecteer **eindpunten**.

3. Selecteer **Toevoegen**.

4. In **eindpunt toevoegen**, gebruik de volgende instellingen voor Azure Stack:

   - Voor **Type**, selecteer **Extern eindpunt**.
   - Voer een **naam** voor dit eindpunt.
   - Voor **volledig gekwalificeerde domeinnaam (FQDN) of IP-** de externe URL invoert voor uw Azure Stack web-app.
   - Voor **gewicht**, gebruik de standaardwaarde, **1**. Dit gewicht resulteert in al het verkeer naar dit eindpunt als deze in orde is.
   - Laat **toevoegen als uitgeschakeld** uitgeschakeld.

5. Selecteer **OK** om op te slaan van het Azure Stack-eindpunt.

Vervolgens moet u het Azure-eindpunt configureren.

1. Op **Traffic Manager-profiel**, selecteer **eindpunten**.
2. Selecteer **+ toevoegen**.
3. Op **eindpunt toevoegen**, gebruik de volgende instellingen voor Azure:

   - Voor **Type**, selecteer **Azure-eindpunt**.
   - Voer een **naam** voor dit eindpunt.
   - Voor **Doelresourcetype**, selecteer **App Service**.
   - Voor **Doelresource**, selecteer **kiest u een appservice** voor een overzicht van Web-Apps in hetzelfde abonnement.
   - Kies in **Resource** de app-service die u als eerste eindpunt wilt toevoegen.
   - Voor **gewicht**, selecteer **2**. Dit resulteert in al het verkeer naar dit eindpunt als het primaire eindpunt beschadigd is of u een regel/waarschuwing dat opnieuw verkeer hebt stuurt wanneer ze worden geactiveerd.
   - Laat **toevoegen als uitgeschakeld** uitgeschakeld.

4. Selecteer **OK** om op te slaan van het Azure-eindpunt.

Nadat de beide eindpunten zijn geconfigureerd, worden weergegeven in **Traffic Manager-profiel** wanneer u selecteert **eindpunten**. Het voorbeeld in de volgende schermafbeelding ziet twee eindpunten, met de status en configuratie-informatie voor elk criterium.

![Eindpunten](media/azure-stack-solution-hybrid-cloud/image20.png)

## <a name="set-up-application-insights-monitoring-and-alerting"></a>Application Insights voor bewaking en waarschuwingen instellen

Azure Application Insights kunt u voor het bewaken van uw toepassing en het verzenden van meldingen op basis van voorwaarden die u configureert. Enkele voorbeelden zijn: de toepassing is niet beschikbaar, er fouten of prestatieproblemen wordt weergegeven.

U kunt metrische gegevens van Application Insights gebruiken om waarschuwingen te maken. Als deze waarschuwingen activeren, wordt uw Web-Apps-exemplaar wordt automatisch overschakelen van de Azure Stack in Azure om uit te schalen en vervolgens weer terug naar Azure stack op schaal in.

### <a name="create-an-alert-from-metrics"></a>Een waarschuwing maken vanuit metrische gegevens

Navigeer naar de resourcegroep voor deze zelfstudie, en selecteer vervolgens de Application Insights-exemplaar te openen **Application Insights**.

![Application Insights](media/azure-stack-solution-hybrid-cloud/image21.png)

In deze weergave kunt u een scale-out waarschuwing en een schaalset maken in de waarschuwing.

### <a name="create-the-scale-out-alert"></a>De scale-out waarschuwing maken

1. Onder **configureren**, selecteer **waarschuwingen (klassiek)**.
2. Selecteer **metrische waarschuwing toevoegen (klassiek)**.
3. In **regel toevoegen**, configureert u het volgende:

   - Voor **naam**, voer **in Azure-Cloud-Burst**.
   - Een **beschrijving** is optioneel.
   - Onder **bron**, **waarschuwen bij**, selecteer **metrische gegevens**.
   - Onder **Criteria**, selecteer uw abonnement, de resourcegroep voor uw Traffic Manager-profiel en de naam van Traffic Manager-profiel voor de Resource.

4. Voor **Metric**, selecteer **snelheid van aanvragen voor**.
5. Voor **voorwaarde**, selecteer **groter is dan**.
6. Voor **drempelwaarde**, voer **2**.
7. Voor **periode**, selecteer **in de afgelopen 5 minuten**.
8. Onder **hierover te informeren via**:
   - Schakel het selectievakje in voor **e-eigenaren, bijdragers en lezers**.
   - Voer uw e-mailadres voor **beheerder email(s)**.

9. Selecteer op de menubalk **opslaan**.

### <a name="create-the-scale-in-alert"></a>De schaal in waarschuwing maken

1. Onder **configureren**, selecteer **waarschuwingen (klassiek)**.
2. Selecteer **metrische waarschuwing toevoegen (klassiek)**.
3. In **regel toevoegen**, configureert u het volgende:

   - Voor **naam**, voer **schaal terug in Azure Stack**.
   - Een **beschrijving** is optioneel.
   - Onder **bron**, **waarschuwen bij**, selecteer **metrische gegevens**.
   - Onder **Criteria**, selecteer uw abonnement, de resourcegroep voor uw Traffic Manager-profiel en de naam van Traffic Manager-profiel voor de Resource.

4. Voor **Metric**, selecteer **snelheid van aanvragen voor**.
5. Voor **voorwaarde**, selecteer **minder dan**.
6. Voor **drempelwaarde**, voer **2**.
7. Voor **periode**, selecteer **in de afgelopen 5 minuten**.
8. Onder **hierover te informeren via**:
   - Schakel het selectievakje in voor **e-eigenaren, bijdragers en lezers**.
   - Voer uw e-mailadres voor **beheerder email(s)**.

9. Selecteer op de menubalk **opslaan**.

De volgende schermafbeelding ziet u de waarschuwingen voor scale-out en schalen in.

   ![Waarschuwingen (klassiek)](media/azure-stack-solution-hybrid-cloud/image22.png)

## <a name="redirect-traffic-between-azure-and-azure-stack"></a>Omleiden van verkeer tussen Azure en Azure Stack

U kunt handmatig of automatisch overschakelen van uw Web-appverkeer schakelen tussen Azure en Azure Stack.

### <a name="configure-manual-switching-between-azure-and-azure-stack"></a>Configureren van handmatige schakelen tussen Azure en Azure Stack

Wanneer uw website de drempels die u configureert bereikt, krijgt u een waarschuwing. Gebruik de volgende stappen handmatig verkeer omleiden naar Azure.

1. Selecteer uw Traffic Manager-profiel in de Azure-portal.

    ![Traffic Manager-eindpunten](media/azure-stack-solution-hybrid-cloud/image20.png)

2. Selecteer **eindpunten**.
3. Selecteer de **Azure-eindpunt**.
4. Onder **Status** , selecteer **ingeschakeld**, en selecteer vervolgens **opslaan**.

    ![Azure-eindpunt inschakelen](media/azure-stack-solution-hybrid-cloud/image23.png)

5. Op **eindpunten** voor het Traffic Manager-profiel, selecteer **Extern eindpunt**.
6. Onder **Status** , selecteer **uitgeschakelde**, en selecteer vervolgens **opslaan**.

    ![Azure Stack-eindpunt uitschakelen](media/azure-stack-solution-hybrid-cloud/image24.png)

Nadat de eindpunten zijn geconfigureerd, gaat u toepassingsverkeer naar uw Azure scale-out-web-app in plaats van de Azure Stack web-app.

 ![Eindpunten gewijzigd](media/azure-stack-solution-hybrid-cloud/image25.png)

Als u wilt de stroom omkeren terug naar Azure Stack, gebruikt u de vorige stappen voor het:

- Het Azure Stack-eindpunt inschakelen
- Het Azure-eindpunt uitschakelen

### <a name="configure-automatic-switching-between-azure-and-azure-stack"></a>Configureren van automatisch overschakelen tussen Azure en Azure Stack

U kunt ook gebruiken voor Application Insights-bewaking als uw toepassing wordt uitgevoerd een [serverloze](https://azure.microsoft.com/overview/serverless-computing/) omgeving geleverd door Azure Functions.

In dit scenario kunt u Application Insights voor het gebruik van een webhook die een functie-app roept configureren. Deze app wordt automatisch Hiermee of een eindpunt in reactie op een waarschuwing wordt uitgeschakeld.

Gebruik de volgende stappen uit als richtlijn configureren automatische verkeer overschakelen.

1. Een Azure-functie-app maken.
2. Een HTTP-geactiveerde functie maken.
3. Het importeren van de Azure SDK's voor Resource Manager, Web-Apps en Traffic Manager.
4. Code voor het ontwikkelen:

   - Worden geverifieerd bij uw Azure-abonnement.
   - Gebruik een parameter die omschakelt van het Traffic Manager-eindpunten voor verkeer naar Azure of Azure Stack.

5. Sla uw code en Voeg URL van de functie-app met de juiste parameters op de **Webhook** sectie van de instellingen van de waarschuwingsregel Application Insights.
6. Verkeer wordt automatisch omgeleid wanneer er een Application Insights-waarschuwing wordt geactiveerd.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure Cloud-patronen, [Cloudontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).

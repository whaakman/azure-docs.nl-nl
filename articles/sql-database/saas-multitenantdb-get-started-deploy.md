---
title: Een database van shard multitenant SaaS-app die gebruikmaakt van Azure SQL Database implementeren | Microsoft Docs
description: Implementeren en de shard Wingtip Tickets SaaS multitenant databasetoepassing, die u laat SaaS patronen zien met behulp van Azure SQL Database verkennen.
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: billgib;anjangsh
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: genemi
ms.openlocfilehash: a7e6e319fb2fa8fee762055b625427403d14d679
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application-that-uses-azure-sql-database"></a>Implementeren en een shard multitenant-toepassing die gebruikmaakt van Azure SQL Database verkennen

In deze zelfstudie maakt u implementeert en een SaaS multitenant database voorbeeldtoepassing met de naam Wingtip Tickets verkennen. De app Wingtip is ontworpen om hier functies van Azure SQL Database die de implementatie van de SaaS-scenario's vereenvoudigen.

Deze implementatie van Wingtips maakt gebruik van een patroon shard multitenant-database. De sharding is door de tenant-id. Gegevens van de tenant wordt gedistribueerd naar een bepaalde database volgens de tenant-id-waarden. Ongeacht hoeveel tenants een bepaalde database bevat, zijn alle databases die meerdere tenants in de zin dat het tabelschema een tenant-id bevatten. 

Dit patroon van een database kunt u een of meer tenants opslaan in elke shard of de database. U kunt voor de laagste kosten optimaliseren door elke database worden gedeeld door meerdere tenants. Of u kunt optimaliseren voor isolatie door elke tenant slechts één opslaan database. Uw keuze optimalisatie kan afzonderlijk worden gemaakt voor elke specifieke tenant. Uw keuze kan worden gemaakt wanneer de tenant voor het eerst wordt opgeslagen of kunt u later van gedachten verandert. De toepassing is ontworpen voor gebruik van goed in beide gevallen.

#### <a name="app-deploys-quickly"></a>App snel worden geïmplementeerd

De volgende Implementatiesectie biedt de blauwe **implementeren in Azure** knop. Wanneer de knop wordt ingedrukt, wordt de app Wingtip een vijf minuten later volledig geïmplementeerd. De app Wingtip wordt uitgevoerd in de Azure-cloud en Azure SQL Database gebruikt. Wingtip wordt geïmplementeerd in uw Azure-abonnement. U hebt volledige toegang tot het werken met de afzonderlijke toepassingsonderdelen.

De toepassing wordt geïmplementeerd met gegevens voor drie voorbeeld tenants. De tenants worden samen in een multitenant-database opgeslagen.

Iedereen kan de broncode C# en PowerShell downloaden voor Wingtip Tickets van [de GitHub-opslagplaats][link-github-wingtip-multitenantdb-55g].

#### <a name="learn-in-this-tutorial"></a>In deze zelfstudie leert

> [!div class="checklist"]
> - Klik hier voor meer informatie over het implementeren van de Wingtip SaaS-toepassing.
> - Waar u de broncode van de toepassing en beheerscripts schrijven.
> - Over de servers en databases die gezamenlijk de app.
> - Hoe tenants zijn toegewezen aan hun gegevens met de *catalogus*.
> - Het inrichten van een nieuwe tenant.
> - Het controleren van de tenant-activiteit in de app.

Er is een reeks gerelateerde zelfstudies beschikbaar die bouwen voort op deze initiële implementatie. De zelfstudies verkennen een bereik van SaaS-patronen voor ontwerpen en beheren. Wanneer u de zelfstudies doorloopt, wordt u aangeraden stap tot en met de opgegeven scripts om te zien hoe de andere SaaS-patronen worden geïmplementeerd.

## <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

- De nieuwste Azure PowerShell is geïnstalleerd. Zie voor meer informatie [aan de slag met Azure PowerShell][link-azure-get-started-powershell-41q].

## <a name="deploy-the-wingtip-tickets-app"></a>Implementeer de Wingtip Tickets app

#### <a name="plan-the-names"></a>De namen plannen

In de stappen van deze sectie worden twee plaatsen waar moet u de namen voor u als een *gebruiker* en voor uw nieuwe *resourcegroep*. Voor een persoon met de naam *Anne Finley*, we raden aan de volgende namen:
- *Gebruiker:* &nbsp; **af1** &nbsp; *(haar initialen, plus een cijfer.)*
- *Resourcegroep:* &nbsp; **wingtip af1** &nbsp; *(wordt aangeraden alleen kleine letters bevatten. Voegt een koppelteken vervolgens de gebruikersnaam.)*

Kies nu uw namen en schrijf ze op.

#### <a name="steps"></a>Stappen

1. Klik op de volgende blauwe **implementeren in Azure** knop.
    - De Azure-portal wordt geopend met de sjabloon Wingtip Tickets SaaS-implementatie.

    [![Knop voor implementeren in Azure.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

2. Geef de vereiste parameter-waarden voor de implementatie.

    > [!IMPORTANT]
    > Voor deze demonstratie gebruik niet alle bestaande resourcegroepen, servers of groepen. Kies in plaats daarvan **een nieuwe resourcegroep maken**. Verwijder deze resourcegroep wanneer u klaar bent om gerelateerde facturering te stoppen.
    > Gebruik deze toepassing of alle bronnen die wordt gemaakt, niet voor productie. Sommige aspecten van verificatie en de server firewall-instellingen zijn opzettelijk onveilig in de app te vergemakkelijken de demonstratie.

    - Voor **resourcegroep** : Selecteer **nieuw**, en geef vervolgens een **naam** voor de resourcegroep (hoofdlettergevoelig).
        - Selecteer een **locatie** uit de vervolgkeuzelijst.
    - Voor **gebruiker** -het is raadzaam dat u een korte kiest **gebruiker** waarde.

3. **Implementeer de toepassing**.

    - Klik om de voorwaarden en bepalingen te accepteren.
    - Klik op **Kopen**.

4. Implementatiestatus controleren door te klikken op **meldingen**, namelijk het belpictogram rechts van het zoekvak. Implementatie van de app Wingtip duurt ongeveer vijf minuten.

   ![implementatie gelukt](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Download en blokkering opheffen van de management-scripts

Download de bron en het beheer toepassingsscripts tijdens de implementatie van de toepassing.

> [!NOTE]
> Uitvoerbare inhoud (scripts, dll-bestanden) mogelijk geblokkeerd door Windows als zip-bestanden van een externe bron wordt gedownload en uitgepakt. Bij het uitpakken van de scripts uit een zipbestand, gebruikt u de volgende stappen uit om de blokkering van het ZIP-bestand voor het uitpakken van te. Het ZIP-bestand deblokkering, zorgt u ervoor dat de scripts mogen worden uitgevoerd.

1. Blader naar [de WingtipTicketsSaaS MultiTenantDb GitHub-repo](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb).
2. Klik op **klonen of downloaden**.
3. Klik op **ZIP downloaden** en sla het bestand.
4. Met de rechtermuisknop op de **WingtipTicketsSaaS-MultiTenantDb-master.zip** bestand en selecteer **eigenschappen**.
5. Op de **algemene** tabblad **blokkering**, en klik op **toepassen**.
6. Klik op **OK**.
7. Pak de bestanden.

De scripts bevinden zich in de *... \\WingtipTicketsSaaS MultiTenantDb master\\Learning-Modules\\*  map.

## <a name="update-the-configuration-file-for-this-deployment"></a>Het configuratiebestand voor deze implementatie bijwerken

Voordat u de scripts, stelt u de *resourcegroep* en *gebruiker* in waarden **UserConfig.psm1**. Deze variabelen ingesteld op dezelfde waarden als die u tijdens de implementatie instellen.

1. Open... \\Learning-Modules\\*UserConfig.psm1* in de *PowerShell ISE*.
2. Update *ResourceGroupName* en *naam* met de specifieke waarden voor uw implementatie (op regels 10 en 11 alleen).
3. Sla de wijzigingen op.

De waarden in dit bestand worden gebruikt door de scripts, dus is het belangrijk dat ze juist zijn. Als u de app implementeren, moet u verschillende waarden voor de gebruiker en de resourcegroep. Werk het bestand UserConfig.psm1 opnieuw vervolgens met de nieuwe waarden.

## <a name="run-the-application"></a>De toepassing uitvoeren

De tenants zijn in de app Wingtip plaatsen. U wilt een mag overleg hall, de vereniging van een sport of een andere locatie die als host fungeert voor gebeurtenissen. De plaatsen in Wingtip registreren als klanten en de id van een tenant wordt gegenereerd voor elke locatie vast. Elke locatie vast overzicht van de toekomstige gebeurtenissen in Wingtip, zodat het publiek kaartjes voor de gebeurtenissen kunt aanschaffen.

Elke locatie vast Hiermee haalt u een aangepaste web-app lijst met hun gebeurtenissen en tickets verkopen. Elke web-app is onafhankelijk en geïsoleerd van andere tenants. Intern in Azure SQL Database, elk van de gegevens voor elke tenant standaard in een shard multitenant-database opgeslagen wordt. Alle gegevens is gemarkeerd met de tenant-id.

Een centraal **gebeurtenissen Hub** webpagina bevat een lijst met koppelingen voor de tenants in uw specifieke implementatie. Gebruik de volgende stappen om de **gebeurtenissen Hub** webpagina en een afzonderlijke web-app:

1. Open de **gebeurtenissen Hub** in uw webbrowser:
    - http://events.Wingtip. &lt;Gebruiker&gt;. trafficmanager.net &nbsp; *(Vervang &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie.)*

    ![events hub](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. Klik in de **Events Hub** op **Fabrikam Jazz Club**.

   ![Gebeurtenissen](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Voor het beheren van de distributie van binnenkomende aanvragen de Wingtip app gebruikmaakt van [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). De pagina gebeurtenissen voor elke tenant bevat de naam van de tenant in de URL. Elke URL bevat ook de waarde van uw specifieke gebruiker. Elke URL gehoorzaamt aan de indeling weergegeven met behulp van de volgende stappen uit:

- http://events.Wingtip. &lt;Gebruiker&gt;.trafficmanager.net/*fabrikamjazzclub*

1. De app gebeurtenissen parseert de tenantnaam van de URL. De tenantnaam van de is *fabrikamjazzclub* in het voorgaande voorbeeld-URL.
2. De app vervolgens de naam van de tenant te maken van een sleutel voor toegang tot een catalogus met hashes [shard kaart management](sql-database-elastic-scale-shard-map-management.md).
3. De app in de catalogus gevonden met de sleutel en haalt de bijbehorende locatie van de tenant-database.
4. De app gebruikmaakt van de locatie-informatie vinden en openen van de ene database waarin alle gegevens voor de tenant.

#### <a name="events-hub"></a>Gebeurtenissen Hub

1. De **gebeurtenissen Hub** geeft een lijst van de tenants die zijn geregistreerd in de catalogus en hun plaatsen.
2. De **gebeurtenissen Hub** uitgebreide metagegevens in de catalogus gebruikt voor het ophalen van de tenantnaam die zijn gekoppeld aan elke wordt toegewezen aan de URL's samenstellen.

In een productieomgeving u doorgaans een DNS CNAME-record te maken [het internetdomein van een bedrijf wijzen](../traffic-manager/traffic-manager-point-internet-domain.md) aan traffic manager-profiel.

## <a name="start-generating-load-on-the-tenant-databases"></a>Workloads genereren voor de tenant-databases

Nu dat de app is geïmplementeerd, laten we aan de slag! De *Demo LoadGenerator* PowerShell-script voor een werkbelasting die wordt uitgevoerd voor elke tenant wordt gestart. De werkelijke belasting van veel SaaS-apps is doorgaans sporadisch onvoorspelbaar. Om te simuleren belasting, levert de generator belasting verdeeld over alle tenants. De belasting omvat willekeurige bursts op elke tenant optreedt op willekeurige intervallen. Het duurt enkele minuten voor het patroon load te komen, is het verstandig om te laten de generator uitvoeren voor ten minste drie tot vier minuten voor de bewaking van de belasting.

1. In de *PowerShell ISE*, open de... \\Learning-Modules\\hulpprogramma's\\*Demo LoadGenerator.ps1* script.
2. Druk op **F5** om het script uit te voeren en de load-generator te starten (gebruik voorlopig de standaardwaarden van het script).

De *Demo LoadGenerator.ps1* script waarin de load-generator wordt uitgevoerd een andere PowerShell-sessie wordt geopend. De load-generator wordt uitgevoerd in deze sessie een voorgrond-taak die wordt aangeroepen achtergrond generatie load-taken, één voor elke tenant.

Nadat de voorgrond-taak wordt gestart, blijft deze in een status-taak wordt aangeroepen. De taak wordt gestart in de volgende taken voor alle nieuwe tenants die vervolgens zijn ingericht.

De PowerShell-sessie sluit stopt alle taken.

Mogelijk wilt de sessie laden generator voor het gebruik van andere parameterwaarden opnieuw starten. Zo ja, sluit u de PowerShell generatie sessie en voer de *Demo LoadGenerator.ps1*.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Een nieuwe tenant in te richten in de shard-database

De eerste implementatie omvat drie voorbeeld tenants in de *Tenants1* database. Stel een andere tenant maken en bekijk de gevolgen voor de gedistribueerde toepassing. Druk op één sleutel voor het maken van een nieuwe tenant in deze stap:

1. Open... \\Learning-Modules\\inrichten en catalogus\\*Demo ProvisionTenants.ps1* in de *PowerShell ISE*.
2. Druk op **F5** (geen **F8**) het script uit te voeren (laat de standaardwaarden voor deze oefening).

   > [!NOTE]
   > U moet de PowerShell-scripts uitvoeren door alleen maar te drukken de **F5** sleutel, niet door te drukken **F8** een deel van het script uitvoeren. Het probleem met **F8** is dat de *$PSScriptRoot* variabele wordt niet geëvalueerd. Deze variabele wordt door veel scripts nodig om te navigeren van mappen, of om aan te roepen andere scripts of voor het importeren van modules.

De nieuwe tenant voor rode esdoorn race wordt toegevoegd aan de *Tenants1* database en geregistreerd in de catalogus. De nieuwe tenant de ticket verkopen **gebeurtenissen** site in uw browser geopend:

![Nieuwe tenant](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Vernieuw de **gebeurtenissen Hub**, en de nieuwe tenant wordt nu weergegeven in de lijst.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Een nieuwe tenant in de eigen database inrichten

De shard multi-tenant model, kunt u kiezen of voor het inrichten van een nieuwe tenant in een database met andere tenants of in een database met een eigen. Een tenant in zijn eigen database geïsoleerd leuk vindt de volgende voordelen:
- De prestaties van de tenant-database kan worden beheerd zonder de noodzaak binnendringen met de behoeften van andere tenants.
- Indien nodig, kan de database worden hersteld naar een eerder tijdstip, omdat er geen andere tenants wordt beïnvloed.

U kunt kiezen om de gratis proefversie klanten of economie klanten, in de databases van meerdere tenants geplaatst. In een eigen toegewezen databaseserver kan u elke premium-tenant geplaatst. Als u veel databases die slechts één tenant bevatten maakt, kunt u ze allemaal gezamenlijk beheren in een elastische pool resourcekosten optimaliseren.

We inrichten vervolgens dit moment in een eigen database een andere tenant:

1. In... \\Learning-Modules\\inrichten en catalogus\\*Demo ProvisionTenants.ps1*, wijzigen *$TenantName* naar **Salix Salsa**, *$VenueType* naar **dans** en *$Scenario* naar **2**.

2. Druk op **F5** het script opnieuw uitvoeren.
    - Dit **F5** druk op de nieuwe tenant in een aparte database inricht. De database en de tenant worden geregistreerd in de catalogus. De browser wordt geopend op de pagina gebeurtenissen van de tenant.

   ![Pagina Salix Salsa-gebeurtenissen](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Ga naar de onderkant van de pagina. Er in de banner ziet u de naam van de database waarin de gegevens van de tenant is opgeslagen.

3. Vernieuw de **gebeurtenissen Hub** en de twee nieuwe tenants wordt nu weergegeven in de lijst.

## <a name="explore-the-servers-and-tenant-databases"></a>Verken de servers en tenant-databases

Nu kijken we een aantal van de resources die zijn geïmplementeerd:

1. In de [Azure-portal](http://portal.azure.com), blader naar de lijst met resourcegroepen. Open de resourcegroep die u hebt gemaakt toen u de toepassing is geïmplementeerd.

   ![resourcegroep](./media/saas-multitenantdb-get-started-deploy/resource-group.png)

2. Klik op **catalogus mt&lt;gebruiker&gt;**  server. De catalogusserver bevat twee databases met de naam *tenantcatalog* en *basetenantdb*. De *basetenantdb* database is een lege sjabloon-database. Deze is gekopieerd voor het maken van een nieuwe tenant-database, of gebruikt voor meerdere tenants of slechts één tenant.

   ![catalogusserver](./media/saas-multitenantdb-get-started-deploy/catalog-server.png)

3. Ga terug naar de resourcegroep en selecteer de *tenants1 mt* server die de tenant-databases bevat.
    - De database tenants1 is een multitenant-database waarin de oorspronkelijke drie tenants, plus de eerste tenant die u hebt toegevoegd, worden opgeslagen. Het is geconfigureerd als een 50 DTU Standard database.
    - De **salixsalsa** database bevat de Salix Salsa dans plaats als de enige tenant. Dit is standaard geconfigureerd als de database van een Standard-editie met 50 dtu's.

   ![server voor tenants](./media/saas-multitenantdb-get-started-deploy/tenants-server.png)


## <a name="monitor-the-performance-of-the-database"></a>De prestaties van de database controleren

Als de load-generator actief is geweest gedurende enkele minuten, is genoeg telemetrie beschikbaar om te kijken naar de database bewakingsmogelijkheden ingebouwd in de Azure-portal.

1. Blader naar de **tenants1 mt&lt;gebruiker&gt;**  server en op **tenants1** Resourcegebruik voor de database met vier tenants erin weergeven. Elke tenant is onderworpen aan een enkel geval zware belasting van de load-generator:

   ![monitor tenants1](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   De DTU-gebruik grafiek illustreert mooi hoe een multitenant-database een onvoorspelbare werkbelasting over meerdere tenants kan ondersteunen. In dit geval wordt de load-generator een enkel geval belasting van ongeveer 30 dtu's toepassen op elke tenant. Deze laadbewerking gelijkstaat aan 60% gebruik van een 50 DTU-database. Pieken die groter zijn dan 60% zijn het resultaat van de belasting die wordt toegepast op meer dan één tenant op hetzelfde moment.

2. Blader naar de **tenants1 mt&lt;gebruiker&gt;**  server en op de **salixsalsa** database. U ziet het Resourcegebruik op deze database waarin slechts één tenant.

   ![salixsalsa database](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

De load-generator wordt een soortgelijke belasting aan elke tenant, ongeacht welke database elke tenant wordt toegepast. Met slechts één tenant in de **salixsalsa** database, kunt u zien dat de database een veel hogere belasting dan de database met verschillende tenants kan tolereren. 

#### <a name="resource-allocations-vary-by-workload"></a>Resource-toewijzingen, is afhankelijk van de werkbelasting

Soms een multitenant-database vereist meer bronnen voor goede prestaties dan een één-tenant-database, maar niet altijd. De optimale toewijzing van resources is afhankelijk van de kenmerken van bepaalde belasting voor de tenants in uw systeem.

De werkbelastingen die worden gegenereerd door het script van de generator load zijn alleen ter illustratie.

## <a name="additional-resources"></a>Aanvullende resources

- Zie voor meer informatie over multitenant SaaS-toepassingen, [ontwerppatronen voor multitenant SaaS-toepassingen](saas-tenancy-app-design-patterns.md).

- Voor meer informatie over elastische pools, Zie:
    - [Elastische pools helpen u te beheren en schalen van meerdere Azure SQL-databases](sql-database-elastic-pool.md)
    - [Uitbreiden met Azure SQL Database](sql-database-elastic-scale-introduction.md)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> - Klik hier voor meer informatie over het implementeren van de toepassing Wingtip Tickets SaaS multitenant Database.
> - Over de servers en databases die gezamenlijk de app.
> - Tenants zijn toegewezen aan hun gegevens met de *catalogus*.
> - Het inrichten van nieuwe tenants in een multitenant-database en de database voor één tenant.
> - Klik hier voor meer informatie over het gebruik van de groep van toepassingen voor het controleren van de activiteit van de tenant weergeven.
> - Klik hier voor meer informatie over het verwijderen van de voorbeeldresources om te stoppen gerelateerde facturering.

Nu proberen de [inrichten en catalogus zelfstudie](sql-database-saas-tutorial-provision-and-catalog.md).


<!--  Link references.

A [series of related tutorials] is available that build upon this initial deployment.
[link-wtp-overivew-jumpto-saas-tutorials-97j]: saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials

-->

[link-aka-ms-deploywtp-mtapp-52k]: http://aka.ms/deploywtp-mtapp


[link-azure-get-started-powershell-41q]: https://docs.microsoft.com/powershell/azure/get-started-azureps

[link-github-wingtip-multitenantdb-55g]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/



<!--  Image references.

[image-deploy-to-azure-blue-48d]: http://aka.ms/deploywtp-mtapp "Button for Deploy to Azure."
-->

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png "Knop voor het implementeren van Azure."


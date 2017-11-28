---
title: Een database van shard multitenant SaaS-app die gebruikmaakt van Azure SQL Database implementeren | Microsoft Docs
description: Implementeren en de shard Wingtip Tickets SaaS multitenant databasetoepassing, die u laat SaaS patronen zien met behulp van Azure SQL Database verkennen.
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: billgib;MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: sstein
ms.openlocfilehash: 1ef4355f7234bc6a534d21a57fa52b480983b99b
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application-that-uses-azure-sql-database"></a>Implementeren en een shard multitenant-toepassing die gebruikmaakt van Azure SQL Database verkennen

In deze zelfstudie maakt u implementeert en een SaaS multitenant database voorbeeldtoepassing met de naam Wingtip Tickets verkennen. De app Wingtip is ontworpen om hier functies van Azure SQL Database die de implementatie van de SaaS-scenario's vereenvoudigen.

Deze implementatie van Wingtips maakt gebruik van een patroon shard multitenant-database. De sharding is door de tenant-id. Gegevens van de tenant wordt gedistribueerd naar een bepaalde database volgens de tenant-id-waarden. Ongeacht hoeveel tenants een bepaalde database bevat, zijn alle databases die meerdere tenants in de zin dat het tabelschema een tenant-id bevatten. 

Dit patroon van een database kunt u een of meer tenants opslaan in elke shard of de database. U kunt voor de laagste kosten optimaliseren door elke database worden gedeeld door meerdere tenants. Of u kunt optimaliseren voor isolatie door elke tenant slechts één opslaan database. Uw keuze optimalisatie kan afzonderlijk worden gemaakt voor elke specifieke tenant. Uw keuze kan worden gemaakt wanneer de tenant voor het eerst wordt opgeslagen of kunt u later van gedachten verandert. De toepassing is ontworpen voor gebruik van goed in beide gevallen.

#### <a name="app-deploys-quickly"></a>App snel worden geïmplementeerd

De volgende Implementatiesectie biedt de **implementeren in Azure** knop. Wanneer de knop wordt ingedrukt, wordt de app Wingtip een vijf minuten later volledig geïmplementeerd. De app Wingtip wordt uitgevoerd in de Azure-cloud en Azure SQL Database gebruikt. Wingtip wordt geïmplementeerd in uw Azure-abonnement. U hebt volledige toegang tot het werken met de afzonderlijke toepassingsonderdelen.

De toepassing wordt geïmplementeerd met gegevens voor drie voorbeeld tenants. De tenants worden samen in een multitenant-database opgeslagen.

Iedereen kan de broncode C# en PowerShell downloaden voor Wingtip Tickets van [onze GitHub-opslagplaats][link-github-wingtip-multitenantdb-55g].

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

1. Klik op de volgende **implementeren in Azure** knop.
    - De Azure-portal wordt geopend met de sjabloon Wingtip Tickets SaaS-implementatie. De sjabloon vereist twee parameterwaarden: een naam voor een nieuwe resourcegroep en een waarde 'user' waarmee wordt onderscheiden van deze implementatie van andere implementaties van de app. De volgende stap biedt details voor het instellen van deze parameterwaarden.
        - Zorg ervoor dat de exacte waarden die u gebruikt, let u hebt ze later voor een configuratiebestand.

    [![Knop voor implementeren in Azure.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

2. Voer de vereiste parameterwaarden voor de implementatie.

    > [!IMPORTANT]
    > Sommige verificatie en de server firewall-instellingen zijn opzettelijk onbeveiligde ter bevordering van de demonstratie. Kies **een nieuwe resourcegroep maken**, en gebruik niet bestaande resourcegroepen, servers of groepen. Gebruik deze toepassing of alle bronnen die wordt gemaakt, niet voor productie. Verwijder deze resourcegroep wanneer u klaar bent om gerelateerde facturering te stoppen.

    Het is raadzaam gebruik alleen kleine letters, cijfers en afbreekstreepjes in uw resourcenamen.

    - Voor **resourcegroep** : Selecteer **nieuw**, en geef vervolgens een **naam** voor de resourcegroep (hoofdlettergevoelig).
        - Het is raadzaam dat alle letters in de naam van de resourcegroep kleine letters zijn.
        - Het is raadzaam dat u een streepje, gevolgd door uw initialen, gevolgd door een cijfer toevoegen: bijvoorbeeld *wingtip af1*.
        - Selecteer een **locatie** uit de vervolgkeuzelijst.
    - Voor **gebruiker** -het is raadzaam dat u een korte kiest **gebruiker** waarde, zoals uw initialen plus een cijfer: bijvoorbeeld *af1*.

3. **Implementeer de toepassing**.

    - Klik om de voorwaarden en bepalingen te accepteren.
    - Klik op **Kopen**.

4. Implementatiestatus controleren door te klikken op **meldingen**, namelijk het belpictogram rechts van het zoekvak. Implementatie van de app Wingtip duurt ongeveer vijf minuten.

   ![implementatie gelukt](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Download en blokkering opheffen van de management-scripts

Download de bron en het beheer toepassingsscripts tijdens de implementatie van de toepassing.

> [!IMPORTANT]
> Uitvoerbare inhoud (scripts, dll-bestanden) mogelijk geblokkeerd door Windows als zip-bestanden van een externe bron wordt gedownload en uitgepakt. Bij het uitpakken van de scripts uit een zipbestand, gebruikt u de volgende stappen uit om de blokkering van het ZIP-bestand voor het uitpakken van te. Het ZIP-bestand deblokkering zorgt u ervoor dat de scripts mogen worden uitgevoerd.

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

1. Open ...\\Learning Modules\\*UserConfig.psm1* in de *PowerShell ISE*.
2. Update *ResourceGroupName* en *naam* met de specifieke waarden voor uw implementatie (op regels 10 en 11 alleen).
3. Sla de wijzigingen op.

De waarden in dit bestand worden gebruikt door alle scripts, dus is het belangrijk dat ze juist zijn. Als u de app implementeren, zorg ervoor dat u een andere resource groep en gebruiker waarde kiezen. Werk vervolgens de UserConfig met de nieuwe waarden.

## <a name="run-the-application"></a>De toepassing uitvoeren

In de app worden venues gepresenteerd, zoals concertzalen, jazzclubs en stadions, die bepaalde evenementen organiseren. Plaatsen registreren als klanten (of tenants) van het platform Wingtip voor een eenvoudige manier om een lijst van gebeurtenissen en tickets verkopen. Elke venue krijgt een eigen web-app voor het beheren en vermelden van hun evenementen en het verkopen van tickets, onafhankelijk en gescheiden van andere tenants. Achter de wordt gegevens voor elke tenant standaard opgeslagen in een shard multitenant-database.

Een centraal **gebeurtenissen Hub** geeft een lijst met koppelingen voor de tenants in uw specifieke implementatie.

1. Open de *gebeurtenissen Hub* in uw webbrowser:
    - http://events.Wingtip. &lt;Gebruiker&gt;. trafficmanager.net &nbsp; *(vervangen door de waarde van de gebruiker van uw implementatie.)*

    ![events hub](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. Klik in de **Events Hub** op *Fabrikam Jazz Club*.

   ![Gebeurtenissen](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

Om te bepalen van de distributie van inkomende aanvragen en het gebruik van de app [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). De gebeurtenissen pagina's, die tenantspecifieke, bevatten de naam van de tenant in de URL. De URL's opnemen van uw specifieke waarde van de gebruiker ook en volgt deze indeling:

- http://events.Wingtip. &lt;Gebruiker&gt;.trafficmanager.net/*fabrikamjazzclub*
 
De app gebeurtenissen parseert de tenantnaam van de URL en hash-voor het maken van een sleutel voor toegang tot een catalogus met [shard kaart management](sql-database-elastic-scale-shard-map-management.md). De sleutel van de catalogus wordt toegewezen aan de locatie van de database van de tenant. De **gebeurtenissen Hub** geeft een lijst van de tenants die zijn geregistreerd in de catalogus. De **gebeurtenissen Hub** uitgebreide metagegevens in de catalogus gebruikt voor het ophalen van de tenantnaam die zijn gekoppeld aan elke wordt toegewezen aan de URL's samenstellen.

In een productieomgeving zou u meestal een DNS CNAME-record te maken [het internetdomein van een bedrijf wijzen](../traffic-manager/traffic-manager-point-internet-domain.md) aan traffic manager-profiel.

## <a name="start-generating-load-on-the-tenant-databases"></a>Workloads genereren voor de tenant-databases

Nu dat de app is geïmplementeerd, laten we aan de slag! De *Demo LoadGenerator* PowerShell-script voor een werkbelasting die wordt uitgevoerd voor elke tenant wordt gestart. De werkelijke belasting van veel SaaS-apps is doorgaans sporadisch onvoorspelbaar. Om te simuleren belasting, levert de generator belasting verdeeld over alle tenants. De belasting omvat willekeurige bursts op elke tenant optreedt op willekeurige intervallen. Het duurt enkele minuten voor het patroon load te komen, is het verstandig om te laten de generator uitvoeren voor ten minste drie tot vier minuten voor de bewaking van de belasting.

1. In de *PowerShell ISE*, open de... \\Learning-Modules\\hulpprogramma's\\*Demo LoadGenerator.ps1* script.
2. Druk op **F5** om het script uit te voeren en de load-generator te starten (gebruik voorlopig de standaardwaarden van het script).

> [!IMPORTANT]
> De *Demo LoadGenerator.ps1* script waarin de load-generator wordt uitgevoerd een andere PowerShell-sessie wordt geopend. De load-generator wordt uitgevoerd in deze sessie een voorgrond-taak die wordt aangeroepen achtergrond generatie load-taken, één voor elke tenant.

Nadat de voorgrond-taak wordt gestart, blijft deze in een status-taak wordt aangeroepen. De taak wordt gestart in de volgende taken voor alle nieuwe tenants die vervolgens zijn ingericht.

De PowerShell-sessie sluit stopt alle taken.

Mogelijk wilt de sessie laden generator voor het gebruik van andere parameterwaarden opnieuw starten. Zo ja, sluit u de PowerShell generatie sessie en voer de *Demo LoadGenerator.ps1*.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Een nieuwe tenant in te richten in de shard-database

De eerste implementatie omvat drie voorbeeld tenants in de *Tenants1* database. We gaan een andere tenant om te zien hoe dit heeft gevolgen voor de gedistribueerde toepassing maken. In deze stap maakt maken u snel een nieuwe tenant.

1. Open... \\Learning Modules\Provision en catalogus\\*Demo ProvisionTenants.ps1* in de *PowerShell ISE*.
2. Druk op **F5** het script uit te voeren (laat de standaardwaarden voor deze oefening).

   > [!NOTE]
   > Veel Wingtip Tickets SaaS-scripts gebruiken *$PSScriptRoot* om toe te staan de navigatie van mappen, of om aan te roepen andere scripts of voor het importeren van modules. Deze variabele alleen geëvalueerd wanneer het script wordt uitgevoerd als een geheel door te drukken **F5**.  Syntaxismarkering en een selectie actief (**F8**) kunnen leiden tot fouten, indrukt **F5** wanneer het uitvoeren van scripts.

De nieuwe tenant voor rode esdoorn race wordt toegevoegd aan de *Tenants1* database en geregistreerd in de catalogus. De nieuwe tenant de ticket verkopen *gebeurtenissen* site in uw browser geopend:

![Nieuwe tenant](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Vernieuw de *gebeurtenissen Hub* en de nieuwe tenant wordt nu weergegeven in de lijst.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Een nieuwe tenant in de eigen database inrichten

De shard multitenant-model kunt u kiezen of voor het inrichten van een nieuwe tenant in een database die andere tenants bevat, of voor het inrichten van de tenant in een database met een eigen. Een tenant in de eigen voordelen van de gegevens die zijn geïsoleerd van de gegevens van andere tenants hebben. De isolatie kunt u voor het beheren van de prestaties van deze tenant onafhankelijk van andere tenants. Het is ook eenvoudiger te de gegevens herstellen naar een eerder tijdstip voor de geïsoleerde tenant. U kunt kiezen om de gratis proefversie of reguliere klanten in een multitenant-database en premium-klanten in afzonderlijke databases plaatst. Als u een groot aantal databases dat elke tenant slechts één bevatten maakt, kunt u ze allemaal gezamenlijk beheren in een elastische pool resourcekosten optimaliseren.  

Nu voorzien we in een andere tenant dit moment in een eigen database.

1. In... \\Learning-Modules\\inrichten en catalogus\*Demo-ProvisionTenants.ps1* wijzigen *$TenantName* naar **Salix Salsa**, *$VenueType*  naar **dans** en *$Scenario* naar **2**.

2. Druk op **F5** het script opnieuw uitvoeren.
    - Deze F5 drukt u op inricht de nieuwe tenant in een aparte database. De database en de tenant worden geregistreerd in de catalogus. De browser wordt geopend op de pagina gebeurtenissen van de tenant.

   ![Pagina Salix Salsa-gebeurtenissen](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Ga naar de onderkant van de pagina. Er in de banner ziet u de naam van de database waarin de gegevens van de tenant is opgeslagen.

3. Vernieuw de Hub gebeurtenissen en de twee nieuwe tenants wordt nu weergegeven in de lijst.



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

Als de load-generator actief is geweest gedurende enkele minuten, is genoeg telemetrie beschikbaar om te kijken naar enkele van de database die is ingebouwd in de portal bewakingsmogelijkheden starten.

1. Blader naar de **tenants1 mt&lt;gebruiker&gt;**  server en op **tenants1** Resourcegebruik voor de database met vier tenants erin weergeven. Elke tenant is onderworpen aan een enkel geval zware belasting van de load-generator:

   ![monitor tenants1](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   De DTU-gebruik grafiek illustreert mooi hoe een multitenant-database een onvoorspelbare werkbelasting over meerdere tenants kan ondersteunen. In dit geval wordt de load-generator een enkel geval belasting van ongeveer 30 dtu's toepassen op elke tenant. Deze laadbewerking gelijkstaat aan 60% gebruik van een 50 DTU-database. Pieken die groter zijn dan 60% zijn het resultaat van de belasting die wordt toegepast op meer dan één tenant op hetzelfde moment.

2. Blader naar de **tenants1 mt&lt;gebruiker&gt;**  server en op de **salixsalsa** database om het Resourcegebruik op deze database waarin slechts één tenant weer te geven.

   ![salixsalsa database](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

De load-generator wordt een soortgelijke belasting aan elke tenant, ongeacht welke database elke tenant wordt toegepast. Met slechts één tenant in de **salixsalsa** database, kunt u zien dat de database een veel hogere belasting dan de database met verschillende tenants kan tolereren. 

> [!NOTE]
> De belasting die wordt gemaakt door de load-generator zijn uitsluitend ter illustratie.  De resources die zijn toegewezen aan meerdere tenant- en één databases en het aantal tenants dat u in een multitenant-database hosten kunt, is afhankelijk van de werkelijke werkbelasting patronen in uw toepassing.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> - Het implementeren van de toepassing Wingtip Tickets SaaS multitenant Database
> - Over de servers en databases die gezamenlijk de app.
> - Tenants worden via de *catalogus* toegewezen aan hun gegevens
> - Het inrichten van nieuwe tenants in een multitenant-database en de database voor één tenant.
> - Poolgebruik bekijken om de activiteit in een tenant te controleren
> - Voorbeeldresources verwijderen om gerelateerde facturering te stoppen

Nu proberen de [inrichten en catalogus zelfstudie](sql-database-saas-tutorial-provision-and-catalog.md).



## <a name="additional-resources"></a>Aanvullende bronnen

- Informatie over SaaS-toepassingen voor meerdere tenants vindt u in [*Design patterns for multi-tenant SaaS applications*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications) (Ontwerppatronen voor SaaS-toepassingen voor meerdere tenants).








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

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png


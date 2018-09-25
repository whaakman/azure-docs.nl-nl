---
title: Implementeren van een database voor shard multitenant SaaS-app die gebruikmaakt van Azure SQL Database | Microsoft Docs
description: Toepassing implementeren en verkennen de shard Wingtip Tickets SaaS multitenant-database, die u laat SaaS-patronen zien met behulp van Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib, stein
manager: craigg
ms.date: 04/02/2018
ms.openlocfilehash: b91960920f0181939e634a221080d493fb8cea63
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47056655"
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application-that-uses-azure-sql-database"></a>Een shard multitenant-toepassing die gebruikmaakt van Azure SQL Database implementeren en verkennen

In deze zelfstudie, implementeren en een voorbeeld van multitenant SaaS-toepassing met de naam Wingtip Tickets verkennen. De Wingtip Tickets-app is ontworpen om u te presenteren functies van Azure SQL Database die de uitvoering van de SaaS-scenario's te vereenvoudigen.

Deze implementatie van de Wingtip Tickets-app maakt gebruik van een patroon shard multitenant-database. De sharding is door de tenant-id. Gegevens van de tenant wordt gedistribueerd naar een bepaalde database op basis van de tenant-id-waarden. 

Dit patroon database kunt u voor het opslaan van een of meer tenants in elke shard of de database. U kunt voor de laagste kosten optimaliseren door elk van deze databases worden gedeeld door meerdere tenants. Of u kunt optimaliseren voor isolatie door elke database opslaan slechts één tenant. Uw keuze optimalisatie kan onafhankelijk van elkaar worden gemaakt voor elke specifieke tenant. Uw keuze kan worden gemaakt wanneer de tenant voor het eerst wordt opgeslagen, of u kunt u doen later wijzigen. De toepassing is ontworpen voor gebruik van goed in beide gevallen.

#### <a name="app-deploys-quickly"></a>App worden snel geïmplementeerd

De app wordt uitgevoerd in de Azure-cloud en maakt gebruik van Azure SQL Database. De volgende Implementatiesectie biedt de blauwe **implementeren in Azure** knop. Wanneer de knop wordt gedrukt, wordt de app volledig aan uw Azure-abonnement binnen vijf minuten geïmplementeerd. U hebt volledige toegang tot het werken met de afzonderlijke toepassingsonderdelen.

De toepassing wordt geïmplementeerd met gegevens voor drie voorbeeldtenants. De tenants worden samen in een multitenant-database opgeslagen.

Iedereen kan de broncode voor C# en PowerShell downloaden voor Wingtip Tickets van [de GitHub-opslagplaats][link-github-wingtip-multitenantdb-55g].

#### <a name="learn-in-this-tutorial"></a>Meer informatie in deze zelfstudie

> [!div class="checklist"]
> - Klik hier voor meer informatie over het implementeren van de Wingtip Tickets SaaS-toepassing.
> - Waar de broncode van de toepassing en scripts voor downloaden.
> - Over de servers en databases waaruit de app.
> - Hoe tenants worden toegewezen aan hun gegevens met de *catalogus*.
> - Klik hier voor meer informatie over het inrichten van een nieuwe tenant.
> - Klik hier voor meer informatie over het bewaken van activiteit in een tenant in de app.

Er is een reeks gerelateerde zelfstudies beschikbaar die voortbouwt op deze initiële implementatie. De zelfstudies verkennen een bereik van de SaaS-ontwerp- en -patronen. Wanneer u met de zelfstudies werkt, wordt u aangeraden te doorlopen de bijgeleverde scripts om te zien hoe de verschillende SaaS-patronen worden geïmplementeerd.

## <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

- De nieuwste Azure PowerShell is geïnstalleerd. Zie voor meer informatie, [aan de slag met Azure PowerShell][link-azure-get-started-powershell-41q].

## <a name="deploy-the-wingtip-tickets-app"></a>Implementeren van de Wingtip Tickets-app

#### <a name="plan-the-names"></a>De namen van plan bent

In de stappen van deze sectie bieden u een *gebruiker* waarde die wordt gebruikt om ervoor te zorgen resourcenamen wereldwijd uniek zijn en een naam voor de *resourcegroep* waarin alle resources die zijn gemaakt door een implementatie van de app. Voor een persoon met de naam *Anne Finley*, wordt aangeraden:
- *Gebruiker:* **af1***(haar initialen plus een cijfer.   Gebruik een andere waarde (bijvoorbeeld af2) als u de app een tweede keer implementeren.)*
- *Resourcegroep:* **wingtip-mt-af1** *(wingtip-mt geeft aan dat dit is de shard multitenant-app. Toevoegen van de gebruiker de naam af1 overeenkomt met de naam van de resourcegroep met de namen van de resources die deze bevat.)*

Kies de namen van de nu en schrijf ze op. 

#### <a name="steps"></a>Stappen

1. Klik op de volgende blauwe **implementeren in Azure** knop.
    - De Azure-portal wordt het geopend met een sjabloon voor de implementatie van de Wingtip Tickets SaaS.

    [![Knop voor het implementeren in Azure.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

1. Voer de vereiste parameterwaarden voor de implementatie.

    > [!IMPORTANT]
    > Voor deze demonstratie gebruik niet een reeds bestaande resourcegroepen, servers of pools. Kies in plaats daarvan **maken van een nieuwe resourcegroep**. Verwijder deze resourcegroep wanneer u klaar bent om gerelateerde facturering te stoppen.
    > Gebruik deze toepassing, of eventuele resources die zijn gemaakt, niet voor productie. Sommige aspecten van verificatie en de server firewall-instellingen zijn opzettelijk onveilig zijn in de app om de demonstratie mogelijk te maken.

    - Voor **resourcegroep** : Selecteer **nieuw**, en geef vervolgens een **naam** voor de resourcegroep (hoofdlettergevoelig).
        - Selecteer een **locatie** uit de vervolgkeuzelijst.
    - Voor **gebruiker** -het is raadzaam dat u een korte **gebruiker** waarde.

1. **Implementeer de toepassing**.

    - Klik op om de voorwaarden en bepalingen te accepteren.
    - Klik op **Kopen**.

1. Implementatiestatus controleren door te klikken op **meldingen**, dit is het belpictogram aan de rechterkant van het zoekvak. Implementeren van de Wingtip-app duurt ongeveer vijf minuten.

   ![implementatie gelukt](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Downloaden en de scripts voor de blokkering opheffen

Terwijl de toepassing wordt geïmplementeerd, downloadt u de toepassing source code en scripts.

> [!NOTE]
> Uitvoerbare inhoud (scripts, dll's) mogelijk geblokkeerd door Windows als zip-bestanden worden gedownload vanuit een externe bron en uitgepakt. Bij het uitpakken van de scripts in een zip-bestand, gebruikt u de volgende stappen uit om de blokkering van het ZIP-bestand voor het extraheren van te. Door het deblokkeren van het ZIP-bestand, zorgt u ervoor dat de scripts mogen worden uitgevoerd.

1. Blader naar [de WingtipTicketsSaaS MultiTenantDb GitHub-opslagplaats](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb).
2. Klik op **klonen of downloaden**.
3. Klik op **ZIP downloaden** en sla het bestand.
4. Met de rechtermuisknop op de **WingtipTicketsSaaS-MultiTenantDb-master.zip** bestand en selecteer **eigenschappen**.
5. Op de **algemene** tabblad **opheffen van blokkeringen**, en klikt u op **toepassen**.
6. Klik op **OK**.
7. Pak de bestanden.

De scripts bevinden zich in de *... \\WingtipTicketsSaaS-MultiTenantDb-master\\Learning Modules\\*  map.

## <a name="update-the-configuration-file-for-this-deployment"></a>Het configuratiebestand voor deze implementatie bijwerken

Voordat u alle scripts, stelt u de *resourcegroep* en *gebruiker* waarden in **UserConfig.psm1**. Deze variabelen ingesteld op dezelfde waarden die u tijdens de implementatie hebt ingesteld.

1. Open... \\Learning Modules\\*UserConfig.psm1* in de *PowerShell ISE*.
2. Update *ResourceGroupName* en *naam* met de specifieke waarden voor uw implementatie (op regel 10 en 11 alleen).
3. Sla de wijzigingen op.

De waarden in dit bestand worden gebruikt door de scripts, dus is het belangrijk dat ze juist zijn. Als u de app opnieuw implementeren, moet u verschillende waarden voor de gebruiker en de resourcegroep. Werk het bestand UserConfig.psm1 opnieuw vervolgens met de nieuwe waarden.

## <a name="run-the-application"></a>De toepassing uitvoeren

De tenants zijn in de app Wingtip plaatsen. Een locatie die kan worden concert hall, een club sport, of op een andere locatie die als host fungeert voor gebeurtenissen. De venues registreren in Wingtip als klanten en een tenant-id is gegenereerd voor elke locatie. Elke venue geeft een lijst van de toekomstige gebeurtenissen in Wingtip, zodat het publiek tickets op de gebeurtenissen kunt aanschaffen.

Elke venue krijgt een eigen web-app te vermelden van hun evenementen en tickets kunnen verkopen. Elke web-app is onafhankelijk en gescheiden van andere tenants. Intern in Azure SQL Database, elk van de gegevens voor elke tenant standaard in een shard multitenant-database opgeslagen wordt. Alle gegevens worden gemarkeerd met de tenant-id.

Een centraal **Events Hub** webpagina bevat een lijst met koppelingen naar de tenants in uw specifieke implementatie. Gebruik de volgende stappen om de **Events Hub** webpagina en een afzonderlijke web-app:

1. Open de **Events Hub** in uw webbrowser:
    - http://events.wingtip-mt.&lt; gebruiker&gt;. trafficmanager.net &nbsp; *(Vervang &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie.)*

    ![events hub](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. Klik in de **Events Hub** op **Fabrikam Jazz Club**.

   ![Gebeurtenissen](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Voor het beheren van de distributie van inkomende aanvragen, de Wingtip-app gebruikt [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). De pagina gebeurtenissen voor elke tenant bevat de naam van de tenant in de URL. Elke URL bevat ook de waarde van uw specifieke gebruiker. Elke URL gehoorzaamt aan de indeling weergegeven met behulp van de volgende stappen uit:

- http://events.wingtip-mt.&lt; gebruiker&gt;.trafficmanager.net/*fabrikamjazzclub*

1. De app parseert de tenantnaam van de URL. Naam van de tenant is *fabrikamjazzclub* in het voorgaande voorbeeld-URL.
2. De app vervolgens de naam van de tenant te maken van een sleutel voor toegang tot een catalogus met hashes [shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md).
3. De app zoekt naar de sleutel in de catalogus, en haalt de bijbehorende locatie van de database van de tenant.
4. De app maakt gebruik van de locatie-informatie te vinden en openen van de ene database waarin alle gegevens voor de tenant.

#### <a name="events-hub"></a>Events Hub

1. De **Events Hub** geeft een lijst van alle tenants die zijn geregistreerd in de catalogus en hun plaatsen.
2. De **Events Hub** uitgebreide metagegevens in de catalogus gebruikt voor het ophalen van de tenantnaam die zijn gekoppeld aan elke toewijzing te maken van de URL's.

In een productieomgeving moet u doorgaans een DNS CNAME-record maken [internetdomein van een bedrijf](../traffic-manager/traffic-manager-point-internet-domain.md) naar de traffic manager-profiel.

## <a name="start-generating-load-on-the-tenant-databases"></a>Workloads genereren voor de tenant-databases

Nu dat de app is geïmplementeerd, laten we om te werken. De *Demo-LoadGenerator* PowerShell-script wordt een workload voor elke tenant wordt gestart. De werkelijke belasting van veel SaaS-apps is meestal sporadisch en onvoorspelbaar. Als u wilt dit type load simuleren, produceert de generator een belasting verdeeld over alle tenants. De belasting bevat willekeurige pieken op elke tenant optreden met willekeurige intervallen. Het duurt enkele minuten voor het patroon load te geven, dus het is raadzaam om te laten de ten minste drie of vier minuten voordat de bewaking van de load-generator.

1. In de *PowerShell ISE*, open de... \\Learning Modules\\hulpprogramma's voor\\*Demo-LoadGenerator.ps1* script.
2. Druk op **F5** om het script uit te voeren en de load-generator te starten (gebruik voorlopig de standaardwaarden van het script).

De *Demo-LoadGenerator.ps1* -script wordt geopend voor een andere PowerShell-sessie waarop de load-generator wordt uitgevoerd. De load-generator wordt uitgevoerd in deze sessie als een taak van voorgrond die load genereren achtergrondtaken, één voor elke tenant aanroept.

Nadat de taak van voorgrond is gestart, wordt deze in een status voor het aanroepen van de taak blijft. De taak wordt gestart als u meer achtergrondtaken voor alle nieuwe tenants die later zijn ingericht.

De PowerShell-sessie wordt gesloten, stopt alle taken.

Mogelijk wilt u de sessie van de load-generator voor het gebruik van andere parameterwaarden opnieuw starten. Als dit het geval is, sluit u de PowerShell generatie sessie en voer de *Demo-LoadGenerator.ps1*.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Een nieuwe tenant inrichten in de shard-database

De eerste implementatie omvat drie voorbeeldtenants in de *Tenants1* database. We gaan maken van een andere tenant en bekijk de gevolgen voor de geïmplementeerde toepassing. In deze stap maakt u drukt u op een toets om te maken van een nieuwe tenant:

1. Open... \\Learning Modules\\inrichten en catalogiseren\\*Demo-ProvisionTenants.ps1* in de *PowerShell ISE*.
2. Druk op **F5** (niet **F8**) het script uit te voeren (gebruik voorlopig de standaardwaarden).

   > [!NOTE]
   > U moet de PowerShell-scripts uitvoeren door alleen maar te drukken de **F5** sleutel, niet door te drukken **F8** om uit te voeren van een geselecteerde onderdeel van het script. Het probleem met **F8** is dat de *$PSScriptRoot* variabele wordt niet geëvalueerd. Deze variabele is vereist door veel scripts om te navigeren van mappen, andere scripts aanroepen en -modules importeren.

De nieuwe tenant voor Red Maple Racing wordt toegevoegd aan de *Tenants1* database en geregistreerd in de catalogus. De nieuwe tenant de verkopen van tickets **gebeurtenissen** site wordt geopend in uw browser:

![Nieuwe tenant](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Vernieuw de **Events Hub**, en de nieuwe tenant wordt nu weergegeven in de lijst.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Een nieuwe tenant in een eigen database inrichten

De shard multitenant-model kunt u kiezen of ze een nieuwe tenant inrichten in een database met andere tenants of in een database met een eigen. Een tenant geïsoleerd in een eigen database, kan de volgende voordelen:
- De prestaties van de database van de tenant kan worden beheerd zonder de noodzaak te manipuleren met de behoeften van andere tenants.
- Indien nodig, kan de database worden hersteld naar een eerder tijdstip, omdat er geen andere tenants worden niet beïnvloed.

U kunt kiezen om klanten van de gratis proefversie of klanten van economie, in databases van meerdere tenants. U kunt elke premium-tenant plaatsen in een eigen toegewezen databaseserver. Als u veel databases met slechts één tenant maakt, kunt u ze allemaal gezamenlijk beheren in een elastische pool het optimaliseren van resourcekosten.

We richten vervolgens dit moment in een eigen database een andere tenant:

1. In... \\Learning Modules\\inrichten en catalogiseren\\*Demo-ProvisionTenants.ps1*, wijzigen *$TenantName* naar **Salix Salsa**, *$VenueType* naar **dance** en *$Scenario* naar **2**.

2. Druk op **F5** het script opnieuw uitvoeren.
    - Dit **F5** druk op de nieuwe tenant in een afzonderlijke database inricht. De database en de tenant worden geregistreerd in de catalogus. De browser opent vervolgens naar de pagina gebeurtenissen van de tenant.

   ![Pagina Salix Salsa-gebeurtenissen](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Ga naar de onderkant van de pagina. Er in de banner ziet u de naam van de database waarin de gegevens van de tenant is opgeslagen.

3. Vernieuw de **Events Hub** en de twee nieuwe tenants wordt nu weergegeven in de lijst.

## <a name="explore-the-servers-and-tenant-databases"></a>Verken de servers en tenant-databases

Nu kijken we naar enkele van de resources die zijn geïmplementeerd:

1. In de [Azure-portal](http://portal.azure.com), blader naar de lijst met resourcegroepen. Open de resourcegroep die u hebt gemaakt toen u de toepassing is geïmplementeerd.

   ![resourcegroep](./media/saas-multitenantdb-get-started-deploy/resource-group.png)

2. Klik op **catalogus mt&lt;gebruiker&gt;**  server. De catalogusserver bevat twee databases met de naam *tenantcatalog* en *basetenantdb*. De *basetenantdb* database is een lege sjabloon-database. Het is voor het maken van een nieuwe tenantdatabase gekopieerd of voor meerdere tenants of slechts één tenant gebruikt.

   ![catalogusserver](./media/saas-multitenantdb-get-started-deploy/catalog-server.png)

3. Ga terug naar de resourcegroep en selecteer de *tenants1-mt* server die de tenantdatabases bevat.
    - De tenants1-database is een multitenant-database waarin de oorspronkelijke drie tenants, plus de eerste tenant die u hebt toegevoegd, worden opgeslagen. Deze is geconfigureerd als een 50 DTU Standard-database.
    - De **salixsalsa** database bevat de Salix Salsa instel-locatie als de enige tenant. Dit is standaard geconfigureerd als een Standard edition-database met 50 dtu's.

   ![tenants-server](./media/saas-multitenantdb-get-started-deploy/tenants-server.png)


## <a name="monitor-the-performance-of-the-database"></a>De prestaties van de database controleren

Als de load-generator actief is geweest gedurende enkele minuten, is voldoende telemetrie beschikbaar om te kijken naar de database bewakingsmogelijkheden die is ingebouwd in de Azure-portal.

1. Blader naar de **tenants1-mt&lt;gebruiker&gt;**  server en op **tenants1** om Resourcegebruik voor de database met vier tenants in deze weer te geven. Elke tenant is onderworpen aan een sporadisch zware belasting van de load-generator:

   ![monitor tenants1](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   De DTU-grafiek voor Databasegebruik illustreert mooi hoe een multitenant-database een onvoorspelbare werkbelasting voor veel tenants kan ondersteunen. In dit geval wordt de load-generator een sporadisch belasting van ongeveer 30 dtu's toepassen op elke tenant. Deze belasting is gelijk aan 60% gebruik van een 50 DTU-database. Pieken die groter is dan 60% zijn het resultaat van taakverdeling wordt toegepast op meer dan één tenant op hetzelfde moment.

2. Blader naar de **tenants1-mt&lt;gebruiker&gt;**  server en op de **salixsalsa** database. U kunt het Resourcegebruik zien op deze database slechts één tenant bevat.

   ![salixsalsa database](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

De load-generator is een vergelijkbare belasting van toepassing op elke tenant, ongeacht welke database elke tenant zich bevindt. Met slechts één tenant in de **salixsalsa** -database, kunt u zien dat de database een veel hoger belasting dan de database met meerdere tenants kan tolereren. 

#### <a name="resource-allocations-vary-by-workload"></a>Resourcetoewijzingen zijn afhankelijk van werkbelasting

Soms een multitenant-database vereist voor andere bronnen voor goede prestaties dan een database met één tenant, maar niet altijd. De optimale toewijzing van resources is afhankelijk van de kenmerken van de specifieke werkbelasting voor de tenants in uw systeem.

De werkbelastingen die worden gegenereerd door de load-generator script zijn alleen ter illustratie.

## <a name="additional-resources"></a>Aanvullende resources

- Zie voor meer informatie over SaaS-toepassingen met meerdere tenants, [ontwerppatronen voor SaaS-toepassingen met meerdere tenants](saas-tenancy-app-design-patterns.md).

- Voor meer informatie over elastische pools, Zie:
    - [Elastische pools kunt u beheren en schalen van meerdere Azure SQL-databases](sql-database-elastic-pool.md)
    - [Uitbreiden met Azure SQL Database](sql-database-elastic-scale-introduction.md)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> - Klik hier voor meer informatie over het implementeren van de toepassing Wingtip Tickets SaaS multitenant-Database.
> - Over de servers en databases waaruit de app.
> - Tenants zijn toegewezen aan hun gegevens met de *catalogus*.
> - Klik hier voor meer informatie over het inrichten van nieuwe tenants in een multitenant-database en één tenant-database.
> - Klik hier voor meer informatie over het gebruik van de groep van toepassingen voor het controleren van activiteit in een tenant weergeven.
> - Klik hier voor meer informatie over het verwijderen van de voorbeeldresources om gerelateerde facturering te stoppen.

Probeer nu de [inrichten en catalogiseren zelfstudie](sql-database-saas-tutorial-provision-and-catalog.md).


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

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png "Knop voor het implementeren op Azure."


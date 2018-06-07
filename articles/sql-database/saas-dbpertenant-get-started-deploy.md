---
title: Zelfstudie voor database per tenant SaaS - Azure SQL Database | Microsoft Docs
description: Implementeren en de multitenant Wingtip Tickets SaaS-toepassing die u laat zien van het patroon database per tenant en andere SaaS-patronen met behulp van Azure SQL Database verkennen.
keywords: zelfstudie sql-database
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: genemi
ms.openlocfilehash: 95d4fc1886e16785b6de8f3a395b218b66d193ff
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34645358"
---
# <a name="deploy-and-explore-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Implementeren en een multitenant SaaS-app die gebruikmaakt van het patroon database per tenant met SQL Database verkennen

In deze zelfstudie, implementeren en Verken de Wingtip Tickets SaaS-database per tenant-toepassing (Wingtip). De app gebruikmaakt van een database per tenant-patroon voor het opslaan van de gegevens van meerdere tenants. De app is ontworpen om hier functies van Azure SQL Database die het inschakelen van SaaS-scenario's vereenvoudigen.

Vijf minuten nadat u hebt geselecteerd **implementeren in Azure**, hebt u een multitenant SaaS-toepassing. De app bevat een SQL-database die wordt uitgevoerd in de cloud. De app wordt geïmplementeerd met drie voorbeeld-tenants, elk met een eigen database. Alle databases zijn geïmplementeerd in een elastische SQL-groep. De app wordt geïmplementeerd op uw Azure-abonnement. U hebt volledige toegang om te verkennen en werken met de afzonderlijke onderdelen van de app. De toepassing C#-broncode en de management-scripts zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant GitHub-repo-][github-wingtip-dpt].

In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> - Klik hier voor meer informatie over het implementeren van de Wingtip SaaS-toepassing.
> - Waar u de toepassingsbron en het beheer van scripts.
> - Over de servers, pools en databases die gezamenlijk de app.
> - Hoe tenants zijn toegewezen aan hun gegevens met de *catalogus*.
> - Het inrichten van een nieuwe tenant.
> - Het controleren van de tenant-activiteit in de app.

Een [reeks zelfstudies gerelateerde](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) biedt wilt experimenteren met verschillende SaaS-ontwerp- en patronen. De zelfstudies bouwen afgezien van deze initiële implementatie. Wanneer u de zelfstudies gebruikt, kunt u de opgegeven scripts om te zien hoe de andere SaaS-patronen zijn geïmplementeerd kunt bekijken. De scripts laten zien hoe de functies van SQL-Database de ontwikkeling van SaaS-toepassingen vereenvoudigen.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt voltooid, moet dat Azure PowerShell is geïnstalleerd. Zie voor meer informatie [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-saas-application"></a>De Wingtip Tickets SaaS-toepassing implementeren

#### <a name="plan-the-names"></a>De namen plannen

In de stappen van deze sectie bieden u een waarde die wordt gebruikt voor Zorg ervoor dat resourcenamen globaal uniek zijn. U ook opgeven een naam voor de resourcegroep met de resources die zijn gemaakt door een implementatie van de app. We raden voor fictieve personen met de naam Anne Finley:

- **Gebruiker**: *af1* bestaat uit de Anne Finley initialen plus een cijfer. Als u een tweede keer de app implementeren, gebruikt u een andere waarde. Een voorbeeld is af2.
- **Resourcegroep**: *wingtip-dpt-af1* geeft aan dat dit de app database per tenant. Toevoegen van de gebruiker de naam van af1 voor de bijbehorende naam van de resourcegroep met de namen van de resources die deze bevat.

Kies nu uw namen en schrijf ze op. 

#### <a name="steps"></a>Stappen

1. Als de sjabloon Wingtip Tickets SaaS-database per tenant-implementatie in de Azure portal, schakelt **implementeren in Azure**.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. Voer waarden in de sjabloon voor de vereiste parameters.

    > [!IMPORTANT]
    > Sommige firewalls verificatie en de server zijn opzettelijk onbeveiligde voor demonstratiedoeleinden. U wordt aangeraden dat u een nieuwe resourcegroep maken. Gebruik geen bestaande resourcegroepen, servers of groepen. Deze toepassing, scripts of alle geïmplementeerde resources niet gebruiken voor productie. Deze resourcegroep verwijderen wanneer u klaar bent met de toepassing stoppen gerelateerde facturering.

    - **Resourcegroep**: Selecteer **nieuw**, en geef de unieke naam die u hebt gekozen eerder voor de resourcegroep. 
    - **Locatie**: Selecteer een locatie in de vervolgkeuzelijst.
    - **Gebruiker**: Gebruik de waarde van de gebruiker u eerder hebt gekozen.

3. Implementeer de toepassing.

    a. Selecteer om de voorwaarden en bepalingen te accepteren.

    b. Selecteer **aankoop**.

4. Om de implementatiestatus controleren, selecteer **meldingen** (het belpictogram rechts van het zoekvak). Implementatie van de app Wingtip Tickets SaaS duurt ongeveer vijf minuten.

   ![Implementatie is voltooid](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Downloaden en de Wingtip Tickets management scripts deblokkeren

Terwijl de toepassing wordt geïmplementeerd, download u de bron en het beheer-scripts.

> [!IMPORTANT]
> Uitvoerbare inhoud (scripts en dll-bestanden) mogelijk geblokkeerd door Windows als ZIP-bestanden van een externe bron wordt gedownload en uitgepakt. Volg de stappen om de blokkering van het ZIP-bestand voordat u de scripts uitpakken. Deblokkeren zorgt dat de scripts mogen worden uitgevoerd.

1. Blader naar de [WingtipTicketsSaaS DbPerTenant GitHub-repo-][github-wingtip-dpt].
2. Selecteer **klonen of downloaden**.
3. Selecteer **ZIP downloaden**, en sla het bestand.
4. Met de rechtermuisknop op de **WingtipTicketsSaaS-DbPerTenant-master.zip** bestand en selecteer vervolgens **eigenschappen**.
5. Op de **algemene** tabblad **blokkering** > **toepassen**.
6. Selecteer **OK**, en pak de bestanden

Scripts bevinden zich in de... \\WingtipTicketsSaaS DbPerTenant master\\map Learning-Modules.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Het configuratiebestand van de gebruiker voor deze implementatie bijwerken

Voordat u de scripts uitvoert, moet u de resource groeps- en de waarden in het configuratiebestand van de gebruiker bijgewerkt. Deze variabelen ingesteld op de waarden die u tijdens de implementatie gebruikt.

1. Open in de PowerShell ISE... \\Learning-Modules\\**UserConfig.psm1** 
2. Update **ResourceGroupName** en **naam** met de specifieke waarden voor uw implementatie (op regels 10 en 11 alleen).
3. Sla de wijzigingen op.

Deze waarden wordt verwezen in bijna elk script.

## <a name="run-the-application"></a>De toepassing uitvoeren

De app gepresenteerd plaatsen die als host fungeren van gebeurtenissen. U wilt typen zijn concertzalen jazz clubs en sportverenigingen. In Wingtip-Tickets worden plaatsen geregistreerd als tenants. Een tenant heeft een u wilt een eenvoudige manier aan de lijst met gebeurtenissen en tickets verkopen aan hun klanten. Elke locatie vast Hiermee haalt u een aangepaste website voor een lijst met hun gebeurtenissen en tickets verkopen.

Intern opgehaald elke tenant in de app, een SQL-database die is geïmplementeerd in een elastische SQL-groep.

Een centraal **gebeurtenissen Hub** pagina bevat een lijst met koppelingen voor de tenants in uw implementatie.

1. De URL gebruiken om te openen van de Hub gebeurtenissen in uw webbrowser: http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net. Vervang &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie.

    ![Gebeurtenissen Hub](media/saas-dbpertenant-get-started-deploy/events-hub.png)

2. Selecteer **Fabrikam Jazz vereniging** in de Hub gebeurtenissen.

    ![Gebeurtenissen](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Maakt gebruik van de toepassing Wingtip [ *Azure Traffic Manager* ](../traffic-manager/traffic-manager-overview.md) om de verspreiding van binnenkomende aanvragen. De URL voor toegang tot de pagina gebeurtenissen voor een specifieke tenant gebruikt de volgende indeling:

- http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/fabrikamjazzclub

    De onderdelen van de voorgaande indeling worden in de volgende tabel beschreven.

    | URL-onderdeel        | Beschrijving       |
    | :-------------- | :---------------- |
    | http://events.wingtip-dpt | De onderdelen van de gebeurtenissen van de app Wingtip.<br /><br /> *-dpt* onderscheidt de *database per tenant* implementatie van Wingtip Tickets uit andere implementaties. Voorbeelden zijn de *zelfstandige* app per tenant (*-sa*) of *multitenant database* (*- mt*) implementaties. |
    | .  *&lt;gebruiker&gt;* | *af1* in het voorbeeld. |
    | .trafficmanager.net/ | Traffic Manager basis-URL. |
    | fabrikamjazzclub | De tenant met de naam Fabrikam Jazz vereniging identificeert. |
    | &nbsp; | &nbsp; |

* De tenantnaam wordt door de app gebeurtenissen van de URL geparseerd.
* Naam van de tenant wordt gebruikt om een sleutel te maken.
* De sleutel wordt gebruikt voor toegang tot de catalogus voor de locatie van de tenant-database.
    - De catalogus wordt geïmplementeerd met behulp van *shard kaart management*.
* De Hub gebeurtenissen gebruikt uitgebreide metagegevens in de catalogus voor het maken van de URL van de pagina van de lijst van gebeurtenissen voor elke tenant.

In een productieomgeving, maakt u gewoonlijk een DNS CNAME-record [ *het internetdomein van een bedrijf wijzen* ](../traffic-manager/traffic-manager-point-internet-domain.md) in de Traffic Manager-DNS-naam.

## <a name="start-generating-load-on-the-tenant-databases"></a>Workloads genereren voor de tenant-databases

Nu dat de app is geïmplementeerd, laten we aan de slag.

De *Demo LoadGenerator* PowerShell-script wordt gestart van een werkbelasting die wordt uitgevoerd op alle databases van de tenant. De werkelijke belasting van veel SaaS-apps is het sporadisch en onvoorspelbaar. Om te simuleren belasting, levert de generator van een werklast met willekeurige pieken of bursts van activiteiten op elke tenant. De bursts optreden op willekeurige intervallen. Het duurt enkele minuten voor het patroon load te geven. Laat de generator uitvoeren voor ten minste drie tot vier minuten voordat u de belasting bewaken.

1. Open in de PowerShell ISE, de... \\Learning-Modules\\hulpprogramma's\\*Demo LoadGenerator.ps1* script.
2. Druk op F5 het script uitvoert en de load-generator starten. Laat de standaardwaarde parameterwaarden nu.
3. Aanmelden bij uw Azure-account en selecteer het abonnement dat u gebruiken wilt, indien nodig.

Het script van de generator load achtergrondtaak voor elke database in de catalogus begint en stopt. Als u het script van de generator load opnieuw, stopt hij een achtergrondtaken die worden uitgevoerd voordat het een nieuwe begint.

#### <a name="monitor-the-background-jobs"></a>Controleren van de achtergrondtaken

Als u wilt beheren en controleren van de achtergrondtaken, gebruikt u de volgende cmdlets:

- `Get-Job`
- `Receive-Job`
- `Stop-Job`

#### <a name="demo-loadgeneratorps1-actions"></a>Demo LoadGenerator.ps1 acties

*Demo LoadGenerator.ps1* lijkt op een actieve werkbelasting van de klanttransacties. De volgende stappen beschrijven de volgorde van de acties die *Demo LoadGenerator.ps1* initieert:

1. *Demo LoadGenerator.ps1* begint *LoadGenerator.ps1* op de voorgrond.

    - Beide .ps1-bestanden worden opgeslagen onder de mappen Learning-Modules\\hulpprogramma's\\.

2. *LoadGenerator.ps1* lussen via alle tenant-databases in de catalogus.

3. *LoadGenerator.ps1* PowerShell achtergrondtaak voor elke tenant-database wordt gestart:

    - Standaard worden de achtergrondtaken gedurende 120 minuten uitgevoerd.
    - Elke taak veroorzaakt een op basis van CPU-belasting op één tenant database door het uitvoeren van *sp_CpuLoadGenerator*. De intensiteit en de duur van de belasting varieert afhankelijk van `$DemoScenario`. 
    - *sp_CpuLoadGenerator* lussen rond een SQL SELECT-instructie dat ervoor zorgt een hoog CPU-belasting dat. Het tijdsinterval tussen problemen van de is geselecteerd, is afhankelijk van parameterwaarden voor het maken van een instelbare CPU-belasting. Belastingsniveaus en intervallen wordt willekeurig om te simuleren meer realistische laadt.
    - Dit .sql-bestand wordt opgeslagen onder *WingtipTenantDB\\dbo\\StoredProcedures\\*.

4. Als `$OneTime = $false`, de load-generator begint de achtergrondtaken en vervolgens verder uitgevoerd. Elke 10 seconden bewaakt Hiermee u voor elke nieuwe tenants die zijn ingericht. Als u instelt `$OneTime = $true`, de LoadGenerator begint de achtergrondtaken en vervolgens niet meer wordt uitgevoerd op de voorgrond. Voor deze zelfstudie laat `$OneTime = $false`.

  Ctrl + C of stoppen bewerking Ctrl-Break gebruiken als u wilt stoppen of opnieuw starten van de load-generator. 

  Als u de load-generator actief op de achtergrond laat, gebruikt u een ander exemplaar van de PowerShell ISE andere PowerShell-scripts uitvoeren.

&nbsp;

Voordat u met de volgende sectie doorgaat, laat u de load-generator uitgevoerd in de status van de taak wordt aangeroepen.

## <a name="provision-a-new-tenant"></a>Een nieuwe tenant inrichten

De eerste implementatie maakt drie voorbeeld tenants. Nu maken u een andere tenant om te zien van de impact op de geïmplementeerde toepassing. In de app Wingtip de werkstroom voor het inrichten van nieuwe tenants wordt uitgelegd in de [inrichten en catalogus zelfstudie](saas-dbpertenant-provision-and-catalog.md). In deze fase maakt u een nieuwe tenant, die minder dan een minuut duurt.

1. Open een nieuw PowerShell ISE.
2. Open... \\Learning Modules\Provision en catalogus\\*Demo ProvisionAndCatalog.ps1*.
3. Het script wordt uitgevoerd, druk op F5. Laat de standaardwaarden voor nu.

   > [!NOTE]
   > Veel Wingtip SaaS-scripts gebruiken *$PSScriptRoot* om te bladeren mappen voor het aanroepen van functies in andere scripts. Deze variabele alleen geëvalueerd wanneer de volledige script wordt uitgevoerd door op F5 te drukken. Markering en uitvoeren van een selectie met F8 kunnen leiden tot fouten. Druk op F5 om de scripts worden uitgevoerd.

De nieuwe tenant-database is:

- In een elastische SQL-groep gemaakt.
- Geïnitialiseerd.
- Geregistreerd in de catalogus.

Na een geslaagde inrichting wordt de *gebeurtenissen* site van de nieuwe tenant wordt weergegeven in uw browser.

![Nieuwe tenant](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Vernieuw de Hub gebeurtenissen zodat de nieuwe tenant in de lijst weergegeven.

## <a name="explore-the-servers-pools-and-tenant-databases"></a>Servers, pools en tenant-databases

Nu dat u hebt gestart met het uitvoeren van een werklast in de verzameling tenants, bekijken we enkele van de bronnen die zijn geïmplementeerd.

1. In de [Azure-portal](http://portal.azure.com), blader naar de lijst met SQL-servers. Open vervolgens de **catalogus-dpt -&lt;gebruiker&gt;**  server.
    - De catalogusserver bevat twee databases **tenantcatalog** en **basetenantdb** (een sjabloondatabase die wordt gekopieerd voor het maken van nieuwe tenants).

   ![Databases](./media/saas-dbpertenant-get-started-deploy/databases.png)

2. Ga terug naar de lijst met SQL-servers.

3. Open de **tenants1-dpt -&lt;gebruiker&gt;**  server die de tenant-databases bevat.

4. Zie de volgende items:

    - Elke tenant-database is een **Standard elastische** database in een standaard 50 eDTU-pool.
    - De Rode esdoorn race-database is de tenant-database die u eerder hebt ingericht.

   ![Server-databases](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>De pool bewaken

Na *LoadGenerator.ps1* wordt uitgevoerd voor enkele minuten onvoldoende gegevens beschikbaar moeten zijn om te kijken naar sommige bewakingsmogelijkheden starten. Deze mogelijkheden zijn ingebouwd in pools en databases.

Blader naar de server **tenants1-dpt -&lt;gebruiker&gt;**, en selecteer **Pool1** om Resourcegebruik voor de groep weer te geven. In de volgende diagrammen is de load-generator gedurende één uur uitgevoerd.

   ![Monitor voor toepassingen](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

- De eerste grafiek met het label **Resourcegebruik**, toont de opslaggroep eDTU-gebruik.
- Het tweede diagram toont de eDTU-gebruik van de vijf meest actieve databases in de groep.

De twee grafieken laten zien dat de elastische pools en SQL-Database zeer geschikt voor werkbelastingen van onvoorspelbare SaaS-toepassingen zijn. De grafieken weergegeven dat vier databases zijn elke sturen naar zo veel 40 edtu's en nog probleemloos alle databases worden ondersteund door een 50 eDTU-pool. De 50 eDTU-pool kan zelfs zwaardere belastingen kan ondersteunen. Als de databases worden ingericht als zelfstandige databases, moet elk criterium een S2 (50 DTU) voor de ondersteuning van de bursts. De kosten van vier zelfstandige S2 databases is bijna drie keer de prijs van de groep. SQL-Database klanten start in de echte situaties, maximaal 500 databases in 200 eDTU-toepassingen. Zie voor meer informatie de [prestaties bewaking zelfstudie](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Aanvullende resources

- Zie voor meer informatie, aanvullende [zelfstudies die zijn gebaseerd op de toepassing van de database per tenant Wingtip Tickets SaaS](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
- Zie voor meer informatie over elastische pools, [wat is er een Azure SQL elastische pool?](sql-database-elastic-pool.md).
- Zie voor meer informatie over elastische taken [beheren cloud uitgebreide databases](sql-database-elastic-jobs-overview.md).
- Zie voor meer informatie over multitenant SaaS-toepassingen, [ontwerppatronen voor multitenant SaaS-toepassingen](saas-tenancy-app-design-patterns.md).


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> - Klik hier voor meer informatie over het implementeren van de Wingtip Tickets SaaS-toepassing.
> - Over de servers, pools en databases die gezamenlijk de app.
> - Hoe tenants zijn toegewezen aan hun gegevens met de *catalogus*.
> - Het inrichten van nieuwe tenants.
> - Klik hier voor meer informatie over het gebruik van de groep van toepassingen voor het controleren van de activiteit van de tenant weergeven.
> - Klik hier voor meer informatie over het verwijderen van de voorbeeldresources om te stoppen gerelateerde facturering.

Probeer vervolgens de [inrichten en catalogus zelfstudie](saas-dbpertenant-provision-and-catalog.md).



<!-- Link references. -->

[github-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant 


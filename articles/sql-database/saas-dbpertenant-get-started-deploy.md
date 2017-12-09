---
title: Zelfstudie voor database per tenant SaaS - Azure SQL Database | Microsoft Docs
description: Implementeer en Verken de Wingtip Tickets SaaS multitenant-toepassing, die u laat zien van de Database per Tenant en andere SaaS-patronen met Azure SQL Database.
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: genemi
ms.openlocfilehash: 5342b5290fab9826a2b38cd7ada63a6736c77601
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-the-database-per-tenant-pattern-with-azure-sql-database"></a>Implementeren en een multitenant SaaS-toepassing die gebruikmaakt van de database per tenant patroon met Azure SQL Database verkennen

In deze zelfstudie maakt u implementeert en Verken de Wingtip Tickets SaaS *database per tenant* toepassing (Wingtip). De app gebruikmaakt van een database per tenant-patroon voor het opslaan van de gegevens van meerdere tenants. De app is ontworpen om hier functies van Azure SQL Database die inschakelen SaaS-scenario's vereenvoudigen.

Vijf minuten nadat u op de blauwe knop met het label **implementeren in Azure**, hebt u een multitenant SaaS-toepassing. De app bevat een Azure SQL Database uitgevoerd in de Microsoft Azure-cloud. De app wordt geïmplementeerd met drie voorbeeld-tenants, elk met een eigen database. Alle databases zijn geïmplementeerd in een SQL *elastische pool*. De app wordt geïmplementeerd op uw Azure-abonnement. U hebt volledige toegang om te verkennen en werken met de afzonderlijke onderdelen van de app. De toepassing C#-broncode en de scripts management zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant GitHub-repo-][github-wingtip-dpt].

In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> - Klik hier voor meer informatie over het implementeren van de Wingtip SaaS-toepassing.
> - Waar u de broncode van de toepassing en beheerscripts schrijven.
> - Over de servers, pools en databases die gezamenlijk de app.
> - Hoe tenants zijn toegewezen aan hun gegevens met de *catalogus*.
> - Het inrichten van een nieuwe tenant.
> - Het controleren van de tenant-activiteit in de app.

Een [reeks zelfstudies gerelateerde](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) biedt wilt experimenteren met verschillende SaaS-ontwerp- en patronen. De zelfstudies bouwen afgezien van deze initiële implementatie.
Wanneer u de zelfstudies gebruikt, kunt u zien hoe de andere SaaS-patronen worden geïmplementeerd door de opgegeven scripts in.
De scripts laten zien hoe de functies van SQL-Database de ontwikkeling van SaaS-toepassingen vereenvoudigen.

## <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* Azure PowerShell is geïnstalleerd. Zie voor meer informatie [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-saas-application"></a>De Wingtip Tickets SaaS-toepassing implementeren

Implementeer de app:

1. Kies en vergeet niet de waarden die u gebruiken voor de volgende parameters moet:

    - **Gebruiker**: Kies een korte waarde, zoals uw initialen gevolgd door een cijfer. Bijvoorbeeld: *af1*. De parameter gebruiker mag alleen letters, cijfers en afbreekstreepjes (zonder spaties). Het eerste en laatste teken moet een letter of een cijfer zijn. Het is raadzaam dat alle letters kleine letters worden.
    - **Resourcegroep**: elke keer dat u de toepassing Wingtip implementeren, moet u een andere unieke naam voor de nieuwe resourcegroep. Het is raadzaam dat u de naam van de gebruiker aan een basisnaam voor de resourcegroep toevoegen. Een voorbeeld van de naam van resourcegroep kan worden *wingtip af1*. Opnieuw, is het raadzaam dat alle letters kleine letters worden.

2. De Wingtip Tickets SaaS-Database per Tenant implementatiesjabloon in de Azure portal openen door te klikken op de blauwe **implementeren in Azure** knop.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

3. Voer in de sjabloon, waarden voor de vereiste parameters:

    > [!IMPORTANT]
    > Om het overzichtelijk te houden zijn bepaalde verificatieprocessen weggelaten. Ook zijn de firewalls op servers uitgeschakeld voor deze zelfstudie. We raden aan dat u *een nieuwe resourcegroep maken*. Gebruik geen bestaande resourcegroepen, servers of groepen. Gebruik deze toepassing, scripts of alle geïmplementeerde resources niet voor productie. Verwijder deze resourcegroep wanneer u klaar bent om gerelateerde facturering te stoppen.

    - **Resourcegroep** : Selecteer **nieuw** en geef de unieke **naam** u eerder hebt gekozen voor de resourcegroep. 
    - **Locatie** : Selecteer een **locatie** uit de vervolgkeuzelijst.
    - **Gebruiker** -gebruik van de gebruiker de naam van waarde die u eerder hebt gekozen.

4. Implementeer de toepassing.

    - Klik om de voorwaarden en bepalingen te accepteren.
    - Klik op **Kopen**.

5. Implementatiestatus controleren door te klikken op **meldingen**, namelijk het belpictogram rechts van het zoekvak. Implementatie van de app Wingtip Tickets SaaS duurt ongeveer vijf minuten.

   ![implementatie gelukt](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Downloaden en de Wingtip Tickets management scripts deblokkeren

Download de bron en het beheer scripts tijdens de implementatie van de toepassing.

> [!IMPORTANT]
> Uitvoerbare inhoud (scripts, dll-bestanden) mogelijk geblokkeerd door Windows als ZIP-bestanden van een externe bron wordt gedownload en uitgepakt. Bij het uitpakken van de scripts uit een zipbestand, gebruikt u de volgende stappen uit om de blokkering van het ZIP-bestand voor het uitpakken van te. Het opheffen van de blokkering zorgt ervoor dat de scripts mogen worden uitgevoerd.

1. Blader naar de [WingtipTicketsSaaS DbPerTenant GitHub-repo-][github-wingtip-dpt].
2. Klik op **klonen of downloaden**.
3. Klik op **ZIP downloaden**, en sla het bestand.
4. Met de rechtermuisknop op de **WingtipTicketsSaaS-DbPerTenant-master.zip** bestand en selecteer vervolgens **eigenschappen**.
5. Op de **algemene** tabblad **blokkering**, en klik vervolgens op **toepassen**.
6. Klik op **OK**.
7. Pak de bestanden.

Scripts bevinden zich in de *... \\WingtipTicketsSaaS DbPerTenant master\\Learning-Modules* map.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Het configuratiebestand van de gebruiker voor deze implementatie bijwerken

Voordat u de scripts uitvoert, werken de *resourcegroep* en *gebruiker* in waarden **UserConfig.psm1**. Deze variabelen ingesteld op de waarden die u tijdens de implementatie gebruikt.

1. In de *PowerShell ISE*open... \\Learning-Modules\\*UserConfig.psm1* 
2. Update *ResourceGroupName* en *naam* met de specifieke waarden voor uw implementatie (op regels 10 en 11 alleen).
3. De wijzigingen opslaan!

Deze waarden wordt verwezen in bijna elk script.

## <a name="run-the-application"></a>De toepassing uitvoeren

De app gepresenteerd plaatsen die als host fungeren van gebeurtenissen. U wilt typen zijn concertzalen jazz clubs en sportverenigingen. In Wingtip-Tickets worden plaatsen geregistreerd als tenants. Een tenant heeft een u wilt een eenvoudige manier aan de lijst met gebeurtenissen en tickets verkopen aan hun klanten. Elke locatie vast Hiermee haalt u een aangepaste website voor een lijst met hun gebeurtenissen en tickets verkopen.

Intern opgehaald elke tenant in de app, een SQL-database die is geïmplementeerd in een elastische SQL-groep.

Een centraal **gebeurtenissen Hub** pagina bevat een lijst met koppelingen voor de tenants in uw implementatie.

1. Open de *gebeurtenissen Hub* in uw webbrowser: http://events.wingtip-dpt.&lt;gebruiker&gt;. trafficmanager.net (vervangen door &lt;gebruiker&gt; met uw implementatie gebruiker waarde):

    ![events hub](media/saas-dbpertenant-get-started-deploy/events-hub.png)

2. Klik in de **Events Hub** op *Fabrikam Jazz Club*.

    ![Gebeurtenissen](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Maakt gebruik van de toepassing Wingtip [ *Azure Traffic Manager* ](../traffic-manager/traffic-manager-overview.md) om de verspreiding van binnenkomende aanvragen. De URL voor toegang tot de hub gebeurtenissen voor één tenant moet zich aan de volgende indeling:

- http://events.Wingtip-DPT.&lt;gebruiker&gt;.trafficmanager.net/fabrikamjazzclub

De onderdelen van de voorgaande indeling worden in de volgende tabel beschreven.

| URL-onderdeel | Beschrijving |
| :------- | :---------- |
| http://events.Wingtip-DPT | De onderdelen van de gebeurtenissen van de app Wingtip.<br /><br />de ***-dpt*** gedeelte onderscheidt de *Database Per Tenant* uitvoering van Wingtip van andere implementaties enigszins anders. Andere documentatieartikelen bieden bijvoorbeeld Wingtip voor *Standalong* (*-sa*), of voor *multitenant DB*. |
| .  *&lt;GEBRUIKER&gt;* | *af1* in ons voorbeeld. |
| .trafficmanager.NET/ | Met Azure Traffic Manager basis-URL. |
| fabrikamjazzclub | Voor de tenant met de naam *Fabrikam Jazz vereniging*. |
| &nbsp; | &nbsp; |

1. De tenantnaam wordt door de app gebeurtenissen uit de URL geparseerd.
2. Naam van de tenant wordt gebruikt om een sleutel te maken.
3. De sleutel wordt gebruikt voor toegang tot de catalogus en u kunt de locatie van de tenant-database.
    - De catalogus wordt geïmplementeerd met behulp van *shard kaart management*.
4. De *gebeurtenissen Hub* uitgebreide metagegevens in de catalogus gebruikt voor het verkrijgen van een lijst met URL's van de gebeurtenis.

In een productieomgeving, maakt u gewoonlijk een DNS CNAME-record [ *het internetdomein van een bedrijf wijzen* ](../traffic-manager/traffic-manager-point-internet-domain.md) aan traffic manager-profiel.

## <a name="start-generating-load-on-the-tenant-databases"></a>Workloads genereren voor de tenant-databases

Nu dat de app is geïmplementeerd, laten we aan de slag!
De *Demo LoadGenerator* PowerShell-script wordt gestart van een werkbelasting die wordt uitgevoerd op alle databases van de tenant.
De werkelijke belasting van veel SaaS-apps is het sporadisch en onvoorspelbaar.
Om te simuleren belasting, levert de generator van een werklast met willekeurige pieken of bursts van activiteiten op elke tenant.
De bursts optreden op willekeurige intervallen.
Het duurt enkele minuten voor het patroon load te geven. Het is daarom aangeraden de generator uitvoeren voor ten minste drie tot vier minuten voor de bewaking van de belasting.

1. In de *PowerShell ISE*, open de... \\Learning-Modules\\hulpprogramma's\\*Demo LoadGenerator.ps1* script.
2. Druk op **F5** voert u het script en de load-generator starten. (Laat de standaardwaarde parameterwaarden nu.)

Hergebruik geen hetzelfde exemplaar van de PowerShell ISE voor iets anders dan mogelijk opnieuw uitvoeren van *Demo LoadGenerator.ps1*. Als u wilt dat andere PowerShell-scripts uitvoeren, start u een aparte PowerShell ISE.

#### <a name="rerun-with-different-parameters"></a>Opnieuw uitvoeren met andere parameters

Volg deze stappen als u opnieuw wilt uitvoeren van de test werkbelasting met andere parameters:

1. Stop *LoadGenerator.ps1*.
    - Gebruiken **Ctrl + C**, of klik op de **stoppen** knop.
    - Deze stopzetting niet stoppen of invloed op een onvolledige achtergrondtaken die nog wordt uitgevoerd.

2. Voer *Demo LoadGenerator.ps1*.
    - Deze opnieuw uitvoeren van de achtergrondtaken die mogelijk nog steeds actief eerst stopt *sp_CpuLoadGenerator*.

Of u kunt de PowerShell ISE-instantie die alle achtergrondtaken stopt beëindigen. Vervolgens start u een nieuw exemplaar van de PowerShell ISE en opnieuw *Demo LoadGenerator.ps1*.

#### <a name="monitor-the-background-jobs"></a>Controleren van de achtergrondtaken

Als u wilt beheren en controleren van de achtergrondtaken, kunt u de volgende cmdlets:

- Get-Job
- Receive-Job
- Taak stoppen

#### <a name="demo-loadgeneratorps1-actions"></a>Demo LoadGenerator.ps1 acties

*Demo LoadGenerator.ps1* lijkt op een actieve werkbelasting van de klanttransacties. De volgende stappen beschrijven de volgorde van de acties die *Demo LoadGenerator.ps1* initieert:

1. *Demo LoadGenerator.ps1* begint *LoadGenerator.ps1* op de voorgrond.
    - Beide .ps1 bestanden worden opgeslagen onder de mappen *Learning-Modules\\hulpprogramma's\\*.

2. *LoadGenerator.ps1* lussen via alle tenant-databases die zijn geregistreerd in de catalogus.

3. Voor elke database tenant *LoadGenerator.ps1* begint de uitvoering van de Transact-SQL opgeslagen procedure met de naam *sp_CpuLoadGenerator*.
    - De uitvoeringen worden gestart op de achtergrond door het aanroepen van de *Invoke-SqlAzureWithRetry* cmdlet.
    - *sp_CpuLoadGenerator* lussen rond een SQL SELECT-instructie voor een standaardduur van 60 seconden. Het tijdsinterval tussen problemen van de is geselecteerd, is afhankelijk van parameterwaarden.
    - Dit .sql-bestand wordt opgeslagen onder *WingtipTenantDB\\dbo\\StoredProcedures\\*.

4. Voor elke database tenant *LoadGenerator.ps1* ook begint de *starttaak* cmdlet.
    - *Start-Job* lijkt op een werkbelasting van de ticket verkopen.

5. *LoadGenerator.ps1* worden uitgevoerd, wordt de bewaking voortgezet voor alle nieuwe tenants die zijn ingericht.

&nbsp;

Voordat u doorgaat met de volgende sectie, laat u de load-generator uitgevoerd in de status van de taak wordt aangeroepen.

## <a name="provision-a-new-tenant"></a>Een nieuwe tenant inrichten

De eerste implementatie maakt drie voorbeeld tenants. Nu maken u een andere tenant om te zien van de impact op de geïmplementeerde toepassing. In de app Wingtip de werkstroom voor het inrichten van nieuwe tenants wordt uitgelegd in de [inrichten en catalogus zelfstudie](saas-dbpertenant-provision-and-catalog.md). In deze fase maakt u een nieuwe tenant, die minder dan een minuut duurt.

1. In de *PowerShell ISE*open... \\Learning Modules\Provision en catalogus\\*Demo ProvisionAndCatalog.ps1* .
2. Druk op **F5** om het script uit te voeren. (Laat de standaardwaarden voor nu.)

   > [!NOTE]
   > Veel Wingtip SaaS-scripts gebruiken *$PSScriptRoot* om te navigeren mappen voor het aanroepen van functies in andere scripts. Deze variabele wordt alleen geëvalueerd wanneer de volledige script is uitgevoerd door te drukken **F5**.  Markering en wordt uitgevoerd een selectie met **F8** kan leiden tot fouten. Dus de scripts uitvoeren door te drukken **F5**.

De nieuwe tenant-database is:

- In een elastische SQL-groep gemaakt.
- Geïnitialiseerd.
- Geregistreerd in de catalogus.

Na een geslaagde inrichting wordt de *gebeurtenissen* site van de nieuwe tenant wordt weergegeven in uw browser:

![Nieuwe tenant](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Vernieuw de *gebeurtenissen Hub* waarmee de nieuwe tenant in de lijst weergegeven.

## <a name="explore-the-servers-pools-and-tenant-databases"></a>Servers, pools en tenant-databases

Nu dat u hebt gestart met het uitvoeren van een werklast in de verzameling tenants, bekijken we enkele van de bronnen die zijn geïmplementeerd:

1. In de [Azure-portal](http://portal.azure.com), blader naar de lijst met SQL-servers en open vervolgens de **catalogus-dpt -&lt;gebruiker&gt;**  server.
    - De catalogusserver bevat twee databases de **tenantcatalog** en de **basetenantdb** (een sjabloondatabase die wordt gekopieerd voor het maken van nieuwe tenants).

   ![databases](./media/saas-dbpertenant-get-started-deploy/databases.png)

2. Ga terug naar de lijst met SQL-servers.

3. Open de **tenants1-dpt -&lt;gebruiker&gt;**  server die de tenant-databases bevat.

4. Zie de volgende items:
    - Elke tenant-database is een *Standard elastische* database in een standaard 50 eDTU-pool.
    - De *Rode esdoorn race* database op die de tenant-database die u eerder hebt ingericht.

   ![server](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>De pool bewaken

Na *LoadGenerator.ps1* wordt uitgevoerd voor enkele minuten onvoldoende gegevens beschikbaar moeten zijn om te kijken naar sommige bewakingsmogelijkheden starten. Deze mogelijkheden zijn ingebouwd in pools en databases.

Blader naar de server **tenants1-dpt -&lt;gebruiker&gt;**, en klik op **Pool1** om Resourcegebruik voor de groep weer te geven. In de volgende diagrammen is de load-generator gedurende één uur uitgevoerd.

   ![pool bewaken](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

- De eerste grafiek met het label **Resourcegebruik**, toont de opslaggroep eDTU-gebruik.
- Het tweede diagram toont de eDTU-gebruik van de bovenste vijf databases in de groep.

De twee grafieken laten zien dat de elastische pools en SQL-Database goed geschikt voor werkbelastingen van de SaaS-toepassingen zijn.
De grafieken weergegeven dat 4 databases zijn elke sturen naar zo veel 40 edtu's en nog probleemloos alle databases worden ondersteund door een 50 eDTU-pool. De 50 eDTU-pool kan zelfs zwaardere belastingen kan ondersteunen.
Als ze zijn ingericht als zelfstandige databases, zoals elke moeten een S2 (50 DTU) voor de ondersteuning van de bursts.
De kosten van 4 zelfstandige S2 databases zou zijn bijna 3 keer de prijs van de groep.
In de praktijk situaties worden SQL-Database klanten momenteel uitgevoerd maximaal 500 databases in 200 eDTU-toepassingen.
Zie voor meer informatie de [zelfstudie over prestatiebewaking](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Aanvullende bronnen

- Aanvullende [zelfstudies die zijn gebaseerd op de Database Wingtip Tickets SaaS per Tenant-toepassing](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
- Informatie over elastische pools vindt u in [*Wat is een elastische pool van Azure SQL?*](sql-database-elastic-pool.md).
- Informatie over elastische taken vindt u in [*Managing scaled-out cloud databases*](sql-database-elastic-jobs-overview.md) (Uitgeschaalde clouddatabases beheren).
- Informatie over SaaS-toepassingen voor meerdere tenants vindt u in [*Design patterns for multi-tenant SaaS applications*](saas-tenancy-app-design-patterns.md) (Ontwerppatronen voor SaaS-toepassingen voor meerdere tenants).


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> - De Wingtip Tickets SaaS-toepassing implementeren
> - Informatie over de servers, pools en databases waaruit de app bestaat
> - Tenants worden via de *catalogus* toegewezen aan hun gegevens
> - Inrichten van nieuwe tenants
> - Poolgebruik bekijken om de activiteit in een tenant te controleren
> - Voorbeeldresources verwijderen om gerelateerde facturering te stoppen

Probeer vervolgens de [inrichten en catalogus zelfstudie](saas-dbpertenant-provision-and-catalog.md).



<!-- Link references. -->

[github-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant 


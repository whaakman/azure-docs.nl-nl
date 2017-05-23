---
title: Een SaaS-toepassing implementeren en verkennen die gebruikmaakt van Azure SQL Database | Microsoft Docs
description: De Azure SQL Database-voorbeeld-app Wingtip Tickets (WTP) implementeren en verkennen
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 04859fb8a9f4a8bf04d92e7139d9202885c52503
ms.contentlocale: nl-nl
ms.lasthandoff: 05/12/2017


---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-azure-sql-database"></a>Een voorbeeld van een SaaS-toepassing voor meerdere tenants implementeren en verkennen die gebruikmaakt van Azure SQL Database

In deze zelfstudie gaat u de SaaS-toepassing WTP (Wingtip Tickets Platform) implementeren en verkennen. De toepassing maakt gebruik van het SaaS-toepassingspatroon database-per-tenant om zo meerdere tenants te kunnen bedienen. De toepassing is ontworpen om de functies van Azure SQL Database die SaaS-scenario’s ondersteunen, optimaal onder de aandacht te brengen, zoals ontwerp- en -beheerpatronen voor SaaS.

Vijf minuten nadat u hieronder op de knop *Distribueren naar Azure* hebt geklikt, beschikt u over een SaaS-toepassing voor meerdere tenants die met behulp van SQL Database wordt uitgevoerd in de cloud. De toepassing wordt geïmplementeerd met drie voorbeeldtenants, elk met een eigen database, die worden opgenomen in een elastische SQL-pool. De app wordt geïmplementeerd naar uw Azure-abonnement, zodat u volledige toegang hebt tot de app en de afzonderlijke toepassingsonderdelen kunt controleren en ermee kunt werken.

De scripts en broncode van de toepassing zijn beschikbaar in de GitHub-opslagplaats [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS).

In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]

> * Implementeren van de WTP-toepassing
> * Informatie over de servers, pools en databases waaruit de app bestaat
> * Tenants worden via de *catalogus* toegewezen aan hun gegevens
> * Inrichten van nieuwe tenants
> * Poolgebruik bekijken om de activiteit in een tenant te controleren
> * Voorbeeldresources verwijderen om gerelateerde facturering te stoppen

Om een beeld te krijgen van verschillende ontwerp- en beheerpatronen voor SaaS-toepassingen, hebben we een [reeks gerelateerde zelfstudies](sql-database-wtp-overview.md) ontwikkeld die voortbouwt op deze initiële implementatie. Terwijl u de verschillende zelfstudies doorneemt, kunt u de bijgeleverde scripts bestuderen en ontdekken hoe de verschillende SaaS-patronen worden geïmplementeerd. U kunt de scripts in elke zelfstudie stapsgewijs doorlopen om inzicht te krijgen in de verschillende SQL Database-functies die beschikbaar zijn om op een eenvoudige manier SaaS-toepassingen te ontwikkelen.

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="deploy-the-wingtip-tickets-wtp-saas-application"></a>De SaaS-toepassing WTP (Wingtip Tickets Platform) implementeren

U kunt de toepassing WTP als volgt in minder dan vijf minuten implementeren:

1. Klik op deze knop om het proces te starten:

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Voer de vereiste parameterwaarden in voor de implementatie:

    > [!IMPORTANT]
    > Om het overzichtelijk te houden zijn bepaalde verificatieprocessen weggelaten. Ook zijn de firewalls op servers uitgeschakeld voor deze zelfstudie. **Maak een nieuwe resourcegroep**, en gebruik geen bestaande resourcegroepen, servers of pools, en gebruik deze toepassing, of eventuele resources die met de toepassing worden gemaakt, niet voor productiedoeleinden. Verwijder deze resourcegroep wanneer u klaar bent om gerelateerde facturering te stoppen.

    * **Resourcegroep**: selecteer **Nieuwe maken** en geef waarden op voor **Naam** en **Locatie**.
    * **Gebruiker**: sommige resources moeten namen hebben die uniek zijn binnen alle Azure-abonnementen. U kunt ervoor zorgen dat de namen uniek zijn door hier een waarde op te geven waarmee de resources die u maakt, worden onderscheiden van resources die worden gemaakt door andere gebruikers die de WTP-toepassing implementeren. Het is raadzaam om een korte naam op te geven voor **Gebruiker**, zoals uw initialen plus een nummer (bijvoorbeeld *bg1*), en die naam te gebruiken in de naam van de resourcegroep (bijvoorbeeld *wingtip-bg1*). De parameter **Gebruiker** mag alleen letters, cijfers en afbreekstreepjes bevatten. Voor het eerste en laatste teken moet u een letter of een cijfer gebruiken. Het wordt aanbevolen om alleen kleine letters te gebruiken.

     ![sjabloon](./media/sql-database-saas-tutorial/template.png)

1. **Implementeer de toepassing**.

    * Geef aan dat u akkoord gaat met de voorwaarden.
    * Selecteer **Vastmaken aan dashboard**.
    * Klik op **Kopen**.

1. Controleer de voortgang van de implementatie door te klikken op **Meldingen** (het belpictogram naast het zoekvak). Het implementeren van de WTP-app duurt ongeveer vier minuten.

   ![implementatie gelukt](media/sql-database-saas-tutorial/succeeded.png)

## <a name="explore-the-application"></a>De toepassing verkennen

In de app worden venues gepresenteerd, zoals concertzalen, jazzclubs en stadions, die bepaalde evenementen organiseren. Venues registreren zich als klant (of tenant) van WTP (Wingtip Tickets Platform), zodat ze op een gemakkelijke manier evenementen onder de aandacht kunnen brengen en tickets kunnen verkopen. Elke venue krijgt een eigen web-app voor het beheren en vermelden van hun evenementen en het verkopen van tickets, onafhankelijk en gescheiden van andere tenants. Onzichtbaar voor de gebruiker krijgt elke tenant een SQL-database die deel uitmaakt van een elastische SQL-pool.

Een centrale **Events Hub** bevat een lijst met tenant-URL's die specifiek zijn voor uw implementatie. Alle tenant-URL's bevatten de unieke waarde voor *Gebruiker* die u eerder hebt opgegeven en hebben deze notatie: http://events.wtp.&lt;GEBRUIKER&gt;.trafficmanager.net/*fabrikamjazzclub*. 

1. Open de _Events Hub_: http://events.wtp.&lt;GEBRUIKER&gt;.trafficmanager.net (vervangen door uw gebruikersnaam):

    ![events hub](media/sql-database-saas-tutorial/events-hub.png)

1. Klik in de **Events Hub** op *Fabrikam Jazz Club*.

   ![Gebeurtenissen](./media/sql-database-saas-tutorial/fabrikam.png)

1. Klik op **Tickets** en bekijk de interface voor het kopen van een ticket voor een evenement.

De toepassing WTP maakt gebruik van [*Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) om de distributie van inkomend verkeer te regelen. De pagina's van evenementen, die specifiek zijn voor een tenant, werken alleen als de namen van de tenants zijn opgenomen in de URL's. De app parseert de tenantnaam uit de URL en gebruikt deze om een sleutel te maken en zo via [*Shard Map Manager*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-scale-shard-map-management) toegang te krijgen tot een catalogus. In de catalogus is de sleutel gekoppeld aan de locatie van de database van de tenant. De **Events Hub** maakt gebruik van uitgebreide metagegevens in de catalogus om de naam op te halen van de tenant die aan elke database is gekoppeld.

In een productieomgeving wordt er meestal een CNAME DNS-record gemaakt om [*het internetdomein van uw bedrijf te laten verwijzen*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) naar het profiel van Traffic Manager.

## <a name="get-the-wingtip-application-scripts"></a>De scripts van de Wingtip-toepassing downloaden

De scripts en broncode van de Wingtip Tickets-toepassing zijn beschikbaar in de GitHub-opslagplaats [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). U vindt de scriptbestanden in de map [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Download de map **Learning Modules** naar de lokale computer en behoud de mapstructuur.

## <a name="provision-a-new-tenant"></a>Een nieuwe tenant inrichten

Tijdens de eerste implementatie zijn er drie voorbeeldtenants gemaakt, maar voor de beste ervaring hebben we meer tenants nodig. In deze stap gaan we snel een nieuwe tenant maken. Later kunt u dan [deze zelfstudie](sql-database-saas-tutorial-provision-and-catalog.md) volgen om alles te leren over het inrichten van nieuwe tenants. Hier kunt u ook zien hoe eenvoudig het is om een registratiecomponent te implementeren in de toepassing, zodat er automatisch tenants worden ingericht als klanten zich registreren.

### <a name="initialize-the-user-config-file-for-your-deployment"></a>Het UserConfig-bestand voor uw implementatie initialiseren

Omdat dit het eerste script is dat u gaat uitvoeren na de eerste implementatie van WTP, moet u het gebruikersconfiguratiebestand (UserConfig) bijwerken. Werk de variabelen bij met de specifieke waarden voor uw implementatie.

   1. Open ...\\Learning Modules\\*UserConfig.psm1* in de *PowerShell ISE*.
   1. Wijzig _$userConfig.ResourceGroupName_ in de waarde voor _Resourcegroep_ die u hebt ingesteld tijdens het implementeren van de app.
   1. Wijzig _$userConfig.Name_ in de waarde voor _Gebruiker_ die u hebt ingesteld tijdens het implementeren van de app.

### <a name="provision-the-new-tenant"></a>De nieuwe tenant inrichten

1. Open ...\\Learning Modules\Provision and Catalog\\*Demo-ProvisionAndCatalog.ps1* in de *PowerShell ISE*.
1. Druk op **F5** om het script uit te voeren (gebruik voorlopig de standaardwaarden van het script).

De nieuwe tenant wordt geregistreerd in de catalogus. De bijbehorende database wordt gemaakt en toegevoegd aan een elastische SQL-pool. Als de inrichting is voltooid, wordt de site voor het verkopen van tickets van de nieuwe tenant weergegeven in uw browser:

![Nieuwe tenant](./media/sql-database-saas-tutorial/red-maple-racing.png)

Vernieuw de Events Hub en controleer of de nieuwe tenant in de lijst staat.

## <a name="start-generating-load-on-the-tenant-databases"></a>Workloads genereren voor de tenant-databases

We hebben nu verschillende tenant-databases waarmee we aan de slag kunnen. We hebben een PowerShell-script ontwikkeld waarmee voor alle tenant-databases een workload wordt gesimuleerd. De workload wordt standaard gedurende 60 minuten uitgevoerd. Wingtip Tickets Platform is een SaaS-app en de werkelijke workload van een SaaS-app is meestal sporadisch en onvoorspelbaar. Om dit te simuleren, produceert de load-generator een willekeurige workload die over alle tenants wordt verdeeld. Aangezien het een paar minuten duurt voordat er een patroon ontstaat, moet u de load-generator 5-10 minuten laten draaien voordat u de workload in de volgende secties gaat volgen.

1. Open ...\\Learning Modules\\Utilities\\*Demo-LoadGenerator.psm1* in de **PowerShell ISE**
1. Druk op **F5** om het script uit te voeren en de load-generator te starten (gebruik voorlopig de standaardwaarden van het script).

> [!IMPORTANT]
> De load-generator voert een reeks taken uit in de lokale PowerShell-sessie, dus is het belangrijk dat u de PowerShell-sessie niet afsluit. Als u PowerShell sluit, of het tabblad waarop de load-generator is gestart, of de computer in de slaapstand zet, worden de taken beëindigd.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Servers, pools en tenant-databases

U weet nu hoe de toepassing in elkaar zit, hoe u een nieuwe tenant inricht en hoe u een workload start. Laten we nu eens kijken welke resources er allemaal zijn geïmplementeerd voor de app.

1. Open in [Azure Portal](http://portal.azure.com) de server **catalog-&lt;GEBRUIKER&gt;**. De catalogusserver bevat twee databases: **tenantcatalog** en **basetenantdb** (een lege *golden* database die wordt gekopieerd om nieuwe tenants te maken).

   ![databases](./media/sql-database-saas-tutorial/databases.png)

1. Open de server **tenants1-&lt;GEBRUIKER&gt;** waarop de tenant-databases zijn opgeslagen. U ziet dat elke tenant-database een database van het type _Standard - elastisch_ is in een standaardpool van 50 eDTU. Er wordt ook een database vermeld voor _Red Maple Racing_, de tenant die u eerder hebt ingericht.

   ![server](./media/sql-database-saas-tutorial/server.png)

## <a name="monitor-the-pool"></a>De pool bewaken

Als de load-generator enkele minuten actief is, moeten er voldoende gegevens beschikbaar zijn om eens te kijken naar enkele bewakingsfuncties die zijn ingebouwd in pools en databases.

1. Blader naar de server *tenants1-&lt;GEBRUIKER&gt;* en klik op **Pool1** om het resourcegebruik voor de pool weer te geven:

   ![pool bewaken](./media/sql-database-saas-tutorial/monitor-pool.png)

Wat deze twee grafieken mooi laten zien, is dat elastische pools en SQL Database uitermate geschikt zijn voor workloads van SaaS-toepassingen. Vier databases die elk 'bursten' met maximaal 40 eDTU's kunnen makkelijk worden ondersteund in een pool van 50 eDTU. Als elke database zou worden ingericht als een zelfstandige database, moet elke database van niveau S2 (50 DTU) zijn om de bursts te ondersteunen. De kosten van vier zelfstandige S2-databases zijn echter bijna drie keer zo hoog als de kosten van de pool. En de pool heeft dan nog steeds plaats voor een heleboel extra databases. In praktijksituaties draaien klanten momenteel maximaal 500 databases in pools van 200 eDTU. Zie voor meer informatie de [zelfstudie over prestatiebewaking](sql-database-saas-tutorial-performance-monitoring.md).


## <a name="deleting-the-resources-created-with-this-tutorial"></a>De resources verwijderen die met deze zelfstudie zijn gemaakt

Als u alles goed hebt bekeken en de WTP-app niet meer nodig hebt, bladert u in de portal naar de resourcegroep van de toepassing en verwijdert u de groep om alle facturering voor deze implementatie te stoppen. Als u een van de andere zelfstudies uit de reeks hebt gebruikt, worden de resources van die zelfstudies ook verwijderd.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Implementeren van de WTP-toepassing
> * Informatie over de servers, pools en databases waaruit de app bestaat
> * Tenants worden via de *catalogus* toegewezen aan hun gegevens
> * Inrichten van nieuwe tenants
> * Poolgebruik bekijken om de activiteit in een tenant te controleren
> * Voorbeeldresources verwijderen om gerelateerde facturering te stoppen

U kunt nu [de zelfstudie over het inrichten en catalogiseren van tenants](sql-database-saas-tutorial-provision-and-catalog.md) volgen als u dat wilt.



## <a name="additional-resources"></a>Aanvullende bronnen

* [Aanvullende zelfstudies die voortbouwen op de eerste implementatie van WTP (Wingtip Tickets Platform)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* Informatie over elastische pools vindt u in [*Wat is een elastische pool van Azure SQL?*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool).
* Informatie over elastische taken vindt u in [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview) (Uitgeschaalde clouddatabases beheren).
* Informatie over SaaS-toepassingen voor meerdere tenants vindt u in [*Design patterns for multi-tenant SaaS applications*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications) (Ontwerppatronen voor SaaS-toepassingen voor meerdere tenants).


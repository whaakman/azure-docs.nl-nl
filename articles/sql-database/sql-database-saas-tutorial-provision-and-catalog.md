---
title: Nieuwe tenants inrichten in een app met meerdere tenants met behulp van Azure SQL Database | Microsoft Docs
description: Nieuwe tenants inrichten en in de catalogus opnemen in de SQL Database SaaS-voorbeeld-app Wingtip Tickets (WTP)
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
ms.openlocfilehash: aae5d85a18f93b7821a6ef8fc7161dd9a6ebe533
ms.contentlocale: nl-nl
ms.lasthandoff: 05/12/2017


---
# <a name="provision-new-tenants-and-register-them-in-the-catalog"></a>Nieuwe tenants inrichten en registreren in de catalogus

In deze zelfstudie gaat u nieuwe tenants inrichten in de SaaS-toepassing WTP (Wingtip Tickets Platform). U leert hoe u tenants en tenant-databases maakt, en hoe u tenants registreert in de catalogus. De *catalogus* is een database die de toewijzing onderhoudt tussen de vele tenants van de SaaS-toepassing en hun gegevens. Gebruik deze scripts om inzicht te krijgen in de gebruikte patronen voor het inrichten en catalogiseren, en hoe de registratie van nieuwe tenants in de catalogus in zijn werk gaat. De catalogus speelt een belangrijke rol bij het doorsturen van toepassingsaanvragen naar de juiste databases.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Eén nieuwe tenant inrichten
> * Verschillende tenants tegelijk inrichten
> * Details bekijken van het inrichten van nieuwe tenants en tenants registreren in de catalogus


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De WTP-app is geïmplementeerd. Zie [De WTP SaaS-toepassing implementeren en verkennen](sql-database-saas-tutorial.md) om dit in minder dan vijf minuten te doen
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-the-saas-catalog-pattern"></a>Informatie over het SaaS-cataloguspatroon

In een SaaS-toepassing met meerdere tenants en database-ondersteuning is het belangrijk om te weten waar informatie voor elke tenant wordt opgeslagen. In het SaaS-cataloguspatroon wordt een catalogusdatabase gebruikt om informatie te onderhouden over de toewijzing tussen tenants en de locatie waar hun gegevens worden opgeslagen. Hoewel de WTP-app een databasearchitectuur met één tenant gebruikt, wordt de informatie over de toewijzing van tenant en database toch onderhouden in een catalogus. Dit basispatroon wordt namelijk altijd gehanteerd, ongeacht of een database wordt gebruikt voor één of meer tenants.

Elke tenant krijgt een sleutel toegewezen om de gegevens van de tenant in de catalogus te identificeren. In de WTP-toepassing wordt de sleutel gevormd door een hash van de naam van de tenant. Dit patroon maakt het mogelijk om het gedeelte van de toepassings-URL met de naam van de tenant te gebruiken voor het samenstellen van de sleutel en om de verbinding van een specifieke tenant op te halen. Het is mogelijk om andere id-schema's te gebruiken zonder dat dit gevolgen heeft voor het algemene patroon.

De catalogus in de WTP-app wordt geïmplementeerd via Shard Management-technologie in de [EDCL (Elastic Database Client Library)](sql-database-elastic-database-client-library.md). EDCL is verantwoordelijk voor het maken en beheren van een _catalogus_ met database-ondersteuning waar een _shard map_ wordt onderhouden. De catalogus bevat de toewijzing tussen sleutels (tenants) en de bijbehorende databases (shards).

> [!IMPORTANT]
> De toewijzingsgegevens zijn toegankelijk in de catalogusdatabase, maar u mag ze *niet bewerken*. Wijzig de toewijzingsgegevens alleen via API's van EDCL. Als u de toewijzingsgegevens rechtstreeks bewerkt, kan de catalogus worden beschadigd. Deze manier van bewerken wordt daarom niet ondersteund.

In de SaaS-app Wingtip worden nieuwe tenants ingericht door een *golden* database te kopiëren.

## <a name="get-the-wingtip-application-scripts"></a>De scripts van de Wingtip-toepassing downloaden

De scripts en broncode van de Wingtip Tickets-toepassing zijn beschikbaar in de GitHub-opslagplaats [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). U vindt de scriptbestanden in de map [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Download de map **Learning Modules** naar de lokale computer en behoud de mapstructuur.

## <a name="provision-a-new-tenant"></a>Een nieuwe tenant inrichten

Als u al een tenant in de eerste zelfstudie met WTP hebt gemaakt, kunt u zonder problemen verdergaan naar het volgende gedeelte: [Verschillende tenants tegelijk inrichten](#provision-a-batch-of-tenants).

Voer het script *Demo ProvisionAndCatalog* uit om snel een tenant te maken en deze te registreren in de catalogus:

1. Open **Demo ProvisionAndCatalog.ps1** in de PowerShell ISE en stel de volgende waarden in:
   * **$TenantName** = de naam van de nieuwe venue (bijvoorbeeld *Bushwillow Blues*). 
   * **$VenueType** = een van de vooraf gedefinieerde typen venues: blues, classicalmusic, dance, jazz, judo, motorracing, multipurpose, opera, rockmusic, soccer.
   * **$DemoScenario** = 1, laat deze waarde op _1_ staan om **één tenant in te richten**.

1. Druk op **F5** om het script uit te voeren.

Nadat het script is voltooid, wordt de nieuwe tenant ingericht, en wordt de bijbehorende app *Events* geopend in de browser:

![Nieuwe tenant](./media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Verschillende tenants tegelijk inrichten

In deze oefening gaat u verschillende tenants tegelijk toevoegen. Het is raadzaam dit te doen voordat u andere zelfstudies met WTP gaat volgen.

1. Open ...\\Learning Modules\\Utilities\\*Demo-ProvisionAndCatalog.ps1* in d *PowerShell ISE* en stel de volgende waarde in:
   * **$DemoScenario** = **3**, stel deze waarde in op **3** om **verschillende tenants tegelijk in te richten**.
1. Druk op **F5** om het script uit te voeren.

Met het script worden verschillende tenants tegelijk ingericht. Het script maakt gebruik van een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-manager-template-walkthrough.md) om de batch met tenants te bepalen, waarna de inrichting van elke database wordt gedelegeerd aan een gekoppelde sjabloon. Door op deze manier sjablonen in te zetten, kan Azure Resource Manager als broker optreden voor het inrichtingsproces voor het script. Waar mogelijk worden de databases gelijktijdig ingericht met de sjablonen. Indien nodig worden bewerkingen opnieuw gestart om het algehele proces zo goed mogelijk te laten verlopen. Het script is idempotent en dus moet u het opnieuw uitvoeren als het wordt onderbroken.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Controleren of de implementatie van de tenants is gelukt

* Open de server *tenants1* in [Azure Portal](https://portal.azure.com) en klik op **SQL-databases**:

   ![databaselijst](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)


## <a name="provision-and-catalog-detailed-walkthrough"></a>Gedetailleerd overzicht van inrichten en catalogiseren

Als u een duidelijk beeld wilt hebben van de manier waarop nieuwe tenants worden ingericht door de toepassing Wingtip, voert u het script de *Demo ProvisionAndCatalog* opnieuw uit om nog een tenant in te richten. Dit keer voegt u echter een onderbrekingspunt toe, zodat u de werkstroom stapsgewijs kunt doorlopen:

1. Open ...\\Learning Modules\Utilities\_Demo-ProvisionAndCatalog.ps1_ en geef hieronder waarden voor nieuwe tenants op die nog niet aanwezig zijn in de huidige catalogus:
   * **$TenantName** = geef een nieuwe naam op (zoals *Hackberry Hitters*).
   * **$VenueType** = gebruik een van de vooraf gedefinieerde typen venues (bijvoorbeeld *judo*).
   * **$DemoScenario** = 1, stel deze waarde in op **1** om **één tenant in te richten**.

1. Voeg een onderbrekingspunt toe door de cursor ergens op de volgende regel te plaatsen: *New-Tenant `*. Druk vervolgens op **F9**.

   ![onderbrekingspunt](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Druk op **F5** om het script uit te voeren. Wanneer het onderbrekingspunt is bereikt, drukt u op **F11** om details van de stap weer te geven. Bekijk de uitvoering van het script door met **F10** en **F11** de aangeroepen functies over te slaan of weer te geven. [Tips voor het werken met en het opsporen van fouten in PowerShell-scripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise) (Engelstalig).

### <a name="examine-the-provision-and-catalog-implementation-in-detail-by-stepping-through-the-script"></a>Het inrichten en catalogiseren in detail bekijken door het script stapsgewijs te doorlopen

Het script omvat de volgende stappen voor het inrichten en catalogiseren van nieuwe tenants:

1. **Importeren van de module SubscriptionManagement.psm1** met daarin functies voor het aanmelden bij Azure en het selecteren van het Azure-abonnement waarmee u werkt.
1. **Importeren van de module CatalogAndDatabaseManagement.psm1** die met behulp van de [Shard Management](sql-database-elastic-scale-shard-map-management.md)-functies zorgt voor een abstractie op het niveau van catalogus en tenant. Dit is een belangrijke module waarin veel elementen van het cataloguspatroon aan bod komen en die zeker de moeite waard is om goed te bestuderen.
1. **Ophalen van configuratiedetails**. Vraag de details van _Get-Configuration_ op (met **F11**) en kijk hoe de app-configuratie wordt opgegeven. Dit is de plek waar de namen van resources en andere app-specifieke waarden worden gedefinieerd. Het is raadzaam om deze waarden pas te wijzigen als u precies weet hoe de scripts werken.
1. **Ophalen van het catalogusobject**. Bekijk de details van *Get-Catalog* om te zien hoe de catalogus wordt geïnitialiseerd met behulp van de functies van Shard Management die worden geïmporteerd uit **AzureShardManagement.psm1**. De catalogus bestaat uit de volgende objecten:
   * $catalogServerFullyQualifiedName wordt samengesteld aan de hand van de standaardstam plus uw gebruikersnaam: _catalog-\<gebruiker\>.database.windows.net_.
   * $catalogDatabaseName wordt opgehaald uit de configuratie *tenantcatalog*.
   * Het object $shardMapManager wordt geïnitialiseerd uit de catalogusdatabase.
   * Het object $shardMap wordt geïnitialiseerd uit de shard map *tenantcatalog* in de catalogusdatabase.
   Er wordt een catalogusobject samengesteld en geretourneerd, dat vervolgens wordt gebruikt in het script op het bovenliggende niveau.
1. **Berekenen van de nieuwe tenant-sleutel**. Er wordt een hash-functie gebruikt om de tenant-sleutel te maken op basis van de naam van de tenant.
1. **Controleren of de tenant-sleutel al bestaat**. De catalogus wordt gecontroleerd om er zeker van te zijn dat de sleutel beschikbaar is.
1. **De tenant-database wordt ingericht met behulp van New-TenantDatabase.** Gebruik **F11** om details van deze stap te bekijken en te zien hoe de database wordt ingericht met behulp van een Resource Manager-sjabloon.
    
De databasenaam wordt samengesteld aan de hand van de naam van de tenant om duidelijk aan te geven welke shard bij welke tenant hoort. (Er kunnen ook heel eenvoudig andere strategieën voor naamgeving van de databases worden gebruikt.)

Er wordt een sjabloon van Resource Manager gebruikt om **een tenant-database te maken door een *golden* database** (baseTenantDB) te kopiëren op de catalogusserver.  Als alternatief voor deze methode kunt u een lege database maken en deze vervolgens initialiseren door een bacpac te importeren.

De Resource Manager-sjabloon bevindt zich in de map …\\Learning Modules\\Common\\:  *tenantdatabasecopytemplate.json*

Nadat de database van de tenant is gemaakt, wordt deze verder geïnitialiseerd met de naam van de venue (tenant) en het type venue. Hier kunnen ook andere initialisaties plaatsvinden.

De tenant-database wordt met behulp van *Add-TenantDatabaseToCatalog* en de tenant-sleutel geregistreerd in de catalogus. Gebruik **F11** om de details van deze bewerkingen te bekijken:

* De catalogusdatabase wordt toegevoegd aan de shard map (de lijst met bekende databases).
* De toewijzing tussen de sleutelwaarde en de shard wordt gemaakt.
* Er worden aanvullende metagegevens (de naam van de venue) van de tenant toegevoegd.

Als het inrichten is voltooid, wordt de uitvoering teruggegeven aan het oorspronkelijke script *Demo ProvisionAndCatalog*, waarna de pagina **Events** voor de nieuwe tenant wordt geopend in de browser:

   ![events](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant2.png)


## <a name="other-provisioning-patterns"></a>Andere inrichtingspatronen

Dit zijn twee inrichtingspatronen die niet aan bod zijn gekomen in deze zelfstudie:

**Vooraf inrichten van databases.** Dit patroon vloeit voort uit het feit dat databases in een elastische pool geen extra kosten met zich meebrengen (de kosten zijn voor de elastische pool, niet voor de databases in de pool) en dat niet-actieve databases geen resources verbruiken. Door databases vooraf in te richten in een pool en ze pas toe te wijzen wanneer ze nodig zijn, kan het inrichten van tenants aanzienlijk sneller verlopen. Het aantal databases dat vooraf wordt ingericht, kan naar wens worden aangepast om zo te beschikken over een buffer die overeenkomt met de verwachte vraag naar nieuwe tenants.

**Automatische inrichting.** Bij dit patroon wordt een speciale inrichtingsservice gebruikt om naar behoefte servers, groepen en databases automatisch in te richten. Deze service kan ook worden gebruikt om databases desgewenst vooraf in te richten in elastische pools. Als databases buiten gebruik worden gesteld en verwijderd, kunnen hiaten in elastische pools indien nodig met de inrichtingsservice worden opgevuld. Een dergelijke service kan eenvoudig of complex zijn. Voorbeelden zijn inrichting van tenants voor verschillende geografische locaties of het automatisch instellen van geo-replicatie als deze strategie wordt gebruikt voor herstel na noodgevallen. In het geval van het patroon voor automatische inrichting wordt door een clienttoepassing of -script een aanvraag voor inrichting in de wachtrij gezet voor verwerking door de inrichtingsservice, waarna de service regelmatig wordt bevraagd om te bepalen of de inrichting is voltooid. Als inrichting vooraf wordt gebruikt, worden aanvragen snel verwerkt, waarbij de service zorgt voor het inrichten van een vervangende database die op de achtergrond wordt uitgevoerd.


## <a name="stopping-wingtip-saas-application-related-billing"></a>Facturering voor de SaaS-toepassing Wingtip stoppen

Als u niet van plan bent om nog een andere zelfstudie met WTP te volgen, is het raadzaam om alle resources te verwijderen en zo de facturering te stoppen. Verwijder de resourcegroep waarin WTP is geïmplementeerd om zo alle gekoppelde resources te verwijderen.

* Blader in de portal naar de resourcegroep van de toepassing en verwijder de groep om alle facturering voor deze WTP-implementatie te stoppen.

## <a name="tips"></a>Tips

* EDCL biedt ook belangrijke functies waarmee clienttoepassingen verbinding kunnen maken met de catalogus en deze kunnen bewerken. U kunt EDCL ook gebruiken om een ADO.NET-verbinding voor een bepaalde sleutelwaarde op te halen, zodat de toepassing met de juiste database verbinding kan maken. Deze verbindingsgegevens worden opgeslagen in de cache van de client om het verkeer naar de catalogusdatabase tot een minimum te beperken en de toepassing sneller te maken.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Eén nieuwe tenant inrichten
> * Verschillende tenants tegelijk inrichten
> * Details bekijken van het inrichten van nieuwe tenants en tenants registreren in de catalogus

[Zelfstudie over prestatiebewaking](sql-database-saas-tutorial-performance-monitoring.md)

## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies die voortbouwen op de eerste implementatie van WTP (Wingtip Tickets Platform)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Clientbibliotheek voor Elastic Database](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [How to Debug Scripts in Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise) (Fouten opsporen in scripts met Windows PowerShell ISE)


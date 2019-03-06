---
title: Nieuwe tenants inrichten in een multitenant-app die gebruikmaakt van Azure SQL Database | Microsoft Docs
description: Meer informatie over het inrichten en catalogiseren van nieuwe tenants in een multitenant SaaS-app van Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 09/24/2018
ms.openlocfilehash: ada0a54f014e4e896c0e91d1eeafb94e6d254d5b
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57445075"
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Leer hoe u nieuwe tenants inrichten en deze in de catalogus registreren

In deze zelfstudie leert u over het inrichten en catalogiseren van SaaS-patronen. U leert ook hoe ze zijn geïmplementeerd in de toepassing Wingtip Tickets SaaS-database-per-tenant. U maakt en initialiseren van de nieuwe tenant-databases en deze in de toepassingscatalogus-tenant te registreren. De catalogus is een database die de toewijzing tussen de SaaS-toepassing veel tenants en hun gegevens onderhoudt. De catalogus speelt een belangrijke rol bij het doorsturen van toepassings- en management-aanvragen met de juiste database.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Eén nieuwe tenant inrichten.
> * Extra tenants tegelijk inrichten.


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De Wingtip Tickets SaaS-database-per-tenant-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en verkennen van de toepassing Wingtip Tickets SaaS-database-per-tenant](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-the-saas-catalog-pattern"></a>Inleiding tot de SaaS-cataloguspatroon

In een database-ondersteuning multitenant SaaS-toepassing is het belangrijk te weten waar informatie voor elke tenant wordt opgeslagen. In de SaaS-cataloguspatroon wordt een catalogusdatabase gebruikt voor het opslaan van de toewijzing tussen elke tenant en de database waarin de gegevens worden opgeslagen. Dit patroon is van toepassing wanneer gegevens van de tenant wordt verdeeld over meerdere databases.

Elke tenant wordt aangeduid met een sleutel in de catalogus, die is toegewezen aan de locatie van de database. In de app Wingtip Tickets, wordt de sleutel gevormd door een hash van de naam van de tenant. Dit schema kunt de app te maken van de sleutel van de naam van de tenant opgenomen in de URL van de toepassing. Andere schema's voor tenant-sleutel kunnen worden gebruikt.  

De catalogus kan de naam of locatie van de database met minimale gevolgen voor de toepassing worden gewijzigd. In een multitenant databasemodel, deze mogelijkheid ook geschikt is voor een tenant verplaatsen tussen databases. De catalogus kan ook worden gebruikt om aan te geven of een tenant of de database offline voor onderhoud of andere acties. Deze mogelijkheid is verkend de [terugzetten één tenant zelfstudie](saas-dbpertenant-restore-single-tenant.md).

De catalogus kunt ook opslaan extra tenant of metagegevens van een database, zoals de versie van het schema, service-plan of sla's die worden aangeboden aan tenants. De catalogus kan andere gegevens waarmee toepassingsbeheer, klantenondersteuning of DevOps opslaan. 

Naast de SaaS-toepassing, kunt de catalogus Databasehulpprogramma's inschakelen. In het voorbeeld van Wingtip Tickets SaaS-database-per-tenant, de catalogus wordt gebruikt voor query's tussen tenants, wat is verkend in de [Ad-hocrapportage zelfstudie](saas-tenancy-cross-tenant-reporting.md). Taakbeheer cross-database wordt verkend de [Schemabeheer](saas-tenancy-schema-management.md) en [Tenant-analytics](saas-tenancy-tenant-analytics.md) zelfstudies. 

De catalogus is in de voorbeelden Wingtip Tickets SaaS geïmplementeerd met behulp van de functies van Shard Management van de [Elastic Database-clientbibliotheek (EDCL)](sql-database-elastic-database-client-library.md). De EDCL is beschikbaar in Java en .NET Framework. De EDCL kan een toepassing te maken, beheren en gebruiken van een database-ondersteuning shard-toewijzing. 

Een shard-toewijzing bevat een lijst van shards (databases) en de toewijzing tussen sleutels (tenants) en shards. EDCL functies worden gebruikt tijdens de tenants worden ingericht voor het maken van de vermeldingen in de shard-toewijzing. Ze worden gebruikt tijdens de uitvoering van toepassingen verbinding maken met de juiste database. EDCL caches verbindingsgegevens voor het minimaliseren van verkeer naar de catalogusdatabase en snelheid van de toepassing. 

> [!IMPORTANT]
> De toewijzingsgegevens zijn toegankelijk in de catalogusdatabase, maar *niet bewerken*. Van toewijzingsgegevens met behulp van Elastic Database Client Library-API's alleen bewerken. De toewijzingsgegevens rechtstreeks bewerken kan de catalogus en wordt niet ondersteund.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Inleiding tot het inrichtingsproces SaaS-patroon

Wanneer u een nieuwe tenant in een SaaS-toepassing die gebruikmaakt van een databasemodel met één tenant-toevoegt, moet u een nieuwe tenantdatabase inrichten. De database moet worden gemaakt in de juiste locatie en de servicelaag. Deze worden ook geïnitialiseerd met het juiste schema en de referentiegegevens. En moet deze worden geregistreerd in de catalogus onder de juiste tenant-sleutel. 

Verschillende benaderingen voor het inrichten van de database kunnen worden gebruikt. U kunt SQL-scripts uit te voeren, een bacpac implementeren of de sjabloondatabase van een kopiëren. 

Inrichting van de database moet deel uitmaken van uw strategie voor het beheer van schema. U moet ervoor zorgen dat nieuwe databases die zijn ingericht met de meest recente schema. Deze vereiste is verkend de [Schema management zelfstudie](saas-tenancy-schema-management.md). 

De database-per-tenant-app Wingtip Tickets worden nieuwe tenants ingericht door te kopiëren van de sjabloondatabase van een met de naam _basetenantdb_, die wordt geïmplementeerd op de catalogusserver. Inrichting kan worden geïntegreerd in de toepassing als onderdeel van een registratie-ervaring. Deze functie kan ook worden ondersteund offline met behulp van scripts. Deze zelfstudie worden ingericht met behulp van PowerShell. 

Inrichting scripts kopiëren de _basetenantdb_ database naar een nieuwe tenantdatabase maken in een elastische pool. De tenant-database wordt gemaakt in de tenantserver die is toegewezen aan de _newtenant_ DNS-alias. Deze alias onderhoudt een verwijzing naar de server die wordt gebruikt voor het inrichten van nieuwe tenants en wordt bijgewerkt om te verwijzen naar een herstelserver tenant in de disaster recovery-zelfstudies ([DR met georestore](saas-dbpertenant-dr-geo-restore.md), [DR met geografische replicatie](saas-dbpertenant-dr-geo-replication.md)). De scripts vervolgens initialiseren van de database met de tenant-specifieke gegevens en deze te registreren in de catalogus shard-toewijzing. Tenant-databases zijn opgegeven namen op basis van de naam van de tenant. Deze schematische naam is niet een belangrijk onderdeel van het patroon. De catalogus wordt de tenant-sleutel toegewezen aan de naam van de database, zodat elke naamconventie kan worden gebruikt. 


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De database-per-tenant-toepassing Wingtip Tickets SaaS scripts ophalen

De Wingtip Tickets SaaS-scripts en broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets SaaS-scripts.


## <a name="provision-and-catalog-detailed-walkthrough"></a>Gedetailleerd overzicht van inrichten en catalogiseren

Voeg een onderbrekingspunt toe om te begrijpen hoe de Wingtip Tickets-toepassing implementeert voor nieuwe tenants worden ingericht, en volgt u de werkstroom terwijl u een tenant in te richten.

1. Open in de PowerShell ISE... \\Learning Modules\\ProvisionAndCatalog\\_Demo-ProvisionAndCatalog.ps1_ en stel de volgende parameters:

   * **$TenantName** = de naam van de nieuwe venue (bijvoorbeeld *Bushwillow Blues*).
   * **$VenueType** = een van de vooraf gedefinieerde locatietypen: _blues, classicalmusic, dance, jazz, judo, motor racing, multipurpose, opera, rockmusic, soccer_.
   * **$DemoScenario** = **1**, *één tenant in te richten*.

2. Als u wilt een onderbrekingspunt toevoegen, plaats de cursor ergens op de regel met de melding dat *New-Tenant '*. Druk op F9.

   ![Onderbrekingspunt](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

3. Druk op F5 om het script uitvoert.

4. Nadat de uitvoering van het script wordt gestopt op het onderbrekingspunt, drukt u op F11 naar stap in de code.

   ![Foutopsporing](media/saas-dbpertenant-provision-and-catalog/debug.png)



Uitvoering van het script traceren met behulp van de **Debug** menu-opties. Druk op F10 en F11 naar stap over of de aangeroepen functies. Zie voor meer informatie over het opsporen van fouten in PowerShell-scripts, [Tips over het werken met en het opsporen van fouten in PowerShell-scripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


U moet niet expliciet volgen deze werkstroom. Hierin wordt uitgelegd hoe u fouten opsporen in het script.

* **Importeer de module CatalogAndDatabaseManagement.psm1.** Het biedt een catalogus en een abstractie op tenantniveau ten opzichte van de [Shard Management](sql-database-elastic-scale-shard-map-management.md) functies. Deze module wordt veel van het cataloguspatroon ingekapseld en is waard.
* **Importeer de module SubscriptionManagement.psm1.** Het bevat functies voor het aanmelden bij Azure en selecteren van het Azure-abonnement dat u wilt werken.
* **Ophalen van configuratiedetails.** Get-configuratie met behulp van F11 binnengaat en Zie hoe de app-configuratie wordt opgegeven. De namen en andere app-specifieke waarden worden hierna beschreven. Deze waarden niet worden gewijzigd totdat u bekend met de scripts bent.
* **De catalogusobject ophalen.** Stap in de Get-Catalog, stelt het bericht en retourneert een catalogusobject dat wordt gebruikt in het script op een hoger niveau. Deze functie maakt gebruik van functies van Shard Management die worden geïmporteerd vanuit **AzureShardManagement.psm1**. Het catalogusobject bestaat uit de volgende elementen:

   * $catalogServerFullyQualifiedName wordt samengesteld met behulp van de standaardstam plus uw gebruikersnaam: _catalog -\<gebruiker\>. database.windows .net_.
   * $catalogDatabaseName wordt opgehaald uit de configuratie *tenantcatalog*.
   * Het object $shardMapManager wordt geïnitialiseerd uit de catalogusdatabase.
   * Het object $shardMap wordt geïnitialiseerd uit de shard map _tenantcatalog_ in de catalogusdatabase. Een catalogusobject samengesteld en geretourneerd. Het wordt gebruikt in het script op een hoger niveau.
* **Berekenen van de nieuwe tenantsleutel.** Er wordt een hash-functie gebruikt om de tenant-sleutel te maken op basis van de naam van de tenant.
* **Controleren of de tenant-sleutel al bestaat.** De catalogus wordt gecontroleerd om ervoor te zorgen dat de sleutel beschikbaar is.
* **De tenant-database wordt ingericht met behulp van New-TenantDatabase.** F11 gebruiken om in de manier waarop de database is ingericht met behulp van een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-manager-template-walkthrough.md).

    De databasenaam wordt samengesteld aan de hand van de naam van de tenant om duidelijk aan te geven welke shard bij welke tenant hoort. U kunt ook andere naamgevingsregels database gebruiken. Resource Manager-sjabloon maakt u een tenantdatabase door de sjabloondatabase van een kopiëren (_baseTenantDB_) op de catalogusserver. Als alternatief kunt u een database maken en het door het importeren van een bacpac te initialiseren. Of u kunt een initialisatie-script uitvoeren vanuit een bekende locatie.

    De Resource Manager-sjabloon is in de map van de Modules\Common\ ...\Learning: *tenantdatabasecopytemplate.json*

* **De tenant-database is verder geïnitialiseerd.** De naam van de venue (tenant) en het type venue worden toegevoegd. U kunt ook andere initialisaties hier doen.

* **Database van de tenant is geregistreerd in de catalogus.** Deze geregistreerd bij *Add-TenantDatabaseToCatalog* met behulp van de tenant-sleutel. F11 gebruiken om de details:

    * De catalogusdatabase wordt toegevoegd aan de shard map (de lijst met bekende databases).
    * De toewijzing tussen de sleutelwaarde en de shard wordt gemaakt.
    * Aanvullende metagegevens over de tenant (naam van de venue) wordt toegevoegd aan de tabel Tenants in de catalogus. De tabel Tenants maakt geen deel uit van het schema Shard Management en deze door de EDCL is niet geïnstalleerd. Deze tabel ziet u hoe de catalogusdatabase kan worden uitgebreid ter ondersteuning van aanvullende toepassingsspecifieke gegevens.


Nadat het inrichten is voltooid, kan worden uitgevoerd op de oorspronkelijke retourneert *Demo ProvisionAndCatalog* script. De **gebeurtenissen** pagina wordt geopend voor de nieuwe tenant in de browser.

   ![Pagina gebeurtenissen](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Batch met tenants inrichten

In deze oefening gaat 17 tenants tegelijk. Het is raadzaam dat u deze batch met tenants inrichten voordat u begint met andere zelfstudies Wingtip Tickets SaaS-database-per-tenant. Er zijn meer dan een paar databases om te werken met.

1. Open in de PowerShell ISE... \\Learning Modules\\ProvisionAndCatalog\\*Demo-ProvisionAndCatalog.ps1*. Wijzig de *$DemoScenario* parameter 3:

   * **$DemoScenario** = **3**, *batch met tenants inrichten*.
2. Druk op F5 om het script uitvoert.

Met het script worden verschillende tenants tegelijk ingericht. Hierbij een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-manager-template-walkthrough.md) die Hiermee bepaalt u de batch en delegeert de inrichting van elke database naar een gekoppelde sjabloon. Door op deze manier sjablonen in te zetten, kan Azure Resource Manager als broker optreden voor het inrichtingsproces voor het script. De sjablonen inrichten databases in parallel en verwerk nieuwe pogingen, indien nodig. Het script is idempotent zijn, dus als het mislukt of gestopt voor een bepaalde reden, het opnieuw uitvoeren.

### <a name="verify-the-batch-of-tenants-that-successfully-deployed"></a>Controleer of de batch met tenants die is geïmplementeerd

* In de [Azure-portal](https://portal.azure.com), blader naar de lijst met servers en open de *tenants1* server. Selecteer **SQL-databases**, en controleer of de batch van 17 extra databases is nu in de lijst.

   ![Databaselijst](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Andere inrichtingspatronen

Andere inrichtingspatronen die niet zijn opgenomen in deze zelfstudie:

**Vooraf inrichten van databases**: Het vooraf inrichten patroon vloeit voort uit het feit dat databases in een elastische pool toevoegen geen extra kosten. Hiermee geeft u de kosten zijn voor de elastische pool, niet voor de databases. Niet-actieve databases verbruiken geen resources. U kunt de tijd voor het toevoegen van tenants beperken door vooraf inrichten databases in een pool en het toewijzen van wanneer dit nodig is. Het aantal databases dat vooraf wordt ingericht kan worden aangepast, indien nodig om te beschikken over een buffer geschikt is voor de verwachte frequentie van de inrichting.

**Automatische inrichting**: In het patroon voor automatische inrichting bepalingen een inrichtingsservice servers, pools en databases automatisch nodig. Als u wilt, kunt u vooraf inrichten van databases in elastische pools kunt opnemen. Als databases buiten gebruik gesteld en verwijderd, kunnen de hiaten in elastische pools worden ingevuld met de inrichtingsservice. Een dergelijke service kan worden eenvoudig of complex zijn, zoals het verwerken van inrichting voor verschillende geografische locaties en het instellen van geo-replicatie voor herstel na noodgevallen. 

Met het patroon voor automatische inrichting verzendt een clienttoepassing of -script een aanvraag voor inrichting in een wachtrij om te worden verwerkt door de provisioning-service. Vervolgens de service om te bepalen van de voltooiing worden opgevraagd. Als inrichting vooraf wordt gebruikt, worden aanvragen snel verwerkt. De service voorziet een vervangende database op de achtergrond.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Eén nieuwe tenant inrichten.
> * Extra tenants tegelijk inrichten.
> * Stap in de details van tenants inrichten en registreren in de catalogus.

Probeer de [zelfstudie over prestatiebewaking](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Aanvullende resources

* Aanvullende [zelfstudies die op de toepassing Wingtip Tickets SaaS-database-per-tenant voortbouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Clientbibliotheek voor Elastic Database](sql-database-elastic-database-client-library.md)
* [Fouten opsporen in scripts in de Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

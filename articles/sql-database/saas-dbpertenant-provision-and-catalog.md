---
title: Inrichten van nieuwe tenants in een multitenant-app die gebruikmaakt van Azure SQL Database | Microsoft Docs
description: Informatie over het inrichten en nieuwe tenants in een multitenant SaaS-app van Azure SQL Database-catalogus
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 4ddb870d0513d6834aacf0964c240260f18df0fd
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Informatie over het inrichten van nieuwe tenants en registreert u ze in de catalogus

In deze zelfstudie leert u het inrichten en patronen voor SaaS-catalogus. U leert ook hoe ze geïmplementeerd in de toepassing Wingtip Tickets SaaS-database per tenant. U maakt en initialiseren van de nieuwe tenant-databases en deze in de toepassingscatalogus tenant te registreren. De catalogus is een database die de toewijzing tussen de SaaS-toepassing tal van tenants en hun gegevens onderhoudt. De catalogus speelt een belangrijke rol bij het doorsturen van de toepassing en de management-aanvragen met de juiste database.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Inrichten van een enkele nieuwe tenant.
> * Een batch extra tentants inrichten.


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De database per tenant Wingtip Tickets SaaS app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing van de database per tenant Wingtip Tickets SaaS verkennen](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell is geïnstalleerd. Zie voor meer informatie [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>Inleiding tot het patroon van SaaS-catalogus

In een database back multitenant SaaS-toepassing is het belangrijk te weten waar informatie voor elke tenant wordt opgeslagen. In het patroon van de catalogus SaaS wordt een catalogusdatabase gebruikt om de toewijzing tussen elke tenant en de database waarin gegevens worden opgeslagen. Dit patroon is van toepassing wanneer de gegevens van de tenant is verdeeld over meerdere databases.

Elke tenant wordt aangeduid met een sleutel in de catalogus die is toegewezen aan de locatie van de database. De sleutel wordt in de app Wingtip Tickets gevormd door een hash van de naam van de tenant. Dit schema, kan de app om de sleutel van de naam van de tenant opgenomen in de aanvraag-URL samen te stellen. Andere schema's voor tenant-sleutel kunnen worden gebruikt.  

De catalogus kan de naam of locatie van de database worden gewijzigd met minimale gevolgen voor de toepassing. In een multitenant databasemodel deze mogelijkheid ook geschikt is voor een tenant verplaatsen tussen databases. De catalogus kan ook worden gebruikt om aan te geven of een tenant of de database offline voor onderhoud of andere acties is. Deze mogelijkheid is verkend de [terugzetten één tenant zelfstudie](saas-dbpertenant-restore-single-tenant.md).

De catalogus kunt ook opslaan extra tenant of databasemetagegevens, zoals de schemaversie, service-abonnement of sla's die worden aangeboden aan tenants. De catalogus kan andere informatie waarmee toepassingsbeheer, klantenondersteuning of DevOps opslaan. 

Afgezien van de SaaS-toepassing kunt de catalogus Databasehulpprogramma's inschakelen. In de steekproef Wingtip Tickets SaaS-database per tenant de catalogus wordt gebruikt voor cross-tenant-query is verkend in de [Ad-hoc reporting zelfstudie](saas-tenancy-cross-tenant-reporting.md). Jobbeheer cross-database wordt verkend de [Schemabeheer](saas-tenancy-schema-management.md) en [Tenant analytics](saas-tenancy-tenant-analytics.md) zelfstudies. 

In de voorbeelden Wingtip Tickets SaaS de catalogus wordt geïmplementeerd met behulp van de Shard-beheerfuncties van de [clientbibliotheek voor elastische Database (EDCL)](sql-database-elastic-database-client-library.md). De EDCL is beschikbaar in Java en .NET Framework. De EDCL kan een toepassing maken, beheren en gebruiken van een database back shard-toewijzing. 

Een shard-toewijzing bevat een lijst met shards (databases) en de toewijzing tussen sleutels (tenants) en shards. EDCL functies worden gebruikt tijdens de tenant die wordt ingericht voor het maken van de vermeldingen in de shard-toewijzing. Deze worden gebruikt tijdens de uitvoering door toepassingen voor het verbinding maken met de juiste database. EDCL caches van verbindingsgegevens voor het verkeer naar de catalogusdatabase minimaliseren en de toepassing versnellen. 

> [!IMPORTANT]
> De toewijzingsgegevens is toegankelijk in de catalogusdatabase, maar *niet bewerken*. Bewerken toewijzingsgegevens alleen via elastische Database Client bibliotheek-API's. Rechtstreeks bewerken van de toewijzingsgegevens risico's te beschadigen de catalogus en wordt niet ondersteund.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Inleiding tot het patroon van SaaS-inrichting

Wanneer u een nieuwe tenant in een SaaS-toepassing die gebruikmaakt van een één-tenant databasemodel toevoegt, moet u een nieuwe tenant database inrichten. De database moet worden gemaakt met de juiste locatie en de servicelaag. Deze moet ook worden geïnitialiseerd met het bijbehorende schema en de referentiegegevens. En moet deze worden geregistreerd in de catalogus onder de juiste tenantsleutel. 

Verschillende benaderingen voor het inrichten van de database kunnen worden gebruikt. U kunt SQL-scripts worden uitgevoerd, een bacpac implementeren of de sjabloondatabase van een kopiëren. 

Inrichting van de database moet deel uitmaken van uw strategie voor het beheer van schema. U moet ervoor zorgen dat nieuwe databases zijn ingericht met de meest recente schema. Deze vereiste is verkend de [Schema management zelfstudie](saas-tenancy-schema-management.md). 

De database per tenant Wingtip Tickets app richt nieuwe tenants door te kopiëren van de sjabloondatabase van een met de naam _basetenantdb_, die is geïmplementeerd op de catalogusserver. Inrichting kan worden geïntegreerd in de toepassing als onderdeel van een registratie-ervaring. Deze kan ook worden ondersteund offline met behulp van scripts. Deze zelfstudie behandelt inrichten met behulp van PowerShell. 

Inrichting scripts kopiëren de _basetenantdb_ database naar een nieuwe tenant-database in een elastische pool te maken. De scripts initialiseren van de database met tenant-specifieke informatie vervolgens en deze te registreren in de catalogus shard-toewijzing. Tenant-databases zijn gegeven namen op basis van de naam van de tenant. Deze schematische naam is niet een essentieel onderdeel van het patroon. De catalogus worden de tenantsleutel toegewezen aan de naam van de database, zodat een naamgevingsconventie kan worden gebruikt. 


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De database per tenant-toepassingsscripts Wingtip Tickets SaaS ophalen

De Wingtip Tickets SaaS-scripts en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.


## <a name="provision-and-catalog-detailed-walkthrough"></a>Gedetailleerd overzicht van inrichten en catalogiseren

Een onderbrekingspunt toevoegen om te begrijpen hoe de toepassing Wingtip Tickets nieuwe tenant inrichting implementeert, en volgt u de werkstroom tijdens het inrichten van een tenant.

1. Open in de PowerShell ISE... \\Learning-Modules\\ProvisionAndCatalog\\_Demo ProvisionAndCatalog.ps1_ en de volgende parameters instellen:

   * **$TenantName** = de naam van de nieuwe venue (bijvoorbeeld *Bushwillow Blues*).
   * **$VenueType** = een van de vooraf gedefinieerde wilt typen: _blauwe, classicalmusic, dans, jazz, judo, motor race multipurpose, opera, rockmusic, doel_.
   * **$DemoScenario** = **1**, *inrichten van een enkele tenant*.

2. Als u wilt een onderbrekingspunt toevoegen, plaats de cursor overal op de regel waarin staat dat *New-Tenant '*. Druk op F9.

   ![Breakpoint](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

3. Het script wordt uitgevoerd, druk op F5.

4. Nadat de uitvoering van het script stopt bij het onderbrekingspunt, drukt u op F11 naar stap in de code in.

   ![Foutopsporing](media/saas-dbpertenant-provision-and-catalog/debug.png)



De uitvoering van het script traceren met behulp van de **Debug** menuopties. Druk op F10 en F11 naar stap via of in de opgeroepen functies. Zie voor meer informatie over foutopsporing van PowerShell-scripts [Tips voor het werken met en foutopsporing van PowerShell-scripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


U hoeft niet te volgen expliciet deze workflow. Hierin wordt uitgelegd hoe u het script voor foutopsporing.

* **Importeer de module CatalogAndDatabaseManagement.psm1.** Biedt een catalogus en op tenantniveau abstractie via de [Shard Management](sql-database-elastic-scale-shard-map-management.md) functies. Deze module kapselt veel van de catalogus patroon en waard is.
* **Importeer de module SubscriptionManagement.psm1.** Het bevat functies voor aanmelden bij Azure en het selecteren van het Azure-abonnement dat u wilt werken.
* **Configuratiegegevens worden opgehaald.** Stap in de Get-configuratie met behulp van F11 en Zie hoe de app-configuratie is opgegeven. De namen en andere waarden van app-specifiek worden hier gedefinieerd. U kunt deze waarden niet wijzigen totdat u bekend met de scripts bent.
* **Ophalen van de catalogusobject.** Stap in de Get-catalogus stelt het bericht en een catalogusobject geretourneerd dat wordt gebruikt in het script op een hoger niveau. Hierbij wordt gebruikgemaakt van Shard-Management-functies die worden geïmporteerd vanuit **AzureShardManagement.psm1**. Het catalogusobject bestaat uit de volgende elementen:

   * $catalogServerFullyQualifiedName is opgesteld met behulp van de standaard stam plus uw gebruikersnaam: _catalogus -\<gebruiker\>. database.windows .net_.
   * $catalogDatabaseName wordt opgehaald uit de configuratie *tenantcatalog*.
   * Het object $shardMapManager wordt geïnitialiseerd uit de catalogusdatabase.
   * Het object $shardMap wordt geïnitialiseerd uit de shard map _tenantcatalog_ in de catalogusdatabase. Een catalogusobject is samengesteld en geretourneerd. Het wordt gebruikt in het script op een hoger niveau.
* **De nieuwe tenantsleutel berekenen.** Er wordt een hash-functie gebruikt om de tenant-sleutel te maken op basis van de naam van de tenant.
* **Controleer of de tenantsleutel al bestaat.** De catalogus wordt gecontroleerd om ervoor te zorgen dat de sleutel beschikbaar is.
* **De tenant-database wordt ingericht met behulp van New-TenantDatabase.** Gebruik F11 naar stap in de manier waarop de database is ingericht met behulp van een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-manager-template-walkthrough.md).

    De databasenaam wordt samengesteld aan de hand van de naam van de tenant om duidelijk aan te geven welke shard bij welke tenant hoort. U kunt ook andere naamgevingsregels database gebruiken. Resource Manager-sjabloon maakt u een tenant-database door het kopiëren van een sjabloondatabase (_baseTenantDB_) op de catalogusserver. Als alternatief kunt u een database maken en het door het importeren van een bacpac initialiseren. Of u kunt een script voor initialisatie van een bekende locatie uitvoeren.

    De Resource Manager-sjabloon bevindt zich in de map van de Modules\Common\ ...\Learning: *tenantdatabasecopytemplate.json*

* **De tenant-database is meer geïnitialiseerd.** De naam van de wetten (tenant) en het type u wilt worden toegevoegd. U kunt ook andere initialisatie hier doen.

* **De database van de tenant is geregistreerd in de catalogus.** Deze is geregistreerd bij *toevoegen TenantDatabaseToCatalog* met behulp van de tenantsleutel. Gebruik F11 voor de stap in de details:

    * De catalogusdatabase wordt toegevoegd aan de shard map (de lijst met bekende databases).
    * De toewijzing tussen de sleutelwaarde en de shard wordt gemaakt.
    * Aanvullende metagegevens over de tenant (de wetten name) is toegevoegd aan de tabel Tenants in de catalogus. De Tenants tabel maakt geen deel uit van het schema voor de Shard-beheer en het door de EDCL is niet geïnstalleerd. Deze tabel ziet u hoe de catalogusdatabase kan worden uitgebreid ter ondersteuning van aanvullende toepassingsspecifieke gegevens.


Nadat het inrichten is voltooid, worden uitgevoerd op de oorspronkelijke retourneert *Demo ProvisionAndCatalog* script. De **gebeurtenissen** pagina wordt geopend voor de nieuwe tenant in de browser.

   ![Pagina gebeurtenissen](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Een batch van tenants inrichten

In deze oefening voorziet in een batch 17 tenants. Het is raadzaam dat u deze batch van tenants inrichten voordat u andere zelfstudies Wingtip Tickets SaaS-database per tenant. Er zijn meer dan een paar databases met werkt.

1. Open in de PowerShell ISE... \\Learning-Modules\\ProvisionAndCatalog\\*Demo ProvisionAndCatalog.ps1*. Wijzig de *$DemoScenario* parameter in 3:

   * **$DemoScenario** = **3**, *inrichten van een batch van tenants*.
2. Het script wordt uitgevoerd, druk op F5.

Met het script worden verschillende tenants tegelijk ingericht. Dit maakt gebruik van een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-manager-template-walkthrough.md) die de batch regelt en het inrichten van elke database naar een gekoppelde sjabloon delegeert. Door op deze manier sjablonen in te zetten, kan Azure Resource Manager als broker optreden voor het inrichtingsproces voor het script. De sjablonen inrichten databases in parallelle instructies en ingang nieuwe pogingen, indien nodig. Het script is idempotent, dus als dit mislukt of gestopt om welke reden, opnieuw uitvoeren.

### <a name="verify-the-batch-of-tenants-that-successfully-deployed"></a>Controleer of de batch van tenants die geïmplementeerd

* In de [Azure-portal](https://portal.azure.com), aan de lijst met servers bladeren en open de *tenants1* server. Selecteer **SQL-databases**, en controleer of de batch van 17 extra databases nu in de lijst.

   ![databaselijst](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Andere inrichtingspatronen

Andere inrichting patronen niet opgenomen in deze zelfstudie:

**Databases vooraf inrichten**: het vooraf inrichten patroon misbruik maakt van het feit dat er geen databases in een elastische pool toevoegen extra kosten verbonden. Facturering is voor de elastische groep, niet de databases. Niet-actieve databases geen resources in beslag nemen. Vooraf inrichten databases in een pool en het toewijzen van wanneer dit nodig is, kunt u de tijd voor het toevoegen van tenants beperken. Het aantal databases dat vooraf is ingericht kan worden aangepast naar behoefte te houden van een buffer geschikt is voor de verwachte inrichting snelheid.

**Automatische inrichting**: In het patroon automatische inrichting een inrichting service voorziet servers, -adresgroepen en -databases automatisch nodig. Als u wilt, kunt u de databases in een elastische pools vooraf inrichten kunt opnemen. Als de databases worden gesteld en wordt verwijderd, kunnen u mogelijke hiaten in de elastische pools door de inrichting service invullen. Deze service is eenvoudig of complex zijn, zoals het verwerken van de inrichting voor meerdere locaties en geo-replicatie voor herstel na noodgevallen in te stellen. 

Verzendt een inrichting aanvraag voor een wachtrij kan worden verwerkt door de inrichting service met het patroon automatische inrichting een clienttoepassing of script. Vervolgens de service om te bepalen van de voltooiing worden opgevraagd. Als vooraf inrichten wordt gebruikt, worden aanvragen snel verwerkt. De service voorziet in een vervanging-database op de achtergrond.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Inrichten van een enkele nieuwe tenant.
> * Een batch extra tentants inrichten.
> * Stap in de details van de inrichting van tenants en geregistreerd in de catalogus.

Probeer de [prestaties bewaking zelfstudie](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Aanvullende resources

* Aanvullende [zelfstudies die zijn gebaseerd op de toepassing Wingtip Tickets SaaS-database per tenant](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Clientbibliotheek voor Elastic Database](sql-database-elastic-database-client-library.md)
* [Fouten opsporen in scripts in de Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

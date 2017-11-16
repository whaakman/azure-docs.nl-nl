---
title: Inrichten van nieuwe tenants in een multitenant-app die gebruikmaakt van Azure SQL Database | Microsoft Docs
description: Informatie over het inrichten en nieuwe tenants in een Azure SQL Database multitenant SaaS-app-catalogus
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: sstein
ms.openlocfilehash: 909be4e9d0586a71d6ebbf55c3d62400513b6228
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="learn-how-to-provision-and-catalog-new-tenants-and-register-them-in-the-catalog"></a>Informatie over het inrichten en nieuwe tenants catalogus en registreert u ze in de catalogus

In deze zelfstudie leert u de SaaS-patronen inrichten en de catalogus, en hoe ze worden geïmplementeerd in de Wingtip SaaS-toepassing. U maakt en initialiseren van de nieuwe tenant-databases en deze in de toepassingscatalogus tenant te registreren. De catalogus is een database die de toewijzing tussen de SaaS-toepassing tal van tenants en hun gegevens onderhoudt. De catalogus speelt een belangrijke rol doorsturen van aanvragen van toepassingen met de juiste database.  

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Een enkele nieuwe tenant, met inbegrip van hoe deze wordt geïmplementeerd doorlopen inrichten
> * Verschillende tenants tegelijk inrichten


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De Wingtip SaaS-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en Verken de Wingtip SaaS-toepassing](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-the-saas-catalog-pattern"></a>Informatie over het SaaS-cataloguspatroon

In een database back multitenant SaaS-toepassing is het belangrijk te weten waar informatie voor elke tenant wordt opgeslagen. In het patroon van de catalogus SaaS een catalogusdatabase wordt gebruikt om de toewijzing tussen elke tenant en waar hun gegevens worden opgeslagen. De Wingtip SaaS-app gebruikmaakt van een één-tenant per database-architectuur, maar het basispatroon van toewijzing van de tenant naar database opslaan in een catalogus van toepassing of een database met meerdere of één tenant wordt gebruikt.

Elke tenant een sleutel die ze kunnen worden geïdentificeerd in de catalogus is toegewezen en die is toegewezen aan de locatie van de juiste database. De sleutel wordt in de app Wingtip SaaS gevormd door een hash van de naam van de tenant. Hierdoor kan de tenant-gedeelte van de URL van de toepassing moet worden gebruikt om de sleutel samen te stellen. Andere schema's voor tenant-sleutel kunnen worden gebruikt.  

De catalogus kan de naam of locatie van de database worden gewijzigd met minimale gevolgen voor de toepassing.  In een multitenant databasemodel dit ook geschikt is voor 'verplaatsen' een tenant tussen databases.  De catalogus kan ook worden gebruikt om aan te geven of een tenant of de database offline voor onderhoud of andere acties is. Dit is verkend de [herstellen van de zelfstudie voor één tenant](saas-dbpertenant-restore-single-tenant.md).

De catalogus, een Beheerdatabase met van een SaaS-toepassing actief is, kan bovendien aanvullende tenant of database-metagegevens, zoals de lagen of editie van een database, schemaversie, service-abonnement of sla's die worden aangeboden aan tenants en andere informatie waarmee toepassingsbeheer, klantenondersteuning of devops-processen worden opgeslagen.  

Afgezien van de SaaS-toepassing kunt de catalogus Databasehulpprogramma's inschakelen.  In de steekproef Wingtip SaaS de catalogus wordt gebruikt voor cross-tenant query, verkend de [ad-hoc analytics zelfstudie](saas-tenancy-adhoc-analytics.md). Jobbeheer cross-database wordt verkend de [Schemabeheer](saas-tenancy-schema-management.md) en [tenant analytics](saas-tenancy-tenant-analytics.md) zelfstudies. 

In de app Wingtip SaaS de catalogus wordt geïmplementeerd met behulp van de Shard-beheerfuncties van de [elastische Database Client bibliotheek (EDCL)](sql-database-elastic-database-client-library.md). De EDCL kan een toepassing maken, beheren en gebruiken van een database back shard-toewijzing. Een shard-toewijzing bevat een lijst met shards (databases) en de toewijzing tussen sleutels (tenants) en databases.  EDCL functies kunnen worden gebruikt vanuit toepassingen of PowerShell-scripts tijdens de tenant die wordt ingericht voor het maken van de vermeldingen in de shard-toewijzing en toepassingen van efficiënt verbinding maken met de juiste database. EDCL caches van verbindingsgegevens voor het verkeer naar de catalogusdatabase minimaliseren en de toepassing versnellen.  

> [!IMPORTANT]
> De toewijzingsgegevens zijn toegankelijk in de catalogusdatabase, maar u mag ze *niet bewerken*. Wijzig de toewijzingsgegevens alleen via API's van EDCL. Als u de toewijzingsgegevens rechtstreeks bewerkt, kan de catalogus worden beschadigd. Deze manier van bewerken wordt daarom niet ondersteund.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Inleiding tot het inrichten van SaaS-patroon

Bij het voorbereiden van een nieuwe tenant in een SaaS-toepassing die gebruikmaakt van moet een databasemodel voor één tenant-een nieuwe tenant-database worden ingericht.  Het moet worden gemaakt met de juiste locatie en de servicelaag geïnitialiseerd met het bijbehorende schema- en referentiegegevens en vervolgens geregistreerd in de catalogus onder de juiste tenantsleutel.  

Verschillende benaderingen kunnen worden gebruikt in database-inrichting, waaronder bijvoorbeeld SQL-scripts uitvoeren, een bacpac implementeren of kopiëren van een sjabloondatabase van de 'gouden.  

De inrichting aanpak die u gebruikt moet worden comprehended in de algemene schema beheerstrategie, die u ervoor zorgen moet dat nieuwe databases zijn ingericht met de meest recente schema.  Dit is verkend de [schema management zelfstudie](saas-tenancy-schema-management.md).  

Wingtip SaaS app bepalingen nieuwe tenants door het kopiëren van een gouden database met de naam basetenantdb, op de GC-server geïmplementeerd.  Inrichting kan worden geïntegreerd in de toepassing als onderdeel van een aanmelding ervaring en/of ondersteunde offline met behulp van scripts. Deze zelfstudie verkennen inrichten met behulp van PowerShell. De inrichting scripts Kopieer de basetenantdb voor het maken van een nieuwe tenant-database in een elastische pool en vervolgens het met tenant-specifieke gegevens initialiseren en registreren in de catalogus shard-toewijzing.  In de voorbeeld-app databases op basis van de naam van de tenant namen zijn opgegeven, maar dit is geen een essentieel onderdeel van het patroon: het gebruik van de catalogus kunnen elke willekeurige naam moet worden toegewezen aan de database. + 


## <a name="get-the-wingtip-application-scripts"></a>De scripts van de Wingtip-toepassing downloaden

De Wingtip SaaS-scripts en de broncode van toepassing zijn beschikbaar in de [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-opslagplaats. [Stappen voor het downloaden van de scripts Wingtip SaaS](saas-dbpertenant-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts).


## <a name="provision-and-catalog-detailed-walkthrough"></a>Gedetailleerd overzicht van inrichten en catalogiseren

Om te begrijpen hoe de toepassing Wingtip nieuwe tenant inrichting implementeert, moet u een onderbrekingspunt en stap via de werkstroom toevoegen tijdens het inrichten van een tenant:

1. Open... \\Learning-Modules\\ProvisionAndCatalog\\_Demo ProvisionAndCatalog.ps1_ en de volgende parameters instellen:
   * **$TenantName** = de naam van de nieuwe venue (bijvoorbeeld *Bushwillow Blues*).
   * **$VenueType** = een van de vooraf gedefinieerde wilt typen: *blauwe*, classicalmusic, dans, jazz, judo, motorracing multipurpose, opera, rockmusic, doel.
   * **$DemoScenario** = **1**, ingesteld op **1** naar *inrichten van een enkele tenant*.

1. Een onderbrekingspunt toevoegen door een willekeurige plaats de cursor in de regel 48, de mededeling dat plaatsen: *New-Tenant '*, en druk op **F9**.

   ![onderbrekingspunt](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

1. Druk op script uitvoeren **F5**.

1. Nadat de uitvoering van het script stopt bij het onderbrekingspunt, drukt u op **F11** naar stap in de code in.

   ![onderbrekingspunt](media/saas-dbpertenant-provision-and-catalog/debug.png)



Traceren van het script kan worden uitgevoerd met behulp van de **Debug** menuopties - **F10** en **F11** naar stap via of in de opgeroepen functies. Zie voor meer informatie over foutopsporing van PowerShell-scripts [Tips voor het werken met en foutopsporing van PowerShell-scripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


De volgende zijn niet stappen expliciet uitvoeren, maar een uitleg van de werkstroom die u tijdens het opsporen van het script hebt doorlopen:

1. **Importeren van de module SubscriptionManagement.psm1** met daarin functies voor het aanmelden bij Azure en het selecteren van het Azure-abonnement waarmee u werkt.
1. **Importeren van de module CatalogAndDatabaseManagement.psm1** die met behulp van de [Shard Management](sql-database-elastic-scale-shard-map-management.md)-functies zorgt voor een abstractie op het niveau van catalogus en tenant. Dit is een belangrijke module waarin veel elementen van het cataloguspatroon aan bod komen en die zeker de moeite waard is om goed te bestuderen.
1. **Ophalen van configuratiedetails**. Stap in de Get-configuratie (met F11) en Zie hoe de app-configuratie is opgegeven. Dit is de plek waar de namen van resources en andere app-specifieke waarden worden gedefinieerd. Het is raadzaam om deze waarden pas te wijzigen als u precies weet hoe de scripts werken.
1. **Ophalen van het catalogusobject**. Stap in Get-catalogus die stelt het bericht en retourneert een catalogusobject dat wordt gebruikt in het script op een hoger niveau.  Hierbij wordt gebruikgemaakt van Shard-Management-functies die worden geïmporteerd vanuit **AzureShardManagement.psm1**. Het catalogusobject bestaat uit het volgende:
   * $catalogServerFullyQualifiedName wordt samengesteld aan de hand van de standaardstam plus uw gebruikersnaam: _catalog-\<gebruiker\>.database.windows.net_.
   * $catalogDatabaseName wordt opgehaald uit de configuratie *tenantcatalog*.
   * Het object $shardMapManager wordt geïnitialiseerd uit de catalogusdatabase.
   * Het object $shardMap wordt geïnitialiseerd uit de shard map *tenantcatalog* in de catalogusdatabase.
   Er wordt een catalogusobject samengesteld en geretourneerd, dat vervolgens wordt gebruikt in het script op het bovenliggende niveau.
1. **Berekenen van de nieuwe tenant-sleutel**. Er wordt een hash-functie gebruikt om de tenant-sleutel te maken op basis van de naam van de tenant.
1. **Controleren of de tenant-sleutel al bestaat**. De catalogus wordt gecontroleerd om er zeker van te zijn dat de sleutel beschikbaar is.
1. **De tenant-database wordt ingericht met behulp van New-TenantDatabase.** Gebruik **F11** stap in en Zie hoe de database is ingericht met behulp van een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-manager-template-walkthrough.md).

De databasenaam wordt samengesteld aan de hand van de naam van de tenant om duidelijk aan te geven welke shard bij welke tenant hoort. (Andere strategieën voor het benoemen van de database kunnen eenvoudig worden gebruikt.) + Een Resource Manager-sjabloon wordt gebruikt voor een tenant-database maken door het kopiëren van een gouden database (baseTenantDB) op de catalogusserver. Een alternatieve methode kan worden naar een lege database maken en deze vervolgens initialiseren door het importeren van een bacpac of uit te voeren een script voor initialisatie vanuit een bekende locatie.  

De Resource Manager-sjabloon bevindt zich in de map van de Modules\Common\ ...\Learning: *tenantdatabasecopytemplate.json*

Nadat de database van de tenant is gemaakt, is deze verdere **geïnitialiseerd met de naam wilt (tenant) en het type wilt**. Hier kunnen ook andere initialisaties plaatsvinden.

De **database van de tenant is geregistreerd in de catalogus** met *toevoegen TenantDatabaseToCatalog* met behulp van de tenantsleutel. Gebruik **F11** om de details van deze bewerkingen te bekijken:

* De catalogusdatabase wordt toegevoegd aan de shard map (de lijst met bekende databases).
* De toewijzing tussen de sleutelwaarde en de shard wordt gemaakt.
* Aanvullende meta-gegevens (naam van de wetten) over de tenant is toegevoegd aan de tabel Tenants in de catalogus.  De Tenants tabel maakt geen deel uit van het schema ShardManagement en niet is geïnstalleerd door de EDCL.  Deze tabel ziet u hoe de catalogus-database kan worden uitgebreid ter ondersteuning van aanvullende toepassingsspecifieke gegevens.   


Nadat het inrichten is voltooid, worden uitgevoerd op de oorspronkelijke retourneert *Demo ProvisionAndCatalog* script, wordt geopend de **gebeurtenissen** pagina voor de nieuwe tenant in de browser:

   ![events](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Een batch van tenants inrichten

In deze oefening voorziet in een batch 17 tenants. Het raadzaam dat u deze batch van tenants inrichten voordat u andere zelfstudies Wingtip SaaS zodat er meer dan een paar databases werken met.

1. Open... \\Learning-Modules\\ProvisionAndCatalog\\*Demo ProvisionAndCatalog.ps1* in de *PowerShell ISE* en wijzig de *$DemoScenario* parameter in 3:
   * **$DemoScenario** = **3**, wijzigt u in **3** naar *inrichten van een batch van tenants*.
1. Druk op **F5** om het script uit te voeren.

Met het script worden verschillende tenants tegelijk ingericht. Het script maakt gebruik van een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-manager-template-walkthrough.md) om de batch met tenants te bepalen, waarna de inrichting van elke database wordt gedelegeerd aan een gekoppelde sjabloon. Door op deze manier sjablonen in te zetten, kan Azure Resource Manager als broker optreden voor het inrichtingsproces voor het script. Waar mogelijk worden de databases gelijktijdig ingericht met de sjablonen. Indien nodig worden bewerkingen opnieuw gestart om het algehele proces zo goed mogelijk te laten verlopen. Het script idempotent is dus als dit mislukt of gestopt om welke reden, opnieuw uitvoeren.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Controleren of de implementatie van de tenants is gelukt

* Open de *tenants1* server door te bladeren naar uw lijst met servers in de [Azure-portal](https://portal.azure.com), klikt u op **SQL-databases**, en controleer of de batch van 17 extra databases worden nu in de lijst:

   ![databaselijst](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Andere inrichtingspatronen

Dit zijn twee inrichtingspatronen die niet aan bod zijn gekomen in deze zelfstudie:

**Vooraf inrichten van databases.** Het vooraf inrichten patroon misbruik maakt van het feit dat de databases in een elastische pool Voeg geen extra kosten verbonden. Facturering voor de elastische groep, niet de databases is en niet-actieve databases geen resources gebruiken. Databases in een pool vooraf worden ingericht en wijzen wanneer dit nodig is, kan de tenant onboarding tijd aanzienlijk worden verkleind. Het aantal databases dat vooraf wordt ingericht, kan naar wens worden aangepast om zo te beschikken over een buffer die overeenkomt met de verwachte vraag naar nieuwe tenants.

**Automatische inrichting.** Een speciale inrichting service wordt in het patroon automatische inrichting gebruikt naar behoefte inrichten servers, -adresgroepen en -databases automatisch – waaronder vooraf inrichten databases in een elastische pools indien gewenst. Als databases buiten gebruik worden gesteld en verwijderd, kunnen hiaten in elastische pools indien nodig met de inrichtingsservice worden opgevuld. Deze service kan eenvoudig of complex – bijvoorbeeld afhandeling van de inrichting voor meerdere locaties en kan geo-replicatie wordt automatisch ingesteld als deze strategie wordt gebruikt voor herstel na noodgevallen. In het geval van het patroon voor automatische inrichting wordt door een clienttoepassing of -script een aanvraag voor inrichting in de wachtrij gezet voor verwerking door de inrichtingsservice, waarna de service regelmatig wordt bevraagd om te bepalen of de inrichting is voltooid. Als inrichting vooraf wordt gebruikt, worden aanvragen snel verwerkt, waarbij de service zorgt voor het inrichten van een vervangende database die op de achtergrond wordt uitgevoerd.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Eén nieuwe tenant inrichten
> * Verschillende tenants tegelijk inrichten
> * Stap in de details van de inrichting van tenants en ze te registreren in de catalogus

Probeer de [prestaties bewaking zelfstudie](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Aanvullende resources

* Aanvullende [zelfstudies waarin voort op de Wingtip SaaS-toepassing bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Clientbibliotheek voor Elastic Database](sql-database-elastic-database-client-library.md)
* [How to Debug Scripts in Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise) (Fouten opsporen in scripts met Windows PowerShell ISE)

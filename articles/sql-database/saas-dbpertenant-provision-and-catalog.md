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
ms.openlocfilehash: b82623f63681daff502f1e23d052da7480dda942
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Informatie over het inrichten van nieuwe tenants en registreert u ze in de catalogus

In deze zelfstudie leert u de SaaS-patronen inrichten en de catalogus, en hoe ze worden geïmplementeerd in de Database Wingtip Tickets SaaS per Tenant-toepassing. U maakt en initialiseren van de nieuwe tenant-databases en deze in de toepassingscatalogus tenant te registreren. De catalogus is een database die de toewijzing tussen de SaaS-toepassing tal van tenants en hun gegevens onderhoudt. De catalogus speelt een belangrijke rol voor het routeren van toepassings- en management-aanvragen met de juiste database.  

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Een enkele nieuwe tenant, met inbegrip van hoe deze wordt geïmplementeerd doorlopen inrichten
> * Verschillende tenants tegelijk inrichten


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De Database Wingtip Tickets SaaS Database per Tenant-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de Database Wingtip Tickets SaaS per Tenant toepassing verkennen](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-the-saas-catalog-pattern"></a>Informatie over het SaaS-cataloguspatroon

In een database back multitenant SaaS-toepassing is het belangrijk te weten waar informatie voor elke tenant wordt opgeslagen. In het patroon van de catalogus SaaS wordt een catalogusdatabase gebruikt om de toewijzing tussen elke tenant en de database waarin gegevens worden opgeslagen. De Wingtip Tickets SaaS-Database per tenant app gebruikmaakt van een enkele tenant per databasemodel, maar het basispatroon van toewijzing van de tenant naar database opslaan in een catalogus van toepassing wanneer de gegevens van de tenant is verdeeld over meerdere databases of een multitenant of Single-tenant-database wordt gebruikt.

Elke tenant een sleutel die ze kunnen worden geïdentificeerd in de catalogus is toegewezen en die is toegewezen aan de locatie van de juiste database. De sleutel wordt in de app Wingtip Tickets SaaS gevormd door een hash van de naam van de tenant. Hierdoor kan de tenant-gedeelte van de URL van de toepassing moet worden gebruikt om de sleutel samen te stellen. Andere schema's voor tenant-sleutel kunnen worden gebruikt.  

De catalogus kan de naam of locatie van de database worden gewijzigd met minimale gevolgen voor de toepassing.  In een multitenant databasemodel dit ook geschikt is voor 'verplaatsen' een tenant tussen databases.  De catalogus kan ook worden gebruikt om aan te geven of een tenant of de database offline voor onderhoud of andere acties is. Dit is verkend de [herstellen van de zelfstudie voor één tenant](saas-dbpertenant-restore-single-tenant.md).

De catalogus, een Beheerdatabase met van een SaaS-toepassing actief is, kan bovendien aanvullende tenant of database-metagegevens, zoals de lagen of editie van een database, schemaversie, service-abonnement of sla's die worden aangeboden aan tenants en andere informatie waarmee toepassingsbeheer, klantenondersteuning of devops-processen worden opgeslagen.  

Afgezien van de SaaS-toepassing kunt de catalogus Databasehulpprogramma's inschakelen.  In de Wingtip Tickets SaaS-Database per Tenant voorbeeld de catalogus wordt gebruikt voor cross-tenant query, verkend de [ad-hoc analytics zelfstudie](saas-tenancy-adhoc-analytics.md). Jobbeheer cross-database wordt verkend de [Schemabeheer](saas-tenancy-schema-management.md) en [tenant analytics](saas-tenancy-tenant-analytics.md) zelfstudies. 

In de voorbeelden Wingtip Tickets SaaS de catalogus wordt geïmplementeerd met behulp van de Shard-beheerfuncties van de [elastische Database Client bibliotheek (EDCL)](sql-database-elastic-database-client-library.md). De EDCL is beschikbaar in Java en .net Framework. De EDCL kan een toepassing maken, beheren en gebruiken van een database back shard-toewijzing. Een shard-toewijzing bevat een lijst met shards (databases) en de toewijzing tussen sleutels (tenants) en shards.  EDCL functies kunnen worden gebruikt vanuit toepassingen of PowerShell-scripts tijdens de tenant die wordt ingericht voor het maken van de vermeldingen in de shard-toewijzing en toepassingen van efficiënt verbinding maken met de juiste database. EDCL caches van verbindingsgegevens voor het verkeer naar de catalogusdatabase minimaliseren en de toepassing versnellen.  

> [!IMPORTANT]
> De toewijzingsgegevens zijn toegankelijk in de catalogusdatabase, maar u mag ze *niet bewerken*. Wijzig de toewijzingsgegevens alleen via API's van EDCL. Als u de toewijzingsgegevens rechtstreeks bewerkt, kan de catalogus worden beschadigd. Deze manier van bewerken wordt daarom niet ondersteund.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Inleiding tot het inrichten van SaaS-patroon

Bij het voorbereiden van een nieuwe tenant in een SaaS-toepassing die gebruikmaakt van moet een databasemodel voor één tenant-een nieuwe tenant-database worden ingericht.  Het moet worden gemaakt met de juiste locatie en de servicelaag geïnitialiseerd met het bijbehorende schema- en referentiegegevens en vervolgens geregistreerd in de catalogus onder de juiste tenantsleutel.  

Verschillende manieren kunnen worden gebruikt database inricht, waaronder bijvoorbeeld SQL-scripts uitvoeren, een bacpac implementeren of kopiëren van een sjabloondatabase.  

De inrichting aanpak die u gebruikt moet worden comprehended in de algemene schema beheerstrategie, die u ervoor zorgen moet dat nieuwe databases zijn ingericht met de meest recente schema.  Dit is verkend de [schema management zelfstudie](saas-tenancy-schema-management.md).  

De Database Wingtip Tickets SaaS per Tenant app richt nieuwe tenants door te kopiëren van de sjabloondatabase van een met de naam _basetenantdb_, geïmplementeerd op de GC-server.  Inrichting kan worden geïntegreerd in de toepassing als onderdeel van een aanmelding ervaring en/of ondersteunde offline met behulp van scripts. Deze zelfstudie behandelt inrichten met behulp van PowerShell. De inrichting scripts Kopieer de basetenantdb database voor het maken van een nieuwe tenant-database in een elastische pool en vervolgens het met tenant-specifieke gegevens initialiseren en registreren in de catalogus shard-toewijzing.  In Wingtip Tickets SaaS Database Per tenant-app krijgen tenant databases namen op basis van de naam van de tenant, maar dit is geen een essentieel onderdeel van het patroon – het gebruik van de catalogus kunnen elke willekeurige naam moet worden toegewezen aan de tenant-databases. + 


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De toepassingsscripts Wingtip Tickets SaaS Database Per Tenant ophalen

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.


## <a name="provision-and-catalog-detailed-walkthrough"></a>Gedetailleerd overzicht van inrichten en catalogiseren

Om te begrijpen hoe de toepassing Wingtip nieuwe tenant inrichting implementeert, moet u een onderbrekingspunt en stap via de werkstroom toevoegen tijdens het inrichten van een tenant:

1. In _PowerShell ISE_open... \\Learning-Modules\\ProvisionAndCatalog\\_Demo ProvisionAndCatalog.ps1_ en de volgende parameters instellen:
   * **$TenantName** = de naam van de nieuwe venue (bijvoorbeeld *Bushwillow Blues*).
   * **$VenueType** = een van de vooraf gedefinieerde wilt typen: *blauwe*, classicalmusic, dans, jazz, judo, motorracing multipurpose, opera, rockmusic, doel.
   * **$DemoScenario** = **1**, naar *inrichten van een enkele tenant*.

1. Een onderbrekingspunt toevoegen door een willekeurige plaats de cursor in de regel 48, de mededeling dat plaatsen: *New-Tenant '*, en druk op **F9**.

   ![onderbrekingspunt](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

1. Druk op script uitvoeren **F5**.

1. Nadat de uitvoering van het script stopt bij het onderbrekingspunt, drukt u op **F11** naar stap in de code in.

   ![Foutopsporing](media/saas-dbpertenant-provision-and-catalog/debug.png)



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

    De databasenaam wordt samengesteld aan de hand van de naam van de tenant om duidelijk aan te geven welke shard bij welke tenant hoort. (Er kunnen ook heel eenvoudig andere strategieën voor naamgeving van de databases worden gebruikt.) Resource Manager-sjabloon wordt gebruikt voor een tenant-database maken door het kopiëren van een sjabloondatabase (baseTenantDB) op de catalogusserver. Een alternatieve methode kan worden naar een lege database maken en deze vervolgens initialiseren door het importeren van een bacpac of uit te voeren een script voor initialisatie vanuit een bekende locatie.  

    De Resource Manager-sjabloon bevindt zich in de map van de Modules\Common\ ...\Learning: *tenantdatabasecopytemplate.json*

1. Nadat de database van de tenant is gemaakt, is deze verdere **geïnitialiseerd met de naam wilt (tenant) en het type wilt**. Hier kunnen ook andere initialisaties plaatsvinden.

1. De **database van de tenant is geregistreerd in de catalogus** met *toevoegen TenantDatabaseToCatalog* met behulp van de tenantsleutel. Gebruik **F11** om de details van deze bewerkingen te bekijken:

    * De catalogusdatabase wordt toegevoegd aan de shard map (de lijst met bekende databases).
    * De toewijzing tussen de sleutelwaarde en de shard wordt gemaakt.
    * Aanvullende metagegevens over de tenant (de wetten name) is toegevoegd aan de tabel Tenants in de catalogus.  De Tenants tabel maakt geen deel uit van het schema ShardManagement en niet is geïnstalleerd door de EDCL.  Deze tabel ziet u hoe de catalogus-database kan worden uitgebreid ter ondersteuning van aanvullende toepassingsspecifieke gegevens.   


Nadat het inrichten is voltooid, worden uitgevoerd op de oorspronkelijke retourneert *Demo ProvisionAndCatalog* script, wordt geopend de **gebeurtenissen** pagina voor de nieuwe tenant in de browser:

   ![events](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Een batch van tenants inrichten

In deze oefening voorziet in een batch 17 tenants. Het raadzaam dat u deze batch van tenants inrichten voordat u andere Wingtip Tickets SaaS-Database per Tenant zelfstudies, zodat er meer dan een paar databases werken met.

1. In de *PowerShell ISE*open... \\Learning-Modules\\ProvisionAndCatalog\\*Demo ProvisionAndCatalog.ps1* en wijzig de *$DemoScenario* parameter in 3:
   * **$DemoScenario** = **3**, naar *inrichten van een batch van tenants*.
1. Druk op **F5** om het script uit te voeren.

Met het script worden verschillende tenants tegelijk ingericht. Het script maakt gebruik van een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-manager-template-walkthrough.md) om de batch met tenants te bepalen, waarna de inrichting van elke database wordt gedelegeerd aan een gekoppelde sjabloon. Door op deze manier sjablonen in te zetten, kan Azure Resource Manager als broker optreden voor het inrichtingsproces voor het script. Waar mogelijk worden de databases gelijktijdig ingericht met de sjablonen. Indien nodig worden bewerkingen opnieuw gestart om het algehele proces zo goed mogelijk te laten verlopen. Het script idempotent is dus als dit mislukt of gestopt om welke reden, opnieuw uitvoeren.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Controleren of de implementatie van de tenants is gelukt

* In de [Azure-portal](https://portal.azure.com), aan de lijst met servers bladeren en open de *tenants1* server. Klik op **SQL-databases** en controleer of de batch van 17 extra databases worden nu in de lijst:

   ![databaselijst](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Andere inrichtingspatronen

Dit zijn twee inrichtingspatronen die niet aan bod zijn gekomen in deze zelfstudie:

**Vooraf inrichten van databases.** Het vooraf inrichten patroon misbruik maakt van het feit dat de databases in een elastische pool Voeg geen extra kosten verbonden. Facturering voor de elastische groep, niet de databases is en niet-actieve databases geen resources gebruiken. Databases in een pool vooraf worden ingericht en wijzen wanneer dit nodig is, kan de tenant onboarding tijd aanzienlijk worden verkleind. Het aantal databases dat vooraf is ingericht kan worden aangepast naar behoefte te houden van een buffer geschikt is voor de verwachte inrichting snelheid.

**Automatische inrichting.** Servers, -adresgroepen en -databases waar nodig automatisch een inrichting service voorziet in het patroon automatische inrichting – inclusief vooraf inrichten databases in een elastische pools indien gewenst. En als de databases zijn opdracht ongedaan en verwijderd, hiaten in de elastische pools kunnen worden gevuld door de inrichting-service. Deze service kan eenvoudig of complex – bijvoorbeeld afhandeling van de inrichting voor meerdere locaties en geo-replicatie voor herstel na noodgevallen kan worden ingesteld. Een aanvraag voor inrichting aan een wachtrij moeten worden verwerkt door de inrichting service verzendt en vervolgens de service om te bepalen van de voltooiing worden opgevraagd met het patroon automatische inrichting een clienttoepassing of script. Als vooraf inrichten wordt gebruikt, zou aanvragen snel worden verwerkt met de service voor het inrichten van een vervanging-database op de achtergrond.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Eén nieuwe tenant inrichten
> * Verschillende tenants tegelijk inrichten
> * Stap in de details van de inrichting van tenants en ze te registreren in de catalogus

Probeer de [prestaties bewaking zelfstudie](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Aanvullende resources

* Aanvullende [zelfstudies waarin voort op de Database Wingtip Tickets SaaS per Tenant-toepassing bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Clientbibliotheek voor Elastic Database](sql-database-elastic-database-client-library.md)
* [How to Debug Scripts in Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise) (Fouten opsporen in scripts met Windows PowerShell ISE)

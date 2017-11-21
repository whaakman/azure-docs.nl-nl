---
title: Inrichten en nieuwe tenants in een SaaS-toepassing met een Azure SQL Database-SQL-database multitenant catalogus | Microsoft Docs
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
ms.date: 11/17/2017
ms.author: billgib
ms.openlocfilehash: f6707b85cc80178da663d7e2b95eeb5c9550789c
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-multi-tenant-sql-database"></a>Inrichten en catalogus nieuwe tenants in een SaaS-toepassing met behulp van een multitenant-SQL-database

In deze zelfstudie leert u patronen voor het inrichten en tenants catalogiseren bij het werken met een databasemodel shard multitenant. 

Een multitenant-schema maakt het mogelijk gegevens van meerdere tenants in één database worden opgeslagen. Gegevens van de tenant is ter ondersteuning van grote aantallen van tenants, verdeeld over meerdere shards of databases. De gegevens voor een tenant één is altijd volledig in één database opgenomen.  Een catalogus wordt gebruikt om de toewijzing van tenants naar databases.   

U kunt ook een aantal databases met slechts één tenant vullen. De voorkeur voor de databases die meerdere tenants bevatten een lagere kosten per tenant ten koste van de isolatie van tenants.  Databases die slechts één tenant bevatten voorkeur isolatie boven kosten.  Databases met meerdere tenants en één tenants kunnen worden gecombineerd in één SaaS-toepassing om te optimaliseren, kosten of isolatie voor elke tenant. Tenants kunnen krijgen hun eigen database wanneer ingericht of later naar hun eigen database kunnen worden verplaatst.

   ![De app shard multitenant-database met tenant-catalogus](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)


## <a name="tenant-catalog-pattern"></a>Tenant catalogus patroon

Als u gegevens van de tenant is verdeeld over meerdere databases, is het belangrijk te weten waar elke tenant gegevens worden opgeslagen. Een catalogusdatabase bevat de toewijzing tussen een tenant en de database waarin gegevens worden opgeslagen.

Elke tenant wordt aangeduid met een sleutel in de catalogus. In de Wingtip Tickets SaaS-apps, wordt de tenantsleutel gevormd door een hash van de naam van de tenant. Hierdoor kan de toepassing op de naam van de tenant extraheren uit een webpagina-URL en gebruik deze voor de verbinding met de juiste database. Andere schema's voor tenant-sleutel kunnen ook worden gebruikt.

Een catalogus met, kunt de naam of locatie van een tenant-database worden gewijzigd na het inrichten zonder de toepassing te verstoren.  In een multitenant databasemodel kan dit worden gebruikt bij het verplaatsen van een tenant tussen databases.  De catalogus kan ook worden gebruikt om aan te geven tot een toepassing of een tenant offline voor onderhoud of andere acties.

De catalogus kan worden uitgebreid voor het opslaan van extra tenant of database-metagegevens, zoals de lagen of editie van een database, de schemaversie van tenantnaam en service-abonnement of SLA en andere informatie toepassingsbeheer, klantenondersteuning of devops inschakelen processen.  

De catalogus kan ook worden gebruikt voor cross-tenant rapportage, Schemabeheer inschakelen en gegevens extraheren voor analytics doeleinden. 

### <a name="elastic-database-client-library"></a>Clientbibliotheek voor Elastic Database 
In de Wingtip Tickets SaaS-apps, de catalogus is geïmplementeerd in de *tenantcatalog* database met de Shard-beheerfuncties van de [elastische Database Client bibliotheek (EDCL)](sql-database-elastic-database-client-library.md). De bibliotheek kan een toepassing maken, beheren en gebruiken van een database back 'shard-toewijzing'. Een shard-toewijzing bevat een lijst met shards (databases) en de toewijzing tussen sleutels (tenants) en shards.  EDCL functies kunnen worden gebruikt vanuit toepassingen of PowerShell-scripts tijdens tenant inrichting te maken van de vermeldingen in de shard-toewijzing en hoger, verbinding maken met de juiste database. De bibliotheek opslaat in de cache van verbindingsgegevens voor het verkeer op de catalogusdatabase minimaliseren en verbinding versnellen. 

> [!IMPORTANT]
> De toewijzingsgegevens zijn toegankelijk in de catalogusdatabase, maar u mag ze *niet bewerken*. Wijzig de toewijzingsgegevens alleen via API's van EDCL. Als u de toewijzingsgegevens rechtstreeks bewerkt, kan de catalogus worden beschadigd. Deze manier van bewerken wordt daarom niet ondersteund.


## <a name="tenant-provisioning-pattern"></a>Tenant inrichting patroon

Bij het inrichten van een nieuwe tenant in de shard-multitenant-databasemodel moet eerst worden bepaald als de tenant worden ingericht in een gedeelde database of een eigen database opgegeven. Als een gedeelde database moet worden vastgesteld als er ruimte in een bestaande database of een nieuwe database is vereist. Als een nieuwe database is vereist, moet u deze voorzien in de juiste locatie en de servicelaag geïnitialiseerd met het juiste schema- en referentiegegevens samenvoegt en vervolgens geregistreerd in de catalogus. Ten slotte kan de toewijzing tenant worden toegevoegd om te verwijzen naar de juiste shard.

Inrichting van de database kan worden bereikt door het uitvoeren van SQL-scripts, een bacpac implementeren of kopiëren van een sjabloondatabase. De Wingtip Tickets SaaS-toepassingen kopiëren een sjabloondatabase voor het maken van nieuwe tenant databases.

De inrichting van aanpak database moet worden comprehended in het algemene schema strategie voor het beheer, dat vereist is om ervoor te zorgen dat nieuwe databases zijn ingericht met de meest recente schema.  Dit is verkend verder in de [schema management zelfstudie](saas-tenancy-schema-management.md).  

De tenant inrichting scripts in deze zelfstudie zijn beide, inrichten van een tenant in een bestaande multitenant-database, en een nieuwe tenant-database. Tenant-gegevens worden vervolgens geïnitialiseerd en vervolgens geregistreerd in de catalogus shard-toewijzing. In de voorbeeld-app zijn databases met een enkele tenant namen op basis van de naam van de tenant. Databases die meerdere tenants bevatten, krijgen een algemeen _tenantsN_ naam terwijl de databases met alleen een één-tenant, de naam van de tenant krijgen. De specifieke naamconventies gebruikt in het voorbeeld zijn niet een essentieel onderdeel van het patroon, zoals het gebruik van een catalogus elke willekeurige naam kunnen moet worden toegewezen aan de database.  

## <a name="provision-and-catalog-tutorial"></a>Zelfstudie voor het inrichten en de catalogus

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Een tenant in te richten in een multitenant-database
> * Een tenant in te richten in een één-tenant-database
> * Een batch van tenants in te richten in multitenant- en één tenant databases
> * Registreren van een database en de huurder toewijzen in een catalogus

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De app Wingtip Tickets SaaS multitenant Database wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS multitenant Database verkennen](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="get-the-wingtip-tickets-management-scripts"></a>De management Wingtip Tickets scripts ophalen

De management-scripts en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub-opslagplaats. <!--See [Steps to download the Wingtip SaaS scripts](saas-tenancy-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts).-->


## <a name="provision-tenant-walkthrough-1"></a>Overzicht van de tenant inrichten #1

Om te begrijpen hoe de toepassing Wingtip Tickets nieuwe tenant inrichting met een database multitenant implementeert, moet u een onderbrekingspunt en stap via de werkstroom toevoegen tijdens het inrichten van een tenant in een gedeelde database wordt met andere tenants:

1. In de _PowerShell ISE_open... \\Learning-Modules\\ProvisionAndCatalog\\_Demo ProvisionAndCatalog.ps1_ en de volgende parameters instellen:
   * **$TenantName** = **Bushwillow blauwe**, de naam van een nieuwe plaats.
   * **$VenueType** = **blauwe**, een van de vooraf gedefinieerde wilt typen: blauwe, classicalmusic, dans, jazz, judo, motorracing multipurpose, opera, rockmusic, doel (kleine letters, zonder spaties).
   * **$DemoScenario** = **1**, naar *inrichten van een tenant in een gedeelde database wordt met andere tenants*.

1. Een onderbrekingspunt toevoegen door een willekeurige plaats de cursor in de regel 38, de mededeling dat plaatsen: *New-Tenant '*, en druk op **F9**.

   ![onderbrekingspunt](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

1. Druk op script uitvoeren **F5**.

1. Nadat de uitvoering van het script stopt bij het onderbrekingspunt, drukt u op **F11** naar stap in de code in.

   ![Fouten opsporen](media/saas-multitenantdb-provision-and-catalog/debug.png)

Traceren van het script kan worden uitgevoerd met behulp van de **Debug** menuopties - **F10** en **F11** naar stap via of in de opgeroepen functies. Zie voor meer informatie over foutopsporing van PowerShell-scripts [Tips voor het werken met en foutopsporing van PowerShell-scripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Hier volgen de belangrijkste elementen van de werkstroom die u tijdens het traceren van het script hebt doorlopen:

* **Berekenen van de nieuwe tenant-sleutel**. Er wordt een hash-functie gebruikt om de tenant-sleutel te maken op basis van de naam van de tenant.
* **Controleren of de tenant-sleutel al bestaat**. De catalogus wordt gecontroleerd om te controleren of dat de sleutel niet al is geregistreerd.
* **Initialiseren van de tenant in de database van de tenant standaard**. De tenant-database wordt bijgewerkt als de nieuwe tenant-informatie wilt toevoegen.  
* **Tenant registreren in de catalogus** de toewijzing tussen de nieuwe tenantsleutel en de bestaande tenants1-database wordt toegevoegd aan de catalogus. 
* **De Tenantnaam van de is toegevoegd aan de catalogus**. De naam wetten wordt toegevoegd aan de tabel Tenants in de catalogus.  Dit toont hoe de catalogus-database kan worden uitgebreid ter ondersteuning van aanvullende toepassingsspecifieke gegevens.
* **Open pagina gebeurtenissen voor de nieuwe tenant**. De *Bushwillow blauwe* pagina gebeurtenissen in de browser wordt geopend:

   ![events](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)


## <a name="provision-tenant-walkthrough-2"></a>Overzicht van de tenant inrichten #2

Nu overzicht het proces bij het maken van een tenant in zijn eigen database:

1. Nog steeds in... \\Learning-Modules\\ProvisionAndCatalog\\_Demo ProvisionAndCatalog.ps1_ de volgende parameters instellen:
   * **$TenantName** = **sequoia grootste**, de naam van een nieuwe plaats.
   * **$VenueType** = **grootste**, een van de vooraf gedefinieerde wilt typen: blauwe, classicalmusic, dans, jazz, judo, motorracing multipurpose, opera, rockmusic, doel (kleine letters, zonder spaties).
   * **$DemoScenario** = **2**, naar *inrichten van een tenant in een gedeelde database wordt met andere tenants*.

1. Een nieuwe onderbrekingspunt toevoegen door een willekeurige plaats de cursor in de regel 57, de mededeling dat plaatsen:  *& &nbsp;$PSScriptRoot\New-TenantAndDatabase '*, en druk op **F9**.

   ![onderbrekingspunt](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

1. Druk op **F5** om het script uit te voeren.

1. Nadat de uitvoering van het script stopt bij het onderbrekingspunt, drukt u op **F11** naar stap in de code in.  Gebruik **F10** en **F11** naar stap en stap in de functies voor het traceren van de uitvoering.

Hier volgen de belangrijkste elementen van de werkstroom die u tijdens het traceren van het script hebt doorlopen:

* **Berekenen van de nieuwe tenant-sleutel**. Er wordt een hash-functie gebruikt om de tenant-sleutel te maken op basis van de naam van de tenant.
* **Controleren of de tenant-sleutel al bestaat**. De catalogus wordt gecontroleerd om te controleren of dat de sleutel niet al is geregistreerd.
* **Maak een nieuwe database van de tenant**. De database is gemaakt met het kopiëren van de *basetenantdb* database gebruiken met Resource Manager-sjabloon.  Naam van de database is gebaseerd op de naam van de tenant.
* **Database toevoegen aan de catalogus**. De database van de tenant is geregistreerd als een shard in de catalogus.
* **Initialiseren van de tenant in de database van de tenant standaard**. De tenant-database wordt bijgewerkt als de nieuwe tenant-informatie wilt toevoegen.  
* **Tenant registreren in de catalogus** de toewijzing tussen de nieuwe tenantsleutel en de *sequoiasoccer* database wordt toegevoegd aan de catalogus.
* **De Tenantnaam van de is toegevoegd aan de catalogus**. De naam wetten wordt toegevoegd aan de tabel Tenants in de catalogus.
* **Open pagina gebeurtenissen voor de nieuwe tenant**. De *Sequoia grootste* pagina gebeurtenissen in de browser wordt geopend:

   ![events](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)


## <a name="provision-a-batch-of-tenants"></a>Een batch van tenants inrichten

In deze oefening richt snel een batch 17 tenants. Het raadzaam dat u deze batch van tenants inrichten voordat u andere zelfstudies Wingtip Tickets zodat er meer dan een paar databases werken met.

1. In de *PowerShell ISE*open... \\Learning-Modules\\ProvisionAndCatalog\\*Demo ProvisionAndCatalog.ps1* en wijzig de *$DemoScenario* parameter in 3:
   * **$DemoScenario** = **3**, naar *een batch van tenants in te richten in een gedeelde database*.
1. Druk op **F5** om het script uit te voeren.


### <a name="verify-the-deployed-set-of-tenants"></a>Controleer of de geïmplementeerde reeks tenants 
U hebt een combinatie van tenants in een gedeelde database wordt geïmplementeerd en tenants in hun eigen databases geïmplementeerd in deze fase. De Azure-portal kan worden gebruikt om te controleren van de databases die zijn gemaakt:  

* In de [Azure-portal](https://portal.azure.com), open de **tenants1-mt -\<gebruiker\>**  server door te bladeren naar de lijst met SQL-servers.  De **SQL-databases** lijst met de gedeelde moet bevatten **tenants1** database en de databases voor de tenants die zich in hun eigen database:

   ![databaselijst](media/saas-multitenantdb-provision-and-catalog/databases.png)

Terwijl de Azure-portal de tenant databases toont, het laten we ziet u de tenants in de gedeelde database. De volledige lijst met tenants ziet u in de pagina van de hub Wingtip Tickets gebeurtenissen.   

* Open de pagina gebeurtenissen Hub in de browser (http:events.wingtip-mt.\<gebruiker\>. trafficmanager.net)  

De volledige lijst van tenants en hun bijbehorende database is beschikbaar in de catalogus. Een SQL-weergave is opgegeven in de database tenantcatalog die lid wordt van de naam van de tenant opgeslagen in de tabel Tenants de naam van de database in de Shard-Management-tabellen. Deze weergave toont mooi aan de waarde van het uitbreiden van de metagegevens die zijn opgeslagen in de catalogus.

* In *SQL Server Management Studio (SSMS)* verbinding maken met de server tenants op **tenants1-onderwerp mt.\<gebruiker\>. database.windows.net**, met aanmelding: **ontwikkelaars** , Wachtwoord:**P@ssword1**

    ![Het dialoogvenster verbinding SSMS](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

* In de *Objectverkenner*, blader naar de weergaven in de *tenantcatalog* database.
* Klik met de rechtermuisknop op *ExtendedTenants* en kies **Selecteer Top 1000 rijen** en noteer de toewijzing tussen tenantnaam en -database voor de verschillende tenants.

    ![De weergave ExtendedTenants in SSMS](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)
      
## <a name="other-provisioning-patterns"></a>Andere inrichtingspatronen

Dit zijn twee inrichtingspatronen die niet aan bod zijn gekomen in deze zelfstudie:

**Vooraf inrichten databases in een elastische pools.** Het vooraf inrichten patroon misbruik maakt van het feit dat de databases in een elastische pool Voeg geen extra kosten verbonden. Facturering voor de elastische groep, niet de databases is en niet-actieve databases geen resources gebruiken. Door de databases in een pool vooraf worden ingericht en wijzen wanneer dit nodig is, worden is de tijd aan on-board tenants in hun eigen database aanzienlijk verminderd. Het aantal databases dat vooraf wordt ingericht, kan naar wens worden aangepast om zo te beschikken over een buffer die overeenkomt met de verwachte vraag naar nieuwe tenants.

**Automatische inrichting.** Een speciale inrichting service wordt in het patroon automatische inrichting gebruikt naar behoefte inrichten servers, -adresgroepen en -databases automatisch – waaronder vooraf inrichten databases in een elastische pools indien gewenst. Als databases buiten gebruik worden gesteld en verwijderd, kunnen hiaten in elastische pools indien nodig met de inrichtingsservice worden opgevuld. Deze service kan eenvoudig of complex – bijvoorbeeld afhandeling van de inrichting voor meerdere locaties en geo-replicatie voor herstel na noodgevallen kan worden ingesteld. Het patroon automatische inrichting een clienttoepassing of script zou een inrichting aanvraag voor een wachtrij kan worden verwerkt door een inrichting service verzenden en zou vervolgens pollen om te bepalen is voltooid. Als vooraf inrichten wordt gebruikt, zou aanvragen snel worden verwerkt met de service beheert het inrichten van een vervanging-database op de achtergrond uitgevoerd.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Eén nieuwe tenant inrichten
> * Verschillende tenants tegelijk inrichten
> * Stap in de details van de inrichting van tenants en ze te registreren in de catalogus

Probeer de [prestaties bewaking zelfstudie](saas-multitenantdb-performance-monitoring.md).

## <a name="additional-resources"></a>Aanvullende bronnen

<!--* Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Clientbibliotheek voor Elastic Database](sql-database-elastic-database-client-library.md)
* [How to Debug Scripts in Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise) (Fouten opsporen in scripts met Windows PowerShell ISE)

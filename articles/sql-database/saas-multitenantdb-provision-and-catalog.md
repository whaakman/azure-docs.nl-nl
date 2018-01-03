---
title: Inrichten in multitenant SaaS Azure | Microsoft Docs
description: Informatie over het inrichten en nieuwe tenants in een Azure SQL Database multitenant SaaS-app-catalogus
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: MightyPen
ms.reviewer: billgib;andrela;genemi
ms.assetid: 
ms.service: sql-database
ms.custom: saas apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: billgib
ms.openlocfilehash: 42bbb6131aa71520410b22af4d74e99a63fe81cf
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-sharded-multi-tenant-azure-sql-database"></a>Inrichten en catalogus nieuwe tenants in een SaaS-toepassing met een shard multitenant Azure SQL database

In dit artikel bevat informatie over de inrichting en catalogiseren van nieuwe tenants in een *multitenant shard database* model of patroon.

In dit artikel bevat twee hoofdonderdelen:

- [Algemene discussie](#goto_2_conceptual) en de inrichting van nieuwe tenants worden gecatalogiseerd.

- [Zelfstudie](#goto_1_tutorial) die worden gemarkeerd met de PowerShell-script-code die kunnen worden behaald de inrichting en gecatalogiseerd.
    - De zelfstudie maakt gebruik van de Wingtip Tickets SaaS-toepassing, aangepast aan het patroon multitenant shard-database.

<a name="goto_2_conceptual"/>

## <a name="database-pattern"></a>Patroon van de database

Deze sectie, plus een aantal meer die volgen, bespreken de concepten van het patroon multitenant shard-database.

In dit model van de multitenant-shard omvatten het tabelschema binnen elke database een tenantsleutel in de primaire sleutel van tabellen die gegevens van de tenant. De tenantsleutel kan elke individuele database voor het opslaan van 0, 1 of vele tenants. Het gebruik van shard databases gemakkelijk het systeem toepassing ter ondersteuning van een zeer groot aantal tenants. Alle gegevens voor een een tenant wordt opgeslagen in een database. Het grote aantal tenants worden verdeeld over de shard-veel databases. Een catalogusdatabase slaat de toewijzing van elke tenant met de database.

#### <a name="isolation-versus-lower-cost"></a>Isolatie versus lagere kosten

Een tenant met een database naar zichzelf leuk vindt de voordelen van isolatie. De tenant kan de database hersteld naar een eerdere datum zonder wordt beperkt door de gevolgen voor andere tenants hebben. Prestaties van de database kan worden afgestemd om te optimaliseren voor de één tenant opnieuw zonder binnendringen met andere tenants. Het probleem is dat isolatie duurder dan het kost om een database deelt met andere tenants.

Wanneer een nieuwe tenant is ingericht, kan deze een database deelt met andere tenants of deze in een eigen nieuwe database kan worden geplaatst. U kunt later van gedachten verandert en verplaats de database naar de andere situatie.

Databases met meerdere tenants en één tenants worden in de dezelfde SaaS-toepassing om te optimaliseren, kosten of isolatie voor elke tenant vermengd.

   ![De app shard multitenant-database met tenant-catalogus](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)

## <a name="tenant-catalog-pattern"></a>Tenant catalogus patroon

Wanneer er twee of meer databases dat ten minste één tenant bevatten, kan de toepassing moet een manier om te ontdekken welke database slaat de tenant van het huidige interesse hebben. Een catalogusdatabase slaat deze toewijzing.

#### <a name="tenant-key"></a>Tenantsleutel

De toepassing Wingtip kunt voor elke tenant een unieke sleutel die de tenantsleutel worden afgeleid. De app haalt de tenantnaam van de webpagina-URL. De app-hashes de naam van de sleutel verkrijgen. De app gebruikt de sleutel voor toegang tot de catalogus. De catalogus kruisverwijzingen informatie over de database waarin de tenant is opgeslagen. De app gebruikmaakt van de database-informatie over verbinding maken. Andere schema's voor tenant-sleutel kunnen ook worden gebruikt.

Een catalogus met, kunt de naam of locatie van een tenant-database worden gewijzigd na het inrichten zonder de toepassing te verstoren. In een multitenant databasemodel de catalogus is geschikt voor een tenant verplaatsen tussen databases.

#### <a name="tenant-metadata-beyond-location"></a>Metagegevens van de tenant buiten de locatie

De catalogus kan ook aangeven of een tenant offline voor onderhoud of andere acties. En de catalogus kan worden uitgebreid voor het opslaan van extra tenant of de databasemetagegevens, zoals de volgende items:
- De servicelaag of de editie van een database.
- De versie van schema van de database.
- Naam van de tenant en bijbehorende SLA (service level agreement).
- Klik hier voor meer informatie over het inschakelen van Toepassingsbeheer, klantenondersteuning of devops-processen.  

De catalogus kan ook worden gebruikt voor cross-tenant rapportage, Schemabeheer inschakelen en gegevens extraheren voor analytics doeleinden. 

### <a name="elastic-database-client-library"></a>Clientbibliotheek voor Elastic Database 

In Wingtip, de catalogus is geïmplementeerd in de *tenantcatalog* database. De *tenantcatalog* wordt gemaakt met de Shard-beheerfuncties van de [elastische Database Client bibliotheek (EDCL)](sql-database-elastic-database-client-library.md). De bibliotheek kan een toepassing maken, beheren en gebruiken een *shard-toewijzing* die is opgeslagen in een database. Een shard-toewijzing kruisverwijzingen de tenantsleutel met de shard, wat betekent dat de shard-database.

Tijdens tenant inrichting, kunnen EDCL functies uit toepassingen of PowerShell-scripts worden gebruikt voor het maken van de vermeldingen in de shard-toewijzing. De functies EDCL kunnen later worden gebruikt om verbinding met de juiste database te. De EDCL caches van verbindingsgegevens voor het verkeer op de catalogusdatabase minimaliseren en het proces van het verbinden van versnellen.

> [!IMPORTANT]
> Voer *niet* Bewerk de gegevens in de catalogusdatabase via directe toegang! Directe updates worden niet ondersteund vanwege de hoge risico van gegevensbeschadiging. In plaats daarvan de toewijzingsgegevens bewerken met behulp van API's EDCL alleen.

## <a name="tenant-provisioning-pattern"></a>Tenant inrichting patroon

#### <a name="checklist"></a>Controlelijst

Als u een nieuwe tenant in te richten in een bestaande gedeelde database wilt, van de gedeelde database moet u de volgende vragen:
- Er onvoldoende ruimte beschikbaar voor de nieuwe tenant?
- Deze heeft tabellen met de benodigde referentiegegevens voor de nieuwe tenant of kan de gegevens worden toegevoegd?
- Heeft dit de juiste variatie van het basisschema voor de nieuwe tenant?
- Is het in de juiste geografische locatie dicht bij de nieuwe tenant?
- Is het op de juiste servicelaag voor de nieuwe tenant?

Als u wilt dat de nieuwe tenant worden geïsoleerd in een eigen database, kunt u dit om te voldoen aan de specificaties voor de tenant maken.

Nadat het inrichten voltooid is, moet u de tenant registreren in de catalogus. Ten slotte kan de toewijzing tenant worden toegevoegd om te verwijzen naar de juiste shard.

#### <a name="template-database"></a>Sjabloondatabase

Inrichting van de database op SQL-scripts uitvoeren, een bacpac implementeren of kopiëren van een sjabloondatabase. De apps Wingtip kopiëren een sjabloondatabase voor het maken van nieuwe tenant databases.

Net als elke toepassing wordt aangepast, Wingtip gedurende een bepaalde periode. Tijd tot tijd moeten Wingtip wijzigingen in de database. Wijzigingen mogelijk zijn de volgende items:
- Nieuwe of gewijzigde schema.
- Een verwijzing naar nieuwe of gewijzigde gegevens.
- Routinematig databaseonderhoudstaken voor optimale appprestaties te garanderen.

Bij een SaaS-toepassing moeten deze wijzigingen op een gecoördineerde wijze worden geïmplementeerd in een mogelijk enorme reeks tenantdatabases. Ze moeten worden opgenomen in het inrichtingsproces deze wijzigingen worden in de toekomst tenant-databases. Deze uitdaging wordt verkend verder in de [schema management zelfstudie](saas-tenancy-schema-management.md).

#### <a name="scripts"></a>Scripts

De tenant inrichting scripts in deze zelfstudie ondersteunen beide van de volgende scenario's:
- Het inrichten van een tenant in een bestaande database gedeeld met andere tenants.
- Het inrichten van een tenant in zijn eigen database.

Tenant-gegevens worden vervolgens geïnitialiseerd en geregistreerd in de catalogus shard-toewijzing. In de voorbeeld-app-databases die meerdere tenants bevatten gegeven een algemene naam zoals *tenants1* of *tenants2*. Databases die een enkele tenant bevatten, krijgen de naam van de tenant. De specifieke naamconventies gebruikt in het voorbeeld zijn niet een essentieel onderdeel van het patroon, zoals het gebruik van een catalogus elke willekeurige naam kunnen moet worden toegewezen aan de database.  

<a name="goto_1_tutorial"/>

## <a name="tutorial-begins"></a>Zelfstudie begint

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een tenant in te richten in een multitenant-database
> * Een tenant in te richten in een één-tenant-database
> * Een batch van tenants in te richten in multitenant- en één tenant databases
> * Registreren van een database en de huurder toewijzen in een catalogus

#### <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

- Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

- De app Wingtip Tickets SaaS multitenant Database wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS multitenant Database verkennen](saas-multitenantdb-get-started-deploy.md)

- Haal de Wingtip scripts en de broncode:
    - De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub-opslagplaats.
    - Zie de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip deblokkeren. 

## <a name="provision-a-tenant-into-a-database-shared-with-other-tenants"></a>Een tenant in te richten in een database *gedeelde* met andere tenants

In deze sectie maakt overzicht u een van de primaire acties voor het inrichten van die door de PowerShell-scripts worden uitgevoerd. Vervolgens kunt u het foutopsporingsprogramma PowerShell ISE Doorloop de scripts wilt zien welke acties in de code.

#### <a name="major-actions-of-provisioning"></a>Belangrijke acties van inrichting

Hier volgen de belangrijkste elementen van de inrichting werkstroom doorlopen:

- **Berekenen van de nieuwe tenantsleutel**: een hash-functie gebruikt voor het maken van de tenantsleutel van de naam van de tenant.
- **Controleer of de tenantsleutel al bestaat**: de catalogus wordt gecontroleerd of de sleutel niet al is geregistreerd.
- **Initialiseren van de tenant in de database van de tenant standaard**: de database van de tenant wordt bijgewerkt als de nieuwe tenant-informatie wilt toevoegen.  
- **Tenant registreren in de catalogus**: de toewijzing tussen de nieuwe tenantsleutel en de bestaande tenants1-database wordt toegevoegd aan de catalogus. 
- **De naam van de tenant toevoegen aan een tabel van de extensie catalogus**: de naam wetten wordt toegevoegd aan de tabel Tenants in de catalogus.  Deze toevoeging ziet u hoe de catalogus-database kan worden uitgebreid ter ondersteuning van aanvullende toepassingsspecifieke gegevens.
- **Open pagina gebeurtenissen voor de nieuwe tenant**: de *Bushwillow blauwe* pagina gebeurtenissen in de browser wordt geopend.

   ![events](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)

#### <a name="debugger-steps"></a>Stappen voor foutopsporing

Om te begrijpen hoe de app Wingtip nieuwe tenant inrichten in een gedeelde database wordt geïmplementeerd, moet u een onderbrekingspunt en stap via de werkstroom toevoegen:

1. In de *PowerShell ISE*open... \\Learning-Modules\\ProvisionTenants\\*Demo ProvisionTenants.ps1* en de volgende parameters instellen:
   - **$TenantName** = **Bushwillow blauwe**, de naam van een nieuwe plaats.
   - **$VenueType** = **blauwe**, een van de vooraf gedefinieerde wilt typen: blauwe, classicalmusic, dans, jazz, judo, motorracing multipurpose, opera, rockmusic, doel (kleine, zonder spaties).
   - **$DemoScenario** = **1**, voor het inrichten van een tenant in een gedeelde database wordt met andere tenants.

2. Een onderbrekingspunt toevoegen door een willekeurige plaats de cursor in de regel 38, de mededeling dat plaatsen: *New-Tenant '*, en druk vervolgens op **F9**.

   ![onderbrekingspunt](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

3. Voer het script door te drukken **F5**.

4. Nadat de uitvoering van het script stopt bij het onderbrekingspunt, drukt u op **F11** naar stap in de code in.

   ![fouten opsporen](media/saas-multitenantdb-provision-and-catalog/debug.png)

5. Traceren van het script kan worden uitgevoerd met behulp van de **Debug** opties voor het menu **F10** en **F11**, naar stap via of in opgeroepen functies.

Zie voor meer informatie over foutopsporing van PowerShell-scripts [Tips voor het werken met en foutopsporing van PowerShell-scripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

## <a name="provision-a-tenant-in-its-own-database"></a>Inrichten van een tenant in de *eigen* database

#### <a name="major-actions-of-provisioning"></a>Belangrijke acties van inrichting

Hier volgen de belangrijkste elementen van de werkstroom die u tijdens het traceren van het script hebt doorlopen:

- **Berekenen van de nieuwe tenantsleutel**: een hash-functie gebruikt voor het maken van de tenantsleutel van de naam van de tenant.
- **Controleer of de tenantsleutel al bestaat**: de catalogus wordt gecontroleerd of de sleutel niet al is geregistreerd.
- **Maak een nieuwe database van de tenant**: de database is gemaakt met het kopiëren van de *basetenantdb* database gebruiken met Resource Manager-sjabloon.  De naam van de nieuwe database is gebaseerd op de naam van de tenant.
- **Database toevoegen aan de catalogus**: de nieuwe database van de tenant is geregistreerd als een shard in de catalogus.
- **Initialiseren van de tenant in de database van de tenant standaard**: de database van de tenant wordt bijgewerkt als de nieuwe tenant-informatie wilt toevoegen.  
- **Tenant registreren in de catalogus**: de toewijzing tussen de nieuwe tenantsleutel en de *sequoiasoccer* database wordt toegevoegd aan de catalogus.
- **De tenantnaam van de is toegevoegd aan de catalogus**: de naam wetten wordt toegevoegd aan de tabel van de extensie Tenants in de catalogus.
- **Open pagina gebeurtenissen voor de nieuwe tenant**: de *Sequoia grootste* pagina gebeurtenissen in de browser wordt geopend.

   ![events](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)

#### <a name="debugger-steps"></a>Stappen voor foutopsporing

Nu zien door het scriptproces bij het maken van een tenant in zijn eigen database:

1. Nog steeds in... \\Learning-Modules\\ProvisionTenants\\*Demo ProvisionTenants.ps1* de volgende parameters instellen:
   - **$TenantName** = **sequoia grootste**, de naam van een nieuwe plaats.
   - **$VenueType** = **grootste**, een van de vooraf gedefinieerde wilt typen: blauwe, classicalmusic, dans, jazz, judo, motorracing multipurpose, opera, rockmusic, doel (kleine letters, zonder spaties).
   - **$DemoScenario** = **2**, voor het inrichten van een tenant in de eigen database.

2. Een nieuwe onderbrekingspunt toevoegen door een willekeurige plaats de cursor in de regel 57, de mededeling dat plaatsen:  *& &nbsp;$PSScriptRoot\New-TenantAndDatabase '*, en druk op **F9**.

   ![onderbrekingspunt](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

3. Voer het script door te drukken **F5**.

4. Nadat de uitvoering van het script stopt bij het onderbrekingspunt, drukt u op **F11** naar stap in de code in.  Gebruik **F10** en **F11** naar stap en stap in de functies voor het traceren van de uitvoering.

## <a name="provision-a-batch-of-tenants"></a>Een batch van tenants inrichten

In deze oefening voorziet in een batch 17 tenants. Het raadzaam dat u deze batch van tenants inrichten voordat u andere zelfstudies Wingtip Tickets start, dus er meer databases zijn werken met.

1. In de *PowerShell ISE*open... \\Learning-Modules\\ProvisionTenants\\*Demo ProvisionTenants.ps1* en wijzig de *$DemoScenario* parameter tot en met 4:
   - **$DemoScenario** = **4**, voor het inrichten van een batch van tenants in een gedeelde database.

2. Druk op **F5** om het script uit te voeren.

### <a name="verify-the-deployed-set-of-tenants"></a>Controleer of de geïmplementeerde reeks tenants 

U hebt een combinatie van tenants in een gedeelde database wordt geïmplementeerd en tenants in hun eigen databases geïmplementeerd in deze fase. De Azure-portal kan worden gebruikt om te controleren van de databases die zijn gemaakt. In de [Azure-portal](https://portal.azure.com), open de **tenants1-mt -\<gebruiker\>**  server door te bladeren naar de lijst met SQL-servers.  De **SQL-databases** lijst met de gedeelde moet bevatten **tenants1** database en de databases voor de tenants die zich in hun eigen database:

   ![databaselijst](media/saas-multitenantdb-provision-and-catalog/Databases.png)

Terwijl de Azure-portal de tenant databases toont, u kunt hier geen raadpleegt de tenants *binnen* de gedeelde database. De volledige lijst met tenants kan worden weergegeven de **gebeurtenissen Hub** webpagina van Wingtip en door te bladeren door de catalogus.

#### <a name="using-wingtip-tickets-events-hub-page"></a>Met behulp van Wingtip Tickets gebeurtenissen hub pagina

Open de pagina gebeurtenissen Hub in de browser (http:events.wingtip-mt.\<gebruiker\>. trafficmanager.net)  

#### <a name="using-catalog-database"></a>Met behulp van de catalogusdatabase

De volledige lijst van tenants en de bijbehorende database voor elk is beschikbaar in de catalogus. Een SQL-weergave is opgegeven dat joins naam van de tenant de databasenaam. De weergave ziet mooi u de waarde van het uitbreiden van de metagegevens die zijn opgeslagen in de catalogus.
- De SQL-weergave is beschikbaar in de database tenantcatalog.
- Naam van de tenant wordt opgeslagen in de tabel Tenants.
- Naam van de database wordt opgeslagen in de Shard-Management-tabellen.

1. In SQL Server Management Studio (SSMS), verbinding maken met de server tenants op **catalogus-onderwerp mt.\<gebruiker\>. database.windows.net**, met aanmelding = **developer**, en het wachtwoord =**P@ssword1**

    ![Het dialoogvenster verbinding SSMS](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

2. Blader in de Objectverkenner SSMS naar de weergaven in de *tenantcatalog* database.

3. Klik met de rechtermuisknop op de weergave *TenantsExtended* en kies **Selecteer Top 1000 rijen**. Noteer de toewijzing tussen tenantnaam en -database voor de verschillende tenants.

    ![De weergave ExtendedTenants in SSMS](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)
      
## <a name="other-provisioning-patterns"></a>Andere inrichtingspatronen

Deze sectie wordt beschreven andere interessante patronen voor inrichting.

#### <a name="pre-provisioning-databases-in-elastic-pools"></a>Databases in een elastische pools vooraf inrichten

Het vooraf inrichten patroon misbruik maakt van het feit dat bij gebruik van elastische pools facturering voor de groep niet de databases is. Databases kunnen dus worden toegevoegd aan een elastische pool, voordat ze nodig zijn, zonder extra kosten verbonden. Deze vooraf geïntegreerde aanzienlijk minder tijd die nodig is voor een tenant in te richten in een database. Het aantal databases dat vooraf is ingericht kan worden aangepast naar behoefte te houden van een buffer geschikt is voor de verwachte inrichting snelheid.

#### <a name="auto-provisioning"></a>Automatische inrichting

Een speciale inrichting service wordt in het patroon automatische inrichting gebruikt voor het inrichten van servers, -adresgroepen en -databases automatisch indien nodig. Deze automatisering omvat het vooraf inrichten van de databases op elastische pools. En als databases worden gesteld en wordt verwijderd, de hiaten die Hiermee maakt u in de elastische pools kunnen worden gevuld door de inrichting service desgewenst.

Dit type geautomatiseerde service kan eenvoudig of complex worden. Bijvoorbeeld, de automatisering kan verwerken, inrichting voor meerdere locaties en geo-replicatie voor herstel na noodgevallen kan worden ingesteld. Met het patroon automatische inrichting zou een clienttoepassing of script een inrichting aanvraag voor een wachtrij kan worden verwerkt door een inrichting service verzenden. Het script zou vervolgens pollen om te detecteren is voltooid. Als vooraf inrichten wordt gebruikt, wordt aanvragen zou worden snel verwerkt tijdens een achtergrondservice de inrichting van de database van een vervanging beheren.

## <a name="additional-resources"></a>Aanvullende resources

<!-- - Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
- [Clientbibliotheek voor Elastic Database](sql-database-elastic-database-client-library.md)
- [How to Debug Scripts in Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise) (Fouten opsporen in scripts met Windows PowerShell ISE)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een enkele nieuwe tenant in te richten in een gedeelde multitenant-database en een eigen database
> * Verschillende tenants tegelijk inrichten
> * Doorloop de details van de inrichting van tenants en ze te registreren in de catalogus

Probeer de [prestaties bewaking zelfstudie](saas-multitenantdb-performance-monitoring.md).


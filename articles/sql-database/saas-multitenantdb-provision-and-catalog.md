---
title: Inrichten in meerdere tenants voor SaaS Azure | Microsoft Docs
description: Meer informatie over het inrichten en catalogiseren van nieuwe tenants in een Azure SQL Database multitenant SaaS-app
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib,andrela,stein
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: e37bc5f46a1a56357e3dff9d1f67de7dcc2537b0
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055302"
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-sharded-multi-tenant-azure-sql-database"></a>Nieuwe tenants inrichten en catalogiseren in een SaaS-toepassing met behulp van een shard Azure SQL database voor meerdere tenants

In dit artikel bevat informatie over het inrichten en catalogiseren van nieuwe tenants in een *multitenant shard-database* model of patroon.

In dit artikel heeft twee hoofdonderdelen:

- [Algemene discussie](#goto_2_conceptual) van het inrichten en catalogiseren van nieuwe tenants.

- [Zelfstudie](#goto_1_tutorial) die de code van het PowerShell-script waarmee het inrichten en catalogiseren worden gemarkeerd.
    - De zelfstudie maakt gebruik van de Wingtip Tickets SaaS-toepassing, aangepast aan het patroon voor multitenant shard-database.

<a name="goto_2_conceptual"/>

## <a name="database-pattern"></a>Database-patroon

In deze sectie, plus een aantal meer die volgen, bespreken de concepten van het patroon voor multitenant shard-database.

In dit multitenant shard-model zoals het tabelschema binnen elke database een tenant-sleutel in de primaire sleutel van de tabellen waarin gegevens van de tenant. De tenant-sleutel kunt elke afzonderlijke database voor het opslaan van 0, 1 of vele tenants. Het gebruik van de shard-databases kunt eenvoudig de toepassing-systeem voor de ondersteuning van een zeer groot aantal tenants. Alle gegevens voor één tenant in de ene database is opgeslagen. Het grote aantal tenants worden verdeeld over de vele shard-databases. Een catalogusdatabase slaat de toewijzing van elke tenant met de database.

#### <a name="isolation-versus-lower-cost"></a>Isolatie en lagere kosten

Een tenant die een database naar zichzelf heeft, kan de voordelen van isolatie. De tenant kan de database hersteld naar een eerdere datum zonder wordt beperkt door de gevolgen voor andere tenants hebben. Prestaties van de database kan worden afgestemd om te optimaliseren voor de ene tenant opnieuw zonder te manipuleren met andere tenants. Het probleem is dat isolatie is duurder dan kost het om een database deelt met andere tenants.

Wanneer een nieuwe tenant is ingericht, kan dit een database deelt met andere tenants of worden geplaatst in een eigen nieuwe database. U kunt later van gedachten veranderen en verplaats de database naar de andere situatie.

Databases met meerdere tenants en één huurders zijn gemengde in dezelfde SaaS-toepassing, om kosten of isolatie voor elke tenant te optimaliseren.

   ![App met tenantcatalogus shard multitenant-database](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)

## <a name="tenant-catalog-pattern"></a>Tenant-cataloguspatroon

Wanneer u twee of meer databases bevatten met elk ten minste één tenant hebt, moet de toepassing een manier om te ontdekken welke database slaat de tenant van de huidige interesse hebben. Een catalogusdatabase slaat deze toewijzing.

#### <a name="tenant-key"></a>tenant-sleutel

Voor elke tenant, kunt de Wingtip-toepassing afleiden van een unieke sleutel, die de tenant-sleutel. De app haalt de tenantnaam uit de URL van de webpagina. De app de naam van het ophalen van de sleutel een hash-waarde. De app maakt gebruik van de sleutel voor toegang tot de catalogus. De catalogus kruisverwijzingen informatie over de database waarin de tenant is opgeslagen. De app gebruikt de gegevens van de database om verbinding te maken. Andere schema's voor tenant-sleutel kunnen ook worden gebruikt.

Met behulp van een catalogus, kunt de naam of locatie van een tenantdatabase worden gewijzigd na het inrichten zonder te onderbreken van de toepassing. In een multitenant-database-model, de catalogus is geschikt voor een tenant verplaatsen tussen databases.

#### <a name="tenant-metadata-beyond-location"></a>Tenantmetagegevens buiten de locatie

De catalogus kan ook duiden of een tenant offline voor onderhoud of andere acties is. En de catalogus kan worden uitgebreid voor het opslaan van extra tenant of metagegevens van een database, zoals de volgende items:
- De servicelaag of de editie van een database.
- De versie van het databaseschema.
- Naam van de tenant en de SLA (service level agreement).
- Klik hier voor meer informatie over het inschakelen van Toepassingsbeheer, klantenondersteuning of devops-processen.  

De catalogus kan ook worden gebruikt voor het Schemabeheer van cross-tenant-rapportage, en gegevens ophalen voor analytics doeleinden. 

### <a name="elastic-database-client-library"></a>Clientbibliotheek voor Elastic Database 

De catalogus is in Wingtip, geïmplementeerd de *tenantcatalog* database. De *tenantcatalog* wordt gemaakt met behulp van de Shard Management-functies van de [Elastic Database Client Library (EDCL)](sql-database-elastic-database-client-library.md). Kan een toepassing maken, beheren en gebruiken van de bibliotheek een *shard-toewijzing* die is opgeslagen in een database. Een shard-toewijzing kruisverwijzingen de tenant-sleutel met de shard, wat betekent dat de shard-database.

Tijdens de tenants worden ingericht, kunnen EDCL functies van toepassingen of PowerShell-scripts worden gebruikt om de vermeldingen in de shard-toewijzing te maken. De functies EDCL kunnen later worden gebruikt verbinding maken met de juiste database. De EDCL caches verbindingsgegevens voor het minimaliseren van het verkeer op de catalogusdatabase en Versnel het proces van het verbinden van.

> [!IMPORTANT]
> Voer *niet* Bewerk de gegevens in de catalogusdatabase tot en met directe toegang. Automatische updates worden niet ondersteund vanwege het hoge risico van gegevensbeschadiging. Bewerken in plaats daarvan de toewijzingsgegevens alleen via EDCL API's.

## <a name="tenant-provisioning-pattern"></a>Inrichting patroon van tenant

#### <a name="checklist"></a>Controlelijst

Wanneer u een nieuwe tenant in te richten in een bestaande gedeelde database, van de gedeelde database moet u de volgende vragen:
- Heeft geen onvoldoende ruimte beschikbaar voor de nieuwe tenant?
- Heeft geen tabellen met de referentiegegevens die nodig zijn voor de nieuwe tenant of kan de gegevens worden toegevoegd?
- Beschikt het over de juiste variatie van het basis-schema voor de nieuwe tenant?
- Is het in de juiste geografische locatie dicht bij de nieuwe tenant?
- Is het op het juiste serviceniveau voor de nieuwe tenant?

Als u wilt dat de nieuwe tenant worden geïsoleerd in een eigen database, kunt u deze om te voldoen aan de specificaties voor de tenant kunt maken.

Nadat het inrichten voltooid is, moet u de tenant registreren in de catalogus. Ten slotte kan de tenanttoewijzing worden toegevoegd om te verwijzen naar de juiste shard geleid.

#### <a name="template-database"></a>Sjabloondatabase

Inrichting van de database op SQL-scripts uitvoeren, het implementeren van een bacpac of kopiëren van de sjabloondatabase van een. De Wingtip-apps, Kopieer de sjabloondatabase van een voor het maken van nieuwe tenant-databases.

Net als elke toepassing Wingtip na verloop van tijd. Soms moet Wingtip wijzigingen in de database. De volgende items kunnen bijvoorbeeld wijzigingen zijn:
- Nieuwe of gewijzigde schema.
- Een verwijzing naar nieuwe of gewijzigde gegevens.
- Onderhoudstaken routineus database om te controleren of de app optimaal presteert.

Bij een SaaS-toepassing moeten deze wijzigingen op een gecoördineerde wijze worden geïmplementeerd in een mogelijk enorme reeks tenantdatabases. Ze moeten worden opgenomen in het inrichtingsproces voor deze wijzigingen worden in toekomstige tenantdatabases. Deze uitdaging is verkend verder in de [schema management zelfstudie](saas-tenancy-schema-management.md).

#### <a name="scripts"></a>Scripts

De tenant inrichting scripts in deze zelfstudie ondersteunen beide van de volgende scenario's:
- Het inrichten van een tenant naar een bestaande database gedeeld met andere tenants.
- In een eigen database te richten als een tenant.

Gegevens van de tenant is vervolgens geïnitialiseerd en geregistreerd in de catalogus shard-toewijzing. In de voorbeeld-app, databases met meerdere tenants, krijgen een algemene naam, zoals *tenants1* of *tenants2*. Databases met één tenant, krijgen de naam van de tenant. De specifieke naamconventies gebruikt in het voorbeeld zijn niet een belangrijk onderdeel van het patroon, zoals het gebruik van een catalogus kan elke naam zijn die worden toegewezen aan de database.  

<a name="goto_1_tutorial"/>

## <a name="tutorial-begins"></a>Zelfstudie begint

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een tenant in te richten in een multitenant-database
> * Een tenant in te richten in een database met één tenant
> * Batch met tenants inrichten in multitenant- en één tenant-databases
> * Een database en een tenanttoewijzing in een catalogus registreren

#### <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

- Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

- De app Wingtip Tickets SaaS multitenant-Database wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS multitenant-Database verkennen](saas-multitenantdb-get-started-deploy.md)

- De Wingtip-scripts en broncode ophalen:
    - De Wingtip Tickets SaaS multitenant-Database-scripts en broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub-opslagplaats.
    - Zie de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip-scripts. 

## <a name="provision-a-tenant-into-a-database-shared-with-other-tenants"></a>Een tenant in te richten in een database *gedeelde* met andere tenants

In deze sectie ziet u een lijst van de belangrijke acties voor het inrichten van die worden uitgevoerd door de PowerShell-scripts. Vervolgens gebruikt u het foutopsporingsprogramma PowerShell ISE de scripts om te zien van de acties in code kunt doorlopen.

#### <a name="major-actions-of-provisioning"></a>Belangrijke acties van het inrichten

Hier volgen de belangrijkste elementen van de inrichting werkstroom doorlopen:

- **Berekenen van de nieuwe tenantsleutel**: een hash-functie wordt gebruikt om u te maken van de tenant-sleutel van de naam van de tenant.
- **Controleren of de tenant-sleutel al bestaat**: de catalogus wordt gecontroleerd om te controleren of de sleutel nog niet is geregistreerd.
- **Initialiseren van de tenant in de database van de tenant standaard**: de tenant-database wordt bijgewerkt om toe te voegen van de informatie over de nieuwe tenant.  
- **Tenant registreren in de catalogus**: de toewijzing tussen de nieuwe tenant-sleutel en de bestaande tenants1-database wordt toegevoegd aan de catalogus. 
- **De naam van de tenant toevoegen aan een tabel van de extensie catalogus**: de naam van de venue wordt toegevoegd aan de tabel Tenants in de catalogus.  Deze toevoeging laat zien hoe de catalogusdatabase kan worden uitgebreid ter ondersteuning van aanvullende toepassingsspecifieke gegevens.
- **Open gebeurtenissen-pagina voor de nieuwe tenant**: de *Bushwillow Blues* pagina gebeurtenissen in de browser wordt geopend.

   ![events](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)

#### <a name="debugger-steps"></a>Stappen voor het foutopsporingsprogramma

Om te begrijpen hoe de Wingtip-app voor nieuwe tenants worden ingericht in een gedeelde database implementeert, moet u een onderbrekingspunt en doorloop de werkstroom toevoegen:

1. In de *PowerShell ISE*, openen... \\Learning Modules\\ProvisionTenants\\*Demo-ProvisionTenants.ps1* en stel de volgende parameters:
   - **$TenantName** = **Bushwillow Blues**, de naam van een nieuwe locatie.
   - **$VenueType** = **blues**, een van de vooraf gedefinieerde typen venues: blues, classicalmusic, dance, jazz, judo, motorracing, multipurpose, opera, rockmusic, soccer (kleine letters, zonder spaties).
   - **$DemoScenario** = **1**, voor het inrichten van een tenant in een gedeelde database met andere tenants.

2. Voeg een onderbrekingspunt toe door op te nemen de cursor ergens op regel 38, de regel met de tekst: *New-Tenant '*, en druk vervolgens op **F9**.

   ![onderbrekingspunt](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

3. Voer het script uit door te drukken **F5**.

4. Nadat de uitvoering van het script wordt gestopt op het onderbrekingspunt, drukt u op **F11** naar stap in de code.

   ![fouten opsporen](media/saas-multitenantdb-provision-and-catalog/debug.png)

5. Uitvoering van het script traceren met behulp van de **Debug** menuopties, **F10** en **F11**, naar boven of naar de aangeroepen functies stap.

Zie voor meer informatie over het opsporen van fouten in PowerShell-scripts, [Tips over het werken met en het opsporen van fouten in PowerShell-scripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

## <a name="provision-a-tenant-in-its-own-database"></a>Inrichten van een tenant in de *eigen* database

#### <a name="major-actions-of-provisioning"></a>Belangrijke acties van het inrichten

Hier volgen de belangrijkste elementen van de werkstroom die u stapsgewijs bij het traceren van het script:

- **Berekenen van de nieuwe tenantsleutel**: een hash-functie wordt gebruikt om u te maken van de tenant-sleutel van de naam van de tenant.
- **Controleren of de tenant-sleutel al bestaat**: de catalogus wordt gecontroleerd om te controleren of de sleutel nog niet is geregistreerd.
- **Maak een nieuwe tenantdatabase**: de database is gemaakt met het kopiëren van de *basetenantdb* database met behulp van Resource Manager-sjabloon.  De naam van de nieuwe database is gebaseerd op de naam van de tenant.
- **Database toevoegen aan de catalogus**: de database van de nieuwe tenant is geregistreerd als een shard in de catalogus.
- **Initialiseren van de tenant in de database van de tenant standaard**: de tenant-database wordt bijgewerkt om toe te voegen van de informatie over de nieuwe tenant.  
- **Tenant registreren in de catalogus**: de toewijzing tussen de nieuwe tenant-sleutel en de *sequoiasoccer* database is toegevoegd aan de catalogus.
- **De naam van tenant is toegevoegd aan de catalogus**: de naam van de venue wordt toegevoegd aan de tabel van de extensie Tenants in de catalogus.
- **Open gebeurtenissen-pagina voor de nieuwe tenant**: de *Sequoia voetbal* pagina gebeurtenissen in de browser wordt geopend.

   ![events](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)

#### <a name="debugger-steps"></a>Stappen voor het foutopsporingsprogramma

Nu stapsgewijs door het scriptproces bij het maken van een tenant in een eigen database:

1. Klik in... \\Learning Modules\\ProvisionTenants\\*Demo-ProvisionTenants.ps1* de volgende parameters instellen:
   - **$TenantName** = **sequoia voetbal**, de naam van een nieuwe locatie.
   - **$VenueType** = **voetbal**, een van de vooraf gedefinieerde typen venues: blues, classicalmusic, dance, jazz, judo, motorracing, multipurpose, opera, rockmusic, soccer (kleine letters, zonder spaties).
   - **$DemoScenario** = **2**, voor het inrichten van een tenant in een eigen database.

2. Voeg een nieuwe onderbrekingspunt toe door het plaatsen van de cursor ergens op regel 57, de regel met de tekst:  *& &nbsp;$PSScriptRoot\New-TenantAndDatabase '*, en druk op **F9**.

   ![onderbrekingspunt](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

3. Voer het script uit door te drukken **F5**.

4. Nadat de uitvoering van het script wordt gestopt op het onderbrekingspunt, drukt u op **F11** naar stap in de code.  Gebruik **F10** en **F11** stap over en stap in de functies voor het traceren van de uitvoering.

## <a name="provision-a-batch-of-tenants"></a>Batch met tenants inrichten

In deze oefening gaat 17 tenants tegelijk. Het raadzaam dat u deze batch met tenants inrichten voordat u begint met andere zelfstudies Wingtip Tickets, zodat er meer databases om te werken met zijn.

1. In de *PowerShell ISE*, openen... \\Learning Modules\\ProvisionTenants\\*Demo-ProvisionTenants.ps1* en wijzig de *$DemoScenario* parameter tot en met 4:
   - **$DemoScenario** = **4**, voor het inrichten van een batch met tenants in een gedeelde database.

2. Druk op **F5** om het script uit te voeren.

### <a name="verify-the-deployed-set-of-tenants"></a>Controleer of de geïmplementeerde set van tenants 

In dit stadium hebt u een combinatie van tenants die zijn geïmplementeerd in een gedeelde database en tenants die zijn geïmplementeerd in hun eigen databases. De Azure-portal kan worden gebruikt om te controleren van de databases die zijn gemaakt. In de [Azure-portal](https://portal.azure.com), open de **tenants1-mt -\<gebruiker\>**  server door te bladeren naar de lijst met SQL-servers.  De **SQL-databases** lijst bevatten de gedeelde **tenants1** database en de databases voor de tenants die zich in hun eigen database:

   ![databaselijst](media/saas-multitenantdb-provision-and-catalog/Databases.png)

Terwijl de Azure-portal de tenant-databases bevat, kunt u de tenants zien *binnen* de gedeelde database. De volledige lijst met tenants kan worden weergegeven de **Events Hub** webpagina van Wingtip, en door te bladeren door de catalogus.

#### <a name="using-wingtip-tickets-events-hub-page"></a>Met behulp van Wingtip Tickets gebeurtenissen hub-pagina

Open de Events Hub-pagina in de browser (http:events.wingtip-mt.\<gebruiker\>. trafficmanager.net)  

#### <a name="using-catalog-database"></a>Met behulp van de catalogusdatabase

De volledige lijst van tenants en de bijbehorende database voor elk is beschikbaar in de catalogus. Een SQL-weergave wordt opgegeven dat joins de naam van de tenant voor de naam van de database. De weergave is mooi ziet u de waarde voor het uitbreiden van de metagegevens die zijn opgeslagen in de catalogus.
- De SQL-weergave is beschikbaar in de database tenantcatalog.
- Naam van de tenant wordt opgeslagen in de tabel Tenants.
- Naam van de database worden opgeslagen in de Shard Management-tabellen.

1. In SQL Server Management Studio (SSMS), verbinding maken met de server tenants op **catalogus mt.\<gebruiker\>. database.windows.net**, aan te melden met = **developer**, en het wachtwoord = **P@ssword1**

    ![Het dialoogvenster SSMS verbinding](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

2. In de Objectverkenner in SSMS, bladert u naar de weergaven in de *tenantcatalog* database.

3. Klik met de rechtermuisknop op de weergave *TenantsExtended* en kies **Selecteer Top 1000 rijen**. Houd er rekening mee de toewijzing tussen de naam van tenant en database voor de verschillende tenants.

    ![ExtendedTenants weergeven in SSMS](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)
      
## <a name="other-provisioning-patterns"></a>Andere inrichtingspatronen

In deze sectie worden andere interessante inrichtingspatronen besproken.

#### <a name="pre-provisioning-databases-in-elastic-pools"></a>Vooraf inrichten van databases in elastische pools

Het vooraf inrichten patroon vloeit voort uit het feit dat bij het gebruik van elastische pools, de kosten voor de groep niet de databases zijn. Databases kunnen dus worden toegevoegd aan een elastische pool voordat ze nodig zijn wel, zonder extra kosten. Deze vooraf geïntegreerde aanzienlijk vermindert de tijd voor een tenant in te richten in een database. Het aantal databases dat vooraf wordt ingericht kan worden aangepast, indien nodig om te beschikken over een buffer geschikt is voor de verwachte frequentie van de inrichting.

#### <a name="auto-provisioning"></a>Automatische inrichting

In het patroon voor automatische inrichting, een speciale inrichtingsservice gebruikt voor het inrichten van servers, pools en databases automatisch naar behoefte. Deze automatisering omvat het vooraf inrichten van databases in elastische pools. En als databases buiten gebruik gesteld en verwijderd, de hiaten die in elastische pools Hiermee door de provisioning-service naar wens kunnen worden gevuld.

Dit type geautomatiseerde service kan eenvoudig of complex zijn. Bijvoorbeeld, de automatisering kan verwerken in verschillende geografische gebieden wordt ingericht en geo-replicatie voor herstel na noodgevallen kunt instellen. Met het patroon voor automatische inrichting, zou een clienttoepassing of -script een aanvraag voor inrichting in een wachtrij om te worden verwerkt door een provisioning-service verzenden. Het script zou regelmatig wordt bevraagd om te detecteren is voltooid. Als inrichting vooraf wordt gebruikt, aanvragen snel verwerkt, terwijl een achtergrondservice beheren het inrichten van een vervangende database.

## <a name="additional-resources"></a>Aanvullende resources

<!-- - Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
- [Clientbibliotheek voor Elastic Database](sql-database-elastic-database-client-library.md)
- [How to Debug Scripts in Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise) (Fouten opsporen in scripts met Windows PowerShell ISE)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Eén nieuwe tenant in te richten in een gedeelde multitenant-database en een eigen database
> * Verschillende tenants tegelijk inrichten
> * Doorloop de details van tenants inrichten en registreren in de catalogus

Probeer de [zelfstudie over prestatiebewaking](saas-multitenantdb-performance-monitoring.md).


---
title: Een Azure SQL Database-schema beheren in een app met meerdere tenants | Microsoft Docs
description: Het schema voor meerdere tenants beheren in een toepassing met meerdere tenants die gebruikmaakt van Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib, sstein
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 55e414a35574f8437a30ffb75aebd82eded14fa9
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57447913"
---
# <a name="manage-schema-in-a-saas-application-that-uses-sharded-multi-tenant-sql-databases"></a>Het schema in een SaaS-toepassing die gebruikmaakt van shard SQL-databases van meerdere tenants beheren

In deze zelfstudie onderzoekt de uitdagingen bij het onderhouden van een hele vloot van databases in een Software als een dienst (SaaS)-toepassing. Oplossingen worden voor airconditioningapparatuur van schemawijzigingen in de vloot van databases gedemonstreerd.

Net als elke toepassing, de Wingtip Tickets SaaS-app na verloop van tijd en de wijzigingen in de database is vereist. Wijzigingen kunnen invloed hebben op schema of een verwijzing gegevens of databaseonderhoudstaken toepassen. Met een SaaS-toepassing met behulp van een database per tenant patroon moeten de wijzigingen worden gecoördineerd in een mogelijk enorme reeks tenantdatabases. Bovendien moet u deze wijzigingen opnemen in de database inrichtingsproces om te controleren of dat ze zijn opgenomen in nieuwe databases nadat ze zijn gemaakt.

#### <a name="two-scenarios"></a>Twee scenario 's

Deze zelfstudie worden de volgende twee scenario's:
- Bijwerken van verwijzingsgegevens implementeren voor alle tenants.
- Een index voor de tabel met de referentiegegevens opnieuw maken.

De [elastische taken](sql-database-elastic-jobs-overview.md) functie van Azure SQL Database wordt gebruikt voor het uitvoeren van deze bewerkingen in de tenant-databases. De taken worden ook uitgevoerd voor de database van de tenant 'template'. In de voorbeeld-app Wingtip Tickets, wordt de sjabloondatabase van deze voor het inrichten van een nieuwe tenantdatabase gekopieerd.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een taakagent.
> * Een T-SQL-query uitvoeren op meerdere tenantdatabases.
> * Naslaginformatie over gegevens in alle tenantdatabases bijwerken.
> * Een index maken voor een tabel in alle tenantdatabases.

## <a name="prerequisites"></a>Vereisten

- De Wingtip Tickets-app voor multitenant-database moet al worden geïmplementeerd:
    - Zie voor instructies, de eerste zelfstudie die resulteert in de app Wingtip Tickets SaaS-multitenant-database:<br />[Een shard multitenant-toepassing die gebruikmaakt van Azure SQL Database implementeren en verkennen](saas-multitenantdb-get-started-deploy.md).
        - Het implementatieproces wordt uitgevoerd voor minder dan vijf minuten.
    - Hebt u de *multitenant shard* versie van de Wingtip geïnstalleerd. De versies voor *zelfstandige* en *per tenant-Database* bieden geen ondersteuning voor deze zelfstudie.

- De meest recente versie van SQL Server Management Studio (SSMS) moet worden geïnstalleerd. [Download en installeer SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- Azure PowerShell moet worden geïnstalleerd. Zie voor meer informatie, [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> Deze zelfstudie wordt gebruikgemaakt van functies van de Azure SQL Database-service die zich in een beperkte Preview-versie ([taken voor Elastic Database](sql-database-elastic-database-client-library.md)). Als u doen in deze zelfstudie wilt, geeft u uw abonnements-ID naar *SaaSFeedback@microsoft.com* met onderwerp = Elastic Jobs Preview. Wanneer u de bevestiging hebt ontvangen dat uw abonnement is ingeschakeld, [downloadt en installeert u de taak-cmdlets van de voorlopige versie](https://github.com/jaredmoo/azure-powershell/releases). Deze Preview-versie is beperkt, dus neem contact op met *SaaSFeedback@microsoft.com* voor vragen of voor ondersteuning.

## <a name="introduction-to-saas-schema-management-patterns"></a>Kennismaking met patronen voor SaaS-schema

De shard multitenant-database-gegevensmodel wordt gebruikt in dit voorbeeld maakt een tenants-database bevat een of meer tenants. In dit voorbeeld worden de mogelijkheden gebruiken van een combinatie van een veel-tenant- en one databases, waardoor een *hybride* model voor het beheren van tenant. Beheren van wijzigingen aan deze databases kan ingewikkeld zijn. [Elastische taken](sql-database-elastic-jobs-overview.md) vergemakkelijkt en het beheer van grote aantallen database. Met elastische taken kunt u veilig en betrouwbaar Transact-SQL-scripts uitvoeren als taken voor een groep van de tenant-databases. De taken zijn onafhankelijk van de gebruikersinteractie of invoer. Deze methode kan worden gebruikt voor het implementeren van wijzigingen aan schema of algemene verwijzen naar gegevens, voor alle tenants in een toepassing. Elastische taken kunnen ook worden gebruikt om een golden sjabloon kopie van de database. De sjabloon wordt gebruikt om te maken van nieuwe tenants, altijd ervoor te zorgen dat de meest recente schema en referentiegegevens worden gebruikt.

![scherm](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Beperkte preview voor Elastische taken

Er is een nieuwe versie van elastische taken; dit is nu een geïntegreerde functie van Azure SQL Database. Van deze nieuwe versie van Elastische taken is momenteel een beperkte preview beschikbaar. De beperkte preview op dit moment ondersteunt het gebruik van PowerShell om een agent voor taken en T-SQL voor het maken en beheren van taken te maken.
> [!NOTE]
> In deze zelfstudie maakt gebruik van functies van de SQL-Database-service die zich in een beperkte preview (elastische databasetaken). Als u doen in deze zelfstudie wilt, geeft u uw abonnements-ID naar SaaSFeedback@microsoft.com met onderwerp = Elastic Jobs Preview. Nadat u een bevestiging dat uw abonnement is ingeschakeld ontvangt, download en installeer de meest recente voorlopige versie taken-cmdlets. Deze Preview-versie is beperkt, dus neem contact op met SaaSFeedback@microsoft.com voor vragen of voor ondersteuning.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Download de broncode van de toepassing Wingtip Tickets SaaS multitenant-Database en -scripts

De Wingtip Tickets SaaS multitenant-Database-scripts en broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) -bibliotheek op GitHub. Zie de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets SaaS-scripts.

## <a name="create-a-job-agent-database-and-new-job-agent"></a>Een taakagent-database en -agent voor nieuwe taken maken

Deze zelfstudie moet u PowerShell gebruiken om de taken en -agent voor taken te maken. Een taakagent maakt, zoals de MSDB-database die wordt gebruikt door SQL Agent, gebruik van een Azure SQL database voor het opslaan van taakdefinities, de taakstatus en de geschiedenis. Nadat de taakagent is gemaakt, kunt u maken en bewaken van taken onmiddellijk.

1. In **PowerShell ISE**Open *... \\Learning Modules\\Schemabeheer\\Demo-SchemaManagement.ps1*.
2. Druk op **F5** om het script uit te voeren.

De *Demo-SchemaManagement.ps1* script aanroepen de *Deploy-SchemaManagement.ps1* script voor het maken van een database met de naam _jobagent_ op de catalogusserver. Het script maakt u de taakagent aan en geeft de _jobagent_ database als een parameter.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Een taak maken om nieuwe referentiegegevens te implementeren voor alle tenants

#### <a name="prepare"></a>Voorbereiden

Van elke tenantdatabase bevat een reeks locatietypen in de **VenueTypes** tabel. Elk type venue definieert het type van de gebeurtenissen die kunnen worden gehost op een locatie. Deze locatietypen komen overeen met de achtergrondafbeeldingen die u in de app voor tenantevenementen ziet.  In deze oefening maakt implementeert u een update op alle databases om toe te voegen van twee nieuwe locatietypen: *Motorcycle Racing* en *Swimming Club*.

Bekijk eerst de typen venues is opgenomen in elke tenantdatabase. Verbinding maken met een van de tenant-databases in SQL Server Management Studio (SSMS) en controleren van de tabel venuetypes bij.  U kunt ook een query in deze tabel in de Query-editor in Azure portal, toegankelijk via de pagina van de database.

1. Open SSMS en maak verbinding met de tenantserver: *tenants1-dpt -&lt;gebruiker&gt;. database.windows.net*
1. Om te bevestigen dat *Motorcycle Racing* en *Swimming Club* **niet** momenteel is opgenomen, blader naar de *contosoconcerthall* database op de *tenants1-dpt -&lt;gebruiker&gt;*  -server en de query de *VenueTypes* tabel.



#### <a name="steps"></a>Stappen

Nu u een taak om bij te werken maken de **VenueTypes** tabel in elke database tenants, door de twee nieuwe locatietypen toe te voegen.

Als u wilt een nieuw project maakt, gebruikt u de reeks taken in het systeem opgeslagen procedures die zijn gemaakt in de _jobagent_ database. De opgeslagen procedures zijn gemaakt toen de taakagent is gemaakt.

1. In SSMS verbinding maken met de tenantserver: tenants1-mt -&lt;gebruiker&gt;. database.windows.net

2. Blader naar de *tenants1* database.

3. Query de *VenueTypes* tabel om te bevestigen dat *Motorcycle Racing* en *Swimming Club* niet zijn nog in de lijst met resultaten.

4. Verbinding maken met de catalog-server is *catalogus-mt -&lt;gebruiker&gt;. database.windows.net*.

5. Verbinding maken met de _jobagent_ -database in de catalogusserver.

6. Open het bestand in SSMS, *... \\Learning Modules\\Schemabeheer\\DeployReferenceData.sql*.

7. Wijzigen van de instructie: Stel @User = &lt;gebruiker&gt; en vervang de waarde van de gebruiker die wordt gebruikt tijdens de implementatie van de toepassing Wingtip Tickets SaaS multitenant-Database.

8. Druk op **F5** om het script uit te voeren.

#### <a name="observe"></a>Observeren

Bekijk de volgende items in de *DeployReferenceData.sql* script:

- **SP\_toevoegen\_doel\_groep** maakt de doelgroepnaam *DemoServerGroup*, en doelleden worden toegevoegd aan de groep.

- **SP\_toevoegen\_doel\_groep\_lid** voegt de volgende items:
    - Een *server* lidtype als doel.
        - Dit is de *tenants1-mt -&lt;gebruiker&gt;*  server waarop zich de databases van de tenants.
        - Met inbegrip van de server bevat de tenant-databases die aanwezig zijn op het moment dat de taak wordt uitgevoerd.
    - Een *database* doel lidtype voor de sjabloondatabase (*basetenantdb*) die zich op *catalogus-mt -&lt;gebruiker&gt;*  -server
    - Een *database* lidtype om op te nemen als doel de *ad-hoc reporting* database die wordt gebruikt in een latere zelfstudie.

- **SP\_toevoegen\_taak** maakt u een taak met de naam *implementatie van referentiegegevens*.

- **SP\_toevoegen\_jobstep** maakt de taakstap met T-SQL-opdracht voor het bijwerken van de referentietabel venuetypes bij.

- De resterende weergaven in het script tonen het bestaan van de objecten en controleren de taakuitvoering. Deze query's gebruiken om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid. De taak de tenants-database-updates en updates van de twee andere databases die de referentietabel bevatten.

In SSMS, blader naar de tenant-database op de *tenants1-mt -&lt;gebruiker&gt;*  server. Query de *VenueTypes* tabel om te bevestigen dat *Motorcycle Racing* en *Swimming Club* zijn nu toegevoegd aan de tabel. Het totale aantal locatietypen moet hebt verhoogd met twee.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Een taak maken voor het beheren van de referentietabelindex

In deze oefening maakt een taak om de index op de primaire sleutel uit de referentie-tabel op alle tenantdatabases opnieuw te maken. Het herbouwen van een index is een typische databasebeheerbewerking die een beheerder kan uitvoeren na het laden van een grote hoeveelheid gegevens laden, om prestaties te verbeteren.

1. In SSMS verbinding maken met _jobagent_ -database in *catalogus-mt -&lt;gebruiker&gt;. database.windows.net* server.

2. Open in SSMS, *... \\Learning Modules\\Schemabeheer\\OnlineReindex.sql*.

3. Druk op **F5** om het script uit te voeren.

#### <a name="observe"></a>Observeren

Bekijk de volgende items in de *OnlineReindex.sql* script:

* **SP\_toevoegen\_taak** maakt een nieuwe taak met de naam *Online indexeren PK\_\_VenueTyp\_\_265E44FD7FD4C885*.

* **SP\_toevoegen\_jobstep** maakt de taakstap met T-SQL-opdracht voor het bijwerken van de index.

* De resterende weergaven in het script controleren de taakuitvoering. Deze query's gebruiken om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid op alle leden van de doel-groep.

## <a name="additional-resources"></a>Aanvullende resources

<!-- TODO: Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->
* [Uitgeschaalde clouddatabases beheren](sql-database-elastic-jobs-overview.md)
* [Uitgeschaalde clouddatabases maken en beheren](sql-database-elastic-jobs-create-and-manage.md)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * De taakagent van een voor het uitvoeren van T-SQL-taken voor meerdere databases maken
> * Naslaginformatie over gegevens in alle tenantdatabases bijwerken
> * Een index in een tabel maken in alle tenantdatabases

Probeer vervolgens de [Ad-hocrapportage zelfstudie](saas-multitenantdb-adhoc-reporting.md) verkennen gedistribueerde query's uitvoeren voor de tenant van databases.


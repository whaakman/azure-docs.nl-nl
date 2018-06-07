---
title: Een Azure SQL Database-schema beheren in een app met meerdere tenants | Microsoft Docs
description: Het schema voor meerdere tenants beheren in een toepassing met meerdere tenants die gebruikmaakt van Azure SQL Database
keywords: zelfstudie sql-database
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 01/03/2018
ms.reviewers: billgib
ms.author: genemi
ms.openlocfilehash: 816cde31e84eeda8110c042f4e0640f12fb4cc53
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34645987"
---
# <a name="manage-schema-in-a-saas-application-that-uses-sharded-multi-tenant-sql-databases"></a>Schema in een SaaS-toepassing die gebruikmaakt van shard multitenant SQL-databases beheren

Deze zelfstudie onderzoekt de uitdagingen bij het beheren van een wagenpark van databases in een toepassing Software als een Service (SaaS). Oplossingen worden voor airconditioningapparatuur uit wijzigingen in het schema voor de wagenpark databases uitgelegd.

Net als elke toepassing, de Wingtip Tickets SaaS-app gedurende een periode wordt aangepast en wijzigingen in de database is vereist. Wijzigingen kunnen invloed schema of de verwijzing naar gegevens of databaseonderhoudstaken toepassen. Met een SaaS-toepassing met behulp van een database per tenant patroon, moeten de wijzigingen worden gecoördineerd in een wagenpark potentieel grote databases van de tenant. Bovendien moet u deze wijzigingen opnemen in de database inrichtingsproces om te controleren of dat ze zijn opgenomen in nieuwe databases, zoals ze zijn gemaakt.

#### <a name="two-scenarios"></a>Twee scenario 's

Deze zelfstudie behandelt de volgende twee scenario's:
- Verwijzing naar Gegevensupdates implementeren voor alle tenants.
- Een index op de tabel met de referentiegegevens opnieuw worden opgebouwd.

De [elastische taken](sql-database-elastic-jobs-overview.md) functie van Azure SQL Database wordt gebruikt om uit te voeren van deze bewerkingen voor tenant-databases. De taken worden ook uitgevoerd voor de database van de tenant 'template'. De sjabloondatabase van deze wordt in de voorbeeld-app van Wingtip Tickets gekopieerd voor het inrichten van een nieuwe tenant-database.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De agent van een taak maken.
> * Een T-SQL-query uitvoeren op meerdere databases van de tenant.
> * Verwijzing naar gegevens in alle tenant databases bijwerken.
> * Een index maken voor een tabel in alle databases van de tenant.

## <a name="prerequisites"></a>Vereisten

- De app Wingtip Tickets-multitenant-database moet al worden geïmplementeerd:
    - Voor instructies raadpleegt u de eerste zelfstudie, waardoor de Wingtip Tickets SaaS multitenant database app:<br />[Implementeren en een shard multitenant-toepassing die gebruikmaakt van Azure SQL Database verkennen](saas-multitenantdb-get-started-deploy.md).
        - Het proces implementeren wordt uitgevoerd voor minder dan vijf minuten.
    - U moet hebben de *shard multitenant* versie van Wingtip geïnstalleerd. De versies voor *zelfstandige* en *Database per tenant* bieden geen ondersteuning voor deze zelfstudie.

- De meest recente versie van SQL Server Management Studio (SSMS) moet worden geïnstalleerd. [Download en installeer SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- Azure PowerShell moet zijn geïnstalleerd. Zie voor meer informatie [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> Deze zelfstudie maakt gebruik van functies van de Azure SQL Database-service die zich in een beperkte preview ([elastische Database taken](sql-database-elastic-database-client-library.md)). Als u wenst te doen in deze zelfstudie, geeft u uw abonnement-ID *SaaSFeedback@microsoft.com* met de onderwerpnaam = elastische taken Preview. Wanneer u de bevestiging hebt ontvangen dat uw abonnement is ingeschakeld, [downloadt en installeert u de taak-cmdlets van de voorlopige versie](https://github.com/jaredmoo/azure-powershell/releases). Dit voorbeeld is beperkt, dus neem contact op met *SaaSFeedback@microsoft.com* voor vragen over of ondersteuning.

## <a name="introduction-to-saas-schema-management-patterns"></a>Inleiding tot de SaaS-schema management patronen

De shard multitenant-databasemodel gebruikt in dit voorbeeld kunt een database tenants bevat een of meer tenants. Dit voorbeeld wordt de mogelijkheid gebruik een combinatie van een veel-tenant- en one databases, inschakelen verkend een *hybride* model voor het beheer van tenant. Het beheren van wijzigingen in deze databases kan ingewikkeld zijn. [Elastische taken](sql-database-elastic-jobs-overview.md) vereenvoudigt administratie en beheer van grote aantallen van de database. Taken kunnen u veilig en betrouwbaar Transact-SQL-scripts uitvoeren als taken voor een groep van tenant-databases. De taken zijn onafhankelijk van de gebruikersinteractie of invoer. Deze methode kan worden gebruikt om wijzigingen aan schema of algemene verwijzen naar gegevens, over alle tenants in een toepassing te implementeren. Elastische taken kunnen ook worden gebruikt voor het onderhouden van een gouden sjabloon kopie van de database. De sjabloon die wordt gebruikt voor het maken van nieuwe tenants, zodat altijd de meest recente schema en referentiegegevens worden gebruikt.

![scherm](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Beperkte preview voor Elastische taken

Er is een nieuwe versie van de elastische taken die is nu een geïntegreerde functie van Azure SQL Database. Van deze nieuwe versie van Elastische taken is momenteel een beperkte preview beschikbaar. De beperkte preview momenteel ondersteunt het gebruik van PowerShell om een agent taak en T-SQL voor het maken en beheren van taken te maken.
> [!NOTE] 
> Deze zelfstudie maakt gebruik van functies van de service SQL-Database die zich in een beperkte preview (elastische Database taken). Als u wenst te doen in deze zelfstudie, geeft u uw abonnement-ID SaaSFeedback@microsoft.com met de onderwerpnaam = elastische taken Preview. Nadat u ontvangt een bevestiging dat uw abonnement is ingeschakeld, downloaden en installeren van de meest recente voorlopige versie taken-cmdlets. Dit voorbeeld is beperkt, dus neem contact op met SaaSFeedback@microsoft.com voor vragen over of ondersteuning.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Ophalen van de broncode van de Database Wingtip Tickets SaaS-multitenant-toepassing en scripts

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) opslagplaats op Github. Zie de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren. 

## <a name="create-a-job-agent-database-and-new-job-agent"></a>Een taak agent database en de nieuwe taak agent maken

Deze zelfstudie vereist dat u PowerShell gebruiken voor het maken van de taak agent database en de agent van de taak. Zoals de MSDB-database die wordt gebruikt door SQL Agent een taak agent een Azure SQL database gebruikt voor het opslaan van taakdefinities, taakstatus en de geschiedenis. Nadat de agent van de taak is gemaakt, kunt u maken en taken onmiddellijk controleren.

1. In **PowerShell ISE**Open *... \\Learning-Modules\\Schemabeheer\\Demo SchemaManagement.ps1*.
2. Druk op **F5** om het script uit te voeren.

De *Demo SchemaManagement.ps1* script aanroepen de *implementeren SchemaManagement.ps1* script voor het maken van een database met naam _jobagent_ op de catalogusserver. Het script maakt vervolgens de agent taak doorgeven van de _jobagent_ database als een parameter.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Een taak maken om nieuwe referentiegegevens te implementeren voor alle tenants

#### <a name="prepare"></a>Voorbereiden

Elke tenant-database bevat een reeks wilt typen in de **VenueTypes** tabel. Elk type wilt definieert het type gebeurtenissen die kunnen worden gehost op een plaats. Dergelijke wetten overeenkomen met de achtergrondafbeeldingen u in de tenant gebeurtenissen app ziet.  In deze oefening maakt u een update implementeert op alle databases op twee extra wilt typen toevoegen: *motor race* en *zwemmen vereniging*. 

Bekijk eerst de wilt typen opgenomen in elke tenant-database. Verbinding maken met een van de tenant-databases in SQL Server Management Studio (SSMS) en de tabel VenueTypes controleren.  U kunt ook een query in deze tabel in de Query-editor in de Azure portal toegankelijk is vanaf de database. 

1. SSMS opent en verbinding maken met de server van de tenant: *tenants1-dpt -&lt;gebruiker&gt;. database.windows.net*
1. Om te controleren of *motor race* en *zwemmen vereniging* **niet** momenteel deel, blader naar de *contosoconcerthall* database op de *tenants1-dpt -&lt;gebruiker&gt;*  server en de query de *VenueTypes* tabel.



#### <a name="steps"></a>Stappen

Nu u een taak voor het bijwerken de **VenueTypes** tabel in de database van elke tenants, door de twee nieuwe wilt typen toe te voegen.

Voor het maken van een nieuwe taak gebruikt u de verzameling taken systeem opgeslagen procedures die zijn gemaakt in de _jobagent_ database. De opgeslagen procedures zijn gemaakt wanneer de agent van de taak is gemaakt.

1. In SSMS, verbinding maken met de server van de tenant: tenants1-mt -&lt;gebruiker&gt;. database.windows.net

2. Blader naar de *tenants1* database.

3. Query de *VenueTypes* tabel om te bevestigen dat *motor race* en *zwemmen vereniging* zijn niet nog in de lijst met resultaten.

4. Verbinding maken met de catalogusserver *catalogus-mt -&lt;gebruiker&gt;. database.windows.net*.

5. Verbinding maken met de _jobagent_ database in de catalogusserver.

6. Open het bestand in SSMS, *... \\Learning-Modules\\Schemabeheer\\DeployReferenceData.sql*.

7. Wijzig de instructie: Stel @User = &lt;gebruiker&gt; en vervang de waarde van de gebruiker gebruikt wanneer u de toepassing Wingtip Tickets SaaS multitenant Database geïmplementeerd.

8. Druk op **F5** om het script uit te voeren.

#### <a name="observe"></a>Observeren

Houd rekening met de volgende items in de *DeployReferenceData.sql* script:

- **SP\_toevoegen\_doel\_groep** maakt de doelgroepnaam *DemoServerGroup*, en doelleden worden toegevoegd aan de groep.

- **SP\_toevoegen\_doel\_groep\_lid** voegt de volgende items:
    - Een *server* lid doeltype.
        - Dit is de *tenants1-mt -&lt;gebruiker&gt;*  server met de databases tenants.
        - Met inbegrip van de server bevat de tenant-databases die aanwezig zijn op het moment dat de taak wordt uitgevoerd.
    - Een *database* lid doeltype voor de sjabloondatabase (*basetenantdb*) die zich bevindt op *catalogus-mt -&lt;gebruiker&gt;*  -server
    - Een *database* lid dat u wilt opnemen als doel de *ad-hoc reporting* database die wordt gebruikt in een latere zelfstudie.

- **SP\_toevoegen\_taak** maakt een taak die is aangeroepen *verwijzing gegevens implementatie*.

- **SP\_toevoegen\_taakstap** wordt gemaakt met de tekst van T-SQL-opdracht voor het bijwerken van de verwijzingstabel, VenueTypes taakstap.

- De resterende weergaven in het script tonen het bestaan van de objecten en controleren de taakuitvoering. Gebruik deze query's om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid. De taak werkt de database tenants en werkt de twee extra databases die de verwijzingstabel bevatten.

In SSMS, blader naar de tenant-database op de *tenants1-mt -&lt;gebruiker&gt;*  server. Query de *VenueTypes* tabel om te bevestigen dat *motor race* en *zwemmen vereniging* zijn nu toegevoegd aan de tabel. Het totale aantal wilt typen moet hebt verhoogd met twee.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Een taak maken voor het beheren van de referentietabelindex

In deze oefening maakt een taak voor het bouwen van de index voor de verwijzing naar primaire sleutel van de tabel voor alle databases in de tenant. Het opnieuw opbouwen van een index is een bewerking voor het beheer van typische database die een beheerder mogelijk worden uitgevoerd na het laden van een grote hoeveelheid gegevens laden om prestaties te verbeteren.

1. In SSMS, verbinding maken met _jobagent_ -database in *catalogus-mt -&lt;gebruiker&gt;. database.windows.net* server.

2. Open in SSMS, *... \\Learning-Modules\\Schemabeheer\\OnlineReindex.sql*.

3. Druk op **F5** om het script uit te voeren.

#### <a name="observe"></a>Observeren

Houd rekening met de volgende items in de *OnlineReindex.sql* script:

* **SP\_toevoegen\_taak** maakt een nieuwe taak aangeroepen *Online indexeren PK\_\_VenueTyp\_\_265E44FD7FD4C885*.

* **SP\_toevoegen\_taakstap** taakstap met T-SQL-opdrachttekst voor het bijwerken van de index maakt.

* De resterende weergaven in de script monitor taak wordt uitgevoerd. Gebruik deze query's om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid op alle leden van de doel-groep.

## <a name="additional-resources"></a>Aanvullende resources

<!-- TODO: Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->
* [Uitgeschaalde clouddatabases beheren](sql-database-elastic-jobs-overview.md)
* [Uitgeschaalde clouddatabases maken en beheren](sql-database-elastic-jobs-create-and-manage.md)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
.
> * Een agent van de taak voor uitvoering van T-SQL-taken voor meerdere databases maken
> * Verwijzing naar gegevens in alle tenant databases bijwerken
> * Een index in een tabel maken in alle tenantdatabases

Probeer vervolgens de [Ad-hoc reporting zelfstudie](saas-multitenantdb-adhoc-reporting.md) om te verkennen gedistribueerde query's uitvoeren voor tenant databases.


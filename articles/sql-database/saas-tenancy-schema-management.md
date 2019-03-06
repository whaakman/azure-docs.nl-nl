---
title: Azure SQL Database-schema in een app met één tenant beheren | Microsoft Docs
description: Het Schema voor meerdere tenants in een app met één tenant dat gebruikmaakt van Azure SQL Database beheren
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
manager: craigg
ms.date: 09/19/2018
ms.openlocfilehash: 39c4884fcca2b041603305d73526e3310ab99a21
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57441879"
---
# <a name="manage-schema-in-a-saas-application-using-the-database-per-tenant-pattern-with-azure-sql-database"></a>Het schema in een SaaS-toepassing met behulp van het patroon van de database-per-tenant met Azure SQL Database beheren
 
Als een databasetoepassing zich verder ontwikkelt, wijzigingen onvermijdelijk moeten worden aangebracht in de database-schema of een verwijzing gegevens.  Database-onderhoudstaken zijn ook periodiek nodig. Beheren van een toepassing die gebruikmaakt van de database per tenant patroon is vereist dat u deze wijzigingen of onderhoudstaken voor een reeks tenantdatabases toepassen.

In deze zelfstudie worden twee scenario's - implementeren voor alle tenants en opnieuw opbouwen van een index van de tabel met de referentiegegevens. De [elastische taken](sql-database-elastic-jobs-overview.md) functie wordt gebruikt voor het uitvoeren van deze acties op alle tenantdatabases en op de sjabloondatabase die wordt gebruikt om nieuwe tenant-databases maken.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * De taakagent van een maken
> * Ervoor zorgen dat de T-SQL-taken kunnen worden uitgevoerd op alle tenantdatabases
> * Naslaginformatie over gegevens in alle tenantdatabases bijwerken
> * Een index in een tabel maken in alle tenantdatabases


Voor het voltooien van deze zelfstudie moet u ervoor zorgen dat aan de volgende vereisten is voldaan:

* De Wingtip Tickets SaaS Database Per Tenant-app is geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en verkennen van de Wingtip Tickets SaaS-database per tenant-toepassing](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* De laatste versie van SQL Server Management Studio (SSMS) moet zijn geïnstalleerd. [SSMS downloaden en installeren](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> In deze zelfstudie maakt gebruik van functies van de SQL-Database-service die zich in een beperkte preview (elastische databasetaken). Als u doen in deze zelfstudie wilt, geeft u uw abonnements-ID naar SaaSFeedback@microsoft.com met onderwerp = Elastic Jobs Preview. Wanneer u de bevestiging hebt ontvangen dat uw abonnement is ingeschakeld, [downloadt en installeert u de taak-cmdlets van de voorlopige versie](https://github.com/jaredmoo/azure-powershell/releases). Deze Preview-versie is beperkt, dus neem contact op met SaaSFeedback@microsoft.com voor vragen of voor ondersteuning.

## <a name="introduction-to-saas-schema-management-patterns"></a>Kennismaking met patronen voor SaaS-schema

De database per tenant patroon effectief Hiermee isoleert u gegevens van de tenant, maar verhoogt het aantal databases beheren en onderhouden. [Elastische taken](sql-database-elastic-jobs-overview.md) vergemakkelijkt en het beheer van SQL-databases. Met elastische taken kunt u veilig en betrouwbaar taken (T-SQL-scripts) uitvoeren op basis van een groep databases. Taken kunnen schema en de wijzigingen in algemene verwijzingsgegevens in alle tenantdatabases in een toepassing te implementeren. Elastische taken kunnen ook worden gebruikt voor het onderhouden van een *sjabloon* database gebruikt voor het maken van nieuwe tenants, ervoor te zorgen dat deze altijd de meest recente gegevens van de schema- en naslaginformatie over heeft.

![scherm](media/saas-tenancy-schema-management/schema-management-dpt.png)


## <a name="elastic-jobs-limited-preview"></a>Beperkte preview voor Elastische taken

Er is een nieuwe versie van elastische taken; dit is nu een geïntegreerde functie van Azure SQL Database. Van deze nieuwe versie van Elastische taken is momenteel een beperkte preview beschikbaar. Deze beperkte preview ondersteunt momenteel met behulp van PowerShell om een agent voor taken en T-SQL voor het maken en beheren van taken te maken.

> [!NOTE]
> In deze zelfstudie maakt gebruik van functies van de SQL-Database-service die zich in een beperkte preview (elastische databasetaken). Als u doen in deze zelfstudie wilt, geeft u uw abonnements-ID naar SaaSFeedback@microsoft.com met onderwerp = Elastic Jobs Preview. Wanneer u de bevestiging hebt ontvangen dat uw abonnement is ingeschakeld, [downloadt en installeert u de taak-cmdlets van de voorlopige versie](https://github.com/jaredmoo/azure-powershell/releases). Deze Preview-versie is beperkt, dus neem contact op met SaaSFeedback@microsoft.com voor vragen of voor ondersteuning.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De Wingtip Tickets SaaS-database per tenant toepassingsscripts ophalen

De toepassing source code en scripts zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets SaaS-scripts.

## <a name="create-a-job-agent-database-and-new-job-agent"></a>Een taakagent-database en -agent voor nieuwe taken maken

Deze zelfstudie moet dat u PowerShell gebruiken voor het maken van een taakagent en de bijbehorende back-ups taak agent-database. De database van de agent taak bevat taakdefinities, de taakstatus en de geschiedenis. Nadat de taakagent en de bijbehorende database hebt gemaakt, kunt u maken en bewaken van taken onmiddellijk.

1. **In PowerShell ISE**, openen... \\Learning Modules\\Schemabeheer\\*Demo-SchemaManagement.ps1*.
1. Druk op **F5** om het script uit te voeren.

De *Demo-SchemaManagement.ps1* script aanroepen de *Deploy-SchemaManagement.ps1* script voor het maken van een SQL-database met de naam *osagent* op de catalogusserver. Vervolgens wordt de taakagent met behulp van de database als een parameter gemaakt.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Een taak maken om nieuwe referentiegegevens te implementeren voor alle tenants

In de app Wingtip Tickets bevat elke tenantdatabase een reeks locatietypen ondersteund. Elke venue is van een specifieke locatietype, waarmee wordt gedefinieerd welke soorten evenementen die kan worden gehost, en bepaalt van de achtergrondafbeelding gebruikt in de app. Voor de toepassing voor de ondersteuning van nieuwe soorten gebeurtenissen, moet deze verwijzingsgegevens bijgewerkt en nieuwe locatietypen toegevoegd.  In deze oefening maakt implementeert u een update op alle tenantdatabases twee nieuwe locatietypen toevoegen: *Motorcycle Racing* en *Swimming Club*.

Bekijk eerst de typen venues is opgenomen in elke tenantdatabase. Verbinding maken met een van de tenant-databases in SQL Server Management Studio (SSMS) en controleren van de tabel venuetypes bij.  U kunt ook een query in deze tabel in de Query-editor in Azure portal, toegankelijk via de pagina van de database. 

1. Open SSMS en maak verbinding met de tenantserver: *tenants1-dpt -&lt;gebruiker&gt;. database.windows.net*
1. Om te bevestigen dat *Motorcycle Racing* en *Swimming Club* **niet** momenteel is opgenomen, blader naar de _contosoconcerthall_ database op de *tenants1-dpt -&lt;gebruiker&gt;*  -server en de query de *VenueTypes* tabel.

Nu maken we een taak voor het bijwerken van de *VenueTypes* tabel in de tenant-databases naar de nieuwe locatietypen toevoegen.

Als u wilt een nieuw project maakt, gebruikt u een reeks taken systeem opgeslagen procedures gemaakt in de _jobagent_ database waarop de taakagent is gemaakt.

1. In SSMS verbinding maken met de catalog-server: *catalogus-dpt -&lt;gebruiker&gt;. database.windows.net* server 
1. Open het bestand in SSMS... \\Learning Modules\\Schemabeheer\\DeployReferenceData.sql
1. Wijzig de instructie: Stel @wtpUser = &lt;gebruiker&gt; en vervang de waarde van de gebruiker die wordt gebruikt tijdens de implementatie van de app Wingtip Tickets SaaS Database Per Tenant
1. Zorg ervoor dat u bent verbonden met de _jobagent_ database en druk op **F5** het script uitvoeren

Bekijk de volgende elementen in de *DeployReferenceData.sql* script:
* **SP\_toevoegen\_doel\_groep** maakt de doelgroepnaam DemoServerGroup.
* **SP\_toevoegen\_doel\_groep\_lid** wordt gebruikt om de set doeldatabases te definiëren.  Eerste de _tenants1-dpt -&lt;gebruiker&gt;_  -server is toegevoegd.  De server toe te voegen als een doel zorgt ervoor dat de databases op die server op het moment van taakuitvoering moet worden opgenomen in de taak. Vervolgens wordt de _basetenantdb_ database en de *ad-hoc reporting* database (die wordt gebruikt in een latere zelfstudie) worden toegevoegd als de doelen.
* **SP\_toevoegen\_taak** maakt een taak met de naam _implementatie van referentiegegevens_.
* **SP\_toevoegen\_jobstep** maakt de taakstap met T-SQL-opdracht voor het bijwerken van de referentietabel venuetypes bij.
* De resterende weergaven in het script tonen het bestaan van de objecten en controleren de taakuitvoering. Deze query's gebruiken om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid op de doeldatabase.

Als het script is voltooid, kunt u controleren of dat de referentiegegevens is bijgewerkt.  In SSMS, bladert u naar de *contosoconcerthall* database op de *tenants1-dpt -&lt;gebruiker&gt;*  -server en de query de *VenueTypes* tabel.  Controleer of *Motorcycle Racing* en *Swimming Club* **zijn** nu aanwezig zijn.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Een taak maken voor het beheren van de referentietabelindex

In deze oefening gebruikt een taak opnieuw opbouwen van de index op een primaire sleutel van de referentie-tabel.  Dit is een bewerking voor het onderhoud van typische die kan worden uitgevoerd na het laden van grote hoeveelheden gegevens.

Maak een taak met dezelfde in het systeem opgeslagen procedures.

1. Open SSMS en maak verbinding met de _catalogus-dpt -&lt;gebruiker&gt;. database.windows.net_ server
1. Open het bestand _... \\Learning Modules\\Schemabeheer\\OnlineReindex.sql_
1. Klik met de rechtermuisknop, selecteer verbinding en verbinding maken met de _catalogus-dpt -&lt;gebruiker&gt;. database.windows.net_ server, als deze nog niet is verbonden
1. Zorg ervoor dat u bent verbonden met de _jobagent_ database en druk op **F5** het script uitvoeren

Bekijk de volgende elementen in de _OnlineReindex.sql_ script:
* **SP\_toevoegen\_taak** maakt een nieuwe taak met de naam ' Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885 "
* **SP\_toevoegen\_jobstep** maakt de taakstap met T-SQL-opdracht voor het bijwerken van de index
* De resterende weergaven in het script controleren de taakuitvoering. Deze query's gebruiken om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid op alle leden van de doel-groep.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Een agent voor taken om uit te voeren via T-SQL-taken met meerdere databases maken
> * Naslaginformatie over gegevens in alle tenantdatabases bijwerken
> * Een index in een tabel maken in alle tenantdatabases

Probeer vervolgens de [Ad-hocrapportage zelfstudie](saas-tenancy-cross-tenant-reporting.md) verkennen gedistribueerde query's uitvoeren voor de tenant van databases.


## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies voort op de implementatie van de toepassing Wingtip Tickets SaaS Database Per Tenant bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Uitgeschaalde clouddatabases beheren](sql-database-elastic-jobs-overview.md)
* [Uitgeschaalde clouddatabases maken en beheren](sql-database-elastic-jobs-create-and-manage.md)
---
title: Een Azure SQL Database-schema beheren in een app met meerdere tenants | Microsoft Docs
description: Het schema voor meerdere tenants beheren in een toepassing met meerdere tenants die gebruikmaakt van Azure SQL Database
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 07/28/2017
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: a473e87934ba573cc22b2c248ea0398bc5a4c29b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="manage-schema-in-a-saas-application-using-the-database-per-tenant-pattern-with-azure-sql-database"></a>Schema in een SaaS-toepassing met behulp van het patroon database per tenant met Azure SQL Database beheren

Zoals een databasetoepassing zich verder ontwikkelen, moeten wijzigingen onvermijdelijk worden gemaakt met de databasegegevens schema of verwijzing.  Database-onderhoudstaken zijn ook periodiek nodig. Het beheren van een toepassing die gebruikmaakt van de database per tenant patroon is vereist dat u deze wijzigingen of onderhoudstaken voor een wagenpark van tenant-databases toepassen.

Deze zelfstudie behandelt de twee scenario's - verwijzing Gegevensupdates implementeren voor alle tenants en opnieuw opbouwen van een index in de tabel met de referentiegegevens. De [elastische taken](sql-database-elastic-jobs-overview.md) functie wordt gebruikt voor het uitvoeren van deze acties voor alle databases van de tenant, en op de sjabloondatabase gebruikt voor het maken van nieuwe tenant databases.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * De agent van een taak maken
> * Ervoor zorgen dat de T-SQL-taken worden uitgevoerd op alle databases voor tenant
> * Verwijzing naar gegevens in alle tenant databases bijwerken
> * Een index in een tabel maken in alle tenantdatabases


Voor het voltooien van deze zelfstudie moet u ervoor zorgen dat aan de volgende vereisten is voldaan:

* De app Wingtip Tickets SaaS Database Per Tenant wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de database Wingtip Tickets SaaS per tenant toepassing verkennen](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* De laatste versie van SQL Server Management Studio (SSMS) moet zijn geïnstalleerd. [SSMS downloaden en installeren](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> Deze zelfstudie maakt gebruik van functies van de service SQL-Database die zich in een beperkte preview (elastische Database taken). Als u wenst te doen in deze zelfstudie, geeft u uw abonnement-ID SaaSFeedback@microsoft.com met de onderwerpnaam = elastische taken Preview. Wanneer u de bevestiging hebt ontvangen dat uw abonnement is ingeschakeld, [downloadt en installeert u de taak-cmdlets van de voorlopige versie](https://github.com/jaredmoo/azure-powershell/releases). Dit voorbeeld is beperkt, dus neem contact op met SaaSFeedback@microsoft.com voor vragen over of ondersteuning.

## <a name="introduction-to-saas-schema-management-patterns"></a>Inleiding tot de SaaS-schema management patronen

De database per tenant patroon tenant gegevens effectief isoleert, maar verhoogt het aantal databases te beheren en te onderhouden. [Elastische taken](sql-database-elastic-jobs-overview.md) vereenvoudigt administratie en beheer van SQL-databases. Taken kunnen u veilig en betrouwbaar, taken (T-SQL-scripts) uitvoeren op basis van een groep met databases. Taken kunnen schema en wijzigingen in algemene verwijzingen voor alle tenant-databases in een toepassing implementeren. Elastische taken kunnen ook worden gebruikt voor het onderhouden van een *sjabloon* database die wordt gebruikt voor het maken van nieuwe tenants, zodat deze altijd de meest recente gegevens van schema's en verwijzingen is.

![scherm](media/saas-tenancy-schema-management/schema-management-dpt.png)


## <a name="elastic-jobs-limited-preview"></a>Beperkte preview voor Elastische taken

Er is een nieuwe versie van de elastische taken die is nu een geïntegreerde functie van Azure SQL Database. Van deze nieuwe versie van Elastische taken is momenteel een beperkte preview beschikbaar. Deze beperkte preview ondersteunt momenteel met behulp van PowerShell om een agent taak en T-SQL voor het maken en beheren van taken te maken.

> [!NOTE]
> Deze zelfstudie maakt gebruik van functies van de service SQL-Database die zich in een beperkte preview (elastische Database taken). Als u wenst te doen in deze zelfstudie, geeft u uw abonnement-ID SaaSFeedback@microsoft.com met de onderwerpnaam = elastische taken Preview. Wanneer u de bevestiging hebt ontvangen dat uw abonnement is ingeschakeld, [downloadt en installeert u de taak-cmdlets van de voorlopige versie](https://github.com/jaredmoo/azure-powershell/releases). Dit voorbeeld is beperkt, dus neem contact op met SaaSFeedback@microsoft.com voor vragen over of ondersteuning.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De database Wingtip Tickets SaaS per tenant toepassingsscripts ophalen

De bron en het beheer toepassingsscripts zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.

## <a name="create-a-job-agent-database-and-new-job-agent"></a>Een taak agent database en de nieuwe taak agent maken

Deze zelfstudie moet dat u PowerShell gebruiken voor het maken van een taak agent en de bijbehorende back-ups maken taak agent-database. De database van de agent taak bevat taakdefinities, taakstatus en de geschiedenis. Zodra de taak-agent en de bijbehorende database zijn gemaakt, kunt u maken en taken onmiddellijk controleren.

1. **In PowerShell ISE**open... \\Learning-Modules\\Schemabeheer\\*Demo SchemaManagement.ps1*.
1. Druk op **F5** om het script uit te voeren.

De *Demo SchemaManagement.ps1* script aanroepen de *implementeren SchemaManagement.ps1* script voor het maken van een SQL-database met de naam *osagent* op de catalogusserver. Vervolgens wordt de taak-agent, met behulp van de database als een parameter gemaakt.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Een taak maken om nieuwe referentiegegevens te implementeren voor alle tenants

In de app Wingtip Tickets bevat elke tenant-database een set ondersteunde wilt typen. Elke locatie vast is van een type specifieke wetten, waarin de aard van de gebeurtenissen die kunnen worden gehost, en bepaalt de achtergrondafbeelding die wordt gebruikt in de app. Voor de toepassing voor de ondersteuning van nieuwe soorten gebeurtenissen, moet deze referentiegegevens nieuwe en bijgewerkte wilt typen die zijn toegevoegd.  In deze oefening implementeert u een update voor alle tenantdatabases waarbij twee nieuwe locatietypen worden toegevoegd: *Motorcycle Racing* en *Swimming Club*.

Bekijk eerst de wilt typen opgenomen in elke tenant-database. Verbinding maken met een van de tenant-databases in SQL Server Management Studio (SSMS) en de tabel VenueTypes controleren.  U kunt ook een query in deze tabel in de Query-editor in de Azure portal toegankelijk is vanaf de database. 

1. SSMS opent en verbinding maken met de server van de tenant: *tenants1-dpt -&lt;gebruiker&gt;. database.windows.net*
1. Om te controleren of *motor race* en *zwemmen vereniging* **niet** momenteel deel, blader naar de _contosoconcerthall_ database op de *tenants1-dpt -&lt;gebruiker&gt;*  server en de query de *VenueTypes* tabel.

Nu gaan we maken een taak voor het bijwerken de *VenueTypes* tabel in de tenant-databases naar de nieuwe typen van u wilt toevoegen.

Voor het maken van een nieuwe taak die u gebruikt een set taken systeem opgeslagen procedures die zijn gemaakt in de _jobagent_ gegevensbank wanneer de agent van de taak is gemaakt.

1. In SSMS, verbinding maken met de catalogusserver: *catalogus-dpt -&lt;gebruiker&gt;. database.windows.net* server 
1. Open het bestand in SSMS... \\Learning-Modules\\Schemabeheer\\DeployReferenceData.sql
1. Wijzig de instructie: Stel @wtpUser = &lt;gebruiker&gt; en vervang de waarde van de gebruiker gebruikt tijdens de implementatie van de app Wingtip Tickets SaaS Database Per Tenant
1. Zorg ervoor dat u verbonden bent met de _jobagent_ database en druk op **F5** het script uitvoeren

Houd rekening met de volgende elementen in de *DeployReferenceData.sql* script:
* **SP\_toevoegen\_doel\_groep** de doelgroepnaam DemoServerGroup maakt.
* **SP\_toevoegen\_doel\_groep\_lid** wordt gebruikt voor het definiëren van de set met de doeldatabases.  Eerste de _tenants1-dpt -&lt;gebruiker&gt;_  server wordt toegevoegd.  De server toe te voegen als een doel zorgt ervoor dat de databases op die server op het moment van het uitvoeren van de taak moet worden opgenomen in de taak. Vervolgens wordt de _basetenantdb_ database en de *ad-hoc reporting* database (gebruikt in een latere zelfstudie) worden toegevoegd als doelen.
* **SP\_toevoegen\_taak** maakt een taak met de naam _verwijzing gegevens implementatie_.
* **SP\_toevoegen\_taakstap** wordt gemaakt met de tekst van T-SQL-opdracht voor het bijwerken van de verwijzingstabel, VenueTypes taakstap.
* De resterende weergaven in het script tonen het bestaan van de objecten en controleren de taakuitvoering. Gebruik deze query's om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid op de doeldatabases.

Nadat het script is voltooid, kunt u controleren dat de referentiegegevens is bijgewerkt.  Blader in SSMS, naar de *contosoconcerthall* database op de *tenants1-dpt -&lt;gebruiker&gt;*  server en de query de *VenueTypes* tabel.  Controleer of *motor race* en *zwemmen vereniging* **zijn** nu aanwezig.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Een taak maken voor het beheren van de referentietabelindex

In deze oefening gebruikt een taak voor het opnieuw samenstellen van de index op de primaire sleutel van de verwijzing naar tabel.  Dit is een bewerking voor het onderhoud van typische database dat kan worden uitgevoerd na het laden van grote hoeveelheden gegevens.

Maak een taak met dezelfde in het systeem opgeslagen procedures.

1. SSMS opent en verbinding maken met de _catalogus-dpt -&lt;gebruiker&gt;. database.windows.net_ server
1. Open het bestand _... \\Learning-Modules\\Schemabeheer\\OnlineReindex.sql_
1. Klik met de rechtermuisknop, selecteer verbinding en verbinding maken met de _catalogus-dpt -&lt;gebruiker&gt;. database.windows.net_ server moeten, als u nog niet verbonden
1. Zorg ervoor dat u verbonden bent met de _jobagent_ database en druk op **F5** het script uitvoeren

Houd rekening met de volgende elementen in de _OnlineReindex.sql_ script:
* **SP\_toevoegen\_taak** maakt een nieuwe taak genaamd ' Online opnieuw indexeren PK\_\_VenueTyp\_\_265E44FD7FD4C885 '
* **SP\_toevoegen\_taakstap** taakstap met T-SQL-opdrachttekst voor het bijwerken van de index wordt gemaakt
* De resterende weergaven in de script monitor taak wordt uitgevoerd. Gebruik deze query's om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid op alle leden van de doel-groep.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * De agent van een taak om te worden uitgevoerd via een T-SQL-taken van meerdere databases maken
> * Verwijzing naar gegevens in alle tenant databases bijwerken
> * Een index in een tabel maken in alle tenantdatabases

Probeer vervolgens de [Ad-hoc reporting zelfstudie](saas-tenancy-cross-tenant-reporting.md) om te verkennen gedistribueerde query's uitvoeren voor tenant databases.


## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies waarin voort op de toepassingsimplementatie Wingtip Tickets SaaS Database Per Tenant bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Uitgeschaalde clouddatabases beheren](sql-database-elastic-jobs-overview.md)
* [Uitgeschaalde clouddatabases maken en beheren](sql-database-elastic-jobs-create-and-manage.md)
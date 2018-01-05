---
title: Een Azure SQL Database-schema beheren in een app met meerdere tenants | Microsoft Docs
description: Het schema voor meerdere tenants beheren in een toepassing met meerdere tenants die gebruikmaakt van Azure SQL Database
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.reviewers: billgib
ms.author: genemi
ms.openlocfilehash: 135764a7d89dcf711ff7fe9416850f1af9329479
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Schema voor meerdere tenants in een multitenant-toepassing die gebruikmaakt van Azure SQL Database beheren

Deze zelfstudie onderzoekt de uitdagingen bij het beheren van een wagenpark potentieel grote databases in een Software als een dienst (SaaS)-toepassing in de cloud. Oplossingen worden voor het beheren van de schema-uitbreidingen die zijn ontwikkeld en geïmplementeerd tijdens de levensduur van een app uitgelegd.

Als een toepassing zich verder ontwikkelen, wijzigingen optreden van de tabelkolommen of andere schema of de referentiegegevens of voor de prestaties van verwante objecten. Deze wijzigingen moeten op gecoördineerde wijze worden geïmplementeerd in talloze bestaande tenant databases met een SaaS-app. En deze wijzigingen moeten worden opgenomen in toekomstige tenant-databases die worden toegevoegd aan de app. Daarom moeten de wijzigingen ook worden opgenomen in het proces dat voorziet in nieuwe databases.

#### <a name="two-scenarios"></a>Twee scenario 's

Deze zelfstudie behandelt de volgende twee scenario's:
- Verwijzing naar Gegevensupdates implementeren voor alle tenants.
- Een index in de tabel met de referentiegegevens retuning.

De [elastische taken](sql-database-elastic-jobs-overview.md) functie van Azure SQL Database wordt gebruikt om uit te voeren van deze bewerkingen voor tenant-databases. De database van de tenant gouden sjabloon ook de taken uitgevoerd. Deze sjabloon wordt gebruikt wanneer nieuwe databases zijn ingericht.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De account van een taak maken.
> * Een query over meerdere tenants.
> * Gegevens bijwerken in alle databases van de tenant.
> * Een index maken voor een tabel in alle databases van de tenant.

## <a name="prerequisites"></a>Vereisten

- De app Wingtip Tickets moet al geïmplementeerd:
    - Voor instructies raadpleegt u de eerste zelfstudie, waardoor de *Wingtip Tickets* SaaS multitenant database app:<br />[Implementeren en een shard multitenant-toepassing die gebruikmaakt van Azure SQL Database verkennen](saas-multitenantdb-get-started-deploy.md).
        - Het proces implementeren wordt uitgevoerd voor minder dan vijf minuten.
    - U moet hebben de *shard multitenant* versie van Wingtip geïnstalleerd. De versies voor *zelfstandige* en *Database per tenant* bieden geen ondersteuning voor de zelfstudie aanwezig.

- De meest recente versie van SQL Server Management Studio (SSMS) moet worden geïnstalleerd. [Download en installeer SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- Azure PowerShell moet zijn geïnstalleerd. Zie voor meer informatie [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> Deze zelfstudie maakt gebruik van functies van de Azure SQL Database-service die zich in een beperkte preview ([elastische Database taken](sql-database-elastic-database-client-library.md)). Als u wenst te doen in deze zelfstudie, geeft u uw abonnement-ID  *SaaSFeedback@microsoft.com*  met de onderwerpnaam = elastische taken Preview. Wanneer u de bevestiging hebt ontvangen dat uw abonnement is ingeschakeld, [downloadt en installeert u de taak-cmdlets van de voorlopige versie](https://github.com/jaredmoo/azure-powershell/releases). Dit voorbeeld is beperkt, dus neem contact op met  *SaaSFeedback@microsoft.com*  voor vragen over of ondersteuning.

## <a name="introduction-to-saas-schema-management-patterns"></a>Inleiding tot de SaaS-schema management patronen

De shard multitenant-databasemodel gebruikt in dit voorbeeld kunt een database tenants bevat een of meer tenants. Dit voorbeeld wordt de mogelijkheid gebruik een combinatie van een veel-tenant- en one databases, inschakelen verkend een *hybride* model voor het beheer van tenant. Het beheren van deze databases is ingewikkeld. [Elastische taken](sql-database-elastic-jobs-overview.md) vereenvoudigt het onderhoud en het beheer van de SQL-gegevenslaag. Taken kunnen u veilig en betrouwbaar Transact-SQL-scripts uitvoeren als taken voor een groep van tenant-databases. De taken zijn onafhankelijk van de gebruikersinteractie of invoer. Deze methode kan worden gebruikt om wijzigingen aan schema of algemene verwijzen naar gegevens, over alle tenants in een toepassing te implementeren. Elastische taken kunnen ook worden gebruikt voor het onderhouden van een gouden sjabloon kopie van de database. De sjabloon die wordt gebruikt voor het maken van nieuwe tenants, zodat altijd de meest recente schema en referentiegegevens worden gebruikt.

![scherm](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Beperkte preview voor Elastische taken

Er is een nieuwe versie van de elastische taken die is nu een geïntegreerde functie van Azure SQL Database. Door geïntegreerde bedoelen we dat dit is geen extra services of onderdelen vereist. Van deze nieuwe versie van Elastische taken is momenteel een beperkte preview beschikbaar. De beperkte preview ondersteunt momenteel PowerShell taakaccounts te maken en T-SQL te maken en beheren van taken.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Ophalen van de broncode van de Database Wingtip Tickets SaaS-multitenant-toepassing en scripts

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) opslagplaats op Github. Zie de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren. 

## <a name="create-a-job-account-database-and-new-job-account"></a>Een taakaccountdatabase en een nieuw taakaccount maken

Deze zelfstudie vereist dat u PowerShell gebruiken voor het maken van de taak accountdatabase en de account van de taak. Als de MSDB-database die wordt gebruikt door SQL Agent elastische taken een Azure SQL database gebruikt voor het opslaan van taakdefinities, taakstatus en de geschiedenis. Nadat het account van de taak is gemaakt, kunt u maken en taken onmiddellijk controleren.

1. In **PowerShell ISE**Open *... \\Learning-Modules\\Schemabeheer\\Demo SchemaManagement.ps1*.
2. Druk op **F5** om het script uit te voeren.

De *Demo SchemaManagement.ps1* script aanroepen de *implementeren SchemaManagement.ps1* script voor het maken van een laag *S2* database met de naam **jobaccount** op de catalogusserver. Het script maakt u de taak-account, de database jobaccount doorgegeven als parameter voor de taak-account maken-aanroep.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Een taak maken om nieuwe referentiegegevens te implementeren voor alle tenants

#### <a name="prepare"></a>Voorbereiden

Elke database tenants bevat een reeks wilt typen in de **VenueTypes** tabel. De typen u wilt bepalen het soort gebeurtenissen die worden gehost op een plaats. In deze oefening maakt u een update implementeert op alle databases op twee extra wilt typen toevoegen: *motor race* en *zwemmen vereniging*. Deze locatietypen sluiten aan op de achtergrondafbeelding die u in de app voor tenantevenementen ziet.

Voordat u de nieuwe referentiegegevens implementeert, noteer het aantal wilt typen die al bestaan, die mogelijk 10. Opmerking nemen door te klikken op de volgende koppeling om de webgebruikersinterface Wingtip en vervolgens te klikken op de **wetten Type** vervolgkeuzelijst:
- http://events.Wingtip-MT.<USER>. trafficmanager.net

Nu kunt u het aantal oorspronkelijke wilt typen tellen. In het bijzonder, houd er rekening mee dat geen van beide *motor race* noch *zwemmen vereniging* nog bestaan.

#### <a name="steps"></a>Stappen

Nu u een taak voor het bijwerken de **VenueTypes** tabel in de database van elke tenants, door de twee nieuwe wilt typen toe te voegen.

Voor het maken van een nieuwe taak gebruikt u de verzameling taken systeem opgeslagen procedures die zijn gemaakt in de *jobaccount* database. De procedures zijn gemaakt wanneer het account van de taak is gemaakt.

1. In SSMS, verbinding maken met de server van de tenant: tenants1-mt -\<gebruiker\>. database.windows.net

2. Blader naar de *tenants1* database op de *tenants1-mt -\<gebruiker\>. database.windows.net* server.

3. Query de *VenueTypes* tabel om te bevestigen dat *motor race* en *zwemmen vereniging* zijn niet nog in de lijst met resultaten.

4. Verbinding maken met de catalogusserver *catalogus-mt -\<gebruiker\>. database.windows.net*.

5. Verbinding maken met de *jobaccount* database in de catalogusserver.

6. Open het bestand in SSMS, *... \\Learning-Modules\\Schemabeheer\\DeployReferenceData.sql*.

7. Wijzig de instructie: Stel @User = &lt;gebruiker&gt; en vervang de waarde van de gebruiker gebruikt wanneer u de toepassing Wingtip Tickets SaaS multitenant Database geïmplementeerd.

8. Druk op **F5** om het script uit te voeren.

#### <a name="observe"></a>Observeren

Houd rekening met de volgende items in de *DeployReferenceData.sql* script:

- **SP\_toevoegen\_doel\_groep** maakt de doelgroepnaam *DemoServerGroup*, en doelleden worden toegevoegd aan de groep.

- **SP\_toevoegen\_doel\_groep\_lid** voegt de volgende items:
    - Een *server* lid doeltype.
        - Dit is de *tenants1-mt -&lt;gebruiker&gt;*  server met de databases tenants.
        - Alle databases in de server zijn dus opgenomen in de taak wanneer de taak wordt uitgevoerd.
    - Een *database* lid doeltype voor de database gouden (*basetenantdb*) die zich bevindt op *catalogus-mt -&lt;gebruiker&gt;*  -server
    - Een *database* lid dat u wilt opnemen als doel de *ad-hoc reporting* database die wordt gebruikt in een latere zelfstudie.

- **SP\_toevoegen\_taak** maakt een taak die is aangeroepen *verwijzing gegevens implementatie*.

- **SP\_toevoegen\_taakstap** wordt gemaakt met de tekst van T-SQL-opdracht voor het bijwerken van de verwijzingstabel, VenueTypes taakstap.

- De resterende weergaven in het script tonen het bestaan van de objecten en controleren de taakuitvoering. Gebruik deze query's om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid. De taak werkt de database tenants en werkt de twee extra databases die de verwijzingstabel bevatten.

In SSMS, blader naar de tenant-database op de *tenants1-mt -&lt;gebruiker&gt;*  server. Query de *VenueTypes* tabel om te bevestigen dat *motor race* en *zwemmen vereniging* zijn nu toegevoegd aan de tabel. Het totale aantal wilt typen moet hebt verhoogd met twee.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Een taak maken voor het beheren van de referentietabelindex

In deze oefening is vergelijkbaar met de vorige oefening. In deze oefening maakt een taak voor het bouwen van de index op de primaire sleutel van de verwijzing naar tabel. Het opnieuw opbouwen van een index is een bewerking voor het beheer van typische database die een beheerder mogelijk na het laden van een grote hoeveelheden gegevens worden uitgevoerd in een tabel, om prestaties te verbeteren.

1. In SSMS, verbinding maken met *jobaccount* -database in *catalogus-mt -&lt;gebruiker&gt;. database.windows.net* server.

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
> - Een account taak query uitvoeren op meerdere tenants maken.
> - Gegevens bijwerken in alle databases van de tenant.
> - Een index maken voor een tabel in alle databases van de tenant.

Probeer vervolgens de [Ad-hoc reporting zelfstudie](saas-multitenantdb-adhoc-reporting.md).


---
title: Azure SQL-gegevens synchroniseren Best Practices | Microsoft Docs
description: Informatie over aanbevolen procedures voor het configureren en synchroniseren van Azure SQL-gegevens uitgevoerd
services: sql-database
ms.date: 11/2/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7492fffd1c18a149ef12174c79d64b47afbaa3e4
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2017
---
# <a name="best-practices-for-azure-sql-data-sync-preview"></a>Aanbevolen procedures voor het synchroniseren van Azure SQL gegevens (Preview) 

In dit artikel beschrijft aanbevolen procedures voor het synchroniseren van de SQL-gegevens (Preview).

Zie voor een overzicht van de SQL-gegevenssynchronisatie [synchroniseren van gegevens via meerdere cloud en on-premises databases met Azure SQL-gegevenssynchronisatie (Preview)](sql-database-sync-data.md).

## <a name="security-and-reliability"></a>Beveiliging en betrouwbaarheid

### <a name="client-agent"></a>Clientagent

-   Installeer de clientagent met de minste bevoegdheden-account met service toegang tot het netwerk.

-   Het is raadzaam als de clientagent is geïnstalleerd op een afzonderlijke computer van de lokale SQL Server-computer.

-   Een on-premises database niet registreren met meer dan één agent.

-   Zelfs als er verschillende tabellen voor verschillende synchronisatiegroepen synchroniseren.

-   Een on-premises database meerdere client agents vormt uitdagingen met zich mee wordt geregistreerd bij het verwijderen van een van de synchronisatiegroepen.

### <a name="database-accounts-with-least-required-privilege"></a>Database-accounts met minimale vereiste bevoegdheden

-   **Voor synchronisatie Setup**. Maken/Alter Table, Alter Database, het maken van de Procedure selecteren / Schema wijzigen, maakt u de gebruiker gedefinieerd Type.

-   **Voor lopende synchronisatie**. Selecteer / Invoegen / bijwerken / verwijderen op tabellen die zijn geselecteerd voor synchronisatie en metagegevens synchroniseren en bij te houden van tabellen, machtiging niet uitvoeren op opgeslagen procedures die zijn gemaakt door de service, de machtiging uitvoeren op de gebruiker gedefinieerde tabeltypen.

-   **Voor de inrichting ongedaan**. ALTER op tabellen onderdeel van synchronisatie, selecteer / verwijderen op metagegevens synchroniseren tabellen, besturingselement op synchronisatie Sync bijhouden tabellen, opgeslagen procedures en gebruiker gedefinieerde typen bijhouden.

**Hoe kunt u deze informatie gebruiken wanneer er slechts één referenties voor een database in de groep voor synchronisatie?**

-   De referenties voor verschillende fasen wijzigen (bijvoorbeeld credential1 voor installatie en credential2 voor lopende).

-   De machtiging van de referenties wijzigen (dat wil zeggen, de machtiging wijzigen nadat synchronisatie is ingesteld).

## <a name="locate-hub"></a>Waar u de Database-Hub

### <a name="enterprise-to-cloud-scenario"></a>Enterprise-naar-cloud-scenario

Om latentie te beperken, de database van de hub dicht bij de grootste punt waar de synchronisatie-groep database verkeer te bewaren.

### <a name="cloud-to-cloud-scenario"></a>Cloud-naar-cloud-scenario

-   Als alle databases in een groep voor synchronisatie in een datacenter, moet de hub zich bevinden in hetzelfde Datacenter. Deze configuratie vermindert de latentie en de kosten van gegevensoverdracht tussen datacenters.

-   Als de databases in een groep voor synchronisatie in meerdere datacenters, moet de hub zich bevinden in hetzelfde Datacenter als de meeste van de databases en databaseverkeer.

### <a name="mixed-scenarios"></a>Gemengde scenario 's

De voorgaande richtlijnen gelden voor complexere configuraties van synchronisatie.

## <a name="database-considerations-and-constraints"></a>Database-overwegingen en beperkingen

### <a name="sql-database-instance-size"></a>De grootte van de SQL-Database-exemplaar

Wanneer u een nieuw exemplaar van SQL-Database maakt, de maximale grootte zo instellen dat het altijd groter is dan de database die u implementeert. Als u niet de maximale grootte groter is dan de geïmplementeerde database stelt, mislukt de synchronisatie. Hoewel er geen automatische groei - kunt u doen op een ALTER DATABASE voor een verhoging van de grootte van de database nadat deze is gemaakt. Ervoor zorgen dat u binnen de maximale grootte van SQL Database-exemplaar.

> [!IMPORTANT]
> Synchroniseren van de SQL-gegevens worden extra metagegevens met elke database opgeslagen. Zorg ervoor dat bij het berekenen van de benodigde ruimte voor deze metagegevens-account. De hoeveelheid toegevoegd overhead valt onder de breedte van de tabellen (bijvoorbeeld smalle tabellen meer overhead vereist) en de hoeveelheid verkeer.

## <a name="table-considerations-and-constraints"></a>Tabel overwegingen en beperkingen

### <a name="selecting-tables"></a>Tabellen selecteren

Niet alle tabellen in een database moeten zich in een [groep voor synchronisatie](#sync-group). De selectie van die tabellen wilt opnemen in een groep voor synchronisatie en waarmee u wilt uitsluiten (of opnemen in een andere groep voor synchronisatie) kan invloed hebben op efficiëntie en kosten. Alleen de tabellen in een groep voor synchronisatie dat de zakelijke behoeften tabellen waarvan ze afhankelijk zijn en vraag opnemen.

### <a name="primary-keys"></a>Primaire sleutels

Elke tabel in een groep voor synchronisatie moet een primaire sleutel hebben. De service SQL synchroniseren van gegevens (Preview) is niet een tabel die geen primaire sleutel synchroniseren.

Test de prestaties van de initiële en lopende synchronisatie voordat de rollen in productie.

## <a name="provisioning-destination-databases"></a>Bestemming databases inrichten

Synchroniseren van de SQL-gegevens (Preview) Preview biedt basisfuncties voor database automatische inrichting.

Deze sectie worden beschreven de beperkingen van het synchroniseren van de SQL-gegevens (Preview) de inrichting.

### <a name="auto-provisioning-limitations"></a>Automatische inrichting beperkingen

De volgende zijn beperkingen van het synchroniseren van de SQL-gegevens (Preview) automatisch inrichten.

-   De geselecteerde kolommen worden gemaakt in de doeltabel.
Dus als sommige kolommen geen deel uit van de groep voor synchronisatie maken wordt deze kolommen zijn niet in de doeltabellen ingericht.

-   Indexen worden alleen gemaakt voor de geselecteerde kolommen.
Als de tabel bronindex heeft kolommen die geen deel uitmaken van de groep voor synchronisatie worden de indexen in de doeltabellen niet ingericht.

-   Indexen voor kolommen van het type XML zijn niet ingericht.

-   CHECK-beperkingen niet ingericht.

-   Bestaande triggers op de brontabellen zijn niet ingericht.

-   Weergaven en opgeslagen Procedures worden niet gemaakt op de doeldatabase.

### <a name="recommendations"></a>Aanbevelingen

-   Gebruik de automatische inrichting mogelijkheid alleen voor probeert de service.

-   Voor productie, moet u het databaseschema inrichten.

## <a name="avoid-a-slow-and-costly-initial-synchronization"></a>Een initiële synchronisatie langzaam en kostbaar voorkomen

Deze sectie wordt beschreven voor de initiële synchronisatie van een groep voor synchronisatie en wat u kunt doen om te voorkomen dat een initiële synchronisatie duurt langer dan nodig en goedkoper meer dan nodig.

### <a name="how-initial-synchronization-works"></a>Hoe initiële synchronisatie werkt

Wanneer u een groep voor synchronisatie maakt, kunt u beginnen met gegevens in slechts één database. Als u gegevens in meerdere databases hebt, wordt elke rij in synchroniseren van de SQL-gegevens (Preview) beschouwd als een conflict dat oplossen moet. Deze conflictoplossing zorgt ervoor dat de initiële synchronisatie traag gaan duurt enkele dagen tot enkele maanden duren, afhankelijk van de databasegrootte.

Bovendien, als de databases bevinden zich in verschillende datacenters, zijn de kosten van de initiële synchronisatie hoger dan nodig, omdat elke rij moet worden uitgewisseld tussen de verschillende datacenters.

### <a name="recommendation"></a>Aanbeveling

Wanneer mogelijk starten met gegevens in slechts één van de databases van de groep voor synchronisatie.

## <a name="design-to-avoid-synchronization-loops"></a>Ontwerp te vermijden van lussen synchronisatie

Een synchronisatie-lus veroorzaakt wanneer er kringverwijzingen in een groep voor synchronisatie zodat elke wijziging in één database wordt gerepliceerd via de databases in de groep voor synchronisatie circulair en dit leiden tot eindeloze. Wilt u dat synchronisatie lussen worden voorkomen als ze de prestaties nadelig beïnvloeden en kunnen de kosten aanzienlijk verhogen.

## <a name="avoid-out-of-date-databases-and-sync-groups"></a>Verouderde databases vermijden en groepen synchroniseren

Een groep voor synchronisatie of vanuit een groep voor synchronisatie met een database kan worden verouderd zijn. Wanneer een groep voor synchronisatie met de status 'verouderde', niet meer werkt. Als een database de status 'verouderde', kunnen gegevens verloren zijn. Het is raadzaam deze situaties voorkomen in plaats van te herstellen vanaf deze.

### <a name="avoid-out-of-date-databases"></a>Verouderde databases voorkomen

Status van een database is ingesteld op verouderde wanneer deze offline is geweest voor 45 dagen of meer. Vermijd de verouderde status voor een database door ervoor te zorgen dat geen van de databases van 45 dagen of meer offline zijn.

### <a name="avoid-out-of-date-sync-groups"></a>Verouderde synchronisatiegroepen voorkomen

Een groep voor synchronisatie met de status wordt ingesteld op verlopen wanneer een wijziging in de groep voor synchronisatie mislukt doorgeven aan de rest van de groep voor synchronisatie van 45 dagen of meer. De status van de verouderde op een groep voor synchronisatie voorkomen door het regelmatig controleren van de groep voor synchronisatie geschiedenislogboek. Zorg ervoor dat alle conflicten opgelost worden en wijzigingen zijn doorgegeven in de databases van de groep synchroniseren.

Een groep voor synchronisatie mislukken mogelijk een wijziging toepassen redenen zijn:

-   Incompatibel schema tussen tabellen.

-   Gegevens incompatibiliteit tussen tabellen.

-   Invoegen van een rij met een null-waarde in een kolom die null-waarden zijn niet toegestaan.

-   Bijwerken van een rij met een waarde die in strijd met een foreign key-beperking.

U kunt voorkomen dat verouderd synchronisatiegroepen door:

-   Het schema zodat de waarden in de mislukte rijen bijwerken.

-   Werk de refererende sleutel waarden zodanig dat de waarden in de mislukte rijen.

-   Werk de gegevenswaarden in de rij is mislukt voor compatibiliteit met het schema of refererende sleutels in de doeldatabase.

## <a name="avoid-deprovisioning-issues"></a>Opheffen van inrichting problemen voorkomen

Onder bepaalde omstandigheden de registratie van een database met een clientagent kan leiden tot synchronisaties mislukken.

### <a name="scenario"></a>Scenario

1. Groep voor synchronisatie met een is gemaakt met een exemplaar van SQL-Database en een lokale SQL Server-database, die gekoppeld aan de lokale agent 1 is.

2. Dezelfde lokale database is met de lokale agent 2 (deze agent is niet gekoppeld aan een groep voor synchronisatie) geregistreerd.

3. De registratie van de lokale-database van de lokale agent 2 Hiermee verwijdert u de bijhouden/meta-tabellen voor de synchronisatie A voor de lokale database.

4. Nu de synchronisatiebewerkingen groep-A mislukt met de volgende fout: "de huidige bewerking kan niet worden voltooid omdat de database nog niet is ingericht voor synchronisatie of u geen machtigingen voor de synchronisatie van configuratie-tabellen hebt."

### <a name="solution"></a>Oplossing

De situatie volledig door nooit registreren van een database met meer dan één agent voorkomen.

Herstellen van deze situatie:

1. Verwijder de database van elke groep voor synchronisatie hoort bij.

2. De database weer in elke groep voor synchronisatie die vanaf zojuist verwijderd toevoegen.

3. Elke groep betrokken sync (die voorziet in de database) implementeren.

## <a name="handling-changes-that-fail-to-propagate"></a>Verwerken van wijzigingen die niet worden doorgegeven

### <a name="reasons-that-changes-fail-to-propagate"></a>Redenen die wijzigingen niet doorgeven

Wijzigingen kunnen niet worden doorgegeven vele oorzaken. Enkele oorzaken zijn:

-   Schema/Datatype incompatibiliteit.

-   Probeert in te voegen null in niet-nullbare kolommen.

-   Referentiële-sleutelbeperkingen schenden.

### <a name="what-happens-when-changes-fail-to-propagate"></a>Wat gebeurt er wanneer wijzigingen worden doorgegeven mislukken?

-   Groep voor synchronisatie wordt gedemonstreerd met een waarschuwingsstatus.

-   Informatie vindt u in de Portal-gebruikersinterface-Logboeken.

-   Als het probleem niet is opgelost 45 dagen, wordt de database niet actueel.

> [!NOTE]
> Deze wijzigingen doorgegeven nooit. De enige manier om te herstellen is opnieuw maken van de groep voor synchronisatie.

### <a name="recommendation"></a>Aanbeveling

Controleren van de groep voor synchronisatie en de Database status regelmatig via de portal en log-interface.

## <a name="modifying-your-sync-group"></a>Wijzigen van een groep voor synchronisatie

Probeer niet te verwijderen van een database van een groep voor synchronisatie en bewerk vervolgens de groep voor synchronisatie zonder eerste implementatie een van de wijzigingen.

Een database eerst verwijderen uit een groep voor synchronisatie. Vervolgens implementeert u de wijziging en wacht ongedaan inrichten is voltooid. Zodra deze bewerking is voltooid, kunt u deze kunt bewerken van de groep voor synchronisatie en de wijzigingen te implementeren.

Als u probeert te verwijderen van een database en bewerk vervolgens een groep voor synchronisatie zonder de eerste implementatie van een van de wijzigingen, een of de andere bewerking is mislukt en de interface van de portal kan krijgen tot een inconsistente status heeft. In dit geval kunt u de pagina voor het herstellen van de juiste status vernieuwen.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het synchroniseren van de SQL-gegevens:

-   [Synchronisatie van gegevens over meerdere cloud en on-premises databases met synchroniseren van Azure SQL-gegevens](sql-database-sync-data.md)
-   [Aan de slag met Azure SQL-gegevenssynchronisatie](sql-database-get-started-sql-data-sync.md)
-   [Problemen oplossen met het synchroniseren van Azure SQL-gegevens](sql-database-troubleshoot-data-sync.md)

-   Voer de PowerShell-voorbeelden die laten hoe u zien voor het synchroniseren van de SQL-gegevens configureren:
    -   [PowerShell gebruiken om te synchroniseren tussen meerdere Azure SQL-databases](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL Database en een lokale SQL Server-database.](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [De SQL-gegevens synchroniseren REST-API-documentatie downloaden](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Zie voor meer informatie over SQL-Database:

-   [Overzicht van de SQL-Database](sql-database-technical-overview.md)
-   [Database-levenscyclusbeheer](https://msdn.microsoft.com/library/jj907294.aspx)

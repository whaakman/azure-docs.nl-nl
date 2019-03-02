---
title: Aanbevolen procedures voor Azure SQL Data Sync | Microsoft Docs
description: Meer informatie over aanbevolen procedures voor het configureren en uitvoeren van Azure SQL Data Sync.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 12/20/2018
ms.openlocfilehash: c4e0f30f68d778b55e89b956be2c5545041c4e6f
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57214602"
---
# <a name="best-practices-for-sql-data-sync"></a>Aanbevolen procedures voor SQL Data Sync 

Dit artikel wordt beschreven aanbevolen procedures voor Azure SQL Data Sync.

Zie [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync](sql-database-sync-data.md) voor een overzicht van SQL Data Sync.

> [!IMPORTANT]
> Azure SQL Data Sync biedt op dit moment **geen** ondersteuning voor beheerde exemplaren voor Azure SQL Database.

## <a name="security-and-reliability"></a> Beveiliging en betrouwbaarheid

### <a name="client-agent"></a>Clientagent

-   De clientagent installeren met behulp van de minste bevoegdheden gebruikersaccount dat toegang tot het netwerk service heeft.  
-   Installeer de clientagent op een computer die niet van de on-premises SQL Server-computer.  
-   Geen een on-premises database registreren met meer dan één agent.    
    -   Vermijd dit, zelfs als u verschillende tabellen voor verschillende synchronisatiegroepen synchroniseert.  
    -   Registreren van een on-premises database meerdere client agents poses uitdagingen met zich mee wanneer u een van de synchronisatiegroepen verwijderen.

### <a name="database-accounts-with-least-required-privileges"></a>Database-accounts met de minimaal vereiste bevoegdheden

-   **Voor de installatie van sync**. Maak/Alter Table. ALTER Database. Procedure; maken Selecteer / Alter Schema. Maak de gebruiker gedefinieerd Type.

-   **Voor doorlopende synchronisatie**. Selecteer / Invoegen / bijwerken / verwijderen op tabellen die zijn geselecteerd voor het synchroniseren en op metagegevens synchroniseren en bijhouden van tabellen. Machtiging voor uitvoeren op opgeslagen procedures die zijn gemaakt door de service; Machtiging voor uitvoeren op de gebruiker gedefinieerde tabeltypen.

-   **Voor het opheffen van inrichting**. ALTER op tabellen deel uit van synchroniseren; Selecteer / verwijderen op synchroniseren metagegevenstabellen. Bijhouden van tabellen en opgeslagen procedures en gebruiker gedefinieerde typen synchronisatie bepalen.

Azure SQL Database ondersteunt slechts één set referenties. Als u wilt uitvoeren van deze taken binnen deze beperking, houd rekening met de volgende opties:

-   De referenties voor de verschillende fasen wijzigen (bijvoorbeeld *credentials1* voor setup en *credentials2* voor lopende).  
-   De machtiging van de referenties wijzigen (dat wil zeggen, de machtiging wijzigen nadat synchronisatie is ingesteld).

## <a name="setup"></a>Instellen

### <a name="database-considerations-and-constraints"></a> Database-overwegingen en beperkingen

#### <a name="sql-database-instance-size"></a>Grootte van de SQL-Database-exemplaar

Wanneer u een nieuw exemplaar van SQL-Database maakt, de maximale grootte instellen zodat deze altijd groter zijn dan de database die u implementeert. Als u niet de maximale grootte op groter is dan de geïmplementeerde database instelt, mislukt de synchronisatie. Hoewel SQL Data Sync niet automatische groei biedt, kunt u uitvoeren de `ALTER DATABASE` opdracht voor het verhogen van de grootte van de database nadat deze is gemaakt. Zorg ervoor dat u binnen de maximale grootte van SQL Database-instantie blijven.

> [!IMPORTANT]
> SQL Data Sync worden aanvullende metagegevens met elke database opgeslagen. Zorg ervoor dat u voor deze metagegevens rekening bij het berekenen van ruimte nodig. De hoeveelheid toegevoegd overhead is gerelateerd aan de breedte van de tabellen (bijvoorbeeld smalle tabellen meer overhead vereist) en de hoeveelheid verkeer.

### <a name="table-considerations-and-constraints"></a> Tabel overwegingen en beperkingen

#### <a name="selecting-tables"></a>Tabellen selecteren

U hebt geen om op te nemen van alle tabellen die zich in een database in een synchronisatiegroep. De tabellen die u in een groep voor synchronisatie opneemt van invloed zijn op voor efficiëntie en kosten. Bevatten, en de tabellen die waarvan ze afhankelijk zijn, in een groep voor synchronisatie met alleen als het bedrijf moeten worden opgeslagen.

#### <a name="primary-keys"></a>Primaire sleutels

Elke tabel in een groep voor synchronisatie moet een primaire sleutel hebben. Een tabel die geen primaire sleutel kan niet worden gesynchroniseerd met de SQL Data Sync-service.

Voordat u SQL Data Sync in productie, initiële en lopende synchronisatie prestaties te testen.

#### <a name="empty-tables-provide-the-best-performance"></a>Lege tabellen bieden de beste prestaties

Lege tabellen bieden de beste prestaties bij het initialiseren. Als de doeltabel leeg is, wordt Data Sync bulksgewijs invoegen gebruikt om de gegevens te laden. Anders wordt Data Sync een vergelijking van de rij en invoegen om te controleren op conflicten. Als prestaties niet belangrijk is, maar kunt u synchronisatie instellen tussen tabellen die al gegevens bevatten.

### <a name="provisioning-destination-databases"></a> Bestemming databases inrichten

SQL Data Sync biedt autoprovisioning basic-database.

In deze sectie worden de beperkingen van het inrichten in SQL Data Sync besproken.

#### <a name="autoprovisioning-limitations"></a>Autoprovisioning beperkingen

SQL Data Sync heeft de volgende beperkingen voor autoprovisioning:

-   Selecteer alleen de kolommen die zijn gemaakt in de doeltabel. Kolommen die geen deel uitmaken van de groep voor synchronisatie niet zijn ingericht in de doeltabellen.
-   Indexen worden gemaakt voor de geselecteerde kolommen. Als de index van de bron-tabel kolommen die geen deel uitmaken van de groep voor synchronisatie heeft, worden deze indexen niet zijn ingericht in de doeltabellen.  
-   Indexen voor kolommen van het XML-gegevenstype worden niet ingericht.  
-   CHECK-beperkingen zijn niet ingericht.  
-   Bestaande triggers op de brontabellen worden niet ingericht.  
-   Weergaven en opgeslagen procedures worden niet gemaakt op de doeldatabase.
-   ON UPDATE CASCADE- en ON DELETE CASCADE acties op beperkingen voor referentiële sleutels worden niet opnieuw worden gemaakt in de doeltabellen.
-   Hebt u een decimale of numerieke kolommen met een grotere precisie dan 28, kunnen een probleem met de conversie overloop in SQL Data Sync optreden tijdens de synchronisatie. Het is raadzaam dat u de precisie van de decimale of numerieke kolommen tot en met 28 of minder beperkt.

#### <a name="recommendations"></a>Aanbevelingen

-   Gebruik de mogelijkheden van SQL Data Sync autoprovisioning alleen wanneer u probeert de service uit.  
-   Richt het databaseschema voor de productie.

### <a name="locate-hub"></a> Locaties van de hubdatabase

#### <a name="enterprise-to-cloud-scenario"></a>Enterprise-naar-cloud-scenario

Houd de hubdatabase dicht bij de grootste concentratie van de synchronisatiegroep databaseverkeer om latentie te minimaliseren.

#### <a name="cloud-to-cloud-scenario"></a>Cloud-naar-cloud-scenario

-   Als alle databases in een synchronisatiegroep in één datacenter, kan de hub moet zich bevinden in hetzelfde datacenter. Deze configuratie vermindert de latentie en de kosten van gegevensoverdracht tussen datacenters.
-   Als de databases in een synchronisatiegroep in meerdere datacenters, moet de hub zich bevinden in hetzelfde datacenter als het merendeel van de databases en databaseverkeer.

#### <a name="mixed-scenarios"></a>Gemengde scenario 's

De voorgaande richtlijnen gelden voor complexe synchronisatie groepsconfiguraties, zoals computers die een combinatie van scenario's voor enterprise-naar-cloud en cloud-naar-cloud.

## <a name="sync"></a>Sync

### <a name="avoid-a-slow-and-costly-initial-synchronization"></a> Vermijd traag en duur van de initiële synchronisatie

In deze sectie wordt besproken hoe de initiële synchronisatie van een synchronisatiegroep. Leer hoe u om te voorkomen dat een eerste synchronisatie van duurt langer en wordt duurder dan nodig is.

#### <a name="how-initial-sync-works"></a>Hoe initiële synchronisatie werkt

Wanneer u een groep voor synchronisatie maakt, kunt u beginnen met gegevens in slechts één database. Als u gegevens in meerdere databases hebt, wordt elke rij in SQL Data Sync beschouwd als een conflict dat moet worden opgelost. Deze conflictoplossing zorgt ervoor dat de eerste synchronisatie te langzaam gaan. Als u gegevens in meerdere databases hebt, duurt initiële synchronisatie tussen verschillende dagen en enkele maanden, afhankelijk van de grootte van de database.

Als de databases bevinden zich in verschillende datacenters, moet elke rij reistijd tussen de verschillende datacenters. Dit verhoogt de kosten van een eerste synchronisatie.

#### <a name="recommendation"></a>Aanbeveling

Indien mogelijk, kunt u beginnen met gegevens in slechts één van de synchronisatiegroep databases.

### <a name="design-to-avoid-synchronization-loops"></a> Ontwerp om te voorkomen dat de synchronisatie lussen

Een lus synchronisatie treedt op wanneer er kringverwijzingen in een synchronisatiegroep. In dit scenario is elke wijziging in de ene database eindeloos en circulair gerepliceerd via de databases in de groep voor synchronisatie.   

Zorg ervoor dat u synchronisatie lussen, vermijden omdat ze mindere prestaties tot en kosten aanzienlijk verhogen.

### <a name="handling-changes-that-fail-to-propagate"></a> Wijzigingen die niet worden doorgegeven

#### <a name="reasons-that-changes-fail-to-propagate"></a>Redenen die niet voldoen aan wijzigingen doorgeven

Wijzigingen mislukken doorgeven voor een van de volgende redenen:

-   Schema/datatype incompatibiliteit.
-   Invoegen van NULL-kolommen null.
-   Beperkingen voor referentiële sleutels schenden.

#### <a name="what-happens-when-changes-fail-to-propagate"></a>Wat gebeurt er wanneer wijzigingen niet doorvoeren?

-   Synchroniseren van de groep weer dat deel uitmaakt van een **waarschuwing** staat.
-   Details worden weergegeven in de portal UI-Logboeken.
-   Als het probleem niet is opgelost voor 45 dagen, wordt de database niet actueel.

> [!NOTE]
> Deze wijzigingen doorgegeven nooit. De enige manier om te herstellen in dit scenario is om de synchronisatiegroep opnieuw te maken.

#### <a name="recommendation"></a>Aanbeveling

Bewaak de status synchronisatie groep en de database regelmatig via de portal en meld-interface.


## <a name="maintenance"></a>Onderhoud

### <a name="avoid-out-of-date-databases-and-sync-groups"></a> Vermijd het verouderde databases en groepen synchroniseren

Een synchronisatiegroep of een database in een synchronisatiegroep kunt verouderd raken. Wanneer de status van een synchronisatiegroep is **verouderde**, deze niet meer werkt. Wanneer de status van een database is **verouderde**, gegevens verloren zijn gegaan. Het is raadzaam om te voorkomen dat dit scenario in plaats van probeert te herstellen uit.

#### <a name="avoid-out-of-date-databases"></a>Verouderde databases voorkomen

Een database de status is ingesteld op **verouderde** wanneer deze offline is geweest van 45 dagen of meer. Om te voorkomen dat een **verouderde** status van een database, zorg ervoor dat geen van de databases van 45 dagen of meer offline zijn.

#### <a name="avoid-out-of-date-sync-groups"></a>Verouderde synchronisatiegroepen voorkomen

Status van een synchronisatiegroep is ingesteld op **verouderde** wanneer wijzigingen in de groep voor synchronisatie niet kan worden doorgegeven aan de rest van de groep voor synchronisatie van 45 dagen of meer. Om te voorkomen dat een **verouderde** status op een groep voor synchronisatie, Controleer regelmatig de synchronisatiegroep geschiedenislogboek. Zorg ervoor dat alle conflicten worden opgelost, en dat de wijzigingen is tijdens de synchronisatie groep databases zijn doorgegeven.

Een synchronisatiegroep kan een wijziging toepassen voor een van de volgende redenen mislukken:

-   Schema incompatibiliteit tussen tabellen.
-   Gegevens incompatibiliteit tussen tabellen.
-   Invoegen van een rij met een null-waarde in een kolom die geen null-waarden toegestaan.
-   Bijwerken van een rij met een waarde die in strijd is met een foreign key-beperking.

Om te voorkomen dat een verouderde synchronisatiegroepen:

-   Het schema om toe te staan de waarden die zijn opgenomen in de mislukte rijen bijwerken.
-   Werk de waarden voor refererende sleutels om op te nemen van de waarden die zijn opgenomen in de mislukte rijen.
-   Werk de gegevenswaarden in de rij is mislukt, zodat ze compatibel met het schema of refererende sleutels in de doeldatabase zijn.

### <a name="avoid-deprovisioning-issues"></a> Opheffen van inrichting problemen voorkomen

In sommige gevallen kan registratie van een database met een clientagent kan leiden tot synchronisatie mislukken.

#### <a name="scenario"></a>Scenario

1. Groep voor synchronisatie met een is gemaakt met behulp van een SQL Database-exemplaar en een on-premises SQL Server-database, die gekoppeld aan de lokale agent 1 is.
2. Dezelfde on-premises database is geregistreerd bij de lokale agent 2 (deze agent is niet gekoppeld aan een groep voor synchronisatie).
3. Registratie van de on-premises database van de lokale agent 2 Hiermee verwijdert u de tracering en meta-tabellen voor groep A voor de on-premises database synchroniseren.
4. Een van de synchronisatiebewerkingen groep mislukken vanwege de volgende fout: "De huidige bewerking kan niet worden voltooid omdat de database nog niet is ingericht voor synchronisatie of u bent niet gemachtigd voor de synchronisatie van configuratie-tabellen."

#### <a name="solution"></a>Oplossing

Dit scenario voorkomen, niet een database registreren met meer dan één agent.

Herstellen van dit scenario:

1. Verwijder de database uit elke synchronisatiegroep waartoe deze behoort.  
2. De database weer in elke groep voor synchronisatie die u hebt verwijderd uit toevoegen.  
3. Elke betrokken synchronisatiegroep (de database met deze actie bepalingen) implementeren.  

### <a name="modifying-your-sync-group"></a> Wijzigen van een groep voor synchronisatie

Een database uit een synchronisatiegroep verwijderen en bewerk vervolgens de groep voor synchronisatie zonder de eerste implementatie een van de wijzigingen niet proberen.

Eerst verwijderen in plaats daarvan een database van een synchronisatiegroep. Vervolgens implementeert u de wijziging en wachten op het opheffen van inrichting om te voltooien. Wanneer het opheffen van inrichting is voltooid, kunt u bewerken van de groep voor synchronisatie en de wijzigingen implementeert.

Als u probeert een database verwijderen en bewerk vervolgens een synchronisatiegroep zonder de eerste implementatie een van de wijzigingen, één of de andere bewerking is mislukt. De interface van de portal mogelijk inconsistent worden. Als dit gebeurt, vernieuw de pagina voor het herstellen van de juiste status.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over SQL Data Sync:

-   Overzicht: [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync](sql-database-sync-data.md)
-   Data Sync instellen
    - In de portal - [Zelfstudie: SQL Data Sync instellen om gegevens te synchroniseren tussen Azure SQL Database en SQL Server on-premises](sql-database-get-started-sql-data-sync.md)
    - Met PowerShell
        -  [PowerShell gebruiken om meerdere Azure SQL-databases te synchroniseren](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell gebruiken om te synchroniseren tussen een Azure SQL-database en een on-premises database](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Data Sync-agent: [Data Sync-agent voor Azure SQL Data Sync](sql-database-data-sync-agent.md)
-   Monitor - [SQL Data Sync bewaken met Azure Monitor-Logboeken](sql-database-sync-monitor-oms.md)
-   Problemen oplossen: [Problemen met Azure SQL Data Sync oplossen](sql-database-troubleshoot-data-sync.md)
-   Het synchronisatieschema bijwerken
    -   Met Transact-SQL: [De replicatie van schemawijzigingen in Azure SQL Data Sync automatiseren](sql-database-update-sync-schema.md)
    -   Met PowerShell: [PowerShell gebruiken voor het bijwerken van het synchronisatieschema in een bestaande synchronisatiegroep](scripts/sql-database-sync-update-schema.md)

Zie voor meer informatie over SQL Database:

-   [Overzicht van SQL Database](sql-database-technical-overview.md)
-   [Database lifecycle management](https://msdn.microsoft.com/library/jj907294.aspx)

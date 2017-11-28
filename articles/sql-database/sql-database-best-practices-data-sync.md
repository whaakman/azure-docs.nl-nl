---
title: Aanbevolen procedures voor het synchroniseren van Azure SQL gegevens (Preview) | Microsoft Docs
description: Meer informatie over aanbevolen procedures voor het configureren en synchroniseren van Azure SQL gegevens (Preview) wordt uitgevoerd.
services: sql-database
ms.date: 11/13/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1c8ad4b318d52b5cb6af284b3304cfa7ad35522b
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="best-practices-for-sql-data-sync-preview"></a>Aanbevolen procedures voor het synchroniseren van de SQL-gegevens (Preview) 

In dit artikel beschrijft aanbevolen procedures voor Azure SQL-gegevenssynchronisatie (Preview).

Zie voor een overzicht van het synchroniseren van de SQL-gegevens (Preview) [synchroniseren van gegevens via meerdere cloud en on-premises databases met Azure SQL-gegevenssynchronisatie (Preview)](sql-database-sync-data.md).

## <a name="security-and-reliability"></a>Beveiliging en betrouwbaarheid

### <a name="client-agent"></a>Clientagent

-   De clientagent installeren met behulp van de laagst gebruikersaccount dat toegang tot het netwerk-service heeft.  
-   Installeer de clientagent op een computer die niet van het lokale SQL Server-computer.  
-   Geen een on-premises database registreren met meer dan één agent.    
    -   Dit vermijden zelfs als u verschillende tabellen voor verschillende synchronisatiegroepen synchroniseert.  
    -   Een on-premises database meerdere client agents vormt uitdagingen met zich mee wordt geregistreerd wanneer u een van de synchronisatiegroepen verwijderen.

### <a name="database-accounts-with-least-required-privileges"></a>Accounts van de database met de minimaal vereiste bevoegdheden

-   **Voor synchronisatie setup**. Maak/Alter Table. ALTER Database. Maken van de Procedure; Selecteer / Alter Schema. Maak de gebruiker gedefinieerd Type.

-   **Voor lopende synchronisatie**. Selecteer / Invoegen / bijwerken / verwijderen op tabellen die zijn geselecteerd voor synchronisatie en op metagegevens synchroniseren en bijhouden van tabellen. Machtiging voor uitvoeren voor opgeslagen procedures die zijn gemaakt door de service; Machtiging niet uitvoeren op de gebruiker gedefinieerde tabeltypen.

-   **Voor het opheffen van inrichting**. ALTER op tabellen deel van de synchronisatie; Selecteer / verwijderen op synchronisatie metagegevenstabellen. Controleren op synchronisatie tabellen, opgeslagen procedures en gebruiker gedefinieerde typen bijhouden.

Azure SQL Database ondersteunt slechts één set referenties. Houd rekening met de volgende opties om deze taken binnen deze beperking:

-   De referenties voor verschillende fasen wijzigen (bijvoorbeeld *credentials1* voor setup en *credentials2* voor lopende).  
-   De machtiging van de referenties wijzigen (dat wil zeggen, de machtiging wijzigen nadat synchronisatie is ingesteld).

## <a name="setup"></a>Instellen

### <a name="database-considerations-and-constraints"></a>Database-overwegingen en beperkingen

#### <a name="sql-database-instance-size"></a>De grootte van de SQL-Database-exemplaar

Wanneer u een nieuw exemplaar van SQL-Database maakt, de maximale grootte zo instellen dat het altijd groter is dan de database die u implementeert. Als u niet de maximale grootte op groter is dan de geïmplementeerde database instellen, mislukt de synchronisatie. Hoewel het synchroniseren van de SQL-gegevens (Preview) biedt geen automatische groei bieden, kunt u uitvoeren de `ALTER DATABASE` opdracht om de grootte van de database nadat deze is gemaakt. Zorg ervoor dat u altijd binnen de maximale grootte van SQL Database-exemplaar.

> [!IMPORTANT]
> Synchroniseren van de SQL-gegevens (Preview) slaat aanvullende metagegevens met elke database. Zorg ervoor dat u rekening houden met deze metagegevens wanneer u de benodigde ruimte berekenen. De hoeveelheid toegevoegd overhead is gerelateerd aan de breedte van de tabellen (bijvoorbeeld smalle tabellen meer overhead vereist) en de hoeveelheid verkeer.

### <a name="table-considerations-and-constraints"></a>Tabel overwegingen en beperkingen

#### <a name="selecting-tables"></a>Tabellen selecteren

U hoeft niet te omvatten alle tabellen die zich in een database in een groep voor synchronisatie. De tabellen die u in een groep voor synchronisatie opnemen beïnvloeden efficiëntie en kosten. Tabellen en de tabellen die ze afhankelijk zijn, in een groep voor synchronisatie alleen als het nodig zijn.

#### <a name="primary-keys"></a>Primaire sleutels

Elke tabel in een groep voor synchronisatie moet een primaire sleutel hebben. Een tabel die geen primaire sleutel kan niet worden gesynchroniseerd met de service SQL synchroniseren van gegevens (Preview).

Voordat u SQL synchroniseren van gegevens (Preview) in productie, test u de prestaties van de initiële en lopende synchronisatie.

### <a name="provisioning-destination-databases"></a>Bestemming databases inrichten

Synchroniseren van de SQL-gegevens (Preview) Preview biedt basisfuncties voor database autoprovisioning.

Deze sectie worden de beperkingen van het inrichten in synchroniseren van de SQL-gegevens (Preview) beschreven.

#### <a name="autoprovisioning-limitations"></a>Autoprovisioning beperkingen

Synchroniseren van de SQL-gegevens (Preview) heeft de volgende beperkingen op autoprovisioning:

-   Selecteer alleen de kolommen die zijn gemaakt in de doeltabel.  
    Kolommen die geen deel uitmaken van de groep voor synchronisatie zijn niet in de doeltabellen ingericht.
-   Indexen worden alleen gemaakt voor geselecteerde kolommen.  
    Als de tabel bronindex kolommen die geen deel uitmaken van de groep voor synchronisatie heeft, wordt deze indexen worden niet in de doeltabellen ingericht.  
-   Indexen voor kolommen van het type XML zijn niet ingericht.  
-   CHECK-beperkingen zijn niet ingericht.  
-   Bestaande triggers op de brontabellen zijn niet ingericht.  
-   Weergaven en opgeslagen procedures worden niet gemaakt op de doeldatabase.

#### <a name="recommendations"></a>Aanbevelingen

-   Het synchroniseren van de SQL-gegevens (Preview) autoprovisioning functionaliteit alleen gebruiken wanneer u probeert de service uit.  
-   Voor de productie inrichten schema van de database.

### <a name="locate-hub"></a>Waar u de database hub

#### <a name="enterprise-to-cloud-scenario"></a>Enterprise-naar-cloud-scenario

Om latentie te beperken, de database van de hub dicht bij de grootste punt waar de synchronisatie-groep database verkeer te bewaren.

#### <a name="cloud-to-cloud-scenario"></a>Cloud-naar-cloud-scenario

-   Als alle databases in een groep voor synchronisatie in een datacenter, moet de hub zich bevinden in hetzelfde datacenter. Deze configuratie vermindert de latentie en de kosten van gegevensoverdracht tussen datacenters.
-   Als de databases in een groep voor synchronisatie in meerdere datacenters, moet de hub zich bevinden in hetzelfde datacenter als het merendeel van de databases en databaseverkeer.

#### <a name="mixed-scenarios"></a>Gemengde scenario 's

De voorgaande richtlijnen toepassen op complexe sync configuraties, zoals systemen die een combinatie van scenario's met enterprise-naar-cloud- en cloud-naar-cloud.

## <a name="sync"></a>Sync

### <a name="avoid-a-slow-and-costly-initial-synchronization"></a>Vermijd langzaam en kostbaar initiële synchronisatie

In deze sectie bespreken we de eerste synchronisatie uit een groep voor synchronisatie. Informatie over het voorkomen dat er een initiële synchronisatie van duurt langer en wordt duurder dan nodig.

#### <a name="how-initial-sync-works"></a>Hoe initiële synchronisatie werkt

Wanneer u een groep voor synchronisatie maakt, kunt u beginnen met gegevens in slechts één database. Als u gegevens in meerdere databases hebt, wordt elke rij in synchroniseren van de SQL-gegevens (Preview) beschouwd als een conflict dat moet worden omgezet. Deze conflictoplossing zorgt ervoor dat de eerste synchronisatie langzaam gaan. Als u gegevens in meerdere databases hebt, duurt initiële synchronisatie tussen verschillende dagen en enkele maanden duren, afhankelijk van de databasegrootte.

Als de databases bevinden zich in verschillende datacenters, moet elke rij reizen tussen verschillende datacenters. Dit verhoogt de kosten van een initiële synchronisatie.

#### <a name="recommendation"></a>Aanbeveling

Indien mogelijk kunt u beginnen met gegevens in slechts één van de databases van de groep voor synchronisatie.

### <a name="design-to-avoid-synchronization-loops"></a>Ontwerp te vermijden van lussen synchronisatie

Een synchronisatie-lus treedt op wanneer er circulaire verwijzingen in een groep voor synchronisatie zijn. In dit scenario, elke wijziging in één database is dit leiden tot eindeloze en circulair gerepliceerd via de databases in de groep voor synchronisatie.   

Zorg ervoor dat u synchronisatie dat lussen worden voorkomen, omdat ze mindere prestaties tot en kosten aanzienlijk verhogen.

### <a name="handling-changes-that-fail-to-propagate"></a>Wijzigingen die niet worden doorgegeven

#### <a name="reasons-that-changes-fail-to-propagate"></a>Redenen die wijzigingen niet doorgeven

Wijzigingen kunnen worden doorgegeven voor een van de volgende redenen mislukken:

-   Schema/datatype incompatibiliteit.
-   Het invoegen van null in niet-nullbare kolommen.
-   Referentiële-sleutelbeperkingen schenden.

#### <a name="what-happens-when-changes-fail-to-propagate"></a>Wat gebeurt er wanneer wijzigingen worden doorgegeven mislukken?

-   Synchroniseren van de groep bevat die zich in een **waarschuwing** status.
-   Details worden vermeld in de portal UI-Logboeken.
-   Als het probleem niet is opgelost 45 dagen, wordt de database niet actueel.

> [!NOTE]
> Deze wijzigingen doorgegeven nooit. De enige manier om te herstellen in dit scenario is opnieuw maken van de groep voor synchronisatie.

#### <a name="recommendation"></a>Aanbeveling

Controleren van de synchronisatie en de database de status regelmatig via de portal en log-interface.


## <a name="maintenance"></a>Onderhoud

### <a name="avoid-out-of-date-databases-and-sync-groups"></a>Verouderde databases vermijden en groepen synchroniseren

Een groep voor synchronisatie of een database in een groep voor synchronisatie kunt verouderd raken. Wanneer de status van een groep voor synchronisatie is **verouderd**, deze niet meer werkt. Wanneer de status van een database is **verouderd**, mogelijk gegevens verloren gegaan. Het is raadzaam om te voorkomen dat dit scenario in plaats van probeert te herstellen uit.

#### <a name="avoid-out-of-date-databases"></a>Verouderde databases voorkomen

Status van een database is ingesteld op **verouderd** wanneer deze offline is geweest voor 45 dagen of meer. Om te voorkomen dat een **verouderd** status van een database, zorg ervoor dat geen van de databases offline van 45 dagen of meer.

#### <a name="avoid-out-of-date-sync-groups"></a>Verouderde synchronisatiegroepen voorkomen

Een groep voor synchronisatie met de status wordt ingesteld op **verouderd** als wijzigingen in de groep voor synchronisatie doorgeven aan de rest van de groep voor synchronisatie van 45 dagen of meer is mislukt. Om te voorkomen dat een **verouderd** status van een groep voor synchronisatie van de groep voor synchronisatie geschiedenislogboek regelmatig controleren. Zorg ervoor dat alle conflicten worden opgelost en dat wijzigingen met succes zijn doorgegeven in de databases van de groep synchroniseren.

Een groep voor synchronisatie kan een wijziging toepassen voor een van de volgende redenen mislukken:

-   Incompatibel schema tussen tabellen.
-   Gegevens incompatibiliteit tussen tabellen.
-   Invoegen van een rij met een null-waarde in een kolom die geen null-waarden.
-   Bijwerken van een rij met een waarde die in strijd met een foreign key-beperking.

Om te voorkomen dat een verouderd synchronisatiegroepen:

-   Het schema zodat de waarden die zijn opgenomen in de mislukte rijen bijwerken.
-   Werk de refererende sleutel waarden zodanig dat de waarden die zijn opgenomen in de mislukte rijen.
-   De gegevenswaarden in de mislukte rij bijwerken zodat ze compatibel met het schema of refererende sleutels in de doeldatabase zijn.

### <a name="avoid-deprovisioning-issues"></a>Opheffen van inrichting problemen voorkomen

In sommige gevallen kan de registratie van een database met een clientagent mogelijk synchronisatie mislukken.

#### <a name="scenario"></a>Scenario

1. Groep voor synchronisatie met een is gemaakt met behulp van een exemplaar van SQL-Database en een lokale SQL Server-database, die gekoppeld aan de lokale agent 1 is.
2. Dezelfde lokale database is met de lokale agent 2 (deze agent is niet gekoppeld aan een groep voor synchronisatie) geregistreerd.
3. De registratie van de lokale database van de lokale agent 2 Hiermee verwijdert u de tracering en meta-tabellen voor synchronisatie uit groep A voor de lokale database.
4. Synchronisatie groep een mislukken, vanwege de volgende fout: "de huidige bewerking kan niet worden voltooid omdat de database nog niet is ingericht voor synchronisatie of u geen machtigingen voor de synchronisatie van configuratie-tabellen hebt."

#### <a name="solution"></a>Oplossing

Dit scenario voorkomen, niet registreert een database met meer dan één agent.

Herstellen van dit scenario:

1. Verwijder de database van elke groep voor synchronisatie die erbij hoort.  
2. De database weer in elke groep voor synchronisatie die u hebt verwijderd uit toevoegen.  
3. Implementeer een groep voor elke betrokken synchronisatie (deze actie voorziet in de database).  

### <a name="modifying-your-sync-group"></a>Wijzigen van een groep voor synchronisatie

Niet proberen te verwijderen van een database van een groep voor synchronisatie en bewerk vervolgens de groep voor synchronisatie zonder eerste implementatie een van de wijzigingen.

Eerst verwijderen in plaats daarvan een database uit een groep voor synchronisatie. Vervolgens implementeert u de wijziging en wachten op het opheffen van inrichting om te voltooien. Wanneer het opheffen van inrichting is voltooid, kunt u bewerken van de groep voor synchronisatie en de wijzigingen implementeert.

Als u probeert te verwijderen van een database en bewerk vervolgens een groep voor synchronisatie zonder eerste implementatie een van de wijzigingen, worden een of de andere bewerking is mislukt. De interface van de portal mogelijk inconsistent worden. Als dit gebeurt, vernieuw de pagina voor het herstellen van de juiste status.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het synchroniseren van de SQL-gegevens (Preview):

-   [Synchronisatie van gegevens over meerdere cloud en on-premises databases met Azure SQL-gegevenssynchronisatie (Preview)](sql-database-sync-data.md)
-   [Instellen van Azure SQL-gegevenssynchronisatie (Preview)](sql-database-get-started-sql-data-sync.md)
-   [Monitor voor Azure SQL synchroniseren van gegevens (Preview) met OMS Log Analytics](sql-database-sync-monitor-oms.md)
-   [Problemen oplossen met Azure SQL-gegevenssynchronisatie (Preview)](sql-database-troubleshoot-data-sync.md)  
-   Voer de PowerShell-voorbeelden die laten hoe u zien voor het synchroniseren van de SQL-gegevens (Preview) te configureren:  
    -   [PowerShell gebruiken om te synchroniseren tussen meerdere Azure SQL-databases](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL Database en een lokale SQL Server-database.](scripts/sql-database-sync-data-between-azure-onprem.md)  
-   [Het synchroniseren van de SQL-gegevens (Preview) REST-API-documentatie downloaden](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)  

Zie voor meer informatie over SQL-Database:

-   [Overzicht van de SQL-Database](sql-database-technical-overview.md)
-   [Database-levenscyclusbeheer](https://msdn.microsoft.com/library/jj907294.aspx)

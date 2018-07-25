---
title: De replicatie van schemawijzigingen in Azure SQL Data Sync automatiseren | Microsoft Docs
description: Informatie over het automatiseren van de replicatie van schemawijzigingen in Azure SQL Data Sync.
services: sql-database
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.custom: data-sync
ms.openlocfilehash: cc1c9c9385d34f317ff911d131058b9210065edf
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237040"
---
# <a name="automate-the-replication-of-schema-changes-in-azure-sql-data-sync"></a>De replicatie van schemawijzigingen in Azure SQL Data Sync automatiseren

SQL Data Sync kunnen gebruikers gegevens synchroniseren tussen Azure SQL-Databases en on-premises SQL Server in één richting of in beide richtingen. Een van de huidige beperkingen van SQL Data Sync is een gebrek aan ondersteuning voor de replicatie van wijzigingen in het schema. Telkens wanneer u het tabelschema wijzigt, moet u de wijzigingen handmatig op alle eindpunten, met inbegrip van de hub en alle leden, en werk vervolgens het synchronisatieschema.

Dit artikel bevat een oplossing voor wijzigingen in het schema automatisch repliceren naar alle SQL Data Sync-eindpunten.
1. Deze oplossing gebruikt een DDL-trigger voor het bijhouden van wijzigingen in het schema.
2. De trigger Hiermee voegt u het schema wijzigen-opdrachten in een tabel bijhouden.
3. Deze tabel bijhouden is gesynchroniseerd met alle eindpunten met behulp van de Data Sync-service.
4. DML-triggers na invoegen worden gebruikt om de wijzigingen in het schema toepassen op de andere eindpunten.

In dit artikel ALTER TABLE gebruikt als een voorbeeld van een schema wijzigen, maar deze oplossing werkt ook voor andere typen wijzigingen in het schema.

> [!IMPORTANT]
> Het is raadzaam dat u Lees dit artikel zorgvuldig, met name de secties over [probleemoplossing](#troubleshooting) en [andere overwegingen](#other), voordat u begint met het implementeren van geautomatiseerde schemareplicatie wijzigen in uw omgeving synchroniseren. We raden ook aan dat u leest [gegevens synchroniseren tussen meerdere cloud en on-premises databases met SQL Data Sync](sql-database-sync-data.md). Sommige databasebewerkingen werken mogelijk niet meer de oplossing in dit artikel beschreven. Aanvullende kennis van SQL Server en Transact-SQL kan worden vereist om deze problemen op te.

![De replicatie van schemawijzigingen automatiseren](media/sql-database-update-sync-schema/automate-schema-changes.png)

## <a name="set-up-automated-schema-change-replication"></a>Geautomatiseerde schema wijzigen replicatie instellen

### <a name="create-a-table-to-track-schema-changes"></a>Een tabel als u wilt bijhouden van wijzigingen in het schema maken

Maak een tabel voor het volgen van schemawijzigingen in alle databases in de groep voor synchronisatie:

```sql
CREATE TABLE SchemaChanges (
ID bigint IDENTITY(1,1) PRIMARY KEY,
SqlStmt nvarchar(max),
[Description] nvarchar(max)
)
```

Deze tabel bevat een identiteitskolom om bij te houden van de volgorde van de wijzigingen in het schema. U kunt meer velden voor het melden van meer informatie, indien nodig kunt toevoegen.

### <a name="create-a-table-to-track-the-history-of-schema-changes"></a>Een tabel voor het volgen van de geschiedenis van wijzigingen in het schema maken

Maak een tabel voor het volgen van de ID van de opdracht laatst toegepaste schema wijzigen op alle eindpunten.

```sql
CREATE TABLE SchemaChangeHistory (
LastAppliedId bigint PRIMARY KEY
)
GO

INSERT INTO SchemaChangeHistory VALUES (0)
```

### <a name="create-an-alter-table-ddl-trigger-in-the-database-where-schema-changes-are-made"></a>Een ALTER TABLE DDL-trigger maken in de database waarin de schemawijzigingen zijn aangebracht

Maak een DDL-trigger voor ALTER TABLE-bewerkingen. U hoeft deze trigger maken in de database waarin wijzigingen worden aangebracht. Om conflicten te voorkomen, kunt u alleen wijzigingen in het schema in de ene database in een synchronisatiegroep.

```sql
CREATE TRIGGER AlterTableDDLTrigger
ON DATABASE
FOR ALTER_TABLE
AS

-- You can add your own logic to filter ALTER TABLE commands instead of replicating all of them.

IF NOT (EVENTDATA().value('(/EVENT_INSTANCE/SchemaName)[1]', 'nvarchar(512)') like 'DataSync')

INSERT INTO SchemaChanges (SqlStmt, Description)
    VALUES (EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]', 'nvarchar(max)'), 'From DDL trigger')
```

De trigger wordt een record ingevoegd in de tabel voor elke opdracht ALTER TABLE voor bijhouden van schema. In dit voorbeeld wordt een filter om te voorkomen dat het repliceren van wijzigingen in het schema die via het schema toegevoegd **DataSync**, omdat dit waarschijnlijk gemaakt door de Data Sync-service zijn. Voeg meer filters toe als u wilt dat alleen voor het repliceren van bepaalde typen wijzigingen in het schema.

U kunt ook meer triggers voor het repliceren van andere typen wijzigingen in het schema toevoegen. Bijvoorbeeld, CREATE_PROCEDURE, ALTER_PROCEDURE en DROP_PROCEDURE triggers voor het repliceren van wijzigingen naar de opgeslagen procedures maken.

### <a name="create-a-trigger-on-other-endpoints-to-apply-schema-changes-during-insertion"></a>Trigger maken voor andere eindpunten schemawijzigingen toepassen tijdens het invoegen

Deze trigger wordt uitgevoerd de opdracht schema wijzigen wanneer deze wordt gesynchroniseerd met andere eindpunten. U moet deze trigger maken op alle eindpunten, behalve het waar schemawijzigingen zijn aangebracht (dat wil zeggen, in de database waarin de DDL activeren `AlterTableDDLTrigger` wordt gemaakt in de vorige stap).

```sql
CREATE TRIGGER SchemaChangesTrigger
ON SchemaChanges
AFTER INSERT
AS
DECLARE \@lastAppliedId bigint
DECLARE \@id bigint
DECLARE \@sqlStmt nvarchar(max)
SELECT TOP 1 \@lastAppliedId=LastAppliedId FROM SchemaChangeHistory
SELECT TOP 1 \@id = id, \@SqlStmt = SqlStmt FROM SchemaChanges WHERE id \> \@lastAppliedId ORDER BY id
IF (\@id = \@lastAppliedId + 1)
BEGIN
    EXEC sp_executesql \@SqlStmt
        UPDATE SchemaChangeHistory SET LastAppliedId = \@id
    WHILE (1 = 1)
    BEGIN
        SET \@id = \@id + 1
        IF exists (SELECT id FROM SchemaChanges WHERE ID = \@id)
            BEGIN
                SELECT \@sqlStmt = SqlStmt FROM SchemaChanges WHERE ID = \@id
                EXEC sp_executesql \@SqlStmt
                UPDATE SchemaChangeHistory SET LastAppliedId = \@id
            END
        ELSE
            BREAK;
    END
END
```

Deze trigger wordt uitgevoerd na het invoegen en controleert of de huidige opdracht volgende wordt uitgevoerd. De code logica zorgt ervoor dat er geen schema wijzigen-instructie wordt overgeslagen en alle wijzigingen worden toegepast, zelfs als de cursor niet de juiste volgorde is.

### <a name="sync-the-schema-change-tracking-table-to-all-endpoints"></a>De schemawijziging tabel bijhouden in alle eindpunten synchroniseren

U kunt de tabel het schema bijhouden om alle eindpunten met behulp van de bestaande groep voor synchronisatie of een nieuwe groep voor synchronisatie te synchroniseren. Zorg ervoor dat de wijzigingen in de tabel bijhouden kunnen worden gesynchroniseerd met alle eindpunten, vooral wanneer u synchronisatie in één richting.

Worden niet gesynchroniseerd met de geschiedenistabel voor schema wijzigen, omdat deze tabel verschillende status op verschillende eindpunten onderhoudt.

### <a name="apply-the-schema-changes-in-a-sync-group"></a>De wijzigingen in het schema in een groep voor synchronisatie van toepassing

Alleen schemawijzigingen aangebracht in de database waarin de DDL-trigger wordt gemaakt worden gerepliceerd. Schemawijzigingen in andere databases worden niet gerepliceerd.

Nadat de wijzigingen in het schema worden gerepliceerd naar alle eindpunten, moet u ook extra stappen uitvoeren om het bijwerken van het synchronisatieschema om te starten of stoppen van de synchronisatie van de nieuwe kolommen.

#### <a name="add-new-columns"></a>Nieuwe kolommen toevoegen

1.  Controleer het schema wijzigen.

2.  Vermijd een gegevenswijziging waar de nieuwe kolommen zijn betrokken voordat u de stap die wordt gemaakt van de trigger hebt voltooid.

3.  Wacht totdat de wijzigingen in het schema worden toegepast op alle eindpunten.

4.  Vernieuw het databaseschema en de nieuwe kolom toevoegen aan het synchronisatieschema.

5.  Gegevens in de nieuwe kolom wordt gesynchroniseerd tijdens de volgende synchronisatiebewerking.

#### <a name="remove-columns"></a>Kolommen verwijderen

1.  De kolommen uit het synchronisatieschema verwijderen. Synchroniseren van gegevens stopt het synchroniseren van gegevens in deze kolommen.

2.  Controleer het schema wijzigen.

3.  Het databaseschema vernieuwt.

#### <a name="update-data-types"></a>Update-gegevenstypen

1.  Controleer het schema wijzigen.

2.  Wacht totdat de wijzigingen in het schema worden toegepast op alle eindpunten.

3.  Het databaseschema vernieuwt.

4.  Als de nieuwe en het oude gegevenstypen niet volledig compatibel - bijvoorbeeld: zijn als u van wijzigt `int` naar `bigint` -synchronisatie mislukken voordat de stappen die de triggers maken zijn voltooid. Synchronisatie is voltooid na een nieuwe poging.

#### <a name="rename-columns-or-tables"></a>Wijzig de naam van kolommen of tabellen

Naam wijzigen van kolommen of tabellen wordt Data Sync niet meer werken. Maak een nieuwe tabel of kolom backfill de gegevens en verwijder vervolgens de oude tabel of kolom in plaats van de naam wijzigen.

#### <a name="other-types-of-schema-changes"></a>Andere typen wijzigingen in het schema

Voor andere soorten wijzigingen in het schema - bijvoorbeeld: is opgeslagen procedures maken of verwijderen van een index - het synchronisatieschema bijwerken niet vereist.

## <a name="troubleshoot"></a> Problemen met geautomatiseerde schema wijzigen replicatie oplossen

De replicatie-logica wordt beschreven in dit artikel werkt niet in sommige situaties - bijvoorbeeld: als u een schema wijzigen in een on-premises database die wordt niet ondersteund in Azure SQL Database hebt gemaakt. In dat geval mislukt synchroniseren van de tabel voor bijhouden van schema. U moet handmatig dit probleem oplossen:

1.  Schakel de DDL-trigger en te voorkomen dat alle verdere schemawijzigingen die totdat het probleem is opgelost.

2.  Uitschakelen in de eindpunt-database waar het probleem plaatsvindt, de AFTER INSERT-trigger op het eindpunt waar de schemawijziging kan niet worden gemaakt. Met deze actie kunt de opdracht voor het wijzigen van schema moet worden gesynchroniseerd.

3.  Trigger voor de synchronisatie met het synchroniseren van de tabel voor bijhouden van schema.

4.  Query het schema wijzigen in de eindpunt-database waar het probleem plaatsvindt, geschiedenistabel om op te halen van de ID van de laatste opdracht van de toegepaste schema wijzigen.

5.  Query uitvoeren op de tabel als u wilt weergeven van alle opdrachten met een ID die groter is dan de id-waarde dat in de vorige stap hebt opgehaald voor bijhouden van schema.

    a.  Negeer de opdrachten die in de database eindpunt kunnen niet worden uitgevoerd. U moet omgaan met de schema-inconsistentie. De oorspronkelijke schemawijzigingen ongedaan maken als de inconsistentie heeft gevolgen voor uw toepassing.

    b.  Handmatig toepassen de opdrachten die moeten worden toegepast.

6.  Werk de geschiedenistabel voor wijzigen van schema en de laatste toegepaste-ID ingesteld op de juiste waarde.

7.  Controleer of het schema bijgewerkt is.

8.  De AFTER INSERT-trigger uitgeschakeld in de tweede stap opnieuw inschakelen.

9.  De DDL-trigger uitgeschakeld in de eerste stap opnieuw inschakelen.

Als u wilt voor het opschonen van de records in de traceringstabel schema wijzigen, kunt u verwijderen gebruiken in plaats van TRUNCATE. Nooit een seedbewerking uit de identiteitskolom in tabel bijhouden met behulp van DBCC CHECKIDENT schemawijziging. U kunt nieuwe schemawijziging Traceringstabellen maken en bijwerken van de naam van de tabel in de DDL-trigger als reseeding vereist is.

## <a name="other"></a> Andere overwegingen

-   Databasegebruikers die de hub en de lid-databases configureren moeten beschikken over voldoende machtigingen hebben voor het schema wijzigen-opdrachten uit te voeren.

-   U kunt meer filters toevoegen in de DDL-trigger voor het repliceren van alleen schema wijzigen in de geselecteerde tabellen of bewerkingen.

-   U kunt alleen schemawijzigingen aanbrengen in de database waarin de DDL-trigger wordt gemaakt.

-   Als u een wijziging in een on-premises SQL Server-database aanbrengt, controleert u dat de schemawijziging wordt ondersteund in Azure SQL Database.

-   Als er wijzigingen in het schema worden aangebracht in databases dan de database waarin de DDL-trigger wordt gemaakt, worden de wijzigingen worden niet gerepliceerd. U kunt dit probleem, kunt u de DDL-triggers voor het blokkeren van wijzigingen op andere eindpunten maken.

-   Als u wilt wijzigen van het schema van het hulpprogramma voor het bijhouden van de schema-wijzigingstabel, de DDL-trigger uitschakelen voordat u de wijziging hebt aangebracht, en vervolgens handmatig de wijziging op alle eindpunten toepassen. Het schema in een AFTER INSERT-trigger in dezelfde tabel bijwerken werkt niet.

-   Geen een seedbewerking uit de identiteitskolom met behulp van DBCC CHECKIDENT.

-   Gebruik geen TRUNCATE voor het opschonen van gegevens in de tabel voor bijhouden van schema.

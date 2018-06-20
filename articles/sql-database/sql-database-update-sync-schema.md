---
title: De replicatie van wijzigingen in het schema in Azure SQL-gegevenssynchronisatie automatiseren | Microsoft Docs
description: Informatie over het automatiseren van de replicatie van wijzigingen in het schema in het synchroniseren van Azure SQL-gegevens.
services: sql-database
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: Xiaochen.Wu
ms.reviewer: douglasl
manager: craigg
ms.custom: data-sync
ms.openlocfilehash: 6dbbd5c76953b23225951ffb655ac4f5b71fc1e0
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232322"
---
# <a name="automate-the-replication-of-schema-changes-in-azure-sql-data-sync"></a>De replicatie van wijzigingen in het schema in Azure SQL-gegevenssynchronisatie automatiseren

Synchroniseren van de SQL-gegevens kunnen gebruikers de gegevens in Azure SQL-Databases synchroniseren en on-premises SQL Server in één richting of in beide richtingen. Een van de huidige beperkingen van het synchroniseren van de SQL-gegevens is een gebrek aan ondersteuning voor de replicatie van wijzigingen in het schema. Telkens wanneer u het tabelschema wijzigt, moet u de wijzigingen toepassen op alle eindpunten, met inbegrip van de hub en alle leden handmatig en werk vervolgens het synchronisatieschema.

Dit artikel bevat een oplossing voor wijzigingen in het schema automatisch repliceren naar alle eindpunten voor synchroniseren van de SQL-gegevens.
1. Deze oplossing gebruikt een DDL-trigger voor het bijhouden van wijzigingen in het schema.
2. De trigger voegt de opdrachten van schema wijzigen in een tabel bijhouden.
3. Bijhouden van deze tabel is gesynchroniseerd met alle eindpunten met de service voor het synchroniseren van gegevens.
4. DML-triggers na invoeging worden gebruikt om de wijzigingen in het schema toepassen op de andere eindpunten.

In dit artikel ALTER TABLE gebruikt als een voorbeeld van een schemawijziging, maar deze oplossing werkt ook voor andere soorten wijzigingen in het schema.

> [!IMPORTANT]
> Het is raadzaam dat u dit leest artikel zorgvuldig, met name de secties over [probleemoplossing](#troubleshooting) en [andere overwegingen](#other), voordat u begint met het implementeren van geautomatiseerde schemareplicatie wijzigen in uw omgeving synchronisatie. Wordt ook aangeraden dat u leest [synchroniseren van gegevens via meerdere cloud en on-premises databases met SQL-gegevenssynchronisatie](sql-database-sync-data.md). Bepaalde databasebewerkingen mogelijk de oplossing wordt beschreven in dit artikel worden verbroken. Extra domein kennis van SQL Server en Transact-SQL kan moet deze problemen oplossen.

![De replicatie van schemawijzigingen automatiseren](media/sql-database-update-sync-schema/automate-schema-changes.png)

## <a name="set-up-automated-schema-change-replication"></a>Wijziging van geautomatiseerde schemareplicatie instellen

### <a name="create-a-table-to-track-schema-changes"></a>Een tabel waarin het bijhouden van wijzigingen in het schema

Maak een tabel voor het bijhouden van wijzigingen in het schema in alle databases in de groep voor synchronisatie:

```sql
CREATE TABLE SchemaChanges (
ID bigint IDENTITY(1,1) PRIMARY KEY,
SqlStmt nvarchar(max),
[Description] nvarchar(max)
)
```

Deze tabel bevat een identiteitskolom voor het bijhouden van de volgorde van wijzigingen in het schema. U kunt meer velden voor logboekregistratie van meer informatie, indien nodig kunt toevoegen.

### <a name="create-a-table-to-track-the-history-of-schema-changes"></a>Een tabel om bij te houden van de geschiedenis van wijzigingen in het schema maken

Maak een tabel voor het bijhouden van de ID van de opdracht laatst toegepaste schema wijzigen op alle eindpunten.

```sql
CREATE TABLE SchemaChangeHistory (
LastAppliedId bigint PRIMARY KEY
)
GO

INSERT INTO SchemaChangeHistory VALUES (0)
```

### <a name="create-an-alter-table-ddl-trigger-in-the-database-where-schema-changes-are-made"></a>Een ALTER TABLE DDL-trigger maken in de database waar schemawijzigingen zijn aangebracht

Maak een DDL-trigger voor ALTER TABLE-bewerkingen. U hoeft alleen te maken van deze trigger in de database waar schemawijzigingen zijn aangebracht. Om conflicten te voorkomen, dat u alleen wijzigingen in het schema in een database in een groep voor synchronisatie.

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

De trigger wordt een record in de tabel voor elke opdracht ALTER TABLE voor bijhouden van schema ingevoegd. In dit voorbeeld wordt een filter om te voorkomen dat het repliceren van de schemawijzigingen die via het schema **DataSync**, omdat dit waarschijnlijk gemaakt door de service voor het synchroniseren van gegevens zijn. Meer filters toevoegen als u alleen wilt repliceren van bepaalde soorten wijzigingen in het schema.

U kunt ook meer triggers voor het repliceren van andere typen wijzigingen in het schema toevoegen. Bijvoorbeeld, CREATE_PROCEDURE, ALTER_PROCEDURE en DROP_PROCEDURE triggers voor het repliceren van wijzigingen aan opgeslagen procedures maken.

### <a name="create-a-trigger-on-other-endpoints-to-apply-schema-changes-during-insertion"></a>Geen trigger maken in de andere eindpunten schemawijzigingen toepassen tijdens invoegen

Deze trigger wordt uitgevoerd de opdracht schema wijzigen wanneer deze wordt gesynchroniseerd met de andere eindpunten. U moet deze trigger maken op alle eindpunten, behalve het waar wijzigingen in het schema gemaakt zijn (dat wil zeggen, in de database waarin de DDL activeren `AlterTableDDLTrigger` wordt gemaakt in de vorige stap).

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

Deze trigger wordt uitgevoerd na het invoegen en controleert of de huidige opdracht volgende moet uitgevoerd. De code-logica zorgt ervoor dat er geen instructie schema wijzigen wordt overgeslagen en alle wijzigingen worden toegepast, zelfs als de invoegactie onjuist is.

### <a name="sync-the-schema-change-tracking-table-to-all-endpoints"></a>De tabel met alle eindpunten voor het bijhouden schemawijziging synchroniseren

U kunt de tabel van schema bijhouden om alle eindpunten met de bestaande groep voor synchronisatie of een nieuwe groep voor synchronisatie te synchroniseren. Zorg ervoor dat de wijzigingen in de tabel bijhouden kunnen worden gesynchroniseerd naar alle eindpunten, vooral wanneer u synchronisatie in één richting.

De geschiedenistabel schema wijzigen niet worden gesynchroniseerd omdat deze tabel verschillende status op verschillende eindpunten onderhoudt.

### <a name="apply-the-schema-changes-in-a-sync-group"></a>Toepassen van de wijzigingen in het schema in een groep voor synchronisatie

Alleen schemawijzigingen in de database waarin de DDL-trigger is gemaakt worden gerepliceerd. Schemawijzigingen in andere databases worden niet gerepliceerd.

Nadat de schemawijzigingen worden gerepliceerd naar alle eindpunten, moet u ook extra stappen nemen om het synchronisatieschema te starten of stoppen van de synchronisatie van de nieuwe kolommen bijwerken.

#### <a name="add-new-columns"></a>Nieuwe kolommen toevoegen

1.  Controleer het schema wijzigen.

2.  Vermijd gegevens wijzigen waar de nieuwe kolommen betrokken zijn totdat u de stap die wordt gemaakt van de trigger hebt voltooid.

3.  Wacht totdat de schemawijzigingen worden toegepast op alle eindpunten.

4.  Vernieuw het schema van de database en de nieuwe kolom toevoegen aan het synchronisatieschema.

5.  Gegevens in de nieuwe kolom die wordt gesynchroniseerd tijdens de volgende synchronisatie opnieuw.

#### <a name="remove-columns"></a>Kolommen verwijderen

1.  De kolommen verwijderen uit het synchronisatieschema. Synchroniseren van gegevens stopt het synchroniseren van gegevens in deze kolommen.

2.  Controleer het schema wijzigen.

3.  Vernieuw het databaseschema.

#### <a name="update-data-types"></a>Gegevenstypen van de update

1.  Controleer het schema wijzigen.

2.  Wacht totdat de schemawijzigingen worden toegepast op alle eindpunten.

3.  Vernieuw het databaseschema.

4.  Als de nieuwe en het oude gegevenstypen niet volledig compatibel - bijvoorbeeld, zijn als u van overschakelt `int` naar `bigint` -synchronisatie mislukken voordat de stappen die het maken van de triggers zijn voltooid. Synchronisatie slaagt na een nieuwe poging.

#### <a name="rename-columns-or-tables"></a>Wijzig de naam van kolommen of tabellen

Naam wijzigen van kolommen of tabellen maakt synchroniseren van gegevens stopt. Maak een nieuwe tabel of kolom, backfill de gegevens, en verwijder vervolgens de oude tabel of kolom in plaats van de naam van.

#### <a name="other-types-of-schema-changes"></a>Andere typen wijzigingen in het schema

Voor andere soorten wijzigingen in het schema - bijvoorbeeld is opgeslagen procedures maken of verwijderen van een index - het synchronisatieschema bijwerken niet vereist.

## <a name="troubleshoot"></a> Wijziging van geautomatiseerde schemareplicatie oplossen

De replicatie-logica wordt beschreven in dit artikel werkt niet in sommige situaties - bijvoorbeeld, als u een schema wijzigen in een lokale database die wordt niet ondersteund in Azure SQL Database hebt gemaakt. In dat geval mislukt de schemawijziging tabel voor het bijhouden synchroniseren. U moet handmatig dit probleem oplossen:

1.  De DDL-trigger uitschakelen en te voorkomen dat verdere schemawijzigingen als het probleem is opgelost.

2.  In de endpoint-database waar het probleem zich heeft voorgedaan, schakel de AFTER INSERT-trigger op het eindpunt waar de schemawijziging kan niet worden doorgevoerd. Deze actie kunt de opdracht voor het wijzigen van schema moet worden gesynchroniseerd.

3.  Activeren van de synchronisatie voor synchronisatie van de schemawijziging tabel voor het bijhouden.

4.  Query het schema wijzigen in de endpoint-database waar het probleem zich heeft voorgedaan, geschiedenistabel ophalen van de ID van de laatste opdracht van de toegepaste schema wijzigen.

5.  Query uitvoeren op de schemawijziging bijhouden tabel als u wilt weergeven van alle opdrachten met een ID die groter zijn dan de id-waarde dat u in de vorige stap hebt opgehaald.

    a.  Negeer de opdrachten die in de database van het eindpunt kunnen niet worden uitgevoerd. U moet maken met de schema-inconsistentie. De oorspronkelijke schemawijzigingen herstellen als de inconsistentie van invloed is op uw toepassing.

    b.  De opdrachten die moeten worden toegepast handmatig toepassen.

6.  Werk de geschiedenistabel schema wijzigen en de ID van recentste toegepaste ingesteld op de juiste waarde.

7.  Controleer of het schema bijgewerkt is.

8.  De trigger AFTER INSERT is uitgeschakeld in de tweede stap opnieuw inschakelen.

9.  De DDL-trigger uitgeschakeld in de eerste stap opnieuw inschakelen.

Als u opschonen van de records in de tabel schema wijzigen bijhouden wilt, gebruikt u verwijderen in plaats van TRUNCATE. Nooit een seedbewerking de identiteitskolom in tabel bijhouden met DBCC CHECKIDENT schemawijziging. U kunt nieuwe schemawijziging Traceringstabellen maken en bijwerken van de naam van de tabel in de DDL-trigger als reseeding vereist is.

## <a name="other"></a> Andere overwegingen

-   Databasegebruikers die de hub en lid-databases configureren moeten voldoende machtigingen hebben voor het schema wijzigen opdrachten uit te voeren.

-   U kunt meer filters toevoegen in de DDL-trigger voor replicatie van alleen schemawijziging geselecteerde tabellen of bewerkingen.

-   U kunt alleen schemawijzigingen aanbrengen in de database waar de DDL-trigger wordt gemaakt.

-   Als u een wijziging in een lokale SQL Server-database aanbrengt, moet u dat de schemawijziging wordt ondersteund in Azure SQL Database.

-   Als wijzigingen in het schema worden aangebracht in databases dan de database waar de DDL-trigger wordt gemaakt, worden de wijzigingen niet gerepliceerd. Om dit te voorkomen, kunt u de DDL-triggers voor het blokkeren van wijzigingen in de andere eindpunten maken.

-   Als u wilt wijzigen wordt het schema van het schema wijzigen tabel bijhouden, de DDL-trigger uitschakelen voordat u de wijzigingen aanbrengen en vervolgens handmatig de wijziging op alle eindpunten toepassen. Bijwerken van het schema in een AFTER INSERT-trigger op dezelfde tabel werkt niet.

-   Een seedbewerking de identiteitskolom met behulp van DBCC CHECKIDENT niet.

-   Gebruik geen TRUNCATE om gegevens in de tabel voor bijhouden van schema op te schonen.

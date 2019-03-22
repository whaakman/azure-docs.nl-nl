---
title: Gebruik van referentiegegevens uit een SQL-Database voor een Azure Stream Analytics-taak (Preview)
description: Dit artikel wordt beschreven hoe u een SQL-Database gebruikt als referentie-invoer van gegevens voor een Azure Stream Analytics-taak in de Azure-portal en Visual Studio.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/29/2019
ms.openlocfilehash: 3368be291770133cdfa10158f6e30540e17b8223
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58084307"
---
# <a name="use-reference-data-from-a-sql-database-for-an-azure-stream-analytics-job-preview"></a>Gebruik van referentiegegevens uit een SQL-Database voor een Azure Stream Analytics-taak (Preview)

Azure Stream Analytics biedt ondersteuning voor Azure SQL Database als een bron van de invoer voor referentiegegevens. U kunt SQL-Database gebruiken als referentiegegevens voor uw Stream Analytics-taak in de Azure-portal en Visual Studio met Stream Analytics-hulpprogramma's. In dit artikel laat zien hoe u beide methoden doet.

## <a name="azure-portal"></a>Azure Portal

Gebruik de volgende stappen uit om toe te voegen van Azure SQL Database als een referentie-invoerbron met de Azure-portal:

### <a name="portal-prerequisites"></a>Portal-vereisten

1. Een Stream Analytics-taak maken.

2. Maak een opslagaccount om te worden gebruikt door de Stream Analytics-taak.

3. Uw Azure SQL-Database maken met een gegevensset die moet worden gebruikt als verwijzingsgegevens door de Stream Analytics-taak.

### <a name="define-sql-database-reference-data-input"></a>SQL-Database referentie-invoer voor gegevens definiëren

1. Selecteer in de Stream Analytics-taak, **invoer** onder **taaktopologie**. Klik op **verwijzing invoer toevoegen** en kies **SQL-Database**.

   ![Stream Analytics-Taakinvoer](./media/sql-reference-data/stream-analytics-inputs.png)

2. Vul de configuraties van Stream Analytics-invoer. Kies de databasenaam, de servernaam, de gebruikersnaam en wachtwoord. Als u wilt dat uw invoer voor het periodiek vernieuwen referentiegegevens, kiest u 'Aan' om op te geven van de vernieuwingsfrequentie in hh. Als u grote gegevenssets met een korte vernieuwingsfrequentie hebt, kunt u een [delta-query](sql-reference-data.md#delta-query).

   ![Configuratie van SQL Database-verwijzing](./media/sql-reference-data/sql-input-config.png)

3. De snapshot-query in de SQL-query-editor te testen. Zie voor meer informatie, [van de Azure portal SQL query-editor gebruiken om te verbinden en gegevens op te vragen](../sql-database/sql-database-connect-query-portal.md)

### <a name="specify-storage-account-in-job-config"></a>Storage-account in de configuratie van de taak opgeven

Navigeer naar **opslagaccountinstellingen** onder **configureren** en selecteer **storage-account toevoegen**.

   ![Stream Analytics-opslagaccountinstellingen](./media/sql-reference-data/storage-account-settings.png)

### <a name="start-the-job"></a>Taak starten

Als u andere invoer, uitvoer en query hebt geconfigureerd, kunt u de Stream Analytics-taak starten.

## <a name="tools-for-visual-studio"></a>Hulpprogramma's voor Visual Studio

Gebruik de volgende stappen uit om toe te voegen van Azure SQL Database als een referentie-invoerbron met Visual Studio:

### <a name="visual-studio-prerequisites"></a>Visual Studio-vereisten

1. Als u Visual Studio 2017 gebruikt, moet u bijwerken naar 15.8.2 of hoger. Houd er rekening mee dat 16,0 en hoger worden niet ondersteund op dit moment.

2. [De Stream Analytics-hulpprogramma's voor Visual Studio installeren](stream-analytics-tools-for-visual-studio-install.md). De volgende versies van Visual Studio worden ondersteund:

   * Visual Studio 2015
   * Visual Studio 2017

3. Vertrouwd raken met de [Stream Analytics-hulpprogramma's voor Visual Studio](stream-analytics-quick-create-vs.md) Quick Start.

4. Een opslagaccount maken.

### <a name="create-a-sql-database-table"></a>Een SQL-Database-tabel maken

SQL Server Management Studio gebruiken om een tabel voor het opslaan van de referentiegegevens te maken. Zie [ontwerpen van uw eerste Azure SQL-database met behulp van SSMS](../sql-database/sql-database-design-first-database.md) voor meer informatie.

De voorbeeldtabel die wordt gebruikt in het volgende voorbeeld is gemaakt op basis van de volgende instructie:

```SQL
create table chemicals(Id Bigint,Name Nvarchar(max),FullName Nvarchar(max));
```

### <a name="choose-your-subscription"></a>Kies uw abonnement

1. Selecteer in Visual Studio in het menu **Beeld** de optie **Server Explorer**.

2. Klik met de rechtermuisknop op **Azure**, selecteer **verbinding maken met Microsoft Azure-abonnement**, en meld u aan met uw Azure-account.

### <a name="create-a-stream-analytics-project"></a>Een Stream Analytics-project maken

1. Selecteer **Bestand > Nieuw > Project**. 

2. Selecteer **Stream Analytics**in de sjablonenlijst aan de linkerkant en selecteer vervolgens **Azure Stream Analytics Application**. 

3. Voer het project **naam**, **locatie**, en **oplossingsnaam**, en selecteer **OK**.

   ![Nieuwe Stream Analytics-project in Visual Studio](./media/sql-reference-data/stream-analytics-vs-new-project.png)

### <a name="define-sql-database-reference-data-input"></a>SQL-Database referentie-invoer voor gegevens definiëren

1. Maak een nieuwe invoer.

   ![Nieuwe Stream Analytics-invoer in Visual Studio](./media/sql-reference-data/stream-analytics-vs-input.png)

2. Dubbelklik op **Input.JSON ziet** in de **Solution Explorer**.

3. Vul de **Stream Analytics-invoer configuratie**. Kies de databasenaam, de naam van server vernieuwingstype en de vernieuwingsfrequentie. Geef de vernieuwingsfrequentie in de notatie `DD:HH:MM`.

   ![Stream Analytics-invoer configuratie in Visual Studio](./media/sql-reference-data/stream-analytics-vs-input-config.png)

   Als u 'Slechts één keer worden uitgevoerd' of 'Uit te voeren periodiek' kiest, een SQL-CodeBehind-bestand met de naam **[Alias invoer].snapshot.sql** wordt gegenereerd in het project onder de **Input.JSON ziet** bestandsknooppunt.

   ![Invoer code achter in Visual Studio](./media/sql-reference-data/once-or-periodically-codebehind.png)

   Als u 'Vernieuwt periodiek met Delta-', twee SQL CodeBehind bestanden wordt gegenereerd: **[Alias invoer].snapshot.sql** en **[Alias invoer].delta.sql**.

   ![Code achter in de solution explorer](./media/sql-reference-data/periodically-delta-codebehind.png)

4. De SQL-bestand openen in de editor en de SQL-query schrijven.

5. Als u van Visual Studio 2017 gebruikmaakt en u SQL Server Data tools hebt geïnstalleerd, kunt u de query testen door te klikken op **Execute**. Een venster van de wizard kunt u verbinding maken met de SQL-database wordt weergegeven en wordt het queryresultaat weergegeven in het venster onderaan.

### <a name="specify-storage-account"></a>Storage-account opgeven

Open **JobConfig.json** om op te geven van de storage-account voor het opslaan van momentopnamen van de SQL-referentie.

   ![Configuratie van Stream Analytics-taak in Visual Studio](./media/sql-reference-data/stream-analytics-job-config.png)

### <a name="test-locally-and-deploy-to-azure"></a>Lokaal testen en implementeren in Azure

Voordat u de taak implementeert naar Azure, kunt u de querylogica lokaal tegen live invoergegevens testen. Zie voor meer informatie over deze functie [testen van live gegevens lokaal met behulp van Azure Stream Analytics-hulpprogramma's voor Visual Studio (Preview)](stream-analytics-live-data-local-testing.md). Wanneer u klaar bent testen, klikt u op **verzenden naar Azure**. Naslaginformatie over de [een Stream Analytics met behulp van de Azure Stream Analytics-hulpprogramma's voor Visual Studio](stream-analytics-quick-create-vs.md) Quick Start voor meer informatie over om de taak te starten.

## <a name="delta-query"></a>Verschilquery

Bij het gebruik van de delta-query [tijdelijke tabellen in Azure SQL Database](../sql-database/sql-database-temporal-tables.md) worden aanbevolen.

1. Een tijdelijke tabel maken in Azure SQL Database.
   
   ```SQL 
      CREATE TABLE DeviceTemporal 
      (  
         [DeviceId] int NOT NULL PRIMARY KEY CLUSTERED 
         , [GroupDeviceId] nvarchar(100) NOT NULL
         , [Description] nvarchar(100) NOT NULL 
         , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
         , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
         , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
      )  
      WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DeviceHistory));  -- DeviceHistory table will be used in Delta query
   ```
2. De auteur van de momentopname-query. 

   Gebruik de  **\@snapshotTime** parameter om op te geven van de runtime van Stream Analytics om te verkrijgen van de referentiegegevensset van SQL database tijdelijke tabel in de systeemtijd geldig zijn. Als u deze parameter niet opgeeft, kunt u dat een onjuist basis referentiegegevensset gevolg van tijdsverschillen te verkrijgen. Hieronder ziet u een voorbeeld van volledige momentopname query:
   ```SQL
      SELECT DeviceId, GroupDeviceId, [Description]
      FROM dbo.DeviceTemporal
      FOR SYSTEM_TIME AS OF @snapshotTime
   ```
 
2. De auteur van de delta-query. 
   
   Deze query haalt alle rijen in uw SQL-database die zijn ingevoegd of verwijderd binnen een begintijd  **\@deltaStartTime**, en een eindtijd  **\@deltaEndTime**. De delta-query moet retourneren dezelfde kolommen als de momentopname-query, evenals de kolom  **_bewerking_**. Deze kolom wordt gedefinieerd als de rij is ingevoegde of tussen verwijderde  **\@deltaStartTime** en  **\@deltaEndTime**. De resulterende rijen worden gemarkeerd als **1** als de records zijn ingevoegd, of **2** als verwijderd. 

   Records die zijn bijgewerkt, wordt de tijdelijke tabel boekhouding door het vastleggen van een bewerking voor invoegen en verwijderen. De runtime van Stream Analytics worden de resultaten van de delta-query toepassen op de vorige momentopname naar de referentiegegevens up-to-date te houden. Een voorbeeld van delta-query is hieronder weergeven:

   ```SQL
      SELECT DeviceId, GroupDeviceId, Description, 1 as _operation_
      FROM dbo.DeviceTemporal
      WHERE ValidFrom BETWEEN @deltaStartTime AND @deltaEndTime   -- records inserted
      UNION
      SELECT DeviceId, GroupDeviceId, Description, 2 as _operation_
      FROM dbo.DeviceHistory   -- table we created in step 1
      WHERE ValidTo BETWEEN @deltaStartTime AND @deltaEndTime     -- record deleted
   ```
 
   Houd er rekening mee dat door Stream Analytics runtime periodiek de snapshot-query naast de delta-query voor het opslaan van controlepunten kan worden uitgevoerd.

## <a name="faqs"></a>Veelgestelde vragen

**Moet ik betalen voor extra kosten met behulp van SQL referentie-invoer voor gegevens in Azure Stream Analytics?**

Er zijn geen extra [kosten per eenheid streaming](https://azure.microsoft.com/pricing/details/stream-analytics/) in de Stream Analytics-taak. De Stream Analytics-taak moet echter een gekoppelde Azure storage-account hebben. De Stream Analytics-taak vraagt de SQL-database (tijdens de taak starten en interval voor vernieuwen) om op te halen van de referentiegegevensset en winkels die momentopname in de storage-account. Opslaan van deze momentopnamen worden extra kosten uiteengezet in de [pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/) voor Azure storage-account.

**Hoe weet ik verwijzing momentopname van de gegevens worden opgevraagd uit de SQL-database en gebruikt in de Azure Stream Analytics-taak?**

Er zijn twee metrische gegevens die zijn gefilterd op logische naam (onder de metrische gegevens over Azure-Portal) die u gebruiken kunt voor het bewaken van de status van de SQL database-referentiegegevens invoer.

   * Invoergebeurtenissen groter is dan: Met deze metriek wordt het aantal records in van de SQL-database referentiegegevensset geladen.
   * InputEventBytes: Met deze metriek wordt de grootte van de momentopname van verwijzing gegevens geladen in het geheugen van de Stream Analytics-taak. 

De combinatie van beide van deze metrische gegevens kan worden gebruikt als de taak is de SQL-database om op te halen van de referentiegegevensset uitvoeren van query's en deze vervolgens in het geheugen geladen afleiden.

**Moet ik een speciaal type Azure SQL Database?**

Azure Stream Analytics werkt met elk type Azure SQL Database. Het is echter belangrijk te weten dat de vernieuwingsfrequentie instellen voor uw referentie-invoer voor gegevens laden van uw query kan beïnvloeden. Als u wilt de delta-query-optie gebruikt, moet het verdient tijdelijke tabellen gebruiken in Azure SQL Database.

**Kan ik voorbeeld van invoer uit de SQL-Database referentiegegevens invoer?**

Deze functie is niet beschikbaar.

**Waarom Azure Stream Analytics momentopnamen opslaan in Azure Storage-account?**

Stream Analytics garandeert Exactly-once-gebeurtenissenverwerking Least-once-levering van gebeurtenissen. In gevallen waar tijdelijke impact de taak problemen is een kleine hoeveelheid opnieuw afspelen nodig zijn om te herstellen. Om in te schakelen opnieuw afspelen, is het vereist dat deze momentopnamen die zijn opgeslagen in een Azure Storage-account. Zie voor meer informatie over controlepunt opnieuw afspelen [controlepunt en herhaling concepten in Azure Stream Analytics-taken](stream-analytics-concepts-checkpoint-replay.md).

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van referentiegegevens voor zoekacties in Stream Analytics](stream-analytics-use-reference-data.md)
* [Snelstart: Een Stream Analytics-taak maken met behulp van de Azure Stream Analytics-hulpprogramma's voor Visual Studio](stream-analytics-quick-create-vs.md)
* [Testen van live gegevens lokaal met behulp van Azure Stream Analytics-hulpprogramma's voor Visual Studio (Preview)](stream-analytics-live-data-local-testing.md)

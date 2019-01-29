---
title: Aan de slag met tijdelijke tabellen in Azure SQL Database | Microsoft Docs
description: Leer hoe u aan de slag met het gebruik van tijdelijke tabellen in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 03/21/2018
ms.openlocfilehash: ce489bae3a59da47ad6f3677ef493618d01fd6b6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196647"
---
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Aan de slag met tijdelijke tabellen in Azure SQL-Database
Tijdelijke tabellen zijn een nieuwe functie van het programmeren van Azure SQL Database waarmee u kunt bijhouden en analyseren van de volledige geschiedenis van wijzigingen in uw gegevens, zonder de noodzaak voor aangepaste codering. Tijdelijke tabellen houden gegevens die nauw verwant aan de context van de tijd dat opgeslagen gegevens kunnen worden geïnterpreteerd als geldig alleen binnen de bepaalde periode. Deze eigenschap van tijdelijke tabellen kan efficiënt analyse op basis van tijd en verkrijgen van inzichten van de ontwikkeling van gegevens.

## <a name="temporal-scenario"></a>Tijdelijke Scenario
In dit artikel ziet u de stappen voor het gebruik van tijdelijke tabellen in een toepassingsscenario. Stel dat u wilt bijhouden van gebruikersactiviteit op een nieuwe website die volledig is ontwikkeld of op een bestaande website die u wilt uitbreiden met analyse van activiteit-gebruikers. In dit eenvoudige voorbeeld veronderstellen we dat het aantal bezochte webpagina's tijdens een bepaalde periode is een indicator die moet worden vastgelegd en bewaakt in de database van de website die wordt gehost op Azure SQL Database. Het doel van de historische analyse van gebruikersactiviteit is om op te halen van invoer voor het ontwerpen van de website en betere ervaring bieden voor de bezoekers.

Het databasemodel voor dit scenario is heel eenvoudig - gebruiker activiteit metrische waarde wordt weergegeven met een veld afzonderlijke gehele getallen **PageVisited**, en wordt vastgelegd, samen met algemene informatie voor het gebruikersprofiel. Bovendien voor analyse op basis van tijd houdt u een reeks rijen voor elke gebruiker, waarbij elke rij vertegenwoordigt het aantal pagina's die een bepaalde gebruiker binnen een bepaalde periode bezocht.

![Schema](./media/sql-database-temporal-tables/AzureTemporal1.png)

Gelukkig, hoeft u niet om eventuele inzet in uw app om de activiteitsgegevens van deze te beheren. Dit proces is met tijdelijke tabellen geautomatiseerd - waardoor u volledige flexibiliteit tijdens het ontwerpen van de website en meer tijd om zich te richten op de analyse van de gegevens zelf. Het enige wat u moet doen is om ervoor te zorgen dat **WebSiteInfo** tabel is geconfigureerd als [tijdelijke systeemversietabellen](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). De exacte stappen voor het gebruik van tijdelijke tabellen in dit scenario worden hieronder beschreven.

## <a name="step-1-configure-tables-as-temporal"></a>Stap 1: Tabellen als tijdelijke configureren
Afhankelijk van of u vanaf de ontwikkeling van nieuwe of bestaande toepassingen upgraden, wordt u tijdelijke tabellen maken of bestaande wijzigen door het tijdelijke kenmerken toe te voegen. In het algemeen geval uw scenario is een combinatie van deze twee opties. Voer deze actie met behulp van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) of een andere Transact-SQL-ontwikkeling-hulpprogramma.

> [!IMPORTANT]
> Het wordt aanbevolen om altijd de nieuwste versie van Management Studio te gebruiken, zodat uw versie gesynchroniseerd blijft met updates voor Microsoft Azure en SQL Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="create-new-table"></a>Nieuwe tabel maken
Gebruik context menu-item "Nieuwe Systeemversietabel" in SSMS Object Explorer de query-editor met een script van de sjabloon voor de tijdelijke tabel openen en vervolgens "Geef waarden voor Parameters sjabloon' (Ctrl + Shift + M) gebruiken voor het vullen van de sjabloon:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

Kies in SSDT, 'Tijdelijke tabel (System-Versioned)'-sjabloon bij het toevoegen van nieuwe items aan de databaseproject. Die tabel designer openen en kunt u eenvoudig de indeling van de tabel opgeven:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

U kunt ook de tijdelijke tabel maken door rechtstreeks op de Transact-SQL-instructies te geven, zoals wordt weergegeven in het onderstaande voorbeeld. Houd er rekening mee dat de verplichte elementen van elke tijdelijke tabel zijn de definitie van de periode en de SYSTEM_VERSIONING-component met een verwijzing naar een andere gebruikerstabel historische rij-versies worden opgeslagen:

```
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
```

Wanneer u de tijdelijke systeemversietabel maakt, worden de bijbehorende geschiedenistabel met de standaardconfiguratie wordt automatisch gemaakt. De geschiedenistabel standaard bevat een geclusterde B-tree-index in de periode (einde, start)-kolommen met pagina compressie is ingeschakeld. Deze configuratie is geoptimaliseerd voor de meeste scenario's waarin tijdelijke tabellen worden gebruikt, met name voor [gegevens controle](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

In dit specifieke geval streven we ernaar om uit te voeren op basis van tijd trendanalyse gedurende een langere geschiedenis en met grotere gegevenssets, zodat de opslagkeuze voor de geschiedenistabel een geclusterde columnstore-index is. Een geclusterde columnstore bevat zeer goed compressie en prestaties voor analytische query's. Tijdelijke tabellen bieden u de flexibiliteit voor het configureren van indexen voor de huidige en tijdelijke tabellen is het volledig onafhankelijk van elkaar. 

> [!NOTE]
> Columnstore-indexen zijn beschikbaar in de Premium-laag en in de Standard S3-laag en hoger.
>

Het volgende script toont hoe standaardindex in de geschiedenistabel kan worden gewijzigd in de geclusterde columnstore:

```
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
```

Tijdelijke tabellen worden weergegeven in het Object Explorer met het specifieke pictogram voor makkelijker te herkennen, terwijl de geschiedenistabel wordt weergegeven als een onderliggend knooppunt.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-to-temporal"></a>De bestaande tabel naar tijdelijke wijzigen
We hebben betrekking op de alternatieve scenario waarin de tabel WebsiteUserInfo bestaat al, maar is niet ontworpen voor het bijhouden van wijzigingen. In dit geval kunt u gewoon de bestaande tabel om te worden tijdelijke, zoals wordt weergegeven in het volgende voorbeeld uitbreiden:

```
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
```

## <a name="step-2-run-your-workload-regularly"></a>Stap 2: Uw werkbelasting regelmatig wordt uitgevoerd
Het belangrijkste voordeel van tijdelijke tabellen is dat u niet wilt wijzigen of aanpassen van uw website op een manier om uit te voeren van wijzigingen bijhouden. Nadat u hebt gemaakt, behouden tijdelijke tabellen transparant eerdere versies van de rij telkens wanneer u wijzigingen op uw gegevens uitvoeren. 

Als u wilt gebruikmaken van automatisch bijhouden voor dit specifieke scenario, gaan we net kolom bijwerken **PagesVisited** telkens wanneer de gebruiker installatieserver sessie op de website beëindigd:

```
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
```

Het is belangrijk dat u ziet dat de update-query hoeft niet te weten de precieze tijd wanneer de huidige bewerking is opgetreden en hoe de historische gegevens blijven behouden voor toekomstig gebruik. Beide aspecten worden automatisch verwerkt door de Azure SQL Database. Het volgende diagram illustreert hoe gegevens van geschiedenis van elke update wordt gegenereerd.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a>Stap 3: Analyse van historische gegevens
Als tijdelijke systeemsamenstelling is ingeschakeld, is de analyse van historische gegevens nu slechts één query weg van u. In dit artikel biedt we een aantal voorbeelden die veelvoorkomende vragen van de analyse - alle informatie, Verken verschillende opties die zijn geïntroduceerd in de [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) component.

Als u wilt zien van de 10 belangrijkste gebruikers besteld op het aantal bezochte webpagina's vanaf een uur geleden, voer deze query uit:

```
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
```

U kunt deze query voor het analyseren van de site bezoeken vanaf een dag geleden, een maand geleden eenvoudig wijzigen of op elk gewenst moment in het verleden die u wilt.

Gebruik eenvoudige statistische analyses uitvoeren voor de vorige dag, het volgende voorbeeld:

```
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
```

Als u wilt zoeken naar activiteiten van een specifieke gebruiker binnen een bepaalde periode, gebruik de component die zijn opgenomen:

```
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
```

Grafische visualisatie is met name handig is voor tijdelijke query's, zoals u trends en gebruikspatronen in een intuïtieve manier erg eenvoudig weergeven kunt:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a>Tabelschema in ontwikkeling
Normaal gesproken moet u het schema van de tijdelijke tabel niet wijzigen terwijl u app-ontwikkeling doen. Voor die, voert u eenvoudigweg reguliere ALTER TABLE-instructies en Azure SQL Database wordt op de juiste wijze wijzigingen worden doorgegeven aan de geschiedenistabel. Het volgende script toont hoe u extra kenmerk voor het bijhouden kunt toevoegen:

```
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
```

Op deze manier kunt u de definitie van kolom wijzigen terwijl uw werkbelasting actief is:

```
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
```

Ten slotte kunt u een kolom die u niet meer nodig hebt.

```
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
```

U kunt ook de laatste gebruiken [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) schema van de tijdelijke tabel wijzigen wanneer u met de database (onlinemodus) of als onderdeel van de database-project (offlinemodus verbonden bent).

## <a name="controlling-retention-of-historical-data"></a>Retentie van historische gegevens beheren
Met systeemversietabellen tijdelijke tabellen verhoogt de geschiedenistabel mogelijk de grootte van de database van meer dan gewone tabellen. Een grote en groeiende geschiedenistabel kan resulteren in een probleem dat beide vanwege pure opslagkosten, evenals een prestaties opleggen belasting op het tijdelijke query's. Daarom kan is het ontwikkelen van een bewaarbeleid voor gegevens voor het beheren van gegevens in de geschiedenistabel een belangrijk aspect van plannen en beheren van de levenscyclus van elke tijdelijke tabel. Met Azure SQL Database hebt u de volgende methoden voor het beheren van historische gegevens in de tijdelijke tabel:

* [Tabellen partitioneren](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [Script voor aangepaste opschoning](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over tijdelijke tabellen [MSDN-documentatie](https://msdn.microsoft.com/library/dn935015.aspx).
Ga naar Channel 9 graag een [Succesverhaal van een echte klant tijdelijke implementatie](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) en bekijk een [tijdelijke demonstratie live](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).


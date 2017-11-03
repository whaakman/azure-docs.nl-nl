---
title: Aan de slag met tijdelijke tabellen in Azure SQL Database | Microsoft Docs
description: Informatie over het aan de slag met het gebruik van tijdelijke tabellen in Azure SQL Database.
services: sql-database
documentationcenter: 
author: bonova
manager: jhubbard
editor: 
ms.assetid: c8c0f232-0751-4a7f-a36e-67a0b29fa1b8
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 01/10/2017
ms.author: bonova
ms.openlocfilehash: 58f97c142ba0b9282d8988fc1bc037b9c0c69986
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Aan de slag met tijdelijke tabellen in Azure SQL Database
Tijdelijke tabellen zijn een nieuwe programmeerbaarheidsfunctie van Azure SQL Database waarmee u kunt bijhouden en analyseren van de volledige geschiedenis van wijzigingen in uw gegevens, zonder de noodzaak van een aangepaste codering. Tijdelijke tabellen bewaren gegevens die nauw verwant zijn met de tijd context zodat opgeslagen facts kunnen worden geïnterpreteerd als geldig alleen binnen een bepaalde periode. Deze eigenschap van tijdelijke tabellen maakt efficiënte analyse op basis van tijd en ophalen inzicht in gegevens ontwikkeling.

## <a name="temporal-scenario"></a>Tijdelijke Scenario
In dit artikel ziet u de stappen voor het gebruik van tijdelijke tabellen in een toepassingsscenario. Stel dat u wilt bijhouden van gebruikersactiviteit op een nieuwe website die wordt ontwikkeld vanaf het begin of op een bestaande website die u wilt uitbreiden met gebruiker activiteit analytics. In dit eenvoudige voorbeeld gaan we ervan uit dat het aantal bezochte webpagina's in een periode is een indicator die moet worden vastgelegd en bewaakt in de database van de website die wordt gehost op Azure SQL Database. Het doel van de historische analyse van gebruikersactiviteit is invoer voor de website ontwerpen en bieden betere ervaring voor de bezoekers ophalen.

Het databasemodel voor dit scenario is zeer eenvoudig - gebruiker activiteit metriek wordt weergegeven met een enkele gehele-veld **PageVisited**, en samen met basisinformatie over het gebruikersprofiel wordt vastgelegd. Bovendien voor op basis van de analyse houdt u een reeks rijen voor elke gebruiker, waarbij elke rij vertegenwoordigt het aantal pagina's die een bepaalde gebruiker binnen een bepaalde periode bezocht.

![Schema](./media/sql-database-temporal-tables/AzureTemporal1.png)

Gelukkig hoeft u niet alle mogelijke plaatsen in uw app om de activiteitsgegevens van deze te beheren. Dit proces is met tijdelijke tabellen geautomatiseerd - zodat u de volledige flexibiliteit tijdens het websiteontwerp en meer tijd om zich te richten op de analyse van de gegevens zelf. Het enige wat u hoeft te doen is ervoor te zorgen dat **WebSiteInfo** tabel is geconfigureerd als [tijdelijke systeemversietabellen](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). De exacte stappen te kunnen gebruiken voor tijdelijke tabellen in dit scenario worden hieronder beschreven.

## <a name="step-1-configure-tables-as-temporal"></a>Stap 1: Tabellen als tijdelijke configureren
Afhankelijk van of u vanaf de ontwikkeling van nieuwe of bestaande toepassing te upgraden, wordt u tijdelijke tabellen maken of bestaande wijzigen door tijdelijke kenmerken toe te voegen. In het algemeen geval uw scenario is een combinatie van deze twee opties. Voer deze actie met behulp van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS) [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) of een ander Transact-SQL-ontwikkeling-hulpprogramma.

> [!IMPORTANT]
> Het wordt aanbevolen om altijd de nieuwste versie van Management Studio te gebruiken, zodat uw versie gesynchroniseerd blijft met updates voor Microsoft Azure en SQL Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="create-new-table"></a>Nieuwe tabel maken
Contextmenuoptie 'Nieuwe Systeemversietabel' gebruiken in SSMS Object Explorer de query-editor met een tijdelijke tabel sjabloonscript openen en vervolgens 'Geef waarden voor Sjabloonparameters' (Ctrl + Shift + M) gebruiken voor het vullen van de sjabloon:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

In SSDT, gekozen voor de sjabloon 'Tijdelijke tabel (Systeemversietabellen)' bij het toevoegen van nieuwe items aan het databaseproject. Die tabelontwerp openen en kunt u eenvoudig de tabelindeling van de opgeven:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

U kunt ook een tijdelijke tabel maken door op te geven de Transact-SQL-instructies rechtstreeks, zoals wordt weergegeven in het onderstaande voorbeeld. Houd er rekening mee dat de verplichte elementen van elke tijdelijke tabel zijn de periodedefinitie en de SYSTEM_VERSIONING-component met een verwijzing naar een andere gebruikerstabel historische rij-versies worden opgeslagen:

````
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
````

Wanneer u de tijdelijke systeemversietabel maakt, wordt automatisch de bijbehorende geschiedenistabel met de standaardconfiguratie gemaakt. De tabel met standaardtijden geschiedenis bevat een geclusterde B-tree-index in de kolommen (einde start) met pagina compressie is ingeschakeld. Deze configuratie is optimaal is voor de meeste scenario's waarin tijdelijke tabellen worden gebruikt, met name voor [gegevens controle](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

In dit geval streven we ernaar om uit te voeren op basis van tijd trendanalyse gedurende een langere gegevensgeschiedenis en met grotere gegevenssets, zodat de keuze van de opslag voor de geschiedenistabel een geclusterde columnstore-index is. Een geclusterde columnstore biedt zeer goede compressie en prestaties voor analytische query's. Tijdelijke tabellen bieden u de flexibiliteit voor het configureren van indexen voor de huidige en de tijdelijke tabellen volledig onafhankelijk van elkaar. 

> [!NOTE]
> Columnstore-indexen zijn alleen beschikbaar in de premium servicecategorie.
>

Het volgende script toont hoe Standaardindex voor geschiedenistabel kan worden gewijzigd in de geclusterde columnstore:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Tijdelijke tabellen worden weergegeven in Object Explorer met de specifieke pictogram om gemakkelijker te identificeren, terwijl de geschiedenistabel wordt weergegeven als een onderliggend knooppunt.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-to-temporal"></a>Bestaande tabel te tijdelijke wijzigen
We hebben betrekking op de alternatieve scenario waarin de tabel WebsiteUserInfo bestaat al, maar is niet ontworpen voor het bijhouden van wijzigingen. In dit geval kunt u gewoon de bestaande tabel worden de tijdelijke, zoals wordt weergegeven in het volgende voorbeeld uitbreiden:

````
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
````

## <a name="step-2-run-your-workload-regularly"></a>Stap 2: Uw werkbelasting regelmatig wordt uitgevoerd
Het belangrijkste voordeel van tijdelijke tabellen is dat u niet wilt wijzigen of aanpassen van uw website op een manier om uit te voeren wijzigingen bijhouden. Zodra gemaakt, behouden voor tijdelijke tabellen transparant eerdere versies van de rij telkens wanneer u de wijzigingen op uw gegevens uitvoeren. 

Om te kunnen gebruikmaken van automatische wijzigingen bijhouden in dit scenario, gaan we zojuist kolom bijwerken **PagesVisited** telkens wanneer de gebruiker installatieserver sessie op de website wordt:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Het is belangrijk dat u ziet dat de update-query niet hoeft te weten de precieze tijd wanneer de huidige bewerking is opgetreden en hoe u historische gegevens blijven behouden voor toekomstige analyse. Beide aspecten worden automatisch verwerkt door de Azure SQL Database. Het volgende diagram illustreert hoe gegevens van geschiedenis van elke update wordt gegenereerd.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a>Stap 3: Analyse van de historische gegevens
Als tijdelijke systeemsamenstelling is ingeschakeld, is analyse van historische gegevens nu slechts één query weg van u. In dit artikel we enkele voorbeelden die oplossen van algemene analysis-scenario's - verkennen verschillende opties die zijn geïntroduceerd in voor alle details meer leveren de [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) component.

Als de top 10 gebruikers besteld op het aantal bezochte webpagina's vanaf een uur geleden wilt weergeven, moet u deze query uitvoeren:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

U kunt deze query voor het analyseren van de site bezoeken vanaf een dag geleden, een maand geleden eenvoudig wijzigen of op elk gewenst moment in het verleden die u wenst.

Elementaire statistische analyse uitvoeren voor de vorige dag, gebruikt u het volgende voorbeeld:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Als u wilt zoeken naar activiteiten van een specifieke gebruiker binnen een periode, gebruik de component bevat:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Grafische visualisatie is vooral handig voor tijdelijke query's, zoals u trends en gebruikspatronen in een intuïtieve manier heel eenvoudig weergeven kunt:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a>Zich ontwikkelende tabelschema
Normaal gesproken moet u het schema van de tijdelijke tabel niet wijzigen terwijl er zijn geen ontwikkeling van apps. Voor die, voert u eenvoudigweg reguliere ALTER TABLE-instructies en Azure SQL Database wordt op de juiste wijze wijzigingen worden doorgegeven aan de geschiedenistabel. Het volgende script toont hoe u extra kenmerk voor bijhouden kunt toevoegen:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Op deze manier kunt u de definitie van kolom wijzigen terwijl uw werkbelasting actief is:

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Ten slotte kunt u een kolom die u niet meer nodig hebt.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````

Of gebruik de meest recente [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) aan het schema van de tijdelijke tabel niet wijzigen terwijl u met de database (onlinemodus) of als onderdeel van het databaseproject (offlinemodus verbonden bent).

## <a name="controlling-retention-of-historical-data"></a>Bewaren van historische gegevens beheren
Met de tijdelijke systeemversietabellen, kan het verhogen van de geschiedenistabel de grootte van de database van meer dan gewone tabellen. Een grote en voortdurend groeiende geschiedenistabel kan een probleem die beide vanwege pure opslagkosten, evenals een prestaties opleggen belasting op het tijdelijke uitvoeren van query's vormen. Daarom is ontwikkelen van een bewaarbeleid voor gegevens voor het beheren van gegevens in de geschiedenistabel een belangrijk aspect van het plannen en beheren van de levenscyclus van elke tijdelijke tabel. Met Azure SQL Database hebt u de volgende methoden voor het beheren van historische gegevens in de tijdelijke tabel:

* [Partitioneren van tabel](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [Script voor aangepaste opschoning](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over tijdelijke tabellen [MSDN-documentatie](https://msdn.microsoft.com/library/dn935015.aspx).
Ga naar Channel 9 horen een [echte klant tijdelijke implementatie Succesverhaal](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) en bekijk een [tijdelijke demonstratie live](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).


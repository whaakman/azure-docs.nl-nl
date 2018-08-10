---
title: Azure SQL Data Warehouse opmerkingen bij de Release van juli 2018 | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/06/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: ce1e33a2888b2d0798fb0fe6df87476001e6f7b2
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630259"
---
# <a name="whats-new-in-azure-sql-data-warehouse-july-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse? Juli 2018
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. Dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in juli 2018.

## <a name="lightning-fast-query-performance"></a>Snelle Lightning-queryprestaties
[Azure SQL Data Warehouse](https://aka.ms/sqldw) Hiermee stelt u nieuwe referentiepunten voor prestaties met de introductie van directe toegang tot gegevens waarmee de bewerkingen in willekeurige volgorde worden verbeterd. Directe toegang tot gegevens vermindert de overhead voor bewerkingen voor gegevensverplaatsing met behulp van directe SQL-Server tot bewerkingen voor systeemeigen SQL Server. De integratie met de SQL Server-engine rechtstreeks voor verplaatsing van gegevens betekent dat SQL Data Warehouse nu is **67% sneller dan de Amazon Redshift** met behulp van een werkbelasting afgeleid van de erkende industriestandaard [TPC Benchmark™ H (TPC-H)](http://www.tpc.org/tpch/).

![Azure SQL Data Warehouse is sneller en goedkoper dan Amazon Redshift](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/eb3b908a-464d-4847-b384-9f296083a737.png)
<sub>bron: [Onderzoeksanalist analist onderzoeksrapport: Data Warehouse in de Cloud-Benchmark](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/)</sub>

Naast de prestaties van de runtime, de [Onderzoeksanalist onderzoek](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/) rapport gemeten ook de prijs-prestatieverhouding verhouding om te kwantificeren de USD kosten van specifieke werkbelastingen. SQL Data Warehouse is **ten minste 23 procent goedkoper zijn** dan Redshift voor workloads 30 TB. Met SQL Data Warehouse de mogelijkheid om Computing elastisch schalen, evenals onderbreken en hervatten van werkbelastingen, klanten betalen alleen wanneer de service wordt gebruikt, hun kosten verder te verlagen.
![Azure SQL Data Warehouse is sneller en goedkoper dan Amazon Redshift](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/cb76447e-621e-414b-861e-732ffee5345a.png)
<sub>bron: [Onderzoeksanalist analist onderzoeksrapport: Data Warehouse in de Cloud-Benchmark](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/)</sub>

###<a name="query-concurrency"></a>Query gelijktijdigheid
SQL Data Warehouse zorgt er ook voor dat de gegevens toegankelijk voor uw organisatie is. Microsoft heeft uitgebreid, de service ter ondersteuning van 128 gelijktijdige query's zodat meer gebruikers in de dezelfde database zoeken kunnen en niet door andere aanvragen worden geblokkeerd. Ter vergelijking beperkt Amazon Redshift maximum aantal gelijktijdige query's tot 50, beperken van toegang tot de gegevens binnen de organisatie.

SQL Data Warehouse biedt deze query prestaties en query gelijktijdigheid winst zonder een toename van de prijs en het bouwen van de unieke architectuur met omdat opslag en Computing.

## <a name="finer-granularity-for-cross-region-and-server-restores"></a>Hoe fijner granulariteit voor meerdere regio's en -server herstellen
U kunt nu herstellen in regio's en servers met behulp van een herstelpunt in plaats van het selecteren van geografisch redundante back-ups die worden uitgevoerd om de 24 uur. Cross-regio en server terugzetten worden ondersteund voor beide gebruiker gedefinieerde of Automatische herstelpunten fijner granulariteit voor de beveiliging van aanvullende gegevens inschakelen. Met meer beschikbare herstelpunten, u kunt er zeker van zijn dat uw datawarehouse worden logisch consistent is bij het herstellen van verschillende regio's.

![Terugzetten opdracht - Azure SQL datawarehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6ac23972-9ec0-4502-ab10-7b6bc1a3d947.png)
![opties voor terugzetten voor - Azure SQL datawarehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6c63bd0e-9c52-414d-b4be-d3bd3774ee08.png)

Zie voor meer informatie de [Accelerated en flexibele herstelpunten](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) blogbericht.

## <a name="20-minute-restorations"></a>Herstelprocedures van 20 minuten
SQL Data Warehouse biedt nu het herstellen van een datawarehouse is **minder dan 20 minuten** binnen dezelfde regio, ongeacht de grootte van de database. De hersteltijd is van toepassing ongeacht of de herstelbewerking naar dezelfde of een andere logische server binnen dezelfde regio. Het proces is bovendien verbeterd om te verminderen de hoeveelheid tijd die nodig is om een herstelpunt te maken. In kleine prestatieniveaus (lagere DWU-instellingen), de gebruikerservaring is een *2 x vermindering* in de tijd voor het maken van de momentopname.

Zie voor meer informatie de [Accelerated en flexibele herstelpunten](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) blogbericht.

## <a name="custom-restoration-configurations"></a>Aangepaste herstel-configuraties
U kunt nu het prestatieniveau (DWU) wijzigen bij het herstellen in Azure portal. U kunt ook herstellen naar een bijgewerkte Gen2 datawarehouse. Door terug te zetten naar een Gen 2-exemplaar, kunt u nu de impact van Gen2 voordat evalueren [upgraden van uw datawarehouse Gen1](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation).

![Aangepaste herstel-configuratie - Azure SQL datawarehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/f4c410c7-8515-409c-a983-0976792b8628.png)

## <a name="spdescribeundeclaredparameters"></a>SP_DESCRIBE_UNDECLARED_PARAMETERS
De [sp_describe_undeclared_parameters](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql) opgeslagen procedure wordt vaak gebruikt door hulpprogramma's voor het ophalen van metagegevens over parameters in de Transact-SQL-batch. De procedure retourneert één rij voor elke parameter in de batch met de afgeleid type-informatie voor die parameter. 

```sql
EXEC sp_describe_undeclared_parameters
    @tsql = 
    N'SELECT
        object_id, name, type_desc
      FROM
        sys.indexes
      WHERE
        object_id = @id;'
```

De resultatenset bevat metagegevens over de `@id` parameter (gedeeltelijke resultaten die worden weergegeven)
```
parameter_ordinal | name | suggested_system_type_id | suggested_system_type_name
--------------------------------------------------------------------------------
1                 | @id  | 56                       | int
```
## <a name="sprefreshsqlmodule"></a>SP_REFRESHSQLMODULE
De [sp_refreshsqlmodule](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql) opgeslagen procedure werkt de metagegevens voor een database-object als de onderliggende metagegevens is verouderd vanwege wijzigingen van de onderliggende objecten. Dit kan gebeuren als de basistabellen voor een weergave worden gewijzigd en de weergave is niet opnieuw is gemaakt. Dit bespaart u de stap van het verwijderen en opnieuw maken van afhankelijke objecten.

Het volgende voorbeeld ziet een weergave die na de wijziging van de onderliggende tabel verloopt. U ziet dat de gegevens juist zijn voor de eerste kolom wijzigen (1 tot Mollie), maar de kolomnaam ongeldig is en de tweede kolom niet aanwezig is. 
```sql
CREATE TABLE base_table (Id INT);
GO

INSERT INTO base_table (Id) VALUES (1);
GO

CREATE VIEW base_view AS SELECT * FROM base_table;
GO

SELECT * FROM base_view;
GO

-- Id
-- ----
-- 1

DROP TABLE base_table;
GO

CREATE TABLE base_table (fname VARCHAR(10), lname VARCHAR(10));
GO

INSERT INTO base_table (fname, lname) VALUES ('Mollie', 'Gallegos');
GO

SELECT * FROM base_view;
GO

-- Id
-- ----------
-- Mollie

EXEC sp_refreshsqlmodule @Name = 'base_view';
GO

SELECT * FROM base_view;
GO

-- fname     | lname
-- ---------- ----------
-- Mollie    | Gallegos
```

## <a name="next-steps"></a>Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [maken van een SQL Data Warehouse][create a SQL Data Warehouse]. Als u niet bekend bent met Azure, kan de [Azure-woordenlijst][Azure glossary] handig zijn bij het opzoeken van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

* [Succesverhalen van klanten]
* [Blogs]
* [Functieverzoeken]
* [Video's]
* [Teamblogs met adviezen voor klanten]
* [Stack Overflow-forum]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Succesverhalen van klanten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
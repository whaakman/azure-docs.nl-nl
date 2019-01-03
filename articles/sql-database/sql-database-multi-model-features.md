---
title: Mogelijkheden van Azure SQL Database-multi-model | Microsoft Docs
description: Azure SQL Database kunt u werken met meerdere gegevensmodellen in dezelfde database.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
manager: craigg
ms.date: 12/17/2018
ms.openlocfilehash: 0c46ba3d2fcd59f43df2383c3e0471fa36217536
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53551404"
---
# <a name="multi-model-capabilities-of-azure-sql-database"></a>Multi-modeldatabase mogelijkheden van Azure SQL Database

Databases met meerdere modellen kunnen u voor het opslaan en werken met gegevens die worden weergegeven in meerdere gegevensindelingen zoals relationele gegevens, grafieken, JSON/XML-documenten, sleutel / waarde-paren, enzovoort.

Azure SQL Database is ontworpen voor gebruik met het relationele model dat de beste prestaties in de meeste gevallen voor tal van algemene toepassingen biedt. Azure SQL Database is echter niet beperkt tot relationele-alleen gegevens. Azure SQL Database kunt u een groot aantal niet-relationele indelingen die nauw worden geïntegreerd in het relationele model gebruiken. Azure SQL biedt de volgende functies voor meerdere modellen:
- [Graph-functies](#graph-features) kunt u uw gegevens vertegenwoordigen als set knooppunten en randen en standard Transact-SQL-query's, uitgebreid met graph gebruiken `MATCH` operator op die de graph-gegevens op te vragen.
- [JSON-functies](#json-features) kunt u JSON-documenten in tabellen plaatsen, transformeer relationele gegevens in JSON-documenten en vice versa. U kunt de standaard Transact-SQL-taal uitgebreid met JSON-functies voor het parseren van documenten gebruiken en niet-geclusterde indexen, columnstore-indexen of tabellen geoptimaliseerd voor geheugen, het optimaliseren van uw query's gebruiken.
- [Ruimtelijke functies](#spatial-features) kunt u geografische en geometrische gegevens opslaan en ophalen van de gegevens met behulp van ruimtelijke query's indexeren met behulp van de ruimtelijke indexen.
- [XML-functies](#xml-features) kunt u opslaan en indexeren van XML-gegevens in uw database en gebruik de systeemeigen XQuery/XPath-bewerkingen om te werken met XML-gegevens. Azure SQL-database heeft een speciale ingebouwde XML-query-engine waarmee XML-gegevens worden verwerkt.
- [Sleutel / waarde-paren](#key-value-pairs) niet expliciet worden ondersteund als speciale functies omdat sleutel / waarde-Parijs kan systeemeigen worden gemodelleerd als twee kolommen tabellen.

  > [!Note]
  > U kunt het JSON-padexpressie aan, XQuery/XPath-expressies, ruimtelijke functies en graph-query-expressies in de dezelfde Transact-SQL-query gebruiken voor toegang tot alle gegevens die u in de database opgeslagen. Een hulpprogramma of programmeertaal die Transact-SQL-query's kunt uitvoeren kunt ook ook gebruiken dat query-interface voor toegang tot gegevens van meerdere modellen. Dit is het belangrijkste verschil in vergelijking met de databases met meerdere modellen zoals [Azure Cosmos DB](/azure/cosmos-db/) die gespecialiseerde API biedt voor verschillende gegevensmodellen.

In de volgende secties, kunt u meer informatie over de belangrijkste multi-modeldatabase mogelijkheden van Azures SQL-Database.

## <a name="graph-features"></a>Graph-functies

Azure SQL Database biedt mogelijkheden van graph database model veel-op-veel-relaties in de database. Een grafiek is een verzameling van knooppunten (of hoekpunten) en randen (of relaties). Een knooppunt staat voor een entiteit (bijvoorbeeld een persoon of een organisatie) en een rand voor een relatie tussen de twee knooppunten die deze verbinding (voor bijvoorbeeld likes of vrienden maakt). Hier volgen enkele functies die een grafiekdatabase uniek te maken:
- Randen of relaties zijn eersteklas entiteiten in een Grafiekdatabase en kunnen hebben eigenschappen of kenmerken die zijn gekoppeld aan deze.
- Een enkele edge kunt flexibel verbinding maken met meerdere knooppunten in een Grafiekdatabase.
- U kunt jokertekens en Multihop-navigatie-query's eenvoudig express.
- U kunt transitieve sluiting en polymorf query's eenvoudig express.

De graph-relaties en mogelijkheden van graph-query's zijn geïntegreerd in de Transact-SQL en profiteer van de voordelen van het gebruik van SQL Server als de fundamentele database management systeem.
[Grafische verwerking](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) is van het onderdeel van de Database-Engine van SQL Server core, zodat u meer informatie over de grafiek kunt vinden er verwerken.

### <a name="when-to-use-a-graph-capability"></a>Wanneer u de mogelijkheid van een grafiek

Er is niets die een grafiekdatabase bereiken kunt, die niet kan worden bereikt met behulp van een relationele database. Echter, een grafiekdatabase kunt maken het gemakkelijker om bepaalde query's. Uw beslissing kiezen of het andere kan worden gebaseerd op de volgende factoren:

- Model hiërarchische gegevens waar een van de knooppunten meerdere bovenliggende items, hebben zodat HieararchyId kan niet worden gebruikt
- Heeft uw toepassing heeft veel-op-veel-relaties, complexe; Als de toepassing zich verder ontwikkelt, worden nieuwe relaties toegevoegd.
- U hoeft te analyseren met elkaar verbonden gegevens en relaties.

## <a name="json-features"></a>JSON-functies

Azure SQL Database kunt u parseren en query uitvoeren op gegevens die worden weergegeven in JavaScript Object Notation [(JSON)](http://www.json.org/) formatteren en exporteren van uw relationele gegevens als JSON-tekst.

JSON is een veelgebruikte gegevensindeling die wordt gebruikt voor het uitwisselen van gegevens in moderne webtoepassingen en mobiele toepassingen. JSON wordt ook gebruikt voor het opslaan van semi-gestructureerde gegevens in logboekbestanden of in NoSQL-databases zoals [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Veel REST-webservices resultaten opgemaakt als JSON-tekst of accepteer gegevens opgemaakt als JSON. De meeste Azure-services zoals [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), en [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) REST-eindpunten die retourneren of JSON gebruiken.

Azure SQL Database kunt u eenvoudig werken met JSON-gegevens en het integreren van uw database met moderne services. Azure SQL Database biedt de volgende functies voor het werken met JSON-gegevens:

![JSON-functies](./media/sql-database-json-features/image_1.png)

Als u JSON-tekst hebt, kunt u gegevens ophalen uit JSON of Controleer of JSON correct is geformatteerd met behulp van de ingebouwde functies [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), en [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). De [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) functie kunt u de waarde in JSON-tekst wordt bijgewerkt. Voor meer query's en analyse geavanceerde, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) functie's kunt gebruiken om een matrix met JSON-objecten in een set rijen. Een SQL-query kan worden uitgevoerd op de geretourneerde resultatenset. Ten slotte, er is een [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) component waarmee u gegevens die zijn opgeslagen in de relationele tabellen als JSON-tekst opmaken.

Zie voor meer informatie, [over het werken met JSON-gegevens in azure SQL-Database](sql-database-json-features.md).
[JSON](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) is core SQL Server Database Engine-functie, zodat u meer informatie over de JSON-functie er kunt vinden.

### <a name="when-to-use-a-json-capability"></a>Wanneer u een JSON-mogelijkheid

Document-modellen kunnen worden gebruikt in plaats van de relationele modellen in sommige scenario's:
- Hoog-normalisering van het schema doen niet aanzienlijke voordelen omdat u alle velden van objecten in één keer toegang tot, of u genormaliseerde delen van de objecten niet bijwerken. Het model genormaliseerde verhoogt echter de complexiteit van uw query's vanwege het grote aantal tabellen die u nodig hebt om toe te voegen om de gegevens.
- U werkt met de toepassingen die systeemeigen gebruiken JSON-documenten communicatie of gegevensmodellen zijn en u niet wilt introduceren extra lagen waarmee relationele gegevens in JSON en vice versa worden getransformeerd.
- U moet uw gegevensmodel door ongedaan maken normaliseren onderliggende tabellen of entiteit objectwaarde patronen vereenvoudigen.
- U moet laden of exporteren van gegevens die zijn opgeslagen in JSON-indeling zonder enige extra hulpmiddelen die de gegevens worden geparseerd.

## <a name="spatial-features"></a>Ruimtelijke-functies

Ruimtelijke gegevens Hiermee geeft u informatie over de fysieke locatie en de vorm van geometrische objecten. Deze objecten kunnen worden distributiepuntlocaties of meer complexe objecten zoals landen, wegen of meren.

Azure SQL Database ondersteunt twee typen voor ruimtelijke gegevens: het gegevenstype geometry en geography gegevens typt.
- Het type geometry vertegenwoordigt gegevens in een Euclidean (vast) coördinatensysteem.
- Het type geography vertegenwoordigt gegevens in een round-aarde coördinatensysteem.

Er is een aantal ruimtelijke objecten die kunnen worden gebruikt in Azure SQL-database, zoals [punt](https://docs.microsoft.com/sql/relational-databases/spatial/point), [LineString](https://docs.microsoft.com/sql/relational-databases/spatial/linestring), [veelhoek](https://docs.microsoft.com/sql/relational-databases/spatial/polygon), enzovoort.

Azure SQL Database biedt ook gespecialiseerd [ruimtelijke indexen](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-indexes-overview) die kunnen worden gebruikt om de prestaties van uw ruimtelijke query's te verbeteren.

[Ondersteuning voor ruimtelijke](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) is core SQL Server Database Engine-functie, zodat u meer informatie over de ruimtelijke functie er kunt vinden.

## <a name="xml-features"></a>XML-functies

SQL Server biedt een krachtig platform voor het ontwikkelen van uitgebreide toepassingen voor het beheer van semi-gestructureerde gegevens. Ondersteuning voor XML is geïntegreerd in alle componenten in SQL Server en omvat het volgende:

- Het gegevenstype xml. XML-waarden kunnen systeemeigen worden opgeslagen in een XML-kolom met gegevenstype die kan worden getypt op basis van een verzameling van XML-schema's of links getypeerde. U kunt de kolom XML-index.
- De mogelijkheid om op te geven van een XQuery-query op XML-gegevens die zijn opgeslagen in de kolommen en variabelen van het type xml. XQUery-functionaliteit kunnen worden gebruikt in een Transact-SQL-query die toegang hebben tot een gegevensmodel dat u in de database gebruiken.
- Automatisch indexeren alle elementen in XML-documenten met behulp van [primaire XML-index](https://docs.microsoft.com/sql/relational-databases/xml/xml-indexes-sql-server#primary-xml-index) of geef de exacte paden die moeten worden geïndexeerd met behulp van [secundaire XML-index](https://docs.microsoft.com/sql/relational-databases/xml/xml-indexes-sql-server#secondary-xml-indexes).
- OPENROWSET waarmee bulksgewijs laden van XML-gegevens.
- Relationele gegevens transformeren naar XML-indeling.

[XML](https://docs.microsoft.com/sql/relational-databases/xml/xml-data-sql-server) is core SQL Server Database Engine-functie, zodat u meer informatie over de functie van de XML er kunt vinden.

### <a name="when-to-use-an-xml-capability"></a>Wanneer u een XML-mogelijkheden

Document-modellen kunnen worden gebruikt in plaats van de relationele modellen in sommige scenario's:
- Hoog-normalisering van het schema doen niet aanzienlijke voordelen omdat u alle velden van objecten in één keer toegang tot, of u genormaliseerde delen van de objecten niet bijwerken. Het model genormaliseerde verhoogt echter de complexiteit van uw query's vanwege het grote aantal tabellen die u nodig hebt om toe te voegen om de gegevens.
- U werkt met de toepassingen die systeemeigen Gebruik XML-documenten communicatie of gegevensmodellen zijn en u niet wilt introduceren extra lagen waarmee relationele gegevens naar XML en omgekeerd worden getransformeerd.
- U moet uw gegevensmodel door ongedaan maken normaliseren onderliggende tabellen of entiteit objectwaarde patronen vereenvoudigen.
- U moet laden of exporteren van gegevens die zijn opgeslagen in XML-indeling zonder enige extra hulpmiddelen die de gegevens worden geparseerd.

## <a name="key-value-pairs"></a>Sleutel-waardeparen

Azure SQL Database is geen speciale typen of structuren die ondersteuning bieden voor sleutel / waarde-paren omdat sleutel / waarde-structuren kunnen systeemeigen worden weergegeven als standaard relationele tabellen:

```sql
CREATE TABLE Collection (
  Id int identity primary key,
  Data nvarchar(max)
)
```

U kunt deze sleutel / waarde-structuur aan uw behoeften zonder enige beperkingen aan aanpassen. Als u bijvoorbeeld de waarde in plaats van XML-document kan zijn `nvarchar(max)` type, als de waarde is een JSON-document, kunt u plaatsen `CHECK` beperking die controleert of de geldigheid van de JSON-inhoud. U kunt plaatsen van een willekeurig aantal waarden die betrekking hebben op één sleutel in de extra kolommen, toevoegen van berekende kolommen en indexen te vereenvoudigen en gegevenstoegang te optimaliseren, de tabel hebt gedefinieerd als geoptimaliseerd voor geheugen/alleen-schema-tabel ophalen voor betere prestaties, enzovoort.

Zie [hoe BWin gebruikt In-Memory OLTP voor ongeëvenaarde prestaties en schaal](https://blogs.msdn.microsoft.com/sqlcat/2016/10/26/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/) voor het opslaan van de ASP.NET-oplossing die 1.200.000 bereikt batches per seconde, als een voorbeeld van hoe relationele model effectief kan worden gebruikt als sleutel / waarde-paar oplossing in de praktijk.

## <a name="next-steps"></a>Volgende stappen
Multi-modeldatabase mogelijkheden in Azure SQL-Databases zijn ook de belangrijkste SQL Server Database Engine-functies die worden gedeeld tussen Azure SQL Database en SQL Server. Meer informatie over deze functies, gaat u naar de relationele SQL database-documentatiepagina's:

* [Grafische verwerking](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview)
* [JSON-gegevens](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server)
* [Ruimtelijke-ondersteuning](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [XML-gegevens](https://docs.microsoft.com/sql/relational-databases/xml/xml-data-sql-server)

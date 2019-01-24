---
title: Azure SQL Database In-Memory-technologieën | Microsoft Docs
description: Azure SQL Database In-Memory-technologieën verbeteren aanzienlijk de prestaties van de transactionele en workloads voor analyse.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jodebrui
ms.author: jodebrui
ms.reviewer: ''
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 399a0e6dd2b5c83a599aa50973417ba5a9be708d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54813352"
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>Prestaties optimaliseren met behulp van technologieën In het geheugen in SQL-Database

In-Memory-technologieën in Azure SQL Database kunnen u de prestaties van uw toepassing te verbeteren en kosten van de database mogelijk te reduceren. U kunt met behulp van In-Memory-technologieën in Azure SQL Database, verbeterde prestaties met verschillende workloads bereiken:
- **Transactionele** (online transacties (OLTP) verwerken) waar de meeste van de aanvragen lezen of bijwerken van kleinere set gegevens (bijvoorbeeld CRUD-bewerkingen).
- **Analytische** (online analytical processing (OLAP)) waar de meeste van de query's complexe berekeningen voor het melden van hebt gebruikt, met een bepaald aantal query's die worden geladen en gegevens toevoegen aan de bestaande tabellen (dus bulksgewijs laden genoemd) of verwijderen de gegevens uit de tabellen. 
- **Gemengde** (hybride transactie/analytische verwerking (HTAP)) waarin zowel OLTP en OLAP-query's worden uitgevoerd op dezelfde set gegevens.

Technologieën in het geheugen kunnen de prestaties verbeteren van deze workloads doordat de gegevens die moeten worden verwerkt in het geheugen, met systeemeigen compilatie van de query's, of geavanceerde verwerking van dergelijke als batch verwerkt en SIMD instructies die beschikbaar zijn op de de onderliggende hardware.

Azure SQL-Database heeft de volgende In-Memory-technologieën:
- *[In-Memory OLTP](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization)*  , verhoogt u het aantal transacties per seconde en vermindert de latentie voor transactieverwerking. Scenario's die baat bij In-Memory OLTP hebben zijn: hoge doorvoer transactieverwerking zoals handelspartners en games, opname van gegevens mogelijk gebeurtenissen of IoT-apparaten, caching, laden van gegevens, en de tijdelijke tabel en de variabele scenario's voor een tabel.
- *Geclusterde columnstore-indexen* verminderen uw opslagverbruik (maximaal 10 keer) en verbeterde prestaties voor query's voor rapportage en analyse. U kunt deze met feitentabellen in uw datamarts aanpassen en meer gegevens in uw database en de prestaties verbeteren. Bovendien kunt u deze met historische gegevens in de operationele database te archiveren en kunnen maximaal 10 keer meer gegevens opvragen.
- *Niet-geclusterde columnstore-indexen* voor HTAP kunt u realtime inzicht in uw bedrijf met het uitvoeren van query's de operationele database rechtstreeks, zonder de noodzaak om uit te voeren van dure uitpakken, transformeren, laden (ETL)-proces en wacht tot de het datawarehouse wordt gevuld. Niet-geclusterde columnstore-indexen kunnen snel uitvoering van de analytics-query's op de OLTP-database, terwijl het verminderen van de impact op de operationele werkbelasting.
- *Geoptimaliseerd voor geheugen geclusterde columnstore-indexen* voor HTAP kunt u snel transactieverwerking, uitvoeren en *gelijktijdig* analytics-query's zeer snel worden uitgevoerd op dezelfde gegevens.

Zowel columnstore-indexen en In-Memory OLTP zijn sinds deel uitmaken van de SQL Server 2012 en 2014, respectievelijk. Azure SQL Database en SQL Server delen de dezelfde implementatie van technologieën In het geheugen. Vanaf nu worden nieuwe mogelijkheden voor deze technologieën in Azure SQL Database eerst uitgebracht, voordat deze worden gepubliceerd in SQL Server.

## <a name="benefits-of-in-memory-technology"></a>Voordelen van technologie In het geheugen

Vanwege de efficiëntere query's en transactieverwerking kunnen In-Memory-technologieën u ook om kosten te beperken. U hoeft normaal gesproken te upgraden van de prijscategorie van de database om hogere prestaties. In sommige gevallen kunt u zelfs mogelijk de prijscategorie verlagen terwijl u nog steeds zien voor verbeterde prestaties met In-Memory-technologieën.

Hier volgen twee voorbeelden van hoe In-Memory OLTP betrokken zijn geweest bij de prestaties aanzienlijk worden verbeterd:

- Met behulp van In-Memory OLTP, [Quorum zakelijke oplossingen kon op het dubbele van de werkbelasting bij het verbeteren van dtu's met 70%](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).
    - DTU betekent *database transaction Units*, en omvat een meting van het gebruik van resources.
- De volgende video ziet u aanzienlijke verbetering van het resourceverbruik met de voorbeeldwerkbelasting van een: [In-Memory OLTP in Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).
    - Zie voor meer informatie het blogbericht: [In-Memory OLTP in Azure SQL Database-blogbericht](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

> [!NOTE]  
>  
>  In-Memory technologieën zijn beschikbaar in Premium en bedrijfskritiek laag Azure SQL-databases en elastische Premium-pools.

De volgende video wordt uitgelegd mogelijke prestatieverbeteringen met In-Memory-technologieën in Azure SQL Database. Houd er rekening mee dat de prestatieverbetering te bereiken die u altijd ziet afhankelijk is van meerdere factoren, met inbegrip van de aard van de werkbelasting en de gegevens, toegangspatroon van de database, enzovoort.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

In dit artikel wordt beschreven aspecten van In-Memory OLTP en columnstore-indexen die specifiek voor Azure SQL Database zijn en bevat ook voorbeelden:
- Op de opslag en gegevensbeheer groottelimieten ziet u de impact van deze technologieën.
- Hier ziet u hoe u voor het beheren van het verplaatsen van databases die gebruikmaken van deze technologieën tussen de verschillende Prijscategorieën.
- Hier ziet u twee voorbeelden die laten zien van het gebruik van In-Memory OLTP, evenals de columnstore-indexen in Azure SQL Database.

Zie voor meer informatie:
- [Overzicht van in-Memory OLTP en gebruiksscenario's](https://msdn.microsoft.com/library/mt774593.aspx) (met inbegrip van verwijzingen naar casestudy's en informatie aan de slag)
- [Documentatie voor In-Memory OLTP](https://msdn.microsoft.com/library/dn133186.aspx)
- [Gids Columnstore-indexen](https://msdn.microsoft.com/library/gg492088.aspx)
- Hybride transactionele/analytische verwerking (HTAP), ook wel bekend als [real-time operational analytics](https://msdn.microsoft.com/library/dn817827.aspx)

## <a name="in-memory-oltp"></a>In-memory OLTP

In-memory OLTP-technologie biedt zeer snelle bewerkingen voor gegevenstoegang door het behouden van alle gegevens in het geheugen. Gebruikt ook gespecialiseerde indexen, systeemeigen compilatie van query's en vergrendelingsvrije gegevenstoegang om prestaties van de OLTP-werkbelasting te verbeteren. Er zijn twee manieren om uw In-Memory OLTP-gegevens te ordenen:
- **Geoptimaliseerd voor geheugen rowstore** indeling waarin elke rij een afzonderlijke geheugenruimte-object is. Dit is een klassieke In-Memory OLTP-indeling dat is geoptimaliseerd voor high-performance OLTP-workloads. Er zijn twee soorten tabellen geoptimaliseerd voor geheugen die in de indeling van de rowstore geoptimaliseerd voor geheugen kunnen worden gebruikt:
  - *Duurzame tabellen* (SCHEMA_AND_DATA) waar de rijen geplaatst in het geheugen blijven behouden nadat de server opnieuw is opgestart. Dit type tabellen gedraagt zich als een traditioneel rowstore-tabel met de extra voordelen van de optimalisaties in het geheugen.
  - *Niet-duurzame tabellen* (SCEMA_ONLY) waar de rijen worden niet bewaard na opnieuw opstarten. Dit type tabel is ontworpen voor tijdelijke gegevens (bijvoorbeeld vervanging van tijdelijke tabellen) of tabellen waar u nodig hebt om snel gegevens laden voordat u deze naar een permanente tabel verplaatst (dus faseringstabellen genoemd).
- **Geoptimaliseerd voor geheugen columnstore** indeling waarin de gegevens zijn onderverdeeld in kolommen. Deze structuur is ontworpen voor HTAP scenario's waarin u wilt uitvoeren van analytische query's op de dezelfde gegevensstructuur waarop uw OLTP-werkbelasting wordt uitgevoerd.

> [!Note]
> In-Memory OLTP-technologie is ontworpen voor de gegevensstructuren die volledig zich in het geheugen bevinden kunnen. Omdat de In-memory-gegevens kan niet worden ge-offload op schijf, zorg ervoor dat u gebruikmaakt van de database die voldoende geheugen heeft. Zie [en cap voor In-Memory OLTP](#data-size-and-storage-cap-for-in-memory-oltp) voor meer informatie.

Een snelle introductie op de In-Memory OLTP: [Snelstartgids 1: In-Memory OLTP-technologieën voor snellere prestaties met T-SQL](https://msdn.microsoft.com/library/mt694156.aspx) (een ander artikel om u te helpen aan de slag)

Uitgebreide video's over de technologieën:

- [In-Memory OLTP in Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (bevat een demo van prestatievoordelen en stappen voor het reproduceren van deze resultaten zelf)
- [Video's in-Memory OLTP: Wat dat inhoudt en ze wanneer/hoe te gebruiken](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)

Er is een programmatische manier om te begrijpen of In-Memory OLTP ondersteuning biedt voor een bepaalde database. U kunt de volgende Transact-SQL-query uitvoeren:
```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```
Als de query retourneert **1**, In-Memory OLTP wordt ondersteund in deze database. De volgende query's identificeren van alle objecten die worden verwijderd moeten voordat u een database kan worden verlaagd naar Standard/Basic:
```
SELECT * FROM sys.tables WHERE is_memory_optimized=1
SELECT * FROM sys.table_types WHERE is_memory_optimized=1
SELECT * FROM sys.sql_modules WHERE uses_native_compilation=1
```

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>Grootte en de opslag cap voor In-Memory OLTP

In-Memory OLTP bevat tabellen geoptimaliseerd voor geheugen, die worden gebruikt voor het opslaan van gebruikersgegevens. Deze tabellen zijn vereist om in het geheugen te passen. Omdat u geheugen rechtstreeks in de service SQL Database beheert, hebben we het concept van een quotum voor gebruikersgegevens. Dit idee wordt aangeduid als *In-Memory OLTP-opslag*.

Elke ondersteunde één database prijscategorie en elke elastische pool prijscategorie voorziet in een bepaalde hoeveelheid In-Memory OLTP-opslag. Zie [DTU gebaseerde resourcelimieten - individuele database](sql-database-dtu-resource-limits-single-databases.md), [DTU gebaseerde resourcelimieten - elastische pools](sql-database-dtu-resource-limits-elastic-pools.md),[vCore gebaseerde resourcelimieten - individuele databases](sql-database-vcore-resource-limits-single-databases.md) en [vCore gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md).

De volgende items tellen mee voor uw opslaglimiet In-Memory OLTP:

- Rijen van de actieve gebruiker gegevens in tabellen geoptimaliseerd voor geheugen en tabelvariabelen. Houd er rekening mee dat oude versies van de rij niet mee voor de limiet tellen.
- Indexen op tabellen geoptimaliseerd voor geheugen.
- Operationele overhead van ALTER TABLE-bewerkingen.

Als u de limiet hebt bereikt, ontvangt u een out-van-quotumfout en bent u niet langer meer invoegen of bijwerken van gegevens. Als u wilt deze fout verhelpen, gegevens verwijderen of vergroot de prijscategorie van de database of de groep van toepassingen.

Zie voor meer informatie over het gebruik van de In-Memory OLTP-opslag controleren en meldingen configureren wanneer u de limiet bijna bereikt [In-Memory bewaken opslag](sql-database-in-memory-oltp-monitoring.md).

#### <a name="about-elastic-pools"></a>Over elastische pools

Met elastische pools, is de In-Memory OLTP-opslag verdeeld over alle databases in de groep. Daarom kan het gebruik in de ene database mogelijk andere databases beïnvloeden. Er zijn twee oplossingen voor deze:

- Configureer een `Max-eDTU` of `MaxvCore` voor databases die lager is dan het aantal edtu's of vCore voor de groep als geheel. Dit maximum caps het gebruik van de In-Memory OLTP-opslag in elke database in de pool, de grootte die overeenkomt met het aantal eDTU's.
- Configureer een `Min-eDTU` of `MinvCore` groter is dan 0. Dit minimum zorgt ervoor dat elke database in de pool is de hoeveelheid beschikbare In-Memory OLTP-opslag die overeenkomt met de geconfigureerde `Min-eDTU` of `vCore`.

### <a name="changing-service-tiers-of-databases-that-use-in-memory-oltp-technologies"></a>Service-lagen van databases die gebruikmaken van technologieën voor In-Memory OLTP wijzigen

U kunt altijd de database of het exemplaar naar een hogere laag, zoals van upgraden algemeen bedrijfskritiek (of standaard naar Premium). De beschikbare functionaliteit en resources alleen verhogen.

Maar downgraden van de laag kan dit een negatieve invloed hebben op uw database. De impact is met name duidelijk wanneer u een downgrade van bedrijfskritiek naar algemeen gebruik (of van Premium naar Standard- of Basic uitvoeren) als uw database In-Memory OLTP-objecten bevat. Tabellen geoptimaliseerd voor geheugen zijn niet beschikbaar na de downgrade (zelfs als ze zichtbaar blijven). Dezelfde overwegingen van toepassing wanneer u de prijscategorie van een elastische pool te verlagen of verplaatsen van een database met In-Memory-technologieën in een Standard- of Basic elastische pool.

> [!Important]
> In-Memory OLTP wordt niet in het algemeen gebruik-, Standard- of Basic-laag ondersteund. Daarom is het niet mogelijk om te verplaatsen van een database waarvoor geen In-Memory OLTP-objecten naar de Standard- of Basic-laag.

Voordat u de database naar Standard/Basic downgraden, verwijdert u alle tabellen geoptimaliseerd voor geheugen en tabeltypen, evenals alle systeemeigen, gecompileerde T-SQL-modules. 

*Omlaag schalen van resources in de laag bedrijfskritiek*: Gegevens in tabellen geoptimaliseerd voor geheugen moeten passen binnen de In-Memory OLTP-opslag die is gekoppeld aan de laag van de database of het beheerde exemplaar of deze beschikbaar is in de elastische pool. Als u probeert te omlaag schalen de laag of de database verplaatsen naar een groep die beschikt niet over voldoende beschikbare In-Memory OLTP-opslag, de bewerking is mislukt.

## <a name="in-memory-columnstore"></a>In-memory columnstore

U kunt opslaan en query uitvoeren op een grote hoeveelheid gegevens in de tabellen is het inschakelen van in-memory columnstore-technologie. Columnstore-technologie maakt gebruik van gegevens op basis van een kolom opslagindeling en batch queryverwerking voor het bereiken van maximaal 10 keer de prestaties van query's in de OLAP-workloads krijgen dan bij traditionele rij-georiënteerde opslag. U kunt ook winsten maximaal 10 keer de compressie van de gegevens over de grootte van de niet-gecomprimeerde gegevens bereiken.
Er zijn twee soorten columnstore-modellen die u gebruiken kunt om uw gegevens te ordenen:
- **Geclusterde columnstore** waarbij alle gegevens in de tabel wordt georganiseerd in de indeling in kolomvorm. In dit model worden in kolomvorm indeling die zeer de gegevens worden gecomprimeerd en kunt u snelle, analytische query's en rapporten uitvoeren voor de tabel alle rijen in de tabel geplaatst. De grootte van uw gegevens mogelijk, afhankelijk van de aard van uw gegevens, verlaagde 10 x-100 x. Geclusterde columnstore-model kunt ook snelle opname van grote hoeveelheden gegevens (bulk-load) sinds grote hoeveelheden gegevens groter is dan 100K rijen worden gecomprimeerd voordat ze worden opgeslagen op schijf. Dit model is een goede keuze voor de scenario's met klassieke datawarehouse. 
- **Niet-geclusterde columnstore** waarin de gegevens worden opgeslagen in traditionele rowstore-tabel en er is een index in de columnstore-indeling die wordt gebruikt voor de analytische query's. Dit model kunt hybride transactionele analytische verwerking (HTAP): de mogelijkheid om te krachtige realtime analyses uitvoeren op een transactiewerkvolume. OLTP-query's worden uitgevoerd op rowstore-tabel die is geoptimaliseerd voor toegang tot een klein aantal rijen, terwijl de OLAP-query's worden uitgevoerd op de columnstore-index die betere keuze voor scans en analyses. Optimalisatie van Azure SQL Database-Query wordt dynamisch gekozen rowstore- of columnstore-indeling op basis van de query. Niet-geclusterde columnstore-indexen niet de grootte van de gegevens niet verkleinen omdat de oorspronkelijke gegevensset wordt bewaard in de oorspronkelijke rowstore-tabel zonder wijziging. De grootte van extra columnstore-index moet echter wel in orde van grootte kleiner is dan de equivalente B-tree-index.

> [!Note]
> In-memory columnstore-technologie blijven alleen de gegevens die nodig voor verwerking in het geheugen, is terwijl de gegevens die niet in het geheugen geschikt wordt opgeslagen op schijf. De hoeveelheid gegevens in de In-memory columnstore structuren kan daarom langer zijn dan de hoeveelheid beschikbaar geheugen. 

Uitgebreide video over de technologie:
- [Columnstore-Index: Video's in-Memory analyse van Ignite 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

### <a name="data-size-and-storage-for-columnstore-indexes"></a>Gegevensgrootte van en opslag voor columnstore-indexen

Columnstore-indexen zijn niet vereist om aan te passen in het geheugen. De enige bovengrens voor de grootte van de indexen is daarom maximale totale grootte van de database die wordt beschreven in de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) artikelen.

Wanneer u geclusterde columnstore-indexen, wordt voor de opslag van de basistabel in kolomvorm compressie gebruikt. Deze compressie kan het opslagverbruik van uw gebruikersgegevens, wat betekent dat u meer gegevens in de database passen aanzienlijk verminderen. En de compressie kan verder worden verhoogd met [in kolomvorm archivering compressie](https://msdn.microsoft.com/library/cc280449.aspx#using-columnstore-and-columnstore-archive-compression). De hoeveelheid compressie dat kan worden behaald, is afhankelijk van de aard van de gegevens, maar 10 keer de compressie is niet ongewoon.

Als u een database met een maximale grootte van 1 terabyte (TB) en u 10 keer de compressie bereiken met behulp van de columnstore-indexen, kunt u bijvoorbeeld een totaal van 10 TB aan gebruikersgegevens aanpassen in de database.

Wanneer u niet-geclusterde columnstore-indexen, wordt de basistabel nog steeds opgeslagen in de traditionele rowstore-indeling. De besparing in opslag zijn daarom niet even groot zijn als met geclusterde columnstore-indexen. Als u een aantal niet-geclusterde indexen traditionele met een enkele columnstore-index vervangt, kunt u echter nog steeds een algehele besparing in het opslagverbruik voor de tabel zien.

### <a name="changing-service-tiers-of-databases-containing-columnstore-indexes"></a>Wijzigen van de service-lagen van databases met Columnstore-indexen

*Het downgraden van één Database naar Basic of Standard* wellicht niet mogelijk als de doel-laag lager dan S3 is. Columnstore-indexen worden ondersteund, alleen op de Business-kritische/Premium-prijscategorie en op de Standard S3-laag en hoger en niet op de Basic-laag. Wanneer u uw database naar een niet-ondersteunde servicelaag of het niveau downgraden, wordt de columnstore-index niet beschikbaar. Het systeem onderhoudt de columnstore-index, maar deze nooit maakt gebruik van de index. Als u later naar een ondersteunde servicelaag of het niveau upgraden, kan de columnstore-index onmiddellijk opnieuw worden gebruikt.

Als u hebt een **geclusterde** columnstore-index, de hele tabel niet meer beschikbaar is na de downgrade. Daarom raden wij aan dat u alle neerzetten *geclusterde* columnstore-indexen voordat u uw database naar een niet-ondersteunde servicelaag of het niveau downgraden.

> [!Note]
> Beheerd exemplaar ondersteunt Columstore indexen in alle lagen.

<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

## <a name="next-steps"></a>Volgende stappen

- [Snelstartgids 1: In-Memory OLTP-technologieën voor snellere prestaties met T-SQL](https://msdn.microsoft.com/library/mt694156.aspx)

- [Gebruik In-Memory OLTP in een bestaande Azure SQL-toepassing](sql-database-in-memory-oltp-migration.md)

- [Monitor In-Memory OLTP-opslag](sql-database-in-memory-oltp-monitoring.md) voor In-Memory OLTP

- [Probeer het In-memory-functies in Azure SQL Database](sql-database-in-memory-sample.md)

## <a name="additional-resources"></a>Aanvullende resources

#### <a name="deeper-information"></a>Meer informatie

- [Informatie over hoe Quorum verdubbelt de belangrijkste databaseworkload terwijl u tegelijkertijd DTU terugdringt met 70% met In-Memory OLTP in SQL-Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [In-Memory OLTP in Azure SQL Database-blogbericht](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [Meer informatie over In-Memory OLTP](https://msdn.microsoft.com/library/dn133186.aspx)

- [Meer informatie over de columnstore-indexen](https://msdn.microsoft.com/library/gg492088.aspx)

- [Meer informatie over real-time operational analytics](https://msdn.microsoft.com/library/dn817827.aspx)

- Zie [algemene patronen van de werkbelasting en overwegingen bij migraties](https://msdn.microsoft.com/library/dn673538.aspx) (welke optie beschrijft werkbelasting patronen waar In-Memory OLTP vaak aanzienlijke prestatievoordelen biedt)

#### <a name="application-design"></a>Het ontwerp van toepassing

- [In-Memory OLTP (optimalisatie In het geheugen)](https://msdn.microsoft.com/library/dn133186.aspx)

- [Gebruik In-Memory OLTP in een bestaande Azure SQL-toepassing](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Hulpprogramma's

- [Azure Portal](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)

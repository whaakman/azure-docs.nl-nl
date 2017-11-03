---
title: Overzicht van Azure SQL Database elastische query | Microsoft Docs
description: Elastische query kunt u een Transact-SQL-query die meerdere databases omvat uit te voeren.
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: a8bf0e2c-bc74-44d0-9b1e-bcc9a6aa2e33
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: mlandzic
ms.openlocfilehash: 6389702b1be5e52c7191e6e57d17b48289e800b2
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="azure-sql-database-elastic-query-overview-preview"></a>Overzicht Azure SQL Database elastische query (preview)
De functie elastische query (in preview) kunt u een verspreid over meerdere databases in Azure SQL Database Transact-SQL-query uitvoeren. Hiermee kunt u cross-databasequery's voor toegang tot externe tabellen, en verbinding maken met Microsoft en derden hulpprogramma's (Excel, Power BI, Tableau, enz.) om op te vragen over gegevenslagen met meerdere databases. Met deze functie kunt u query's kunnen grote hoeveelheden gegevenslagen in SQL-Database uitbreiden en de resultaten weergeven in business intelligence (BI)-rapporten.


## <a name="why-use-elastic-queries"></a>Waarom elastische query's gebruiken?

**Azure SQL Database**

Vragen over Azure SQL-databases volledig in T-SQL. Hierdoor kan alleen-lezen query's naar externe databases. Dit biedt een optie voor het huidige lokale SQL Server-klanten voor het migreren van toepassingen met drie en vierkleuren part namen of gekoppelde server aan de SQL-database.

**Beschikbaar in standard-laag**

Elastische query wordt ondersteund op de prestatielaag Standard naast de prestatielaag Premium. Zie het gedeelte over Preview beperkingen hieronder op prestaties beperkingen voor lagere prestatielagen.

**Push naar externe databases**

Elastische query's kunnen nu push-SQL-parameters met de externe databases voor uitvoering.

**Opgeslagen procedure kan worden uitgevoerd**

Uitvoeren van extern opgeslagen procedureaanroepen of externe functies met [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714).

**Flexibiliteit**

Externe tabellen met elastische query kunnen nu verwijzen naar externe tabellen met een ander schema of de tabelnaam.

## <a name="elastic-query-scenarios"></a>Elastische query scenario 's

Het doel is het opvragen scenario's waarbij meerdere databases rijen in een enkel algehele resultaat bijdragen mogelijk te maken. De query kan ofwel worden samengesteld door de gebruiker of toepassing direct of indirect via hulpprogramma's die zijn verbonden met de database. Dit is vooral nuttig bij het maken van rapporten met commerciële BI of gegevens integratie-hulpprogramma's, of een toepassing die niet kan worden gewijzigd. U kunt met een elastische query opvragen via verschillende databases met behulp van de vertrouwde ervaring van de SQL Server-connectiviteit in hulpprogramma's zoals Excel, Power BI, Tableau of Hiermee.
Een elastische query kan eenvoudig toegang krijgen tot een hele verzameling-databases via query's die zijn uitgegeven door SQL Server Management Studio of Visual Studio en vereenvoudigt tussen meerdere databases opvragen van Entity Framework of andere ORM-omgevingen. Afbeelding 1 toont een scenario waarbij een bestaande cloud toepassing (dat via de [clientbibliotheek voor elastische database](sql-database-elastic-database-client-library.md)) bouwt voort op de gegevens van een uitgebreide laag en een elastische query wordt gebruikt voor het melden van meerdere databases.

**Afbeelding 1** elastische query gebruikt op de uitgebreide gegevenslaag

![Elastische query gebruikt op de uitgebreide gegevenslaag][1]

Klant-scenario's voor de elastische query worden gekenmerkt door de volgende topologieën:

* **Verticale partities - query's voor meerdere databases** (topologie 1): de gegevens verticaal is gepartitioneerd tussen een aantal databases in een gegevenslaag. Normaal gesproken zich verschillende sets van tabellen op verschillende databases. Dat betekent dat het schema van andere databases verschilt. Alle tabellen voor inventarisatie zijn bijvoorbeeld op de ene database terwijl alle accounting-gerelateerde tabellen op een tweede database zijn. Algemene gebruiksvoorbeelden met deze topologie vereisen een query op of voor het compileren van rapporten over tabellen in verschillende databases.
* **Horizontale partitionering - Sharding** (topologie 2): de gegevens zijn gepartitioneerd horizontaal over te verdelen rijen een uitgebreid uit gegevens die laag. Met deze methode is het schema identiek zijn op alle deelnemende databases. Deze aanpak wordt ook 'sharding' genoemd. Sharding kan worden uitgevoerd en beheerd met behulp van (1) de elastische database hulpprogramma's voor bibliotheken of (2) self-sharding. Een elastische query wordt gebruikt voor een query uitvoert of het compileren van rapporten over veel shards.

> [!NOTE]
> Elastische query werkt het beste voor incidentele reporting scenario's waar de meeste van de verwerking kan worden uitgevoerd op de gegevenslaag. Voor zware reporting werklasten of datawarehousescenario's met meer complexe query's ook overwegen [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
>  

## <a name="vertical-partitioning---cross-database-queries"></a>Verticale partities - query's met meerdere databases

Als u wilt coderen, Zie [aan de slag met cross-databasequery (verticale partities)](sql-database-elastic-query-getting-started-vertical.md).

Een elastische query kan worden gebruikt om gegevens die zich in een SQL-database beschikbaar is voor andere SQL-databases te maken. Hiermee kunt query's uit een database om te verwijzen naar tabellen in een andere externe SQL-database. De eerste stap is voor het definiëren van een externe gegevensbron voor elke externe database. De externe gegevensbron is gedefinieerd in de lokale database waaruit u wilt toegang te krijgen tot tabellen die zich op de externe database. Er zijn geen wijzigingen nodig op de externe database. Voor typische verticale partities scenario's waarbij verschillende databases verschillende schema's, kunnen elastische query's worden gebruikt voor het implementeren van algemene gebruiksvoorbeelden zoals toegang tot de referentiegegevens en uitvoeren van query's voor meerdere databases.

> [!IMPORTANT]
> U moet beschikken over een externe gegevensbron ALTER machtiging. Deze machtiging is opgenomen in de machtiging ALTER DATABASE. EEN externe gegevensbron ALTER machtigingen nodig om te verwijzen naar de onderliggende gegevensbron.
>

**Referentiegegevens**: de topologie voor gegevensbeheer verwijzing wordt gebruikt. In de onderstaande afbeelding worden twee tabellen (T1 en T2) referentiegegevens worden bewaard op een specifieke database. Met een elastische query, u kunt nu toegang tot tabellen T1 en T2 op afstand uit andere databases, zoals weergegeven in de afbeelding. Topologie gebruik 1 als verwijzingsdimensies kleine of externe query's in de verwijzingstabel zijn hebben selectief predicaten.

**Afbeelding 2** verticale partities - met behulp van referentiegegevens elastische query-query

![Verticale partities - met behulp van referentiegegevens elastische query-query][3]

**Opvragen van meerdere databases**: elastische query inschakelen gebruiksvoorbeelden waarvoor het uitvoeren van query's in verschillende SQL-databases. Afbeelding 3 ziet u vier verschillende databases: CRM, inventaris, HR en producten. Query's uitgevoerd in een van de databases moeten ook toegang tot een of alle de andere databases. Een elastische query kunt u de database voor deze aanvraag configureren door het uitvoeren van een paar eenvoudige DDL-componenten op elk van de vier databases. Toegang tot een externe tabel is na deze eenmalige configuratie net zo eenvoudig als het verwijst naar een lokale tabel van de T-SQL-query's of van uw BI-hulpprogramma's. Deze aanpak wordt aanbevolen als de externe query's niet veel resultaten retourneren.

**Afbeelding 3** verticale partities - elastische query-query met via verschillende databases

![Verticale partities - elastische query-query met via verschillende databases][4]

De volgende stappen configureren elastische databasequery's voor verticale partities scenario's waarvoor toegang tot een tabel die zich op de externe SQL-databases met hetzelfde schema:

* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* [CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource van het type **RDBMS**
* [CREATE/DROP externe tabel](https://msdn.microsoft.com/library/dn935021.aspx) mytable

U kunt toegang tot externe tabel "MijnTabel" alsof het een lokale tabel na het uitvoeren van de DDL-instructies. Azure SQL Database automatisch een verbinding met de externe database wordt geopend, verwerkt uw aanvraag op de externe database en de resultaten geretourneerd.

## <a name="horizontal-partitioning---sharding"></a>Horizontale partitioneren - sharding
Met elastische query voor het uitvoeren van rapportagetaken via een shard, dat wil zeggen, horizontaal is gepartitioneerd, gegevenslaag vereist een [elastische database shard-toewijzing](sql-database-elastic-scale-shard-map-management.md) vertegenwoordigt de databases van de gegevenslaag. Normaal gesproken alleen een enkele shard-toewijzing in dit scenario wordt gebruikt en een specifieke database met elastische querymogelijkheden (hoofdknooppunt) fungeert als het toegangspunt voor het melden van query's. Alleen deze specifieke database moet toegang tot de shard-toewijzing. Afbeelding 4 ziet u deze topologie en de configuratie met de query voor elastische database en de shard-kaart. De databases in de gegevenslaag kunnen zijn van een Azure SQL Database versie of editie. Zie voor meer informatie over de clientbibliotheek voor elastische database en het maken van kaarten shard [Shard kaart management](sql-database-elastic-scale-shard-map-management.md).

**Afbeelding 4** horizontale partitioneren - elastische query gebruiken voor rapportage over de lagen van de gedeelde gegevens

![Horizontale partitioneren - elastische query gebruiken voor rapportage over de lagen van de gedeelde gegevens][5]

> [!NOTE]
> Elastische query uitvoeren op Database (hoofdknooppunt) mag een aparte database of kan dezelfde database die als host fungeert voor de shard-toewijzing. Welke configuratie u kiest, zorg ervoor dat die laag service en de prestaties van de database is hoog genoeg is voor het afhandelen van het verwachte aantal aanmelding/queryaanvragen.

De volgende stappen configureren elastische databasequery's voor horizontale partitionering scenario's waarvoor toegang tot een set van de tabel die zich bevinden op (meestal) verschillende externe SQL-databases:

* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* Maak een [shard-toewijzing](sql-database-elastic-scale-shard-map-management.md) die de gegevenslaag met behulp van de clientbibliotheek voor elastische database.   
* [CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource van het type **SHARD_MAP_MANAGER**
* [CREATE/DROP externe tabel](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Wanneer u deze stappen hebt uitgevoerd, kunt u de horizontaal gepartitioneerde tabel "MijnTabel" openen alsof het een lokale tabel. Azure SQL Database automatisch wordt geopend meerdere parallelle verbindingen met de externe databases waarin de tabellen fysiek zijn opgeslagen, verwerkt de aanvragen van de externe databases en retourneert de resultaten.
Meer informatie over de stappen die nodig zijn voor de horizontale partitionering scenario vindt u [elastische query voor het partitioneren van de horizontale](sql-database-elastic-query-horizontal-partitioning.md).

Als u wilt coderen, Zie [aan de slag met elastische query voor horizontale partitioneren (sharding)](sql-database-elastic-query-getting-started.md).

## <a name="t-sql-querying"></a>T-SQL-query
Als u uw externe gegevensbronnen en uw externe tabellen hebt gedefinieerd, kunt u reguliere SQL Server-verbindingsreeksen verbinding maken met de databases waar u uw externe tabellen gedefinieerd. Vervolgens kunt u uitvoeren T-SQL-instructies over de externe tabellen voor die verbinding met de beperkingen die hieronder worden beschreven. U vindt meer informatie over en voorbeelden van T-SQL-query's in de documentatie-onderwerpen voor [horizontaal partitioneren](sql-database-elastic-query-horizontal-partitioning.md) en [verticale partities](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Connectiviteit voor hulpprogramma 's
Reguliere verbindingsreeksen voor SQL Server kunt u verbinding maken met uw toepassingen en gegevens of BI-integratie-hulpprogramma's voor databases waarvoor externe tabellen. Zorg ervoor dat SQL Server wordt ondersteund als een gegevensbron voor het hulpprogramma. Eenmaal zijn verbonden, verwijzen naar de elastische database en de externe tabellen in de database, net zoals u zou doen met een andere SQL Server-database waarmee u verbinding met het hulpprogramma maakt.

> [!IMPORTANT]
> Verificatie met behulp van Azure Active Directory met elastische query's is momenteel niet ondersteund.
> 
> 

## <a name="cost"></a>Kosten
Elastische query is opgenomen in de kosten van Azure SQL Database-databases. Denk eraan dat waar uw externe databases zich bevindt in een ander datacenter dan het eindpunt elastische query-topologieën worden ondersteund, maar de uitgaande gegevens uit externe databases worden in rekening gebracht regular [Azure tarieven](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Preview-beperkingen
* Uw eerste elastische query uit te voeren kan een paar minuten duren op de prestatielaag Standard. Deze tijd is nodig om het laden van de elastische queryfunctionaliteit; laden van de prestaties worden verbeterd met hogere prestatielagen.
* Scripts van externe gegevensbronnen of externe tabellen van SSMS of SSDT is nog niet ondersteund.
* Importeren/exporteren voor SQL database ondersteunt nog geen externe gegevensbronnen en externe tabellen. Als u gebruiken voor importeren/exporteren wilt, deze objecten verwijderen voordat u exporteert en vervolgens opnieuw maken na het importeren.
* Elastische query ondersteunt momenteel alleen-lezen toegang tot externe tabellen. U kunt alle functies van T-SQL echter gebruiken op de database waarbij de externe tabel is gedefinieerd. Dit kan handig zijn bij, zoals tijdelijke resultaten gebruikt, bijvoorbeeld behouden, selecteert u < column_list > in < local_table > of voor het definiëren van opgeslagen procedures op de elastische database die verwijzen naar externe tabellen zijn.
* Met uitzondering van nvarchar(max), worden LOB-typen niet ondersteund in definities van de externe tabel. Als tijdelijke oplossing kunt u een weergave op de externe database met het type LOB in nvarchar(max) cast maken, de externe tabel via de weergave in plaats van de basistabel definiëren en vervolgens geconverteerd naar het oorspronkelijke LOB-type in uw query's.
* Kolom statistieken over externe tabellen worden momenteel niet ondersteund. Tabellen statistieken worden ondersteund, maar moeten handmatig worden gemaakt.

## <a name="feedback"></a>Feedback
Geef feedback over uw ervaringen met elastische query's met ons op Livefyre hieronder, de MSDN-forums of delen op Stackoverflow. We zijn geïnteresseerd in alle soorten feedback over de service (defecte producten, ruwe randen, functie hiaten).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een verticale partitie zelfstudie [aan de slag met cross-databasequery (verticale partities)](sql-database-elastic-query-getting-started-vertical.md).
* Zie voor de syntaxis en voorbeelden query's voor verticaal gepartitioneerde gegevens [gegevens opvragen verticaal gepartitioneerd)](sql-database-elastic-query-vertical-partitioning.md)
* Zie voor een zelfstudie horizontaal partitioneren (sharding) [aan de slag met elastische query voor horizontale partitioneren (sharding)](sql-database-elastic-query-getting-started.md).
* Zie voor de syntaxis en voorbeelden query's voor horizontaal gepartitioneerde gegevens [gegevens opvragen horizontaal gepartitioneerd)](sql-database-elastic-query-horizontal-partitioning.md)
* Zie [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) voor een opgeslagen procedure die een Transact-SQL-instructie op een enkele externe Azure SQL Database of een set van databases die fungeren als shards in een horizontale partitieschema wordt uitgevoerd.

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->

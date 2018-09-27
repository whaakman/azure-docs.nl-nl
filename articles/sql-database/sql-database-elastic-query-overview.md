---
title: Overzicht van Azure SQL Database elastische query | Microsoft Docs
description: Elastische query's kunt u een Transact-SQL-query die meerdere databases omvat uit te voeren.
services: sql-database
ms.service: sql-database
subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 8a7962866b70ae0ec99b8425a365575fbd4e5913
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47164364"
---
# <a name="azure-sql-database-elastic-query-overview-preview"></a>Azure SQL Database elastische query overzicht (preview)

De functie elastische query's (in preview) kunt u een Transact-SQL-query die meerdere databases in Azure SQL Database omvat uit te voeren. Hiermee kunt u query's die databaseoverschrijdend voor toegang tot externe tabellen, en verbinding maken met Microsoft en hulpprogramma's van derden (Excel, Power BI, Tableau, enzovoort) om op te vragen voor de gegevenslagen met meerdere databases. Met deze functie kunt u uitschalen van query's voor de lagen van grote hoeveelheden gegevens in SQL-Database en de resultaten in business intelligence (BI)-rapporten weergeven.

## <a name="why-use-elastic-queries"></a>Waarom zou ik elastische query's gebruiken

### <a name="azure-sql-database"></a>Azure SQL Database

Query's op Azure SQL-databases volledig in T-SQL. Hiermee voor alleen-lezen query's naar externe databases en biedt een optie voor het huidige on-premises SQL Server-klanten voor het migreren van toepassingen die gebruikmaken van drie en vierkleuren part namen of gekoppelde server aan de SQL-database.

### <a name="available-on-standard-tier"></a>Beschikbaar in standard-laag

Elastische query's wordt ondersteund op de Standard- en Premium-Servicelagen. Zie de sectie over de beperkingen van Preview hieronder op presteert voor lagere service-lagen.

### <a name="push-parameters-to-remote-databases"></a>Push-parameters met externe databases

Elastische query's kunnen nu SQL-parameters pushen met de externe databases voor uitvoering.

### <a name="stored-procedure-execution"></a>Uitvoering van opgeslagen procedure

Aanroepen van externe opgeslagen procedure of externe functies met [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714).

### <a name="flexibility"></a>Flexibiliteit

Externe tabellen met elastische query's kunnen verwijzen naar externe tabellen met een ander schema of de tabelnaam.

## <a name="elastic-query-scenarios"></a>Scenario's voor elastische query 's

Het doel is om een query uitvoeren op scenario's waarbij meerdere databases rijen in één algemene resultaat bijdragen mogelijk te maken. De query kan ofwel worden samengesteld door de gebruiker of toepassing direct of indirect via hulpprogramma's die zijn verbonden met de database. Dit is vooral handig bij het maken van rapporten, met behulp van commerciële BI of gegevens integratiehulpprogramma's, of een toepassing die kan niet worden gewijzigd. Met een elastische query's, kunt u een query uitvoeren voor meerdere databases met behulp van de vertrouwde SQL Server-connectiviteit-ervaring in hulpprogramma's zoals Excel, Power BI, Tableau of Hiermee.
Een elastische query's kunt eenvoudig toegang tot een hele verzameling-databases via query's dat is uitgegeven door SQL Server Management Studio of Visual Studio en vereenvoudigt het uitvoeren van meerdere databases uitvoeren van query's van Entity Framework of andere omgevingen ORM. Afbeelding 1 toont een scenario waarbij een bestaande cloud-toepassing (welke toepassingen de [clientbibliotheek voor elastic database](sql-database-elastic-database-client-library.md)) bouwt voort op de gegevens van een scale-out-laag en een elastische query wordt gebruikt voor het melden van meerdere databases.

**Afbeelding 1** elastische query die wordt gebruikt voor uitgeschaalde gegevenslaag

![Elastische query die wordt gebruikt voor uitgeschaalde gegevenslaag][1]

Klantscenario's voor elastische query's worden gekenmerkt door de volgende topologieën:

* **Verticale partitionering - query's die databaseoverschrijdend** (topologie 1): de gegevens verticaal zijn gepartitioneerd tussen een aantal databases in een gegevenslaag. Normaal gesproken verschillende sets van tabellen bevinden zich op verschillende databases. Dit betekent dat het schema verschilt voor verschillende databases. Alle tabellen voor de voorraad zijn bijvoorbeeld op de ene database terwijl alle accounting-gerelateerde tabellen op een tweede database zijn. Algemene scenario's met deze topologie vereisen een op query's uitvoeren voor of op het samenstellen van rapporten in tabellen in verschillende databases.
* **Horizontale partitionering - Sharding** (topologie 2): gegevens zijn horizontaal gepartitioneerd voor het distribueren van rijen in een uitgeschaalde gegevens laag. Met deze methode is het schema identiek zijn op alle deelnemende databases. Deze aanpak wordt ook 'sharding' genoemd. Sharding kan worden uitgevoerd en beheerd met behulp van (1) de elastische database-hulpmiddelen bibliotheken of (2) self-sharding. Een elastische query wordt gebruikt om een query of rapporten in veel shards tegelijk worden gecompileerd.

> [!NOTE]
> Elastische query's het beste werkt voor het melden van scenario's waar de meeste van de verwerking (filteren, aggregatie) aan de externe bron kan worden uitgevoerd. Het is niet geschikt voor ETL-bewerkingen waarbij grote hoeveelheden gegevens worden overgebracht van de externe database (s). Voor rapportage werkbelastingen of voor datawarehousescenario's met meer complexe query's, ook kunt u overwegen [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
>  

## <a name="vertical-partitioning---cross-database-queries"></a>Verticale partitionering - query's tussen databases

Als u wilt coderen, Zie [aan de slag met query van meerdere databases (verticale partitionering)](sql-database-elastic-query-getting-started-vertical.md).

Een elastische query kan worden gebruikt om gegevens die zich in een SQL-database die beschikbaar zijn voor andere SQL-databases te maken. Hiermee kunt query's van de ene database om te verwijzen naar tabellen in een andere externe SQL-database. De eerste stap is het een externe gegevensbron definiëren voor elke externe database. De externe gegevensbron is gedefinieerd in de lokale database waaruit u wilt toegang te krijgen tot tabellen die zich op de externe database. Er zijn geen wijzigingen nodig op de externe database. Voor typische verticale partitionering scenario's waarbij verschillende databases verschillende schema's hebben, kunnen elastische query's worden gebruikt om algemene scenario's zoals toegang tot de referentiegegevens te implementeren en uitvoeren van query's tussen databases.

> [!IMPORTANT]
> U moet beschikken over machtigingen voor een externe gegevensbron niet wijzigen. Deze machtiging is opgenomen in de machtiging ALTER DATABASE. EEN externe gegevensbron ALTER machtigingen nodig om te verwijzen naar de onderliggende gegevensbron.
>

**Verwijzen naar gegevens**: de topologie wordt gebruikt voor beheer van verwijzing. In de afbeelding hieronder worden twee tabellen (T1 en T2) met referentiegegevens bewaard op een specifieke database. Met behulp van een elastische query's, u kunt nu toegang tot tabellen T1 en T2 op afstand van andere databases, zoals weergegeven in de afbeelding. Gebruik topologie 1 als tabellen met kleine of externe query's in de verwijzingstabel selectief predicaten hebben.

**Afbeelding 2** verticale partitionering: met behulp van referentiegegevens elastische query-query

![Verticale partitionering: met behulp van referentiegegevens elastische query-query][3]

**Uitvoeren van query's tussen databases**: elastische query's inschakelen van use cases die nodig hebt voor het uitvoeren van query's voor verschillende SQL-databases. Afbeelding 3 ziet u vier verschillende databases: CRM, voorraad, HR en producten. Query's uitgevoerd in een van de databases moeten ook toegang tot een of alle andere databases. Met behulp van een elastische query's, kunt u uw database in dit geval door het uitvoeren van een paar eenvoudige DDL-componenten op elk van de vier databases configureren. Toegang tot een externe tabel is na deze eenmalige configuratie, net zo eenvoudig als die verwijst naar een lokale tabel van de T-SQL-query's of van uw BI-hulpprogramma's. Deze aanpak wordt aanbevolen als de externe query's niet veel resultaten retourneren.

**Afbeelding 3** verticale partitionering: gebruik elastische query-query voor verschillende databases

![Verticale partitionering: gebruik elastische query-query voor verschillende databases][4]

De volgende stappen uit configureren elastische databasequery's voor verticale partitionering scenario's waarvoor toegang tot een tabel die zich op de externe SQL-databases met hetzelfde schema:

* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* [CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource van het type **RDBMS**
* [EXTERNE tabel maken/DROP](https://msdn.microsoft.com/library/dn935021.aspx) mytable

U kunt toegang tot de externe tabel "mytable" alsof het een lokale tabel na het uitvoeren van de DDL-componenten. Azure SQL Database automatisch een verbinding met de externe database wordt geopend, verwerkt de aanvraag op de externe database en worden de resultaten geretourneerd.

## <a name="horizontal-partitioning---sharding"></a>Horizontale partitionering - sharding

Met elastische query's voor het uitvoeren van rapportage taken via een shard, dat wil zeggen, horizontaal gepartitioneerd, gegevenslaag vereist een [shard-toewijzing voor elastische database](sql-database-elastic-scale-shard-map-management.md) om weer te geven van de databases van de gegevenslaag. Normaal gesproken alleen een enkele shard-toewijzing wordt gebruikt in dit scenario en een specifieke database met elastische query-mogelijkheden (hoofdknooppunt) fungeert als het toegangspunt voor het melden van query's. Alleen deze specifieke database moet toegang hebben tot de shard-toewijzing. Afbeelding 4 ziet u deze topologie en de configuratie met de query's in elastic database en de shard-kaart. De databases in de gegevenslaag kunnen van een Azure SQL Database versie of editie zijn. Zie voor meer informatie over de clientbibliotheek voor elastic database en het maken van de shard-toewijzingen [Shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md).

**Afbeelding 4** horizontale partitionering: met elastische query's voor rapportage over de lagen van gedeelde gegevens

![Horizontale partitionering: met elastische query's voor rapportage over de lagen van gedeelde gegevens][5]

> [!NOTE]
> Elastische query uitvoeren op Database (hoofdknooppunt) is een afzonderlijke database of kan de dezelfde database die als host fungeert voor de shard-toewijzing.
> Welke configuratie u kiest, zorg ervoor dat serviceniveau en compute van de grootte van de database is hoog genoeg is voor het afhandelen van de verwachte hoeveelheid aanmelding/queryaanvragen.

De volgende stappen uit configureren elastische databasequery's voor horizontale partitionering scenario's waarvoor toegang tot een set met tabellen die zich op (meestal) verschillende externe SQL-databases:

* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* Maak een [shard-toewijzing](sql-database-elastic-scale-shard-map-management.md) die uw gegevenslaag met behulp van de clientbibliotheek van elastische database vertegenwoordigt.
* [CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource van het type **SHARD_MAP_MANAGER**
* [EXTERNE tabel maken/DROP](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Nadat u deze stappen hebt uitgevoerd, kunt u de horizontaal gepartitioneerde tabel 'mytable' toegang tot alsof het een lokale tabel. Azure SQL Database automatisch geopend van meerdere parallelle verbindingen met de externe databases waarin de tabellen fysiek worden opgeslagen, verwerkt de aanvragen voor de externe databases, en worden de resultaten geretourneerd.
Meer informatie over de stappen die nodig zijn voor de horizontale partitionering scenario kunt u vinden in [elastische query's voor horizontale partitionering](sql-database-elastic-query-horizontal-partitioning.md).

Als u wilt coderen, Zie [aan de slag met elastische query's voor horizontale partitionering (sharding)](sql-database-elastic-query-getting-started.md).

## <a name="t-sql-querying"></a>T-SQL-query 's

Nadat u uw externe gegevensbronnen en uw externe tabellen hebt gedefinieerd, kunt u normale SQL Server-verbindingsreeksen verbinding maken met de databases waar u uw externe tabellen hebt gedefinieerd. U kunt vervolgens T-SQL-instructies via uw externe tabellen uitvoeren op die verbinding met de beperkingen die hieronder worden beschreven. U vindt meer informatie over en voorbeelden van T-SQL-query's in de documentatie-onderwerpen voor [horizontale partitionering](sql-database-elastic-query-horizontal-partitioning.md) en [verticale partitionering](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Connectiviteit voor hulpprogramma 's

Verbindingsreeksen voor normale SQL Server kunt u verbinding maken met uw toepassingen en hulpprogramma's voor BI of gegevens integratie met databases die u externe tabellen hebt. Zorg ervoor dat SQL Server wordt ondersteund als een gegevensbron voor het hulpprogramma. Eenmaal verbinding hebben, raadpleegt u de query's in elastic database en de externe tabellen in de database net zoals u zou doen met een andere SQL Server-database waarmee u verbinding met uw hulpprogramma maken.

> [!IMPORTANT]
> Verificatie met behulp van Azure Active Directory met elastische query's is momenteel niet ondersteund.

## <a name="cost"></a>Kosten

Elastische query's is opgenomen in de kosten van Azure SQL Database-databases. Houd er rekening mee dat topologieën waar uw externe databases zich in een ander datacenter dan het eindpunt van de elastische query's bevinden worden ondersteund, maar de uitgaande gegevens uit externe databases wordt in rekening gebracht regelmatig [Azure-tarieven](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Beperkingen voor Preview

* Uw eerste elastische query uit te voeren met een paar minuten op het serviceniveau Standard kan duren. Deze tijd is noodzakelijk om te laden van de functionaliteit van elastische query's; het laden van de prestaties worden verbeterd met hogere service-lagen en compute-grootten.
* Scripts van externe gegevensbronnen of externe tabellen van SSMS of SSDT is nog niet ondersteund.
* Import/Export voor SQL DB ondersteunt nog geen externe gegevensbronnen en externe tabellen. Als u gebruiken voor importeren/exporteren wilt, deze objecten verwijderen voordat u exporteert en vervolgens opnieuw maken na het importeren.
* Elastische query's ondersteunt momenteel alleen alleen-lezen toegang tot externe tabellen. U kunt volledige T-SQL-functionaliteit echter gebruiken voor de database waarin de externe tabel is gedefinieerd. Dit kan handig zijn bij, zoals tijdelijke resultaten met, bijvoorbeeld voordoen, < column_list > in < local_table > Selecteer of voor het definiëren van opgeslagen procedures op de database elastische query's die verwijzen naar externe tabellen zijn.
* Met uitzondering van nvarchar(max), worden LOB-typen niet ondersteund in de definities van de externe tabel. Als tijdelijke oplossing, kunt u een weergave op de externe database die het type LOB in nvarchar(max) geworpen maken, uw externe tabel over de weergave in plaats van de basistabel definiëren en vervolgens geconverteerd naar het oorspronkelijke LOB-type in uw query's.
* Kolommen van het gegevenstype nvarchar(max) in resultaat set uitschakelen geavanceerde batchverwerking technics gebruikt in een elastische Query-implementatie en kunnen invloed hebben op prestaties van query voor een orde van grootte, of zelfs twee orde van grootte in niet-standaard use-cases waar grote hoeveelheid niet-samengevoegde gegevens worden overgebracht als gevolg van de query.
* Kolomstatistieken over externe tabellen worden momenteel niet ondersteund. Tabelstatistieken worden ondersteund, maar moeten handmatig worden gemaakt.

## <a name="feedback"></a>Feedback

Feedback geven over uw ervaring met elastische query's met ons hieronder, op de MSDN-forums of op Stackoverflow. We zijn geïnteresseerd in allerlei soorten feedback over de service (defecten, ruwe randen, functiehiaten).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een verticale partitionering zelfstudie [aan de slag met query van meerdere databases (verticale partitionering)](sql-database-elastic-query-getting-started-vertical.md).
* Zie voor een voorbeeld en de syntaxis van query's voor verticaal gepartitioneerde gegevens [opvragen van verticaal gepartitioneerde gegevens)](sql-database-elastic-query-vertical-partitioning.md)
* Zie voor een zelfstudie horizontale partitionering (sharding) [aan de slag met elastische query's voor horizontale partitionering (sharding)](sql-database-elastic-query-getting-started.md).
* Zie voor een voorbeeld en de syntaxis van query's voor horizontaal gepartitioneerde gegevens [gegevens opvragen horizontaal gepartitioneerd)](sql-database-elastic-query-horizontal-partitioning.md)
* Zie [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) voor een opgeslagen procedure die een Transact-SQL-instructie op een één externe SQL-Database van Azure of een set van databases die fungeren als shards in een horizontale partitionering wordt uitgevoerd.

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->

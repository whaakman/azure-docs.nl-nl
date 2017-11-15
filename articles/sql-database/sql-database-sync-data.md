---
title: Synchroniseren van Azure SQL-gegevens (Preview) | Microsoft Docs
description: Dit overzicht introduceert Azure SQL-gegevenssynchronisatie (Preview)
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: fe11926cb7f6b2a80913895b685acfcc433e9805
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync-preview"></a>Synchronisatie van gegevens over meerdere cloud en on-premises databases met SQL synchroniseren van gegevens (Preview)

Synchroniseren van de SQL-gegevens is een service die is gebouwd op Azure SQL Database waarmee u de gegevens die u twee richtingen op meerdere SQL-databases en exemplaren van SQL Server selecteert synchroniseren.

Synchroniseren van gegevens is gebaseerd op het concept van een groep voor synchronisatie. Een groep voor synchronisatie is een groep met databases die u wilt synchroniseren.

Een groep voor synchronisatie heeft de volgende eigenschappen:

-   De **synchronisatieschema** wordt beschreven welke gegevens worden gesynchroniseerd.

-   De **Sync richting** kan in twee richtingen of kan stromen slechts in één richting. Dat wil zeggen, de richting van de synchronisatie kan worden *Hub lid* of *lid op Hub*, of beide.

-   De **synchronisatie-Interval** hoe vaak synchronisatie plaatsvindt.

-   De **Conflict resolutie beleid** is een niveau Groepsbeleid, wat kan *Hub wins* of *lid wins*.

Een hub en spoke-topologie gegevenssynchronisatie gebruikt om gegevens te synchroniseren. U definieert een van de databases in de groep als de Database van de Hub. De rest van de databases zijn lid databases. Synchronisatie doet zich alleen tussen de Hub en de afzonderlijke leden.
-   De **Hub Database** moet een Azure SQL Database.
-   De **lid databases** SQL-Databases, de lokale SQL Server-databases of de SQL Server-exemplaren op virtuele machines in Azure.
-   De **synchronisatiedatabase** bevat de metagegevens en het logboek voor synchroniseren van gegevens. De synchronisatiedatabase is dat een Azure SQL Database zich in dezelfde regio bevinden als de Database van de Hub. De synchronisatiedatabase is klant gemaakt en die eigendom zijn van de klant.

> [!NOTE]
> Als u een on-premises-database gebruikt, hebt u [configureren van een lokale agent](sql-database-get-started-sql-data-sync.md#add-on-prem).

![Gegevens tussen databases synchroniseren](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-to-use-data-sync"></a>Wanneer met het synchroniseren van gegevens

Synchroniseren van gegevens is handig in gevallen waarbij gegevens moet worden bewaard up-to-date te houden in verschillende Azure SQL-Databases of SQL Server-databases. Hier volgen de belangrijkste gebruiksvoorbeelden voor synchroniseren van gegevens:

-   **Hybride gegevenssynchronisatie:** met het synchroniseren van gegevens, kun je gegevens worden gesynchroniseerd tussen uw on-premises-databases en de Azure SQL-Databases hybride toepassingen inschakelen. Deze mogelijkheid kan beroep instellen voor klanten aan wie u van plan bent te verplaatsen naar de cloud en wilt enkele van de toepassing in Azure te plaatsen.

-   **Gedistribueerde toepassingen:** In veel gevallen is het nuttig om andere werkbelastingen scheiden over verschillende databases. Bijvoorbeeld als u een grote productiedatabase hebben, maar u moet ook een rapport of analytics werkbelasting uitvoeren op deze gegevens, is het handig om een tweede database voor deze extra belasting. Hierdoor minimaliseert de prestatie-invloed van de werkbelasting van uw productieomgeving. Synchroniseren van gegevens kunt u deze twee databases gesynchroniseerd houden.

-   **Globaal gedistribueerde toepassingen:** veel bedrijven verschillende regio's en zelfs meerdere landen omvatten. Om te beperken netwerklatentie, is het raadzaam om uw gegevens in een regio dicht bij u. U kunt databases eenvoudig in regio's over de hele wereld gesynchroniseerd houden met het synchroniseren van gegevens.

Synchroniseren van gegevens is niet geschikt is voor de volgende scenario's:

-   Herstel na noodgevallen

-   Lezen van schaal

-   ETL (OLTP met OLAP)

-   Migratie van on-premises SQL Server naar Azure SQL Database

## <a name="how-does-data-sync-work"></a>Hoe werkt synchroniseren van gegevens? 

-   **Bijhouden van gegevenswijzigingen:** synchroniseren van gegevens houdt wijzigingen met behulp van invoegen, bijwerken en verwijderen van triggers. De wijzigingen worden opgenomen in een tabel aan de in de database.

-   **Synchroniseren van gegevens:** synchroniseren van gegevens in een Hub en Spoke-model is ontworpen. De Hub wordt afzonderlijk gesynchroniseerd met elk lid. Wijzigingen van de Hub zijn gedownload naar het lid en vervolgens de wijzigingen van het lid worden geüpload naar de Hub.

-   **Het oplossen van conflicten:** synchroniseren van gegevens biedt twee opties voor conflictoplossing, *Hub wins* of *lid wins*.
    -   Als u selecteert *Hub wins*, de wijzigingen in de hub altijd overschreven, wijzigingen in het lid.
    -   Als u selecteert *lid wins*, de wijzigingen in de wijzigingen voor het overschrijven van lid in de hub. Als er meer dan één lid, afhankelijk van de uiteindelijke waarde waarvoor de eerste wordt gesynchroniseerd.

## <a name="common-questions"></a>Veelgestelde vragen

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Hoe vaak kunt synchroniseren van gegevens mijn gegevens Synchroniseer? 
De minimale frequentie is om de vijf minuten.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Kan ik synchroniseren van gegevens te synchroniseren tussen alleen lokale-databases in SQL Server gebruiken? 
Niet rechtstreeks. U kunt synchroniseren tussen de SQL Server-databases voor lokale indirect echter door het maken van een Hub-database in Azure en vervolgens de lokale databases toe te voegen aan de groep voor synchronisatie.
   
### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-keep-them-synchronized"></a>Kan ik synchroniseren van gegevens voor seedgegevens van mijn productiedatabase in een lege database gebruiken, en vervolgens deze gesynchroniseerd houden? 
Ja. Het schema handmatig maken in de nieuwe database door het uitvoeren van scripts in het oorspronkelijke. Nadat u het schema maakt, moet u de tabellen toevoegen aan een groep voor synchronisatie met de gegevens kopiëren en het gesynchroniseerd houden.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Waarom zie ik tabellen die ik niet hebt gemaakt?  
Synchroniseren van gegevens maakt aan tabellen in de database voor het bijhouden. Deze niet verwijderen of werkt niet meer synchroniseren van gegevens.
   
### <a name="i-got-an-error-message-that-said-cannot-insert-the-value-null-into-the-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-the-error"></a>Ik heb een foutbericht weergegeven dat gezegd ' kan de waarde NULL niet invoegen in de kolom \<kolom\>. Kolom mag geen null-waarden." Wat betekent dit en hoe kan ik de fout oplossen? 
Dit foutbericht geeft aan dat een van de twee volgende problemen:
1.  Mogelijk zijn er geen tabel zonder een primaire sleutel. U kunt dit probleem oplossen door moet u een primaire sleutel toevoegen aan alle tabellen die u wilt synchroniseren.
2.  Mogelijk zijn er een WHERE-component in de instructie CREATE INDEX. Deze voorwaarde verwerkt synchronisatie niet. U kunt dit probleem oplossen door de component WHERE verwijderen of wijzigingen handmatig aanbrengen voor alle databases. 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-the-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Hoe wordt gegevenssynchronisatie kringverwijzingen verwerkt? Dat wil zeggen, wanneer dezelfde gegevens in meerdere synchronisatiegroepen is gesynchroniseerd en als gevolg hiervan blijft wijzigen?
Synchroniseren van gegevens kunnen kringverwijzingen niet worden verwerkt. Zorg ervoor dat ze kunnen worden vermeden. 

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Hoe kan ik exporteren en importeren van een database met het synchroniseren van gegevens?
Na het exporteren van een database als een `.bacpac` bestand en importeer het bestand om een nieuwe database te maken, moet u de volgende twee taken uitvoeren om te synchroniseren van gegevens in de nieuwe database te gebruiken:
1.  Opschonen van de objecten synchroniseren van gegevens en de tabellen op de **nieuwe database** met behulp van [dit script](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/clean_up_data_sync_objects.sql). Dit script worden alle vereiste objecten synchroniseren van gegevens uit de database verwijderd.
2.  Maak de groep voor synchronisatie met de nieuwe database opnieuw. Als u niet langer de oude groep voor synchronisatie, verwijderen.

## <a name="sync-req-lim"></a>Vereisten en beperkingen

### <a name="general-requirements"></a>Algemene vereisten

-   Elke tabel moet een primaire sleutel hebben. De waarde van de primaire sleutel in elke rij niet te wijzigen. Als u hebt om dit te doen, verwijdert u de rij en maak deze opnieuw met de nieuwe waarde voor de primaire sleutel. 

-   Een tabel kan niet een identiteitskolom die niet de primaire sleutel hebben.

-   De namen van objecten (databases, tabellen en kolommen) kunnen bevatten de afdrukbare tekens punt (.), vierkante linkerhaak ([) of rechts vierkante haak (]).

-   Snapshot-isolatie moet zijn ingeschakeld. Zie voor meer informatie [Snapshot-isolatie in SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="general-considerations"></a>Algemene overwegingen

#### <a name="eventual-consistency"></a>Uiteindelijke consistentie
Omdat synchroniseren van gegevens op basis van een trigger, transactionele consistentie kan niet worden gegarandeerd. Microsoft zorgt ervoor dat alle uiteindelijk wijzigingen en synchroniseren van gegevens veroorzaakt geen gegevens verloren gaan.

#### <a name="performance-impact"></a>Prestatie-invloed
Gegevens synchroniseren gebruikt invoegen, bijwerken en verwijderen van triggers voor het bijhouden van wijzigingen. Tabellen wordt gemaakt in de database voor het bijhouden. Deze wijziging bijhouden activiteiten heeft dit gevolgen voor de werkbelasting van uw database. De servicelaag beoordelen en indien nodig te upgraden.

### <a name="general-limitations"></a>Algemene beperkingen

#### <a name="unsupported-data-types"></a>Niet-ondersteunde gegevenstypen

-   FileStream

-   SQL/CLR UDT

-   XMLSchemaCollection (XML wordt ondersteund)

-   Cursor, Timestamp, Hierarchyid

#### <a name="limitations-on-service-and-database-dimensions"></a>Beperkingen met betrekking tot dimensies service en -database

| **Dimensies**                                                      | **Limiet**              | **Tijdelijke oplossing**              |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| Maximum aantal synchronisatiegroepen die elke database kan deel uitmaken.       | 5                      |                             |
| Maximum aantal eindpunten in een enkel sync-groep              | 30                     | Meerdere synchronisatiegroepen maken |
| Maximum aantal lokale eindpunten in een enkel sync-groep. | 5                      | Meerdere synchronisatiegroepen maken |
| Database-, tabel-, schema-en kolomnamen                       | 50 tekens per naam |                             |
| Tabellen in een groep voor synchronisatie                                          | 500                    | Meerdere synchronisatiegroepen maken |
| Kolommen in een tabel in een groep voor synchronisatie                              | 1000                   |                             |
| Grootte van de rij gegevens in een tabel                                        | 24 mb                  |                             |
| Minimale synchronisatie-interval                                           | 5 minuten              |                             |
|||

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het synchroniseren van de SQL-gegevens:

-   [Synchroniseren van Azure SQL-gegevens instellen](sql-database-get-started-sql-data-sync.md)
-   [Aanbevolen procedures voor het synchroniseren van Azure SQL-gegevens](sql-database-best-practices-data-sync.md)
-   [Monitor Azure SQL-gegevens synchroniseren met OMS Log Analytics](sql-database-sync-monitor-oms.md)
-   [Problemen oplossen met het synchroniseren van Azure SQL-gegevens](sql-database-troubleshoot-data-sync.md)

-   Voer de PowerShell-voorbeelden die laten hoe u zien voor het synchroniseren van de SQL-gegevens configureren:
    -   [PowerShell gebruiken om te synchroniseren tussen meerdere Azure SQL-databases](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL Database en een lokale SQL Server-database.](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [De SQL-gegevens synchroniseren REST-API-documentatie downloaden](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Zie voor meer informatie over SQL-Database:

-   [Overzicht van de SQL-Database](sql-database-technical-overview.md)
-   [Database-levenscyclusbeheer](https://msdn.microsoft.com/library/jj907294.aspx)

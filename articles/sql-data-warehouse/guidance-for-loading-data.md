---
title: Richtlijnen voor gegevens laden - Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen voor het laden van gegevens en het uitvoeren van ELT met Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 7b698cad-b152-4d33-97f5-5155dfa60f79
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 12/13/2017
ms.author: barbkess
ms.openlocfilehash: 8903be1361d1574a5d81b69223f608ccb7a698ea
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="guidance-for-loading-data-into-azure-sql-data-warehouse"></a>Richtlijnen voor het laden van gegevens in Azure SQL Data Warehouse
Aanbevelingen en prestatieoptimalisatie voor het laden van gegevens in Azure SQL Data Warehouse. 

- Zie voor meer informatie over PolyBase en het ontwerpen van een Extract, Load en Transform (ELT)-proces [ELT ontwerpen voor SQL Data Warehouse](design-elt-data-loading.md).
- Gebruik voor het laden van een zelfstudie [PolyBase om gegevens te laden uit Azure blob-opslag naar Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).


## <a name="extract-source-data"></a>Brongegevens extraheren

Bij het exporteren van gegevens naar een ORC-bestandsformaat van SQL Server of Azure SQL Data Warehouse kunnen zware tekstkolommen worden beperkt tot 50 kolommen vanwege fouten door onvoldoende geheugen in java. U kunt dit probleem omzeilen door slechts een subset van de kolommen te exporteren.


## <a name="land-data-to-azure"></a>Gegevens overbrengen naar Azure
PolyBase kan rijen met meer dan 1 miljoen bytes aan gegevens niet laden. Wanneer u gegevens in de tekstbestanden in Azure Blob-opslag of Azure Data Lake Store zet, moeten deze minder dan 1 miljoen bytes aan gegevens bevatten. Dit geldt ongeacht het gedefinieerde tabelschema.

Bepaal uw opslaglaag en uw datawarehouse om de latentie te minimaliseren.

## <a name="data-preparation"></a>Gegevensvoorbereiding

Alle bestandsindelingen hebben verschillende prestatiekenmerken. Gebruik voor het snelste laadproces gecomprimeerde tekstbestanden. Het verschil tussen UTF-8- en UTF-16-prestaties is minimaal.

Splits grote gecomprimeerde bestanden in kleinere gecomprimeerde bestanden.

## <a name="create-designated-loading-users"></a>Aangewezen gebruikers voor het laadproces aanmaken
Als u loads wilt uitvoeren met geschikte rekenresources, maakt u gebruikers voor het laadproces die zijn aangewezen voor het uitvoeren van loads. Wijs elke gebruiker voor het laadproces toe aan een specifieke resourceklasse. Als u een belasting wilt uitvoeren, meldt u zich aan als een van de gebruikers voor het laadproces en voert u de belasting uit. De load wordt uitgevoerd met de resourceklasse van de gebruiker.  Deze methode is eenvoudiger dan de resourceklasse van een gebruiker aanpassen om te voldoen aan de huidige benodigde resourceklasse.

Deze code maakt een gebruiker voor het laadproces voor de resourceklasse staticrc20. Zo wordt toestemming verkregen voor gebruikerscontrole bij een database en de gebruiker wordt vervolgens toegevoegd als lid van de databaserol staticrc20. Meld u aan in als LoaderRC20 en voer de belasting uit om deze uit te voeren met resources voor de statiRC20-resourceklassen. 

    ```sql
    CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
    EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
    ```

Voer loads bij voorkeur uit onder statische en niet onder dynamische resourceklassen. Met de statische resourceklassen worden dezelfde resources gegarandeerd ongeacht het [serviceniveau](performance-tiers.md#service-levels). Als u een dynamische resourceklasse gebruikt, variëren de resources afhankelijk van uw serviceniveau. Voor dynamische klassen betekent een lager serviceniveau dat u waarschijnlijk een grotere resourceklasse moet gebruiken voor uw gebruiker van het laadproces.

### <a name="example-for-isolating-loading-users"></a>Voorbeeld voor het isoleren van gebruikers van het laadproces

Er is vaak behoefte aan meerdere gebruikers die gegevens kunnen laden in een SQL DW. Omdat de [CREATE TABLE AS SELECT (Transact-SQL)] [CREATE TABLE AS SELECT (Transact-SQL)] een machtiging vereist voor beheer van de database, verschijnen meerdere gebruikers met toegangsbeheer via alle schema's. Als u wilt dit beperken, kunt u de instructie DENY CONTROL gebruiken.

Denk bijvoorbeeld aan databaseschema's, schema_A voor afdeling A, en schema_B voor afdeling B. Laat databasegebruikers gebruiker_A en gebruiker_B gebruikers zijn voor PolyBase die respectievelijk laden in afdeling A en B. Beide zijn voorzien van databasemachtigingen voor CONTROL. De makers van schema A en B vergrendelen nu hun schema's met DENY:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   

Gebruiker_A en gebruiker_B worden nu geblokkeerd door het schema van de andere afdeling.


## <a name="load-to-a-staging-table"></a>Laden naar een faseringstabel

Laad in een round_robin, heap faseringstabel voor de snelste laadsnelheid. Dit is de meest efficiënte manier voor het verplaatsen van de gegevens van de Azure Storage-laag naar SQl Data Warehouse.

Schaal uw datawarehouse omhoog als u een grote loadtaak verwacht.

Voer slechts één loadtaak tegelijk uit voor optimale laadprestaties

### <a name="optimize-columnstore-index-loads"></a>Laden van columnstore-index optimaliseren

Columnstore-indexen vereisen een grote hoeveelheid geheugen voor het comprimeren van gegevens tot hoogwaardige rijgroepen. Voor de beste compressie en efficiëntie van de index moet de columnstore-index 1.048.576 rijen in elke rijgroep comprimeren. Dit is het maximum aantal rijen per rijgroep. Bij geheugenbelasting kan het zijn dat de columnstore-index de maximale compressiesnelheden niet kan halen. Dit kan van invloed op de queryresultaten. Zie voor gedetailleerde informatie [Columnstore geheugenoptimalisaties](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

- Zorg dat de gebruiker van het laadproces voldoende geheugen heeft om maximale compressiesnelheden te bereiken. Gebruik hiervoor gebruikers voor het laadproces die lid zijn van een middelgrote of grote resourceklasse. 
- Laad genoeg rijen om nieuwe rijgroepen volledig te vullen. Bij bulksgewijs laden gaan alle 1.048.576 rijen rechtstreeks naar de columnstore. Loads met minder dan 102.400 rijen verzenden de rijen naar de deltastore, die rijen in een geclusterde index vasthoudt totdat er genoeg zijn voor compressie. Als u te weinig rijen laadt, gaan deze mogelijk allemaal naar de deltastore en worden ze niet direct naar columnstore-indeling gecomprimeerd.


### <a name="handling-loading-failures"></a>Afhandeling van fouten bij het laden

Een load met behulp van een externe tabel kan mislukken met de fout *Query afgebroken--de maximale weigeringsdrempelwaarde is bereikt tijdens het lezen vanuit een externe bron*. Dit geeft aan dat uw externe gegevens *vervuilde* records bevatten. Een record wordt als 'vervuild' beschouwd als de werkelijke gegevenstypen of aantal kolommen niet overeenkomen met de kolomdefinities van de externe tabel of als de gegevens niet overeenkomen met de externe bestandsindeling. 

U kunt dit verhelpen door ervoor te zorgen dat uw externe tabel- en bestandsformaatdefinities correct zijn en dat uw externe gegevens overeenstemmen met deze definities. Als een subset van externe gegevensrecords ongeldig is, kunt u ervoor kiezen deze records voor uw query’s te weigeren door gebruik te maken van de weigeringsopties in CREATE EXTERNAL TABLE DDL.



## <a name="insert-data-into-production-table"></a>Gegevens in productietabel invoegen
Dit zijn aanbevelingen voor het invoegen van rijen in de productietabellen.


### <a name="batch-insert-statements"></a>Batch INSERT-instructies
Een eenmalige load naar een kleine tabel met een [INSERT-instructie](/sql/t-sql/statements/insert-transact-sql.md) of zelfs een periodieke reload van een resultaat kan prima aan uw vereisten voldoen met een instructie zoals `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Eén ton invoegen is niet zo efficiënt als bulksgewijs laden. 

Als u de hele dag door duizenden of meerdere enkele gegevens wilt invoeren, voeg de gegevens dan samen tot een batch zodat deze bulksgewijs kunt laden.  Ontwikkel uw processen om de afzonderlijke gegevens aan een bestand toe te voegen en maak vervolgens een ander proces dat het bestand periodiek laadt.

### <a name="create-statistics-after-the-load"></a>Statistieken maken na het laden

Voor optimale resultaten van uw query's is het belangrijk dat u statistieken maakt voor alle kolommen van alle tabellen nadat de gegevens voor het eerst zijn geladen of wanneer de gegevens substantieel zijn gewijzigd.  Zie [Statistics][Statistics] voor een gedetailleerde uitleg van statistieken. In het volgende voorbeeld worden statistieken in vijf kolommen van de tabel Customer_Speed aangemaakt.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="rotate-storage-keys"></a>Opslagsleutels draaien
Het is verstandig uit veiligheidsoverwegingen de toegangssleutel in de blob-opslag regelmatig te wijzigen. Er zijn twee opslagsleutels voor uw blob-opslagaccount. Zo kunt u de sleutels veranderen.

Sleutels van het Microsoft Azure Storage-account draaien:

1. Maak een tweede database-scoped referentie op basis van de secundaire toegangssleutel.
2. Maak een tweede externe gegevensbron gebaseerd op deze nieuwe referentie.
3. Verwijder en maak de externe tabel(len) zodat deze naar de nieuwe externe gegevensbronnen wijzen. 

Voer na het migreren van uw externe tabellen naar de nieuwe gegevensbron deze opschoningstaken uit:

1. Verwijder de eerste externe gegevensbron.
2. Verwijder de eerste database-scoped referentie op basis van de primaire toegangssleutel.
3. Meld u aan bij Azure en genereer de primaire toegangssleutel opnieuw zodat deze klaar is voor uw volgende draaiing.


## <a name="next-steps"></a>Volgende stappen
Zie voor het controleren van het laadproces [Uw workload controleren met DMV's](sql-data-warehouse-manage-monitor.md).




---
title: 'Migreren: Datawarehouse migratiehulpprogramma | Microsoft Docs'
description: Migreren naar SQL datawarehouse.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 4d7ad981-ef31-4513-b337-50bdc4709c09
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: e8a8a84153a950f2d1bc002b34c83dc5ed8a5eb8
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="data-warehouse-migration-utility-preview"></a>Datawarehouse migratiehulpprogramma (Preview)
> [!div class="op_single_selector"]
> * [Migratiehulpprogramma voor downloaden][Download Migration Utility]
> 
> 

Data Warehouse migratieprogramma is een hulpprogramma waarmee schema en de gegevens van SQL Server en Azure SQL Database naar Azure SQL Data Warehouse migreren. Tijdens de migratie van schema, het hulpprogramma automatisch toegewezen voor het bijbehorende schema van bron naar doel. Nadat het schema is gemigreerd, biedt de hulpprogramma's voor de mogelijkheid om gegevens met een automatisch gegenereerde scripts te verplaatsen.

Naast de migratie van het schema en de gegevens kunt met dit hulpprogramma u de optie voor het genereren van compatibiliteitsrapporten die geven een overzicht van compatibiliteitsproblemen tussen de doel- en exemplaren waardoor gestroomlijnde migratie.

## <a name="get-started"></a>Aan de slag
Als een vereiste voor installatie moet u het opdrachtregelprogramma BCP migratiescripts en Office om weer te geven van het compatibiliteitsrapport uitvoeren. Na het starten van het uitvoerbare bestand dat is gedownload wordt u gevraagd een standaard EULA accepteren voordat u het hulpprogramma wordt geïnstalleerd.

Bovendien voor het uitvoeren van de migratie Utiliy, moet u de volgende machtigingen op de database die u wilt migreren: CREATE DATABASE, ALTER ANY DATABASE of een WEERGAVEDEFINITIE.

### <a name="launching-the-tool-and-connecting"></a>Het hulpprogramma starten en verbinding te maken
Start het hulpprogramma door te klikken op het bureaubladpictogram die wordt weergegeven na installatie. Bij het openen van het hulpprogramma, wordt u gevraagd een eerste verbinding pagina u de bron- en doel voor het hulpprogramma voor migratie kunt. Op dit moment ondersteunen we SQL Server en Azure SQL Database als bronnen en SQL Data Warehouse als doel. Nadat u dit hebt geselecteerd wordt u gevraagd om te verbinden met de bronserver door de servernaam invullen en verifiëren en klik vervolgens op 'Verbinding'.

Als de verificatie is gelukt, wordt het hulpprogramma voor een lijst met databases die aanwezig in de server waarmee u bent zijn met verbonden weergegeven. De migratie kunt u beginnen door te selecteren van een database die u wilt migreren en vervolgens te klikken op 'Migreren geselecteerd'.

## <a name="migration-report"></a>Migratierapport
'Controleer databasecompatibiliteit' selecteren in het hulpprogramma wordt een samenvatting van alle object compatibiliteitsproblemen met de database aangevraagde rapport voor het migreren van gegenereerd. Een uitgebreidere lijst met enkele van de SQL Server-functionaliteit die is niet aanwezig in SQL Data Warehouse vindt u in onze [migratiedocumentatie][migration documentation]. Nadat het rapport is gegenereerd kunt u zich kunt opslaan en het rapport openen in Excel.

Houd er rekening mee dat bij het genereren van het schema van de migratie, de meeste problemen geïdentificeerd als 'Object' om een directe migratie van die gegevens moet worden aangepast. Controleer de wijzigingen om te controleren of dat u niet wilt dat extra aanpassingen maken voordat u het schema.

## <a name="migrate-schema"></a>Schema migreren
Nadat u verbinding maakt, selecteren 'Migreren Schema' wordt een schema migratiescript genereren voor de geselecteerde tabellen. Deze poorten script de structuur van de tabel, maps incompatibele gegevens typen aan meer compatibele formulieren en beveiligingsreferenties en het schema maakt als dit wordt aangegeven door de gebruiker in het migratie-instellingen. Deze code kan worden uitgevoerd op basis van het exemplaar van SQL Data Warehouse, opgeslagen in een bestand, gekopieerd naar het Klembord of zelfs bewerkt in-line alvorens verdere actie te ondernemen.  

Als opgemerkt, wanneer migreren schema controleren de migratie wordt gewijzigd dat het hulpprogramma heeft gemaakt, om ervoor te zorgen dat dat u volledig deze begrijpt.  

## <a name="migrate-data"></a>Gegevens migreren
Door te klikken op de optie 'gegevens migreren, kunt u BCP-scripts die uw gegevens eerst naar platte bestanden op uw server gaat genereren en vervolgens rechtstreeks in uw SQL Data Warehouse. Het is raadzaam om dit proces voor het verplaatsen van kleine hoeveelheden gegevens, en als nieuwe pogingen zijn niet ingebouwde en kan mislukken als er een verlies van de netwerkverbinding. Als u wilt uitvoeren, moet u het opdrachtregelprogramma BCP geïnstalleerd hebben en het schema voor de gegevens, moet al zijn gemaakt.

Nadat u de parameters hierboven hebt ingevuld moet u gewoon op migratie uitvoeren te klikken en een set van twee pakketten worden gegenereerd voor de opgegeven locatie. Het exportbestand uitgevoerd om gegevens uit uw migratiebron exporteren naar platte bestanden en het uitvoeren van het importbestand om te kunnen uw gegevens importeren in SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen
Nu dat u sommige gegevens hebt gemigreerd, zoeken over [ontwikkelen][develop].

<!--Image references-->

<!--Article references-->
[migration documentation]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Download Migration Utility]: https://www.microsoft.com/en-us/download/details.aspx?id=49100

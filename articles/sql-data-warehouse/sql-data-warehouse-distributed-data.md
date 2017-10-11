---
title: Hoe gedistribueerde gegevens in Azure SQL Data Warehouse werkt | Microsoft Docs
description: Meer informatie over hoe gegevens worden gedistribueerd voor Massively parallelle verwerking (MPP) en de opties voor het distribueren van tabellen in Azure SQL Data Warehouse en Parallel Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: bae494a6-7ac5-4c38-8ca3-ab2696c63a9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/12/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 3c166acb17193caae32d7bad133ec510ff679353
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Gedistribueerde gegevens en gedistribueerde tabellen voor Massively parallelle verwerking (MPP)
Meer informatie over hoe gebruikersgegevens wordt gedistribueerd in Azure SQL Data Warehouse en Parallel Data Warehouse van Microsoft Massively parallelle verwerking MPP-systemen zijn. Het ontwerpen van uw datawarehouse effectief gebruik van gedistribueerde gegevens helpt u bij het bereiken van de voordelen van de MPP-architectuur verwerken van query's. Enkele database ontwerpbeslissingen die kunnen een aanzienlijke gevolgen hebben voor verbetering van de prestaties van query's.  

> [!NOTE]
> Azure SQL Data Warehouse en Parallel Data Warehouse gebruiken hetzelfde Massively parallelle verwerking MPP-ontwerp, maar ze hebben een paar verschillen vanwege het onderliggende platform. SQL Data Warehouse is een Platform als een Service (PaaS) die wordt uitgevoerd op Azure. Parallel Data Warehouse wordt uitgevoerd op Analytics Platform System (AP) Dit is een lokale apparaat dat wordt uitgevoerd op Windows Server.
> 
> 

## <a name="what-is-distributed-data"></a>Wat is een gedistribueerde gegevens?
In SQL Data Warehouse en Parallel Data Warehouse verwijst gedistribueerde gegevens naar de gebruikersgegevens die op meerdere locaties is opgeslagen in de MPP-systeem. Elk van deze locaties fungeert als een onafhankelijke opslag en verwerkingseenheid dat query's op het gedeelte van de gegevens wordt uitgevoerd. Gedistribueerde gegevens is van cruciaal belang voor het uitvoeren van query's parallel met hoge queryprestaties.

Voor het distribueren van gegevens, wijst het datawarehouse elke rij van een gebruikerstabel naar één gedistribueerde locatie.  U kunt distribueren tabellen met een hash-distributiemethode of een round robin-methode. De distributiemethode is opgegeven in de instructie CREATE TABLE. 

## <a name="hash-distributed-tables"></a>Gedistribueerd hash tabellen
Het volgende diagram illustreert hoe een volledige (tabel niet gedistribueerd) wordt opgeslagen als een tabel met hash gedistribueerd. Elke rij te behoren tot een distributiepunt wordt toegewezen door een deterministische functie. In de tabeldefinitie, is een van de kolommen aangewezen als de distributie-kolom. De hash-functie maakt gebruik van de waarde in de kolom distributie elke rij toewijzen aan een distributiepunt.

Er zijn prestatie-overwegingen voor de selectie van een distributie-kolom, zoals onderscheidbaarheid, gegevens Tijdverschil en de typen query's uitvoeren op het systeem.

![Gedistribueerde tabel](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "gedistribueerde tabel")  

* Elke rij hoort bij een distributiepunt.  
* Elke rij een deterministische hash-algoritme toegewezen aan een distributiepunt.  
* Het aantal rijen per distributiepunt verschilt zoals aangegeven door de verschillende grootten van tabellen.

## <a name="round-robin-distributed-tables"></a>Round robin gedistribueerde tabellen
Een gedistribueerde tabel round robin distribueert de rijen door elke rij toewijzen aan een distributiepunt in de juiste volgorde. Het is heel eenvoudig om gegevens te laden in een tabel round robin, maar queryprestaties mogelijk langzamer.  Meestal lid van een round robin-tabel, moet de rijen om in te schakelen van de query voor het produceren van een accuraat resultaat opleveren, die duurt reshuffling.

## <a name="distributed-storage-locations-are-called-distributions"></a>Gedistribueerde opslaglocaties worden distributies genoemd
Elke gedistribueerde locatie heet een distributiepunt. Wanneer een query wordt parallel uitgevoerd, voert elke distributie een SQL-query op het gedeelte van de gegevens. SQL Data Warehouse maakt gebruik van SQL-Database voor de query. SQL Server Parallel Data Warehouse gebruikt de query uit te voeren. Dit architectuurontwerp niets wordt gedeeld is van cruciaal belang bij het bereiken van scale-out parallelle berekeningen.

### <a name="can-i-view-the-distributions"></a>Kan ik de Distributies weergeven?
Elk distributiepunt heeft een distributie-ID en zichtbaar is in systeemweergaven die betrekking op de SQL Data Warehouse en Parallel Data Warehouse hebben. De ID van het distributiepunt kunt u problemen met prestaties van query's en andere problemen oplossen. Zie voor een lijst van de systeemweergaven de [MPP systeemweergave](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Verschil tussen een distributiepunt en een rekenknooppunt
Een distributiepunt is de basiseenheid voor gedistribueerde gegevens op te slaan en de verwerking van parallelle query's. Distributies zijn gegroepeerd in rekenknooppunten. Elk rekenknooppunt houdt een of meer verdelingen.  

* Rekenknooppunten Analytics Platform System gebruikt als een centrale onderdeel van de hardwaremogelijkheden voor architectuur en scale-out. Deze gebruikt altijd acht distributies per rekenknooppunt, zoals wordt weergegeven in het diagram voor gedistribueerde hash tabellen. Het aantal rekenknooppunten en daarom op het aantal distributies, wordt bepaald door het aantal rekenknooppunten die u voor het toestel koopt. Als u acht rekenknooppunten koopt, krijgt u bijvoorbeeld 64 distributies (8 knooppunten x 8 distributies/rekenknooppunt). 
* SQL Data Warehouse heeft een vast aantal 60 distributies en een flexibele aantal rekenknooppunten. De rekenknooppunten worden geïmplementeerd met Azure-resources berekeningen en opslag. Het aantal rekenknooppunten kunt wijzigen op basis van de werkbelasting van de service back-end en de computercapaciteit (dwu's), die u kunt voor het datawarehouse opgeven. Wanneer het aantal rekenknooppunten wordt gewijzigd, wordt het aantal distributies per rekenknooppunt ook gewijzigd. 

### <a name="can-i-view-the-compute-nodes"></a>Kan ik de rekenknooppunten weergeven?
Elk rekenknooppunt heeft een knooppunt-ID en zichtbaar is in systeemweergaven die betrekking op de SQL Data Warehouse en Parallel Data Warehouse hebben.  U ziet het rekenknooppunt voor de kolom node_id in systeemweergaven waarvan de naam met sys.pdw_nodes begint. Zie voor een lijst van de systeemweergaven de [MPP systeemweergave](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Gerepliceerde tabellen
Een tabel die is gerepliceerd heeft een volledig kopie van de tabel op elk rekenknooppunt worden opgeslagen. Bezig met het repliceren van een tabel verwijdert de behoefte om over te dragen van gegevens tussen rekenknooppunten voordat een join of aggregatie. Gerepliceerde tabellen zijn alleen mogelijk met kleine tabellen vanwege de extra opslag vereist voor het opslaan van de volledige tabel op elk rekenknooppunt.  

Het volgende diagram toont een gerepliceerde tabel die is opgeslagen op elk rekenknooppunt. Voor SQL Data Warehouse, wordt de gerepliceerde tabel volledig gekopieerd naar een distributiedatabase op elk rekenknooppunt. De gerepliceerde tabel is opgeslagen voor Parallel Data Warehouse over alle schijven die zijn toegewezen aan het rekenknooppunt.  Deze schijf-strategie is geïmplementeerd met behulp van SQL Server-bestandsgroepen.  

![Gerepliceerde tabel](media/sql-data-warehouse-distributed-data/replicated-table.png "gerepliceerde tabel") 

## <a name="next-steps"></a>Volgende stappen
Gedistribueerde tabellen als effectief wilt gebruiken, Zie [tabellen in SQL Data Warehouse distribueren](sql-data-warehouse-tables-distribute.md)  


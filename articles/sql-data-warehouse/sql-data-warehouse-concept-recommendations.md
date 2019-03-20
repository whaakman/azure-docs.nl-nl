---
title: SQL Data Warehouse aanbevelingen - concepten | Microsoft Docs
description: Meer informatie over aanbevelingen voor SQL Data Warehouse en hoe ze worden gegenereerd
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 11/05/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: b275f23209979e1a8068ecd99465f7b52392bc6c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58082905"
---
# <a name="sql-data-warehouse-recommendations"></a>Aanbevelingen voor SQL datawarehouse

Dit artikel beschrijft de aanbevelingen die is geleverd door SQL Data Warehouse via Azure Advisor.  

SQL Data Warehouse biedt aanbevelingen om te controleren of uw datawarehouse wordt voortdurend geoptimaliseerd voor prestaties. Datawarehouse aanbevelingen zijn nauw geïntegreerd met [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) zodat u kunt met best practices rechtstreeks binnen de [Azure-portal](https://aka.ms/Azureadvisor). SQL Data Warehouse de huidige status van uw datawarehouse analyseert, Telemetrie en bevat aanbevelingen voor de werkbelasting van uw actieve dagelijks worden verzameld. De aanbeveling scenario's met ondersteunde datawarehouse worden hieronder beschreven, samen met het toepassen van de aanbevolen acties.

Als u feedback over de SQL Data Warehouse Advisor hebt of ondervindt problemen, contact op met [ sqldwadvisor@service.microsoft.com ](mailto:sqldwadvisor@service.microsoft.com).   

Klik op [hier](https://aka.ms/Azureadvisor) om te controleren op uw aanbevelingen vandaag nog! Deze functie is momenteel van toepassing op datawarehouses alleen Gen2. 

## <a name="data-skew"></a>Gegevensverschil

Gegevensverschil kan leiden tot van knelpunten in verkeer of de resource aanvullende gegevens wanneer uw workload wordt uitgevoerd. De volgende documentatie beschreven show gegevensverschil identificeren en te voorkomen dat dit gebeurt door het selecteren van een optimale distributiesleutel.

- [Identificeren en scheeftrekken verwijderen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Geen verouderde statistieken of

Uit suboptimale statistieken kan grote invloed is op prestaties van query's, dit leiden de SQL Data Warehouse-queryoptimalisatieprogramma voor het genereren van suboptimale queryplannen tot kan. De volgende documentatie worden de aanbevolen procedures behandeld voor het maken en bijwerken van statistieken beschreven:

- [Het maken en bijwerken van tabelstatistieken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics)

Als u wilt de lijst met betrokken tabellen zien door deze aanbevelingen, voer de volgende [T-SQL-script](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables). Advisor wordt continu uitgevoerd voor de dezelfde T-SQL-script voor het genereren van deze aanbevelingen.

## <a name="replicate-tables"></a>Tabellen repliceren

Voor gerepliceerde tabel aanbevelingen detecteert Advisor tabel kandidaten op basis van de volgende fysieke kenmerken:

- Tabelgrootte gerepliceerd
- Aantal kolommen
- Tabel Distributietype
- Aantal partities

Advisor continu maakt gebruik van heuristiek zoals tabel toegang frequentie, op basis van een werkbelasting rijen geretourneerd, gemiddeld en drempelwaarden rond gegevens datawarehouse-grootte en de activiteit om ervoor te zorgen goede aanbevelingen worden gegenereerd. 

Hierna wordt beschreven op basis van een werkbelasting methodiek voor die u in de Azure-portal voor elke aanbeveling gerepliceerde tabel vinden kan:

- Scan Gem.-het gemiddelde percentage van de rijen die zijn geretourneerd uit de tabel voor elke tabeltoegang gedurende de afgelopen zeven dagen
- Frequente lees-, geen update - geeft aan dat de tabel niet in de afgelopen zeven dagen bijgewerkt is tijdens de weergave van de activiteit voor het openen
- Verhouding tussen lezen/bijwerken - de verhouding van hoe vaak de tabel is geopend ten opzichte van wanneer deze gedurende de afgelopen zeven dagen wordt bijgewerkt
- Activiteit - Hiermee wordt het gebruik op basis van de activiteit voor het openen. Dit vergelijkt de activiteit voor het openen van tabel ten opzichte van de activiteit voor het openen van gemiddelde tabel in het datawarehouse gedurende de afgelopen zeven dagen. 

Op dit moment wordt Advisor alleen weergegeven maximaal vier gerepliceerde tabel kandidaten in één keer met geclusterde columnstore-indexen meer prioriteit toekennen aan de hoogste activiteit.

> [!IMPORTANT]
> De aanbeveling gerepliceerde tabel is niet volledig bewijs en hierbij wordt geen rekening gegevensverplaatsingsbewerkingen account. Er wordt gewerkt aan toe te voegen als een heuristiek, maar in de tussentijd kunt u altijd uw workload moet valideren nadat de aanbeveling is toegepast. Neem contact op met sqldwadvisor@service.microsoft.com als u gerepliceerde tabel aanbevelingen die ervoor zorgt dat uw workload ontdekt aan gaat. Voor meer informatie over gerepliceerde tabellen, gaat u naar de volgende [documentatie](https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables#what-is-a-replicated-table).

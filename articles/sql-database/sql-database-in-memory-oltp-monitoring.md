---
title: XTP geheugenopslag bewaken | Microsoft Docs
description: Schatting en monitor XTP geheugenopslag gebruiken, capaciteit; capaciteit fout 41823 oplossen
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jodebrui
ms.openlocfilehash: 613a9ced91d71cc9a65ea67e6ede1a78a03b4bd5
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="monitor-in-memory-oltp-storage"></a>Monitor In-geheugen OLTP-opslag
Wanneer u [In het geheugen OLTP](sql-database-in-memory.md), gegevens in tabellen geoptimaliseerd voor geheugen en tabelvariabelen bevindt zich in In het geheugen OLTP-opslag. Elke servicelaag Premium heeft een maximale In het geheugen OLTP opslaggrootte, die wordt beschreven in [één database limieten](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) en [limieten elastische pool](sql-database-resource-limits.md#elastic-pool-change-storage-size). Wanneer deze limiet wordt overschreden, invoegen en bijwerken bewerkingen mogelijk mislukken (met de fout 41823). Op dat moment wordt u nodig aan gegevens om geheugen vrij te verwijderen of upgraden van de prestatielaag van de database.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Bepalen of gegevens binnen het kapje in het geheugen, opslag past
Bepaal de caps opslag van de verschillende Servicelagen van Premium. Zie [één database limieten](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) en [limieten elastische pool](sql-database-resource-limits.md#elastic-pool-change-storage-size).

Het schatten van geheugenvereisten voor een tabel geoptimaliseerd voor geheugen works voor SQL Server dezelfde manier als het komt in Azure SQL Database. Enkele minuten duren om te controleren dat onderwerp op [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Houd er rekening mee dat de tabel en variabele tabelrijen, evenals indexen, voor de gegevensgrootte max gebruiker meetellen. ALTER TABLE moet bovendien voldoende ruimte voor het maken van een nieuwe versie van de gehele tabel en de indexen ervan.

## <a name="monitoring-and-alerting"></a>Bewaking en waarschuwingen
U kunt opslaggebruik in het geheugen als percentage van het kapje opslag controleren voor de prestatielaag in de [Azure-portal](https://portal.azure.com/): 

1. Zoek het gebruik van Resource op de blade Database en klik op bewerken.
2. Selecteer de metriek `In-Memory OLTP Storage percentage`.
3. Als u wilt een waarschuwing toevoegen, klikt u op in het vak Resourcegebruik om de metriek blade te openen en klik vervolgens op waarschuwing toevoegen.

Of gebruik de volgende query voor het gebruik van de opslag in het geheugen weergeven:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Juiste-geheugen situaties - fout 41823
-Geheugen resultaten uitgevoerd in invoegen, bijwerken en bewerkingen is mislukt met foutbericht 41823 maakt.

Foutbericht 41823 geeft aan dat de tabellen geoptimaliseerd voor geheugen en tabelvariabelen de maximale grootte overschreden.

Deze fout, ofwel oplossen:

* Gegevens verwijderen uit de tabellen geoptimaliseerd voor geheugen, mogelijk offloading van de gegevens naar de traditionele, schijfgebaseerde tabellen. of,
* De servicetier upgraden naar een met voldoende opslagruimte in het geheugen voor de gegevens die u wilt bewaren in tabellen geoptimaliseerd voor geheugen.

## <a name="next-steps"></a>Volgende stappen
Zie voor richtlijnen bewaking, [bewaking Azure SQL Database met dynamische beheerweergaven](sql-database-monitoring-with-dmvs.md).

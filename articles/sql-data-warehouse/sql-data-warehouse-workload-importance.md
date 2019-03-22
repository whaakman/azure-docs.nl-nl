---
title: Urgentie van de werkbelasting | Microsoft Docs
description: Richtlijnen voor het instellen van belang is voor query's in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: e53a6fcefb0f5370f6e24cc50fad2ad4ad4c64e3
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57864037"
---
# <a name="sql-data-warehouse-workload-importance-preview"></a>SQL Data Warehouse workload belang (Preview)

In dit artikel wordt uitgelegd hoe belang van de werkbelasting van invloed zijn op de volgorde van de uitvoering voor SQL Data Warehouse aanvragen.

> [!Note]
> Urgentie van de werkbelasting is beschikbaar in SQL Data Warehouse Gen2.

## <a name="importance"></a>Urgentie

> [!Video https://www.youtube.com/embed/_2rLMljOjw8]

Behoeften van uw bedrijf kunnen vereisen datawarehousing-workloads te belangrijker dan andere.  U hebt een scenario waarbij essentiële verkoop gegevens wordt geladen voordat de fiscale sluiten periode.  Gegevens zijn geladen voor andere bronnen, zoals weergegevens geen strikte Sla's.   Hoge urgentie van een aanvraag voor het laden van gegevens van internetverkopen en lage urgentie instelt op een aanvraag voor het laden of gegevens zorgt ervoor de belasting verkoopgegevens dat eerste toegang tot resources opgehaald en sneller voltooid.

## <a name="importance-levels"></a>Urgentieniveaus

Er zijn vijf niveaus van belangrijkheid: laag, below_normal, normaal, above_normal en hoog.  Aanvragen die niet urgentie zijn het standaardniveau van normaal worden toegewezen.  Aanvragen met hetzelfde urgentieniveau hebben hetzelfde schema gedrag die vandaag de dag.

## <a name="importance-scenarios"></a>Urgentie scenario 's

Naast de hierboven beschreven met verkoop- en weergegevens basic belang-scenario, moet u er andere scenario's waarbij werkbelasting belang helpt bij het voldoen aan de gegevensverwerking en het uitvoeren van query's behoeften zijn.

### <a name="locking"></a>Vergrendelen

Toegang tot de vergrendelingen voor lezen en schrijven van de activiteit is een gebied van natuurlijk conflicten.  Activiteiten zoals [partitie overschakelen](/azure/sql-data-warehouse/sql-data-warehouse-tables-partition) of [naam OBJECT](/sql/t-sql/statements/rename-transact-sql) vereist verhoogde wordt vergrendeld.  SQL Data Warehouse wordt zonder de werkbelasting van belang geoptimaliseerd voor doorvoer.  Optimaliseren voor doorvoer houdt in dat bij het uitvoeren van aanvragen in de wachtrij dezelfde vergrendelen behoeften hebben en bronnen beschikbaar zijn, kunnen de aanvragen in de wachtrij aanvragen met hogere vergrendelen vereisten die eerder in de wachtrij voor aanvragen ontvangen omzeilen.  Nadat het belang van de werkbelasting wordt toegepast op aanvragen met hogere vergrendeling vereist. Aanvraag met hogere prioriteit worden uitgevoerd voordat de aanvraag met lagere prioriteit.

Kijk een naar het volgende voorbeeld:

Q1 wordt actief wordt uitgevoerd en gegevens van SalesFact selecteren.
Q2 is in de wachtrij in afwachting van Q1 om te voltooien.  Deze om 9 uur is ingediend en wordt geprobeerd nieuwe gegevenspartities switch in SalesFact.
Q3 wordt verzonden om 9:01 uur en wil gegevens selecteren uit SalesFact.

Als Q2 en Q3 even belangrijk zijn en Q1 nog wordt uitgevoerd, begint Q3 wordt uitgevoerd. Wachten op een exclusieve vergrendeling op SalesFact blijven Q2.  Als Q2 heeft een hogere prioriteit dan K3, wacht K3 totdat Q2 is voltooid voordat deze kan worden uitgevoerd.

### <a name="non-uniform-requests"></a>Niet-uniforme aanvragen

Een ander scenario waar belang kunt voldoen aan een query uitvoeren op vereisten is wanneer aanvragen met verschillende resourceklassen worden verzonden.  Zoals eerder is vermeld, onder even belangrijk zijn, is SQL Data Warehouse wordt geoptimaliseerd voor doorvoer.  Wanneer gemengde grootte aanvragen (zoals smallrc of mediumrc) in de wachtrij geplaatst zijn, wordt de vroegste binnenkomende aanvraag die met de beschikbare resources overeenkomt kiezen in SQL Data Warehouse.  Als u werkbelasting prioriteit wordt toegepast, wordt de hoogste prioriteit-aanvraag vervolgens gepland.
  
Houd rekening met het volgende voorbeeld op DW500c:

K1, k2, K3 en K4 worden smallrc query's uitgevoerd.
Vraag 5 wordt verzonden met de resourceklasse mediumrc om 9 uur.
Q6 wordt verzonden met de resourceklasse smallrc om 9:01 uur.

Omdat vraag 5 mediumrc is, worden hiervoor twee gelijktijdigheidssleuven in beslag.  Vraag 5 moet wachten tot twee van het uitvoeren van query's om te voltooien.  Echter, wanneer een van de actieve query's (Q1-K4) is voltooid, Q6 is gepland onmiddellijk omdat de resources voor het uitvoeren van de query bestaat.  Als de vraag 5 heeft een hogere prioriteit dan Q6, wacht Q6 totdat vraag 5 wordt uitgevoerd voordat deze kan uitvoeren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over SQL Data Warehouse workload classificatie [SQL Data Warehouse Workload classificatie](sql-data-warehouse-workload-classification.md) en [maken van een classificatie van de werkbelasting](quickstart-create-a-workload-classifier-tsql.md). Zie [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) om query's en het belang toegewezen weer te geven.

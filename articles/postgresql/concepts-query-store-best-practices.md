---
title: Aanbevolen procedures voor query Store in Azure Database for PostgreSQL
description: Dit artikel wordt beschreven aanbevolen procedures voor de Query Store in Azure Database voor PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 4e133458476a61538c2bddaaeb2f6d56d4ea5a26
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46954905"
---
# <a name="best-practices-for-query-store"></a>Aanbevolen procedures voor Query Store

**Is van toepassing op:** Azure Database for PostgreSQL 9.6 en 10

> [!IMPORTANT]
> De functie voor Query Store is in openbare Preview.


In dit artikel bevat een overzicht van de aanbevolen procedures voor het gebruik van de Query Store in Azure Database voor PostgreSQL.

## <a name="set-the-optimal-query-capture-mode"></a>Instellen van de optimale query-modus voor vastleggen
Laat Query Store vastleggen van de gegevens die voor u belangrijk is. 

|**pg_qs.query_capture_mode** | **Scenario**|
|---|---|
|_Alle_  |Analyseer uw workload grondig in termen van alle query's en de frequentie kan worden uitgevoerd en andere statistieken. Nieuwe query's in uw werkbelasting identificeren. Als ad-hocquery's worden gebruikt voor identificatie van de mogelijkheden voor de gebruiker of parameterisering automatisch worden gedetecteerd. _Alle_ wordt geleverd met een hogere resourceverbruik kosten. |
|_Boven_  |Richt uw aandacht op de belangrijkste query's, die zijn uitgegeven door clients.
|_Geen_ |U hebt al een queryset vastgelegd en tijdvenster op dat u wilt onderzoeken en u wilt de storende die tot andere query's leiden. _Geen_ is geschikt is voor testen en benchmarken omgevingen. _Geen_ moet worden gebruikt met zorgvuldig te werk als u de mogelijkheid om te houden en optimaliseren van belangrijke nieuwe query's mogelijk gemist. U kunt gegevens in de afgelopen tijd windows niet herstellen. |

Query Store bevat ook een archief voor wait statistieken. Er is een query in de aanvullende vastleggen die wacht statistieken: **pgms_wait_sampling.query_capture_mode** kan worden ingesteld op _geen_ of _alle_. 

> [!NOTE] 
> **pg_qs.query_capture_mode** vervangt **pgms_wait_sampling.query_capture_mode**. Als pg_qs.query_capture_mode _geen_, de instelling pgms_wait_sampling.query_capture_mode heeft geen effect. 


## <a name="keep-the-data-you-need"></a>De benodigde gegevens behouden
De **pg_qs.retention_period_in_days** parameter geeft u in de bewaarperiode voor Query Store dagen. Oudere query en statistische gegevens worden verwijderd. Query Store is standaard geconfigureerd als u wilt de gegevens gedurende zeven dagen bewaren. Voorkom dat historische gegevens die u niet van plan bent te gebruiken. Verhoog de waarde als u wilt gegevens niet langer bewaren.


## <a name="set-the-frequency-of-wait-stats-sampling"></a>Stel de frequentie van Wachtstatistieken voor steekproeven 
De **pgms_wait_sampling.history_period** parameter geeft aan hoe vaak (in milliseconden) wacht gebeurtenissen worden vastgelegd. Hoe korter de periode, frequente hoe meer de steekproeven. Meer informatie wordt opgehaald, maar die wordt geleverd met de kosten van meer verbruik van databaseresources. Deze periode verhogen als de server belast wordt of u niet de granulatie hoeft


## <a name="get-quick-insights-into-query-store"></a>Verkrijg snel inzicht in Query Store
U kunt [Query Performance Insight](concepts-query-performance-insight.md) in Azure portal snel inzicht krijgt in de gegevens in Query Store. De visualisaties de langste uitvoeren van query's het oppervlak en wacht langst gebeurtenissen na verloop van tijd.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het ophalen of instellen met behulp van parameters de [Azure-portal](howto-configure-server-parameters-using-portal.md) of de [Azure CLI](howto-configure-server-parameters-using-cli.md).
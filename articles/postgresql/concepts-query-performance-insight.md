---
title: Query Performance Insight in Azure Database for PostgreSQL
description: Dit artikel beschrijft de Query Performance Insight-functie in Azure Database voor PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 6d92515a49060c8113fb7c2c75100103a75e5d49
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971641"
---
# <a name="query-performance-insight"></a>Inzicht in queryprestaties 

**Is van toepassing op:** Azure Database for PostgreSQL 9.6 en 10

> [!IMPORTANT]
> De Query Performance Insight-functie is beschikbaar als openbare preview. 

Query Performance Insight kunt u snel vinden wat uw langst lopende query's zijn, hoe ze na verloop van tijd veranderen en welke wacht van invloed zijn op deze.

## <a name="permissions"></a>Machtigingen
**De eigenaar van** of **Inzender** machtigingen die nodig zijn om de tekst van de query's in Query Performance Insight weer te geven. **Lezer** kunt grafieken en tabellen weergeven maar geen query uitvoeren op tekst.

## <a name="prerequisites"></a>Vereisten
Voor Query Performance Insight functie, gegevens moeten aanwezig zijn in de [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Prestatie-inzichten weergeven
De [Query Performance Insight](concepts-query-performance-insight.md) weergeven in Azure portal verschijnt visualisaties op belangrijke informatie van de Query Store. 

Selecteer in de portal-pagina van uw Azure Database for PostgreSQL-server, **Query performance Insight** onder de **ondersteuning en probleemoplossing** sectie van de in de menubalk.

![Query Performance Insight langlopende query 's](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

De **langlopende query's** tabblad toont de top 5 query's door de gemiddelde duur per uitvoering, samengevoegd in intervallen van 15 minuten. U kunt meer query's weergeven door te selecteren in de **nummer van query's** vervolgkeuzelijst. Kleuren in de grafiek kunnen wijzigen voor een specifieke Query-ID als u dit doet.

U kunt Klik en sleep in de grafiek om te beperken tot een specifiek tijdstip-venster. Het zoomniveau in- en pictogrammen ook gebruiken om respectievelijk een kleiner of groter periode weer te geven.

In de tabel onder de grafiek geeft meer informatie over de langlopende query's in dat tijdvenster.

Selecteer de **wacht statistieken** tabblad om de bijbehorende visualisaties op wacht op de server weer te geven.

![Query Performance Insight Wachtstatistieken](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [bewaking en afstemming](concepts-monitoring.md) in Azure Database for PostgreSQL.



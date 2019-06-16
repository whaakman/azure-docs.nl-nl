---
title: Query Performance Insight in Azure Database for PostgreSQL - één Server
description: Dit artikel beschrijft de Query Performance Insight-functie in Azure Database voor PostgreSQL - één Server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: d45b79e2ca3b3d478102bebdcff3c8892bef2cb5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067545"
---
# <a name="query-performance-insight"></a>Inzicht in queryprestaties 

**Van toepassing op:** Azure Database for PostgreSQL - servergegevens 9.6 en 10

Query Performance Insight kunt u snel vinden wat uw langst lopende query's zijn, hoe ze na verloop van tijd veranderen en welke wacht van invloed zijn op deze.

## <a name="permissions"></a>Machtigingen
De machtigingen **Eigenaar** of **Inzender** zijn vereist om de tekst van de query's weer te geven in Query Performance Insight. Met de machtiging **Lezer** kunt u grafieken en tabellen weergeven maar geen tekst opvragen.

## <a name="prerequisites"></a>Vereisten
Voor Query Performance Insight functie, gegevens moeten aanwezig zijn in de [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Prestatie-inzichten weergeven
De weergave [Query Performance Insight](concepts-query-performance-insight.md) in de Azure Portal toont visualisaties van belangrijke informatie uit de Query Store. 

Selecteer in de portal-pagina van uw Azure Database for PostgreSQL-server, **Query performance Insight** onder de **intelligente prestaties** sectie van de in de menubalk.

![Query Performance Insight langlopende query 's](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

De **langlopende query's** tabblad toont de top vijf query's door de gemiddelde duur per uitvoering, samengevoegd in intervallen van 15 minuten. U kunt meer query's weergeven door een optie te selecteren in de vervolgkeuzelijst **Aantal query's**. Het is mogelijk dat de grafiekkleuren voor een specifieke query-id verschillen wanneer u dit doet.

U kunt in de grafiek klikken en slepen om de tijdspanne te beperken tot een specifiek tijdvenster. Het zoomniveau in- en pictogrammen ook gebruiken om respectievelijk een kleiner of groter periode weer te geven.

In de tabel onder de grafiek geeft meer informatie over de langlopende query's in dat tijdvenster.

Selecteer het tabblad **Wachtstatistieken** om de bijbehorende visualisaties voor wachttijden in de server weer te geven.

![Query Performance Insight wacht statistieken](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [bewaking en afstemming](concepts-monitoring.md) in Azure Database for PostgreSQL.



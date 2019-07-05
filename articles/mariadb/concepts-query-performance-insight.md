---
title: Query Performance Insight in Azure Database voor MariaDB
description: In dit artikel beschrijft de Query Performance Insight-functie in Azure Database voor MariaDB
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 48ff1fdc08e0df463ec48fd1415c7b67d5beb744
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462108"
---
# <a name="query-performance-insight-in-azure-database-for-mariadb"></a>Query Performance Insight in Azure Database voor MariaDB

**Is van toepassing op:**  Azure Database voor MariaDB 10.2

> [!NOTE]
> Query Performance Insight is beschikbaar als preview.

Query Performance Insight kunt u snel vinden wat uw langst lopende query's zijn, hoe ze na verloop van tijd veranderen en welke wacht van invloed zijn op deze.

## <a name="common-scenarios"></a>Algemene scenario's

### <a name="long-running-queries"></a>Langlopende query 's

- Identificeren van de langste uitvoeren van query's in het verleden X uur
- Top N-query's die op resources wachten identificeren
 
### <a name="wait-statistics"></a>Wachtstatistieken

- Understanding wacht aard van een query
- Informatie over trends voor resource wacht en waar bronconflicten bestaat

## <a name="permissions"></a>Machtigingen

**De eigenaar van** of **Inzender** machtigingen die nodig zijn om weer te geven van de tekst van de query's in Query Performance Insight. ** Lezer** kunt grafieken en tabellen weergeven maar geen query uitvoeren op tekst.

## <a name="prerequisites"></a>Vereisten

Voor Query Performance Insight functie, gegevens moeten aanwezig zijn in de [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Prestatie-inzichten weergeven

De weergave [Query Performance Insight](concepts-query-performance-insight.md) in de Azure Portal toont visualisaties van belangrijke informatie uit de Query Store.

Selecteer in de portal-pagina van uw Azure Database voor MariaDB-server, **Query Performance Insight** onder de **intelligente prestaties** sectie van de in de menubalk.

### <a name="long-running-queries"></a>Langlopende query 's

De **langlopende query's** tabblad toont de top 5 query's door de gemiddelde duur per uitvoering, samengevoegd in intervallen van 15 minuten. U kunt meer query's weergeven door te selecteren in de **nummer van query's** vervolgkeuzelijst. Het is mogelijk dat de grafiekkleuren voor een specifieke query-id verschillen wanneer u dit doet.

U kunt in de grafiek klikken en slepen om de tijdspanne te beperken tot een specifiek tijdvenster. Het zoomniveau in- en pictogrammen ook gebruiken om respectievelijk een kleiner of groter periode weer te geven.

![Query Performance Insight langlopende query 's](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

### <a name="wait-statistics"></a>Wachtstatistieken 

> [!NOTE]
> Wacht statistieken zijn bedoeld voor het oplossen van problemen met query-prestaties. Het verdient aanbeveling om te worden ingeschakeld alleen voor het oplossen van problemen.

Wacht statistieken biedt een overzicht van de wait-gebeurtenissen die zich tijdens het uitvoeren van een specifieke query voordoen. Meer informatie over de gebeurtenistypen wachten in de [MySQL-engine documentatie](https://go.microsoft.com/fwlink/?linkid=2098206).

Selecteer de **wacht statistieken** tabblad om de bijbehorende visualisaties op wacht op de server weer te geven.

Query's weergegeven in de weergave van de statistieken wacht zijn gegroepeerd op de query's met de grootste wacht tijdens het opgegeven tijdsinterval.

![Query Performance Insight wacht statistieken](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [bewaking en afstemming](concepts-monitoring.md) in Azure Database voor MariaDB.
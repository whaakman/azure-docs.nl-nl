---
title: Query Performance Insight in Azure Database for MySQL
description: In dit artikel beschrijft de Query Performance Insight-functie in Azure Database for MySQL
author: ajlam
ms.author: andrela
ms.service: MySQL
ms.topic: conceptual
ms.date: 06/05/2019
ms.openlocfilehash: 1243ae8ae20d08ea643661606639abedbc56ab9c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078779"
---
# <a name="query-performance-insight-in-azure-database-for-mysql"></a>Query Performance Insight in Azure Database for MySQL

**Is van toepassing op:**  Azure Database voor MySQL 5.7

> [!NOTE]
> Query Performance Insight is beschikbaar als preview. Ondersteuning voor Query Performance Insight in Azure portal is geïmplementeerd en kan nog niet beschikbaar in uw regio.

Query Performance Insight kunt u snel vinden wat uw langst lopende query's zijn, hoe ze na verloop van tijd veranderen en welke wacht van invloed zijn op deze.

## <a name="permissions"></a>Machtigingen

**De eigenaar van** of **Inzender** machtigingen die nodig zijn om weer te geven van de tekst van de query's in Query Performance Insight. ** Lezer** kunt grafieken en tabellen weergeven maar geen query uitvoeren op tekst.

## <a name="prerequisites"></a>Vereisten

Voor Query Performance Insight functie, gegevens moeten aanwezig zijn in de [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Prestatie-inzichten weergeven

De weergave [Query Performance Insight](concepts-query-performance-insight.md) in de Azure Portal toont visualisaties van belangrijke informatie uit de Query Store.

Selecteer in de portal-pagina van uw Azure Database for MySQL-server, **Query Performance Insight** onder de **intelligente prestaties** sectie van de in de menubalk.

![Query Performance Insight langlopende query 's](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png) 

De **langlopende query's** tabblad toont de top 5 query's door de gemiddelde duur per uitvoering, samengevoegd in intervallen van 15 minuten. U kunt meer query's weergeven door te selecteren in de **nummer van query's** vervolgkeuzelijst. Het is mogelijk dat de grafiekkleuren voor een specifieke query-id verschillen wanneer u dit doet.

U kunt in de grafiek klikken en slepen om de tijdspanne te beperken tot een specifiek tijdvenster. Het zoomniveau in- en pictogrammen ook gebruiken om respectievelijk een kleiner of groter periode weer te geven.

Selecteer de **wacht statistieken** tabblad om de bijbehorende visualisaties op wacht op de server weer te geven.

Query's weergegeven in de weergave van de statistieken wacht zijn gegroepeerd op de query's met de grootste wacht tijdens het opgegeven tijdsinterval.

![Query Performance Insight wacht statistieken](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [bewaking en afstemming](concepts-monitoring.md) in Azure Database for MySQL.
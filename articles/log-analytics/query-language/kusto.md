---
title: Azure Monitor Log Analytics en Kusto taalverschillen | Microsoft Docs
description: Hierin wordt beschreven verschillen tussen Log Analytics-query's en core Kusto-taal.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 109ffa6abb34dad6a00210a5c2c726bdfdde094f
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184787"
---
# <a name="log-analytics-and-kusto-language-differences"></a>Verschillen in log Analytics en Kusto taal
[Meld u Analytics-query's](../log-analytics-queries.md) zijn geschreven met de [Kusto-taal](/azure/kusto/query). Er zijn enkele verschillen van de standaard taal en de Log Analytics-implementatie echter zoals beschreven in dit artikel.


## <a name="statements-not-supported-in-log-analytics"></a>Instructies worden niet ondersteund in Log Analytics
De volgende instructies worden niet ondersteund in Log Analytics.

* [Alias](/kusto/query/aliasstatement)
* [Queryparameters](/azure/kusto/query/queryparametersstatement)

## <a name="functions-not-supported-in-log-analytics"></a>Niet ondersteunde functies in Log Analytics
De volgende functies worden niet ondersteund in Log Analytics.

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

## <a name="operators-not-supported-in-log-analytics"></a>Operatoren niet ondersteund in Log Analytics
De volgende operators worden niet ondersteund in Log Analytics.

* [Cross-Cluster koppelen](/azure/kusto/query/joincrosscluster)
* [externaldata operator](/azure/kusto/query/externaldata-operator)

## <a name="plugins-not-supported-in-log-analytics"></a>Invoegtoepassingen die niet wordt ondersteund in Log Analytics
De volgende invoegtoepassingen worden niet ondersteund in Log Analytics.
* [sql_request-invoegtoepassing](/azure/kusto/query/sqlrequestplugin)


## <a name="log-analytics-specific-operators"></a>Log Analytics-specifieke operators
* [App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over query's in [Log Analytics](../log-analytics-queries.md).
- Doorloop een les schrijven op een [Log Analytics-query](/log-analytics/query-language/get-started-queries.md).
- Toegang tot de volledige [referentiedocumentatie voor Kusto](/azure/kusto/query/).
---
title: Naslaggids voor Azure Monitor Log Analytics | Microsoft Docs
description: Naslaginformatie voor Data Explorer query-taal die wordt gebruikt door Log Analytics. Bevat aanvullende elementen die specifiek zijn voor Log Analytics en elementen in Log Analytics-query's niet ondersteund.
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
ms.date: 10/29/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: e6d097749dae49cf6f1d710bcf01cf99dcd98a4c
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244120"
---
# <a name="log-analytics-query-language-differences"></a>Meld u Analytics query language verschillen

Terwijl [Log Analytics](../log-analytics-queries.md) is gebouwd op [Azure Data Explorer](/azure//data-explorer) en maakt gebruik van de [hetzelfde querytaal](/azure/kusto/query), de versie van de taal heeft enkele verschillen. In dit artikel geeft elementen die verschillen tussen de versie van de taal die wordt gebruikt voor de Data Explorer en de versie die wordt gebruikt voor Log Analytics-query's.

## <a name="data-explorer-elements-not-supported-in-log-analytics"></a>Data Explorer elementen niet ondersteund in Log Analytics
De volgende secties beschrijven van de Data Explorer-querytaal elementen die niet worden ondersteund door Log Analytics.

### <a name="statements-not-supported-in-log-analytics"></a>Instructies worden niet ondersteund in Log Analytics

* [Alias](/kusto/query/aliasstatement)
* [Queryparameters](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-log-analytics"></a>Niet ondersteunde functies in Log Analytics

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-log-analytics"></a>Operatoren niet ondersteund in Log Analytics

* [Cross-Cluster koppelen](/azure/kusto/query/joincrosscluster)
* [externaldata operator](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-log-analytics"></a>Invoegtoepassingen die niet wordt ondersteund in Log Analytics

* [sql_request-invoegtoepassing](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-log-analytics"></a>Aanvullende operatoren in Log Analytics
De volgende operatoren ondersteuning voor specifieke functies van de Log Analytics en zijn niet beschikbaar is buiten Log Analytics.

* [App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Volgende stappen

<<<<<<< HEAD:articles/log-analytics/query-language/data-explorer-difference.md
- Ophalen van verwijzingen naar andere [resources voor het schrijven van Log Analytics-query's](kusto.md).
- Toegang tot de volledige [verwijzen naar de documentatie voor Data Explorer-querytaal](/azure/kusto/query/).
=======
- Meer informatie over query's in [Log Analytics](../log-analytics-queries.md).
- Doorloop een les schrijven op een [Log Analytics-query](/log-analytics/query-language/get-started-queries.md).
- Toegang tot de volledige [referentiedocumentatie voor Kusto](/azure/kusto/query/).
>>>>>>> 4bccab5ecb17c887658a4d2ed1bab6b22bf29ffd:articles/log-Analytics/Query-Language/kusto.MD

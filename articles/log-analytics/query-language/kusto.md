---
title: Naslaggids voor Azure Monitor Log Analytics | Microsoft Docs
description: Referentie-informatie voor Kusto-taal die wordt gebruikt door Log Analytics. Bevat aanvullende elementen die specifiek zijn voor Log Analytics en elementen in Log Analytics-query's niet ondersteund.
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
ms.openlocfilehash: 5173790436a29fa9947346d711da1a2ddb32bf62
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451065"
---
# <a name="log-analytics-query-language-reference"></a>Meld u Analytics query language-referentie
[Meld u Analytics-query's](../log-analytics-queries.md) gebruik de dezelfde querytaal en -engine die worden gebruikt door [Azure Data Explorer](/azure/data-explorer/). U kunt toegang krijgen tot de taalverwijzing en andere details weer over de taal van de volgende locatie: [Kusto-taalverwijzing](/azure/kusto/query)



## <a name="kusto-elements-not-support-in-log-analytics"></a>Kusto-elementen worden niet ondersteund in Log Analytics
Log Analytics-query's gebruik van een implementatie van Kusto, maar er zijn enkele Kusto-elementen die biedt geen ondersteuning voor zoals beschreven in de volgende secties.

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
Ter ondersteuning van specifieke functies van de Log Analytics, zijn de volgende aanvullende Kusto-operators opgegeven die niet buiten Log Analytics beschikbaar zijn. 

* [App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over query's in [Log Analytics](../log-analytics-queries.md).
- Doorloop een les schrijven op een [Log Analytics-query](/log-analytics/query-language/get-started-queries.md).
- Toegang tot de volledige [referentiedocumentatie voor Kusto](/azure/kusto/query/).
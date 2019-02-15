---
title: Azure Monitor log-query language verschillen | Microsoft Docs
description: Referentie-informatie voor Kusto-query-taal die wordt gebruikt door Azure Monitor. Bevat aanvullende elementen die specifiek zijn voor Azure Monitor en elementen in Azure Monitor logboeken-query's niet ondersteund.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2018
ms.author: bwren
ms.openlocfilehash: 1185f3f96fd39f168d138d7dbf66e7780884b1fa
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56266727"
---
# <a name="azure-monitor-log-query-language-differences"></a>Azure Monitor log query language-verschillen

Terwijl [registreert in Azure Monitor](log-query-overview.md) is gebouwd op [Azure Data Explorer](/azure/data-explorer) en maakt gebruik van dezelfde [Kusto-querytaal](/azure/kusto/query), de versie van de taal heeft enkele verschillen. In dit artikel geeft elementen die verschillen tussen de versie van de taal die wordt gebruikt voor de Data Explorer en de versie die wordt gebruikt voor Azure Monitor logboeken-query's.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="kql-elements-not-supported-in-azure-monitor"></a>KQL elementen niet ondersteund in Azure Monitor
De volgende secties beschrijven van de querytaal Kusto-elementen die niet worden ondersteund door Azure Monitor.

### <a name="statements-not-supported-in-azure-monitor"></a>Instructies worden niet ondersteund in Azure Monitor

* [Alias](/azure/kusto/query/aliasstatement)
* [Queryparameters](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-azure-monitor"></a>Niet ondersteunde functies in Azure Monitor

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-azure-monitor"></a>Operatoren niet ondersteund in Azure Monitor

* [Cross-Cluster koppelen](/azure/kusto/query/joincrosscluster)
* [externaldata operator](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-azure-monitor"></a>Invoegtoepassingen die niet wordt ondersteund in Azure Monitor

* [sql_request-invoegtoepassing](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-azure-monitor"></a>Aanvullende operatoren in Azure Monitor
De volgende operatoren ondersteuning voor specifieke functies van Azure Monitor en zijn niet beschikbaar buiten Azure Monitor.

* [app()](app-expression.md)
* [workspace()](workspace-expression.md)

## <a name="next-steps"></a>Volgende stappen

- Ophalen van verwijzingen naar andere [resources voor het schrijven van Azure Monitor query's bijgehouden](query-language.md).
- Toegang tot de volledige [referentiedocumentatie voor Kusto-query-taal](/azure/kusto/query/).

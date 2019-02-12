---
title: Azure Monitor log-query language verschillen | Microsoft Docs
description: Naslaginformatie voor Data Explorer query-taal die wordt gebruikt door Azure Monitor. Bevat aanvullende elementen die specifiek zijn voor Azure Monitor en elementen in Azure Monitor logboeken-query's niet ondersteund.
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
ms.openlocfilehash: 9c58796fa19ffb6d38582c809f7bb6ca948bd92c
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56003620"
---
# <a name="azure-monitor-log-query-language-differences"></a>Azure Monitor log query language-verschillen

Terwijl [registreert in Azure Monitor](log-query-overview.md) is gebouwd op [Azure Data Explorer](/azure/data-explorer) en maakt gebruik van de [hetzelfde querytaal](/azure/kusto/query), de versie van de taal heeft enkele verschillen. In dit artikel geeft elementen die verschillen tussen de versie van de taal die wordt gebruikt voor de Data Explorer en de versie die wordt gebruikt voor Azure Monitor logboeken-query's.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="data-explorer-elements-not-supported-in-azure-monitor"></a>Data Explorer elementen niet ondersteund in Azure Monitor
De volgende secties worden de elementen van de Data Explorer-querytaal die niet worden ondersteund door Azure Monitor beschreven.

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
- Toegang tot de volledige [verwijzen naar de documentatie voor Data Explorer-querytaal](/azure/kusto/query/).

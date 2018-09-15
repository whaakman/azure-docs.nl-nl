---
title: de expressie Workspace() in Azure Log Analytics-query | Microsoft Docs
description: De werkruimte-expressie wordt gebruikt in een Log Analytics-query voor het ophalen van gegevens uit een specifieke werkruimte in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement.
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
ms.date: 09/10/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 13bf79c919eff220c1b6fbbc7247a44b2c424466
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45637525"
---
# <a name="workspace-expression-in-log-analytics-query"></a>de expressie Workspace() in Log Analytics-query

De `workspace` expressie wordt gebruikt in een Log Analytics-query voor het ophalen van gegevens uit een specifieke werkruimte in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement. Dit is handig om op te nemen van logboekgegevens in een Application Insights-query en om gegevens te doorzoeken in meerdere werkruimten in een logboekquery.


## <a name="syntax"></a>Syntaxis

`workspace(`*ID*`)`

## <a name="arguments"></a>Argumenten

- *Id*: identificeert de werkruimte met behulp van een van de indelingen in de onderstaande tabel.

| Id | Beschrijving | Voorbeeld
|:---|:---|:---|
| Resourcenaam | Menselijke leesbare naam van de werkruimte (AKA "naam van onderdeel") | Workspace("contosoretail") |
| Gekwalificeerde naam | Volledige naam van de werkruimte in de vorm: "resourceGroup-subscriptionName/componentName" | Workspace('Contoso/ContosoResource/ContosoWorkspace') |
| Id | GUID van de werkruimte | Workspace("b438b3f6-912a-46d5-9db1-b42069242ab4") |
| Azure-Resource-ID | ID voor de Azure-resource | Workspace("/Subscriptions/e4227-645-44e-9c67-3b84b5982/ResourceGroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/Workspaces/contosoretail") |


## <a name="notes"></a>Opmerkingen

* U moet leestoegang hebben tot de werkruimte.
* Een bijbehorende expressie is `app` waarmee u query's uitvoeren voor Application Insights-toepassingen.

## <a name="examples"></a>Voorbeelden

```
workspace("contosoretail").Update | count
```
```
workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count
```
```
workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
```
```
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Volgende stappen

- Zie de [app expressie](workspace-expression.md) om te verwijzen naar Application Insights-app.
- Meer informatie over hoe u [Log Analytics-gegevens](../../log-analytics/log-analytics-log-search.md) is opgeslagen.
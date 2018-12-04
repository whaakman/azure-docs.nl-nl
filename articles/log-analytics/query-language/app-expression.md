---
title: de expressie App() in Azure Log Analytics-query | Microsoft Docs
description: De app-expressie wordt gebruikt in een Log Analytics-query voor het ophalen van gegevens uit een specifieke Application Insights-app in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement.
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
ms.openlocfilehash: 84d1a08cd8ec4aafcc382ca74987bc5486f673d7
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849748"
---
# <a name="app-expression-in-log-analytics-query"></a>de expressie App() in Log Analytics-query

De `app` expressie wordt gebruikt in een Log Analytics-query voor het ophalen van gegevens uit een specifieke Application Insights-app in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement. Dit is handig om op te nemen van toepassingsgegevens in een Log Analytics-query en om gegevens te doorzoeken voor meerdere toepassingen in een Application Insights-query.



## <a name="syntax"></a>Syntaxis

`app(`*ID*`)`


## <a name="arguments"></a>Argumenten

- *Id*: de app met behulp van een van de indelingen in de onderstaande tabel wordt geïdentificeerd.

| Id | Beschrijving | Voorbeeld
|:---|:---|:---|
| Resourcenaam | Menselijke leesbare naam van de app (AKA "naam van onderdeel") | App("fabrikamapp") |
| Gekwalificeerde naam | Volledige naam van de app in het formulier: "resourceGroup-subscriptionName/componentName" | App('AI-prototype/Fabrikam/fabrikamapp') |
| Id | GUID van de app. | App("988ba129-363e-4415-8fe7-8cbab5447518") |
| Azure-Resource-ID | ID voor de Azure-resource |App("/Subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/ResourceGroups/Fabrikam/providers/Microsoft.Insights/Components/fabrikamapp") |


## <a name="notes"></a>Opmerkingen

* U moet leestoegang tot de toepassing hebben.
* Identificeren van een toepassing door de naam ervan, wordt ervan uitgegaan dat het is uniek voor alle toegankelijke abonnementen. Als u meerdere toepassingen met de opgegeven naam hebt, wordt de query mislukt vanwege een dubbelzinnigheid. In dit geval moet u een van de andere id's gebruiken.
* Gebruik de bijbehorende expressie [werkruimte](workspace-expression.md) naar query uitvoeren in Log Analytics-werkruimten.

## <a name="examples"></a>Voorbeelden

```Kusto
app("fabrikamapp").requests | count
```
```Kusto
app("AI-Prototype/Fabrikam/fabrikamapp").requests | count
```
```Kusto
app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count
```
```Kusto
app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Volgende stappen

- Zie de [werkruimte expressie](workspace-expression.md) om te verwijzen naar Log Analytics-werkruimte.
- Meer informatie over hoe u [Log Analytics-gegevens](../../azure-monitor/log-query/log-query-overview.md) is opgeslagen.
---
title: de expressie App() in Logboeken-query's van Azure Monitor | Microsoft Docs
description: De app-expressie wordt gebruikt in een query voor Azure Monitor voor het ophalen van gegevens uit een specifieke Application Insights-app in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement.
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
ms.date: 01/25/2019
ms.author: bwren
ms.openlocfilehash: db92db315875348ff05ec3a34b1a9fa946479c27
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55998532"
---
# <a name="app-expression-in-azure-monitor-query"></a>app() expression in Azure Monitor query

De `app` expressie in een Azure Monitor-query wordt gebruikt voor het ophalen van gegevens uit een specifieke Application Insights-app in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement. Dit is handig om op te nemen van toepassingsgegevens in een query voor Azure Monitor en om gegevens te doorzoeken voor meerdere toepassingen in een Application Insights-query.



## <a name="syntax"></a>Syntaxis

`app(`*Identifier*`)`


## <a name="arguments"></a>Argumenten

- *Id*: Hiermee geeft u de app met een van de indelingen in de onderstaande tabel.

| Id | Description | Voorbeeld
|:---|:---|:---|
| Resourcenaam | Menselijke leesbare naam van de app (AKA "naam van onderdeel") | app("fabrikamapp") |
| Gekwalificeerde naam | Volledige naam van de app in het formulier: "resourceGroup-subscriptionName/componentName" | app('AI-Prototype/Fabrikam/fabrikamapp') |
| Id | GUID van de app. | App("988ba129-363e-4415-8fe7-8cbab5447518") |
| Azure-Resource-ID | ID voor de Azure-resource |app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp") |


## <a name="notes"></a>Opmerkingen

* U moet leestoegang tot de toepassing hebben.
* Identificeren van een toepassing door de naam ervan, wordt ervan uitgegaan dat het is uniek voor alle toegankelijke abonnementen. Als u meerdere toepassingen met de opgegeven naam hebt, wordt de query mislukt vanwege een dubbelzinnigheid. In dit geval moet u een van de andere id's gebruiken.
* Gebruik de bijbehorende expressie [werkruimte](workspace-expression.md) naar query uitvoeren in Log Analytics-werkruimten.
* De expressie app() wordt momenteel niet ondersteund in de zoekquery bij het gebruik van de Azure-portal maken een [aangepaste log search waarschuwingsregel](../platform/alerts-log.md), tenzij een Application Insights-toepassing wordt gebruikt als de resource voor de waarschuwingsregel.

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

- Zie de [werkruimte expressie](workspace-expression.md) om te verwijzen naar een Log Analytics-werkruimte.
- Meer informatie over hoe u [Azure Monitor gegevens](../../azure-monitor/log-query/log-query-overview.md) is opgeslagen.
- Toegang tot volledige documentatie voor de [Data Explorer-querytaal](/azure/kusto/query/).
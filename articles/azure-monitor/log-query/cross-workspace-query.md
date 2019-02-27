---
title: Query voor resources met Azure Monitor | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt een query naar resources van meerdere werkruimten en Application Insights-app in uw abonnement.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: magoedte
ms.openlocfilehash: d69ddcd44fd947f3d1dc61ac960e7b55258c163e
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56872009"
---
# <a name="perform-cross-resource-log-queries-in-azure-monitor"></a>Meerdere bronnen logboeken-query's uitvoeren in Azure Monitor  

Eerder met Azure Monitor, u kunt gegevens alleen analyseren in de huidige werkruimte en deze kunt gebruiken om query's beperkt in meerdere werkruimten die zijn gedefinieerd in uw abonnement.  Bovendien kunt u alleen telemetrie-items die worden verzameld van uw webtoepassing met Application Insights rechtstreeks in Application Insights of vanuit Visual Studio zoeken.  Dit ook dat het een uitdaging voor het systeemeigen analyseren van operationele en toepassingsgegevens samen.   

U kunt nu een query niet alleen over meerdere Log Analytics-werkruimten, maar ook gegevens uit een specifieke Application Insights-app in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement. Dit biedt u een systeembreed overzicht van uw gegevens.  U kunt alleen uitvoeren met deze typen query's in [Log Analytics](portals.md).

## <a name="cross-resource-query-limits"></a>Limieten voor meerdere bronnen query 

* Het nummer van Application Insights-resources die u in één query opnemen kunt is beperkt tot 100.
* Meerdere bronnen query wordt niet ondersteund in de ontwerper. U kunt een query in Log Analytics maken en vastmaken aan Azure-dashboard en [een logboekzoekopdracht visualiseren](../../azure-monitor/learn/tutorial-logs-dashboards.md#visualize-a-log-search). 
* De query meerdere bronnen in de waarschuwingen wordt ondersteund in de nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Azure Monitor gebruikt standaard de [verouderde Log Analytics-waarschuwing API](../platform/api-alerts.md) voor het maken van nieuwe logboek waarschuwingsregels van Azure-portal, tenzij u van overschakelt [verouderde Log waarschuwingen API](../platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api). Na de switch, de nieuwe API wordt de standaardwaarde voor nieuwe regels voor waarschuwingen in Azure portal en u kunt meerdere bronnen query log regels waarschuwingen maken. U kunt meerdere bronnen querylogboek waarschuwingsregels maken zonder de overstap maken met behulp van de [ARM-sjabloon voor API scheduledQueryRules](../platform/alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template) –, maar deze waarschuwingsregel wordt al beheerd [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) en niet vanuit Azure portal.


## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Uitvoeren van query's in Log Analytics-werkruimten en Application Insights
Om te verwijzen naar een andere werkruimte in uw query, gebruikt u de [ *werkruimte* ](https://docs.microsoft.com/azure/log-analytics/query-language/workspace-expression) -id, en voor een app uit de Application Insights, gebruikt u de [ *app* ](https://docs.microsoft.com/azure/log-analytics/query-language/app-expression)id.  

### <a name="identifying-workspace-resources"></a>Werkruimteresources identificeren
De volgende voorbeelden ziet u query's in Log Analytics-werkruimten om terug te keren samengevatte tellingen van Logboeken van de tabel bijwerken in een werkruimte met de naam *contosoretail it*. 

Identificeren van een werkruimte kan worden uitgevoerd op verschillende manieren:

* De resourcenaam van de - is een leesbare naam van de werkruimte, ook wel genoemd *onderdeelnaam*. 

    `workspace("contosoretail-it").Update | count`
 
    >[!NOTE]
    >Identificeren van een werkruimte met de naam wordt ervan uitgegaan dat uniekheid voor alle toegankelijke abonnementen. Hebt u meerdere toepassingen met de opgegeven naam, de query is mislukt vanwege een dubbelzinnigheid. In dit geval moet u een van de andere id.

* Gekwalificeerde naam - is de 'volledige naam' van de werkruimte, die bestaat uit de naam van abonnement, resourcegroep en Onderdeelnaam in deze indeling: *resourceGroup-subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/contosoretail-it').Update | count `

    >[!NOTE]
    >Omdat Azure-abonnementnamen zijn niet uniek is, is het mogelijk dat deze id niet eenduidig. 
    >

* Werkruimte-ID - een werkruimte-ID is de unieke, onveranderbare, id toegewezen aan elke werkruimte weergegeven als een globally unique identifier (GUID).

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* Azure Resource-ID: de Azure gedefinieerde unieke id van de werkruimte. U gebruikt de Resource-ID als naam van de resource niet eenduidig is.  Voor werkruimten, de indeling is: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. OperationalInsights/werkruimten/componentName*.  

    Bijvoorbeeld:
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail-it").Update | count
    ```

### <a name="identifying-an-application"></a>Een toepassing te identificeren
Retourneren samengevatte aantal aanvragen voor een app met de naam van de volgende voorbeelden *fabrikamapp* in Application Insights. 

Identificeren van een toepassing in Application Insights kan worden bewerkstelligd met de *app(Identifier)* expressie.  De *id* argument geeft u de app met een van de volgende:

* De resourcenaam van de - is een mens leesbaar naam van de app, soms aangeduid als de *onderdeelnaam*.  

    `app("fabrikamapp")`

* Gekwalificeerde naam - is de 'volledige naam' van de app, die bestaat uit de naam van abonnement, resourcegroep en Onderdeelnaam in deze indeling: *resourceGroup-subscriptionName/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Omdat Azure-abonnementnamen zijn niet uniek is, is het mogelijk dat deze id niet eenduidig. 
    >

* ID - de app-GUID van de toepassing.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* Azure Resource-ID - de Azure gedefinieerde unieke id van de app. U gebruikt de Resource-ID als naam van de resource niet eenduidig is. De indeling is: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. OperationalInsights/onderdelen/componentName*.  

    Bijvoorbeeld:
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

### <a name="performing-a-query-across-multiple-resources"></a>Een query uitvoeren op meerdere resources
U kunt meerdere resources een query uit een van de resource-exemplaren, deze werkruimten en apps kunnen worden gecombineerd.
    
Voorbeeld voor de query in twee werkruimten:    

```
union Update, workspace("contosoretail-it").Update, workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update
| where TimeGenerated >= ago(1h)
| where UpdateState == "Needed"
| summarize dcount(Computer) by Classification
```

## <a name="using-cross-resource-query-for-multiple-resources"></a>Met behulp van meerdere bronnen query voor meerdere bronnen
Als u meerdere bronnen query's voor het correleren van gegevens uit meerdere Log Analytics-werkruimten en Application Insights-resources, wordt de query kan worden complex en moeilijk te onderhouden. U moet gebruikmaken van [functies in Azure Monitor query's bijgehouden](functions.md) het scheiden van de querylogica van het bereik van de query-resources, waardoor de querystructuur eenvoudiger. Het volgende voorbeeld ziet u hoe u kunt meerdere Application Insights-resources controleren en visualiseren van het aantal mislukte aanvragen met de toepassingsnaam. 

Een query als volgt die verwijst naar het bereik van Application Insights-resources maken. De `withsource= SourceApp` opdracht voegt een kolom die de toepassingsnaam Hiermee wordt aangegeven dat het logboek verzonden. [De query opslaan als functie](functions.md#create-a-function) met de alias _applicationsScoping_.

```Kusto
// crossResource function that scopes my Application Insights resources
union withsource= SourceApp
app('Contoso-app1').requests, 
app('Contoso-app2').requests,
app('Contoso-app3').requests,
app('Contoso-app4').requests,
app('Contoso-app5').requests
```



U kunt nu [Gebruik deze functie](../../azure-monitor/log-query/functions.md#use-a-function) in een query meerdere bronnen, zoals hieronder. De functiealias _applicationsScoping_ retourneert de samenvoeging van de tabel ' requests ' van de gedefinieerde toepassingen. De query en filters voor mislukte aanvragen en de trends visualiseert door toepassing. De _parseren_ operator is optioneel in dit voorbeeld. Pakt deze de naam van de toepassing van _SourceApp_ eigenschap.

```Kusto
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| render timechart
```
![tijdgrafiek](media/cross-workspace-query/chart.png)

## <a name="next-steps"></a>Volgende stappen

- Beoordeling [analyseren logboekgegevens in Azure Monitor](log-query-overview.md) voor een overzicht van Logboeken-query's en de structuur van logboekgegevens van Azure Monitor.
- Beoordeling [logboeken-query's van Azure Monitor](query-language.md) om alle resources voor Azure Monitor logboeken-query's weer te geven.

---
title: Lever een geïntegreerde ervaring meerdere Azure Monitor Application Insights-resources | Microsoft Docs
description: Dit artikel bevat informatie over het gebruik van een functie in Azure controleren logboeken naar meerdere Application Insights-resources opvragen en visualiseren van gegevens.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: magoedte
ms.openlocfilehash: 3f3de81197b05d4f025a3fd8638cffe4b07cecad
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429513"
---
# <a name="unify-multiple-azure-monitor-application-insights-resources"></a>Lever een geïntegreerde ervaring meerdere Azure Monitor Application Insights-resources 
In dit artikel wordt beschreven hoe u query's uitvoeren en Bekijk alle uw Application Insights toepassing logboekgegevens op één plek, zelfs wanneer ze zich in verschillende Azure-abonnementen, als vervanging voor de afschaffing van de Application Insights-Connector. Het aantal resources Application Insights-resources die u in één query opnemen kunt is beperkt tot 100.  

## <a name="recommended-approach-to-query-multiple-application-insights-resources"></a>Aanbevolen benadering query uitvoeren op meerdere Application Insights-resources 
Aanbieding meerdere Application Insights-resources in een query kan worden omslachtig en moeilijk te onderhouden. U kunt in plaats daarvan gebruikmaken van functie zodat deze de querylogica scheiden van de toepassingen scopes.  

In dit voorbeeld laat zien hoe u kunt meerdere Application Insights-resources controleren en visualiseren van het aantal mislukte aanvragen met de toepassingsnaam. Voordat u begint, moet u deze query uitvoeren in de werkruimte die is verbonden met Application Insights-resources om de lijst met verbonden toepassingen: 

```
ApplicationInsights
| summarize by ApplicationName
```

Maak een functie met behulp van de operator union met de lijst met toepassingen, en sla de query in uw werkruimte als functie met de alias *applicationsScoping*.  

```
union withsource=SourceApp 
app('Contoso-app1').requests,  
app('Contoso-app2').requests, 
app('Contoso-app3').requests, 
app('Contoso-app4').requests, 
app('Contoso-app5').requests 
| parse SourceApp with * "('" applicationName "')" *  
```

>[!NOTE]
>U kunt de vermelde toepassingen op elk gewenst moment in de portal wijzigen door te gaan naar Query explorer in uw werkruimte en het selecteren van de functie voor het bewerken en vervolgens op te slaan of met behulp van de `SavedSearch` PowerShell-cmdlet. De `withsource= SourceApp` opdracht voegt een kolom aan de resultaten die de toepassing Hiermee wordt aangegeven dat het logboek verzonden. 
>
>De query gebruikt Application Insights-schema, maar de query wordt uitgevoerd in de werkruimte, omdat de functie applicationsScoping de gegevensstructuur van Application Insights retourneert. 
>
>De operator parseren is optioneel in dit voorbeeld, naam van de toepassing van de eigenschap SourceApp worden uitgepakt. 

U bent nu klaar voor gebruik van applicationsScoping-functie in de query meerdere bronnen:  

```
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| render timechart
```

De functiealias retourneert de samenvoeging van de aanvragen van de gedefinieerde toepassingen. De query en filters voor mislukte aanvragen en de trends visualiseert door toepassing.

![Voorbeeld van de resultaten van cross-query](media/unify-app-resource-data/app-insights-query-results.png)

## <a name="query-across-application-insights-resources-and-workspace-data"></a>Query's uitvoeren voor Application Insights-resources en werkruimte gegevens 
Wanneer u stopt de Connector en moet u query's uitvoeren op een tijdsbereik dat is verkleind door Application Insights-Gegevensretentie (90 dagen), moet u uitvoeren [query's voor meerdere bronnen](../../azure-monitor/log-query/cross-workspace-query.md) in de werkruimte en het Application Insights resources voor een tussenliggende periode. Dit is totdat de gegevens van uw toepassingen worden bij elkaar opgeteld per de nieuwe Application Insights-gegevens bewaren die hierboven worden vermeld. De query is bepaalde bewerkingen vereist omdat de schema's in de werkruimte en Application Insights verschillend zijn. Zie de tabel verderop in deze sectie de schemaverschillen markeren. 

>[!NOTE]
>[Meerdere bronnen query](../log-query/cross-workspace-query.md) in logboek waarschuwingen wordt ondersteund in de nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Azure Monitor gebruikt standaard de [verouderde Log Analytics-waarschuwing API](../platform/api-alerts.md) voor het maken van nieuwe logboek waarschuwingsregels van Azure-portal, tenzij u van overschakelt [verouderde Log waarschuwingen API](../platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api). Na de switch, de nieuwe API wordt de standaardwaarde voor nieuwe regels voor waarschuwingen in Azure portal en u kunt meerdere bronnen query log regels waarschuwingen maken. U kunt maken [meerdere bronnen query](../log-query/cross-workspace-query.md) melden van regels voor waarschuwingen zonder dat de switch met behulp van de [ARM-sjabloon voor scheduledQueryRules API](../platform/alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template) –, maar deze waarschuwingsregel wordt al beheerd [ scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) en niet vanuit Azure portal.

Bijvoorbeeld, als de connector is gestopt op 2018-11-01, werken wanneer u een query naar Logboeken in Application Insights-bronnen en toepassingen van gegevens in de werkruimte, kan de query zou worden samengesteld als in het volgende voorbeeld:

```
applicationsScoping //this brings data from Application Insights resources 
| where timestamp between (datetime("2018-11-01") .. now()) 
| where success == 'False' 
| where duration > 1000 
| union ( 
    ApplicationInsights //this is Application Insights data in Log Analytics workspace 
    | where TimeGenerated < (datetime("2018-12-01") 
    | where RequestSuccess == 'False' 
    | where RequestDuration > 1000 
    | extend duration = RequestDuration //align to Application Insights schema 
    | extend timestamp = TimeGenerated //align to Application Insights schema 
    | extend name = RequestName //align to Application Insights schema 
    | extend resultCode = ResponseCode //align to Application Insights schema 
    | project-away RequestDuration , RequestName , ResponseCode , TimeGenerated 
) 
| project timestamp , duration , name , resultCode 
```

## <a name="application-insights-and-log-analytics-workspace-schema-differences"></a>Schemaverschillen voor Application Insights en Log Analytics-werkruimte
De volgende tabel ziet u de schemaverschillen tussen Log Analytics en Application Insights.  

| Meld u eigenschappen van de Analytics-werkruimte| Application Insights-resource-eigenschappen|
|------------|------------| 
| AnonUserId | user_id|
| ApplicationId | appId|
| ApplicationName | appName|
| ApplicationTypeVersion | application_Version |
| AvailabilityCount | itemCount |
| AvailabilityDuration | duur |
| AvailabilityMessage | message |
| AvailabilityRunLocation | location |
| AvailabilityTestId | id |
| AvailabilityTestName | naam |
| AvailabilityTimestamp | tijdstempel |
| Browser | client_browser |
| Plaats | client_city |
| ClientIP | client_IP |
| Computer | cloud_RoleInstance | 
| Land/regio | client_CountryOrRegion | 
| CustomEventCount | itemCount | 
| CustomEventDimensions | customDimensions |
| CustomEventName | naam | 
| DeviceModel | client_Model | 
| Apparaattype | client_Type | 
| ExceptionCount | itemCount | 
| ExceptionHandledAt | handledAt |
| ExceptionMessage | message | 
| ExceptionType | type |
| Bewerkings-id | operation_id |
| OperationName | operation_Name | 
| OS | client_OS | 
| PageViewCount | itemCount |
| PageViewDuration | duur | 
| PageViewName | naam | 
| ParentOperationID | operation_Id | 
| RequestCount | itemCount | 
| RequestDuration | duur | 
| Aanvraag-id | id | 
| RequestName | naam | 
| RequestSuccess | voltooid | 
| ResponseCode | resultCode | 
| Rol | cloud_RoleName |
| RoleInstance | cloud_RoleInstance |
| sessie-id | type session_Id | 
| SourceSystem | operation_SyntheticSource |
| TelemetryTYpe | type |
| URL | _url |
| UserAccountId | user_AccountId |

## <a name="next-steps"></a>Volgende stappen

Gebruik [zoeken in logboeken](../../azure-monitor/log-query/log-query-overview.md) om gedetailleerde informatie voor uw Application Insights-apps weer te geven.

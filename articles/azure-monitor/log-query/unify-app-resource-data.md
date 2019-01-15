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
ms.date: 01/10/2019
ms.author: magoedte
ms.openlocfilehash: e3b118306b5a139ba31029bc6191368690b36666
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265206"
---
# <a name="unify-multiple-azure-monitor-application-insights-resources"></a>Lever een geïntegreerde ervaring meerdere Azure Monitor Application Insights-resources 
In dit artikel wordt beschreven hoe u query's uitvoeren en Bekijk alle uw Application Insights toepassing logboekgegevens op één plek, zelfs wanneer ze zich in verschillende Azure-abonnementen, als vervanging voor de afschaffing van de Application Insights-Connector.  

## <a name="recommended-approach-to-query-multiple-application-insights-resources"></a>Aanbevolen benadering query uitvoeren op meerdere Application Insights-resources 
Aanbieding meerdere Application Insights-resources in een query kan worden omslachtig en moeilijk te onderhouden. U kunt in plaats daarvan gebruikmaken van functie zodat deze de querylogica scheiden van de toepassingen scopes.  

In dit voorbeeld laat zien hoe u kunt meerdere Application Insights-resources controleren en visualiseren van het aantal mislukte aanvragen met de toepassingsnaam. Voordat u begint, moet u deze query uitvoeren in de werkruimte die is verbonden met Application Insights-resources om de lijst met verbonden toepassingen: 

```
ApplicationInsights
| summarize by ApplicationName
```

Maak een functie met behulp van de operator union met de lijst met toepassingen, en sla de query als functie met de alias *applicationsScoping*.  

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
>U kunt de vermelde toepassingen op elk gewenst moment wijzigen door te navigeren naar Query explorer in de portal van Logboeken en de functie wordt bewerkt of met behulp van de `SavedSearch` PowerShell-cmdlet. De `withsource= SourceApp` opdracht voegt een kolom aan de resultaten die de toepassing Hiermee wordt aangegeven dat het logboek verzonden. 
>
>De query gebruikt Application Insights-schema, maar de query wordt uitgevoerd in de werkruimte, omdat de functie applicationsScoping de gegevensstructuur van Application Insights retourneert. 
>
>De operator parseren is optioneel in dit voorbeeld, naam van de toepassing van de eigenschap SourceApp worden uitgepakt. 

U bent nu klaar voor gebruik van applicationsScoping-functie in de query meerdere bronnen. De functiealias retourneert de samenvoeging van de aanvragen van de gedefinieerde toepassingen. De query en filters voor mislukte aanvragen en de trends visualiseert door toepassing. ![Voorbeeld van de resultaten van cross-query](media/unify-app-resource-data/app-insights-query-results.png)

## <a name="query-across-application-insights-resources-and-workspace-data"></a>Query's uitvoeren voor Application Insights-resources en werkruimte gegevens 
Wanneer u stopt de Connector en moet u query's uitvoeren op een tijdsbereik dat is verkleind door Application Insights-Gegevensretentie (90 dagen), moet u uitvoeren [query's voor meerdere bronnen](../../azure-monitor/log-query/cross-workspace-query.md) in de werkruimte en het Application Insights resources voor een tussenliggende periode. Dit is totdat de gegevens van uw toepassingen worden bij elkaar opgeteld per de nieuwe Application Insights-gegevens bewaren die hierboven worden vermeld. De query is bepaalde bewerkingen vereist omdat de schema's in de werkruimte en Application Insights verschillend zijn. Zie de tabel verderop in deze sectie de schemaverschillen markeren. 

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
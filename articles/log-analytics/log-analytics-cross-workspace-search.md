---
title: Zoeken in resources met Azure Log Analytics | Microsoft Docs
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
ms.devlang: na
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 75116e0ba50c3f195d528d33822af0c446acd5fe
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42058751"
---
# <a name="perform-cross-resource-log-searches-in-log-analytics"></a>Uitvoeren van meerdere bronnen zoekopdrachten in Logboeken in Log Analytics  

Eerder met Azure Log Analytics, u kunt gegevens alleen analyseren in de huidige werkruimte en deze kunt gebruiken om query's beperkt in meerdere werkruimten die zijn gedefinieerd in uw abonnement.  Bovendien kunt u alleen telemetrie-items die worden verzameld van uw webtoepassing met Application Insights rechtstreeks in Application Insights of vanuit Visual Studio zoeken.  Dit ook dat het een uitdaging voor het systeemeigen analyseren van operationele en toepassingsgegevens samen.   

U kunt nu een query niet alleen over meerdere Log Analytics-werkruimten, maar ook gegevens uit een specifieke Application Insights-app in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement. Dit biedt u een systeembreed overzicht van uw gegevens.  U kunt alleen uitvoeren met deze typen query's in de [Log Analytics-pagina (preview)](log-analytics-log-search-portals.md#log-analytics-page-preview), dus niet in de Azure-portal. Het aantal resources (Log Analytics-werkruimten en Application Insights-app) die u in één query opnemen kunt is beperkt tot 100. 

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Uitvoeren van query's in Log Analytics-werkruimten en Application Insights
Om te verwijzen naar een andere werkruimte in uw query, gebruikt u de [ *werkruimte* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) -id, en voor een app uit de Application Insights, gebruikt u de [ *app* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())id.  

### <a name="identifying-workspace-resources"></a>Werkruimteresources identificeren
De volgende voorbeelden ziet u query's in Log Analytics-werkruimten om terug te keren samengevatte tellingen van Logboeken van de tabel bijwerken in een werkruimte met de naam *contosoretail it*. 

Identificeren van een werkruimte kan worden uitgevoerd op verschillende manieren:

* De resourcenaam van de - is een leesbare naam van de werkruimte, ook wel genoemd *onderdeelnaam*. 

    `workspace("contosoretail").Update | count`
 
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
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Update | count
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

## <a name="next-steps"></a>Volgende stappen

Controleer de [Log Analytics melden verwijzing naar de](https://docs.loganalytics.io/docs/Language-Reference) om alle van de query-syntaxis opties die beschikbaar zijn in Log Analytics weer te geven.    

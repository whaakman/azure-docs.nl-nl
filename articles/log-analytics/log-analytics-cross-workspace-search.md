---
title: Uitvoeren van query's via Azure Log Analytics-werkruimten | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt een query op meerdere werkruimten en specifieke App Insights-app in uw abonnement.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: magoedte
ms.openlocfilehash: 403448995c28ff7172d2c3abbf3b9d67341017b4
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-perform-queries-across-multiple-log-analytics-workspaces"></a>Het uitvoeren van query's via meerdere Log Analytics-werkruimten

Eerder met Azure Log Analytics alleen analyseren gegevens vanuit de huidige werkruimte en het beperkt uw query over meerdere werkruimten die zijn gedefinieerd in uw abonnement.  

U kunt nu een query niet alleen over meerdere Log Analytics-werkruimten, maar ook gegevens uit een specifieke Application Insights-app in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement. Dit biedt u een weergave van het hele systeem van uw gegevens.  U kunt alleen uitvoeren met dit type query in de [geavanceerde portal](log-analytics-log-search-portals.md#advanced-analytics-portal), niet in de Azure-portal.  

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Uitvoeren van query's over Log Analytics-werkruimten en van Application Insights
Om te verwijzen naar een andere werkruimte in de query, gebruiken de [ *werkruimte* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) -id, en gebruik voor een app in Application Insights de [ *app* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())id.  

De eerste query retourneert bijvoorbeeld samengevatte tellingen van die nodig zijn voor hun classificatie van de tabel voor het bijwerken van de huidige werkruimte en een andere werkruimte met de naam *contosoretail it*.  Het tweede voorbeeld in de query retourneert het samengevatte aantal aanvragen op basis van een app met de naam *fabrikamapp* in Application Insights. 

### <a name="identifying-workspace-resources"></a>Identificeren van bronnen van de werkruimte
Identificeren van een werkruimte worden uitgevoerd verschillende manieren:

* Resourcenaam - is een leesbare naam van de werkruimte wordt soms aangeduid als *onderdeelnaam*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Een werkruimte identificeren door de naam ervan wordt ervan uitgegaan dat deze uniek is voor alle toegankelijke abonnementen. Als er meerdere toepassingen met de opgegeven naam, de query is mislukt vanwege de dubbelzinnigheid. In dit geval moet u een van de andere id.

* Gekwalificeerde naam - is de 'volledige naam' van de werkruimte bestaat uit de naam van abonnement, resourcegroep en Onderdeelnaam in deze indeling: *resourceGroup-subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Omdat Azure-abonnementnamen niet uniek zijn, kan deze id dubbelzinnig zijn. 
    >

* werkruimte-ID - werkruimte-ID is de unieke, niet-wijzigbaar, id toegewezen aan elke werkruimte weergegeven als een globally unique identifier (GUID).

    `workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count`

* Azure Resource-ID: de Azure gedefinieerde unieke id van de werkruimte. U gebruikt de Resource-ID als de resourcenaam niet eenduidig is.  Voor werkruimten, de indeling is: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Werkruimten-OperationalInsights/componentName*.  

    Bijvoorbeeld:
    ``` 
    workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

### <a name="identifying-an-application"></a>Een toepassing te identificeren

Identificeren van een toepassing in Application Insights kan worden uitgevoerd met de *app(Identifier)* expressie.  De *id* argument Hiermee geeft u de app met een van de volgende:

* Resourcenaam - is een menselijke leesbare naam van de app, ook wel de *onderdeelnaam*.  

    `app("fabrikamapp")`

* Gekwalificeerde naam - is de 'volledige naam' van de app, bestaat uit de naam van abonnement, resourcegroep en Onderdeelnaam in deze indeling: *resourceGroup-subscriptionName/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Omdat Azure-abonnementnamen niet uniek zijn, kan deze id dubbelzinnig zijn. 
    >

* ID - de app-GUID van de toepassing.

    `app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count`

* Azure Resource-ID - de Azure gedefinieerde unieke id van de app. U gebruikt de Resource-ID als de resourcenaam niet eenduidig is. De indeling is: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Onderdelen-OperationalInsights/componentName*.  

    Bijvoorbeeld:
    ```
    app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

## <a name="next-steps"></a>Volgende stappen

Controleren de [logboekanalyse Meld zoeken verwijzing](https://docs.loganalytics.io/docs/Language-Reference) alle van de query syntaxis van de beschikbare opties in logboekanalyse bekijken.    
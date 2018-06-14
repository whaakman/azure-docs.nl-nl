---
title: Zoeken in resources met Azure Log Analytics | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt een query op basis van bronnen van meerdere werkruimten en de App Insights-app in uw abonnement.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: magoedte
ms.openlocfilehash: e46ae3af3a718703f9e1d6b847b2342469bf3a1e
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
ms.locfileid: "31517220"
---
# <a name="perform-cross-resource-log-searches-in-log-analytics"></a>Cross-resource logboek zoekopdrachten uit te voeren in Log Analytics  

Eerder met Azure Log Analytics alleen analyseren gegevens vanuit de huidige werkruimte en het beperkt uw query over meerdere werkruimten die zijn gedefinieerd in uw abonnement.  Bovendien kunt u alleen telemetrie-items die zijn verzameld wordt van uw webtoepassing met Application Insights rechtstreeks in Application Insights of vanuit Visual Studio zoeken.  Dit ook dat het een challenge systeemeigen analyseren operationele en toepassingsgegevens samen.   

U kunt nu een query niet alleen over meerdere Log Analytics-werkruimten, maar ook gegevens uit een specifieke Application Insights-app in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement. Dit biedt u een weergave van het hele systeem van uw gegevens.  U kunt alleen uitvoeren met deze typen query's in de [geavanceerde portal](log-analytics-log-search-portals.md#advanced-analytics-portal), niet in de Azure-portal.  

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Uitvoeren van query's over Log Analytics-werkruimten en van Application Insights
Om te verwijzen naar een andere werkruimte in de query, gebruiken de [ *werkruimte* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) -id, en gebruik voor een app in Application Insights de [ *app* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())id.  

### <a name="identifying-workspace-resources"></a>Identificeren van bronnen van de werkruimte
De volgende voorbeelden ziet u query's over Log Analytics-werkruimten retourneren samengevatte tellingen van updates op de tabel bijwerken in een werkruimte met de naam *contosoretail it*. 

Een werkruimte te identificeren, kan de uitgevoerde één van de verschillende manieren zijn:

* Resourcenaam - is een leesbare naam van de werkruimte wordt soms aangeduid als *onderdeelnaam*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Een werkruimte identificeren met de naam wordt ervan uitgegaan dat uniekheid voor alle toegankelijke abonnementen. Als er meerdere toepassingen met de opgegeven naam, de query is mislukt vanwege de dubbelzinnigheid. In dit geval moet u een van de andere id.

* Gekwalificeerde naam - is de 'volledige naam' van de werkruimte bestaat uit de naam van abonnement, resourcegroep en Onderdeelnaam in deze indeling: *resourceGroup-subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Omdat Azure-abonnementnamen niet uniek zijn, kan deze id dubbelzinnig zijn. 
    >

* werkruimte-ID - werkruimte-ID is de unieke, niet-wijzigbaar, id toegewezen aan elke werkruimte weergegeven als een globally unique identifier (GUID).

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* Azure Resource-ID: de Azure gedefinieerde unieke id van de werkruimte. U gebruikt de Resource-ID als de resourcenaam niet eenduidig is.  Voor werkruimten, de indeling is: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Werkruimten-OperationalInsights/componentName*.  

    Bijvoorbeeld:
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

### <a name="identifying-an-application"></a>Een toepassing te identificeren
De volgende voorbeelden retourneren samengevatte aantal aanvragen op basis van een app met de naam *fabrikamapp* in Application Insights. 

Identificeren van een toepassing in Application Insights kan worden bewerkstelligd met de *app(Identifier)* expressie.  De *id* argument Hiermee geeft u de app met een van de volgende:

* Resourcenaam - is een menselijke leesbare naam van de app, ook wel de *onderdeelnaam*.  

    `app("fabrikamapp")`

* Gekwalificeerde naam - is de 'volledige naam' van de app, bestaat uit de naam van abonnement, resourcegroep en Onderdeelnaam in deze indeling: *resourceGroup-subscriptionName/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Omdat Azure-abonnementnamen niet uniek zijn, kan deze id dubbelzinnig zijn. 
    >

* ID - de app-GUID van de toepassing.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* Azure Resource-ID - de Azure gedefinieerde unieke id van de app. U gebruikt de Resource-ID als de resourcenaam niet eenduidig is. De indeling is: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Onderdelen-OperationalInsights/componentName*.  

    Bijvoorbeeld:
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

## <a name="next-steps"></a>Volgende stappen

Controleren de [logboekanalyse Meld zoeken verwijzing](https://docs.loganalytics.io/docs/Language-Reference) alle van de query syntaxis van de beschikbare opties in logboekanalyse bekijken.    
---
title: Uitvoeren van query's via Azure Log Analytics-werkruimten | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt een query op meerdere werkruimten in uw abonnement met voorbeelden te volgen.
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
ms.date: 10/04/2017
ms.author: magoedte
ms.openlocfilehash: 6b6dc6f472a87178c006301f4f692aeff15e257e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="perform-queries-across-multiple-log-analytics-workspaces"></a>Uitvoeren van query's via meerdere Log Analytics-werkruimten

Eerder met Azure Log Analytics analyseren alleen gegevens vanuit de huidige werkruimte en deze beperkt de mogelijkheid om te query over meerdere werkruimten die zijn gedefinieerd in uw abonnement.  

Nu u kunt een query op meerdere werkruimten, biedt een weergave van het hele systeem van uw gegevens.  U kunt alleen uitvoeren met dit type query in de [geavanceerde portal](log-analytics-log-search-portals.md#advanced-analytics-portal), niet in de Azure-portal.  

## <a name="querying-across-log-analytics-workspaces"></a>Uitvoeren van query's over Log Analytics-werkruimten
Om te verwijzen naar een andere werkruimte in de query, gebruiken de *werkruimte* id.  De volgende query retourneert bijvoorbeeld samengevatte tellingen van die nodig zijn voor hun classificatie van de tabel voor het bijwerken van de huidige werkruimte en een andere werkruimte met de naam *contosoretail it*.  


## <a name="identifying-resources"></a>Identificeren van bronnen
Identificeren van een werkruimte worden uitgevoerd verschillende manieren:

* Resourcenaam - is een leesbare naam van de werkruimte wordt soms aangeduid als *onderdeelnaam*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Een werkruimte identificeren door de naam ervan wordt ervan uitgegaan dat deze uniek is voor alle toegankelijke abonnementen. Als er meerdere toepassingen met de opgegeven naam, de query is mislukt vanwege de dubbelzinnigheid. In dit geval moet u een van de andere id's gebruiken.

* Gekwalificeerde naam - is 'volledige name' van de werkruimte bestaat uit de naam van het abonnement, resourcegroep en onderdeel name in deze indeling: *resourceGroup-subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Omdat Azure-abonnementnamen niet uniek zijn, kan deze id dubbelzinnig zijn. 
    >

* werkruimte-ID - werkruimte-ID is de unieke, niet-wijzigbaar, id toegewezen aan elke werkruimte weergegeven als een globally unique identifier (GUID).

    `workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count`

* Azure Resource-ID: de Azure gedefinieerde unieke id van de werkruimte. U gebruikt dit wanneer de resourcenaam niet eenduidig is.  Voor werkruimten, de indeling is: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Werkruimten-OperationalInsights/componentName*.  

    Bijvoorbeeld:
    ``` 
    `workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

## <a name="next-steps"></a>Volgende stappen

Controleren de [logboekanalyse Meld zoeken verwijzing](https://docs.loganalytics.io/docs/Language-Reference) alle van de query syntaxis van de beschikbare opties in logboekanalyse bekijken.    
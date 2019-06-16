---
title: Beheer de toegang tot Azure-resources met behulp van RBAC en de REST-API - Azure | Microsoft Docs
description: Informatie over het beheren van toegang tot Azure-resources voor gebruikers, groepen en toepassingen die gebruikmaken van op rollen gebaseerd toegangsbeheer (RBAC) en de REST-API. U vindt hier instructies voor het weergeven van toegang, het verlenen van toegang en het intrekken van toegang.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 3602e4ca83e828270ebef56c688670b896ca58a4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66472744"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-the-rest-api"></a>Beheer de toegang tot Azure-resources met behulp van RBAC en de REST-API

[Op rollen gebaseerd toegangsbeheer (RBAC)](overview.md) is de manier waarop u de toegang tot Azure-resources beheert. Dit artikel wordt beschreven hoe u de toegang voor gebruikers, groepen en toepassingen die gebruikmaken van RBAC en de REST-API beheren.

## <a name="list-access"></a>Toegang opvragen

In RBAC, de toegang van de lijst, u lijst maken met de roltoewijzingen. Lijst van roltoewijzingen, gebruikt u een van de [roltoewijzingen - lijst](/rest/api/authorization/roleassignments/list) REST-API's. Uw om resultaten te verfijnen, geeft u een bereik en een optioneel filter.

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. Vervang in de URI, *{bereik}* met het bereik waarvoor u wilt weergeven van de roltoewijzingen.

    | Scope | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |
    
       
     > [!NOTE]
     > In het bovenstaande voorbeeld dat Microsoft.web is dat de resourceprovider wordt gebruikt die verwijst naar App service-exemplaar. Op dezelfde manier kunt u gebruikmaken van elke andere resourceprovider en bouwen van de Scope-URI. Om te begrijpen Raadpleeg voor meer [Azure resourceproviders en typen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services) en ondersteunde [Azure RM-resourceproviderbewerkingen](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations).  
     
1. Vervang *{filter}* met de voorwaarde die u wilt toepassen op filter de lijst met toewijzingen van de rol.

    | Filteren | Description |
    | --- | --- |
    | `$filter=atScope()` | Geeft een lijst van de roltoewijzing voor alleen het opgegeven bereik niet inclusief de roltoewijzingen weergegeven bij subscopes. |
    | `$filter=principalId%20eq%20'{objectId}'` | Geeft een lijst van roltoewijzingen voor een opgegeven gebruiker, groep of service-principal. |
    | `$filter=assignedTo('{objectId}')` | Geeft een lijst van roltoewijzingen voor een opgegeven gebruiker of service-principal. Als de gebruiker lid is van een groep waarvoor een roltoewijzing is, wordt deze roltoewijzing ook weergegeven. Dit filter is transitieve voor groepen, wat betekent dat als de gebruiker een lid van een groep is en die groep is lid van een andere groep met een roltoewijzing, die de roltoewijzing wordt ook vermeld. Dit filter accepteert alleen een object-id voor een gebruiker of een service-principal. U kunt een object-id niet doorgeven voor een groep. |

## <a name="grant-access"></a>Toegang verlenen

In RBAC verleent u toegang door een roltoewijzing te maken. Gebruik voor het maken van een roltoewijzing de [roltoewijzingen: Maak](/rest/api/authorization/roleassignments/create) REST-API en geeft u de beveiligings-principal, rol en het bereik. Voor het aanroepen van deze API, u moet toegang hebben tot de `Microsoft.Authorization/roleAssignments/write` bewerking. Van de ingebouwde rollen, alleen [eigenaar](built-in-roles.md#owner) en [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator) toegang krijgen tot deze bewerking.

1. Gebruik de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) REST-API of Zie [ingebouwde rollen](built-in-roles.md) om op te halen van de id voor de roldefinitie van de die u wilt toewijzen.

1. Een GUID-hulpprogramma gebruiken voor het genereren van een unieke id die wordt gebruikt voor de rol-ID-toewijzing. De id heeft de indeling: `00000000-0000-0000-0000-000000000000`

1. Begin met de volgende aanvraag en hoofdtekst:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```
    
1. Vervang in de URI, *{bereik}* met het bereik voor de roltoewijzing.

    | Scope | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleAssignmentName}* met de GUID-id van de roltoewijzing.

1. Vervang in de hoofdtekst van de aanvraag, *{subscriptionId}* met uw abonnements-id.

1. Vervang *{roleDefinitionId}* met de rol-id.

1. Vervang *{principalId}* met de object-id van de gebruiker, groep of service-principal die de rol wordt toegewezen.

## <a name="remove-access"></a>Toegang intrekken

Als u in RBAC de toegang wilt intrekken voor een rol, verwijdert u de roltoewijzing. Als u wilt een roltoewijzing verwijderen, gebruikt u de [verwijderen van roltoewijzingen:](/rest/api/authorization/roleassignments/delete) REST-API. Voor het aanroepen van deze API, u moet toegang hebben tot de `Microsoft.Authorization/roleAssignments/delete` bewerking. Van de ingebouwde rollen, alleen [eigenaar](built-in-roles.md#owner) en [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator) toegang krijgen tot deze bewerking.

1. Ophalen van de toewijzing van rol-id (GUID). Deze id wordt geretourneerd wanneer u eerst de roltoewijzing maken of u deze ophalen kunt door de roltoewijzingen weer te geven.

1. Beginnen met de volgende aanvraag:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. Vervang in de URI, *{bereik}* met het bereik voor het verwijderen van de roltoewijzing.

    | Scope | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleAssignmentName}* met de GUID-id van de roltoewijzing.

## <a name="next-steps"></a>Volgende stappen

- [Resources implementeren met Resource Manager-sjablonen en Resource Manager REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Azure REST API-naslaginformatie](/rest/api/azure/)
- [Aangepaste rollen voor Azure-resources met behulp van de REST-API maken](custom-roles-rest.md)

---
title: Beheren van toegang met RBAC en de REST-API - Azure | Microsoft Docs
description: Informatie over het beheren van toegang voor gebruikers, groepen en toepassingen met behulp van op rollen gebaseerde toegangsbeheer (RBAC) en de REST-API. Dit omvat het weergeven van access, toegang verlenen en toegang verwijderen.
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
ms.topic: article
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: cfcb87fdff8105b25d4f7e63b775aaf9243d2a90
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317004"
---
# <a name="manage-access-using-rbac-and-the-rest-api"></a>Beheren van toegang met RBAC en de REST-API

[Op rollen gebaseerde toegangsbeheer (RBAC)](overview.md) is de manier waarop dat u toegang tot bronnen in Azure beheren. Dit artikel wordt beschreven hoe het beheren van toegang voor gebruikers, groepen en toepassingen die gebruikmaken van RBAC en de REST-API.

## <a name="list-access"></a>Lijst met toegang

In RBAC, voor toegang tot lijst, u de naam de roltoewijzingen. Roltoewijzingen weergeven door een van de [roltoewijzingen - lijst](/rest/api/authorization/roleassignments/list) REST-API's. Uw om resultaten te verfijnen, geeft u een bereik en een optioneel filter. De API aanroepen, u moet toegang hebben tot de `Microsoft.Authorization/roleAssignments/read` bewerking bij het opgegeven bereik. Verschillende [ingebouwde rollen](built-in-roles.md) krijgen toegang tot deze bewerking.

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. Vervang in de URI *{bereik}* met het bereik waarvoor u wilt de roltoewijzingen lijst.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{filter}* met de voorwaarde die u wilt toepassen om de rol toewijzingslijst te filteren.

    | Filteren | Beschrijving |
    | --- | --- |
    | `$filter=atScope()` | Roltoewijzingen voor alleen het opgegeven bereik niet met inbegrip van de roltoewijzingen op subscopes weergeven. |
    | `$filter=principalId%20eq%20'{objectId}'` | Lijst van roltoewijzingen voor een opgegeven gebruiker, groep of service-principal. |
    | `$filter=assignedTo('{objectId}')` | Lijst roltoewijzingen voor een opgegeven gebruiker, inclusief overgenomen van groepen. |

## <a name="grant-access"></a>Toegang verlenen

In RBAC, voor het verlenen van toegang, maakt u een roltoewijzing. Gebruik voor het maken van een roltoewijzing de [roltoewijzingen - maken](/rest/api/authorization/roleassignments/create) REST-API en geef de beveiligings-principal, de roldefinitie en het bereik. Deze API aanroepen, u moet toegang hebben tot de `Microsoft.Authorization/roleAssignments/write` bewerking. Van de ingebouwde rollen alleen [eigenaar](built-in-roles.md#owner) en [beheerder voor gebruikerstoegang](built-in-roles.md#user-access-administrator) krijgen toegang tot deze bewerking.

1. Gebruik de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) REST-API of Zie [ingebouwde rollen](built-in-roles.md) ophalen van de id voor de roldefinitie die u wilt toewijzen.

1. Een GUID-hulpprogramma gebruiken voor het genereren van een unieke id die wordt gebruikt voor de rol-ID-toewijzing. De id heeft de indeling: `00000000-0000-0000-0000-000000000000`

1. Beginnen met de volgende aanvraag en hoofdtekst:

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
    
1. Vervang in de URI *{bereik}* met het bereik voor de toewijzing van rollen.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleAssignmentName}* met de id van de GUID van de roltoewijzing.

1. Vervang in de aanvraagtekst *{subscriptionId}* met uw abonnements-id.

1. Vervang *{roleDefinitionId}* met de rol-id.

1. Vervang *{principalId}* met de object-id van de gebruiker, groep of service-principal die de rol wordt toegewezen.

## <a name="remove-access"></a>Toegang verwijderen

In RBAC, als u wilt verwijderen, access, u een roltoewijzing. U kunt een roltoewijzing verwijderen met de [roltoewijzingen - verwijderen](/rest/api/authorization/roleassignments/delete) REST-API. Deze API aanroepen, u moet toegang hebben tot de `Microsoft.Authorization/roleAssignments/delete` bewerking. Van de ingebouwde rollen alleen [eigenaar](built-in-roles.md#owner) en [beheerder voor gebruikerstoegang](built-in-roles.md#user-access-administrator) krijgen toegang tot deze bewerking.

1. De toewijzing rol-id (GUID) worden opgehaald. Deze id wordt geretourneerd wanneer u eerst de toewijzing van rollen maken of u dit downloaden kunt door de roltoewijzingen te bieden.

1. Beginnen met de volgende aanvraag:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. Vervang in de URI *{bereik}* met het bereik voor het verwijderen van de roltoewijzing.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleAssignmentName}* met de id van de GUID van de roltoewijzing.

## <a name="next-steps"></a>Volgende stappen

- [Resources implementeren met Resource Manager-sjablonen en Resource Manager REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Azure REST API-verwijzing](/rest/api/azure/)
- [Aangepaste rollen met behulp van de REST-API maken](custom-roles-rest.md)
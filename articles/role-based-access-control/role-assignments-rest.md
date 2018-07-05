---
title: Beheren van toegang met RBAC en de REST-API - Azure | Microsoft Docs
description: Informatie over het beheren van toegang voor gebruikers, groepen en toepassingen, met behulp van op rollen gebaseerd toegangsbeheer (RBAC) en de REST-API. U vindt hier instructies voor het weergeven van toegang, het verlenen van toegang en het intrekken van toegang.
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
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 859a410a4ff9204e8e52fbd2cc3b38823f4bb830
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435215"
---
# <a name="manage-access-using-rbac-and-the-rest-api"></a>Beheren van toegang met RBAC en de REST-API

[Op rollen gebaseerde toegangsbeheer (RBAC)](overview.md) is de manier waarop u de toegang tot resources in Azure beheert. Dit artikel wordt beschreven hoe u de toegang voor gebruikers, groepen en toepassingen die gebruikmaken van RBAC en de REST-API beheren.

## <a name="list-access"></a>Toegang opvragen

In RBAC, de toegang van de lijst, u lijst maken met de roltoewijzingen. Lijst van roltoewijzingen, gebruikt u een van de [roltoewijzingen - lijst](/rest/api/authorization/roleassignments/list) REST-API's. Uw om resultaten te verfijnen, geeft u een bereik en een optioneel filter. Voor het aanroepen van de API, u moet toegang hebben tot de `Microsoft.Authorization/roleAssignments/read` bewerking bij het opgegeven bereik. Verschillende [ingebouwde rollen](built-in-roles.md) toegang krijgen tot deze bewerking.

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. Vervang in de URI, *{bereik}* met het bereik waarvoor u wilt weergeven van de roltoewijzingen.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{filter}* met de voorwaarde die u wilt toepassen op filter de lijst met toewijzingen van de rol.

    | Filteren | Beschrijving |
    | --- | --- |
    | `$filter=atScope()` | Roltoewijzingen voor alleen het opgegeven bereik niet inclusief de roltoewijzingen weergegeven bij subscopes lijst. |
    | `$filter=principalId%20eq%20'{objectId}'` | Lijst met roltoewijzingen voor een opgegeven gebruiker, groep of service-principal. |
    | `$filter=assignedTo('{objectId}')` | Lijst-roltoewijzingen voor een opgegeven gebruiker, inclusief overgenomen van groepen. |

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

    | Bereik | Type |
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

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleAssignmentName}* met de GUID-id van de roltoewijzing.

## <a name="next-steps"></a>Volgende stappen

- [Resources implementeren met Resource Manager-sjablonen en Resource Manager REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Azure REST API-verwijzing](/rest/api/azure/)
- [Aangepaste rollen met behulp van de REST-API maken](custom-roles-rest.md)
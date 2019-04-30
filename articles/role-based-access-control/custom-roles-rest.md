---
title: Aangepaste rollen maken voor Azure-resources met behulp van de REST-API - Azure | Microsoft Docs
description: Informatie over het maken van aangepaste rollen met op rollen gebaseerd toegangsbeheer (RBAC) voor Azure-resources met behulp van de REST-API. Dit omvat het weergeven, maken, bijwerken en verwijderen van aangepaste rollen.
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
ms.date: 04/18/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 4024f6fdb40c752ef61f348d15f681e81d81c08c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60596682"
---
# <a name="create-custom-roles-for-azure-resources-using-the-rest-api"></a>Aangepaste rollen voor Azure-resources met behulp van de REST-API maken

Als de [ingebouwde rollen voor Azure-resources](built-in-roles.md) niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen aangepaste rollen maken. In dit artikel wordt beschreven hoe u maken en beheren van aangepaste rollen met behulp van de REST-API.

## <a name="list-custom-roles"></a>Aangepaste rollen opvragen

U kunt alle aangepaste rollen in een map, gebruiken de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) REST-API.

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. Vervang *{filter}* met het Roltype.

    | Filteren | Description |
    | --- | --- |
    | `$filter=type%20eq%20'CustomRole'` | Filteren op basis van het type CustomRole |

## <a name="list-custom-roles-at-a-scope"></a>Lijst met aangepaste rollen met een bereik

U kunt aangepaste rollen met een bereik, gebruiken de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) REST-API.

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. Vervang in de URI, *{bereik}* met het bereik waarvoor u wilt weergeven van de rollen.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{filter}* met het Roltype.

    | Filteren | Description |
    | --- | --- |
    | `$filter=type%20eq%20'CustomRole'` | Filteren op basis van het type CustomRole |

## <a name="list-a-custom-role-definition-by-name"></a>Lijst van een aangepaste roldefinitie met de naam

Voor informatie over een aangepaste rol op de weergavenaam, gebruikt de [roldefinities - ophalen](/rest/api/authorization/roledefinitions/get) REST-API.

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. Vervang in de URI, *{bereik}* met het bereik waarvoor u wilt weergeven van de rollen.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{filter}* met de weergavenaam voor de rol.

    | Filteren | Description |
    | --- | --- |
    | `$filter=roleName%20eq%20'{roleDisplayName}'` | Gebruik het formulier URL-codering van de exacte naam van de rol. Bijvoorbeeld: `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

## <a name="list-a-custom-role-definition-by-id"></a>Lijst van een aangepaste roldefinitie op ID

Voor informatie over een aangepaste rol door de unieke id, gebruikt de [roldefinities - ophalen](/rest/api/authorization/roledefinitions/get) REST-API.

1. Gebruik de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) REST-API voor het ophalen van de GUID-id voor de rol.

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Vervang in de URI, *{bereik}* met het bereik waarvoor u wilt weergeven van de rollen.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleDefinitionId}* met de GUID-id van de roldefinitie.

## <a name="create-a-custom-role"></a>Een aangepaste rol maken

U kunt een aangepaste rol maken met de [roldefinities - maken of bijwerken](/rest/api/authorization/roledefinitions/createorupdate) REST-API. Voor het aanroepen van deze API, moet u zijn aangemeld met een gebruiker die een rol heeft die is toegewezen de `Microsoft.Authorization/roleDefinitions/write` machtiging op alle de `assignableScopes`. Van de ingebouwde rollen, alleen [eigenaar](built-in-roles.md#owner) en [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator) bevatten deze machtiging.

1. Bekijk de lijst [resourceproviderbewerkingen](resource-provider-operations.md) die beschikbaar zijn voor de machtigingen voor uw aangepaste rol maken.

1. Een GUID-hulpprogramma gebruiken voor het genereren van een unieke id die wordt gebruikt voor de aangepaste rol-id. De id heeft de indeling: `00000000-0000-0000-0000-000000000000`

1. Begin met de volgende aanvraag en hoofdtekst:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. Vervang in de URI, *{bereik}* waarbij de eerste `assignableScopes` van de aangepaste rol.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleDefinitionId}* met de GUID-id van de aangepaste rol.

1. In de hoofdtekst van de aanvraag in de `assignableScopes` eigenschap, Vervang *{roleDefinitionId}* met de GUID-id.

1. Vervang *{subscriptionId}* met uw abonnements-id.

1. In de `actions` eigenschap toevoegen van de bewerkingen die de rol kan worden uitgevoerd.

1. In de `notActions` eigenschap toevoegen van de bewerkingen die zijn uitgesloten van het toegestane aantal `actions`.

1. In de `roleName` en `description` eigenschappen, naam van een unieke rol en een beschrijving opgeven. Zie voor meer informatie over de eigenschappen van [aangepaste rollen](custom-roles.md).

    Hier volgt een voorbeeld van een aanvraagtekst:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
              "Microsoft.Storage/*/read",
              "Microsoft.Network/*/read",
              "Microsoft.Compute/*/read",
              "Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action",
              "Microsoft.Authorization/*/read",
              "Microsoft.ResourceHealth/availabilityStatuses/read",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="update-a-custom-role"></a>Een aangepaste rol bijwerken

Gebruiken voor het bijwerken van een aangepaste rol, de [roldefinities - maken of bijwerken](/rest/api/authorization/roledefinitions/createorupdate) REST-API. Voor het aanroepen van deze API, moet u zijn aangemeld met een gebruiker die een rol heeft die is toegewezen de `Microsoft.Authorization/roleDefinitions/write` machtiging op alle de `assignableScopes`. Van de ingebouwde rollen, alleen [eigenaar](built-in-roles.md#owner) en [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator) bevatten deze machtiging.

1. Gebruik de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) of [roldefinities - ophalen](/rest/api/authorization/roledefinitions/get) REST-API voor informatie over de aangepaste rol. Voor meer informatie, Zie de eerdere [lijst aangepaste rollen](#list-custom-roles) sectie.

1. Beginnen met de volgende aanvraag:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Vervang in de URI, *{bereik}* waarbij de eerste `assignableScopes` van de aangepaste rol.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleDefinitionId}* met de GUID-id van de aangepaste rol.

1. Op basis van de informatie over de aangepaste rol, maakt een aanvraagtekst met de volgende indeling:

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. De aanvraagtekst bijwerken met de wijzigingen die u wilt maken voor de aangepaste rol.

    De volgende toont een voorbeeld van een aanvraagtekst met een nieuwe actie voor diagnostische instellingen toegevoegd:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
              "Microsoft.Storage/*/read",
              "Microsoft.Network/*/read",
              "Microsoft.Compute/*/read",
              "Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action",
              "Microsoft.Authorization/*/read",
              "Microsoft.ResourceHealth/availabilityStatuses/read",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Insights/diagnosticSettings/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen

Als u wilt een aangepaste rol verwijderen, gebruikt u de [roldefinities - verwijderen](/rest/api/authorization/roledefinitions/delete) REST-API. Voor het aanroepen van deze API, moet u zijn aangemeld met een gebruiker die een rol heeft die is toegewezen de `Microsoft.Authorization/roleDefinitions/delete` machtiging op alle de `assignableScopes`. Van de ingebouwde rollen, alleen [eigenaar](built-in-roles.md#owner) en [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator) bevatten deze machtiging.

1. Gebruik de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) of [roldefinities - ophalen](/rest/api/authorization/roledefinitions/get) REST-API voor het ophalen van de GUID-id van de aangepaste rol. Voor meer informatie, Zie de eerdere [lijst aangepaste rollen](#list-custom-roles) sectie.

1. Beginnen met de volgende aanvraag:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Vervang in de URI, *{bereik}* met het bereik dat u wilt dat de aangepaste beveiligingsrol te verwijderen.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleDefinitionId}* met de GUID-id van de aangepaste rol.

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste rollen voor Azure-resources](custom-roles.md)
- [Beheer de toegang tot Azure-resources met behulp van RBAC en de REST-API](role-assignments-rest.md)
- [Azure REST API-naslaginformatie](/rest/api/azure/)
---
title: Maken van aangepaste rollen met de REST-API - Azure | Microsoft Docs
description: Informatie over het maken van aangepaste rollen voor op rollen gebaseerde toegangsbeheer (RBAC) met de REST API. Dit omvat de lijst, maken, bijwerken en verwijderen van aangepaste rollen.
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
ms.openlocfilehash: 8267846fed30baf2c37dcddd453ae9ead9341da9
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320588"
---
# <a name="create-custom-roles-using-the-rest-api"></a>Aangepaste rollen met behulp van de REST-API maken

Als de [ingebouwde rollen](built-in-roles.md) niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen aangepaste rollen maken. In dit artikel wordt beschreven hoe maken en beheren van aangepaste rollen met behulp van de REST-API.

## <a name="list-roles"></a>Lijst met rollen

Als u alle rollen lijst of informatie ophalen over een enkele rol met de weergavenaam, gebruiken de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) REST-API. Deze API aanroepen, u moet toegang hebben tot de `Microsoft.Authorization/roleDefinitions/read` bewerking in het bereik. Verschillende [ingebouwde rollen](built-in-roles.md) krijgen toegang tot deze bewerking.

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. Vervang in de URI *{bereik}* met het bereik waarvoor u wilt weergeven van de rollen.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{filter}* met de voorwaarde die u wilt toepassen om de lijst van de rol te filteren.

    | Filteren | Beschrijving |
    | --- | --- |
    | `$filter=atScopeAndBelow()` | Lijst van functies die beschikbaar zijn voor toewijzing bij het opgegeven bereik en een van de onderliggende bereiken. |
    | `$filter=roleName%20eq%20'{roleDisplayName}'` | Gebruik het URL-gecodeerd-formulier van de exacte weergavenaam van de rol. Bijvoorbeeld: `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="get-information-about-a-role"></a>Informatie ophalen over een rol

Als u informatie over een rol met de rol definitie-id, gebruikt de [roldefinities - ophalen](/rest/api/authorization/roledefinitions/get) REST-API. Deze API aanroepen, u moet toegang hebben tot de `Microsoft.Authorization/roleDefinitions/read` bewerking in het bereik. Verschillende [ingebouwde rollen](built-in-roles.md) krijgen toegang tot deze bewerking.

Als u informatie over de weergegeven naam met één functie, Zie vorige [lijst rollen](custom-roles-rest.md#list-roles) sectie.

1. Gebruik de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) REST-API om op te halen van de GUID-id voor de rol. Voor de ingebouwde rollen kunt u ook de id van krijgen [ingebouwde rollen](built-in-roles.md).

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Vervang in de URI *{bereik}* met het bereik waarvoor u wilt weergeven van de rollen.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleDefinitionId}* met de id van de GUID van de functiedefinitie.

## <a name="create-a-custom-role"></a>Een aangepaste beveiligingsrol maken

Gebruik voor het maken van een aangepaste rol de [roldefinities - maken of bijwerken](/rest/api/authorization/roledefinitions/createorupdate) REST-API. Deze API aanroepen, u moet toegang hebben tot de `Microsoft.Authorization/roleDefinitions/write` bewerking op alle de `assignableScopes`. Van de ingebouwde rollen alleen [eigenaar](built-in-roles.md#owner) en [beheerder voor gebruikerstoegang](built-in-roles.md#user-access-administrator) krijgen toegang tot deze bewerking. 

1. Bekijk de lijst met [resource provider operations](resource-provider-operations.md) die beschikbaar zijn voor het maken van de machtigingen voor uw aangepaste rol.

1. Een GUID-hulpprogramma gebruiken voor het genereren van een unieke id die wordt gebruikt voor de aangepaste rol-id. De id heeft de indeling: `00000000-0000-0000-0000-000000000000`

1. Beginnen met de volgende aanvraag en hoofdtekst:

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

1. Vervang in de URI *{bereik}* met het eerste `assignableScopes` van de aangepaste rol.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleDefinitionId}* met de GUID-id van de aangepaste rol.

1. In de hoofdtekst van de aanvraag in de `assignableScopes` eigenschap vervangen *{roleDefinitionId}* met de GUID-id.

1. Vervang *{subscriptionId}* met uw abonnements-id.

1. In de `actions` eigenschap toevoegbewerkingen waarmee de rol moet worden uitgevoerd.

1. In de `notActions` -eigenschap toevoegen van de bewerkingen die zijn uitgesloten van de toegestane `actions`.

1. In de `roleName` en `description` eigenschappen, Geef een unieke naam en beschrijving op. Zie voor meer informatie over de eigenschappen [aangepaste rollen](custom-roles.md).

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

## <a name="update-a-custom-role"></a>Bijwerken van een aangepaste rol

Voor het bijwerken van een aangepaste beveiligingsrol, gebruiken de [roldefinities - maken of bijwerken](/rest/api/authorization/roledefinitions/createorupdate) REST-API. Deze API aanroepen, u moet toegang hebben tot de `Microsoft.Authorization/roleDefinitions/write` bewerking op alle de `assignableScopes`. Van de ingebouwde rollen alleen [eigenaar](built-in-roles.md#owner) en [beheerder voor gebruikerstoegang](built-in-roles.md#user-access-administrator) krijgen toegang tot deze bewerking. 

1. Gebruik de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) of [roldefinities - ophalen](/rest/api/authorization/roledefinitions/get) REST-API voor informatie over de aangepaste rol. Voor meer informatie Zie eerder [lijst rollen](custom-roles-rest.md#list-roles) sectie.

1. Beginnen met de volgende aanvraag:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Vervang in de URI *{bereik}* met het eerste `assignableScopes` van de aangepaste rol.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleDefinitionId}* met de GUID-id van de aangepaste rol.

1. Op basis van de informatie over de aangepaste rol, maakt u een aanvraagtekst met de volgende indeling:

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

    Hier volgt een voorbeeld van een aanvraagtekst met een nieuwe diagnostische instellingen voor actie toegevoegd:

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

U een aangepaste rol verwijderen met de [roldefinities - verwijderen](/rest/api/authorization/roledefinitions/delete) REST-API. Deze API aanroepen, u moet toegang hebben tot de `Microsoft.Authorization/roleDefinitions/delete` bewerking op alle de `assignableScopes`. Van de ingebouwde rollen alleen [eigenaar](built-in-roles.md#owner) en [beheerder voor gebruikerstoegang](built-in-roles.md#user-access-administrator) krijgen toegang tot deze bewerking. 

1. Gebruik de [roldefinities - lijst](/rest/api/authorization/roledefinitions/list) of [roldefinities - ophalen](/rest/api/authorization/roledefinitions/get) REST-API om op te halen van de GUID-id van de aangepaste rol. Voor meer informatie Zie eerder [lijst rollen](custom-roles-rest.md#list-roles) sectie.

1. Beginnen met de volgende aanvraag:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Vervang in de URI *{bereik}* met het bereik dat u wilt dat de aangepaste beveiligingsrol te verwijderen.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{roleDefinitionId}* met de GUID-id van de aangepaste rol.

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste rollen in Azure](custom-roles.md)
- [Beheren van toegang met RBAC en de REST-API](role-assignments-rest.md)
- [Azure REST API-verwijzing](/rest/api/azure/)
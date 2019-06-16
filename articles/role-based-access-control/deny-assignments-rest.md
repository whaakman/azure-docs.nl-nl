---
title: Lijst weigeren toewijzingen voor Azure-resources met behulp van de REST-API - Azure | Microsoft Docs
description: Meer informatie over het lijst weigeren toewijzingen voor gebruikers, groepen en toepassingen die gebruikmaken van op rollen gebaseerd toegangsbeheer (RBAC) voor Azure-resources en de REST-API.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 0bc49456f5965846a2de542b4a063bab2d1838bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67118287"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>Lijst weigeren toewijzingen voor Azure-resources met behulp van de REST-API

[Toewijzingen weigeren](deny-assignments.md) blokkeren dat gebruikers kunnen uitvoeren van acties die specifieke Azure-resource, zelfs als een roltoewijzing deze toegang wordt verleend. In dit artikel wordt beschreven hoe u aan de lijst weigeren toewijzingen met behulp van de REST-API.

> [!NOTE]
> U kunt geen rechtstreeks maken uw eigen toewijzingen weigeren. Zie voor informatie over hoe u weigeren toewijzingen worden gemaakt, [weigeren toewijzingen](deny-assignments.md).

## <a name="prerequisites"></a>Vereisten

Voor informatie over de toewijzing van een weigeren, moet u het volgende hebben:

- `Microsoft.Authorization/denyAssignments/read` machtiging die is opgenomen in de meeste [ingebouwde rollen voor Azure-resources](built-in-roles.md).

## <a name="list-a-single-deny-assignment"></a>Lijst met één opdracht weigeren

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. Vervang in de URI, *{bereik}* met het bereik waarvoor u wilt weergeven van de toewijzingen weigeren.

    | Scope | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{weigeren-toewijzing-id}* met de weigeren toewijzing-id die u wilt ophalen.

## <a name="list-multiple-deny-assignments"></a>Lijst met meerdere toewijzingen weigeren

1. Begin met een van de volgende aanvragen:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    Met de volgende optionele parameters:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Vervang in de URI, *{bereik}* met het bereik waarvoor u wilt weergeven van de toewijzingen weigeren.

    | Scope | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{filter}* met de voorwaarde die u wilt toepassen op filter de lijst weigeren toewijzing.

    | Filteren | Description |
    | --- | --- |
    | (geen filter) | Lijst met alle weigeren toewijzingen aan, boven en onder het opgegeven bereik. |
    | `$filter=atScope()` | Lijst weigeren toewijzingen voor alleen het opgegeven bereik en hoger. Geen toewijzingen op subscopes weigeren. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Lijst weigeren toewijzingen met de opgegeven naam. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Lijst met toewijzingen voor de scope hoofdmap (/) weigeren

1. Uw toegangsrechten zoals beschreven in [toegangsrechten voor een globale beheerder in Azure Active Directory](elevate-access-global-admin.md).

1. Gebruik de volgende aanvraag:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Vervang *{filter}* met de voorwaarde die u wilt toepassen op filter de lijst weigeren toewijzing. Een filter is vereist.

    | Filteren | Description |
    | --- | --- |
    | `$filter=atScope()` | Lijst weigeren toewijzingen voor alleen het bereik van de hoofdmap. Geen toewijzingen op subscopes weigeren. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Lijst weigeren toewijzingen met de opgegeven naam. |

1. Verwijder toegang met verhoogde bevoegdheid.

## <a name="next-steps"></a>Volgende stappen

- [Informatie over toewijzingen voor Azure-resources te weigeren](deny-assignments.md)
- [Toegangsbevoegdheid voor een globale beheerder verhogen in Azure Active Directory](elevate-access-global-admin.md)
- [Azure REST API-naslaginformatie](/rest/api/azure/)

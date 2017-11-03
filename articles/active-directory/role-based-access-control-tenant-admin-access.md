---
title: Tenantbeheerder toegangsrechten - Azure AD uitbreiden | Microsoft Docs
description: Dit onderwerp beschrijft de ingebouwde rollen voor op rollen gebaseerde toegangsbeheer (RBAC).
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andredm
ms.openlocfilehash: 22b62be1773c5042ecf6ee078e68a4ffdf791d53
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a>Toegangsrechten uitbreiden als een tenantbeheerder met toegangsbeheer op basis van rollen

Toegangsbeheer op basis van rollen kunt tenantbeheerders tijdelijke uitbreidingen in toegang krijgen, zodat ze hoger dan normaal machtigen kunnen. Een tenantbeheerder kan zichzelf uitbreiden naar de rol beheerder voor gebruikerstoegang wanneer deze nodig is. Die rol, krijgt de tenant beheerdersmachtigingen te verlenen aan zichzelf of andere functies op het bereik '/'.

Deze functie is belangrijk omdat hiermee de tenantbeheerder om te zien van de abonnementen die zijn opgenomen in een organisatie. Kunt u ook voor automation-apps (zoals facturering en controle) voor toegang tot alle abonnementen en bieden een nauwkeurige weergave van de status van de organisatie voor facturerings- of asset management.  

## <a name="how-to-use-elevateaccess-for-tenant-access-with-azure-ad-admin-center"></a>Het gebruik van elevateAccess voor tenant-toegang met Azure AD-beheercentrum

In de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) u kunt deze functie van aanroepen **eigenschappen**.
De functie wordt aangeroepen **globale beheerder Azure-abonnementen kunt beheren**. De indruk is dat dit is een globale eigenschap voor Azure Active Directory, maar het functioneert op basis van per gebruiker voor de momenteel aangemelde gebruiker. Wanneer u rechten voor globale beheerder in Azure Active Directory hebt, kunt u de functie elevateAccess voor de gebruiker die u momenteel bent aangemeld bij Azure Active Directory-beheercentrum kunt aanroepen.

Selecteren **Ja** en vervolgens **opslaan**: dit **wijst** de **beheerder voor gebruikerstoegang** rol in de hoofdmap '/' (Root Scope) voor de gebruiker met die u momenteel bent aangemeld bij de Portal.

Selecteren **Nee** en vervolgens **opslaan**: dit **verwijdert** de **beheerder voor gebruikerstoegang** rol in de hoofdmap '/' (Root Scope) voor de gebruiker met die u momenteel bent aangemeld bij de Portal.

![Azure AD-beheercentrum - eigenschappen - Globaladmin kunt beheren Azure-abonnement - schermafbeelding](./media/role-based-access-control-tenant-admin-access/aad-azure-portal-global-admin-can-manage-azure-subscriptions.png)

## <a name="how-to-use-elevateaccess-to-give-tenant-access-with-the-rest-api"></a>Het gebruik van elevateAccess tenant toegang met de REST-API

Het basisproces werkt met de volgende stappen uit:

1. REST, aanroepen *elevateAccess*, waarmee u de rol van beheerder voor gebruikerstoegang op verleent ' / ' bereik.

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. Maak een [roltoewijzing](/rest/api/authorization/roleassignments) aan elke rol op een bereik toe te wijzen. Het volgende voorbeeld ziet u de eigenschappen voor het toewijzen van de rol lezer op ' / ' bereik:

    ```
    { "properties":{
    "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/acdd72a7338548efbd42f606fba81ae7",
    "principalId": "cbc5e050-d7cd-4310-813b-4870be8ef5bb",
    "scope": "/"
    },
    "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
    }
    ```

3. Tijdens een gebruiker toegang beheerder, kunt u ook verwijderen roltoewijzingen op ' / ' bereik.

4. De gebruiker toegang tot de beheerdersmachtigingen intrekken totdat ze weer nodig zijn.


## <a name="how-to-undo-the-elevateaccess-action-with-the-rest-api"></a>Ongedaan maken van de actie elevateAccess met de REST-API

Als u aanroept *elevateAccess* u een roltoewijzing maken voor uzelf, zodat deze bevoegdheden intrekken moet u de toewijzing verwijderen.

1.  Roep [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) waar roleName = beheerder voor gebruikerstoegang om te bepalen van de naam van de GUID van de rol beheerder voor gebruikerstoegang. Het antwoord ziet er als volgt:

    ```
    {"value":[{"properties":{
    "roleName":"User Access Administrator",
    "type":"BuiltInRole",
    "description":"Lets you manage user access to Azure resources.",
    "assignableScopes":["/"],
    "permissions":[{"actions":["*/read","Microsoft.Authorization/*","Microsoft.Support/*"],"notActions":[]}],
    "createdOn":"0001-01-01T08:00:00.0000000Z",
    "updatedOn":"2016-05-31T23:14:04.6964687Z",
    "createdBy":null,
    "updatedBy":null},
    "id":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "type":"Microsoft.Authorization/roleDefinitions",
    "name":"18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"}],
    "nextLink":null}
    ```

    Sla de GUID van de *naam* parameter in dit geval **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.

2. Roep [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) waar principalId = uw eigen ObjectId. Hier ziet u alle toewijzingen in de tenant. Zoek naar de waar de scope is '/' en de RoleDefinitionId eindigt met de naam van de rol GUID die u in stap 1 hebt gevonden. De roltoewijzing moet er als volgt uitzien:

    ```
    {"value":[{"properties":{
    "roleDefinitionId":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "principalId":"{objectID}",
    "scope":"/",
    "createdOn":"2016-08-17T19:21:16.3422480Z",
    "updatedOn":"2016-08-17T19:21:16.3422480Z",
    "createdBy":"93ce6722-3638-4222-b582-78b75c5c6d65",
    "updatedBy":"93ce6722-3638-4222-b582-78b75c5c6d65"},
    "id":"/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
    "type":"Microsoft.Authorization/roleAssignments",
    "name":"e7dd75bc-06f6-4e71-9014-ee96a929d099"}],
    "nextLink":null}
    ```

    Opnieuw in en sla de GUID van de *naam* parameter in dit geval **e7dd75bc-06f6-4e71-9014-ee96a929d099**.

3. Tenslotte roept [verwijderen roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) waar roleAssignmentId = de naam van de GUID die u in stap 2 hebt vastgesteld.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [toegangsbeheer op basis van rollen met REST beheren](role-based-access-control-manage-access-rest.md)

- [Beheren van toegangstoewijzingen](role-based-access-control-manage-assignments.md) in de Azure portal

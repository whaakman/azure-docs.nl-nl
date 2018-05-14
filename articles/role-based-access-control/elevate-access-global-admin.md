---
title: Toegangsrechten voor een globale beheerder in Azure Active Directory uitbreiden | Microsoft Docs
description: Beschrijft hoe een globale beheerder in Azure Active Directory met Azure portal of REST-API toegang uitbreiden.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: rolyon
ms.openlocfilehash: b671ff6b473093e59bce18c7bf98b32e9849bbb0
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Toegangsrechten voor een globale beheerder in Azure Active Directory uitbreiden

Als u een [hoofdbeheerder](../active-directory/active-directory-assign-admin-roles-azure-portal.md#global-administrator) in Azure Active Directory (Azure AD), kan gebeuren als u wilt de volgende handelingen uit:

- Weer toegang tot een Azure-abonnement als een gebruiker toegang heeft verloren
- Verleen een andere gebruiker of zelf de toegang tot een Azure-abonnement
- Zie alle Azure-abonnementen in een organisatie
- Een automation-app (zoals een facturering of controle-app) voor toegang tot alle Azure-abonnementen toestaan

Beheren (RBAC) functies komen niet span Azure AD en Azure standaard Azure AD-beheerdersrollen en -toegang tot Azure op basis van rollen. Als u een globale beheerder in Azure AD, kunt u uw toegang tot het beheer van Azure-abonnementen en beheergroepen uitbreiden. Wanneer u de toegang tot de bevoegdheden, u krijgt de [beheerder voor gebruikerstoegang](built-in-roles.md#user-access-administrator) rol (een RBAC-rol) voor alle abonnementen voor een bepaalde tenant. De beheerder voor gebruikerstoegang-functie kunt u andere gebruikers toegang verlenen tot Azure-resources voor het root-bereik (`/`).

Deze uitbreiding van bevoegdheden moet tijdelijk en alleen uitgevoerd wanneer deze nodig is.

## <a name="elevate-access-for-a-global-administrator-using-the-azure-portal"></a>Toegangsrechten uitbreiden voor een globale beheerder met behulp van de Azure-portal

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) of de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com).

1. Klik in de lijst navigatie op **Azure Active Directory** en klik vervolgens op **eigenschappen**.

   ![Eigenschappen van Azure AD - schermafbeelding](./media/elevate-access-global-admin/aad-properties.png)

1. Onder **globale beheerder kunnen worden beheerd Azure-abonnementen en beheergroepen**, instellen van de switch op **Ja**.

   ![Globale beheerder kunnen worden beheerd Azure-abonnementen en beheergroepen - schermafbeelding](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Als u de switch instelt op **Ja**, uw globale beheerdersaccount (momenteel aangemelde gebruiker) wordt toegevoegd aan de rol beheerder voor gebruikerstoegang in Azure RBAC voor het root-bereik (`/`), hebt u toegang tot de weergave en het rapport op alle Azure-abonnementen die zijn gekoppeld aan uw Azure AD-tenant.

   Als u de switch instelt op **Nee**, uw globale beheerdersaccount (momenteel aangemelde gebruiker) is verwijderd uit de rol beheerder voor gebruikerstoegang in Azure RBAC. U kunt niet alle Azure-abonnementen die gekoppeld aan de Azure AD-tenant zijn bekijken en u kunt weergeven en beheren van alleen de Azure-abonnementen waartoe u toegang hebben gekregen.

1. Klik op **opslaan** om op te slaan van de instelling.

   Deze instelling is geen globale eigenschap en geldt alleen voor de momenteel aangemelde gebruiker.

1. De taken die u wilt maken op de uitgebreide toegangsrechten uitvoeren. Als u klaar bent, stelt u de schakeloptie terug naar **Nee**.

## <a name="list-role-assignment-at-the-root-scope--using-powershell"></a>Lijst van de roltoewijzing bij het root-bereik (/) met behulp van PowerShell

Voor een lijst met de toewijzing van de rol beheerder voor gebruikerstoegang voor een gebruiker bij het root-bereik (`/`), gebruikt u de [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) opdracht.

```azurepowershell
Get-AzureRmRoleAssignment | where {$_.RoleDefinitionName -eq "User Access Administrator" `
  -and $_.SignInName -eq "<username@example.com>" -and $_.Scope -eq "/"}
```

```Example
RoleAssignmentId   : /providers/Microsoft.Authorization/roleAssignments/098d572e-c1e5-43ee-84ce-8dc459c7e1f0
Scope              : /
DisplayName        : username
SignInName         : username@example.com
RoleDefinitionName : User Access Administrator
RoleDefinitionId   : 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9
ObjectId           : d65fd0e9-c185-472c-8f26-1dafa01f72cc
ObjectType         : User
```

## <a name="delete-a-role-assignment-at-the-root-scope--using-powershell"></a>Een roltoewijzing bij het root-bereik (/) met behulp van PowerShell verwijderen

Verwijderen van een beheerder voor gebruikerstoegang roltoewijzing voor een gebruiker bij het root-bereik (`/`), gebruikt u de [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) opdracht.

```azurepowershell
Remove-AzureRmRoleAssignment -SignInName <username@example.com> `
  -RoleDefinitionName "User Access Administrator" -Scope "/"
```

## <a name="elevate-access-for-a-global-administrator-using-the-rest-api"></a>Toegangsrechten voor een globale beheerder met de REST API uitbreiden

Gebruik de volgende basisstappen wilt uitbreiden toegang tot een globale beheerder met behulp van de REST-API.

1. REST, aanroepen `elevateAccess`, verleent dat u de rol beheerder voor gebruikerstoegang voor het root-bereik (`/`).

   ```http
   POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
   ```

1. Maak een [roltoewijzing](/rest/api/authorization/roleassignments) aan elke rol op een bereik toe te wijzen. Het volgende voorbeeld ziet u de eigenschappen voor het toewijzen van de rol {roleDefinitionID} bij het root-bereik (`/`):

   ```json
   { 
     "properties": {
       "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionID}",
       "principalId": "{objectID}",
       "scope": "/"
     },
     "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
     "type": "Microsoft.Authorization/roleAssignments",
     "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
   }
   ```

1. Tijdens een beheerder voor gebruikerstoegang, kunt u ook functietoewijzingen voor de scope hoofdmap verwijderen (`/`).

1. Uw beheerder voor gebruikerstoegang-machtigingen intrekken totdat ze weer nodig zijn.


## <a name="how-to-undo-the-elevateaccess-action-with-the-rest-api"></a>Ongedaan maken van de actie elevateAccess met de REST-API

Als u aanroept `elevateAccess`, u een roltoewijzing voor uzelf maken, dus in te trekken van de bevoegdheden moet u de toewijzing te verwijderen.

1. Roep [GET roleDefinitions](/rest/api/authorization/roledefinitions/get) waar `roleName` gelijk is aan de beheerder voor gebruikerstoegang om te bepalen van de naam-ID van de rol beheerder voor gebruikerstoegang.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter=roleName+eq+'User Access Administrator'
    ```

    ```json
    {
      "value": [
        {
          "properties": {
        "roleName": "User Access Administrator",
        "type": "BuiltInRole",
        "description": "Lets you manage user access to Azure resources.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "*/read",
              "Microsoft.Authorization/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedOn": "2016-05-31T23:14:04.6964687Z",
        "createdBy": null,
        "updatedBy": null
          },
          "id": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
          "type": "Microsoft.Authorization/roleDefinitions",
          "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"
        }
      ],
      "nextLink": null
    }
    ```

    Sla de ID van de `name` parameter in dit geval `18d7d88d-d35e-4fb5-a5c3-7773c20a72d9`.

2. U moet er ook voor het weergeven van de roltoewijzing voor tenantbeheerder binnen het bereik van de tenant. Lijst van alle toewijzingen in het bereik van de tenant voor de `principalId` van de tenantbeheerder die de uitbreiden toegang aanroepen. Hiermee wordt een lijst van alle toewijzingen in de tenant voor de object-id.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Een tenantbeheerder mag geen veel toewijzingen hebben als de vorige query te veel toewijzingen, u kunt ook een query retourneert voor alle toewijzingen van op scopeniveau tenant Just en vervolgens de resultaten filteren: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
    2. Het aanroepen van de vorige retourneren een lijst met roltoewijzingen. De roltoewijzing vinden waar de scope is '/' en de `roleDefinitionId` eindigt op de rol naam-ID die u in stap 1 hebt gevonden en `principalId` overeenkomt met de object-id van de tenantbeheerder. 
    
    Voorbeeld van roltoewijzing:

        ```json
        {
          "value": [
            {
              "properties": {
                "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
                "principalId": "{objectID}",
                "scope": "/",
                "createdOn": "2016-08-17T19:21:16.3422480Z",
                "updatedOn": "2016-08-17T19:21:16.3422480Z",
                "createdBy": "93ce6722-3638-4222-b582-78b75c5c6d65",
                "updatedBy": "93ce6722-3638-4222-b582-78b75c5c6d65"
              },
              "id": "/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
              "type": "Microsoft.Authorization/roleAssignments",
              "name": "e7dd75bc-06f6-4e71-9014-ee96a929d099"
            }
          ],
          "nextLink": null
        }
        ```
        
    Opnieuw in en sla de ID van de `name` parameter in dit geval e7dd75bc-06f6-4e71-9014-ee96a929d099.

    3. Gebruik tot slot de roltoewijzings-ID te verwijderen van de toewijzing toegevoegd door `elevateAccess`:

    ```http
    DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01
    ```

## <a name="next-steps"></a>Volgende stappen

- [Toegangsbeheer op basis van rollen met REST](role-assignments-rest.md)
- [Toewijzingen access beheren](role-assignments-users.md)

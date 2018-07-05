---
title: Toegangsrechten voor een globale beheerder in Azure Active Directory | Microsoft Docs
description: Beschrijft hoe u toegangsrechten voor een globale beheerder in Azure Active Directory met Azure portal of REST-API.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: bagovind
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/29/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 0abf0a5971435fc3842a93e79d39468cba5c74da
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445208"
---
# <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Toegangsrechten voor een globale beheerder in Azure Active Directory

Als u een [hoofdbeheerder](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) in Azure Active Directory (Azure AD), er kan gebeuren als u wilt het volgende doen:

- Opnieuw toegang krijgen tot een Azure-abonnement wanneer een gebruiker heeft geen toegang meer
- Een andere gebruiker of voor uzelf toegang tot een Azure-abonnement verlenen
- Zie alle Azure-abonnementen in een organisatie
- Een automation-app (zoals een facturering of controle-app) voor toegang tot alle Azure-abonnementen toestaan

Standaard Azure AD-beheerdersrollen en Azure op rollen gebaseerd toegangsbeheer (RBAC) rollen wilt niet span Azure AD en Azure. Als u een globale beheerder in Azure AD, kunt u de toegang tot het beheer van Azure-abonnementen en beheergroepen verhogen. Wanneer u uw toegangsrechten, u krijgt de [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator) rol (een RBAC-rol) voor alle abonnementen voor een bepaalde tenant. De rol Administrator voor gebruikerstoegang kunt u andere gebruikers toegang verlenen tot Azure-resources op het bereik van de hoofdmap (`/`).

Deze uitbreiding van bevoegdheden moet tijdelijke en alleen uitgevoerd wanneer dat nodig is.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="elevate-access-for-a-global-administrator-using-the-azure-portal"></a>Toegangsrechten voor een globale beheerder met behulp van de Azure portal

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) of de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com).

1. Klik in de navigatielijst **Azure Active Directory** en klik vervolgens op **eigenschappen**.

   ![Eigenschappen van Azure AD - schermafbeelding](./media/elevate-access-global-admin/aad-properties.png)

1. Onder **globale beheerder kan beheren Azure-abonnementen en beheergroepen**, instellen van de switch op **Ja**.

   ![Globale beheerder kan beheren Azure-abonnementen en beheergroepen - schermafbeelding](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Als u de switch instelt op **Ja**, uw account voor globale beheerders (momenteel aangemelde gebruiker) wordt toegevoegd aan de rol Administrator voor gebruikerstoegang in Azure RBAC bij het root-bereik (`/`), hebt u toegang tot het weergeven en rapporteren op alle Azure-abonnementen die zijn gekoppeld aan uw Azure AD-tenant.

   Als u de switch instelt op **Nee**, uw account voor globale beheerders (momenteel aangemelde gebruiker) wordt verwijderd uit de rol Administrator voor gebruikerstoegang in Azure RBAC. Alle Azure-abonnementen die gekoppeld aan de Azure AD-tenant zijn niet wordt weergegeven, en u kunt weergeven en beheren van alleen de Azure-abonnementen waartoe u toegang is verleend.

1. Klik op **opslaan** om op te slaan van de instelling.

   Deze instelling is niet een globale eigenschap en geldt alleen voor de momenteel aangemelde gebruiker.

1. De taken die u wilt maken op de uitgebreide toegangsrechten uitvoeren. Wanneer u klaar bent, stelt u de switch terug naar **Nee**.

## <a name="list-role-assignment-at-the-root-scope--using-powershell"></a>Lijst met roltoewijzing bij het root-bereik (/) met behulp van PowerShell

Om de toewijzing van de rol Administrator voor gebruikerstoegang voor een gebruiker bij het bereik van de hoofdmap weer te geven (`/`), gebruikt u de [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) opdracht.

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

## <a name="remove-a-role-assignment-at-the-root-scope--using-powershell"></a>Verwijderen van een roltoewijzing bij het root-bereik (/) met behulp van PowerShell

Verwijderen van een roltoewijzing Administrator voor gebruikerstoegang voor een gebruiker bij het root-bereik (`/`), gebruikt u de [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) opdracht.

```azurepowershell
Remove-AzureRmRoleAssignment -SignInName <username@example.com> `
  -RoleDefinitionName "User Access Administrator" -Scope "/"
```

## <a name="elevate-access-for-a-global-administrator-using-the-rest-api"></a>Toegangsrechten voor een globale beheerder met behulp van de REST-API

Gebruik de volgende eenvoudige stappen aan de toegangsrechten voor een globale beheerder met behulp van de REST-API.

1. Met behulp van REST, aanroepen `elevateAccess`, verleent dat u de rol Administrator voor gebruikerstoegang voor het root-bereik (`/`).

   ```http
   POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
   ```

1. Maak een [roltoewijzing](/rest/api/authorization/roleassignments) om toe te wijzen een rol op een bereik. Het volgende voorbeeld ziet u de eigenschappen voor het toewijzen van de rol {roleDefinitionID} in het bereik van de hoofdmap (`/`):

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

1. Tijdens een beheerder van gebruikerstoegang, u kunt ook roltoewijzingen weergegeven bij het root-bereik verwijderen (`/`).

1. Verwijder de bevoegdheden van de beheerder van gebruikerstoegang totdat ze weer nodig zijn.

## <a name="list-role-assignments-at-the-root-scope--using-the-rest-api"></a>Lijst van roltoewijzingen weergegeven bij het root-bereik (/) met behulp van de REST-API

U kunt vindt u al de roltoewijzingen voor een gebruiker bij het root-bereik (`/`).

- Bel [GET roleAssignments](/rest/api/authorization/roleassignments/listforscope) waar `{objectIdOfUser}` is de object-ID van de gebruiker waarvan u wilt ophalen van roltoewijzingen.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

## <a name="remove-elevated-access-using-the-rest-api"></a>Verwijderen van toegang met verhoogde bevoegdheid met behulp van de REST-API

Als u aanroept `elevateAccess`, u een roltoewijzing voor uzelf maken, dus deze bevoegdheden intrekken moet u de toewijzing verwijderen.

1. Bel [GET roleDefinitions](/rest/api/authorization/roledefinitions/get) waar `roleName` gelijk is aan de Administrator voor gebruikerstoegang om te bepalen van de naam-ID van de rol Administrator voor gebruikerstoegang.

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

    Sla de ID van de `name` parameter, in dit geval `18d7d88d-d35e-4fb5-a5c3-7773c20a72d9`.

2. U moet ook om de roltoewijzing voor tenantbeheerder binnen het tenantbereik weer te geven. Lijst van alle toewijzingen in het tenantbereik voor de `principalId` van de tenantbeheerder die de toegang toestaan tot aanroepen. Hiermee worden alle toewijzingen in de tenant voor de object-id.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Een tenantbeheerder moet niet veel toewijzingen hebben als de vorige query te veel toewijzingen, u kunt ook een query geeft voor alle toewijzingen gewoon op het niveau van bereik tenant en vervolgens de resultaten te filteren: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
    2. Het aanroepen van de vorige retourneert een lijst van roltoewijzingen. De roltoewijzing vinden waar het bereik is `"/"` en de `roleDefinitionId` eindigt met de rol van naam-ID die u in stap 1 hebt gevonden en `principalId` komt overeen met de object-id van de tenantbeheerder. 
    
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
        
    Nogmaals, sla de ID van de `name` parameter, in dit geval e7dd75bc-06f6-4e71-9014-ee96a929d099.

    3. Gebruik tot slot de roltoewijzings-ID te verwijderen van de toewijzing is toegevoegd door `elevateAccess`:

    ```http
    DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01
    ```

## <a name="next-steps"></a>Volgende stappen

- [Op rollen gebaseerd toegangsbeheer met REST](role-assignments-rest.md)
- [Toegang tot Azure-resources met Privileged Identity Management beheren](pim-azure-resource.md)
- [Toegang tot Azure-beheer met voorwaardelijke toegang beheren](conditional-access-azure-management.md)

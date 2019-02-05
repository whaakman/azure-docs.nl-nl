---
title: Toegangsrechten voor het beheren van alle Azure-abonnementen en beheergroepen | Microsoft Docs
description: Beschrijft hoe u toegangsrechten voor een globale beheerder voor het beheren van alle abonnementen en beheergroepen in Azure Active Directory met Azure portal of REST-API.
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
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: d6778e1749493a04a73d0ac210c1557b89343d00
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55695577"
---
# <a name="elevate-access-to-manage-all-azure-subscriptions-and-management-groups"></a>Toegangsrechten voor het beheren van alle Azure-abonnementen en beheergroepen

Als een globale beheerder in Azure Active Directory (Azure AD), mogelijk niet hebt toegang tot alle abonnementen en beheergroepen in uw directory. Dit artikel beschrijft de methoden die u toegang naar alle abonnementen en beheergroepen uitbreiden kunt.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="why-would-you-need-to-elevate-your-access"></a>Waarom zou u moet uw toegangsrechten?

Als u een globale beheerder bent, is er mogelijk dat u het volgende doen:

- Toegang tot een Azure-abonnement of management-groep krijgen wanneer een gebruiker heeft geen toegang meer
- Verlenen van een andere gebruiker of zelf de toegang tot een Azure-abonnement of management-groep
- Zie alle Azure-abonnementen of beheergroepen in een organisatie
- Een automation-app (zoals een facturering of controle-app) voor toegang tot alle Azure-abonnementen of beheergroepen toestaan

## <a name="how-does-elevate-access-work"></a>Hoe toegang tot werk verhogen?

Azure AD en Azure-resources onafhankelijk van elkaar zijn beveiligd. Dat wil zeggen, Azure AD-roltoewijzingen Verleen geen toegang tot Azure-resources en Azure roltoewijzingen geen toegang verlenen aan Azure AD. Echter, als u een [hoofdbeheerder](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) in Azure AD, kunt u toewijzen zelf toegang tot alle Azure-abonnementen en beheergroepen in uw directory. Deze functie gebruiken als u geen toegang tot resources van de Azure-abonnement, zoals virtuele machines of storage-accounts hebt, en u uw globale beheerder-bevoegdheden gebruiken wilt voor toegang tot deze resources.

Wanneer u uw toegangsrechten, u krijgt de [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator) rol in Azure in het bereik van de hoofdmap (`/`). Hiermee kunt u alle resources weergeven en toegang in een abonnement of beheergroep in de map toewijzen. Beheerder van gebruikerstoegang roltoewijzingen kunnen worden verwijderd met behulp van PowerShell.

Wanneer u de wijzigingen die u wilt aanbrengen in het bereik van de hoofdmap hebt gemaakt, moet u deze toegang met verhoogde bevoegdheid verwijderen.

![Toegangsrechten](./media/elevate-access-global-admin/elevate-access.png)

## <a name="azure-portal"></a>Azure Portal

Volg deze stappen om de toegangsrechten voor een globale beheerder met behulp van de Azure portal.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) of de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) als globale beheerder.

1. Klik in de navigatielijst **Azure Active Directory** en klik vervolgens op **eigenschappen**.

   ![Eigenschappen van Azure AD - schermafbeelding](./media/elevate-access-global-admin/aad-properties.png)

1. Onder **Access management voor Azure-resources**, zet de wisselknop op **Ja**.

   ![Toegangsbeheer voor Azure-resources - schermafbeelding](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Als u de wisselknop instelt op **Ja**, de rol Administrator voor gebruikerstoegang in Azure RBAC bij het root-bereik (/) aan u zijn toegewezen. Hiermee verkrijgt u machtiging voor het toewijzen van rollen in alle Azure-abonnementen en beheergroepen die zijn gekoppeld aan deze Azure AD-adreslijst. Deze optie is alleen beschikbaar voor gebruikers die de rol globale beheerder zijn toegewezen in Azure AD.

   Als u de wisselknop instelt op **Nee**, de rol Administrator voor gebruikerstoegang in Azure RBAC is verwijderd uit uw gebruikersaccount. U kunt niet meer rollen in alle Azure-abonnementen en beheergroepen die gekoppeld aan deze Azure AD-adreslijst zijn. U kunt weergeven en beheren van alleen de Azure-abonnementen en beheergroepen waaraan u toegang is verleend.

1. Klik op **opslaan** om op te slaan van de instelling.

   Deze instelling is niet een globale eigenschap en geldt alleen voor de momenteel aangemelde gebruiker. U kunt geen toegangsrechten voor alle leden van de rol globale beheerder.

1. Meld u af en meld u weer aan het vernieuwen van uw toegang.

    U hebt nu toegang tot alle abonnementen en beheergroepen in uw directory. U zult zien dat u de rol Administrator voor gebruikerstoegang in het bereik van de hoofdmap zijn toegewezen.

   ![Roltoewijzingen abonnement met een bereik van root - schermafbeelding](./media/elevate-access-global-admin/iam-root.png)

1. Breng de wijzigingen die u wilt maken op de uitgebreide toegangsrechten.

    Zie voor meer informatie over het toewijzen van rollen [toegang met RBAC en de Azure-portal beheren](role-assignments-portal.md). Als u gebruikmaakt van Azure AD Privileged Identity Management (PIM), raadpleegt u [detecteren Azure-resources te beheren in PIM](../active-directory/privileged-identity-management/pim-resource-roles-discover-resources.md) of [toewijzen Azure resource-rollen in PIM](../active-directory/privileged-identity-management/pim-resource-roles-assign-roles.md).

1. Wanneer u klaar bent, stel de **Access management voor Azure-resources** overzet naar **Nee**. Omdat dit een instelling per gebruiker is, moet u zijn aangemeld als de gebruiker als is gebruikt voor toegang met verhoogde bevoegdheden.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

### <a name="list-role-assignment-at-the-root-scope-"></a>Lijst met roltoewijzing bij het root-bereik (/)

Om de toewijzing van de rol Administrator voor gebruikerstoegang voor een gebruiker bij het bereik van de hoofdmap weer te geven (`/`), gebruikt u de [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) opdracht.

```azurepowershell
Get-AzRoleAssignment | where {$_.RoleDefinitionName -eq "User Access Administrator" `
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
CanDelegate        : False
```

### <a name="remove-a-role-assignment-at-the-root-scope-"></a>Een roltoewijzing bij het root-bereik (/) verwijderen

Verwijderen van een roltoewijzing Administrator voor gebruikerstoegang voor een gebruiker bij het root-bereik (`/`), als volgt te werk.

1. Meld u aan als een gebruiker die toegang met verhoogde bevoegdheid kunt verwijderen. Dit kan dezelfde gebruiker is die is gebruikt voor het verhogen van toegang of een andere globale beheerder met verhoogde toegang bij het root-bereik zijn.


1. Gebruik de [Remove-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment) opdracht om te verwijderen van de toewijzing van de rol Administrator voor gebruikerstoegang.

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName <username@example.com> `
      -RoleDefinitionName "User Access Administrator" -Scope "/"
    ```

## <a name="rest-api"></a>REST-API

### <a name="elevate-access-for-a-global-administrator"></a>Toegangsrechten voor een globale beheerder

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

### <a name="list-role-assignments-at-the-root-scope-"></a>Lijst met roltoewijzingen weergegeven bij de scope van de hoofdmap (/)

U kunt vindt u al de roltoewijzingen voor een gebruiker bij het root-bereik (`/`).

- Bel [GET roleAssignments](/rest/api/authorization/roleassignments/listforscope) waar `{objectIdOfUser}` is de object-ID van de gebruiker waarvan u wilt ophalen van roltoewijzingen.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

### <a name="list-deny-assignments-at-the-root-scope-"></a>Lijst met toewijzingen voor de scope hoofdmap (/) weigeren

U kunt alle toewijzingen van de weigeren voor een gebruiker bij het bereik van de hoofdmap van een lijst (`/`).

- Roep GET denyAssignments waar `{objectIdOfUser}` is de object-ID van de gebruiker waarvan de weigeren toewijzingen die u wilt ophalen.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter=gdprExportPrincipalId+eq+'{objectIdOfUser}'
   ```

### <a name="remove-elevated-access"></a>Toegang met verhoogde bevoegdheid verwijderen

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

2. U moet ook om de roltoewijzing voor de directory-beheerder in het bereik van de directory weer te geven. Lijst van alle toewijzingen in het bereik van de directory voor de `principalId` van de directorybeheerder die de toegang toestaan tot aanroepen. Hiermee worden alle toewijzingen in de map voor de object-id.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Een directory-beheerder moet niet veel toewijzingen hebben als de vorige query te veel toewijzingen, u kunt ook een query geeft voor alle toewijzingen gewoon op het niveau van bereik directory en vervolgens de resultaten te filteren: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
    2. Het aanroepen van de vorige retourneert een lijst van roltoewijzingen. De roltoewijzing vinden waar het bereik is `"/"` en de `roleDefinitionId` eindigt met de rol van naam-ID die u in stap 1 hebt gevonden en `principalId` komt overeen met de object-id van de directory-beheerder. 
    
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

- [Informatie over de verschillende rollen](rbac-and-directory-admin-roles.md)
- [Op rollen gebaseerd toegangsbeheer met REST](role-assignments-rest.md)

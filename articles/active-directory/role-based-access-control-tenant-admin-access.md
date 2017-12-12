---
title: Tenantbeheerder toegangsrechten - Azure AD uitbreiden | Microsoft Docs
description: Dit onderwerp beschrijft de ingebouwde rollen voor op rollen gebaseerde toegangsbeheer (RBAC).
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2017
ms.author: andredm
ms.openlocfilehash: 894ccd13684a79590b75821514ef6922abb8fdaf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a>Toegangsrechten uitbreiden als een tenantbeheerder met toegangsbeheer op basis van rollen

Toegangsbeheer op basis van rollen kunt tenantbeheerders tijdelijke uitbreidingen in toegang krijgen, zodat ze hoger dan normaal machtigen kunnen. Een tenantbeheerder kan zichzelf uitbreiden naar de rol beheerder voor gebruikerstoegang wanneer deze nodig is. Die rol, krijgt de tenant beheerdersmachtigingen te verlenen aan zichzelf of andere functies op het bereik '/'.

Deze functie is belangrijk omdat hiermee de tenantbeheerder om te zien van de abonnementen die zijn opgenomen in een organisatie. Kunt u ook voor automatisering apps zoals facturering en controle voor toegang tot alle abonnementen en geef een nauwkeurige weergave van de status van de organisatie voor facturerings- of beheer van bedrijfsmiddelen.  

## <a name="use-elevateaccess-for-tenant-access-with-azure-ad-admin-center"></a>ElevateAccess gebruiken voor toegang van de tenant met Azure AD-beheercentrum

1. Ga naar de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) en meld u aan met u referenties.

2. Kies **eigenschappen** links menu van Azure AD.

3. In de **eigenschappen** blade vinden **globale beheerder Azure-abonnementen kunt beheren**, kies **Ja**, klikt u vervolgens **opslaan**.
    > [!IMPORTANT] 
    > Wanneer u de optie **Ja**, wijst de **beheerder voor gebruikerstoegang** rol in de hoofdmap '/' (Root Scope) voor de gebruiker waarmee u momenteel bent aangemeld bij de Portal. **Hierdoor kan de gebruiker voor een overzicht van alle andere Azure-abonnementen.**
    
    > [!NOTE] 
    > Wanneer u de optie **Nee**, verwijdert u de **beheerder voor gebruikerstoegang** rol in de hoofdmap '/' (Root Scope) voor de gebruiker waarmee u momenteel bent aangemeld bij de Portal.

> [!TIP] 
> De indruk is dat dit is een globale eigenschap voor Azure Active Directory, maar het functioneert op basis van per gebruiker voor de momenteel aangemelde gebruiker. Wanneer u rechten voor globale beheerder in Azure Active Directory hebt, kunt u de functie elevateAccess voor de gebruiker die u momenteel bent aangemeld bij Azure Active Directory-beheercentrum kunt aanroepen.

![Azure AD-beheercentrum - eigenschappen - Globaladmin kunt beheren Azure-abonnement - schermafbeelding](./media/role-based-access-control-tenant-admin-access/aad-azure-portal-global-admin-can-manage-azure-subscriptions.png)

## <a name="view-role-assignments-at-the--scope-using-powershell"></a>Roltoewijzingen bij het "/" bereik met behulp van PowerShell weergeven
Om weer te geven de **beheerder voor gebruikerstoegang** toewijzing in de  **/**  bereik, gebruikt u de `Get-AzureRmRoleAssignment` PowerShell-cmdlet.
    
```
Get-AzureRmRoleAssignment* | where {$_.RoleDefinitionName -eq "User Access Administrator" -and $_SignInName -eq "<username@somedomain.com>" -and $_.Scope -eq "/"}
```

**Voorbeeld van uitvoer**:

RoleAssignmentId: /providers/Microsoft.Authorization/roleAssignments/098d572e-c1e5-43ee-84ce-8dc459c7e1f0    
Bereik: /    
Weergavenaam: gebruikersnaam    
SignInName:username@somedomain.com    
RoleDefinitionName: Beheerder voor gebruikerstoegang    
RoleDefinitionId: 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9    
Object-id: d65fd0e9-c185-472c-8f26-1dafa01f72cc    
ObjectType: gebruiker    

## <a name="delete-the-role-assignment-at--scope-using-powershell"></a>De roltoewijzing bij verwijderen ' / ' bereik met behulp van Powershell:
U kunt de volgende PowerShell-cmdlet met toewijzing verwijderen:
```
Remove-AzureRmRoleAssignment -SignInName <username@somedomain.com> -RoleDefinitionName "User Access Administrator" -Scope "/" 
```

## <a name="use-elevateaccess-to-give-tenant-access-with-the-rest-api"></a>ElevateAccess gebruiken voor toegang van de tenant met de REST-API

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

1.  Aanroepen van GET-roldefinities waar roleName = beheerder voor gebruikerstoegang om te bepalen van de naam van de GUID van de rol beheerder voor gebruikerstoegang.
    1.  OPHALEN van *https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$ filter = roleName + eq +'+ + beheerder voor gebruikerstoegang*

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

2. U moet er ook voor het weergeven van de roltoewijzing voor tenantbeheerder binnen het bereik van de tenant. Lijst van alle toewijzingen in het bereik van de tenant voor de PrincipalId van de TenantAdmin die de uitbreiden toegang aanroepen. Hiermee wordt een lijst van alle toewijzingen in de tenant voor de object-id. 
    1. OPHALEN van *https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$ filter = principalId + eq + {objectid}*
    
        >[!NOTE] 
        >Een tenantbeheerder mag geen veel toewijzingen hebben als de bovenstaande query te veel toewijzingen, u kunt ook een query retourneert voor alle toewijzingen van op scopeniveau tenant Just en vervolgens de resultaten filteren: ophalen *https://management.azure.com/providers/ Microsoft.Authorization/roleAssignments? api-version = 2015-07-01 & $filter=atScope()*
        
    2. Het aanroepen van de bovenstaande retourneren een lijst met roltoewijzingen. De roltoewijzing vinden waar de scope is '/' en de RoleDefinitionId eindigt met de naam van de rol GUID die u in stap 1 hebt gevonden en PrincipalId overeenkomt met de object-id van de Tenant-beheerder. De roltoewijzing ziet er als volgt:

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

    3. Gebruik tot slot de gemarkeerde **RoleAssignment ID** verwijderen van de toewijzing die door de bevoegdheden toegang toegevoegd:

        Https://management.azure.com /providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01 verwijderen

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [toegangsbeheer op basis van rollen met REST beheren](role-based-access-control-manage-access-rest.md)

- [Beheren van toegangstoewijzingen](role-based-access-control-manage-assignments.md) in de Azure portal

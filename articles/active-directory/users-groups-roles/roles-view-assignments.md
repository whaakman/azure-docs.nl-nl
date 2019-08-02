---
title: Machtigingen voor beheerdersrol weer geven in het beheer centrum-Azure Active Directory | Microsoft Docs
description: U kunt nu leden van een Azure AD-beheerdersrol weer geven en beheren in het Azure AD-beheer centrum.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf45d45481712e30d40bacec9a3c4d80d1ed56b6
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707550"
---
# <a name="view-custom-role-assignments-in-azure-active-directory"></a>Aangepaste roltoewijzingen in Azure Active Directory weer geven

In dit artikel wordt beschreven hoe u aangepaste rollen kunt weer geven die u hebt toegewezen in Azure Active Directory (Azure AD). In Azure Active Directory (Azure AD) kunnen rollen worden toegewezen op mapniveau of met een bereik van één toepassing. Roltoewijzingen in het bereik van de map worden toegevoegd aan de lijst met toewijzingen voor afzonderlijke toepassings rollen, maar roltoewijzingen in het enkele toepassings bereik worden niet toegevoegd aan de lijst met toewijzingen op mapniveau.

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-the-azure-ad-portal"></a>De toewijzingen van een rol met een directory bereik weer geven met behulp van de Azure AD-Portal

1. Meld u aan bij het [Azure AD-beheer centrum](https://aad.portal.azure.com) met privileged Role Administrator of Global Administrator Permissions in de Azure AD-organisatie.
1. Selecteer **Azure Active Directory**, selecteer **rollen en beheerders**en selecteer vervolgens een rol om deze te openen en de eigenschappen ervan weer te geven.
1. Selecteer **toewijzingen** om de toewijzingen voor de rol weer te geven.

    ![Roltoewijzingen en-machtigingen weer geven bij het openen van een rol in de lijst](./media/roles-view-assignments/role-assignments.png)

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-azure-ad-powershell"></a>De toewijzingen van een rol met een directory bereik weer geven met behulp van Azure AD Power shell

U kunt automatiseren hoe u Azure AD-beheerders rollen toewijst aan gebruikers met behulp van Azure PowerShell. In dit artikel wordt gebruikgemaakt van de [Azure Active Directory module Power shell versie 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles) .

### <a name="prepare-powershell"></a>Power shell voorbereiden

Eerst moet u [de Azure ad preview Power shell-module downloaden](https://www.powershellgallery.com/packages/AzureAD/).

Als u de Azure AD Power shell-module wilt installeren, gebruikt u de volgende opdrachten:

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

Als u wilt controleren of de module gereed is voor gebruik, gebruikt u de volgende opdracht:

``` PowerShell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

### <a name="view-the-assignments-of-a-role"></a>De toewijzingen van een rol weer geven

Voor beeld van het weer geven van de toewijzingen van een rol.

``` PowerShell
# Fetch list of all directory roles with object ID
Get-AzureADDirectoryRole

# Fetch a specific directory role by ID
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"

# Fetch role membership for a role
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
```

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-microsoft-graph-api"></a>De toewijzingen van een rol met een directory bereik weer geven met behulp van Microsoft Graph-API

HTTP-aanvraag voor het ophalen van een roltoewijzing voor een bepaalde roldefinitie.

GET

``` HTTP
https://graph.windows.net/<tenantDomain-or-tenantId>/roleAssignments?api-version=1.61-internal&$filter=roleDefinitionId eq ‘<object-id-or-template-id-of-role-definition>’
```

Antwoord

``` HTTP
HTTP/1.1 200 OK
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":["/"]
}
```

## <a name="view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview"></a>De toewijzingen van een rol met een bereik met één toepassing weer geven met behulp van de Azure AD-Portal (preview)

1. Meld u aan bij het [Azure AD-beheer centrum](https://aad.portal.azure.com) met privileged Role Administrator of Global Administrator Permissions in de Azure AD-organisatie.
1. Selecteer Azure Active Directory, selecteer **app-registraties**en selecteer vervolgens de app-registratie om de eigenschappen ervan weer te geven. Mogelijk moet u **alle toepassingen** selecteren om de volledige lijst van app-registraties in uw Azure AD-organisatie weer te geven.

    ![App-registraties maken of bewerken op de App-registraties pagina](./media/roles-create-custom/appreg-all-apps.png)

1. Selecteer **rollen en beheerders**en selecteer vervolgens een rol om de eigenschappen ervan weer te geven.

    ![Roltoewijzingen van de app-registratie weer geven op de pagina App-registraties](./media/roles-view-assignments/appreg-assignments.png)

1. Selecteer **toewijzingen** om de toewijzingen voor de rol weer te geven.

    ![Roltoewijzingen van de app-registratie weer geven vanuit de eigenschappen van een app-registratie](./media/roles-view-assignments/appreg-assignments-2.png)

## <a name="next-steps"></a>Volgende stappen

* U kunt graag delen met ons op het [forum van Azure AD-beheerders](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Zie [beheerders rollen toewijzen](directory-assign-admin-roles.md)voor meer informatie over functies en de toewijzing van beheerdersrol.
* Zie voor standaard gebruikers machtigingen een [vergelijking van de standaard machtigingen voor gast-en gebruikers rechten](../fundamentals/users-default-permissions.md).

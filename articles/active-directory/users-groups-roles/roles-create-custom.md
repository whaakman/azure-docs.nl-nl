---
title: Een aangepaste roldefinitie maken in azure AD op rollen gebaseerd toegangs beheer-Azure Active Directory | Microsoft Docs
description: Aangepaste Azure AD-rollen maken met een resource bereik op Azure Active Directory resources.
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
ms.openlocfilehash: ae773bc1a4e1831dbe462149bb827c26b7e74b96
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68722317"
---
# <a name="create-a-custom-role-and-assign-a-resource-scope-in-azure-active-directory"></a>Een aangepaste rol maken en een resource bereik toewijzen in Azure Active Directory

In dit artikel wordt beschreven hoe u nieuwe aangepaste rollen maakt in Azure Active Directory (Azure AD). U kunt aangepaste rollen maken op het tabblad **rollen en beheerders** op de pagina overzicht van Azure AD of [de pagina Toepassings registratie](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview). De rol kan worden toegewezen op het bereik van de directory-niveau of in het bereik van app-registraties.

Zie [overzicht van aangepaste functies](roles-custom-overview.md) voor de basis principes van aangepaste rollen voor meer informatie.

## <a name="using-the-azure-ad-portal"></a>De Azure AD-Portal gebruiken

### <a name="create-a-new-custom-role-to-grant-access-to-manage-app-registrations"></a>Een nieuwe aangepaste rol maken voor het verlenen van toegang voor het beheren van app-registraties

1. Meld u aan bij het [Azure AD-beheer centrum](https://aad.portal.azure.com) met privileged Role Administrator of Global Administrator Permissions in de Azure AD-organisatie.
1. Selecteer **Azure Active Directory** > **rollen en beheerders** > **nieuwe aangepaste rol**.

   ![Rollen maken of bewerken op de pagina rollen en beheerders](./media/roles-create-custom/new-custom-role.png)

1. Geef op het tabblad **basis beginselen** een naam en beschrijving voor de rol op.

   ![Geef een naam en beschrijving op voor een aangepaste rol op het tabblad basis beginselen](./media/roles-create-custom/basics-tab.png)

1. De benodigde machtigingen voor het beheren van app-registratie referenties en basis eigenschappen, zoals de naam, selecteren:
   1. Voer referenties in de zoek balk in en selecteer de `microsoft.directory/applications/credentials/update` machtiging.

      ![De machtigingen voor een aangepaste rol selecteren op het tabblad Machtigingen](./media/roles-create-custom/permissions-tab.png)

   1. Voer ' Basic ' in de zoek balk in, selecteer `microsoft.directory/applications/basic/update` de machtiging en klik vervolgens op **volgende**.
1. Controleer op het tabblad **controleren en maken** de machtigingen en selecteer **maken**.

Uw aangepaste rol wordt weer gegeven in de lijst met beschik bare rollen die moeten worden toegewezen.

## <a name="assign-a-role-scoped-to-a-resource"></a>Een rollen bereik toewijzen aan een resource

Net als ingebouwde rollen kunnen aangepaste rollen worden toegewezen aan het standaard organisatie bereik om toegang te verlenen aan alle app-registraties. Maar aangepaste rollen kunnen ook worden toegewezen bij object bereik. Zo kunt u de toegewezen gebruiker de machtiging geven om referenties en basis eigenschappen van één app bij te werken zonder dat u een tweede aangepaste rol hoeft te maken.

1. Meld u aan bij het [Azure AD-beheer centrum](https://aad.portal.azure.com) met toepassings ontwikkelaars machtigingen in de Azure AD-organisatie.
1. Selecteer **App-registraties**.
1. Selecteer de app-registratie waaraan u toegang wilt verlenen. Mogelijk moet u **alle toepassingen** selecteren om de volledige lijst van app-registraties in uw Azure AD-organisatie weer te geven.

    ![Selecteer de app-registratie als een resource bereik voor een roltoewijzing](./media/roles-create-custom/appreg-all-apps.png)

1. Selecteer in de app-registratie **rollen en beheerders**. Als u er nog geen hebt gemaakt, vindt u de instructies in de [voor gaande procedure](#create-a-new-custom-role-to-grant-access-to-manage-app-registrations).

    Als u deze rol toewijst vanuit de context van een open app-registratie, heeft de toegewezen gebruiker machtigingen voor de registratie van één app. De rol die u toewijst, wordt weer gegeven in de lijst bij elke app-registratie. Dit toegangs model, waaraan een eigenaar machtigingen kan toewijzen voor specifieke Azure AD-resources op basis van rollen, is vergelijkbaar met het model dat wordt gebruikt in [Azure RBAC](../../role-based-access-control/overview.md) voor Azure resource Access Control.

1. Selecteer de rol voor het openen van de pagina **toewijzingen** .
1. Selecteer **toewijzing toevoegen** om een gebruiker toe te voegen. De gebruiker krijgt geen machtigingen voor een andere app-registratie dan het geselecteerde abonnement.

## <a name="create-a-custom-role-using-azure-ad-powershell"></a>Een aangepaste rol maken met behulp van Azure AD Power shell

### <a name="prepare-powershell"></a>Power shell voorbereiden

Eerst moet u [de Azure AD Power shell-module downloaden](https://www.powershellgallery.com/packages/AzureAD/).

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

### <a name="create-the-custom-role"></a>De aangepaste rol maken

Maak een nieuwe rol met behulp van het volgende Power shell-script:

``` PowerShell
# Basic role information
$description = "Application Support Administrator"
$displayName = "Can manage basic aspects of application registrations."
$templateId = (New-Guid).Guid

# Set of permissions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/allProperties/read",
    "microsoft.directory/applications/basic/update",
    "microsoft.directory/applications/credentials/update"
)
$resourceActions = @{'allowedResourceActions'= $allowedResourceAction}
$rolePermission = @{'resourceActions' = $resourceActions}
$rolePermissions = $rolePermission

# Create new custom admin role
$customAdmin = New-AzureAdRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

## <a name="create-a-custom-role-using-microsoft-graph-api"></a>Een aangepaste rol maken met behulp van Microsoft Graph-API

1. De roldefinitie maken.

    HTTP-aanvraag voor het maken van een aangepaste roldefinitie.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions
    ```

    Hoofdtekst

    ``` HTTP
    {
    "description":"Can manage basic aspects of application registrations.",
    "displayName":"Application Support Administrator",
    "isEnabled":true,
    "rolePermissions":
    [
        {
            "allowedResourceActions": 
            [
                "microsoft.directory/applications/basic/read",
                "microsoft.directory/applications/credentials/update"
            ]
        }
    ]
    }
    ```

1. Maak de roltoewijzing.

    HTTP-aanvraag voor het maken van een aangepaste roldefinitie.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions/roleAssignments
    ```

    Hoofdtekst

    ``` HTTP
    {
    "principalId":"<GUID OF USER>",
    "roleDefinitionId":"<GUID OF ROLE DEFINITION>",
    "resourceScope":["/<GUID OF APPLICATION REGISTRATION>"]
    }
    ```

## <a name="next-steps"></a>Volgende stappen

- U kunt graag delen met ons op het [forum van Azure AD-beheerders](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
- Zie [beheerders rollen toewijzen](directory-assign-admin-roles.md)voor meer informatie over functies en de toewijzing van beheerdersrol.
- Zie voor standaard gebruikers machtigingen een [vergelijking van de standaard machtigingen voor gast-en gebruikers rechten](../fundamentals/users-default-permissions.md).

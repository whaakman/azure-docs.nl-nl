---
title: Zelfstudie - Toegang verlenen aan een gebruiker met RBAC en Azure PowerShell | Microsoft Docs
description: Gebruik op rollen gebaseerd toegangsbeheer (RBAC) om een gebruiker toestemming te geven om alles in een abonnement te bekijken en om alles in een resourcegroep te beheren met Azure PowerShell.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 06/11/2018
ms.author: rolyon
ms.openlocfilehash: cac585b36c3b5969a18c941215b623443850cd4c
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301725"
---
# <a name="tutorial-grant-access-for-a-user-using-rbac-and-azure-powershell"></a>Zelfstudie - Toegang verlenen aan een gebruiker met RBAC en Azure PowerShell

[Op rollen gebaseerde toegangsbeheer (RBAC)](overview.md) is de manier waarop u de toegang tot resources in Azure beheert. In deze zelfstudie geeft u een gebruiker toestemming om alles in een abonnement te bekijken en om alles in een resourcegroep te beheren met Azure PowerShell.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een gebruiker op verschillende niveaus toegang geven
> * Toegang opvragen
> * Toegang intrekken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

- Toestemming om gebruikers te maken in Azure Active Directory (of beschikken over een bestaande gebruiker)
- [Azure Cloud Shell](/azure/cloud-shell/quickstart-powershell)

## <a name="role-assignments"></a>Roltoewijzingen

In RBAC verleent u toegang door een roltoewijzing te maken. Een roltoewijzing bestaat uit drie elementen: beveiligings-principal, roldefinitie en bereik (ook wel scope of niveau genoemd). Dit zijn de twee roltoewijzingen die u gaat uitvoeren in deze zelfstudie:

| Beveiligings-principal | Roldefinitie | Bereik |
| --- | --- | --- |
| Gebruiker<br>(RBAC Tutorial User) | [Lezer](built-in-roles.md#reader) | Abonnement |
| Gebruiker<br>(RBAC Tutorial User)| [Inzender](built-in-roles.md#contributor) | Resourcegroep<br>(rbac-tutorial-resource-group) |

   ![Roltoewijzingen voor een gebruiker](./media/tutorial-role-assignments-user-powershell/rbac-role-assignments-user.png)

## <a name="create-a-user"></a>Een gebruiker maken

Als u een rol wilt toewijzen, hebt u een gebruiker, groep of service-principal nodig. Als u nog geen gebruiker hebt, kunt u er een maken.

1. Maak in Azure Cloud Shell een wachtwoord dat voldoet aan de vereisten voor wachtwoordcomplexiteit.

    ```azurepowershell
    $PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    $PasswordProfile.Password = "Password"
    ```

1. Maak een nieuwe gebruiker voor uw domein met de opdracht [New-AzureADUser](/powershell/module/azuread/new-azureaduser).

    ```azurepowershell
    New-AzureADUser -DisplayName "RBAC Tutorial User" -PasswordProfile $PasswordProfile `
      -UserPrincipalName "rbacuser@example.com" -AccountEnabled $true -MailNickName "rbacuser"
    ```
    
    ```Example
    ObjectId                             DisplayName        UserPrincipalName    UserType
    --------                             -----------        -----------------    --------
    11111111-1111-1111-1111-111111111111 RBAC Tutorial User rbacuser@example.com Member
    ```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

U gaat een resourcegroep maken om te laten zien hoe u een rol kunt toewijzen op het niveau van een resourcegroep.

1. Vraag een lijst met regiolocaties op met behulp van de opdracht [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation).

   ```azurepowershell
   Get-AzureRmLocation | select Location
   ```

1. Selecteer een locatie bij u in de buurt en wijs deze toe aan een variabele.

   ```azurepowershell
   $location = "westus"
   ```

1. Maak een nieuwe resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).

   ```azurepowershell
   New-AzureRmResourceGroup -Name "rbac-tutorial-resource-group" -Location $location
   ```

   ```Example
   ResourceGroupName : rbac-tutorial-resource-group
   Location          : westus
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
   ```

## <a name="grant-access"></a>Toegang verlenen

U verleent toegang aan een gebruiker door met de opdracht [New AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) een rol toe te wijzen. U moet hierbij de beveiligings-principal, de roldefinitie en het bereik opgeven.

1. Vraag de id van uw abonnement op met de opdracht [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

    ```azurepowershell
    Get-AzureRmSubscription
    ```

    ```Example
    Name     : Pay-As-You-Go
    Id       : 00000000-0000-0000-0000-000000000000
    TenantId : 22222222-2222-2222-2222-222222222222
    State    : Enabled
    ```

1. Sla het abonnementsbereik op in een variabele.

    ```azurepowershell
    $subScope = "/subscriptions/00000000-0000-0000-0000-000000000000"
    ```

1. Wijs de rol [Lezer](built-in-roles.md#reader) toe aan de gebruiker op abonnementsniveau.

    ```azurepowershell
    New-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/44444444-4444-4444-4444-444444444444
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

1. Wijs de rol [Inzender](built-in-roles.md#contributor) toe aan de gebruiker op het niveau van de resourcegroep.

    ```azurepowershell
    New-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

## <a name="list-access"></a>Toegang opvragen

1. Om de toegang voor het abonnement te controleren, gebruikt u de opdracht [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) om een lijst van de roltoewijzingen weer te geven.

    ```azurepowershell
    Get-AzureRmRoleAssignment -SignInName rbacuser@example.com -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    In de uitvoer ziet u dat de rol Lezer (Reader) op abonnementsniveau is toegewezen aan RBAC Tutorial User.

1. Om de toegang voor de resourcegroep te controleren, gebruikt u de opdracht [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) om een lijst van de roltoewijzingen weer te geven.

    ```azurepowershell
    Get-AzureRmRoleAssignment -SignInName rbacuser@example.com -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    In de uitvoer ziet u dat zowel de rol Inzender (Contributor) als Lezer (Reader) is toegewezen aan RBAC Tutorial User. De rol Inzender is toegewezen aan de resourcegroep rbac-tutorial-resource-group en de rol Lezer wordt overgenomen op abonnementsniveau.

## <a name="optional-list-access-using-the-azure-portal"></a>(Optioneel) Toegang opvragen via Azure Portal

1. Als u de roltoewijzingen wilt bekijken in Azure Portal, gaat u naar de blade **Toegangsbeheer (IAM)** voor het abonnement.

    ![Roltoewijzingen voor een gebruiker op abonnementsniveau](./media/tutorial-role-assignments-user-powershell/role-assignments-subscription-user.png)

1. Open de blade **Toegangsbeheer (IAM)** voor de resourcegroep.

    ![Roltoewijzingen voor een gebruiker op het niveau van een resourcegroep](./media/tutorial-role-assignments-user-powershell/role-assignments-resource-group-user.png)

## <a name="remove-access"></a>Toegang intrekken

U kunt de toegang voor gebruikers, groepen en toepassingen intrekken door met de opdracht [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) een roltoewijzing te verwijderen.

1. Gebruik de volgende opdracht om op het niveau van de resourcegroep de toewijzing van de rol Inzender te verwijderen voor de gebruiker.

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

1. Gebruik de volgende opdracht om op abonnementsniveau de toewijzing van de rol Lezer te verwijderen voor de gebruiker.

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

## <a name="clean-up-resources"></a>Resources opschonen

Als u de in deze zelfstudie gemaakte resources wilt opschonen, verwijdert u de resourcegroep en de gebruiker.

1. Verwijder de resourcegroep met de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

    ```azurepowershell
    Remove-AzureRmResourceGroup -Name "rbac-tutorial-resource-group"
    ```

    ```Example
    Confirm
    Are you sure you want to remove resource group 'rbac-tutorial-resource-group'
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```
    
1. Wanneer u wordt gevraagd om de bewerking te bevestigen, typt u **Y**. Het duurt een paar seconden om de groep te verwijderen.

1. De gebruiker kunt u verwijderen met de opdracht [Remove-AzureADUser](/powershell/module/azuread/remove-azureaduser).

    ```azurepowershell
    Remove-AzureADUser -ObjectId "rbacuser@example.com"
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Toegang beheren met RBAC en PowerShell](role-assignments-powershell.md)

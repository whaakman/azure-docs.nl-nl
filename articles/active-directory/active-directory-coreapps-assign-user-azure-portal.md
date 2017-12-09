---
title: Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory | Microsoft Docs
description: Hoe u een enterprise app selecteren om een gebruiker of groep aan toewijzen in Azure Active Directory
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: daveba
ms.reviewer: luleon
ms.openlocfilehash: 827d5c8669eb54630adbaba0306e73b5de6a7137
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory
Als u wilt een gebruiker of groep toewijzen aan een enterprise-app, moet u de juiste machtigingen voor het beheren van de enterprise-app hebt en u moet een globale beheerder voor de map.
> [!NOTE]
> Voor Microsoft Applications (zoals Office 365-apps), PowerShell gebruikers toewijzen aan een enterprise-app te gebruiken.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app-in-the-azure-portal"></a>Hoe wijs ik toegang voor gebruikers in een enterprise-app in de Azure-portal
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **meer services**, Azure Active Directory invoeren in het tekstvak en selecteer vervolgens **Enter**.
3. Op de **Azure Active Directory - *directoryname***  blade (dat wil zeggen, de Azure AD blade voor de map die u beheert), selecteer **bedrijfstoepassingen**.

    ![Openen zakelijke apps](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. Op de **bedrijfstoepassingen** blade Selecteer **alle toepassingen**. Hier ziet u de apps die u kunt beheren.
5. Op de **bedrijfstoepassingen - alle toepassingen** blade, selecteert u een app.
6. Op de ***appname*** blade (dat wil zeggen, de blade met de naam van de geselecteerde app in de titel), selecteer **gebruikers en groepen**.

    ![De opdracht Alles toepassingen selecteren](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. Op de ***appname*** **-gebruiker & groepstoewijzing** blade, selecteer de **toevoegen** opdracht.
8. Op de **toevoegen toewijzing** blade Selecteer **gebruikers en groepen**.

    ![Een gebruiker of groep toewijzen aan de app.](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. Op de **gebruikers en groepen** blade gebruikers of groepen uit de lijst selecteren en selecteer vervolgens de **Selecteer** knop aan de onderkant van de blade.
10. Op de **toevoegen toewijzing** blade Selecteer **rol**. Klik op de **rol selecteren** blade, selecteert u een rol op de geselecteerde gebruikers of groepen wilt toepassen en selecteer vervolgens de **OK** knop aan de onderkant van de blade.
11. Op de **toevoegen toewijzing** blade, selecteer de **toewijzen** knop aan de onderkant van de blade. De toegewezen gebruikers of groepen hebt de machtigingen die zijn gedefinieerd door de geselecteerde rol voor deze app voor de onderneming.

## <a name="how-do-i-assign-a-user-to-an-enterprise-app-using-powershell"></a>Hoe wijs ik een gebruiker naar een enterprise-app met behulp van PowerShell

1. Open een opdrachtprompt van Windows PowerShell.

    >[!NOTE] 
    > U moet de AzureAD-module installeren (Gebruik de opdracht `Install-Module -Name AzureAD`). Als u wordt gevraagd om een NuGet-module of de nieuwe Azure Active Directory V2 PowerShell-module te installeren, typt u j en druk op ENTER.

2. Voer `Connect-AzureAD` en meld u aan met een gebruikersaccount globale beheerder.
3. Gebruik het volgende script een gebruiker en de rol toewijzen aan een toepassing:

    ```powershell
    # Assign the values to the variables
    $username = "<You user's UPN>"
    $app_name = "<Your App's display name>"
    $app_role_name = "<App role display name>"
    
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```     

Voor meer informatie over het toewijzen van een gebruiker aan een toepassingsrol gaat u naar de documentatie voor [nieuw AzureADUserAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0)

### <a name="example"></a>Voorbeeld

In dit voorbeeld wordt de gebruiker Britta Simon toegewezen aan de [Microsoft werkplek Analytics](https://products.office.com/en-us/business/workplace-analytics) toepassing met behulp van PowerShell.

1. Toewijzen in PowerShell de bijbehorende waarden voor de variabelen $username, $app_naam en $app_role_name. 

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

2. In dit voorbeeld weten niet we wat de exacte naam van de toepassingsrol die we wilt toewijzen aan Britta Simon is. Voer de volgende opdrachten om op te halen van de gebruiker ($user) en de service-principal ($sp) gebruikt de UPN van de gebruiker en de service-principal weergavenamen.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```
        
3. Voer de opdracht `$sp.AppRoles` om de functies die beschikbaar zijn voor de toepassing werkplek Analytics weer te geven. In dit voorbeeld willen we rol toe te wijzen Britta Simon de analist (beperkte toegang).
    
    ![Werkplek Analytics rol](media/active-directory-coreapps-assign-user-azure-portal/workplace-analytics-role.png)

4. Toewijzen van de naam van de rol aan de `$app_role_name` variabele.
        
    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    ```

5. Voer de volgende opdracht om de gebruiker toewijzen aan de app-rol:

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="next-steps"></a>Volgende stappen
* [Zie al mijn groepen](active-directory-groups-view-azure-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Uitschakelen van de gebruikersaanmeldingen voor een enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
* [Wijzig de naam of het logo van een enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)

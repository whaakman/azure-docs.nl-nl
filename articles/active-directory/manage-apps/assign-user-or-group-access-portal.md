---
title: Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory | Microsoft Docs
description: Het selecteren van een zakelijke app toewijzen van een gebruiker of groep toe in Azure Active Directory
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: celested
ms.reviewer: luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4daab0255e739e011dca3ae0a016e3a15c7213b0
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/15/2019
ms.locfileid: "59565781"
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory
Als u wilt een gebruiker of groep aan een enterprise-app toewijst, moet u de juiste machtigingen voor het beheren van de enterprise-app hebt en u moet globale beheerder voor de map.

> [!NOTE]
> Zie voor de licentievereisten voor de functies die in dit artikel worden besproken, de [Azure Active Directory pagina met prijzen](https://azure.microsoft.com/pricing/details/active-directory).

> [!NOTE]
> Voor Microsoft Applications (zoals Office 365-apps), PowerShell gebruikers toewijzen aan een enterprise-app te gebruiken.


## <a name="assign-a-user-to-an-app---portal"></a>Een gebruiker toewijzen aan een app - portal
1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een account van een globale beheerder voor de directory.
1. Selecteer **alle services**, voert u Azure Active Directory in het tekstvak in en selecteer vervolgens **Enter**.
1. Selecteer **bedrijfstoepassingen**.
1. Op de **bedrijfstoepassingen - alle toepassingen** deelvenster ziet u een lijst van de apps die u kunt beheren. Selecteer een app.
1. Op de ***appname*** deelvenster (dat wil zeggen, het deelvenster met de naam van de geselecteerde app in de titel), selecteer **gebruikers en groepen**.
1. Op de ***appname*** **-gebruikers en groepen** venster **gebruiker toevoegen**.
1. Op de **toevoegen toewijzing** venster **gebruikers en groepen**.

    ![Een gebruiker of groep toewijzen aan de app.](./media/assign-user-or-group-access-portal/assign-users.png)
1. Op de **gebruikers en groepen** deelvenster, selecteer een of meer gebruikers of groepen in de lijst en kies vervolgens de **Selecteer** knop aan de onderkant van het deelvenster.
1. Op de **toevoegen toewijzing** venster **rol**. Klik vervolgens op de **rol selecteren** deelvenster, selecteert u een rol toe te passen op de geselecteerde gebruikers of groepen, selecteer vervolgens **OK** aan de onderkant van het deelvenster.
1. Op de **toevoegen toewijzing** venster de **toewijzen** knop aan de onderkant van het deelvenster. De toegewezen gebruikers of groepen hebt de machtigingen die zijn gedefinieerd door de geselecteerde rol voor deze app enterprise.

## <a name="allow-all-users-to-access-an-app---portal"></a>Hierdoor kunnen alle gebruikers toegang tot een app - portal
Om alle gebruikers toegang tot een toepassing:

1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een account van een globale beheerder voor de directory.
1. Selecteer **alle services**, voert u Azure Active Directory in het tekstvak in en selecteer vervolgens **Enter**.
1. Selecteer **bedrijfstoepassingen**.
1. Op de **bedrijfstoepassingen** venster **alle toepassingen**. Hier ziet u de apps die u kunt beheren.
1. Op de **bedrijfstoepassingen - alle toepassingen** deelvenster, selecteert u een app.
1. Op de ***appname*** venster **eigenschappen**.
1. Op de  ***appname* -eigenschappen** instellen in het deelvenster de **Gebruikerstoewijzing vereist?** instelt op **Nee**. 

De **Gebruikerstoewijzing vereist?** optie:

- Heeft geen invloed op al dan niet een toepassing wordt weergegeven in het toegangsvenster voor de toepassing. Als u wilt weergeven van de toepassing in het toegangsvenster, moet u een gebruiker of groep toewijzen aan de toepassing.
- Alleen met de cloudtoepassingen die zijn geconfigureerd voor eenmalige aanmelding SAML-functies, en on-premises toepassingen die zijn geconfigureerd met Application Proxy. Zie [eenmalige aanmelding voor toepassingen](what-is-single-sign-on.md).
- Vereist dat gebruikers toestemming voor een toepassing geven. Een beheerder kan toestemming voor alle gebruikers verlenen.  Zie [configureren die de manier waarop eindgebruikers toestemming voor een toepassing geven](configure-user-consent.md).


## <a name="assign-a-user-to-an-app---powershell"></a>Een gebruiker toewijzen aan een app - PowerShell

1. Open een opdrachtprompt met verhoogde bevoegdheid Windows PowerShell.

    >[!NOTE] 
    > U moet de AzureAD-module installeren (Gebruik de opdracht `Install-Module -Name AzureAD`). Als u hierom wordt gevraagd om een NuGet-module of de nieuwe Azure Active Directory V2 PowerShell-module te installeren, typt u Y en druk op ENTER.

1. Voer `Connect-AzureAD` en meld u aan met een globale beheerdersaccount voor de gebruiker.
1. Gebruik het volgende script een gebruiker en rol toewijzen aan een toepassing:

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

Voor meer informatie over hoe u een gebruiker toewijzen aan een toepassingsrol gaat u naar de documentatie voor [New-AzureADUserAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0)

Als u wilt een groep toewijzen aan een enterprise-app, moet u vervangen `Get-AzureADUser` met `Get-AzureADGroup`.

### <a name="example"></a>Voorbeeld

In dit voorbeeld wordt de gebruiker Britta Simon toegewezen aan de [Microsoft Workplace Analytics](https://products.office.com/business/workplace-analytics) toepassing met behulp van PowerShell.

1. Toewijzen in PowerShell, de overeenkomstige waarden ervan op de variabelen $username $app_name en $app_role_name. 

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

1. We weten niet wat de exacte naam van de toepassingsrol die we wilt toewijzen aan Britta Simon is in dit voorbeeld. Voer de volgende opdrachten om op te halen van de gebruiker ($user) en de weergavenamen van de service-principal ($sp) met behulp van de gebruiker UPN en de service-principal.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```
        
1. Voer de opdracht `$sp.AppRoles` om de rollen die beschikbaar zijn voor de toepassing Workplace Analytics weer te geven. In dit voorbeeld dat we willen Britta Simon de rol van de analist (beperkte toegang) toewijzen.
    
    ![Workplace Analytics rol](./media/assign-user-or-group-access-portal/workplace-analytics-role.png)

1. Toewijzen van de naam van de rol aan de `$app_role_name` variabele.
        
    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    ```

1. Voer de volgende opdracht uit om de gebruiker toewijzen aan de rol van de app:

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="next-steps"></a>Volgende stappen
* [Zie alle Mijn groepen](../fundamentals/active-directory-groups-view-azure-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](remove-user-or-group-access-portal.md)
* [Gebruikersaanmeldingen voor een zakelijke app uitschakelen](disable-user-sign-in-portal.md)
* [De naam of het logo van een zakelijke app wijzigen](change-name-or-logo-portal.md)

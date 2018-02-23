---
title: De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory | Microsoft Docs
description: De toewijzing van de toegang van een gebruiker of groep van een enterprise-app in Azure Active Directory verwijderen
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: markvi
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 084ffcbe473290a8734b1c8b8847b517dac4f6b6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory
Het is gemakkelijk om te verwijderen van een gebruiker of groep toegang wordt toegewezen aan een van uw zakelijke toepassingen in Azure Active Directory (Azure AD). U moet de juiste machtigingen voor het beheren van de app voor de onderneming hebben en u moet een globale beheerder voor de map.

> [!NOTE]
> Voor Microsoft Applications (zoals Office 365-apps), PowerShell te gebruiken om gebruikers in een enterprise-app te verwijderen.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Hoe verwijder ik een gebruiker of groepstoewijzing aan een enterprise-app in de Azure portal?
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **meer services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
3. Op de **Azure Active Directory - *directoryname***  pagina (dat wil zeggen, de Azure AD pagina voor de map die u beheert), selecteer **bedrijfstoepassingen**.

    ![Openen zakelijke apps](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. Op de **bedrijfstoepassingen** pagina **alle toepassingen**. Hier ziet u een lijst met de apps die u kunt beheren.
5. Op de **bedrijfstoepassingen - alle toepassingen** pagina, selecteert u een app.
6. Op de ***appname*** pagina (dat wil zeggen, de pagina met de naam van de geselecteerde app in de titel), selecteer **gebruikers en groepen**.

    ![Gebruikers of groepen selecteren](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. Op de ***appname*** **-gebruiker & groepstoewijzing** pagina, een van meer gebruikers of groepen selecteren en selecteer vervolgens de **verwijderen** opdracht. Bevestig uw beslissing bij de opdrachtprompt.

    ![De opdracht Remove selecteren](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>Hoe verwijder ik een gebruiker of groepstoewijzing aan een enterprise-app met behulp van PowerShell
1. Open een opdrachtprompt van Windows PowerShell.

    >[!NOTE] 
    > U moet de AzureAD-module installeren (Gebruik de opdracht `Install-Module -Name AzureAD`). Als u wordt gevraagd om een NuGet-module of de nieuwe Azure Active Directory V2 PowerShell-module te installeren, typt u j en druk op ENTER.

2. Voer `Connect-AzureAD` en meld u aan met een gebruikersaccount globale beheerder.
3. Gebruik het volgende script een gebruiker en de rol toewijzen aan een toepassing:

    ```powershell
    # Store the proper parameters
    $user = get-azureaduser -ObjectId <objectId>
    $spo = Get-AzureADServicePrincipal -ObjectId <objectId>

    #Get the ID of role assignment 
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId | Where {$_.PrincipalDisplayName -eq $user.DisplayName}

    #if you run the following, it will show you what is assigned what
    $assignments | Select *

    #To remove the App role assignment run the following command.
    Remove-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId -AppRoleAssignmentId $assignments[assignment #].ObjectId
    ``` 
## <a name="next-steps"></a>Volgende stappen

- [Zie al mijn groepen](active-directory-groups-view-azure-portal.md)
- [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)
- [Uitschakelen van de gebruikersaanmeldingen voor een enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
- [Wijzig de naam of het logo van een enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)

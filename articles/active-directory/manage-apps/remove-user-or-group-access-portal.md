---
title: De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory | Microsoft Docs
description: De toegang tot de toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/14/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 709489a1e96691a677261bfb2c7b5c29158e62c9
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55162444"
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory
Het is gemakkelijk om te verwijderen van een gebruiker of groep toegang worden toegewezen aan een van uw bedrijfstoepassingen in Azure Active Directory (Azure AD). U moet de juiste machtigingen voor het beheren van de enterprise-app hebt en u moet globale beheerder voor de map.

> [!NOTE]
> Voor Microsoft Applications (zoals Office 365-apps), moet u PowerShell gebruiken om gebruikers met een enterprise-app te verwijderen.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Hoe verwijder ik een gebruiker of groepstoewijzing aan een enterprise-app in Azure portal?
1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een account van een globale beheerder voor de directory.
2. Selecteer **meer services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
3. Op de **Azure Active Directory - *directoryname***  pagina (dat wil zeggen, de Azure AD-pagina voor de map die u beheert), selecteer **bedrijfstoepassingen**.

    ![Bedrijfsapps openen](./media/remove-user-or-group-access-portal/open-enterprise-apps.png)
4. Op de **bedrijfstoepassingen** weergeeft, schakelt **alle toepassingen**. U ziet een lijst van de apps die u kunt beheren.
5. Op de **bedrijfstoepassingen - alle toepassingen** pagina, selecteert u een app.
6. Op de ***appname*** pagina (dat wil zeggen, de pagina met de naam van de geselecteerde app in de titel), selecteer **gebruikers en groepen**.

    ![Gebruikers of groepen selecteren](./media/remove-user-or-group-access-portal/remove-app-users.png)
7. Op de ***appname*** **-gebruiker & groepstoewijzing** pagina, selecteer een van de meer gebruikers of groepen en selecteer vervolgens de **verwijderen** opdracht. Controleer of uw beslissing bij de prompt.

    ![De opdracht Remove selecteren](./media/remove-user-or-group-access-portal/remove-users.png)

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>Hoe verwijder ik een gebruiker of groepstoewijzing aan een enterprise-app met behulp van PowerShell?
1. Open een opdrachtprompt met verhoogde bevoegdheid Windows PowerShell.

    >[!NOTE] 
    > U moet de AzureAD-module installeren (Gebruik de opdracht `Install-Module -Name AzureAD`). Als u hierom wordt gevraagd om een NuGet-module of de nieuwe Azure Active Directory V2 PowerShell-module te installeren, typt u Y en druk op ENTER.

2. Voer `Connect-AzureAD` en meld u aan met een globale beheerdersaccount voor de gebruiker.
3. Gebruik het volgende script een gebruiker en rol toewijzen aan een toepassing:

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

- [Zie alle Mijn groepen](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Een gebruiker of groep toewijzen aan een enterprise-app](assign-user-or-group-access-portal.md)
- [Gebruikersaanmeldingen voor een zakelijke app uitschakelen](disable-user-sign-in-portal.md)
- [De naam of het logo van een zakelijke app wijzigen](change-name-or-logo-portal.md)

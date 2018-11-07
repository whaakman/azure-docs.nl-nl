---
title: Toevoegen van een nieuw account voor Azure Stack-tenant in Azure Active Directory | Microsoft Docs
description: Na de implementatie van Microsoft Azure Stack Development Kit, moet u ten minste één tenant-gebruikersaccount maken, zodat u de tenantportal kunt verkennen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: a75d5c88-5b9e-4e9a-a6e3-48bbfa7069a7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: fa8c8da34a687edd1bd92c1d516183ee5d3e1bd0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240117"
---
# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Een nieuw account voor Azure Stack-tenant in Azure Active Directory toevoegen

Na [implementeren van de Azure Stack Development Kit](azure-stack-run-powershell-script.md), moet u een gebruikersaccount van de tenant zodat u kunt de tenantportal verkennen en testen van uw aanbiedingen en plannen. U kunt een tenantaccount door maken [met behulp van de Azure-portal](#create-an-azure-stack-tenant-account-using-the-azure-portal) of door [met behulp van PowerShell](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Een Azure Stack-tenant-account maken met de Azure-portal

Hebt u een Azure-abonnement het gebruik van Azure portal.

1. Aanmelden bij [Azure](https://portal.azure.com).
2. Selecteer in de linker navigatiebalk **Active Directory** en Ga naar de map die u wilt gebruiken voor Azure Stack, of een nieuwe maken.
3. Selecteer **Azure Active Directory** > **gebruikers** > **nieuwe gebruiker**.

    ![Gebruikers - pagina voor alle gebruikers met de nieuwe gebruiker is gemarkeerd](media/azure-stack-add-new-user-aad/new-user-all-users.png)

4. Op de **gebruiker** pagina, de vereiste gegevens invullen.

    ![Nieuwe gebruiker, met gebruikersgegevens op de pagina gebruiker toevoegen](media/azure-stack-add-new-user-aad/new-user-user.png)

    - **Naam (vereist).** De eerste en laatste naam van de nieuwe gebruiker. Bijvoorbeeld, Mary Parker.
    - **Gebruikersnaam (vereist).** De gebruikersnaam van de nieuwe gebruiker. Bijvoorbeeld mary@contoso.com.
        Het domeingedeelte van de gebruikersnaam moet een van beide de aanvankelijke standaarddomeinnaam, gebruiken <_uwdomeinnaam_>. onmicrosoft.com of een aangepaste domeinnaam, bijvoorbeeld contoso.com. Zie voor meer informatie over het maken van een aangepaste domeinnaam [een aangepaste domeinnaam toevoegen aan Azure Active Directory](../active-directory/fundamentals/add-custom-domain.md).
    - **Profiel.** U kunt desgewenst meer informatie over de gebruiker toevoegen. U kunt ook gebruikersgegevens op een later tijdstip toevoegen. Zie voor meer informatie over het toevoegen van gebruikersgegevens [toevoegen of wijzigen van de informatie uit gebruikersprofielen](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md).
    - **Directory-rol.**  Kies **gebruiker**.

5. Controleer **wachtwoord weergeven** en kopieert u de automatisch gegenereerde wachtwoord opgegeven in de **wachtwoord** vak. U hebt dit wachtwoord voor het initiële aanmeldingsproces.

6. Selecteer **Maken**.

    De gebruiker is gemaakt en toegevoegd aan uw Azure AD-tenant.

7. Aanmelden bij Microsoft Azure-portal met het nieuwe account. Wijzig het wachtwoord wanneer hierom wordt gevraagd.
8. Aanmelden bij `https://portal.local.azurestack.external` met het nieuwe account om te zien van de tenantportal.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Maak een Azure Stack-tenant-account met behulp van PowerShell

Als u geen Azure-abonnement hebt, kunt u de Azure-portal niet gebruiken om toe te voegen een tenant-gebruikersaccount. In dit geval kunt u de Azure Active Directory-Module voor Windows PowerShell in plaats daarvan.

> [!NOTE]
> Als u Microsoft-Account (Live ID) gebruikt voor het implementeren van Azure Stack Development Kit, kunt u AAD PowerShell niet gebruiken voor de tenantaccount maken. 
> 
> 

1. Installeer de [Microsoft Online Services-aanmeldhulp voor IT-Professionals RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).
2. Installeer de [Azure Active Directory-Module voor Windows PowerShell (64-bits versie)](https://go.microsoft.com/fwlink/p/?linkid=236297) en open het bestand.
3. Voer de volgende cmdlets:

    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack Development Kit

            $msolcred = get-credential

    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".

            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId

    ```

1. Meld u aan Microsoft Azure met het nieuwe account. Wijzig het wachtwoord wanneer hierom wordt gevraagd.
2. Aanmelden bij `https://portal.local.azurestack.external` met het nieuwe account om te zien van de tenantportal.


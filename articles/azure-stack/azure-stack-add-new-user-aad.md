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
ms.date: 02/21/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: c312658750c1e9ef024a837ccc16e5cd5be8a5ef
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35914268"
---
# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Een nieuw account voor Azure Stack-tenant in Azure Active Directory toevoegen
Na [implementeren van de Azure Stack Development Kit](azure-stack-run-powershell-script.md), moet u een gebruikersaccount van de tenant zodat u kunt de tenantportal verkennen en testen van uw aanbiedingen en plannen. U kunt een tenantaccount door maken [met behulp van de Azure-portal](#create-an-azure-stack-tenant-account-using-the-azure-portal) of door [met behulp van PowerShell](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Een Azure Stack-tenant-account maken met de Azure-portal
Hebt u een Azure-abonnement het gebruik van Azure portal.

1. Aanmelden bij [Azure](https://portal.azure.com).
2. Klik in de linker navigatiebalk van Microsoft Azure, op **Active Directory**.
3. In de adreslijst, klikt u op de map die u wilt gebruiken voor Azure Stack of een nieuwe maken.
4. Klik op deze directorypagina **gebruikers**.
5. Klik op **Gebruiker toevoegen**.
6. In de **gebruiker toevoegen** wizard, in de **Type gebruiker** Kies **nieuwe gebruiker in uw organisatie**.
7. In de **gebruikersnaam** typt u een naam voor de gebruiker.
8. In de **@** Kies de juiste vermelding.
9. Klik op de pijl Volgende.
10. In de **gebruikersprofiel** pagina van de wizard, typ een **voornaam**, **achternaam**, en **weergavenaam**.
11. In de **rol** Kies **gebruiker**.
12. Klik op de pijl Volgende.
13. Op de **tijdelijk wachtwoord** pagina, klikt u op **maken**.
14. Kopieer de **nieuw wachtwoord**.
15. Meld u aan Microsoft Azure met het nieuwe account. Wijzig het wachtwoord wanneer hierom wordt gevraagd.
16. Aanmelden bij `https://portal.local.azurestack.external` met het nieuwe account om te zien van de tenantportal.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Maak een Azure Stack-tenant-account met behulp van PowerShell
Als u geen Azure-abonnement hebt, kunt u de Azure-portal niet gebruiken om toe te voegen een tenant-gebruikersaccount. In dit geval kunt u de Azure Active Directory-Module voor Windows PowerShell in plaats daarvan.

> [!NOTE]
> Als u Microsoft-Account (Live ID) gebruikt voor het implementeren van Azure Stack Development Kit, kunt u AAD PowerShell niet gebruiken voor de tenantaccount maken. 
> 
> 

1. Installeer de [Microsoft Online Services-aanmeldhulp voor IT-Professionals RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).
2. Installeer de [Azure Active Directory-Module voor Windows PowerShell (64-bits versie)](http://go.microsoft.com/fwlink/p/?linkid=236297) en open het bestand.
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


---
title: Toevoegen van een nieuw account voor Azure-Stack-tenant in Azure Active Directory | Microsoft Docs
description: "Na implementatie van Microsoft Azure-Stack Development Kit, moet u ten minste één tenant-gebruikersaccount maken zodat u de tenantportal kunt verkennen."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a75d5c88-5b9e-4e9a-a6e3-48bbfa7069a7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: b7fd3c36825746a009c01c97fb8664e04278159f
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
*Van toepassing op: Azure stapelen Development Kit*

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Toevoegen van een nieuw account voor Azure-Stack-tenant in Azure Active Directory
Na [implementeren van de Azure-Stack Development Kit](azure-stack-run-powershell-script.md), moet u een gebruikersaccount tenant zodat u kunt de tenantportal verkennen en testen van uw aanbiedingen en plannen. U kunt een tenantaccount maken [met de Azure portal](#create-an-azure-stack-tenant-account-using-the-azure-portal) of door [met behulp van PowerShell](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Een Azure-Stack tenant-account maken met de Azure-portal
U moet de Azure portal gebruiken een Azure-abonnement hebben.

1. Meld u aan bij [Azure](https://portal.azure.com).
2. Klik in Microsoft Azure linkernavigatiebalk op **Active Directory**.
3. Klik op de map die u wilt gebruiken voor Azure-Stack in de lijst directory of maak een nieuwe.
4. Klik op deze directorypagina **gebruikers**.
5. Klik op **gebruiker toevoegen**.
6. In de **gebruiker toevoegen** wizard in de **Type gebruiker** Kies **nieuwe gebruiker in uw organisatie**.
7. In de **gebruikersnaam** typt u een naam voor de gebruiker.
8. In de  **@**  Kies het desbetreffende item.
9. Klik op de pijl Volgende.
10. In de **gebruikersprofiel** pagina van de wizard, typ een **voornaam**, **achternaam**, en **weergavenaam**.
11. In de **rol** Kies **gebruiker**.
12. Klik op de pijl Volgende.
13. Op de **tijdelijk wachtwoord** pagina, klikt u op **maken**.
14. Kopieer de **nieuw wachtwoord**.
15. Meld u aan bij Microsoft Azure met het nieuwe account. Wijzig het wachtwoord als u wordt gevraagd.
16. Meld u aan bij `https://portal.local.azurestack.external` met het nieuwe account voor een overzicht van de tenantportal.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Een Azure-Stack tenant-account maken met PowerShell
Als u geen Azure-abonnement hebt, kunt u de Azure-portal niet gebruiken voor het toevoegen van een tenant-gebruikersaccount. In dit geval kunt u de Azure Active Directory-Module voor Windows PowerShell in plaats daarvan.

> [!NOTE]
> Als u van Microsoft-Account (Live ID gebruikmaakt) Azure Stack Development Kit implementeren, kunt u AAD PowerShell niet gebruiken om tenantaccount te maken. 
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

1. Aanmelden bij Microsoft Azure met het nieuwe account. Wijzig het wachtwoord als u wordt gevraagd.
2. Aanmelden bij `https://portal.local.azurestack.external` met het nieuwe account voor een overzicht van de tenantportal.


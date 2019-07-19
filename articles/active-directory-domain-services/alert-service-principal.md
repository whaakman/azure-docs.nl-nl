---
title: 'Azure Active Directory Domain Services: Problemen met Service-principals oplossen | Microsoft Docs'
description: Problemen met de configuratie van de service-principal voor Azure AD Domain Services oplossen
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: ''
editor: ''
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: 9e5fa8c84f5e7ca58117666846b603a118826150
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234146"
---
# <a name="troubleshoot-invalid-service-principal-configurations-for-azure-active-directory-domain-services"></a>Problemen met ongeldige Service-Principal-configuraties voor Azure Active Directory Domain Services oplossen

Dit artikel helpt u bij het oplossen van problemen met Service-Principal-gerelateerde configuratie fouten die resulteren in het volgende waarschuwings bericht:

## <a name="alert-aadds102-service-principal-not-found"></a>AADDS102 waarschuwing: Service-Principal is niet gevonden

**Waarschuwings bericht:** *Een service-principal die vereist is voor een goede werking van Azure AD Domain Services, is verwijderd uit uw Azure AD-adres lijst. Deze configuratie is van invloed op de mogelijkheid van micro soft om uw beheerde domein te bewaken, te beheren, te patchen en te synchroniseren.*

[Service](../active-directory/develop/app-objects-and-service-principals.md) -principals zijn toepassingen die door micro soft worden gebruikt voor het beheren, bijwerken en onderhouden van uw beheerde domein. Als ze worden verwijderd, verbreekt het de mogelijkheid van micro soft om uw domein te onderhouden.


## <a name="check-for-missing-service-principals"></a>Controleren op ontbrekende service-principals
Gebruik de volgende stappen om te bepalen welke service-principals opnieuw moeten worden gemaakt:

1. Ga naar de pagina [Enter prise Applications-alle toepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) in de Azure Portal.
2. Selecteer in de vervolg keuzelijst **weer geven** **alle toepassingen** en klik op **Toep assen**.
3. Gebruik de volgende tabel om te zoeken naar elke toepassings-ID door de ID in het zoekvak te plakken en op ENTER te drukken. Als de zoek resultaten leeg zijn, moet u de Service-Principal opnieuw maken door de stappen in de kolom oplossing te volgen.

| Toepassings-id | Oplossing |
| :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Een ontbrekende Service-Principal opnieuw maken met Power shell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Opnieuw registreren bij de micro soft. AAD-naam ruimte](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Opnieuw registreren bij de micro soft. AAD-naam ruimte](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Opnieuw registreren bij de micro soft. AAD-naam ruimte](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>Een ontbrekende Service-Principal opnieuw maken met Power shell
Volg deze stappen als u een service-principal met ```2565bd9d-da50-47d4-8b85-4c97f669dc36``` de ID ontbreekt in uw Azure AD-adres lijst.

**Opgelost** U hebt Azure AD Power shell nodig om deze stappen uit te voeren. Zie [dit artikel](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.)voor meer informatie over het installeren van Azure AD Power shell.

Om dit probleem op te lossen, typt u de volgende opdrachten in een Power shell-venster:
1. Installeer de Azure AD Power shell-module en importeer deze.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. Controleer of de vereiste service-principal voor Azure AD Domain Services ontbreekt in uw directory door de volgende Power shell-opdracht uit te voeren:

    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```

3. Maak de Service-Principal door de volgende Power shell-opdracht te typen:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

4. Nadat u de ontbrekende Service-Principal hebt gemaakt, wacht u twee uur en controleert u de status van uw beheerde domein.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Meld u opnieuw aan bij de micro soft AAD-naam ruimte met behulp van de Azure Portal
Volg deze stappen als u een service-principal met ```443155a6-77f3-45e3-882b-22b3a8d431fb``` de ```abba844e-bc0e-44b0-947a-dc74e5d09022``` id ```d87dcbc6-a371-462e-88e3-28ad15ec4e64``` of ontbreekt in uw Azure AD-adres lijst.

**Opgelost** Gebruik de volgende stappen om de domein Services in uw directory te herstellen:

1. Navigeer naar de pagina [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in het Azure Portal.
2. Kies het abonnement uit de tabel die is gekoppeld aan uw beheerde domein
3. Kies **resource providers** in de navigatie in de linkerkant.
4. Zoek naar ' micro soft. AAD ' in de tabel en klik op **opnieuw registreren**
5. Om ervoor te zorgen dat de waarschuwing wordt opgelost, bekijkt u de status pagina voor uw beheerde domein in twee uur.


## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>AADDS105 waarschuwing: De toepassing voor wachtwoord synchronisatie is verouderd

**Waarschuwings bericht:** De service-principal met de toepassings-ID ' d87dcbc6-a371-462e-88e3-28ad15ec4e64 ' is verwijderd en opnieuw gemaakt. De recreatie verloopt achter inconsistente machtigingen voor Azure AD Domain Services resources die nodig zijn om uw beheerde domein te onderhouden. De synchronisatie van wacht woorden op uw beheerde domein kan worden beïnvloed.


**Opgelost** U hebt Azure AD Power shell nodig om deze stappen uit te voeren. Zie [dit artikel](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.)voor meer informatie over het installeren van Azure AD Power shell.

Om dit probleem op te lossen, typt u de volgende opdrachten in een Power shell-venster:
1. Installeer de Azure AD Power shell-module en importeer deze.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```
2. De oude toepassing en het object verwijderen met de volgende Power shell-opdrachten

    ```powershell
    $app = Get-AzureADApplication -Filter "IdentifierUris eq 'https://sync.aaddc.activedirectory.windowsazure.com'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```
3. Nadat u beide hebt verwijderd, wordt het systeem vanzelf hersteld en worden de toepassingen opnieuw gemaakt die nodig zijn voor wachtwoord synchronisatie. Om ervoor te zorgen dat de waarschuwing is hersteld, wacht u twee uur en controleert u de status van uw domein.


## <a name="contact-us"></a>Contact opnemen
Neem contact op met het product team van Azure Active Directory Domain Services om [feedback te delen of voor ondersteuning](contact-us.md).

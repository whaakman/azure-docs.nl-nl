---
title: 'Azure Active Directory Domain Services: Configuratie van Service-Principal voor het oplossen van problemen | Microsoft Docs'
description: De configuratie van het oplossen van problemen met Service-Principal voor Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ergreenl
ms.openlocfilehash: 5bc1212cc6e894cd82a60abb42f92893c0bb2d43
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579541"
---
# <a name="troubleshoot-invalid-service-principal-configuration-for-your-managed-domain"></a>Ongeldige configuratie van de Service-Principal voor uw beheerde domein oplossen

Dit artikel helpt u problemen op te lossen fouten van de service-principal-gerelateerde configuratie die in het volgende bericht weergegeven resulteren:

## <a name="alert-aadds102-service-principal-not-found"></a>Waarschuwing AADDS102: Service-Principal is niet gevonden

**Waarschuwing:** *A Service-Principal is vereist voor Azure AD Domain Services te laten functioneren is verwijderd uit uw Azure AD-directory. Deze configuratie heeft gevolgen voor de mogelijkheid van Microsoft om te controleren, beheren, toepassen van patches en synchroniseren van uw beheerde domein.*

[Service-principals](../active-directory/develop/app-objects-and-service-principals.md) zijn toepassingen die Microsoft gebruikt om te beheren, bijwerken en onderhouden van uw beheerde domein. Als ze worden verwijderd, wordt de mogelijkheid van Microsoft om uw domein.


## <a name="check-for-missing-service-principals"></a>Selectievakje om ontbrekende service-principals
Gebruik de volgende stappen uit om te bepalen welke service principals moeten opnieuw worden gemaakt:

1. Navigeer naar de [bedrijfstoepassingen - alle toepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) pagina in de Azure portal.
2. In de **weergeven** vervolgkeuzelijst **alle toepassingen** en klikt u op **toepassen**.
3. Gebruik de volgende tabel zoeken voor elk toepassings-ID door de ID te plakken in het zoekvak en drukt u op te geven. Als de lijst met zoekresultaten leeg zijn, moet u opnieuw de service-principal maken met de volgende stappen in de kolom 'Oplossing'.

| Toepassings-id | Oplossing |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Maak opnieuw een ontbrekende service-principal met PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Opnieuw registreren bij de naamruimte Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Opnieuw registreren bij de naamruimte Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Service-principals die zelf corrigeren](#service-principals-that-self-correct) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>Maak opnieuw een ontbrekende Service-Principal met PowerShell
Volg deze stappen als een service-principal met de ID ```2565bd9d-da50-47d4-8b85-4c97f669dc36``` ontbreekt in uw Azure AD-directory.

**Oplossing:** u moet Azure AD PowerShell om deze stappen te voltooien. Zie voor meer informatie over het installeren van Azure AD PowerShell [in dit artikel](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Om dit probleem op te lossen, typt u de volgende opdrachten in een PowerShell-venster:
1. De Azure AD PowerShell-module installeren en importeren.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. Controleer of de service-principal voor Azure AD Domain Services vereist in uw directory ontbreekt door het uitvoeren van de volgende PowerShell-opdracht:

    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```

3. De service-principal maken met de volgende PowerShell-opdracht te typen:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

4. Nadat u de ontbrekende service principal gemaakt hebt, wacht twee uur en controleer de status van uw beheerde domein.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Opnieuw registreren bij de Microsoft AAD-naamruimte met behulp van de Azure portal
Volg deze stappen als een service-principal met de ID ```443155a6-77f3-45e3-882b-22b3a8d431fb``` of ```abba844e-bc0e-44b0-947a-dc74e5d09022``` ontbreekt in uw Azure AD-directory.

**Oplossing:** gebruiken de volgende stappen uit om te herstellen van Domain Services op de map:

1. Navigeer naar de [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) pagina in de Azure portal.
2. Kies het abonnement uit de tabel die is gekoppeld aan uw beheerde domein
3. Met de linker navigatielijst kiezen **Resourceproviders**
4. Zoek naar 'Microsoft.AAD' in de tabel en klikt u op **opnieuw te registreren**
5. Om te controleren of de waarschuwing is opgelost, de health-pagina voor uw beheerde domein te bekijken in twee uur.


## <a name="service-principals-that-self-correct"></a>Service-Principals die zelf corrigeren
Volg deze stappen als een service-principal met de ID ```d87dcbc6-a371-462e-88e3-28ad15ec4e64``` ontbreekt in uw Azure AD-directory.

**Oplossing:** Azure AD Domain Services kan worden gedetecteerd wanneer deze specifieke service-principal ontbreekt, is onjuist geconfigureerd of is verwijderd. De service wordt automatisch opnieuw gemaakt deze service-principal. Echter, moet u de toepassing verwijderen en object dat de verwijderde toepassing werkte als wanneer de certificering wordt getotaliseerd via, de toepassing en het object niet langer mogelijk om te worden gewijzigd door de nieuwe service-principal. Dit zal leiden tot een nieuwe fout in uw domein. Volg de stappen in de [sectie voor AADDS105](#alert-aadds105-password-synchronization-application-is-out-of-date) om te voorkomen dat dit probleem. Controleer nadat de status van uw beheerde domein na twee uur om ervoor te zorgen dat de nieuwe service-principal is opnieuw gemaakt.


## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>Waarschuwing AADDS105: Wachtwoord synchronisatie van toepassing is verouderd

**Waarschuwing:** de service-principal met de toepassings-ID "d87dcbc6-a371-462e-88e3-28ad15ec4e64" is verwijderd en vervolgens opnieuw. De recreatie blijven achter inconsistent machtigingen op Azure AD Domain Services-resources die nodig zijn om uw beheerde domein te behouden. Synchronisatie van wachtwoorden in uw beheerde domein kan worden beïnvloed.


**Oplossing:** u moet Azure AD PowerShell om deze stappen te voltooien. Zie voor meer informatie over het installeren van Azure AD PowerShell [in dit artikel](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Om dit probleem op te lossen, typt u de volgende opdrachten in een PowerShell-venster:
1. De Azure AD PowerShell-module installeren en importeren.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```
2. Verwijder de oude toepassing en het object met behulp van de volgende PowerShell-opdrachten

    ```powershell
    $app = Get-AzureADApplication -Filter "IdentifierUris eq 'https://sync.aaddc.activedirectory.windowsazure.com'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```
3. Nadat u beide hebt verwijderd, wordt het systeem zelf herstellen en opnieuw maken van de toepassingen die nodig zijn voor Wachtwoordsynchronisatie. Om te controleren of de waarschuwing is doorgevoerd, wacht twee uur en controleer de status van uw domein.


## <a name="contact-us"></a>Contact opnemen
Neem contact op met het productteam van Azure Active Directory Domain Services naar [feedback geven of voor ondersteuning van](active-directory-ds-contact-us.md).

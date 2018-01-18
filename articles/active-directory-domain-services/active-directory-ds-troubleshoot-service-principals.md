---
title: 'Azure Active Directory Domain Services: Configuratie van de Service-Principal probleemoplossing | Microsoft Docs'
description: Het Service-Principal-configuratie voor Azure AD Domain Services oplossen
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: ergreenl
ms.openlocfilehash: e6144a4018f7fbe7dbf7b4693e3f41884e4a80a2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-service-principal-configuration"></a>Azure AD Domain Services - configuratie van de Service-Principal voor het oplossen van problemen

Microsoft gebruikt om de service, beheren en bijwerken van uw domein, verschillende [Service-Principals](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects) om te communiceren met uw directory. Een is onjuist geconfigureerd of is verwijderd, kan leiden tot een onderbreking in uw service.

## <a name="aadds102-service-principal-not-found"></a>AADDS102: Service-Principal is niet gevonden

**Waarschuwing:**

*Een Service-Principal die is vereist voor Azure AD Domain Services naar behoren is verwijderd van uw Azure AD-directory. Deze configuratie heeft impact op de mogelijkheid van Microsoft als u wilt bewaken, beheren, patch en synchroniseren van uw beheerde domein.*

Service-principals zijn toepassingen die Microsoft gebruikt om te beheren, bijwerken en onderhouden van uw beheerde domein. Als ze worden verwijderd, wordt de mogelijkheid van Microsoft voor het onderhouden van uw domein verbroken. Gebruik de volgende stappen uit om te bepalen welke service principals moeten opnieuw worden gemaakt.

1. Navigeer naar de [bedrijfstoepassingen - alle toepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) pagina in de Azure-portal.
2. Selecteer de vervolgkeuzelijst weergegeven met **alle toepassingen** en klik op **toepassen**.
3. Gebruik de volgende tabel zoeken voor elk toepassings-ID door de ID in het zoekvak te plakken en drukt u op te geven. Als de zoekresultaten leeg zijn, moet u de service-principal opnieuw maken door de stappen in de kolom 'Oplossing'.

| Toepassings-id | Oplossing |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Opnieuw maken van een ontbrekende Service-Principal met PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Opnieuw registreren bij de naamruimte Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Opnieuw registreren bij de naamruimte Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Service-Principals Self corrigeren](#service-principals-that-self-correct) |

### <a name="recreate-a-missing-service-principal-with-powershell"></a>Maak opnieuw een ontbrekende Service-Principal met PowerShell

*Volg deze stappen voor het ontbreken van id's: 2565bd9d-da50-47d4-8b85-4c97f669dc36*

**Herstel:**

U moet Azure AD PowerShell om deze stappen te voltooien. Zie voor meer informatie over het installeren van Azure AD PowerShell [in dit artikel](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Om dit probleem op te lossen, typt u de volgende opdrachten in een PowerShell-venster:
1. Installatie-Module AzureAD
2. AzureAD Import-Module
3. Controleer of de service-principal die is vereist voor Azure AD Domain Services in uw directory ontbreekt door het uitvoeren van de volgende PowerShell-opdracht:
      ```PowerShell
      Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
      ```
4. De service-principal maken door de volgende PowerShell-opdracht te typen:
     ```PowerShell
     New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
     ```
5. Nadat u de ontbrekende Service-Principal gemaakt hebt, wacht twee uren en controleer de status van uw domein.


### <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Opnieuw registreren bij de Microsoft AAD-naamruimte met behulp van de Azure-portal

*Volg deze stappen voor het ontbreken van id's: 443155a6-77f3-45e3-882b-22b3a8d431fb en abba844e-bc0e-44b0-947a-dc74e5d09022*

**Herstel:**

Gebruik de volgende stappen uit om terug te zetten Domain Services op de map:

1. Navigeer naar de [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) pagina in de Azure-portal.
2. Kies het abonnement uit de tabel die is gekoppeld aan uw beheerde domein
3. Met de linkernavigatiebalk kiezen **Resourceproviders**
4. Zoek 'Microsoft.AAD' in de tabel en klik op **opnieuw te registreren**
5. Weergeven om te controleren om ervoor te zorgen dat de waarschuwing is opgelost, uw alert health-pagina in twee uur.


### <a name="service-principals-that-self-correct"></a>Service-Principals self corrigeren

*Volg deze stappen voor het ontbreken van id's: d87dcbc6-a371-462e-88e3-28ad15ec4e64*

**Herstel:**

Microsoft kan bepalen wanneer specifieke Service-Principals ontbreken, zijn onjuist geconfigureerd of is verwijderd. U kunt de service snel oplossen, maakt Microsoft de Service-Principals zelf opnieuw. Controleer de status van uw domein na twee uur om ervoor te zorgen dat de principal is opnieuw gemaakt.

## <a name="contact-us"></a>Contact opnemen
Neem contact op met de Azure Active Directory Domain Services-productteam voor [feedback delen of voor ondersteuning](active-directory-ds-contact-us.md).

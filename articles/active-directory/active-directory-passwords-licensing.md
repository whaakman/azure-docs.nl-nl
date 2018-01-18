---
title: Licentie selfservice voor wachtwoordherstel - Azure Active Directory
description: Azure AD selfservice voor wachtwoordherstel licentievereisten
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: ca6d7a6d2b7ffa49f3656510010cfb49a81e22d7
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Licentievereisten voor selfservicegebruikers Azure AD-wachtwoord opnieuw instellen

Azure Active Directory (Azure AD) voor wachtwoordherstel functie, zodat u *moet ten minste één licentie is toegewezen in uw organisatie hebben*. We afdwingen niet dat per gebruiker licentieverlening op de ervaring van wachtwoord opnieuw instellen. Om te blijven voldoen aan uw Microsoft-gebruiksrechtovereenkomst, moet u licenties toewijzen voor alle gebruikers die gebruikmaken van premium-functies.

* **Alleen in de cloud gebruikers**: Office 365 een betaald SKU of Azure AD Basic
* **Cloud** of **on-premises gebruikers**: Azure AD Premium-P1 of P2, Enterprise Mobility + Security (EMS) of Microsoft 365

## <a name="licenses-required-for-password-writeback"></a>Licenties die vereist zijn voor write-back van wachtwoord

Voor het gebruik van wachtwoord terugschrijven, moet u een van de volgende licenties die zijn toegewezen op uw tenant hebben:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 (Plan E3)
* Microsoft 365 (Plan E5)

> [!WARNING]
> Zelfstandige Office 365-abonnementen licentieverlening *bieden geen ondersteuning voor write-back van wachtwoord* en vereisen dat u een van de voorgaande plannen voor deze functie werkt.
>

Aanvullende licentie-informatie, inclusief kosten, vindt u op de volgende pagina's:

* [Azure Active Directory website prijzen](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory-functies en mogelijkheden](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Groep of gebruiker gebaseerde licentieverlening inschakelen

Azure AD nu ondersteunt op basis van een groep licentieverlening. Beheerders kunnen bulksgewijs licenties toewijzen aan een groep gebruikers in plaats van één voor één toewijzen. Zie voor meer informatie [toewijzen, controleren en oplossen van problemen met licenties](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses).

Sommige Microsoft-services zijn niet beschikbaar op alle locaties. Voordat u een licentie kan worden toegewezen aan een gebruiker, de beheerder moet opgeven de **gebruikslocatie** eigenschap van de gebruiker. Toewijzing van licenties kan worden uitgevoerd onder de **gebruiker** > **profiel** > **instellingen** sectie in de Azure-portal. *Wanneer u de licentietoewijzing van de groep, nemen alle gebruikers zonder een gebruikslocatie opgegeven de locatie van de map.*

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](active-directory-passwords-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](active-directory-passwords-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

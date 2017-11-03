---
title: 'Licentieverlening: Azure AD SSPR | Microsoft Docs'
description: Azure AD selfservice voor wachtwoordherstel licentievereisten
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: db849cd1e9da634064f79fbc041098542580ad02
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Licentievereisten voor selfservicegebruikers Azure AD-wachtwoord opnieuw instellen

Azure AD-wachtwoord opnieuw instellen naar de functie, zodat u **moet ten minste één licentie is toegewezen in uw organisatie hebben**. We afdwingen niet dat per gebruiker licentieverlening op de ervaring van wachtwoord opnieuw instellen. Om te blijven voldoen aan uw Microsoft-gebruiksrechtovereenkomst, moet u licenties toewijzen voor alle gebruikers die gebruikmaken van premium-functies.

* **Alleen in de cloud gebruikers** -Office 365 (O365) een betaald SKU of Azure AD Basic
* **Cloud** en/of **on-premises gebruikers** -Azure AD Premium-P1 of P2, Enterprise Mobility + Security (EMS) of Secure productief Enterprise (Gebeurtenisspe)

## <a name="licenses-required-for-password-writeback"></a>Licenties die vereist zijn voor write-back van wachtwoord

Voor het gebruik van wachtwoord terugschrijven, moet u een van de volgende licenties toegewezen in uw tenant hebben.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 E3
* Microsoft 365 E5

> [!WARNING]
> Zelfstandige Office 365-abonnementen licentieverlening **bieden geen ondersteuning voor write-back van wachtwoord** en vereisen een van de voorgaande plannen voor deze functie werkt.

Aanvullende licenties informatie, inclusief kosten vindt u op de volgende pagina 's

* [Azure Active Directory-prijzen site](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory-functies en mogelijkheden](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Groep of gebruiker gebaseerde licentieverlening inschakelen

Azure AD nu ondersteunt op basis van een groep licentieverlening zodat beheerders bulksgewijs licenties toewijzen aan een groep gebruikers in plaats van één voor één toewijzen. [Toewijzen, controleren en oplossen van problemen met licenties](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Sommige Microsoft-services zijn niet beschikbaar op alle locaties. Voordat u een licentie kan worden toegewezen aan een gebruiker, moet de beheerder de eigenschap 'Gebruikslocatie' op de gebruiker opgeven. Toewijzing van licenties kan worden uitgevoerd onder User > profiel > sectie instellingen in de Azure portal. **Wanneer u de licentietoewijzing van de groep, neemt alle gebruikers zonder een gebruikslocatie opgegeven de locatie van de map.**

## <a name="next-steps"></a>Volgende stappen

* [Hoe ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Opnieuw instellen of wijzigen van uw wachtwoord](active-directory-passwords-update-your-own-password.md).
* [Registreren voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).
* [Welke gegevens wordt gebruikt door de SSPR en welke gegevens moet u voor uw gebruikers vullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties met SSPR?](active-directory-passwords-policy.md)
* [Wat is Write-back van wachtwoord en waarom ik van belang?](active-directory-passwords-writeback.md)
* [Hoe meld ik op activiteit in SSPR?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in de SSPR en wat ze aan de hand?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets is verbroken. Hoe kan ik SSPR oplossen?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag waarvoor is geen ergens anders](active-directory-passwords-faq.md)

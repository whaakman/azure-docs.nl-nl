---
title: Licentie Self-service voor wachtwoord opnieuw instellen - Azure Active Directory
description: Azure AD-selfservice wachtwoord opnieuw instellen van licentievereisten duidelijk zijn
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 64c12177e5cf6c82018731b493c0da22e1895b7f
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855521"
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Vereisten voor licentieverlening voor Azure AD Self-service voor wachtwoord opnieuw instellen

Azure Active Directory (Azure AD) voor wachtwoord opnieuw instellen van functie, zodat u *moet er ten minste één licentie is toegewezen in uw organisatie* voor die gebruiker. U hebt een echte licentie nodig als een gebruiker direct of indirect profiteert van een functie die door die licentie mogelijk wordt gemaakt.

* **Cloudgebruikers**: Office 365 betaalde SKU of Azure AD Basic
* **Cloud** of **on-premises gebruikers**: Azure AD Premium P1 of P2, Enterprise Mobility + Security (EMS) of Microsoft 365

## <a name="licensing-requirements-for-password-writeback"></a>Licentievereisten voor wachtwoord terugschrijven

**Selfservice wachtwoord opnieuw instellen/wijzigen/ontgrendelen met on-premises Write-back is een premium-functie van Azure AD**. Zie voor meer informatie over de licentieverlening voor de [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

Voor het gebruik van het terugschrijven van wachtwoorden, moet u een van de volgende licenties zijn toegewezen op uw tenant hebben:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 of A3
* Enterprise Mobility + Security E5 of A5
* Microsoft 365 E3 of A3
* Microsoft 365 E5 of A5
* Microsoft 365 F1

> [!WARNING]
> Zelfstandige Office 365-abonnementen licentieverlening *bieden geen ondersteuning voor het terugschrijven van wachtwoorden* en vereisen dat u met een van de voorgaande plannen voor deze functionaliteit te werken.
>

Extra licentie-informatie, inclusief kosten, vindt u op de volgende pagina's:

* [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory-functies en mogelijkheden](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Groep of gebruiker op basis van licentieverlening inschakelen

Nu Azure AD biedt ondersteuning voor Groepslicenties. Beheerders kunnen bulksgewijs-licenties toewijzen aan een groep gebruikers, in plaats van één voor één toewijzen. Zie voor meer informatie, [toewijzen, controleren en oplossen van problemen met licenties](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Sommige Microsoft-services zijn niet beschikbaar op alle locaties. Voordat een licentie kan worden toegewezen aan een gebruiker, de beheerder moet opgeven de **gebruikslocatie** eigenschap van de gebruiker. Toewijzing van licenties kan worden uitgevoerd onder de **gebruiker** > **profiel** > **instellingen** sectie in Azure portal. *Wanneer u de licentietoewijzing groep, nemen alle gebruikers geen gebruikslocatie opgegeven de locatie van de map.*

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](howto-sspr-deployment.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](../active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](../active-directory-passwords-reset-register.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](howto-sspr-authenticationdata.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](concept-sspr-howitworks.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](concept-sspr-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](howto-sspr-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](howto-sspr-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](concept-sspr-howitworks.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

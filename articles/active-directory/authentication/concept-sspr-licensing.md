---
title: Licentie voor Azure Active Directory Self-service voor wachtwoord
description: Azure AD-selfservice wachtwoord opnieuw instellen van licentievereisten duidelijk zijn
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 83054c505689768c14d168841764a4557c3e1f8b
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158995"
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Vereisten voor licentieverlening voor Azure AD Self-service voor wachtwoord opnieuw instellen

Azure Active Directory (Azure AD) is beschikbaar in vier edities: gratis, Basic, Premium P1 en Premium P2. Er zijn diverse verschillende functies die van self-service voor wachtwoord opnieuw instellen, met inbegrip van wijzigen, herstellen, ontgrendelen en terugschrijven van wachtwoorden, die beschikbaar zijn in de verschillende edities van Azure AD. In dit artikel wordt geprobeerd om te worden de verschillen uitgelegd. Meer informatie over de functies die zijn opgenomen in elke Azure AD-editie te vinden in de [Azure Active Directory pagina met prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="compare-editions-and-features"></a>Vergelijk functies en -edities

Azure AD-selfservice wachtwoord opnieuw instellen is een licentie per gebruiker, om te zorgen voor naleving organisaties moeten de juiste licentie toewijzen aan gebruikers.

* Wachtwoorden wijzigen via self-service voor cloudgebruikers
   * Ik ben een **gebruikersdirectory die alleen** en weet mijn wachtwoord.
      * Ik wil graag **wijzigen** mijn wachtwoord voor iets nieuws.
   * Deze functionaliteit is opgenomen in alle edities van Azure AD.

* Wachtwoorden opnieuw instellen via self-service voor cloudgebruikers
   * Ik ben een **gebruikersdirectory die alleen** en Mijn wachtwoord vergeten.
      * Ik wil graag **opnieuw** iets dat ik weet mijn wachtwoord.
   * Deze functionaliteit is opgenomen in Azure AD Basic, Premium P1 of P2 Premium-edities.

* Selfservice wachtwoord opnieuw instellen/wijzigen/ontgrendelen **met on-premises Write-back**
   * Ik ben een **hybride gebruiker** mijn on-premises Active Directory-gebruikersaccount is gesynchroniseerd met mijn Azure AD-account met Azure AD Connect. Ik wil graag mijn wachtwoord wijzigen, hebt mijn wachtwoord vergeten of is vergrendeld.
      * Ik wil mijn wachtwoord wijzigen of opnieuw wordt ingesteld op iets dat ik weten of Mijn account ontgrendelen **en** beschikt over dat wijziging worden gesynchroniseerd met on-premises Active Directory.
   * Deze functionaliteit is opgenomen in Azure AD Premium P1 of P2 Premium-edities.

> [!WARNING]
> Zelfstandige Office 365-abonnementen licentieverlening **bieden geen ondersteuning voor het terugschrijven van wachtwoorden** en vereist Azure AD Premium P1 of P2 Premium-edities voor deze functionaliteit wilt werken.
>

Extra licentie-informatie, inclusief kosten, vindt u op de volgende pagina's:

* [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory-functies en mogelijkheden](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Groep of gebruiker op basis van licentieverlening inschakelen

Nu Azure AD biedt ondersteuning voor Groepslicenties. Beheerders kunnen bulksgewijs-licenties toewijzen aan een groep gebruikers, in plaats van één voor één toewijzen. Zie voor meer informatie, [toewijzen, controleren en oplossen van problemen met licenties](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Sommige services van Microsoft zijn niet op alle locaties beschikbaar. Voordat een licentie kan worden toegewezen aan een gebruiker, de beheerder moet opgeven de **gebruikslocatie** eigenschap van de gebruiker. Toewijzing van licenties kan worden uitgevoerd onder de **gebruiker** > **profiel** > **instellingen** sectie in Azure portal. *Wanneer u de licentietoewijzing groep, nemen alle gebruikers geen gebruikslocatie opgegeven de locatie van de map.*

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](howto-sspr-deployment.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](../user-help/active-directory-passwords-reset-register.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](howto-sspr-authenticationdata.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](concept-sspr-howitworks.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](concept-sspr-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](howto-sspr-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](howto-sspr-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](concept-sspr-howitworks.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

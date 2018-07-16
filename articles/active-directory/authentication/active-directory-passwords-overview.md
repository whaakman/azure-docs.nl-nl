---
title: Overzicht van door Azure AD-selfservice wachtwoord opnieuw instellen | Microsoft Docs
description: Wat betekenen kan Azure AD Self-service voor wachtwoord opnieuw instellen voor uw organisatie?
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: e28918fe9e26221738fe234ad41923c58a6ac260
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049018"
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Azure AD Self-service voor wachtwoord opnieuw instellen voor IT-professionals

Met Azure Active Directory (Azure AD) selfservice voor wachtwoordherstel (SSPR), kunnen gebruikers hun wachtwoorden als opnieuw instellen op hun eigen wanneer en waar dat nodig. Beheerders kunnen bepalen hoe het wachtwoord van een gebruiker opnieuw ingesteld op hetzelfde moment. Gebruikers niet meer nodig hebt om aan te roepen een helpdesk maar alleen om hun wachtwoord opnieuw instellen. Azure AD SSPR bevat:

* **Self-service voor wachtwoord wijzigen**: de gebruiker het wachtwoord kent, maar wil dit wijzigen in iets nieuws.
* **Self-service voor wachtwoord opnieuw instellen**: de gebruiker kan niet aan te melden bij en wil hun wachtwoord opnieuw instellen met behulp van een of meer van de volgende gevalideerde verificatiemethoden:
   * Een SMS-bericht verzenden naar een gevalideerde mobiele telefoon.
   * Bellen naar een gevalideerde mobile- of office-telefoon.
   * Een e-mailbericht verzenden naar een gevalideerde secundaire e-mailaccount.
   * Of haar beveiligingsvragen beantwoorden.
* **Ontgrendelen van het account selfservice**: de gebruiker kan niet aanmelden met hun wachtwoord en is vergrendeld. De gebruiker wil dat voor het ontgrendelen van hun account zonder tussenkomst van de beheerder met behulp van hun verificatiemethoden.

> [!VIDEO https://www.youtube.com/embed/hc97Yx5PJiM]

## <a name="why-choose-azure-ad-sspr"></a>Waarom kiezen voor Azure AD SSPR

Azure AD SSPR helpt u bij:

* **Kosten verlagen** als help helpdesk assisted wachtwoorden doorgaans-account voor 20% van een IT-organisatie ondersteuningsaanvragen worden ingediend. 
* **Verbetering van de ervaringen van eindgebruikers** en **verkleinen help helpdesk** door allemaal factoren die eindgebruikers kunnen hun eigen wachtwoord problemen op te lossen. Er is niet nodig een helpdesk te bellen of een ondersteuningsaanvraag openen.
* **Station mobility** als gebruikers hun wachtwoord overal kunnen herstellen.
* **Behoud controle** van het beveiligingsbeleid. Beheerders kunnen doorgaan met het afdwingen van het beleid dat ze vandaag hebben.

Als u klaar bent, u kunt aan de slag met Azure AD SSPR met behulp van onze [richtlijnen voor snel starten-](quickstart-sspr.md). U kunt uw gebruikers snel aan de mogelijkheid om hun eigen wachtwoorden opnieuw in te geven.

## <a name="azure-ad-sspr-availability"></a>Azure AD SSPR-beschikbaarheid

Azure AD SSPR is beschikbaar in drie opslaglagen, afhankelijk van uw abonnement:

* **Azure AD Free**: alleen-Cloud-beheerders kunnen hun eigen wachtwoorden opnieuw instellen.
* **Azure AD Basic** of een **betaald Office 365-abonnement**: Cloud-only gebruikers kunnen hun eigen wachtwoorden opnieuw instellen.
* **Azure AD Premium**: een gebruiker of beheerder, met inbegrip van alleen in de cloud, federatieve, Pass through-verificatie of gebruikers van wachtwoord-hash-gesynchroniseerd, kan hun eigen wachtwoorden opnieuw. On-premises wachtwoorden vereist voor het terugschrijven van wachtwoorden worden ingeschakeld.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD-prijzen, SLA, updates en -roadmap

Meer informatie over licentieverlening, prijzen en toekomstige abonnementen vindt u op de volgende sites:

* [Azure AD prijsinformatie](https://azure.microsoft.com/pricing/details/active-directory/)
* [Prijzen van Office 365](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/)
* [Service Level Agreement voor Microsoft Online Services](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [Azure-updates](https://azure.microsoft.com/updates/)
* [Azure-roadmap](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Volgende stappen

* Bent u klaar om aan de slag met Self-service voor Wachtwoordherstel? [Instellen van Azure AD Self-service voor wachtwoord opnieuw instellen van](quickstart-sspr.md).
* Plannen van een geslaagde implementatie van SSPR aan uw gebruikers met behulp van de richtlijnen die zijn gevonden in onze [implementatie handleiding](howto-sspr-deployment.md).
* [Stel uw wachtwoord opnieuw in of wijzig het](../user-help/active-directory-passwords-update-your-own-password.md).
* [Registreer u voor selfservice voor wachtwoordherstel](../user-help/active-directory-passwords-reset-register.md).

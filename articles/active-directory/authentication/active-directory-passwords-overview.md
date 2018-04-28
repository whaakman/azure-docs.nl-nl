---
title: Azure AD selfservice voor wachtwoordherstel overzicht | Microsoft Docs
description: Wat kan kan Azure AD zelf uw wachtwoord opnieuw instellen voor uw organisatie?
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 7d1103e0ab21d2eac336a0e460156d47012b2a86
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Azure AD selfservice voor wachtwoordherstel voor IT-professionals

Met Azure Active Directory (Azure AD) selfservice voor wachtwoordherstel (SSPR), kunnen gebruikers opnieuw instellen van hun wachtwoorden op hun eigen wanneer en waar ze willen. Beheerders kunnen bepalen hoe een gebruiker opgehaald wachtwoordherstel op hetzelfde moment. Gebruikers moeten niet langer een helpdesk maar alleen om hun wachtwoord opnieuw instellen. Azure AD SSPR omvat:

* **Selfservice voor wachtwoordherstel wijziging**: de gebruiker het wachtwoord kent maar wil veranderen in iets dat nieuwe.
* **Selfservice voor wachtwoordherstel**: de gebruiker kan niet aanmelden en hun wachtwoord opnieuw instellen met behulp van een of meer van de volgende gevalideerde verificatiemethoden wil:
   * Een SMS-bericht verzenden naar een gevalideerde mobiele telefoon.
   * Een telefoongesprek naar een gevalideerde mobile- of -telefoon.
   * Een e-mailbericht naar een gevalideerde secundaire e-mailaccount verzenden.
   * Of haar beveiligingsvragen beantwoorden.
* **Ontgrendelen van het account selfservice**: de gebruiker kan niet aanmelden met hun wachtwoord en is vergrendeld. De gebruiker wil hun account zonder tussenkomst van de beheerder ontgrendelen met behulp van hun verificatiemethoden.

## <a name="why-choose-azure-ad-sspr"></a>Waarom Azure AD SSPR kiezen

Azure AD SSPR helpt u bij:

* **Kosten te verlagen** als help helpdesk assisted wachtwoorden doorgaans-account voor 20% van een IT-organisatie ondersteuningsoproepen. 
* **Ervaringen van eindgebruikers verbeteren** en **verminderen help helpdesk volume** doordat eindgebruikers de macht hun eigen wachtwoord problemen op te lossen. Er is niet nodig een helpdesk of een ondersteuningsaanvraag openen.
* **Station mobility** als gebruikers hun wachtwoorden op elke willekeurige locatie kunnen herstellen.
* **Controle** van het beveiligingsbeleid. Beheerders kunnen blijven de beleidsregels die ze vandaag hebben afdwingen.

Als u klaar bent, u kunt aan de slag met Azure AD SSPR met behulp van onze [snel starten richtlijnen](quickstart-sspr.md). U kunt snel uw gebruikers de mogelijkheid om hun eigen wachtwoorden opnieuw in te geven.

## <a name="azure-ad-sspr-availability"></a>Azure AD SSPR-beschikbaarheid

Azure AD SSPR is beschikbaar in drie lagen, afhankelijk van uw abonnement:

* **Azure AD-vrije**: Cloud alleen-lezen beheerders kunnen hun eigen wachtwoorden opnieuw instellen.
* **Azure AD Basic** of een **betaald Office 365-abonnement**: alleen in de Cloud-gebruikers kunnen hun eigen wachtwoorden opnieuw instellen.
* **Azure AD Premium**: een gebruiker of beheerder, met inbegrip van alleen in de cloud, federatieve, Pass through-verificatie of wachtwoord-hash gesynchroniseerd gebruikers hun eigen wachtwoord kunt herstellen. On-premises wachtwoorden vereisen wachtwoord terugschrijven worden ingeschakeld.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD-prijzen, SLA, updates en roadmap

Meer informatie over licenties, prijzen en toekomstplannen vindt u op de volgende sites:

* [Azure AD prijsgegevens](https://azure.microsoft.com/pricing/details/active-directory/)
* [Prijzen van Office 365](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/)
* [Service Level Agreement voor Microsoft Online Services](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [Azure-updates](https://azure.microsoft.com/updates/)
* [Azure-roadmap](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Volgende stappen

* Bent u klaar om te beginnen met SSPR? [Instellen van Azure AD zelf uw wachtwoord opnieuw instellen](quickstart-sspr.md).
* Plannen van een geslaagde implementatie van SSPR aan uw gebruikers met behulp van de richtlijnen gevonden in onze [implementatie-handleiding](howto-sspr-deployment.md).
* [Stel uw wachtwoord opnieuw in of wijzig het](../active-directory-passwords-update-your-own-password.md).
* [Registreer u voor selfservice voor wachtwoordherstel](../active-directory-passwords-reset-register.md).

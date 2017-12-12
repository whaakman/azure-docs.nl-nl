---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding - werking | Microsoft Docs'
description: Dit artikel wordt beschreven hoe de functie Azure Active Directory naadloze eenmalige aanmelding werkt.
services: active-directory
keywords: Wat is Azure AD Connect, installeer Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 0a28cd9016588d266670aa5a7fcbdd854d7ebce0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory naadloze eenmalige aanmelding: technische diepgaand

In dit artikel biedt u technische gegevens in de werking van de functie Azure Active Directory naadloze eenmalige aanmelding (SSO naadloze).

## <a name="how-does-seamless-sso-work"></a>Hoe werkt naadloze eenmalige aanmelding

Deze sectie heeft twee delen:
1. De installatie van de functie naadloze eenmalige aanmelding.
2. Hoe werkt een enkele gebruiker aanmelden transactie met naadloze eenmalige aanmelding.

### <a name="how-does-set-up-work"></a>Hoe werken instellen?

Naadloze eenmalige aanmelding is ingeschakeld via Azure AD Connect zoals [hier](active-directory-aadconnect-sso-quick-start.md). Tijdens het inschakelen van de functie, plaats de volgende stappen:
- Een account met de naam `AZUREADSSOACC` (geeft Azure AD) is gemaakt in uw on-premises Active Directory (AD).
- Het computeraccount Kerberos ontsleutelingssleutel wordt veilig worden gedeeld met Azure AD.
- Bovendien worden twee Kerberos-SPN-namen (SPN's) gemaakt ter vertegenwoordiging van twee URL's die worden gebruikt tijdens het aanmelden van Azure AD.

>[!NOTE]
> Het computeraccount en de Kerberos-SPN's worden gemaakt in elk AD-forest u synchroniseren met Azure AD (met behulp van Azure AD Connect) en gebruikers voor wie u wilt dat naadloze eenmalige aanmelding. Verplaats de `AZUREADSSOACC` computeraccount aan een organisatie-eenheid (OE) waar de computeraccounts van andere worden opgeslagen om ervoor te zorgen dat het op dezelfde manier wordt beheerd en wordt niet verwijderd.

>[!IMPORTANT]
>Ten zeerste aangeraden dat u [Beweeg de muis over de ontsleutelingssleutel Kerberos](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account) van de `AZUREADSSOACC` computeraccount ten minste elke 30 dagen.

### <a name="how-does-sign-in-with-seamless-sso-work"></a>Hoe aanmelden met naadloze eenmalige aanmelding werk?

Zodra de installatie voltooid is, werkt naadloze eenmalige aanmelding op dezelfde manier als elke andere aanmelden die gebruikmaakt van geïntegreerde Windows-verificatie (IWA). De stroom is als volgt:

1. De gebruiker probeert te krijgen tot een toepassing (bijvoorbeeld de Outlook Web App - https://outlook.office365.com/owa/) van een domein, zakelijke apparaat binnen uw bedrijfsnetwerk.
2. Als de gebruiker niet is al aangemeld, wordt de gebruiker omgeleid naar de aanmeldingspagina van Azure AD.

  >[!NOTE]
  >Als de Azure AD-in de aanvraag bevat een `domain_hint` (identificeren uw tenant - bijvoorbeeld, contoso.onmicrosoft.com) of `login_hint` (identificatie van de gebruiker - bijvoorbeeld user@contoso.onmicrosoft.com of user@contoso.com) parameter en klik vervolgens stap 2 wordt overgeslagen.

3. De gebruiker typt de naam van de gebruiker naar de aanmeldingspagina van Azure AD.
4. Voor informatie over het gebruik van JavaScript in de achtergrond van de uitdagingen Azure AD van de browser, via een 401-niet-geautoriseerde respons, zodat een Kerberos-ticket.
5. Op zijn beurt vraagt de browser een ticket van Active Directory voor de `AZUREADSSOACC` computeraccount (die staat voor Azure AD).
6. Active Directory wordt gezocht naar de computeraccount en retourneert een Kerberos-ticket en de browser die is versleuteld met het computeraccount geheim.
7. De browser stuurt het Kerberos-ticket die is verkregen van Active Directory naar Azure AD (op een van de [Azure AD-URL's eerder zijn toegevoegd aan de browser Intranet-beveiligingszone-instellingen](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).
8. Azure AD ontsleutelt het Kerberos-ticket, waaronder de identiteit van de gebruiker aangemeld bij het bedrijfsapparaat met de eerder gedeelde sleutel.
9. Na evaluatie, is Azure AD retourneert een token terug naar de toepassing of de gebruiker extra bewijzen, zoals multi-factor Authentication uitvoeren wordt gevraagd.
10. Als de gebruiker aanmelden geslaagd is, wordt de gebruiker toegang tot de toepassing is.

Het volgende diagram illustreert de onderdelen en de vereiste stappen.

![Naadloze eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso2.png)

Naadloze eenmalige aanmelding is opportunistisch, dat als dit mislukt, de aanmeldingservaring terugvalt op het normale gedrag - eenledige, de gebruiker moet invoeren van hun wachtwoord aan te melden.

## <a name="next-steps"></a>Volgende stappen

- [**Snel starten** ](active-directory-aadconnect-sso-quick-start.md) - laten en Azure AD naadloze eenmalige aanmelding wordt uitgevoerd.
- [**Veelgestelde vragen** ](active-directory-aadconnect-sso-faq.md) -antwoorden op veelgestelde vragen.
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.

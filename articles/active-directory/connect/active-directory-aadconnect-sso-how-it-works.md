---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding - hoe het werkt | Microsoft Docs'
description: Dit artikel wordt beschreven hoe de functie Azure Active Directory naadloze eenmalige aanmelding werkt.
services: active-directory
keywords: Wat is Azure AD Connect, Active Directory installeren onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 6507158a63de508164fc74bcafe39785046a2c79
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213347"
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory naadloze eenmalige aanmelding: technische details

Dit artikel vindt u technische gegevens in de werking van de functie Azure Active Directory naadloze eenmalige aanmelding (naadloze eenmalige aanmelding).

## <a name="how-does-seamless-sso-work"></a>Hoe werkt de naadloze eenmalige aanmelding?

Dit gedeelte bevat drie onderdelen:
1. De installatie van de functie voor naadloze eenmalige aanmelding.
2. Hoe werkt een enkele gebruiker aanmelden transactie via een webbrowser met naadloze eenmalige aanmelding.
3. Hoe een enkele gebruiker aanmelden transactie op een systeemeigen client werkt met naadloze eenmalige aanmelding.

### <a name="how-does-set-up-work"></a>Hoe werken instellen?

Naadloze eenmalige aanmelding is ingeschakeld met behulp van Azure AD Connect, zoals [hier](active-directory-aadconnect-sso-quick-start.md). Tijdens het inschakelen van de functie, gebeuren de volgende stappen uit:
- Een account met de naam `AZUREADSSOACC` (die vertegenwoordigt Azure AD) is gemaakt in uw on-premises Active Directory (AD) in elk AD-forest.
- Het computeraccount Kerberos ontsleutelingssleutel wordt veilig worden gedeeld met Azure AD. Als er meerdere AD-forests, hebben elk een eigen ontsleutelingssleutel Kerberos.
- Bovendien zijn twee Kerberos-SPN-namen (SPN's) gemaakt voor twee URL's die worden gebruikt tijdens de aanmelding bij Azure AD.

>[!NOTE]
> Het computeraccount en de Kerberos-SPN's worden gemaakt in elk AD-forest u synchroniseren met Azure AD (met behulp van Azure AD Connect) en gebruikers voor wie u wilt dat naadloze eenmalige aanmelding. Verplaats de `AZUREADSSOACC` computeraccount aan een organisatie-eenheid (OE) waar de computeraccounts van andere worden opgeslagen om ervoor te zorgen dat deze op dezelfde manier wordt beheerd en is niet verwijderd.

>[!IMPORTANT]
>Is het raadzaam dat u [Beweeg de muis over de ontsleutelingssleutel Kerberos](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account) van de `AZUREADSSOACC` computeraccount ten minste elke 30 dagen.

Nadat de installatie voltooid is, wordt met naadloze eenmalige aanmelding als een andere aanmelding die gebruikmaakt van geïntegreerde Windows-verificatie (IWA) op dezelfde manier werkt.

### <a name="how-does-sign-in-on-a-web-browser-with-seamless-sso-work"></a>Hoe aanmelden via een webbrowser met naadloze eenmalige aanmelding werk?

De stroom aanmelden via een webbrowser is als volgt:

1. De gebruiker probeert te krijgen tot een webtoepassing (bijvoorbeeld de Outlook Web App - https://outlook.office365.com/owa/) van een domein bedrijfsapparaat binnen uw bedrijfsnetwerk.
2. Als de gebruiker is niet al aangemeld, wordt de gebruiker omgeleid naar de aanmeldingspagina van Azure AD.
3. De gebruiker typt in hun gebruikersnaam in de Azure AD-aanmeldingspagina.

  >[!NOTE]
  >Voor [bepaalde toepassingen](./active-directory-aadconnect-sso-faq.md#what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso), stappen 2 en 3 worden overgeslagen.

4. Met behulp van JavaScript op de achtergrond, uitdagingen Azure AD voor de browser, via een 401-niet-geautoriseerde respons, voor een Kerberos-ticket.
5. De browser, vraagt een ticket op zijn beurt uit Active Directory voor de `AZUREADSSOACC` computeraccount (die staat voor Azure AD).
6. Active Directory wordt gezocht naar het computeraccount en retourneert een Kerberos-ticket naar de browser die is versleuteld met het computeraccount geheim.
7. De browser verzendt het Kerberos-ticket dat wordt verkregen uit Active Directory naar Azure AD.
8. Azure AD ontsleutelt het Kerberos-ticket, waaronder de identiteit van de gebruiker is aangemeld bij de zakelijke apparaat met behulp van de eerder gedeelde sleutel.
9. Na evaluatie, is Azure AD retourneert een token terug naar de toepassing of de gebruiker om uit te voeren extra bewijzen, zoals meervoudige verificatie wordt gevraagd.
10. Als de gebruiker-aanmelding geslaagd is, wordt de gebruiker toegang krijgen tot de toepassing is.

Het volgende diagram illustreert de onderdelen en de stappen die betrokken zijn.

![Naadloze eenmalige aanmelding op - Web-app-stroom](./media/active-directory-aadconnect-sso/sso2.png)

Naadloze eenmalige aanmelding is opportunistisch, wat betekent dat als dit mislukt, de aanmeldingsprocedure terugvalt op het normale gedrag - Internet Explorer, de gebruiker moet hun wachtwoord aan te melden bij invoeren.

### <a name="how-does-sign-in-on-a-native-client-with-seamless-sso-work"></a>Hoe aanmelding op een systeemeigen client met naadloze eenmalige aanmelding werk?

De stroom aanmelden op een systeemeigen client is als volgt:

1. De gebruiker probeert te krijgen tot een systeemeigen toepassing (bijvoorbeeld de Outlook-client) van een domein bedrijfsapparaat binnen uw bedrijfsnetwerk.
2. Als de gebruiker is niet al aangemeld, wordt de gebruikersnaam van de gebruiker in de systeemeigen toepassing opgehaald uit van het apparaat Windows-sessie.
3. De app wordt de gebruikersnaam verzonden naar Azure AD en worden opgehaald van uw tenant WS-Trust MEX-eindpunt.
4. De app vervolgens een query voor de WS-Trust MEX-eindpunt om te zien of geïntegreerde verificatie-eindpunt is beschikbaar.
5. Als stap 4 is gelukt, wordt er een Kerberos-uitdaging weergegeven.
6. Als de app in staat om op te halen van het Kerberos-ticket is, verzendt deze naar Azure AD geïntegreerde verificatie-eindpunt.
7. Azure AD het Kerberos-ticket wordt ontsleuteld en wordt deze gevalideerd.
8. Azure AD de gebruiker zich aanmeldt en problemen met een SAML-token bij de app.
9. De app verzendt vervolgens het SAML-token naar Azure AD OAuth2-token-eindpunt.
10. Azure AD het SAML-token valideert en problemen bij de app een toegangstoken en een vernieuwingstoken voor de opgegeven resource en een id-token.
11. De gebruiker krijgt toegang tot de bron van de app.

Het volgende diagram illustreert de onderdelen en de stappen die betrokken zijn.

![Naadloze eenmalige aanmelden - stroom van de systeemeigen app](./media/active-directory-aadconnect-sso/sso14.png)

## <a name="next-steps"></a>Volgende stappen

- [**Snel aan de slag** ](active-directory-aadconnect-sso-quick-start.md) : aan de slag en Azure AD naadloze eenmalige aanmelding wordt uitgevoerd.
- [**Veelgestelde vragen over** ](active-directory-aadconnect-sso-faq.md) -antwoorden op veelgestelde vragen.
- [**Problemen oplossen** ](active-directory-aadconnect-troubleshoot-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen in te dienen.

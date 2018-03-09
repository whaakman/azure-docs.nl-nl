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
ms.date: 02/15/2018
ms.author: billmath
ms.openlocfilehash: 9d17a4038f2171b74c8ba1dbc21e8335e6893691
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory naadloze eenmalige aanmelding: technische diepgaand

In dit artikel biedt u technische gegevens in de werking van de functie Azure Active Directory naadloze eenmalige aanmelding (SSO naadloze).

## <a name="how-does-seamless-sso-work"></a>Hoe werkt naadloze eenmalige aanmelding

Deze sectie heeft drie onderdelen:
1. De installatie van de functie naadloze eenmalige aanmelding.
2. Hoe werkt een enkele gebruiker aanmelden transactie via een webbrowser met naadloze eenmalige aanmelding.
3. Hoe werkt een enkele gebruiker aanmelden transactie op een native client met naadloze eenmalige aanmelding.

### <a name="how-does-set-up-work"></a>Hoe werken instellen?

Naadloze eenmalige aanmelding is ingeschakeld via Azure AD Connect zoals [hier](active-directory-aadconnect-sso-quick-start.md). Tijdens het inschakelen van de functie, plaats de volgende stappen:
- Een account met de naam `AZUREADSSOACC` (geeft Azure AD) is gemaakt in uw on-premises Active Directory (AD).
- Het computeraccount Kerberos ontsleutelingssleutel wordt veilig worden gedeeld met Azure AD.
- Bovendien worden twee Kerberos-SPN-namen (SPN's) gemaakt ter vertegenwoordiging van twee URL's die worden gebruikt tijdens het aanmelden van Azure AD.

>[!NOTE]
> Het computeraccount en de Kerberos-SPN's worden gemaakt in elk AD-forest u synchroniseren met Azure AD (met behulp van Azure AD Connect) en gebruikers voor wie u wilt dat naadloze eenmalige aanmelding. Verplaats de `AZUREADSSOACC` computeraccount aan een organisatie-eenheid (OE) waar de computeraccounts van andere worden opgeslagen om ervoor te zorgen dat het op dezelfde manier wordt beheerd en wordt niet verwijderd.

>[!IMPORTANT]
>Ten zeerste aangeraden dat u [Beweeg de muis over de ontsleutelingssleutel Kerberos](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account) van de `AZUREADSSOACC` computeraccount ten minste elke 30 dagen.

Zodra de installatie voltooid is, werkt naadloze eenmalige aanmelding op dezelfde manier als elke andere aanmelden die gebruikmaakt van geïntegreerde Windows-verificatie (IWA).

### <a name="how-does-sign-in-on-a-web-browser-with-seamless-sso-work"></a>Hoe aanmelden via een webbrowser met naadloze eenmalige aanmelding werk?

De stroom aanmelden via een webbrowser is als volgt:

1. De gebruiker probeert te krijgen tot een webtoepassing (bijvoorbeeld de Outlook Web App - https://outlook.office365.com/owa/) van een domein, zakelijke apparaat binnen uw bedrijfsnetwerk.
2. Als de gebruiker niet is al aangemeld, wordt de gebruiker omgeleid naar de aanmeldingspagina van Azure AD.
3. De gebruiker typt de naam van de gebruiker naar de aanmeldingspagina van Azure AD.

  >[!NOTE]
  >Voor [bepaalde toepassingen](./active-directory-aadconnect-sso-faq.md#what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso), stappen 2 en 3 worden overgeslagen.

4. Voor informatie over het gebruik van JavaScript in de achtergrond van de uitdagingen Azure AD van de browser, via een 401-niet-geautoriseerde respons, zodat een Kerberos-ticket.
5. Op zijn beurt vraagt de browser een ticket van Active Directory voor de `AZUREADSSOACC` computeraccount (die staat voor Azure AD).
6. Active Directory wordt gezocht naar de computeraccount en retourneert een Kerberos-ticket en de browser die is versleuteld met het computeraccount geheim.
7. De browser stuurt het Kerberos-ticket die is verkregen van Active Directory naar Azure AD.
8. Azure AD ontsleutelt het Kerberos-ticket, waaronder de identiteit van de gebruiker aangemeld bij het bedrijfsapparaat met de eerder gedeelde sleutel.
9. Na evaluatie, is Azure AD retourneert een token terug naar de toepassing of de gebruiker extra bewijzen, zoals multi-factor Authentication uitvoeren wordt gevraagd.
10. Als de gebruiker aanmelden geslaagd is, wordt de gebruiker toegang tot de toepassing is.

Het volgende diagram illustreert de onderdelen en de vereiste stappen.

![Naadloze eenmalige aanmelding op - Web-app-stroom](./media/active-directory-aadconnect-sso/sso2.png)

Naadloze eenmalige aanmelding is opportunistisch, dat als dit mislukt, de aanmeldingservaring terugvalt op het normale gedrag - eenledige, de gebruiker moet invoeren van hun wachtwoord aan te melden.

### <a name="how-does-sign-in-on-a-native-client-with-seamless-sso-work"></a>Hoe aanmelden op een native client met naadloze eenmalige aanmelding werk?

De stroom aanmelden op een native client is als volgt:

1. De gebruiker probeert te krijgen tot een systeemeigen toepassing (bijvoorbeeld de Outlook-client) van een domein, zakelijke apparaat binnen uw bedrijfsnetwerk.
2. Als de gebruiker nog niet is aangemeld, haalt de systeemeigen toepassing de gebruikersnaam van de gebruiker van het apparaat Windows-sessie.
3. De app de gebruikersnaam verzonden naar Azure AD en haalt de WS-Trust MEX-eindpunt voor uw tenant.
4. De app vervolgens een query voor de WS-Trust MEX-eindpunt om te zien of geïntegreerde verificatie-eindpunt is beschikbaar.
5. Als stap 4 slaagt, wordt een Kerberos-uitdaging uitgegeven.
6. Als de app in staat om op te halen van het Kerberos-ticket is, stuurt deze naar Azure AD-geïntegreerde verificatie-eindpunt.
7. Azure AD het Kerberos-ticket wordt ontsleuteld en wordt deze gevalideerd.
8. Azure AD de gebruiker zich aanmeldt, problemen en een SAML-token naar de app.
9. De app verzendt vervolgens het SAML-token naar Azure AD OAuth2-token eindpunt.
10. Azure AD het SAML-token valideert en verstrekt aan de app een toegangstoken en een vernieuwingstoken voor de opgegeven resource en een token id.
11. De gebruiker krijgt toegang tot de bron van de app.

Het volgende diagram illustreert de onderdelen en de vereiste stappen.

![Naadloze één melden - stroom van de systeemeigen app](./media/active-directory-aadconnect-sso/sso14.png)

## <a name="next-steps"></a>Volgende stappen

- [**Snel starten** ](active-directory-aadconnect-sso-quick-start.md) - laten en Azure AD naadloze eenmalige aanmelding wordt uitgevoerd.
- [**Veelgestelde vragen** ](active-directory-aadconnect-sso-faq.md) -antwoorden op veelgestelde vragen.
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.

---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding - snel aan de slag | Microsoft Docs'
description: Dit artikel wordt beschreven hoe u aan de slag met Azure Active Directory naadloze eenmalige aanmelding.
services: active-directory
keywords: Wat is Azure AD Connect, installeer Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 8975a82c5573cc0c284e1fc76cd0ef2c19fbbd72
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory naadloze eenmalige aanmelding: snel starten

## <a name="how-to-deploy-seamless-sso"></a>Naadloze eenmalige aanmelding implementeren

Azure Active Directory naadloze eenmalige aanmelding (Azure AD naadloze SSO) wordt automatisch aangemeld gebruikers wanneer ze zijn op hun zakelijke bureaublad die is verbonden met uw bedrijfsnetwerk. Dit biedt uw gebruikers eenvoudig toegang krijgen tot uw cloud-gebaseerde toepassingen zonder extra on-premises onderdelen.

Als u wilt implementeren naadloze eenmalige aanmelding, moet u als volgt te werk:

## <a name="step-1-check-prerequisites"></a>Stap 1: Controle van vereisten

Zorg ervoor dat de volgende vereisten voldaan is:

1. Instellen van uw Azure AD Connect-server: als u [Pass through-verificatie](active-directory-aadconnect-pass-through-authentication.md) als uw contactmethode aanmelden niets aanvullende vereiste controle is vereist. Als u [synchronisatie van wachtwoordhash](active-directory-aadconnectsync-implement-password-synchronization.md) als uw contactmethode aanmelden en als er een firewall tussen Azure AD Connect en Azure AD, zorg ervoor dat:
- U gebruikt versies 1.1.644.0 of hoger van Azure AD Connect. 
- Als uw firewall of proxyserver kan DNS-whitelisting, geaccepteerde verbindingen met  **\*. msappproxy.net** URL's via poort 443. Als dit niet het geval is, zodat u toegang hebben tot [Azure DataCenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653), die wekelijks worden bijgewerkt. Deze vereiste geldt alleen wanneer u de functie, niet voor de werkelijke gebruikersaanmeldingen inschakelen.

    >[!NOTE]
    >Azure AD Connect-versies 1.1.557.0, 1.1.558.0, 1.1.561.0 en 1.1.614.0 hebben een probleem is gerelateerd aan de synchronisatie van wachtwoordhash. Als u _niet_ wilt synchronisatie van wachtwoordhash gebruiken in combinatie met Pass through-verificatie, lees de [opmerkingen bij de release van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) voor meer informatie.

2. U moet domeinbeheerder-referenties voor elk AD-forest dat u synchroniseren met Azure AD (met behulp van Azure AD Connect) en gebruikers voor wie u wilt naadloze eenmalige aanmelding inschakelen.

## <a name="step-2-enable-the-feature"></a>Stap 2: De functie inschakelen

Naadloze eenmalige aanmelding kan worden ingeschakeld met [Azure AD Connect](active-directory-aadconnect.md).

Als u een nieuwe installatie van Azure AD Connect uitvoert, kiest u de [aangepaste installatiepad](active-directory-aadconnect-get-started-custom.md). Controleer op de pagina 'Gebruiker aanmelden' de optie 'Eenmalige aanmelding inschakelen op'.

![Azure AD Connect - gebruiker aanmelden](./media/active-directory-aadconnect-sso/sso8.png)

Als u al een installatie van Azure AD Connect, kiest u 'Gebruiker aanmelden pagina wijzigen' op Azure AD Connect en klik op 'Volgende'. Controleer vervolgens de optie 'Eenmalige aanmelding inschakelen op'.

![Azure AD Connect - wijziging gebruiker aanmelden](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

De wizard vervolgen, totdat u op de pagina 'Eenmalige aanmelding inschakelen op'. Domeinbeheerder-referenties opgeven voor elk AD-forest dat u synchroniseren met Azure AD (met behulp van Azure AD Connect) en gebruikers voor wie u wilt naadloze eenmalige aanmelding inschakelen. 

Naadloze eenmalige aanmelding is na voltooiing van de wizard wordt ingeschakeld op uw tenant.

>[!NOTE]
> De domeinbeheerder-referenties niet zijn opgeslagen in Azure AD Connect of in Azure AD, maar worden alleen gebruikt de functie in te schakelen.

Volg deze instructies om te controleren of naadloze eenmalige aanmelding correct ingeschakeld:

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de referenties van de globale beheerder voor uw tenant.
2. Selecteer **Azure Active Directory** op de linkernavigatiebalk.
3. Selecteer **Azure AD Connect**.
4. Controleer de **naadloze eenmalige aanmelding** functie wordt weergegeven als **ingeschakeld**.

![Azure-portal - blade van Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a>Stap 3: De functie uitrolt

Als u wilt de functie voor implementatie naar uw gebruikers, moet u de volgende Azure AD-URL's toevoegen aan gebruikers Intranet-beveiligingszone-instellingen met Groepsbeleid in Active Directory:

- https://AutoLogon.microsoftazuread-sso.com
- https://aadg.Windows.NET.nsatc.NET

Bovendien moet u een intranetzone instelling is ingeschakeld (met behulp van Groepsbeleid) 'Updates voor de statusbalk via script toestaan' genoemd.

>[!NOTE]
> De volgende instructies werken alleen voor Internet Explorer en Google Chrome in Windows (als deze reeks URL's van vertrouwde site met Internet Explorer deelt). Lees de volgende sectie voor instructies voor het instellen van Mozilla Firefox en Google Chrome op Mac.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Waarom moet u gebruikers Intranet-beveiligingszone-instellingen wijzigen?

Standaard berekent de browser automatisch de juiste zone (Internet of een Intranet) van een URL. Http://contoso/ is bijvoorbeeld toegewezen aan de zone Internet, terwijl http://intranet.contoso.com/ is toegewezen aan de zone Internet (omdat de URL een periode bevat). Browsers verzenden niet Kerberos-tickets om een cloudeindpunt - als de twee URL voor Azure AD - als de URL is expliciet zijn toegevoegd aan de zone Lokaal Intranet van de browser.

### <a name="detailed-steps"></a>Gedetailleerde stappen

1. Open het hulpprogramma Group Policy Management.
2. Bewerk het groepsbeleidobject dat wordt toegepast op sommige of alle gebruikers. In dit voorbeeld gebruiken we de **standaarddomeinbeleid**.
3. Navigeer naar **Configuration\Administrative Templates\Windows Explorer\Onderdeel Internet Control Panel\Security op de gebruikerspagina** en selecteer **Site lijst van zonetoewijzingen**.
![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso6.png)
4. Schakelt u het beleid en voer de volgende waarden (URL's Azure AD waarin Kerberos-tickets worden doorgestuurd) en gegevens (*1* intranetzone geeft) in het dialoogvenster.

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> Als u weigeren van sommige gebruikers met een naadloze eenmalige aanmelding - bijvoorbeeld wilt, als deze gebruikers op gedeelde kiosken aanmelden zich - de voorgaande waarden instellen op *4*. Deze actie wordt de Azure AD-URL's toegevoegd aan de Zone websites met beperkte toegang en naadloze eenmalige aanmelding voortdurend mislukt.

5. Klik op **OK** en **OK** opnieuw.
![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso7.png)
6. Navigeer naar **gebruiker Configuration\Administrative Templates\Windows Explorer\Onderdeel Internet Control Panel\Security Page\Intranet Zone** en selecteer **toestaan dat updates statusbalk via script**.
![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso11.png)
7. De instelling is ingeschakeld en klik op **OK**.
![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso12.png)

### <a name="browser-considerations"></a>Browser-overwegingen

#### <a name="mozilla-firefox"></a>Mozilla Firefox

Mozilla Firefox niet automatisch Kerberos-verificatie. Elke gebruiker heeft de Azure AD-URL's handmatig toevoegen aan hun Firefox-instellingen met behulp van de volgende stappen uit:
1. Firefox uitvoeren en voer `about:config` in de adresbalk. Meldingen die u ziet worden genegeerd.
2. Zoeken naar de **network.negotiate-auth.trusted-URI's** voorkeur. Deze voorkeur geeft een lijst met vertrouwde sites van Firefox voor Kerberos-verificatie.
3. Met de rechtermuisknop en selecteer 'Wijzigen'.
4. Voer "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" in het veld.
5. Klik op 'OK' en Open de browser.

#### <a name="safari-on-mac-os"></a>Safari op Mac OS

Zorg ervoor dat de computer uitgevoerd Mac OS wordt gekoppeld aan AD. Zie de instructies over hoe u die [hier](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).

#### <a name="google-chrome-on-mac-os"></a>Google Chrome op Mac OS

Raadpleeg voor Google Chrome op Mac OS en andere niet-Windows-platforms, [in dit artikel](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) voor meer informatie over geaccepteerde de Azure AD-URL's voor geïntegreerde verificatie.

De Azure AD-URL's Firefox en Google Chrome implementeert op Mac-gebruikers met behulp van Active Directory-groepsbeleid-uitbreidingen van derden is buiten het bereik van dit artikel.

#### <a name="known-browser-limitations"></a>Bekende browser beperkingen

Naadloze eenmalige aanmelding werkt niet in de privémodus Browse Firefox en rand browsers. Deze ook werkt niet op Internet Explorer als de browser wordt uitgevoerd in de modus voor uitgebreide beveiliging.

>[!IMPORTANT]
>We onlangs teruggedraaid ondersteuning voor de rand om klanten gemelde problemen te onderzoeken.

## <a name="step-4-test-the-feature"></a>Stap 4: De functie testen

Zorg ervoor dat voor het testen van de functie voor een specifieke gebruiker, _alle_ de volgende voorwaarden wordt voldaan:
  - De gebruiker zich aanmeldt op een zakelijke apparaat.
  - Het apparaat is eerder toegevoegd aan uw domein met Active Directory (AD).
  - Het apparaat heeft een directe verbinding naar uw domeincontroller (DC), op het bekabelde of draadloze bedrijfsnetwerk of via een externe verbinding, zoals een VPN-verbinding.
  - U hebt [uitgerold van de functie](##step-3-roll-out-the-feature) aan deze gebruiker met behulp van Groepsbeleid.

Voor het testen van het scenario waarbij de gebruiker alleen de gebruikersnaam, maar niet het wachtwoord invoert:
- Meld u aan bij *https://myapps.microsoft.com/* in een nieuwe persoonlijke browsersessie.

Voor het testen van het scenario waarbij de gebruiker niet heeft de gebruikersnaam of het wachtwoord in te voeren: 
- Meld u aan bij *https://myapps.microsoft.com/contoso.onmicrosoft.com* in een nieuwe persoonlijke browsersessie. Vervang '*contoso*"met de naam van uw tenant.
- Of meld u aan bij *https://myapps.microsoft.com/contoso.com* in een nieuwe persoonlijke browsersessie. Vervang '*contoso.com*' met een geverifieerd domein (niet een federatieve domein) in uw tenant.

## <a name="step-5-roll-over-keys"></a>Stap 5: Sleutels rollover

Azure AD Connect maakt in stap 2 computeraccounts (voor Azure AD) in alle AD-forests waarop naadloze eenmalige aanmelding is ingeschakeld. Meer informatie over meer in detail [hier](active-directory-aadconnect-sso-how-it-works.md). Het wordt aanbevolen dat u regelmatig via het Kerberos ontsleutelingssleutels van deze computeraccounts rouleert voor betere beveiliging. De instructies voor het overschakelen zijn [hier](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account).

>[!IMPORTANT]
>U hoeft niet te doen van deze stap _onmiddellijk_ nadat u de functie hebt ingeschakeld. De Kerberos-sleutels voor ontsleuteling overschakelen met ten minste elke 30 dagen.

## <a name="next-steps"></a>Volgende stappen

- [Technische diepgaand](active-directory-aadconnect-sso-how-it-works.md) -begrijpen hoe deze functie werkt.
- [Veelgestelde vragen](active-directory-aadconnect-sso-faq.md) -antwoorden op veelgestelde vragen.
- [Problemen met](active-directory-aadconnect-troubleshoot-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.

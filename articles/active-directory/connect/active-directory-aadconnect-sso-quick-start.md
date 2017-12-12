---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding - snel aan de slag | Microsoft Docs'
description: Dit artikel wordt beschreven hoe u aan de slag met Azure Active Directory naadloze eenmalige aanmelding
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
ms.date: 12/05/2017
ms.author: billmath
ms.openlocfilehash: b533df58d24b3bc76a229ad09c682d1d8aeaf741
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory naadloze eenmalige aanmelding: snel starten

## <a name="deploy-seamless-single-sign-on"></a>Naadloze eenmalige aanmelding implementeren

Azure Active Directory (Azure AD) naadloze eenmalige aanmelding (SSO naadloze) automatisch meldt zich aan gebruikers wanneer ze op hun zakelijke bureaubladen die zijn verbonden met uw bedrijfsnetwerk. Naadloze eenmalige aanmelding biedt uw gebruikers eenvoudig toegang krijgen tot uw cloud-gebaseerde toepassingen zonder extra on-premises onderdelen.

Volg deze stappen voor het implementeren van naadloze eenmalige aanmelding.

## <a name="step-1-check-the-prerequisites"></a>Stap 1: Controleer de vereisten

Zorg ervoor dat de volgende vereisten voldaan is:

* **Instellen van uw Azure AD Connect-server**: als u [Pass through-verificatie](active-directory-aadconnect-pass-through-authentication.md) als uw contactmethode aanmelden geen aanvullende controle op vereisten is vereist. Als u [synchronisatie van wachtwoordhash](active-directory-aadconnectsync-implement-password-synchronization.md) als uw contactmethode aanmelden en als er een firewall tussen Azure AD Connect en Azure AD, zorg ervoor dat:
   - U versie 1.1.644.0 gebruiken of hoger van Azure AD Connect. 
   - Als uw firewall of proxyserver kunt DNS-whitelisting, geaccepteerde de verbindingen met de  **\*. msappproxy.net** URL's via poort 443. Als dit niet het geval is, zodat u toegang hebben tot de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653), die wekelijks worden bijgewerkt. Deze vereiste geldt alleen wanneer u de functie inschakelt. Het is niet vereist voor de werkelijke gebruikersaanmeldingen.

    >[!NOTE]
    >Azure AD Connect-versies 1.1.557.0, 1.1.558.0 1.1.561.0 en 1.1.614.0 hebben een probleem met de synchronisatie van wachtwoordhash. Als u _niet_ wilt synchronisatie van wachtwoordhash gebruiken in combinatie met Pass through-verificatie, lees de [opmerkingen bij de release van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) voor meer informatie.

* **Instellen van referenties voor de domeinbeheerder**: U moet referenties voor de domeinbeheerder hebben voor elk Active Directory-forest:
    * U synchroniseren met Azure AD via Azure AD Connect.
    * Gebruikers die u wilt inschakelen voor naadloze eenmalige aanmelding bevat.

## <a name="step-2-enable-the-feature"></a>Stap 2: De functie inschakelen

Naadloze eenmalige aanmelding via inschakelen [Azure AD Connect](active-directory-aadconnect.md).

Als u een nieuwe installatie van Azure AD Connect uitvoert, kiest u de [aangepaste installatiepad](active-directory-aadconnect-get-started-custom.md). Op de **gebruikersaanmelding** pagina de **eenmalige aanmelding inschakelen op** optie.

![Azure AD Connect: Gebruiker aanmelden](./media/active-directory-aadconnect-sso/sso8.png)

Als u al een installatie van Azure AD Connect hebt, selecteert u de **wijzigen gebruikersaanmelding** pagina in Azure AD Connect en selecteer vervolgens **volgende**.

![Azure AD Connect: De gebruiker aanmelden wijzigen](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Doorgaan met de wizard totdat u de **eenmalige aanmelding inschakelen op** pagina. Geef referenties voor de domeinbeheerder voor elk Active Directory-forest:
    * U synchroniseren met Azure AD via Azure AD Connect.
    * Gebruikers die u wilt inschakelen voor naadloze eenmalige aanmelding bevat.

Naadloze eenmalige aanmelding is na voltooiing van de wizard wordt ingeschakeld op uw tenant.

>[!NOTE]
> Referenties voor de domeinbeheerder zijn niet opgeslagen in Azure AD Connect of in Azure AD. Ze zijn alleen gebruikt voor de functie inschakelen.

Volg deze instructies om te controleren of naadloze eenmalige aanmelding correct ingeschakeld:

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de referenties van de globale beheerder voor uw tenant.
2. Selecteer **Azure Active Directory** in het linkerdeelvenster.
3. Selecteer **Azure AD Connect**.
4. Controleer de **naadloze eenmalige aanmelding** functie wordt weergegeven als **ingeschakeld**.

![Azure-portal: deelvenster Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a>Stap 3: De functie uitrolt

Als u wilt de functie voor implementatie naar uw gebruikers, moet u de volgende Azure AD-URL's toevoegen aan de gebruikers Intranet-beveiligingszone-instellingen met behulp van Groepsbeleid in Active Directory:

- https://AutoLogon.microsoftazuread-sso.com
- https://aadg.Windows.NET.nsatc.NET

Bovendien moet u een Intranet zone beleidsinstelling opgeroepen inschakelen **toestaan dat updates statusbalk via script** via Groepsbeleid. 

>[!NOTE]
> De volgende instructies werkt alleen voor Internet Explorer en Google Chrome op Windows (als deze een reeks URL's van vertrouwde site met Internet Explorer deelt). Lees de volgende sectie voor instructies over het instellen van Mozilla Firefox en Google Chrome op Mac.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Waarom moet u gebruikers Intranet-beveiligingszone-instellingen wijzigen?

De browser berekent standaard automatisch de juiste zone Internet- of intranethosts van een specifieke URL. Bijvoorbeeld 'http://contoso/' worden toegewezen aan de zone Internet, terwijl "http://intranet.contoso.com/" wordt toegewezen aan de zone Internet (omdat de URL een periode bevat). Browsers verzenden Kerberos-tickets niet naar een cloudeindpunt, zoals de twee Azure AD-URL's, tenzij u expliciet de URL van de browser intranetzone.

### <a name="detailed-steps"></a>Gedetailleerde stappen

1. Open de Editor voor Groepsbeleidsbeheer-hulpprogramma.
2. Bewerk het groepsbeleidobject dat wordt toegepast op sommige of alle gebruikers. In dit voorbeeld wordt **standaarddomeinbeleid**.
3. Blader naar **Gebruikersconfiguratie** > **Beheersjablonen** > **Windows-onderdelen**  >   **Internet Explorer** > **Configuratiescherm Internet** > **beveiligingspagina**. Selecteer vervolgens **Site lijst van zonetoewijzingen**.
    ![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso6.png)
4. Schakelt u het beleid en voer de volgende waarden in het dialoogvenster:
   - **Waardenaam**: de Azure AD-URL's waarin de Kerberos-tickets worden doorgestuurd.
   - **Waarde** (gegevens): **1** geeft aan de intranetzone.

   Het resultaat ziet er als volgt:

    Waarde: https://autologon.microsoftazuread-sso.com
  
    Gegevens: 1
        
   Waarde: https://aadg.windows.net.nsatc.net

    Gegevens: 1

   >[!NOTE]
   > Als u weigeren sommige gebruikers met een naadloze eenmalige aanmelding wilt (bijvoorbeeld, als deze gebruikers zich op gedeelde kiosken aanmelden), de voorgaande waarden instellen op **4**. Deze actie wordt de Azure AD-URL's toegevoegd aan de zone met beperkte toegang en naadloze eenmalige aanmelding voortdurend mislukt.
   >

5. Selecteer **OK**, en selecteer vervolgens **OK** opnieuw.

    ![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso7.png)

6. Blader naar **Gebruikersconfiguratie** > **Beheersjablonen** > **Windows-onderdelen**  >   **Internet Explorer** > **Configuratiescherm Internet** > **beveiligingspagina** > **intranetzone**. Selecteer vervolgens **toestaan dat updates statusbalk via script**.

    ![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso11.png)

7. De instelling is ingeschakeld, en selecteer vervolgens **OK**.

    ![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso12.png)

### <a name="browser-considerations"></a>Browser-overwegingen

#### <a name="mozilla-firefox-all-platforms"></a>Mozilla Firefox (alle platforms)

Mozilla Firefox gebruik niet automatisch van Kerberos-verificatie. Elke gebruiker moet de Azure AD-URL's handmatig toevoegen aan hun Firefox-instellingen met behulp van de volgende stappen uit:
1. Firefox uitvoeren en voer `about:config` in de adresbalk. Meldingen die u ziet worden genegeerd.
2. Zoeken naar de **network.negotiate-auth.trusted-URI's** voorkeur. Deze voorkeur geeft een lijst met vertrouwde sites van Firefox voor Kerberos-verificatie.
3. Met de rechtermuisknop en selecteer **wijzigen**.
4. Voer https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net in het veld.
5. Selecteer **OK** en de browser opnieuw te openen.

#### <a name="safari-mac-os"></a>Safari (Mac OS)

Zorg ervoor dat de machine met de Mac OS wordt gekoppeld aan Azure AD. Zie voor instructies voor deelname aan Azure AD, [Best Practices voor OS X integreren met Active Directory](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).

#### <a name="google-chrome-all-platforms"></a>Google Chrome (alle platforms)

Als u onderdrukt hebt de [AuthNegotiateDelegateWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthNegotiateDelegateWhitelist) of de [AuthServerWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthServerWhitelist) beleidsinstellingen in uw omgeving, zorg ervoor dat u toevoegt, URL's van Azure AD (https:// AutoLogon.microsoftazuread sso.com en https://aadg.windows.net.nsatc.net) naar deze ook.

#### <a name="google-chrome-mac-os-only"></a>Google Chrome (alleen Mac OS)

Raadpleeg voor Google Chrome op Mac OS en andere niet-Windows-platforms, [de lijst van het Project beleid chroom](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) voor meer informatie over geaccepteerde de Azure AD-URL's voor geïntegreerde verificatie.

Het gebruik van Active Directory-groepsbeleid-uitbreidingen van derden de Azure AD-URL's Firefox en Google Chrome implementeert op Mac-gebruikers is buiten het bereik van dit artikel.

#### <a name="known-browser-limitations"></a>Bekende browser beperkingen

Naadloze eenmalige aanmelding werkt niet in de privémodus Browse Firefox en rand browsers. Deze ook werkt niet op Internet Explorer als de browser wordt uitgevoerd in de uitgebreide beveiligde modus.

## <a name="step-4-test-the-feature"></a>Stap 4: De functie testen

Als u wilt testen van de functie voor een specifieke gebruiker, zorg ervoor dat de volgende voorwaarden voldaan is:
  - De gebruiker zich aanmeldt op een zakelijke apparaat.
  - Het apparaat is gekoppeld aan uw Active Directory-domein.
  - Het apparaat heeft een directe verbinding met de domeincontroller (DC) op het bekabelde of draadloze bedrijfsnetwerk of via een externe verbinding, zoals een VPN-verbinding.
  - U hebt [uitgerold van de functie](##step-3-roll-out-the-feature) aan deze gebruiker via Groepsbeleid.

Voor het testen van het scenario waarbij de gebruiker alleen de gebruikersnaam, maar niet het wachtwoord invoert:
   - Aanmelden bij https://myapps.microsoft.com/ in een nieuwe persoonlijke browsersessie.

U kunt testen de scenario, waarin de gebruiker niet de gebruikersnaam of het wachtwoord in te voeren met een van deze stappen: 
   - Aanmelden bij https://myapps.microsoft.com/contoso.onmicrosoft.com in een nieuwe persoonlijke browsersessie. Vervang *contoso* met de naam van uw tenant.
   - Aanmelden bij https://myapps.microsoft.com/contoso.com in een nieuwe persoonlijke browsersessie. Vervang *contoso.com* met een geverifieerd domein (niet een federatieve domein) op uw tenant.

## <a name="step-5-roll-over-keys"></a>Stap 5: Sleutels rollover

Azure AD Connect maakt in stap 2 computeraccounts (voor Azure AD) in alle Active Directory-forests waarop naadloze eenmalige aanmelding is ingeschakeld. Zie voor meer informatie, [Azure Active Directory naadloze eenmalige aanmelding: technische diepgaand](active-directory-aadconnect-sso-how-it-works.md). Voor betere beveiliging wordt u aangeraden dat u regelmatig via het Kerberos ontsleutelingssleutels van deze computeraccounts rouleert. Zie voor instructies over het overschakelen van sleutels [Azure Active Directory naadloze eenmalige aanmelding: veelgestelde vragen over](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account).

>[!IMPORTANT]
>U hoeft niet te doen van deze stap _onmiddellijk_ nadat u de functie hebt ingeschakeld. De Kerberos-sleutels voor ontsleuteling overschakelen met ten minste één keer voor elke 30 dagen.

## <a name="next-steps"></a>Volgende stappen

- [Technische diepgaand](active-directory-aadconnect-sso-how-it-works.md): inzicht in hoe de functie naadloze eenmalige aanmelding werkt.
- [Veelgestelde vragen over](active-directory-aadconnect-sso-faq.md): antwoorden op veelgestelde vragen over naadloze eenmalige aanmelding.
- [Problemen met](active-directory-aadconnect-troubleshoot-sso.md): informatie over het oplossen van veelvoorkomende problemen met de functie naadloze eenmalige aanmelding.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.

---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding - snel starten | Microsoft Docs'
description: In dit artikel wordt beschreven hoe u aan de slag met Azure Active Directory naadloze eenmalige aanmelding
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
ms.date: 02/23/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: f8639cbb5c7ba86b4786f3d0b913d64bad59ad66
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917513"
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory naadloze eenmalige aanmelding: snel starten

## <a name="deploy-seamless-single-sign-on"></a>Naadloze eenmalige aanmelding implementeren

Azure Active Directory (Azure AD) naadloze eenmalige aanmelding (naadloze eenmalige aanmelding) automatisch meldt zich aan gebruikers wanneer ze op hun zakelijke pc's die zijn verbonden met uw bedrijfsnetwerk. Naadloze eenmalige aanmelding biedt uw gebruikers eenvoudig toegang tot uw cloud-toepassingen zonder extra on-premises onderdelen.

Volg deze stappen voor het implementeren van naadloze eenmalige aanmelding.

## <a name="step-1-check-the-prerequisites"></a>Stap 1: Controleer de vereisten

Zorg ervoor dat de volgende vereisten voldaan is:

* **Instellen van uw Azure AD Connect-server**: als u [Pass through-verificatie](active-directory-aadconnect-pass-through-authentication.md) als uw contactmethode aanmelden geen aanvullende controle is vereist. Als u [wachtwoord-hashsynchronisatie](active-directory-aadconnectsync-implement-password-hash-synchronization.md) als uw contactmethode aanmelden en als er een firewall is tussen Azure AD Connect en Azure AD, zorg ervoor dat:
   - U versie 1.1.644.0 of hoger van Azure AD Connect. 
   - Als uw firewall of proxyserver kunt DNS-whitelist, lijst met toegestane adressen de verbindingen met de  **\*. msappproxy.net** URL's via poort 443. Als dit niet het geval is, kunt u toegang tot de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653), die elke week worden bijgewerkt. Deze vereiste is van toepassing alleen wanneer u de functie inschakelt. Het is niet vereist voor de werkelijke gebruikers-aanmeldingen.

    >[!NOTE]
    >Azure AD Connect-versies 1.1.557.0, 1.1.558.0 1.1.561.0 en 1.1.614.0 is een probleem met betrekking tot wachtwoord-hashsynchronisatie. Als u _niet_ wilt wachtwoord-hashsynchronisatie gebruiken in combinatie met Pass through-verificatie, lees de [opmerkingen bij de release van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) voor meer informatie.

* **Instellen van referenties voor de domeinbeheerder**: U moet domeinbeheerdersreferenties hebben voor elk Active Directory-forest:
    * U synchroniseren met Azure AD via Azure AD Connect.
    * Gebruikers die u wilt inschakelen voor naadloze eenmalige aanmelding bevat.

## <a name="step-2-enable-the-feature"></a>Stap 2: De functie inschakelen

Naadloze eenmalige aanmelding via inschakelen [Azure AD Connect](active-directory-aadconnect.md).

Als u een nieuwe installatie van Azure AD Connect uitvoert, kiest u de [aangepaste installatiepad](active-directory-aadconnect-get-started-custom.md). Op de **aanmelden van gebruikers** weergeeft, schakelt de **eenmalige aanmelding inschakelen** optie.

![Azure AD Connect: Aanmelden van gebruikers](./media/active-directory-aadconnect-sso/sso8.png)

Als u al een installatie van Azure AD Connect hebt, selecteert u de **aanmelden van gebruikers wijzigen** pagina in Azure AD Connect en selecteer vervolgens **volgende**.

![Azure AD Connect: Het aanmelden van gebruikers wijzigen](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Ga door met de wizard totdat u de **eenmalige aanmelding inschakelen** pagina. Geef referenties voor de domeinbeheerder voor elk Active Directory-forest:
    * U synchroniseren met Azure AD via Azure AD Connect.
    * Gebruikers die u wilt inschakelen voor naadloze eenmalige aanmelding bevat.

Naadloze eenmalige aanmelding is na voltooiing van de wizard wordt ingeschakeld op uw tenant.

>[!NOTE]
> De referenties voor de domeinbeheerder zijn niet opgeslagen in Azure AD Connect of in Azure AD. Ze worden gebruikt om alleen in te schakelen van de functie.

Volg deze instructies om te controleren of u naadloze eenmalige aanmelding correct hebt ingeschakeld:

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de hoofdbeheerdersreferenties voor uw tenant.
2. Selecteer **Azure Active Directory** in het linkerdeelvenster.
3. Selecteer **Azure AD Connect**.
4. Controleer de **naadloze eenmalige aanmelding** functie wordt weergegeven als **ingeschakeld**.

![Azure-portal: Azure AD Connect-deelvenster](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a>Stap 3: Implementatie van de functie

Voor de functie voor implementatie in uw gebruikers, moet u de volgende Azure AD-URL toevoegen aan zone-instellingen van de gebruikers Intranet met behulp van Groepsbeleid in Active Directory:

- https://autologon.microsoftazuread-sso.com


Bovendien moet u een Intranet zone beleid instelling inschakelen **toestaan van updates op de statusbalk via script** via Groepsbeleid. 

>[!NOTE]
> De volgende instructies werken alleen voor Internet Explorer en Google Chrome in Windows (indien deze een reeks URL's voor vertrouwde sites in Internet Explorer deelt). Lees de volgende sectie voor instructies over het instellen van Mozilla Firefox en Google Chrome op Mac.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Waarom moet u gebruikers Intranet zone-instellingen wijzigen?

Standaard berekent de browser automatisch de juiste zone, Internet- of intranethosts van een specifieke URL. Bijvoorbeeld, "http://contoso/'wordt toegewezen aan de intranetzone, terwijl'http://intranet.contoso.com/' wordt toegewezen aan de zone Internet (omdat de URL een periode bevat). Browsers wordt Kerberos-tickets niet verzenden naar een cloudeindpunt, zoals de URL van de Azure AD, tenzij u expliciet de URL aan de intranetzone van de browser toevoegen.

### <a name="detailed-steps"></a>Gedetailleerde stappen

1. Open de Editor voor Groepsbeleidsbeheer-hulpprogramma.
2. Bewerk het groepsbeleid dat wordt toegepast op sommige of alle gebruikers. In dit voorbeeld wordt **standaarddomeinbeleid**.
3. Blader naar **Gebruikersconfiguratie** > **Beheersjablonen** > **Windows-onderdelen**  >   **Internet Explorer** > **Configuratiescherm Internet** > **beveiligingspagina**. Selecteer vervolgens **Site lijst van zonetoewijzingen**.
    ![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso6.png)
4. Het beleid inschakelen en voer de volgende waarden in het dialoogvenster:
   - **Waardenaam**: de Azure AD-URL waar de Kerberos-tickets worden doorgestuurd.
   - **Waarde** (gegevens): **1** geeft aan dat de intranetzone.

    Het resultaat ziet er zo uit:

    Waarde:https://autologon.microsoftazuread-sso.com
  
    Gegevens: 1

   >[!NOTE]
   > Als u weigeren van sommige gebruikers met een naadloze eenmalige aanmelding wilt (bijvoorbeeld, als deze gebruikers zich aanmelden op gedeelde kiosken), de bovenstaande waarden ingesteld op **4**. Deze actie wordt de URL van de Azure AD toegevoegd aan de zone met beperkte toegang en naadloze eenmalige aanmelding voortdurend mislukt.
   >

5. Selecteer **OK**, en selecteer vervolgens **OK** opnieuw.

    ![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso7.png)

6. Blader naar **Gebruikersconfiguratie** > **Beheersjablonen** > **Windows-onderdelen**  >   **Internet Explorer** > **Configuratiescherm Internet** > **beveiligingspagina** > **intranetzone**. Selecteer vervolgens **toestaan van updates op de statusbalk via script**.

    ![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso11.png)

7. De instelling is ingeschakeld en selecteer vervolgens **OK**.

    ![Eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso12.png)

### <a name="browser-considerations"></a>Overwegingen voor browser

#### <a name="mozilla-firefox-all-platforms"></a>Mozilla Firefox (alle platformen)

Mozilla Firefox niet automatisch Kerberos-verificatie gebruiken. Elke gebruiker moet handmatig de URL van de Azure AD toevoegen aan hun Firefox-instellingen met behulp van de volgende stappen uit:
1. Voer Firefox en geef `about:config` in de adresbalk. Negeren meldingen die worden weergegeven.
2. Zoek de **network.negotiate-auth.trusted-URI's** voorkeur. Met deze voorkeursinstelling geeft een lijst met vertrouwde sites van Firefox voor Kerberos-verificatie.
3. Met de rechtermuisknop op en selecteer **wijzigen**.
4. Voer https://autologon.microsoftazuread-sso.com in het veld.
5. Selecteer **OK** en Open de browser.

#### <a name="safari-mac-os"></a>Safari (Mac OS)

Zorg ervoor dat de machine met de Mac OS is gekoppeld aan AD. Zie voor instructies over het koppelen van AD [Best Practices voor OS X integreren met Active Directory](http://www.isaca.org/Groups/Professional-English/identity-management/GroupDocuments/Integrating-OS-X-with-Active-Directory.pdf).

#### <a name="google-chrome-all-platforms"></a>Google Chrome (alle platformen)

Als u onderdrukt hebt de [AuthNegotiateDelegateWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthNegotiateDelegateWhitelist) of de [AuthServerWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthServerWhitelist) beleidsinstellingen in uw omgeving, zorg ervoor dat u Azure AD-URL toevoegen (https://autologon.microsoftazuread-sso.com) aan hen ook.

#### <a name="google-chrome-mac-os-only"></a>Google Chrome (alleen Mac OS)

Raadpleeg voor Google Chrome op Mac OS- en andere niet-Windows-platforms, [de chroom Project Policy List](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) voor meer informatie over hoe aan lijst met geaccepteerde de URL van de Azure AD voor verificatie geïntegreerd.

Het gebruik van Active Directory-groepsbeleid uitbreidingen van derden in de URL van de Azure AD Firefox en Google Chrome op Mac-gebruikers is buiten het bereik van dit artikel.

#### <a name="known-browser-limitations"></a>Browser bekende beperkingen

Naadloze eenmalige aanmelding werkt niet in de privémodus bladeren in Firefox en Edge browsers. Dit ook werkt niet in Internet Explorer als de browser wordt uitgevoerd in de uitgebreide beveiligde modus.

## <a name="step-4-test-the-feature"></a>Stap 4: De functie testen

Als u wilt testen van de functie voor een specifieke gebruiker, zorg ervoor dat de volgende voorwaarden voldaan is:
  - De gebruiker zich aanmeldt op een bedrijfsapparaat.
  - Het apparaat is toegevoegd aan uw Active Directory-domein.
  - Het apparaat heeft een directe verbinding met de domeincontroller (DC), op het bedrijfsnetwerk bekabelde of draadloze of via een RAS-verbinding, zoals een VPN-verbinding.
  - U hebt [bekendgemaakt die de functie](##step-3-roll-out-the-feature) aan deze gebruiker via Groepsbeleid.

Voor het testen van het scenario waarin de gebruiker alleen de gebruikersnaam, maar niet het wachtwoord invoeren:
   - Aanmelden bij https://myapps.microsoft.com/ in een nieuwe persoonlijke browsersessie.

Als u wilt testen van het scenario waarin de gebruiker beschikt niet over de gebruikersnaam of het wachtwoord invoeren, gebruikt u een van deze stappen: 
   - Aanmelden bij https://myapps.microsoft.com/contoso.onmicrosoft.com in een nieuwe persoonlijke browsersessie. Vervang *contoso* met de naam van uw tenant.
   - Aanmelden bij https://myapps.microsoft.com/contoso.com in een nieuwe persoonlijke browsersessie. Vervang *contoso.com* met een geverifieerd domein (niet een federatief domein) op uw tenant.

## <a name="step-5-roll-over-keys"></a>Stap 5: Meegenomen sleutels

Azure AD Connect maakt in stap 2, computeraccounts (Azure AD representeren) in alle van de Active Directory-forests waarop u naadloze eenmalige aanmelding hebt ingeschakeld. Zie voor meer informatie, [Azure Active Directory naadloze eenmalige aanmelding: technische details](active-directory-aadconnect-sso-how-it-works.md). Voor betere beveiliging wordt u aangeraden dat u periodiek de Kerberos ontsleutelingssleutels van deze computeraccounts terugdraaien. Zie voor instructies over het meenemen van sleutels, [Azure Active Directory naadloze eenmalige aanmelding: veelgestelde vragen over](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account).

>[!IMPORTANT]
>U hoeft te doen in deze stap _onmiddellijk_ nadat u de functie hebt ingeschakeld. Beweeg de muis over de Kerberos-sleutels voor ontsleuteling ten minste één keer voor elke 30 dagen.

## <a name="next-steps"></a>Volgende stappen

- [Technische details](active-directory-aadconnect-sso-how-it-works.md): informatie over de werking van de functie voor naadloze eenmalige aanmelding.
- [Veelgestelde vragen over](active-directory-aadconnect-sso-faq.md): krijg antwoorden op veelgestelde vragen over naadloze eenmalige aanmelding.
- [Problemen oplossen](active-directory-aadconnect-troubleshoot-sso.md): informatie over het oplossen van veelvoorkomende problemen met de functie voor naadloze eenmalige aanmelding.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.

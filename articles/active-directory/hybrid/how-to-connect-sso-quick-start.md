---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding - snel starten | Microsoft Docs'
description: In dit artikel wordt beschreven hoe u aan de slag met Azure Active Directory naadloze eenmalige aanmelding
services: active-directory
keywords: Wat is Azure AD Connect, Active Directory installeren onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 345c97a19f789bb3d850df000824d4c23989a81f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58086816"
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory naadloze eenmalige aanmelding: Snel starten

## <a name="deploy-seamless-single-sign-on"></a>Naadloze eenmalige aanmelding implementeren

Azure Active Directory (Azure AD) naadloze eenmalige aanmelding (naadloze eenmalige aanmelding) automatisch meldt zich aan gebruikers wanneer ze op hun zakelijke pc's die zijn verbonden met uw bedrijfsnetwerk. Naadloze eenmalige aanmelding biedt uw gebruikers eenvoudig toegang tot uw cloud-toepassingen zonder extra on-premises onderdelen.

Volg deze stappen voor het implementeren van naadloze eenmalige aanmelding.

## <a name="step-1-check-the-prerequisites"></a>Stap 1: Vereisten controleren

Zorg ervoor dat de volgende vereisten voldaan is:

* **Instellen van uw Azure AD Connect-server**: Als u [Pass through-verificatie](how-to-connect-pta.md) als uw contactmethode aanmelden geen aanvullende controle is vereist. Als u [wachtwoord-hashsynchronisatie](how-to-connect-password-hash-synchronization.md) als uw contactmethode aanmelden en als er een firewall is tussen Azure AD Connect en Azure AD, zorg ervoor dat:
   - U versie 1.1.644.0 of hoger van Azure AD Connect. 
   - Als uw firewall of proxyserver kunt DNS-whitelist, lijst met toegestane adressen de verbindingen met de  **\*. msappproxy.net** URL's via poort 443. Als dit niet het geval is, kunt u toegang tot de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653), die elke week worden bijgewerkt. Deze vereiste is van toepassing alleen wanneer u de functie inschakelt. Het is niet vereist voor de werkelijke gebruikers-aanmeldingen.

    >[!NOTE]
    >Azure AD Connect-versies 1.1.557.0, 1.1.558.0 1.1.561.0 en 1.1.614.0 is een probleem met betrekking tot wachtwoord-hashsynchronisatie. Als u _niet_ wilt wachtwoord-hashsynchronisatie gebruiken in combinatie met Pass through-verificatie, lees de [opmerkingen bij de release van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) voor meer informatie.

* **Gebruik een ondersteunde Azure AD Connect-topologie**: Zorg ervoor dat u van een van de Azure AD Connect van ondersteunde topologieën beschreven gebruikmaakt [hier](plan-connect-topologies.md).

    >[!NOTE]
    >Naadloze eenmalige aanmelding ondersteunt meerdere AD-forests, of AD-vertrouwensrelaties er daartussen of niet zijn.

* **Instellen van referenties voor de domeinbeheerder**: U moet domeinbeheerdersreferenties hebben voor elk Active Directory-forest:
    * U synchroniseren met Azure AD via Azure AD Connect.
    * Gebruikers die u wilt inschakelen voor naadloze eenmalige aanmelding bevat.
    
* **Moderne verificatie inschakelen**: U moet inschakelen [moderne verificatie](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) op uw tenant voor deze functie om te werken.

* **Gebruik de meest recente versies van Office 365-clients**: Als u een op de achtergrond aanmelding met Office 365-clients (Outlook, Word, Excel en anderen), uw gebruikers moeten versies 16.0.8730.xxxx gebruiken of hoger.

## <a name="step-2-enable-the-feature"></a>Stap 2: De functie inschakelen

Naadloze eenmalige aanmelding via inschakelen [Azure AD Connect](whatis-hybrid-identity.md).

>[!NOTE]
> U kunt ook [inschakelen naadloze eenmalige aanmelding met behulp van PowerShell](tshoot-connect-sso.md#manual-reset-of-the-feature) als Azure AD Connect niet voldoet aan uw vereisten. Gebruik deze optie als u meer dan één domein per Active Directory-forest hebt, en u wilt meer over het domein dat u wilt inschakelen, naadloze eenmalige aanmelding voor zijn gericht.

Als u een nieuwe installatie van Azure AD Connect uitvoert, kiest u de [aangepaste installatiepad](how-to-connect-install-custom.md). Op de **aanmelden van gebruikers** weergeeft, schakelt de **eenmalige aanmelding inschakelen** optie.

>[!NOTE]
> De optie beschikbaar voor selectie wordt alleen als de aanmeldings-methode is **wachtwoord-Hashsynchronisatie** of **Pass through-verificatie**.

![Azure AD Connect: Gebruikersaanmelding](./media/how-to-connect-sso-quick-start/sso8.png)

Als u al een installatie van Azure AD Connect hebt, selecteert u de **aanmelden van gebruikers wijzigen** pagina in Azure AD Connect en selecteer vervolgens **volgende**. Als u van Azure AD Connect-versies 1.1.880.0 gebruikmaakt of hoger, de **eenmalige aanmelding inschakelen** optie, wordt standaard geselecteerd. Als u van oudere versies van Azure AD Connect gebruikmaakt, selecteert u de **eenmalige aanmelding inschakelen** optie.

![Azure AD Connect: De aanmelding voor gebruiker wijzigen](./media/how-to-connect-sso-quick-start/changeusersignin.png)

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

![Azure Portal: Deelvenster in de Azure AD Connect](./media/how-to-connect-sso-quick-start/sso10.png)

>[!IMPORTANT]
> Naadloze eenmalige aanmelding maakt u een account met de naam `AZUREADSSOACC` (die vertegenwoordigt Azure AD) in uw on-premises Active Directory (AD) in elk AD-forest. Dit computeraccount is nodig voor de functie te gebruiken. Als u van Pass-the-Hash en Credential Theft Mitigation architecturen in uw on-premises-omgeving gebruikmaakt, zorg ervoor dat de `AZUREADSSOACC` computeraccount niet in de quarantaine-container terechtkomen. Breng de gewenste wijzigingen aan het computeraccount maken in de container Computers. Als u naadloze eenmalige aanmelding is ingeschakeld in de Azure AD Connect-wizard, verplaatst u de `AZUREADSSOACC` computeraccount aan een organisatie-eenheid (OE) waar de computeraccounts van andere om ervoor te zorgen dat deze is niet verwijderd per ongeluk worden beheerd.

## <a name="step-3-roll-out-the-feature"></a>Stap 3: Implementatie van de functie

U kunt geleidelijk implementeert naadloze eenmalige aanmelding voor uw gebruikers met behulp van de onderstaande instructies. U start met de volgende Azure AD-URL toe te voegen aan alle of Intranet zone-instellingen van gebruikers zijn geselecteerd met behulp van Groepsbeleid in Active Directory:

- `https://autologon.microsoftazuread-sso.com`

Bovendien moet u een Intranet zone beleid instelling inschakelen **toestaan van updates op de statusbalk via script** via Groepsbeleid. 

>[!NOTE]
> De volgende instructies werken alleen voor Internet Explorer en Google Chrome in Windows (indien deze een reeks URL's voor vertrouwde sites in Internet Explorer deelt). Lees de volgende sectie voor instructies over het instellen van Mozilla Firefox en Google Chrome in macOS.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Waarom moet u gebruikers Intranet zone-instellingen wijzigen?

Standaard berekent de browser automatisch de juiste zone, Internet- of intranethosts van een specifieke URL. Bijvoorbeeld, `http://contoso/` dat wordt toegewezen aan de intranetzone `http://intranet.contoso.com/` toegewezen aan de zone Internet (omdat de URL een periode bevat). Browsers wordt Kerberos-tickets niet verzenden naar een cloudeindpunt, zoals de URL van de Azure AD, tenzij u expliciet de URL aan de intranetzone van de browser toevoegen.

Er zijn twee manieren om gebruikers Intranet zone-instellingen te wijzigen:

| Optie | Beheerder overweging | Gebruikerservaring |
| --- | --- | --- |
| Groepsbeleid | Beheerder vergrendelingen omlaag Intranet zone-instellingen te bewerken | Gebruikers kunnen hun eigen instellingen niet wijzigen |
| Voorkeuren voor Groepsbeleid |  Beheerder kan op Intranet zone-instellingen bewerken | Gebruikers kunnen hun eigen instellingen wijzigen |

### <a name="group-policy-option---detailed-steps"></a>"Groepsbeleid" optie - gedetailleerde stappen

1. Open de Editor voor Groepsbeleidsbeheer-hulpprogramma.
2. Bewerk het groepsbeleid dat wordt toegepast op sommige of alle gebruikers. In dit voorbeeld wordt **standaarddomeinbeleid**.
3. Blader naar **Gebruikersconfiguratie** > **beleid** > **Beheersjablonen** > **Windows Onderdelen** > **Internet Explorer** > **Configuratiescherm Internet** > **beveiligingspagina**. Selecteer vervolgens **Site lijst van zonetoewijzingen**.
    ![Eenmalige aanmelding](./media/how-to-connect-sso-quick-start/sso6.png)
4. Het beleid inschakelen en voer de volgende waarden in het dialoogvenster:
   - **Waardenaam**: De Azure AD-URL waar de Kerberos-tickets worden doorgestuurd.
   - **Waarde** (gegevens): **1** geeft aan dat de intranetzone.

     Het resultaat ziet er zo uit:

     Waardenaam: `https://autologon.microsoftazuread-sso.com`
  
     Waarde (gegevens): 1

   >[!NOTE]
   > Als u weigeren van sommige gebruikers met een naadloze eenmalige aanmelding wilt (bijvoorbeeld, als deze gebruikers zich aanmelden op gedeelde kiosken), de bovenstaande waarden ingesteld op **4**. Deze actie wordt de URL van de Azure AD toegevoegd aan de zone met beperkte toegang en naadloze eenmalige aanmelding voortdurend mislukt.
   >

5. Selecteer **OK**, en selecteer vervolgens **OK** opnieuw.

    ![Eenmalige aanmelding](./media/how-to-connect-sso-quick-start/sso7.png)

6. Blader naar **Gebruikersconfiguratie** > **Beheersjablonen** **beleid** > ** > **Windows-onderdelen**  >  **Internet Explorer** > **Configuratiescherm Internet** > **beveiligingspagina**  >   **Intranetzone**. Selecteer vervolgens **toestaan van updates op de statusbalk via script**.

    ![Eenmalige aanmelding](./media/how-to-connect-sso-quick-start/sso11.png)

7. De instelling is ingeschakeld en selecteer vervolgens **OK**.

    ![Eenmalige aanmelding](./media/how-to-connect-sso-quick-start/sso12.png)

### <a name="group-policy-preference-option---detailed-steps"></a>Optie 'Groep voor voorkeuren voor Groepsbeleid' - gedetailleerde stappen

1. Open de Editor voor Groepsbeleidsbeheer-hulpprogramma.
2. Bewerk het groepsbeleid dat wordt toegepast op sommige of alle gebruikers. In dit voorbeeld wordt **standaarddomeinbeleid**.
3. Blader naar **Gebruikersconfiguratie** > **voorkeuren** > **Windows-instellingen** > **register**  >  **Nieuw** > **registeritem**.

    ![Eenmalige aanmelding](./media/how-to-connect-sso-quick-start/sso15.png)

4. Voer de volgende waarden in de juiste velden en klik op **OK**.
   - **Pad sleutel**: ***Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\microsoftazuread-sso.com\autologon***
   - **Waardenaam**: ***https***.
   - **Waardetype**: ***REG_DWORD***.
   - **Waardegegevens**: ***00000001***.
 
     ![Eenmalige aanmelding](./media/how-to-connect-sso-quick-start/sso16.png)
 
     ![Eenmalige aanmelding](./media/how-to-connect-sso-quick-start/sso17.png)

### <a name="browser-considerations"></a>Overwegingen voor browser

#### <a name="mozilla-firefox-all-platforms"></a>Mozilla Firefox (alle platformen)

Mozilla Firefox niet automatisch Kerberos-verificatie gebruiken. Elke gebruiker moet handmatig de URL van de Azure AD toevoegen aan hun Firefox-instellingen met behulp van de volgende stappen uit:
1. Voer Firefox en geef `about:config` in de adresbalk. Negeren meldingen die worden weergegeven.
2. Zoek de **network.negotiate-auth.trusted-URI's** voorkeur. Met deze voorkeursinstelling geeft een lijst met vertrouwde sites van Firefox voor Kerberos-verificatie.
3. Met de rechtermuisknop op en selecteer **wijzigen**.
4. Voer `https://autologon.microsoftazuread-sso.com` in het veld.
5. Selecteer **OK** en Open de browser.

#### <a name="safari-macos"></a>Safari (macOS)

Zorg ervoor dat de machine met de macOS is gekoppeld aan AD. Instructies voor het lidmaatschap van AD uw macOS-apparaat is buiten het bereik van dit artikel.

#### <a name="google-chrome-all-platforms"></a>Google Chrome (alle platformen)

Als u hebt onderdrukt de [AuthNegotiateDelegateWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthNegotiateDelegateWhitelist) of de [AuthServerWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthServerWhitelist) beleidsinstellingen in uw omgeving, zorg ervoor dat u Azure AD-URL toevoegen (`https://autologon.microsoftazuread-sso.com`) naar deze ook.

#### <a name="google-chrome-macos-and-other-non-windows-platforms"></a>Google Chrome (Mac OS en andere niet-Windows-platforms)

Raadpleeg voor Google Chrome op Mac OS- en andere niet-Windows-platforms, [de chroom Project Policy List](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) voor meer informatie over hoe aan lijst met geaccepteerde de URL van de Azure AD voor verificatie geïntegreerd.

Het gebruik van Active Directory-groepsbeleid uitbreidingen van derden in de URL van de Azure AD Firefox en Google Chrome op Mac-gebruikers is buiten het bereik van dit artikel.

#### <a name="known-browser-limitations"></a>Browser bekende beperkingen

Naadloze eenmalige aanmelding werkt niet in de privémodus bladeren in Firefox en Microsoft Edge browsers. Dit ook werkt niet in Internet Explorer als de browser wordt uitgevoerd in de uitgebreide beveiligde modus.

## <a name="step-4-test-the-feature"></a>Stap 4: De functie testen

Als u wilt testen van de functie voor een specifieke gebruiker, zorg ervoor dat de volgende voorwaarden voldaan is:
  - De gebruiker zich aanmeldt op een bedrijfsapparaat.
  - Het apparaat is toegevoegd aan uw Active Directory-domein. Het apparaat _niet_ moet [Azure AD join](../active-directory-azureadjoin-overview.md).
  - Het apparaat heeft een directe verbinding met de domeincontroller (DC), op het bedrijfsnetwerk bekabelde of draadloze of via een RAS-verbinding, zoals een VPN-verbinding.
  - U hebt [bekendgemaakt die de functie](##step-3-roll-out-the-feature) aan deze gebruiker via Groepsbeleid.

Voor het testen van het scenario waarin de gebruiker alleen de gebruikersnaam, maar niet het wachtwoord invoeren:
   - Aanmelden bij `https://myapps.microsoft.com/` in een nieuwe persoonlijke browsersessie.

Als u wilt testen van het scenario waarin de gebruiker beschikt niet over de gebruikersnaam of het wachtwoord invoeren, gebruikt u een van deze stappen: 
   - Aanmelden bij `https://myapps.microsoft.com/contoso.onmicrosoft.com` in een nieuwe persoonlijke browsersessie. Vervang *contoso* met de naam van uw tenant.
   - Aanmelden bij `https://myapps.microsoft.com/contoso.com` in een nieuwe persoonlijke browsersessie. Vervang *contoso.com* met een geverifieerd domein (niet een federatief domein) op uw tenant.

## <a name="step-5-roll-over-keys"></a>Stap 5: Beweeg de muis over sleutels

Azure AD Connect maakt in stap 2, computeraccounts (Azure AD representeren) in alle van de Active Directory-forests waarop u naadloze eenmalige aanmelding hebt ingeschakeld. Zie voor meer informatie, [Azure Active Directory naadloze eenmalige aanmelding: Technische details](how-to-connect-sso-how-it-works.md).

>[!IMPORTANT]
>De Kerberos-ontsleutelingssleutel op een computeraccount, kan als gelekt, worden gebruikt voor het genereren van Kerberos-tickets voor elke gebruiker in de AD-forest. Kwaadwillende actoren kunnen vervolgens Azure AD-aanmeldingen voor verdachte gebruikers imiteren. Het is raadzaam dat u periodiek via deze ontsleutelingssleutels Kerberos - ten minste één keer voor elke 30 dagen terugdraaien.

Zie voor instructies over het meenemen van sleutels, [Azure Active Directory naadloze eenmalige aanmelding: Veelgestelde vragen over](how-to-connect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account). We werken aan een functie in te voeren van geautomatiseerde implementatie via van sleutels.

>[!IMPORTANT]
>U hoeft te doen in deze stap _onmiddellijk_ nadat u de functie hebt ingeschakeld. Beweeg de muis over de Kerberos-sleutels voor ontsleuteling ten minste één keer voor elke 30 dagen.

## <a name="next-steps"></a>Volgende stappen

- [Technische details](how-to-connect-sso-how-it-works.md): Informatie over de werking van de functie voor naadloze eenmalige aanmelding.
- [Veelgestelde vragen over](how-to-connect-sso-faq.md): Vind antwoorden op veelgestelde vragen over naadloze eenmalige aanmelding.
- [Problemen oplossen](tshoot-connect-sso.md): Informatie over het oplossen van veelvoorkomende problemen met de functie voor naadloze eenmalige aanmelding.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): De Active Directory-Forum van Azure gebruiken om nieuwe functieaanvragen.

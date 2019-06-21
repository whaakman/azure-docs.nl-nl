---
title: Probleem opgetreden bij het aanmelden bij een Azure AD-galerie app geconfigureerd voor SSO-wachtwoord | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van problemen met een toepassing met Azure AD-galerie die geconfigureerd voor eenmalige aanmelding wachtwoord.
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 742df882fb64e09ff63ef2eceb5514ca070dc227
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190326"
---
# <a name="sign-in-problems-with-an-azure-ad-gallery-app-configured-for-sso"></a>Problemen met aanmelden met een Azure AD-galerie app geconfigureerd voor eenmalige aanmelding

Toegangsvenster is een portal op Internet. Hiermee kunnen gebruikers die Azure Active Directory (Azure AD) werk of school-accounts voor toegang tot cloud-gebaseerde apps die ze machtigingen hebben. Gebruikers met Azure AD-edities kunnen u ook groepsbeheer met Self-service en mogelijkheden van app-beheer via het toegangsvenster gebruiken.

Toegangsvenster staat los van de Azure-portal. Gebruikers hoeven niet een Azure-abonnement Toegangsvenster gebruiken.

Als u wilt gebruiken op basis van wachtwoorden eenmalige aanmelding (SSO) in het toegangsvenster, moet de extensie Toegangsvenster worden geïnstalleerd in uw browser. De extensie wordt automatisch gedownload wanneer u een app die geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

## <a name="browser-requirements-for-access-panel"></a>Browservereisten voor het toegangsvenster

Toegangsvenster vereist een browser die ondersteuning biedt voor JavaScript en CSS is ingeschakeld.

De volgende browsers bieden ondersteuning voor eenmalige aanmelding op basis van wachtwoorden:

- Internet Explorer 8, 9, 10 en 11 op Windows 7 of hoger

- Chrome op Windows 7 of hoger of op Mac OS X of hoger

- Firefox 26,0 of hoger op Windows XP SP2 of hoger of op Mac OS X 10.6 of hoger

>[!NOTE]
>De eenmalige aanmelding op basis van wachtwoorden extensie beschikbaar voor Microsoft Edge in Windows 10 wanneer ondersteuning voor browser-extensies is toegevoegd aan de Microsoft Edge.

## <a name="install-the-access-panel-browser-extension"></a>Installeer de extensie toegang deelvenster Browser

Volg deze stappen:

1. Open [Toegangsvenster](https://myapps.microsoft.com) in een ondersteunde browser en meld u als een gebruiker in Azure AD.

2. Selecteer een app wachtwoord SSO is ingeschakeld in Toegangsvenster.

3. Wanneer u wordt gevraagd, selecteert u **nu installeren**.

4. U wordt omgeleid naar een downloadkoppeling op basis van uw browser. Selecteer **toevoegen** voor het installeren van de uitbreiding van de browser.

5. Als u wordt gevraagd, selecteert u **inschakelen** of **toestaan**.

6. Na de installatie opnieuw opstarten van uw browser.

7.  Meld u aan bij het toegangsvenster voor en zien als u uw wachtwoord-SSO-functionaliteit geschikte apps kunt starten.

U kunt ook rechtstreeks de extensies voor Chrome en Firefox downloaden via deze koppelingen:

-   [Chrome Toegangsvenster-extensie](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Toegangsvenster Firefox-extensie](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Instellen van een groepsbeleid voor Internet Explorer

U kunt een groepsbeleid waarmee u de extensie van het toegangsvenster voor Internet Explorer op afstand installeren op uw gebruikers virtuele machines kunt instellen.

Dit zijn de vereisten:

-   [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx) moet worden ingesteld, en uw gebruikers machines moeten worden toegevoegd aan uw domein.

-   U hebt 'Instellingen bewerken'-machtigingen voor het bewerken van het groepsbeleidsobject (GPO). Standaard hebben leden van de volgende beveiligingsgroepen deze machtiging: Domeinadministrators, Ondernemingsadministrators en beveiligingsgroep Maker Eigenaar Groepsbeleid. [Meer informatie](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Zie voor het configureren van het Groepsbeleid en implementeren voor gebruikers, [over het implementeren van de extensie van het toegangsvenster voor Internet Explorer met behulp van Groepsbeleid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy).

## <a name="troubleshoot-access-panel-in-internet-explorer"></a>Toegangsvenster in Internet Explorer oplossen

Voor toegang tot een hulpprogramma voor diagnostische gegevens en instructies voor het configureren van de extensie, Zie [oplossen met de extensie van het toegangsvenster voor Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting).

## <a name="configure-password-sso-for-an-azure-ad-gallery-app"></a>Configureren van SSO-wachtwoord voor een app van Azure AD-galerie

Voor het configureren van een app uit de galerie van Azure AD, moet u deze dingen doen:

-   De app uit de galerie van Azure AD toevoegen
-   [De app configureren voor eenmalige aanmelding wachtwoord](#configure-the-app-for-password-sso)
-   [Gebruikers toewijzen aan de app.](#assign-users-to-the-app)

### <a name="add-the-app-from-the-azure-ad-gallery"></a>De app uit de galerie van Azure AD toevoegen

Volg deze stappen:

1. Open de [Azure-portal](https://portal.azure.com) en meld u aan als een globale beheerder of co-beheerder.

2. Selecteer **alle services** aan de bovenkant van het navigatiedeelvenster aan de linkerkant om te openen van de Azure AD-extensie.

3. Type **Azure Active Directory** in het zoekvak van filter en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **bedrijfstoepassingen** in het navigatiedeelvenster van de Azure AD.

5. Selecteer **toevoegen** in de rechterbovenhoek van de **bedrijfstoepassingen** deelvenster.

6. In de **toevoegen vanuit de galerie** sectie, typt u de naam van de app in de **Voer een naam** vak.

7. Selecteer de app die u wilt configureren voor eenmalige aanmelding.

8. *Optioneel:* Voordat u de app toevoegt, kunt u de naam ervan in de **naam** vak.

9. Klik op **toevoegen** om toe te voegen van de app.

   Na een korte vertraging zult u kunnen zien van deelvenster van de configuratie van de app.

### <a name="configure-the-app-for-password-sso"></a>Configureren van de app voor SSO-wachtwoord

Volg deze stappen:

1. Open de [Azure-portal](https://portal.azure.com/) en meld u aan als een globale beheerder of co-beheerder.

2. Selecteer **alle services** aan de bovenkant van het navigatiedeelvenster aan de linkerkant om te openen van de Azure AD-extensie.

3. Type **Azure Active Directory** in het zoekvak van filter en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **bedrijfstoepassingen** in het navigatievenster van de Azure AD.

5. Selecteer **alle toepassingen** om een lijst van uw apps weer te geven.

   > [!NOTE]
   > Als u de app die u wilt niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen**. Stel de **weergeven** optie "Alle toepassingen."

6. Selecteer de app die u wilt configureren voor eenmalige aanmelding.

7. Nadat de app wordt geladen, selecteert u **eenmalige aanmelding** in het deelvenster aan de linkerkant van de app.

8. Selecteer **wachtwoord gebaseerde aanmelding** modus.

9. Gebruikers toewijzen aan de app.

10. U kunt ook referenties opgeven voor gebruikers. (Anders gebruikers wordt gevraagd referenties bij het opstarten van de app in te voeren.) U doet dit door de rijen van de gebruikers te selecteren. Selecteer vervolgens **updatereferenties** en voer de gebruikersnamen en wachtwoorden.

### <a name="assign-users-to-the-app"></a>Gebruikers aan de app toewijzen

Als u wilt toewijzen gebruikers rechtstreeks aan een app, de volgende stappen uit:

1. Open de [Azure-portal](https://portal.azure.com/) en meld u aan als een globale beheerder.

2. Selecteer **alle services** in de pijn navigatie aan de linkerkant om te openen van de Azure AD-extensie.

3. Type **Azure Active Directory** in het zoekvak van filter en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **bedrijfstoepassingen** in het navigatievenster van de Azure AD.

5. Selecteer **alle toepassingen** om een lijst van uw toepassingen weer te geven.

   > [!NOTE]
   > Als u de app die u wilt niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen**. Stel de **weergeven** optie "Alle toepassingen."

6. Selecteer de app die u wilt toewijzen van een gebruiker in de lijst.

7. Nadat de toepassing wordt geladen, selecteert u **gebruikers en groepen** in het navigatievenster van de app aan de linkerkant.

8. Selecteer **toevoegen** aan de bovenkant van de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9. Selecteer **gebruikers en groepen** in de **toevoegen toewijzing** deelvenster.

10. In de **zoeken op naam of e-mailadres** vak, typ de volledige naam of e-mailadres van de gebruiker die u wilt toewijzen.

11. Beweeg de muisaanwijzer over de gebruiker in de lijst. Schakel het selectievakje in naast de profielfoto van de gebruiker of het logo om toe te voegen die gebruiker de **geselecteerde** lijst.

12. *Optioneel:* Typ een andere naam of e-mailadres in om toe te voegen een andere gebruiker, de **zoeken op naam of e-mailadres** vak en selecteer vervolgens het selectievakje in om toe te voegen die gebruiker de **geselecteerde** lijst.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op **Selecteer** aan de lijst met gebruikers en groepen die zijn toegewezen aan de app.

14. *Optioneel:* Klik op **rol selecteren** in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Selecteer **toewijzen** naar de app toewijzen aan de geselecteerde gebruikers.

    Na een korte vertraging worden de gebruikers die apps openen in Toegangsvenster.

## <a name="request-support"></a>Ondersteuning aanvragen 
Als er een foutbericht weergegeven wanneer u een SSO instellen en gebruikers toewijst, moet u een ondersteuningsticket openen. Als veel van de volgende informatie mogelijk opnemen:

-   Correlatie-ID van fout
-   UPN (e-mailadres van gebruiker)
-   TenantID
-   Browsertype
-   Tijdzone en de tijd/tijdsbestek als de fout is opgetreden
-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)

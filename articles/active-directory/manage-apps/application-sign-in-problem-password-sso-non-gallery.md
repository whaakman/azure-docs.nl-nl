---
title: Problemen met aanmelden bij een toepassing in Azure AD-galerie geconfigureerd wachtwoord voor eenmalige aanmelding | Microsoft Docs
description: Probleemgebieden die richtlijnen bieden voor het oplossen van problemen met aanmelden bij Azure AD-galerietoepassingen die zijn geconfigureerd voor eenmalige aanmelding wachtwoord wordt beschreven
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: ef981686143299b41960e81cf827459493c868f6
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356838"
---
# <a name="problems-signing-in-to-an-azure-ad-gallery-application-configured-for-password-single-sign-on"></a>Problemen met aanmelden bij een Azure AD-galerie-toepassing die is geconfigureerd voor eenmalige aanmelding wachtwoord

Het toegangsvenster is een webportal waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) om te bekijken en gebruiken waartoe cloud-gebaseerde toepassingen die Azure AD-beheerder deze toegang heeft gegeven. Een gebruiker met Azure AD-edities kunt u ook groepsbeheer met Self-service en mogelijkheden voor appbeheer via het toegangsvenster gebruiken. Het toegangsvenster is gescheiden van de Azure-portal en hoeft geen gebruikers hebben een Azure-abonnement.

Als u wilt gebruiken op basis van wachtwoorden eenmalige aanmelding (SSO) in het toegangsvenster, moet de extensie Toegangsvenster worden geïnstalleerd in de browser van de gebruiker. Deze extensie wordt automatisch gedownload wanneer een gebruiker selecteert een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Aan de wensen van Browservereisten voor het toegangsvenster

Het toegangsvenster is vereist voor een browser die ondersteuning biedt voor JavaScript en CSS is ingeschakeld. Als u wilt gebruiken op basis van wachtwoorden eenmalige aanmelding (SSO) in het toegangsvenster, moet de extensie Toegangsvenster worden geïnstalleerd in de browser van de gebruiker. Deze extensie wordt automatisch gedownload wanneer een gebruiker selecteert een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

Voor eenmalige aanmelding op basis van wachtwoorden kunnen van de eindgebruiker browsers zijn:

-   Internet Explorer 8, 9, 10, 11--op Windows 7 of hoger

-   Chrome--Op Windows 7 of hoger, en op Mac OS X of hoger

-   Firefox 26,0 of later--op Windows XP SP2 of hoger, en Mac OS X 10,6 of hoger

>[!NOTE]
>De extensie eenmalige aanmelding op basis van wachtwoorden wordt beschikbaar voor Microsoft Edge in Windows 10 als browser-extensies worden ondersteund voor Microsoft Edge.
>
>

## <a name="how-to-install-the-access-panel-browser-extension"></a>Het installeren van de extensie toegang deelvenster Browser

Voor het installeren van de Browseruitbreiding van toegang deelvenster de volgende stappen uit te voeren:

1.  Open de [Toegangsvenster](https://myapps.microsoft.com) in een van de ondersteunde browsers en meld u aan als een **gebruiker** in uw Azure AD.

2.  Klik op een **wachtwoord-SSO-toepassing** in het toegangsvenster.

3.  Selecteer in de vraag of de software te installeren, **nu installeren**.

4.  Op basis van uw browser u omgeleid naar de downloadkoppeling. **Voeg** de uitbreiding van uw browser.

5.  Als hierom wordt gevraagd, selecteert u op een **inschakelen** of **toestaan** de extensie.

6.  Nadat deze is geïnstalleerd, **opnieuw** uw browsersessie.

7.  Meld u aan in het toegangsvenster en zien als u kunt **starten** uw wachtwoord-SSO-toepassingen

U kunt ook de extensie voor Chrome en Firefox downloaden via de rechtstreekse koppelingen hieronder:

-   [Chrome-extensie voor toegang deelvenster](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Uitbreiding van het Configuratiescherm Firefox-toegang](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Instellen van een groepsbeleid voor Internet Explorer

U kunt een groepsbeleid waarmee u kunt de extensie Toegangsvenster voor Internet Explorer op afstand installeren op computers van uw gebruikers instellen.

De vereisten zijn:

-   U hebt ingesteld [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), en u hebt uw gebruikers machines toegevoegd aan uw domein.

-   U moet de machtiging 'Instellingen bewerken' voor het bewerken van het groepsbeleidsobject (GPO) hebben. Standaard hebben leden van de volgende beveiligingsgroepen deze machtiging: Domeinadministrators, Ondernemingsadministrators en beveiligingsgroep Maker Eigenaar Groepsbeleid. [Meer informatie](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Volg de zelfstudie [over het implementeren van het Configuratiescherm-extensie voor toegang voor Internet Explorer met behulp van Groepsbeleid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) voor stapsgewijze instructies over het configureren van het Groepsbeleid en implementeren voor gebruikers.

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a>Oplossen van het toegangsvenster in Internet Explorer

Ga als volgt de [oplossen met de extensie van het Configuratiescherm toegang voor Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting) handleiding voor door de toegang tot een hulpprogramma voor diagnostische gegevens en stapsgewijze instructies over het configureren van de extensie voor IE.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor een toepassing buiten de galerie

Een toepassing uit de Azure AD-galerie die u wilt configureren:

-   [Een toepassing buiten de galerie toevoegen](#add-a-non-gallery-application)

-   [De toepassing configureren voor eenmalige aanmelding wachtwoord](#configure-the-application-for-password-single-sign-on)

-   [Gebruikers toewijzen aan de toepassing](#assign-users-to-the-application)

### <a name="add-a-non-gallery-application"></a>Een toepassing buiten de galerie toevoegen

Als u wilt een toepassing hebt toegevoegd vanuit de Azure AD-galerie, de volgende stappen uit te voeren:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  Klik op **niet in de galerij-toepassing.**

7.  Voer de naam van uw toepassing in de **naam** tekstvak. Selecteer **toevoegen.**

Na een korte periode, kunt u zich aan het deelvenster van de configuratie van de toepassing zien.

### <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing configureren voor eenmalige aanmelding wachtwoord

Voor het configureren van eenmalige aanmelding voor een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Selecteer de modus **wachtwoord gebaseerde aanmelding.**

9.  Voer de **aanmeldings-URL**. Dit is de URL waar gebruikers hun gebruikersnaam en wachtwoord aanmelden bij invoeren. Controleer of dat de aanmelding velden zijn zichtbaar in de URL.

10. Gebruikers toewijzen aan de toepassing.

11. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers selecteren en te klikken op **updatereferenties** en de gebruikersnaam en het wachtwoord namens de gebruikers in te voeren. Anders wordt gebruikers gevraagd om in te voeren van de referenties zich bij het starten.

### <a name="assign-users-to-the-application"></a>Gebruikers toewijzen aan een toepassing

Als u wilt toewijzen een of meer gebruikers rechtstreeks aan een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** in het navigatiemenu aan de van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9.  Klik op de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen in het toegangsvenster te starten.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Als deze stappen voor probleemoplossing niet oplossen door het probleem doet

Open een ondersteuningsticket met de volgende informatie, indien beschikbaar:

-   Correlatie-ID van fout

-   UPN (e-mailadres van gebruiker)

-   TenantID

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens de fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)


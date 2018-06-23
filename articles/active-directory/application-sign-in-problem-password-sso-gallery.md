---
title: Problemen met aanmelden bij een galerie van Azure AD-toepassing die is geconfigureerd voor wachtwoordherstel eenmalige aanmelding | Microsoft Docs
description: Het oplossen van problemen met de galerie van Azure AD-toepassing die is geconfigureerd voor eenmalige aanmelding wachtwoord
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: dbb25ee959193ebc8d1c15ad76555e8d7dc9fe4b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36331493"
---
# <a name="problems-signing-in-to-an-azure-ad-gallery-application-configured-for-password-single-sign-on"></a>Problemen met aanmelden bij een galerie van Azure AD-toepassing die is geconfigureerd voor eenmalige aanmelding wachtwoord

Het Toegangspaneel is een portal op Internet waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) voor weergave in en start cloud-gebaseerde toepassingen die de Azure AD-beheerder heeft deze toegang verleend tot. Een gebruiker met Azure AD-edities kunt ook gebruiken voor groepsbeheer met Self-service en app-mogelijkheden voor beheer via het toegangsvenster. Het Toegangspaneel is gescheiden van de Azure-portal en vereist geen gebruikers een Azure-abonnement hebben.

Als u wilt gebruiken op basis van wachtwoorden eenmalige aanmelding (SSO) in het deelvenster toegang, moet de extensie Toegangspaneel worden geïnstalleerd in de browser van de gebruiker. Deze extensie wordt automatisch gedownload wanneer een gebruiker selecteert een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Vergadering Browservereisten voor het toegangsvenster

Het toegangsvenster vereist een browser die JavaScript ondersteunt en CSS is ingeschakeld. Als u wilt gebruiken op basis van wachtwoorden eenmalige aanmelding (SSO) in het deelvenster toegang, moet de extensie Toegangspaneel worden geïnstalleerd in de browser van de gebruiker. Deze extensie wordt automatisch gedownload wanneer een gebruiker selecteert een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

Voor eenmalige aanmelding op basis van wachtwoorden kunnen van de eindgebruiker browsers zijn:

-   Internet Explorer 8, 9, 10, 11 - in Windows 7 of hoger

-   Chrome - in Windows 7 of hoger, en op Mac OS X of hoger

-   Firefox 26,0 of later - op Windows XP SP2 of hoger, en op Mac OS X 10,6 of hoger

>[!NOTE]
>De extensie eenmalige aanmelding op basis van wachtwoorden komen beschikbaar voor de rand in de Windows 10 als browseruitbreidingen worden ondersteund voor de rand.
>
>

## <a name="how-to-install-the-access-panel-browser-extension"></a>Het installeren van de Browseruitbreiding toegang Configuratiescherm

Volg deze stappen voor het installeren van de Browseruitbreiding toegang Configuratiescherm:

1.  Open de [Toegangspaneel](https://myapps.microsoft.com) in een van de ondersteunde browsers en meld u aan als een **gebruiker** in uw Azure AD.

2.  Klik op een **wachtwoord SSO toepassing** in het deelvenster toegang.

3.  Selecteer in het venster met de vraag om de software te installeren, **nu installeren**.

4.  Op basis van uw browser u omgeleid naar de downloadkoppeling. **Voeg** de uitbreiding van uw browser.

5.  Als uw browser wordt gevraagd, selecteert u op een **inschakelen** of **toestaan** de extensie.

6.  Nadat deze is geïnstalleerd, **opnieuw** uw browsersessie.

7.  Meld u aan in het deelvenster toegang en zien als u kunt **starten** uw wachtwoord SSO-toepassingen

U kunt ook de uitbreiding voor Chrome en Firefox downloaden uit de onderstaande directe koppelingen:

-   [Uitbreiding van chrome toegang Configuratiescherm](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Uitbreiding van het Configuratiescherm Firefox toegang](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Instellen van een groepsbeleid voor Internet Explorer

U kunt een groepsbeleid waarmee u op afstand installeren van de extensie Toegangspaneel voor Internet Explorer op computers van uw gebruikers instellen.

De vereisten zijn:

-   U hebt ingesteld [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), en u hebt uw gebruikers machines toegevoegd aan uw domein.

-   U moet de machtiging 'Instellingen bewerken' voor het bewerken van het groepsbeleidsobject (GPO) hebben. Standaard hebben leden van de volgende beveiligingsgroepen deze machtiging: Domeinadministrators, Ondernemingsadministrators en Maker Eigenaar Groepsbeleid. [Meer informatie](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Volg de zelfstudie [het implementeren van de uitbreiding van het Configuratiescherm toegang voor Internet Explorer met behulp van Groepsbeleid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) voor stapsgewijze instructies voor het configureren van het Groepsbeleid en het implementeren voor gebruikers.

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a>Problemen met het toegangsvenster in Internet Explorer

Ga als volgt de [oplossen met de extensie van het Configuratiescherm toegang voor Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting) geleid voor toegang tot een diagnostische hulpprogramma's en stapsgewijze instructies over het configureren van de extensie voor IE.

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Het wachtwoord eenmalige aanmelding voor een toepassing van de galerie van Azure AD configureren

Een toepassing uit de galerie van Azure AD te hoeft configureren:

-   [Een toepassing toevoegen uit de galerie van Azure AD](#_Add_an_application)

-   [De toepassing voor eenmalige aanmelding wachtwoord configureren](#configure-the-application-for-password-single-sign-on)

-   [Gebruikers toewijzen aan de toepassing](#assign-users-to-the-application)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen uit de galerie van Azure AD

Als u wilt een toepassing uit de galerie van Azure AD toevoegt, de volgende stappen uit:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op de **toevoegen** middenonder in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  In de **Voer een naam** textbox uit de **toevoegen uit de galerie** sectie, typ de naam van de toepassing.

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toevoegt, kunt u de naam van de **naam** textbox.

9.  Klik op **toevoegen** knop, de toepassing toevoegen.

Na een korte periode is het mogelijk om te zien deelvenster van de configuratie van de toepassing.

### <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing voor eenmalige aanmelding wachtwoord configureren

Volg deze stappen voor het configureren van eenmalige aanmelding voor een toepassing:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in de toepassing linkermenubalk menu.

8.  Selecteer de modus **op basis van wachtwoorden eenmalige aanmelding.**

9.  [Gebruikers toewijzen aan de toepassing](#_How_to_assign).

10. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers te selecteren en te klikken op **updatereferenties** en de gebruikersnaam en wachtwoord in te voeren namens de gebruikers. Anders wordt gebruikers gevraagd de referenties zich bij het starten in te voeren.

### <a name="assign-users-to-the-application"></a>Gebruikers toewijzen aan de toepassing

Als u wilt toewijzen een of meer gebruikers rechtstreeks naar een toepassing, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** in de toepassing linkermenubalk menu.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst openen de **toevoegen toewijzing** deelvenster.

9.  Klik op de **gebruikers en groepen** selector van de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo voor uw gebruiker toevoegen aan de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met het selecteren van gebruikers, klikt u op de **Selecteer** om toe te voegen aan de lijst met gebruikers en groepen kunnen worden toegewezen aan de toepassing.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** klikken en selecteer een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop de toepassing toewijzen aan de geselecteerde gebruikers.

Na een korte periode de geselecteerde gebruikers mogelijk om deze toepassingen in het deelvenster toegang te starten.

## <a name="if-these-troubleshoot-steps-dont-resolve-the-issue"></a>Als deze fout opsporen oplossen stappen het probleem niet 
een ondersteuningsticket opent met de volgende informatie, indien beschikbaar:

-   Correlatie fout-ID

-   UPN (e-mailadres van de gebruiker)

-   TenantID

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)

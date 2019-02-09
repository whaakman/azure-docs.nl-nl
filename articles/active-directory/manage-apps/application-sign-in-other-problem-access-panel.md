---
title: Problemen met aanmelden bij een toepassing vanuit het toegangsvenster | Microsoft Docs
description: Het oplossen van problemen met toegang tot een toepassing vanaf de Microsoft Azure AD-Toegangsvenster op myapps.microsoft.com
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: acb1b5749d617bc4608a2165420e709c7412a8d2
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55965185"
---
# <a name="problems-signing-in-to-an-application-from-the-access-panel"></a>Problemen met aanmelden bij een toepassing vanuit het toegangsvenster

Het toegangsvenster is een webportal waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) om te bekijken en starten van cloud-gebaseerde toepassingen die Azure AD-beheerder heeft deze toegang verleend aan. 

Deze toepassingen zijn namens de gebruiker in de Azure AD-portal geconfigureerd. De toepassing moet worden geconfigureerd en toegewezen aan de gebruiker of een groep die de gebruiker een lid van is om te zien van de toepassing in het toegangsvenster.

Het type van een gebruiker wordt mogelijk weergegeven apps kunnen worden onderverdeeld in de volgende categorieën:

-   Office 365-toepassingen

-   Microsoft en derden door toepassingen die zijn geconfigureerd met op basis van een federatieve eenmalige aanmelding

-   Wachtwoord gebaseerde SSO-toepassingen

-   Toepassingen met bestaande oplossingen voor eenmalige aanmelding

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren

-   Zorg ervoor dat uw met een **browser** die voldoet aan de minimale vereisten voor het toegangsvenster.

-   Zorg ervoor dat de browser van de gebruiker de URL van de toepassing is toegevoegd de **vertrouwde sites**.

-   Controleer of de toepassing is **geconfigureerd** correct.

-   Zorg ervoor dat het gebruikersaccount is **ingeschakeld** voor aanmeldingen.

-   Zorg ervoor dat het gebruikersaccount is **niet vergrendeld.**

-   Zorg ervoor dat de gebruiker **wachtwoord niet is verlopen of is vergeten.**

-   Zorg ervoor dat **multi-Factor Authentication** gebruikerstoegang niet blokkeert.

-   Zorg ervoor dat een **beleid voor voorwaardelijke toegang** of **Identity Protection** beleid voor de gebruikerstoegang niet blokkeert.

-   Zorg ervoor dat een gebruiker **contactgegevens voor verificatie** is up-to-date om toe te staan van multi-factor Authentication of voorwaardelijk beleid moeten worden afgedwongen.

-   Zorg ervoor dat ook worden geprobeerd wissen van cookies van uw browser en probeer het opnieuw aanmelden.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Aan de wensen van Browservereisten voor het toegangsvenster

Het toegangsvenster is vereist voor een browser die ondersteuning biedt voor JavaScript en CSS is ingeschakeld. Als u wilt gebruiken op basis van wachtwoorden eenmalige aanmelding (SSO) in het toegangsvenster, moet de extensie Toegangsvenster worden geïnstalleerd in de browser van de gebruiker. Deze extensie wordt automatisch gedownload wanneer een gebruiker selecteert een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

Voor eenmalige aanmelding op basis van wachtwoorden kunnen van de eindgebruiker browsers zijn:

-   Internet Explorer 8, 9, 10, 11--op Windows 7 of hoger

-   Microsoft Edge op Windows 10 Anniversary Edition of hoger

-   Chrome--Op Windows 7 of hoger, en op Mac OS X of hoger

-   Firefox 26,0 of later--op Windows XP SP2 of hoger, en Mac OS X 10,6 of hoger

## <a name="how-to-install-the-access-panel-browser-extension"></a>Het installeren van de extensie toegang deelvenster Browser

Voor het installeren van de Browseruitbreiding van toegang deelvenster de volgende stappen uit te voeren:

1.  Open de [Toegangsvenster](https://myapps.microsoft.com) in een van de ondersteunde browsers en meld u aan als een **gebruiker** in uw Azure AD.

2.  Klik op een **wachtwoord-SSO-toepassing** in het toegangsvenster.

3.  Selecteer in de vraag of de software te installeren, **nu installeren**.

4.  Op basis van uw browser die u worden doorgestuurd naar de downloadkoppeling. **Voeg** de uitbreiding van uw browser.

5.  Als hierom wordt gevraagd, selecteert u op een **inschakelen** of **toestaan** de extensie.

6.  Nadat deze is geïnstalleerd, **opnieuw** uw browsersessie.

7.  Meld u aan in het toegangsvenster en zien als u kunt **starten** uw wachtwoord-SSO-toepassingen

U kunt ook de extensie voor Chrome en Microsoft Edge downloaden via de rechtstreekse koppelingen hieronder:

-   [Chrome-extensie voor toegang deelvenster](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Microsoft Edge-extensie voor toegang deelvenster](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Het configureren van eenmalige aanmelding in voor een toepassing in Azure AD-galerie

Alle toepassingen in de Azure AD-galerie ingeschakeld met Enterprise Single Sign-On-functionaliteit is een stapsgewijze zelfstudie beschikbaar. U hebt toegang tot de [lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) voor een stapsgewijze instructies voor details.

Een toepassing uit de Azure AD-galerie die u wilt configureren:

-   Een toepassing toevoegen via de Azure AD-galerie

-   [Waarden van de metagegevens van de toepassing in Azure AD (aanmeldings-URL-id en antwoord-URL) configureren](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [De metagegevens van de Azure AD en het certificaat ophalen](#download-the-azure-ad-metadata-or-certificate)

-   [Waarden voor metagegevens van Azure AD configureren in de toepassing (aanmelding URL, uitgever, afmeldings-URL en certificaat)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   Gebruikers toewijzen aan een toepassing

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen via de Azure AD-galerie

Als u wilt een toepassing hebt toegevoegd vanuit de Azure AD-galerie, de volgende stappen uit te voeren:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  In de **Voer een naam** tekstvak van de **toevoegen vanuit de galerie** sectie, typt u de naam van de toepassing.

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toe te voegen, kunt u de naam van de **naam** tekstvak.

9.  Klik op **toevoegen** knop om toe te voegen van de toepassing.

Na een korte periode ziet u deelvenster van de configuratie van de toepassing.

### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Configureren van eenmalige aanmelding voor een toepassing uit de Azure AD-galerie

Voor het configureren van eenmalige aanmelding voor een toepassing, de volgende stappen uit te voeren:

1.  <span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Selecteer **SAML gebaseerde aanmelding** uit de **modus** vervolgkeuzelijst.

9.  Voer de vereiste waarden in **domein en URL's.** U krijgt deze waarden van de leverancier van de toepassing.

   1. De aanmeldings-URL is voor het configureren van de toepassing als Serviceprovider geïnitieerde eenmalige aanmelding, een vereiste waarde. De id is voor sommige toepassingen, ook een vereiste waarde.

   2. De antwoord-URL is voor het configureren van de toepassing als id-provider geïnitieerde eenmalige aanmelding, een vereiste waarde. De id is voor sommige toepassingen, ook een vereiste waarde.

10. **Optioneel:** klikt u op **geavanceerde URL-instellingen weergeven** als u wilt zien van de waarden niet vereist.

11. In de **gebruikerskenmerken**, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst.

12. **Optioneel:** klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   2. Klik op **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

13. Klik op **configureren &lt;toepassingsnaam&gt;**  toegang documentatie over het configureren van eenmalige aanmelding in de toepassing. U hebt ook de metagegevens-URL's en dat is vereist voor het instellen van eenmalige aanmelding met de toepassing.

14. Klik op **opslaan** aan de configuratie op te slaan.

15. Gebruikers toewijzen aan de toepassing.

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen

Als u wilt de gebruikers-ID selecteren of toevoegen van gebruikerskenmerken, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing niet ziet u hier weergegeven, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar  **Alle toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Onder de **gebruikerskenmerken** sectie, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst. De geselecteerde optie moet overeenkomen met de verwachte waarde in de toepassing om de gebruiker te verifiëren.

    >[!NOTE]
    >Azure AD selecteren de indeling voor het kenmerk van NameID (gebruikers-id) op basis van het geselecteerde of de indeling die is aangevraagd door de toepassing in het SAML-AuthRequest. Ga naar het artikel voor meer informatie [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy.
    >
    >

9.  Als u wilt toevoegen gebruikerskenmerken, klikt u op **weergeven en bewerk alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   2. Klik op **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

### <a name="download-the-azure-ad-metadata-or-certificate"></a>De metagegevens van de Azure AD of het certificaat downloaden

De metagegevens van de toepassing of het certificaat downloaden van Azure AD, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Ga naar **SAML-handtekeningcertificaat** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding moet, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

    Azure AD biedt geen een URL waarmee u de metagegevens ophalen. De metagegevens kan alleen worden opgehaald als een XML-bestand.

## <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Het configureren van eenmalige aanmelding in voor een toepassing buiten de galerie

Voor het configureren van een toepassing buiten de galerie, moet u beschikken over Azure AD premium en de toepassing ondersteunt SAML 2.0. Voor meer informatie over de versies van Azure AD, gaat u naar [prijzen voor Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Waarden van de metagegevens van de toepassing in Azure AD (aanmeldings-URL-id en antwoord-URL) configureren](#configuring-single-sign-on)

-   [Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [De metagegevens van de Azure AD en het certificaat ophalen](#download-the-azure-ad-metadata-or-certificate)

-   [Waarden voor metagegevens van Azure AD configureren in de toepassing (aanmelding URL, uitgever, afmeldings-URL en certificaat)](#configuring-single-sign-on)

### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Waarden van de metagegevens van de toepassing in Azure AD (aanmeldings-URL-id en antwoord-URL) configureren

Voor het configureren van eenmalige aanmelding voor een toepassing die zich niet in de Azure AD-galerie, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  Klik op **niet in de galerij toepassing** in de **toevoegen aan uw eigen app** sectie.

7.  Voer de naam van de toepassing in de **naam** tekstvak.

8.  Klik op **toevoegen** knop om toe te voegen van de toepassing.

9.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

10. Selecteer **SAML gebaseerde aanmelding** in de **modus** vervolgkeuzelijst

11. Voer de vereiste waarden in **domein en URL's.** U krijgt deze waarden van de leverancier van de toepassing.

  1. Voer de antwoord-URL en de id voor het configureren van de toepassing als id-provider geïnitieerde eenmalige aanmelding.

  2. **Optioneel:** De aanmeldings-URL is voor het configureren van de toepassing als Serviceprovider geïnitieerde eenmalige aanmelding, een vereiste waarde.

12. In de **gebruikerskenmerken**, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst.

13. **Optioneel:** klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   2. Klik op **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

14. Klik op **configureren &lt;toepassingsnaam&gt;**  toegang documentatie over het configureren van eenmalige aanmelding in de toepassing. Bovendien hebt u Azure AD-URL's en dat is vereist voor de toepassing.

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen

Als u wilt de gebruikers-ID selecteren of toevoegen van gebruikerskenmerken, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Onder de **gebruikerskenmerken** sectie, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst. De geselecteerde optie moet overeenkomen met de verwachte waarde in de toepassing om de gebruiker te verifiëren.

   >[!NOTE]
   >Azure AD selecteren de indeling voor het kenmerk van NameID (gebruikers-id) op basis van het geselecteerde of de indeling die is aangevraagd door de toepassing in het SAML-AuthRequest. Ga naar het artikel voor meer informatie [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy.
   >
   >

9.  Als u wilt toevoegen gebruikerskenmerken, klikt u op **weergeven en bewerk alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   2 Klik **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

### <a name="download-the-azure-ad-metadata-or-certificate"></a>De metagegevens van de Azure AD of het certificaat downloaden

De metagegevens van de toepassing of het certificaat downloaden van Azure AD, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Ga naar **SAML-handtekeningcertificaat** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding moet, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

    Azure AD biedt geen een URL waarmee u de metagegevens ophalen. De metagegevens kan alleen worden opgehaald als een XML-bestand.

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor een toepassing in Azure AD-galerie

Een toepassing uit de Azure AD-galerie die u wilt configureren:

-   Een toepassing toevoegen via de Azure AD-galerie

-   De toepassing configureren voor eenmalige aanmelding wachtwoord

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen via de Azure AD-galerie

Als u wilt een toepassing hebt toegevoegd vanuit de Azure AD-galerie, de volgende stappen uit te voeren:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  In de **Voer een naam** tekstvak van de **toevoegen vanuit de galerie** sectie, typt u de naam van de toepassing

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding

8.  Voordat u de toepassing toe te voegen, kunt u de naam van de **naam** tekstvak.

9.  Klik op **toevoegen** knop om toe te voegen van de toepassing.

Na een korte periode ziet u deelvenster van de configuratie van de toepassing.

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

9.  Gebruikers toewijzen aan de toepassing.

10. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers selecteren en te klikken op **updatereferenties** en de gebruikersnaam en het wachtwoord namens de gebruikers in te voeren. Anders wordt gebruikers gevraagd om in te voeren van de referenties zich bij het starten.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor een toepassing buiten de galerie

Een toepassing uit de Azure AD-galerie die u wilt configureren:

-   [Een toepassing buiten de galerie toevoegen](#add-a-non-gallery-application)

-   [De toepassing configureren voor eenmalige aanmelding wachtwoord](#configure-the-application-for-password-single-sign-on)

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

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Selecteer de modus **wachtwoord gebaseerde aanmelding.**

9.  Voer de **aanmeldings-URL**. Dit is de URL waar gebruikers hun gebruikersnaam en wachtwoord aan te melden bij invoeren. Zorg ervoor dat de aanmelding-velden worden weergegeven op de URL.

10. Gebruikers toewijzen aan de toepassing.

11. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers selecteren en te klikken op **updatereferenties** en de gebruikersnaam en het wachtwoord namens de gebruikers in te voeren. Anders wordt gebruikers gevraagd om in te voeren van de referenties zich bij het starten.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Een gebruiker aan een toepassing rechtstreeks toewijzen

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

12. **Optioneel:** Als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** het zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

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


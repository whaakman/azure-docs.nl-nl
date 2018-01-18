---
title: Selfservice voor wachtwoordherstel aanpassing - Azure Active Directory
description: Aanpassingsopties voor Azure AD zelf uw wachtwoord opnieuw instellen
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: a5b4056f3198e998fa7fb127fd2c4bcda90e4bbb
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>De Azure AD-functionaliteit voor selfservice voor wachtwoordherstel aanpassen

IT-professionals die u wilt implementeren selfservice voor wachtwoordherstel (SSPR) in Azure Active directory (Azure AD) kunnen aanpassen zodat deze overeenkomen met de behoeften van hun gebruikers.

## <a name="customize-the-contact-your-administrator-link"></a>De koppeling 'Contact op met uw beheerder' aanpassen

Zelfs als SSPR niet is ingeschakeld, hebben gebruikers nog steeds een koppeling 'Neem contact op met uw beheerder' op de wachtwoord-portal opnieuw instellen. Als een gebruiker selecteert deze koppeling deze beide:
   * E-mails met uw beheerders en voor hulp bij het wijzigen van het wachtwoord van de gebruiker wordt gevraagd. 
   * Uw gebruikers verzendt naar een URL die u voor hulp opgeeft. 

U wordt aangeraden dat u deze contactpersoon ingesteld op iets zoals een e-mailadres of een website die uw gebruikers al voor ondersteuningsvragen gebruiken.

![Neem contact op met][Contact]

De e-mailadres is verzonden naar de volgende ontvangers in de volgende volgorde:

1. Als de **wachtwoordbeheerder** rol wordt toegewezen, beheerders met deze rol op de hoogte worden gesteld.
2. Als er geen wachtwoordbeheerders zijn toegewezen, klikt u vervolgens beheerders met de **beheerderrol** rol worden gewaarschuwd.
3. Als geen van de vorige rollen zijn toegewezen, wordt de **globale beheerders** worden gewaarschuwd.

In alle gevallen maximaal 100 ontvangers worden gewaarschuwd.

Zie voor meer informatie over de verschillende beheerdersrollen en hoe u deze kunt toewijzen, [beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).

### <a name="disable-contact-your-administrator-emails"></a>'Contact op met uw beheerder' e-mailberichten uitschakelen

Als uw organisatie wil niet op de hoogte van de beheerders over wachtwoord opnieuw instellen van aanvragen wilt, kunt u de volgende configuratie inschakelen:

* Selfservice voor wachtwoordherstel voor alle eindgebruikers inschakelen. Deze optie is onder **wachtwoordherstel** > **eigenschappen**.
  
  Als u niet dat gebruikers hun eigen wachtwoorden opnieuw kunnen instellen wilt, kunt u het bereik van toegang tot een lege groep. *Deze optie wordt niet aanbevolen.*
* Aanpassen van de helpdesk-koppeling voor een web-URL of mailto: adres waarmee gebruikers kunt u hulp nodig hebt. Deze optie is onder **wachtwoordherstel** > **aanpassing** > **aangepaste helpdesk e-mail of URL**.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>De AD FS-aanmeldingspagina voor SSPR aanpassen

Active Directory Federation Services (AD FS)-beheerders kunnen een koppeling toevoegen aan hun aanmeldingspagina met behulp van de richtlijnen gevonden in de [toevoegen aanmeldingspagina beschrijving](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description) artikel.

Als een koppeling aan de AD FS-aanmeldingspagina toevoegen, moet u de volgende opdracht gebruiken in uw AD FS-server. Gebruikers kunnen deze pagina gebruiken om in te voeren van de SSPR-werkstroom.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>Het aanmelden pagina en toegang Configuratiescherm uiterlijk aanpassen

U kunt de aanmeldingspagina aanpassen. U kunt een logo dat wordt weergegeven samen met de installatiekopie die past bij uw huisstijl kunt toevoegen.

De afbeeldingen die u kiest, worden weergegeven in de volgende omstandigheden:

* Nadat een gebruiker hun gebruikersnaam invoert
* Als de gebruiker toegang heeft tot de aangepaste URL:
    * Door de *w /* -parameter voor het wachtwoord opnieuw instellen van pagina, zoals 'https://login.microsoftonline.com/?whr=contoso.com'
    * Door de *gebruikersnaam* -parameter voor het wachtwoord opnieuw instellen pagina, zoals ' https://login.microsoftonline.com/?username=admin@contoso.com'

### <a name="graphics-details"></a>Grafische details

Gebruik de volgende instellingen te wijzigen van de visuele kenmerken van de aanmeldingspagina. Ga naar **Azure Active Directory** > **bedrijf huisstijl** > **bewerken huisstijl**:

* De installatiekopie van de aanmeldingspagina moet een PNG- of JPG-bestand, 1420 x 1200 pixels, en niet groter zijn dan 500 KB. Voor de beste resultaten adviseren we dat u deze ongeveer 200 KB.
* De achtergrondkleur van de aanmeldingspagina wordt gebruikt voor verbindingen met hoge latentie en moet in RGB-hexadecimale notatie.
* De banner afbeelding moet een PNG- of JPG-bestand, 60 x 280 pixels en niet groter zijn dan 10 KB.
* De vierkante logo (normaal en donker thema) moet een PNG- of JPG-bestand, 240 x 240 (formaat) pixels, en niet groter zijn dan 10 KB.

### <a name="sign-in-text-options"></a>Opties voor tekst van aanmeldingspagina

Gebruik de volgende instellingen tekst toevoegen aan de aanmeldingspagina die relevant is voor uw organisatie. Ga naar **Azure Active Directory** > **bedrijf huisstijl** > **bewerken huisstijl**:

* **Gebruiker naam hint**: vervangt de voorbeeldtekst van  *someone@example.com*  met iets meer geschikt is voor uw gebruikers. Het is raadzaam dat u de standaard-hint laten wanneer u ondersteuning voor interne en externe gebruikers.
* **Tekst van aanmeldingspagina**: mag maximaal 256 tekens lang zijn. Deze tekst wordt weergegeven waar die uw gebruikers zich online en in de Azure AD Workplace Join-ervaring op Windows 10 aanmelden. Gebruik deze tekst voor de voorwaarden van het gebruik, instructies en tips voor uw gebruikers. 

   >[!IMPORTANT]
   >Iedereen ziet uw aanmeldingspagina, zodat gevoelige informatie hier niet leveren.
   >

### <a name="the-keep-me-signed-in-disabled-setting"></a>De instelling "Me aangemeld houden uitgeschakeld"

Met de **aangemeld uitgeschakeld blijven** optie, gebruikers kunnen aangemeld blijven, wanneer ze sluiten en opnieuw hun browservenster openen. Deze optie heeft geen gevolgen voor de levensduur van de sessie. Ga naar **Azure Active Directory** > **bedrijf huisstijl** > **bewerken huisstijl**.

Sommige functies van Office 2010 en SharePoint Online hebben een afhankelijkheid op mogelijkheden Schakel dit selectievakje in. Als u deze optie verbergt, krijgen gebruikers aanvullende en onverwachte aanmelden wordt u gevraagd.

### <a name="directory-name"></a>Adreslijstnaam

U kunt het kenmerk directory name onder wijzigen **Azure Active Directory** > **eigenschappen**. U kunt een beschrijvende organisatienaam die is gezien weergeven in de portal en in de geautomatiseerde communicatie. Deze optie is het meest zichtbaar in automatische e-mailberichten in de formulieren die volgen:

* De beschrijvende naam in de e-mail, bijvoorbeeld 'Microsoft namens de CONTOSO-demo'
* De onderwerpregel van het e-mailbericht, bijvoorbeeld 'CONTOSO-demo account e verificatiecode'

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md)
* [Hebt u een vraag over licenties?](active-directory-passwords-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](active-directory-passwords-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](active-directory-passwords-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Contact]: ./media/active-directory-passwords-customize/sspr-contact-admin.png "Neem contact op met uw beheerder voor hulp bij het opnieuw instellen van uw wachtwoord e-voorbeeld"

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
ms.openlocfilehash: 526286c7f6b62d165af43487ca63fe9055623d0c
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
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

Meer informatie vinden over het configureren van de huisstijl op het artikel [toevoegen aan uw aanmeldingspagina in Azure AD huisstijl van uw bedrijf](customize-branding.md).

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

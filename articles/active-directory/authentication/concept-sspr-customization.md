---
title: Aanpassen Azure AD Self-service voor wachtwoord opnieuw instellen
description: Opties voor het aanpassen voor Azure AD Self-service voor wachtwoord opnieuw instellen
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 78e15de18e57e949b2e7b7146269204469e61797
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58106947"
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Aanpassen van de Azure AD-functionaliteit voor self-service voor wachtwoord opnieuw instellen

IT-professionals die u wilt om de selfservice voor wachtwoordherstel (SSPR) implementeren in Azure Active directory (Azure AD) kunnen aanpassen zodat deze overeenkomt met de behoeften van hun gebruikers.

## <a name="customize-the-contact-your-administrator-link"></a>De koppeling 'Neem contact op met uw beheerder' aanpassen

Zelfs als SSPR niet is ingeschakeld, hebben gebruikers nog steeds een koppeling 'Neem contact op met uw beheerder' op de wachtwoord-portal opnieuw instellen. Als de gebruiker kiest deze koppeling klikt, wordt een van beide:

   * Een e-mail van uw beheerders en voor hulp bij het wijzigen van het wachtwoord van de gebruiker wordt gevraagd.
   * Uw gebruikers verzendt naar een URL die u voor hulp opgeeft.

U wordt aangeraden dat u deze contact op met ingesteld op iets bijvoorbeeld een e-mailadres of een website die uw gebruikers al voor ondersteuningsvragen gebruiken.

![Neem contact op met][Contact]

Het contact op met e-mailbericht is verzonden naar de volgende ontvangers in de volgende volgorde:

1. Als de **wachtwoordbeheerder** rol is toegewezen, beheerders met deze rol op de hoogte worden gesteld.
2. Als er geen wachtwoordbeheerders zijn toegewezen, klikt u vervolgens beheerders met de **Gebruikerbeheerder** rol op de hoogte worden gesteld.
3. Als geen van de vorige rollen zijn toegewezen, dan zal de **globale beheerders** krijgt een melding.

In alle gevallen moet een maximum van 100 ontvangers worden gewaarschuwd.

Zie voor meer informatie over de verschillende beheerdersrollen en hoe u toe te wijzen, [beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).

### <a name="disable-contact-your-administrator-emails"></a>'Neem contact op met uw beheerder' e-mailberichten uitschakelen

Als uw organisatie wil niet op de hoogte stellen beheerders over het wachtwoord opnieuw instellen aanvragen, kunt u de volgende configuratie inschakelen:

* Self-service voor wachtwoord opnieuw instellen voor alle eindgebruikers inschakelen. Deze optie is onder **wachtwoordherstel** > **eigenschappen**. Als u niet wilt dat gebruikers hun eigen wachtwoorden opnieuw kunnen instellen, kunt u toegang tot een lege groep beperken. *Deze optie wordt niet aanbevolen.*
* Aanpassen van de helpdeskkoppeling voor een web-URL of mailto: adres waarmee gebruikers hulp te krijgen. Deze optie is onder **wachtwoordherstel** > **aanpassing** > **aangepaste helpdesk e-mailadres of URL**.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>De AD FS-aanmeldingspagina voor SSPR aanpassen

Active Directory Federation Services (AD FS)-beheerders kunnen een koppeling toevoegen aan hun aanmeldingspagina met behulp van de richtlijnen die zijn gevonden in de [toevoegen-aanmelden paginabeschrijving](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description) artikel.

Om toe te voegen een koppeling naar de AD FS-aanmeldingspagina, gebruikt u de volgende opdracht uit op uw AD FS-server. Gebruikers kunnen deze pagina gebruiken om in te voeren van de SSPR-werkstroom.

``` powershell
Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href='https://passwordreset.microsoftonline.com' target='_blank'>Can’t access your account?</A></p>"
```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>De aanmelding bij pagina- en toegangsbeheer deelvenster uiterlijk aanpassen

U kunt de aanmeldingspagina aanpassen. U kunt een logo dat wordt weergegeven, samen met de afbeelding die voldoet aan de huisstijl van uw bedrijf kunt toevoegen.

De afbeeldingen die u kiest, worden weergegeven in de volgende omstandigheden:

* Nadat een gebruiker moet hun gebruikersnaam invoeren
* Als de gebruiker toegang heeft tot de aangepaste URL:
    * Door door te geven de `whr` parameter voor het wachtwoord opnieuw instellen pagina, zoals `https://login.microsoftonline.com/?whr=contoso.com`
    * Door door te geven de `username` parameter voor het wachtwoord opnieuw instellen pagina, zoals `https://login.microsoftonline.com/?username=admin@contoso.com`

Meer informatie vinden over het configureren van aangepaste huisstijl in het artikel [Huisstijlwijzigingen naar de aanmeldingspagina in Azure AD](../fundamentals/customize-branding.md).

### <a name="directory-name"></a>Adreslijstnaam

U kunt het kenmerk directory name onder wijzigen **Azure Active Directory** > **eigenschappen**. In de portal en de geautomatiseerde communicatie, kunt u een aangepaste organisatienaam die is gezien weergeven. Deze optie is het meest zichtbaar in automatische e-mailberichten in de formulieren die gaat u als volgt:

* De beschrijvende naam in het e-mailbericht, bijvoorbeeld 'Microsoft namens de CONTOSO-demo'
* De onderwerpregel van het e-mailbericht, bijvoorbeeld 'CONTOSO-demo e-mailbericht verificatiecode van account"

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](howto-sspr-deployment.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](../user-help/active-directory-passwords-reset-register.md)
* [Hebt u een vraag over licenties?](concept-sspr-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](howto-sspr-authenticationdata.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](concept-sspr-howitworks.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](concept-sspr-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](howto-sspr-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](howto-sspr-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](concept-sspr-howitworks.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Contact]: ./media/concept-sspr-customization/sspr-contact-admin.png "Neem contact op met uw beheerder voor meer informatie over het opnieuw instellen van uw wachtwoord e-voorbeeld"

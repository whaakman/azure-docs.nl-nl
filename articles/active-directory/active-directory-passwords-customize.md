---
title: 'Aanpassen: Azure AD SSPR | Microsoft Docs'
description: Aanpassing van opties voor Azure AD self service voor wachtwoordherstel
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 030fb1d87547a4fc78d54a855bca961202f28837
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a>Azure AD-functionaliteit aanpassen voor selfservice voor wachtwoordherstel

IT-Professionals zoeken voor het implementeren van de selfservice voor wachtwoordherstel kunnen aanpassen zodat deze overeenkomen met hun gebruikers.

## <a name="customize-the-contact-your-administrator-link"></a>De contact op met uw beheerder koppeling aanpassen

Zelfs als SSPR is niet ingeschakeld gebruikers nog steeds een 'contact op met uw beheerder' een koppeling in het wachtwoord portal opnieuw instellen.  Op deze koppeling klikt, e-mails met uw beheerders om ondersteuning voor het wijzigen van het wachtwoord van de gebruiker vraagt of uw gebruikers verzendt naar een URL die u opgeeft. U wordt aangeraden dat u dit op iets zoals een e-mailadres of een website die uw gebruikers worden gebruikt instellen voor het gebruik voor ondersteuning.

![Neem contact op met][Contact]

Dit e-mailbericht wordt verzonden naar de volgende ontvangers in de volgende volgorde:

1. Als de **wachtwoordbeheerder** rol wordt toegewezen, zijn een melding van beheerders met deze rol
2. Als er geen wachtwoordbeheerders zijn toegewezen, klikt u vervolgens beheerders met de **beheerderrol** rol worden gewaarschuwd.
3. Als geen van de vorige rollen zijn toegewezen, klikt u vervolgens **globale beheerders** worden gewaarschuwd

In alle gevallen maximaal 100 ontvangers worden gewaarschuwd.

Raadpleeg het document voor meer informatie over de beheerder van de verschillende functies en hoe u ze toewijzen [beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles.md)

### <a name="disable-contact-your-administrator-emails"></a>Schakel Neem contact op met uw beheerder e-mailberichten

Als uw organisatie wil niet dat beheerders op de hoogte wachtwoord opnieuw instellen van aanvragen, kan de volgende configuratie worden ingeschakeld

* Selfservice voor wachtwoordherstel voor alle eindgebruikers inschakelen. Deze optie is onder **wachtwoordherstel > eigenschappen**.
    * Als u niet dat gebruikers hun eigen wachtwoorden opnieuw kunnen instellen wenst, kunt u toegang tot een lege groep bereik **niet raadzaam deze optie**.
* Aanpassen van de helpdesk-koppeling voor een web-URL of mailto: adres waarmee gebruikers kunt u hulp nodig hebt. Deze optie is onder **wachtwoordherstel > aanpassing > aangepaste helpdesk e-mail of URL**.

## <a name="customize-adfs-sign-in-page-for-sspr"></a>AD FS-aanmeldingspagina voor SSPR aanpassen

ADFS-beheerders kunnen een koppeling naar hun met behulp van de richtlijnen gevonden in het artikel aanmeldingspagina toevoegen [toevoegen aanmeldingspagina beschrijving](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Met de opdracht die op uw ADFS-server volgt, voegt een koppeling naar de aanmeldingspagina van AD FS, zodat gebruikers kunnen invoeren van de selfservice voor wachtwoordherstel werkstroom rechtstreeks herstellen.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-and-access-panel-look-and-feel"></a>Het aanmelden en toegang Configuratiescherm uiterlijk aanpassen

Wanneer uw gebruikers toegang krijgen de aanmeldingspagina tot, kunt u het logo dat wordt weergegeven samen met de aanmeldingspagina afbeelding aanpassen aan uw huisstijl aanpassen.

Deze afbeeldingen worden weergegeven in de volgende omstandigheden:

* Nadat een gebruiker typt hun gebruikersnaam
* Gebruiker heeft toegang tot de aangepaste url
    * Door w '/' pagina, zoals 'https://login.microsoftonline.com/?whr=contoso.com'-parameter voor het wachtwoord opnieuw instellen
    * Door het doorgeven van de 'gebruikersnaam'-parameter voor het wachtwoord opnieuw instellen pagina, zoals ' https://login.microsoftonline.com/?username=admin@contoso.com'

### <a name="graphics-details"></a>Grafische details

De volgende instellingen kunt u de visuele kenmerken van de aanmeldingspagina te wijzigen en kunt u vinden onder **Azure Active Directory**, **bedrijf huisstijl**, **huisstijl bewerken**

* Aanmeldingspagina afbeelding moet een PNG- of JPG-bestand 1420 x 1200 pixels en niet groter zijn dan 500KB. We raden aan dat het ongeveer 200 KB voor de beste resultaten.
* Achtergrondkleur van de aanmeldingspagina wordt gebruikt op de verbindingen met hoge latentie en moet de RGB-hexadecimale notatie.
* Banner afbeelding moet een PNG- of JPG-bestand 60 x 280 pixels en niet groter zijn dan 10 KB.
* Vierkante logo (normaal en donker thema) PNG- of JPG 240 x 240 (formaat) niet groter zijn dan 10 KB.

### <a name="sign-in-text-options"></a>Opties voor tekst van aanmeldingspagina

De volgende instellingen kunnen u tekst toevoegen aan de aanmeldingspagina relevant zijn voor uw organisatie. Deze instellingen kunnen worden gevonden in het **Azure Active Directory**, **bedrijf huisstijl**, **huisstijl bewerken**

* **Gebruiker naam hint** vervangt de voorbeeldtekst van someone@example.com met iets meer geschikt is voor uw gebruikers, de aanbevolen standaardwaarde blijven wanneer de ondersteuning van interne en externe gebruikers
* **Tekst van aanmeldingspagina** maximaal 256 tekens lang is. Deze tekst verschijnt overal uw aanmelding voor gebruikers online en in de Azure AD Join-ervaring op Windows 10. Gebruik deze tekst voor voorwaarden van het gebruik, instructies en tips voor uw gebruikers. **Iedereen kunt uw aanmeldingspagina zodat bieden geen gevoelige informatie hier zien.**

### <a name="keep-me-signed-in-disabled"></a>Aangemeld blijven uitgeschakeld

De optie kan 'Behouden mij uitgeschakeld aangemeld' gebruikers aangemeld blijven wanneer ze sluiten en opnieuw hun browservenster openen. Deze optie heeft geen gevolgen voor de levensduur van de sessie. Deze instelling te vinden onder **Azure Active Directory > bedrijf huisstijl > bewerken huisstijl**.

Sommige functies van Office 2010 en SharePoint Online hebben een afhankelijkheid op gebruikers kunnen dit selectievakje inschakelt. Als u deze optie verbergt, kunnen gebruikers meer en onverwachte aanmelden wordt u gevraagd krijgen.

### <a name="directory-name"></a>Mapnaam

U kunt het kenmerk name onder wijzigen **Azure Active Directory > eigenschappen** om weer te geven van een beschrijvende organisatienaam weergegeven in de portal en geautomatiseerde communicatie. Deze optie is zichtbaar zijn in de vorm van automatische e-mailberichten in de formulieren die volgen

* Beschrijvende naam in e-mailbericht 'Microsoft namens de CONTOSO-demo'
* Onderwerpregel in e-mailbericht 'CONTOSO-demo account e verificatiecode'

## <a name="next-steps"></a>Volgende stappen

* [Hoe ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Opnieuw instellen of wijzigen van uw wachtwoord](active-directory-passwords-update-your-own-password.md).
* [Registreren voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).
* [Hebt u een vraag licentieverlening?](active-directory-passwords-licensing.md)
* [Welke gegevens wordt gebruikt door de SSPR en welke gegevens moet u voor uw gebruikers vullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties met SSPR?](active-directory-passwords-policy.md)
* [Wat is Write-back van wachtwoord en waarom ik van belang?](active-directory-passwords-writeback.md)
* [Hoe meld ik op activiteit in SSPR?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in de SSPR en wat ze aan de hand?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets is verbroken. Hoe kan ik SSPR oplossen?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag waarvoor is geen ergens anders](active-directory-passwords-faq.md)

[Contact]: ./media/active-directory-passwords-customize/sspr-contact-admin.png "Neem contact op met uw beheerder voor hulp bij het opnieuw instellen van uw wachtwoord e-voorbeeld"
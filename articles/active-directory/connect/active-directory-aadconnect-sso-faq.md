---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding - Veelgestelde vragen | Microsoft Docs'
description: Antwoorden op veelgestelde vragen over Azure Active Directory naadloze eenmalige aanmelding.
services: active-directory
keywords: Wat is Azure AD Connect, installeer Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.author: billmath
ms.openlocfilehash: bbaed9ee5db895810fac476ea5bf560800b0bdec
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Azure Active Directory naadloze eenmalige aanmelding: veelgestelde vragen

In dit artikel adres we Veelgestelde vragen over Azure Active Directory naadloze eenmalige aanmelding (SSO naadloze). Terug naar de nieuwe inhoud blijven controleren.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>Welke methoden aanmelden werk naadloze eenmalige aanmelding met?

Naadloze eenmalige aanmelding kan worden gecombineerd met ofwel de [synchronisatie van wachtwoordhash](active-directory-aadconnectsync-implement-password-synchronization.md) of [Pass through-verificatie](active-directory-aadconnect-pass-through-authentication.md) aanmeldingsmethoden. Deze functie kan niet echter met Active Directory Federation Services (ADFS) gebruikt.

## <a name="is-seamless-sso-a-free-feature"></a>Naadloze eenmalige aanmelding een beschikbare functie is?

Naadloze eenmalige aanmelding is een gratis functie en u hoeft betaald edities van Azure AD om deze te gebruiken.

## <a name="is-seamless-sso-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Naadloze eenmalige aanmelding beschikbaar is in de [Duitse van Microsoft Azure cloud](http://www.microsoft.de/cloud-deutschland) en de [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/)?

Nee. Naadloze eenmalige aanmelding is alleen beschikbaar in het wereldwijde exemplaar van Azure AD.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Welke toepassingen profiteren van `domain_hint` of `login_hint` parameter mogelijkheid van naadloze eenmalige aanmelding?

We zijn bezig het compileren van de lijst met toepassingen die deze parameters en de waarden die niet verzenden. Als u toepassingen die u geïnteresseerd bent in hebt, laat ons weten in het gedeelte met opmerkingen.

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Biedt ondersteuning voor naadloze eenmalige aanmelding `Alternate ID` als de gebruikersnaam in plaats van `userPrincipalName`?

Ja. Naadloze eenmalige aanmelding ondersteunt `Alternate ID` als de gebruikersnaam wanneer geconfigureerd in Azure AD Connect, zoals [hier](active-directory-aadconnect-get-started-custom.md). Niet alle Office 365-toepassingen ondersteunen `Alternate ID`. Raadpleeg de documentatie voor de instructie ondersteuning van de specifieke toepassing.

## <a name="what-is-the-difference-between-the-single-sign-on-experience-provided-by-azure-ad-joinactive-directory-azureadjoin-overviewmd-and-seamless-sso"></a>Wat is het verschil tussen de ervaring voor eenmalige aanmelding geleverd door [Azure AD Join](../active-directory-azureadjoin-overview.md) en naadloze SSO?

[Azure AD Join](../active-directory-azureadjoin-overview.md) SSO biedt aan gebruikers als hun apparaten zijn geregistreerd bij Azure AD. Deze apparaten moeten niet noodzakelijkerwijs lid zijn van een domein. Eenmalige aanmelding wordt geleverd met *primaire vernieuwen van tokens* of *PRTs*, en niet Kerberos. De gebruikerservaring is meest optimaal op Windows 10-apparaten. Eenmalige aanmelding wordt automatisch op de Edge-browser. Dit werkt op Chrome met het gebruik van een Browseruitbreiding.

U kunt Azure AD Join en naadloze eenmalige aanmelding gebruiken op uw tenant. Deze twee functies zijn aanvullende. Als beide functies zijn ingeschakeld, heeft SSO van Azure AD Join voorrang op naadloze eenmalige aanmelding.

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>Ik wil Windows 10-apparaten registreren met Azure AD, zonder gebruik van AD FS. Kan ik naadloze eenmalige aanmelding in plaats daarvan gebruiken?

Ja, in dit scenario moet versie 2.1 of hoger van de [client voor werkplek koppelen](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account"></a>Hoe kan ik Beweeg de muis over de ontsleutelingssleutel Kerberos van de `AZUREADSSOACC` computeraccount?

Het is belangrijk te vaak overschakelen de ontsleutelingssleutel Kerberos van de `AZUREADSSOACC` computeraccount (die staat voor Azure AD) gemaakt in uw on-premises AD-forest.

>[!IMPORTANT]
>Het is raadzaam dat u de ontsleutelingssleutel Kerberos ten minste elke 30 dagen overschakelen.

Volg deze stappen op de lokale server waarop u Azure AD Connect uitvoert:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Step 1. Lijst met AD-forests waarbij naadloze eenmalige aanmelding is ingeschakeld

1. Eerst downloaden en installeren van de [Microsoft Online Services-aanmeldhulp](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Download en installeer vervolgens de [64-bits Azure Active Directory-module voor Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Navigeer naar de map `%programfiles%\Microsoft Azure Active Directory Connect`.
4. Importeer de naadloze eenmalige aanmelding PowerShell-module met de volgende opdracht: `Import-Module .\AzureADSSO.psd1`.
5. Voer de PowerShell als beheerder. In PowerShell, roept `New-AzureADSSOAuthenticationContext`. Met deze opdracht geeft u een pop-up van uw tenant hoofdbeheerder referenties invoeren.
6. Roep `Get-AzureADSSOStatus`. Deze opdracht kunt u de lijst met AD-forests (zoek op de lijst 'Domeinen') op die deze functie is ingeschakeld.

### <a name="step-2-update-the-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>Stap 2. Bijwerken van de Kerberos-ontsleutelingssleutel op elke AD-forest dat deze is deze instellen op

1. Roep `$creds = Get-Credential`. Wanneer u wordt gevraagd, typt u de domeinbeheerder-referenties voor het beoogde AD-forest.
2. Roep `Update-AzureADSSOForest -OnPremCredentials $creds`. Deze opdracht werkt de ontsleutelingssleutel Kerberos voor de `AZUREADSSOACC` computeraccount in deze specifieke AD-forest en bijgewerkt in Azure AD.
3. Herhaal de voorgaande stappen voor elk AD-forest dat u de functie hebt ingesteld op.

>[!IMPORTANT]
>Zorg ervoor dat u _niet_ uitvoeren de `Update-AzureADSSOForest` opdracht meer dan één keer. Anders wordt de functie werkt niet totdat de Kerberos-tickets voor uw gebruikers laten verlopen en opnieuw worden uitgegeven door uw lokale Active Directory.

## <a name="how-can-i-disable-seamless-sso"></a>Hoe kan ik naadloze eenmalige aanmelding uitschakelen?

Naadloze eenmalige aanmelding kan worden uitgeschakeld via Azure AD Connect.

Azure AD Connect uitgevoerd 'Wijzigen gebruiker aanmeldingspagina' op en klik op 'Volgende'. Schakel de optie 'Eenmalige aanmelding inschakelen op'. De wizard vervolgen. Naadloze eenmalige aanmelding is na voltooiing van de wizard wordt uitgeschakeld op uw tenant.

Echter, ziet u een bericht op het scherm die als volgt uitziet:

'Eenmalige aanmelding is nu uitgeschakeld, maar er zijn extra handmatige stappen uitvoeren om te kunnen voltooien opschonen. Meer informatie'

Voor het voltooien van het proces als volgt handmatig op de lokale server waarop u Azure AD Connect uitvoert:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Step 1. Lijst met AD-forests waarbij naadloze eenmalige aanmelding is ingeschakeld

1. Eerst downloaden en installeren van de [Microsoft Online Services-aanmeldhulp](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Download en installeer vervolgens de [64-bits Azure Active Directory-module voor Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Navigeer naar de map `%programfiles%\Microsoft Azure Active Directory Connect`.
4. Importeer de naadloze eenmalige aanmelding PowerShell-module met de volgende opdracht: `Import-Module .\AzureADSSO.psd1`.
5. Voer de PowerShell als beheerder. In PowerShell, roept `New-AzureADSSOAuthenticationContext`. Met deze opdracht geeft u een pop-up van uw tenant hoofdbeheerder referenties invoeren.
6. Roep `Get-AzureADSSOStatus`. Deze opdracht kunt u de lijst met AD-forests (zoek op de lijst 'Domeinen') op die deze functie is ingeschakeld.

### <a name="step-2-manually-delete-the-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>Stap 2. Verwijder handmatig de `AZUREADSSOACCT` computeraccount van elk AD-forest die u ziet die worden vermeld.

## <a name="next-steps"></a>Volgende stappen

- [**Snel starten** ](active-directory-aadconnect-sso-quick-start.md) - laten en Azure AD naadloze eenmalige aanmelding wordt uitgevoerd.
- [**Technische diepgaand** ](active-directory-aadconnect-sso-how-it-works.md) -begrijpen hoe deze functie werkt.
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.

---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding - Veelgestelde vragen | Microsoft Docs'
description: Antwoorden op veelgestelde vragen over Azure Active Directory naadloze eenmalige aanmelding.
services: active-directory
keywords: Wat is Azure AD Connect, Active Directory installeren onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 8e4cc67af4276bc244d402258a90dfec01d61add
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919016"
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Azure Active Directory naadloze eenmalige aanmelding: veelgestelde vragen

In dit artikel behandelen we Veelgestelde vragen over Azure Active Directory naadloze eenmalige aanmelding (naadloze eenmalige aanmelding). Voor nieuwe inhoud blijven controleren.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>Welke methoden aanmelden werken naadloze eenmalige aanmelding met?

Naadloze eenmalige aanmelding kan worden gecombineerd met ofwel de [wachtwoord-Hashsynchronisatie](active-directory-aadconnectsync-implement-password-hash-synchronization.md) of [Pass through-verificatie](active-directory-aadconnect-pass-through-authentication.md) aanmeldingsmethoden. Echter niet dat deze functie met Active Directory Federation Services (ADFS) worden gebruikt.

## <a name="is-seamless-sso-a-free-feature"></a>Een gratis functie voor naadloze eenmalige aanmelding is?

Naadloze eenmalige aanmelding is een gratis functie en hoeft u betaald edities van Azure AD om deze te gebruiken.

## <a name="is-seamless-sso-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Naadloze eenmalige aanmelding beschikbaar is in de [cloud van Microsoft Azure Duitsland](http://www.microsoft.de/cloud-deutschland) en de [Microsoft Azure Government-cloud](https://azure.microsoft.com/features/gov/)?

Nee. Naadloze eenmalige aanmelding is alleen beschikbaar in het wereldwijde exemplaar van Azure AD.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Welke toepassingen profiteren van `domain_hint` of `login_hint` parameter mogelijkheid van naadloze eenmalige aanmelding?

Hieronder vindt u een niet-volledige lijst met toepassingen die deze parameters verzenden naar Azure AD, en daarom biedt gebruikers een ervaring voor op de achtergrond aanmelding met naadloze eenmalige aanmelding (dat wil zeggen, niet nodig voor uw gebruikers voor het invoeren van de gebruikersnamen):

| De naam van de toepassing | De URL van de toepassing moet worden gebruikt |
| -- | -- |
| Toegangsvenster | myapps.Microsoft.com/contoso.com |
| Webversie van Outlook | outlook.office365.com/contoso.com |

Bovendien krijgen gebruikers een ervaring voor op de achtergrond als een toepassing aanmeldingsaanvragen naar Azure AD-tenants eindpunten - dat wil zeggen verzendt, https://login.microsoftonline.com/contoso.com/<..> of https://login.microsoftonline.com/<tenant_ID>/<..> : in plaats van Azure AD gemeenschappelijk eindpunt - dat wil zeggen, https://login.microsoftonline.com/common/<...>. Hieronder vindt u een niet-volledige lijst met toepassingen die dit aanmeldingsaanvragen soort.

| De naam van de toepassing | De URL van de toepassing moet worden gebruikt |
| -- | -- |
| SharePoint Online | contoso.sharepoint.com |
| Azure Portal | portal.azure.com/contoso.com |

In de bovenstaande tabellen, kunt u 'contoso.com' vervangen door de naam van uw domein om te gaan naar de juiste URL's voor uw tenant.

Als u andere toepassingen met behulp van onze ervaring voor op de achtergrond wilt, laat het ons weten in de feedbacksectie.

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Biedt ondersteuning voor naadloze eenmalige aanmelding `Alternate ID` als de gebruikersnaam in plaats van `userPrincipalName`?

Ja. Naadloze eenmalige aanmelding ondersteunt `Alternate ID` als de gebruikersnaam wanneer in Azure AD Connect hebt geconfigureerd zoals wordt weergegeven [hier](active-directory-aadconnect-get-started-custom.md). Niet alle Office 365-toepassingen ondersteuning bieden voor `Alternate ID`. Raadpleeg de documentatie van de specifieke toepassing voor de instructie ondersteuning.

## <a name="what-is-the-difference-between-the-single-sign-on-experience-provided-by-azure-ad-joinactive-directory-azureadjoin-overviewmd-and-seamless-sso"></a>Wat is het verschil tussen de ervaring voor eenmalige aanmelding verstrekt door [Azure AD Join](../active-directory-azureadjoin-overview.md) en naadloze eenmalige aanmelding?

[Azure AD Join](../active-directory-azureadjoin-overview.md) biedt eenmalige aanmelding voor gebruikers als hun apparaten zijn geregistreerd bij Azure AD. Deze apparaten hoeven niet te worden van domein. Eenmalige aanmelding wordt geleverd met behulp van *primaire vernieuwingstokens* of *PRTs*, en niet Kerberos. De gebruikerservaring is optimale op Windows 10-apparaten. Eenmalige aanmelding wordt automatisch uitgevoerd op de Edge-browser. Het werkt ook op Chrome met het gebruik van een browserextensie.

U kunt Azure AD-aanmelding en naadloze eenmalige aanmelding gebruiken op uw tenant. Deze twee functies zijn aanvullende. Als beide functies zijn ingeschakeld, klikt u vervolgens voorrang eenmalige aanmelding van Azure AD Join boven naadloze eenmalige aanmelding.

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>Ik wil Windows 10-apparaten registreren bij Azure AD, zonder gebruik van AD FS. Kan ik naadloze eenmalige aanmelding in plaats daarvan gebruiken?

Ja, dit scenario moet versie 2.1 of hoger van de [workplace join client](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account"></a>Hoe kan ik de muisaanwijzer op de ontsleutelingssleutel Kerberos van de `AZUREADSSOACC` computeraccount?

Het is belangrijk dat u vaak plaats de muisaanwijzer op de ontsleutelingssleutel Kerberos van de `AZUREADSSOACC` computeraccount (die staat voor Azure AD) hebt gemaakt in uw on-premises AD-forest.

>[!IMPORTANT]
>Het is raadzaam dat u via de ontsleutelingssleutel Kerberos ten minste elke 30 dagen implementeert.

Volg deze stappen op de on-premises server waarop Azure AD Connect:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Step 1. Lijst met AD-forests waarop naadloze eenmalige aanmelding is ingeschakeld

1. Eerst, downloaden en installeren van de [Microsoft Online Services-aanmeldhulp](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Download en installeer vervolgens de [64-bits Azure Active Directory-module voor Windows PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0).
3. Navigeer naar de map `%programfiles%\Microsoft Azure Active Directory Connect`.
4. Importeer de naadloze eenmalige aanmelding PowerShell-module met de volgende opdracht: `Import-Module .\AzureADSSO.psd1`.
5. Voer PowerShell uit als beheerder. In PowerShell, roept u `New-AzureADSSOAuthenticationContext`. Met deze opdracht geeft u een pop-upvenster van uw tenant hoofdbeheerder referenties in te voeren.
6. Bel `Get-AzureADSSOStatus`. Met deze opdracht biedt u de lijst met AD-forests (zoek op de lijst met 'Domeinen') op die deze functie is ingeschakeld.

### <a name="step-2-update-the-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>Stap 2. Bijwerken van de Kerberos-ontsleutelingssleutel op elk AD-forest dat deze is dit instellen op

1. Bel `$creds = Get-Credential`. Wanneer u wordt gevraagd, typt u de domeinbeheerder-referenties voor het beoogde AD-forest.
2. Bel `Update-AzureADSSOForest -OnPremCredentials $creds`. Deze opdracht werkt de ontsleutelingssleutel Kerberos voor de `AZUREADSSOACC` computeraccount in deze specifieke AD-forest en bijgewerkt in Azure AD.
3. Herhaal de voorgaande stappen voor elk AD-forest dat u de functie hebt ingesteld op.

>[!IMPORTANT]
>Zorg ervoor dat u _niet_ uitvoeren de `Update-AzureADSSOForest` meerdere keren opdracht. Anders wordt de functie werkt niet meer totdat de Kerberos-tickets voor uw gebruikers laten verlopen en opnieuw worden uitgegeven door uw on-premises Active Directory.

## <a name="how-can-i-disable-seamless-sso"></a>Hoe kan ik naadloze eenmalige aanmelding uitschakelen?

Naadloze eenmalige aanmelding kan worden uitgeschakeld met behulp van Azure AD Connect.

Azure AD Connect uitvoeren, kies 'Gebruiker aanmelden pagina wijzigen' en klik op 'Volgende'. Schakel de optie 'Eenmalige aanmelding inschakelen op'. Ga door met de wizard. Naadloze eenmalige aanmelding is na voltooiing van de wizard wordt uitgeschakeld op uw tenant.

Echter, ziet u een bericht op het scherm dat als volgt:

'Eenmalige aanmelding is nu uitgeschakeld, maar er zijn aanvullende handmatige stappen voor het uitvoeren als u wilt opschonen te voltooien. Meer informatie"

Volg deze handmatige stappen voor het voltooien van het proces op de on-premises server waarop Azure AD Connect:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Step 1. Lijst met AD-forests waarop naadloze eenmalige aanmelding is ingeschakeld

1. Eerst, downloaden en installeren van de [Microsoft Online Services-aanmeldhulp](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Download en installeer vervolgens de [64-bits Azure Active Directory-module voor Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Navigeer naar de map `%programfiles%\Microsoft Azure Active Directory Connect`.
4. Importeer de naadloze eenmalige aanmelding PowerShell-module met de volgende opdracht: `Import-Module .\AzureADSSO.psd1`.
5. Voer PowerShell uit als beheerder. In PowerShell, roept u `New-AzureADSSOAuthenticationContext`. Met deze opdracht geeft u een pop-upvenster van uw tenant hoofdbeheerder referenties in te voeren.
6. Bel `Get-AzureADSSOStatus`. Met deze opdracht biedt u de lijst met AD-forests (zoek op de lijst met 'Domeinen') op die deze functie is ingeschakeld.

### <a name="step-2-manually-delete-the-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>Stap 2. Verwijder handmatig de `AZUREADSSOACCT` computeraccount in elk AD-forest die u ziet die worden vermeld.

## <a name="next-steps"></a>Volgende stappen

- [**Snel aan de slag** ](active-directory-aadconnect-sso-quick-start.md) : aan de slag en Azure AD naadloze eenmalige aanmelding wordt uitgevoerd.
- [**Technische details** ](active-directory-aadconnect-sso-how-it-works.md) -te begrijpen hoe deze functie werkt.
- [**Problemen oplossen** ](active-directory-aadconnect-troubleshoot-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen in te dienen.

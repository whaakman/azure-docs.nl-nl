---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding - Veelgestelde vragen | Microsoft Docs'
description: Antwoorden op veelgestelde vragen over Azure Active Directory naadloze eenmalige aanmelding.
services: active-directory
keywords: Wat is Azure AD Connect, Active Directory installeren onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/14/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 709fb3be37850be37d6378652921ce26f4ff15fe
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58804374"
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Azure Active Directory naadloze eenmalige aanmelding: Veelgestelde vragen

In dit artikel behandelen we Veelgestelde vragen over Azure Active Directory naadloze eenmalige aanmelding (naadloze eenmalige aanmelding). Voor nieuwe inhoud blijven controleren.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>Welke methoden aanmelden werken naadloze eenmalige aanmelding met?

Naadloze eenmalige aanmelding kan worden gecombineerd met ofwel de [wachtwoord-Hashsynchronisatie](how-to-connect-password-hash-synchronization.md) of [Pass through-verificatie](how-to-connect-pta.md) aanmeldingsmethoden. Echter niet dat deze functie met Active Directory Federation Services (ADFS) worden gebruikt.

## <a name="is-seamless-sso-a-free-feature"></a>Een gratis functie voor naadloze eenmalige aanmelding is?

Naadloze eenmalige aanmelding is een gratis functie en hoeft u betaald edities van Azure AD om deze te gebruiken.

## <a name="is-seamless-sso-available-in-the-microsoft-azure-germany-cloudhttpswwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Naadloze eenmalige aanmelding beschikbaar is in de [cloud van Microsoft Azure Duitsland](https://www.microsoft.de/cloud-deutschland) en de [Microsoft Azure Government-cloud](https://azure.microsoft.com/features/gov/)?

Nee. Naadloze eenmalige aanmelding is alleen beschikbaar in het wereldwijde exemplaar van Azure AD.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Welke toepassingen profiteren van `domain_hint` of `login_hint` parameter mogelijkheid van naadloze eenmalige aanmelding?

Hieronder vindt u een niet-volledige lijst met toepassingen die deze parameters kunt verzenden naar Azure AD, en daarom biedt gebruikers een ervaring voor op de achtergrond aanmelding met naadloze eenmalige aanmelding (dat wil zeggen, niet nodig voor uw gebruikers voor het invoeren van de gebruikersnamen of wachtwoorden):

| De naam van de toepassing | De URL van de toepassing moet worden gebruikt |
| -- | -- |
| Toegangsvenster | https:\//myapps.microsoft.com/contoso.com |
| Webversie van Outlook | https:\//outlook.office365.com/contoso.com |
| Office 365-portals | https:\//portal.office.com?domain_hint=contoso.com, https:\//www.office.com?domain_hint=contoso.com |

Bovendien krijgen gebruikers een ervaring voor op de achtergrond als een toepassing verzendt aanvragen aanmelden voor Azure AD-eindpunten instellen als tenants - dat wil zeggen, https:\//login.microsoftonline.com/contoso.com/ <... >- of https:\//login.microsoftonline.com/ < tenant_ID > / <... >: in plaats van Azure AD gemeenschappelijk eindpunt - dat wil zeggen, https:\//login.microsoftonline.com/common/ <... >. Hieronder vindt u een niet-volledige lijst met toepassingen die dit aanmeldingsaanvragen soort.

| De naam van de toepassing | De URL van de toepassing moet worden gebruikt |
| -- | -- |
| SharePoint Online | https:\//contoso.sharepoint.com |
| Azure Portal | https:\//portal.azure.com/contoso.com |

In de bovenstaande tabellen, kunt u 'contoso.com' vervangen door de naam van uw domein om te gaan naar de juiste URL's voor uw tenant.

Als u andere toepassingen met behulp van onze ervaring voor op de achtergrond wilt, laat het ons weten in de feedbacksectie.

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Biedt ondersteuning voor naadloze eenmalige aanmelding `Alternate ID` als de gebruikersnaam in plaats van `userPrincipalName`?

Ja. Naadloze eenmalige aanmelding ondersteunt `Alternate ID` als de gebruikersnaam wanneer in Azure AD Connect hebt geconfigureerd zoals wordt weergegeven [hier](how-to-connect-install-custom.md). Niet alle Office 365-toepassingen ondersteuning bieden voor `Alternate ID`. Raadpleeg de documentatie van de specifieke toepassing voor de instructie ondersteuning.

## <a name="what-is-the-difference-between-the-single-sign-on-experience-provided-by-azure-ad-joinactive-directory-azureadjoin-overviewmd-and-seamless-sso"></a>Wat is het verschil tussen de ervaring voor eenmalige aanmelding verstrekt door [Azure AD Join](../active-directory-azureadjoin-overview.md) en naadloze eenmalige aanmelding?

[Azure AD Join](../active-directory-azureadjoin-overview.md) biedt eenmalige aanmelding voor gebruikers als hun apparaten zijn geregistreerd bij Azure AD. Deze apparaten hoeven niet te worden van domein. Eenmalige aanmelding wordt geleverd met behulp van *primaire vernieuwingstokens* of *PRTs*, en niet Kerberos. De gebruikerservaring is optimale op Windows 10-apparaten. Eenmalige aanmelding wordt automatisch uitgevoerd op de browser Microsoft Edge. Het werkt ook op Chrome met het gebruik van een browserextensie.

U kunt Azure AD-aanmelding en naadloze eenmalige aanmelding gebruiken op uw tenant. Deze twee functies zijn aanvullende. Als beide functies zijn ingeschakeld, klikt u vervolgens voorrang eenmalige aanmelding van Azure AD Join boven naadloze eenmalige aanmelding.

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>Ik wil Windows 10-apparaten registreren bij Azure AD, zonder gebruik van AD FS. Kan ik naadloze eenmalige aanmelding in plaats daarvan gebruiken?

Ja, dit scenario moet versie 2.1 of hoger van de [workplace join client](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account"></a>Hoe kan ik de muisaanwijzer op de ontsleutelingssleutel Kerberos van de `AZUREADSSOACC` computeraccount?

Het is belangrijk dat u vaak plaats de muisaanwijzer op de ontsleutelingssleutel Kerberos van de `AZUREADSSOACC` computeraccount (die staat voor Azure AD) hebt gemaakt in uw on-premises AD-forest.

>[!IMPORTANT]
>Het is raadzaam dat u via de ontsleutelingssleutel Kerberos ten minste elke 30 dagen implementeert.

Volg deze stappen op de on-premises server waarop Azure AD Connect:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Step 1. Lijst met AD-forests waarop naadloze eenmalige aanmelding is ingeschakeld

1. Eerst, download en installeer [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Navigeer naar de map `%programfiles%\Microsoft Azure Active Directory Connect`.
3. Importeer de naadloze eenmalige aanmelding PowerShell-module met de volgende opdracht: `Import-Module .\AzureADSSO.psd1`.
4. Voer PowerShell uit als beheerder. In PowerShell, roept u `New-AzureADSSOAuthenticationContext`. Met deze opdracht geeft u een pop-upvenster van uw tenant hoofdbeheerder referenties in te voeren.
5. Bel `Get-AzureADSSOStatus | ConvertFrom-Json`. Met deze opdracht biedt u de lijst met AD-forests (zoek op de lijst met 'Domeinen') op die deze functie is ingeschakeld.

### <a name="step-2-update-the-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>Stap 2. Bijwerken van de Kerberos-ontsleutelingssleutel op elk AD-forest dat deze is dit instellen op

1. Bel `$creds = Get-Credential`. Wanneer u wordt gevraagd, typt u de domeinbeheerder-referenties voor het beoogde AD-forest.

   > [!NOTE]
   > We gebruiken de domeinbeheerder username, opgegeven in de User Principal namen (UPN) (johndoe@contoso.com) indeling of domein gekwalificeerde sam-account,-naam (contoso\janjansen of contoso.com\johndoe), om te vinden van de beoogde AD-forest. Als u de domeinnaam van de gekwalificeerde sam-account gebruikt, gebruiken we het domeingedeelte van de gebruikersnaam voor [vinden van de domeincontroller van de beheerder van het domein met behulp van DNS](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx). Als u de UPN in plaats daarvan gebruiken we [vertaald in een domein gekwalificeerde sam-accountnaam](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa) voordat het zoeken naar de desbetreffende domeincontroller.

2. Bel `Update-AzureADSSOForest -OnPremCredentials $creds`. Deze opdracht werkt de ontsleutelingssleutel Kerberos voor de `AZUREADSSOACC` computeraccount in deze specifieke AD-forest en bijgewerkt in Azure AD.
3. Herhaal de voorgaande stappen voor elk AD-forest dat u de functie hebt ingesteld op.

>[!IMPORTANT]
>Zorg ervoor dat u _niet_ uitvoeren de `Update-AzureADSSOForest` meerdere keren opdracht. Anders wordt de functie werkt niet meer totdat de Kerberos-tickets voor uw gebruikers laten verlopen en opnieuw worden uitgegeven door uw on-premises Active Directory.

## <a name="how-can-i-disable-seamless-sso"></a>Hoe kan ik naadloze eenmalige aanmelding uitschakelen?

### <a name="step-1-disable-the-feature-on-your-tenant"></a>Step 1. Schakel de functie op uw tenant

#### <a name="option-a-disable-using-azure-ad-connect"></a>Optie A: Uitschakelen met behulp van Azure AD Connect

1. Azure AD Connect uitvoeren, kiest u **gebruiker aanmelden pagina wijzigen** en klikt u op **volgende**.
2. Schakel de **eenmalige aanmelding inschakelen** optie. Ga door met de wizard.

Nadat de wizard is voltooid, wordt de naadloze eenmalige aanmelding worden uitgeschakeld op uw tenant. Echter, ziet u een bericht op het scherm dat als volgt:

'Eenmalige aanmelding is nu uitgeschakeld, maar er zijn aanvullende handmatige stappen voor het uitvoeren als u wilt opschonen te voltooien. Meer informatie"

Als u wilt de opschonen te voltooien, volgt u de stappen 2 en 3 op de on-premises server waarop Azure AD Connect.

#### <a name="option-b-disable-using-powershell"></a>Optie B: Uitschakelen met behulp van PowerShell

Voer de volgende stappen uit op de on-premises server waarop Azure AD Connect:

1. Eerst, download en installeer [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Navigeer naar de map `%programfiles%\Microsoft Azure Active Directory Connect`.
3. Importeer de naadloze eenmalige aanmelding PowerShell-module met de volgende opdracht: `Import-Module .\AzureADSSO.psd1`.
4. Voer PowerShell uit als beheerder. In PowerShell, roept u `New-AzureADSSOAuthenticationContext`. Met deze opdracht geeft u een pop-upvenster van uw tenant hoofdbeheerder referenties in te voeren.
5. Bel `Enable-AzureADSSO -Enable $false`.

>[!IMPORTANT]
>Naadloze eenmalige aanmelding uitschakelen verandert met behulp van PowerShell niet de status in Azure AD Connect. Naadloze eenmalige aanmelding wordt weergegeven als ingeschakeld in de **aanmelden van gebruikers wijzigen** pagina.

### <a name="step-2-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Stap 2. Lijst met AD-forests waarop naadloze eenmalige aanmelding is ingeschakeld

Voer de taken 1 tot en met 4 hieronder als u naadloze eenmalige aanmelding met Azure AD Connect hebt uitgeschakeld. Als u naadloze eenmalige aanmelding met behulp van PowerShell in plaats daarvan hebt uitgeschakeld, gaat u verder naar taak 5 hieronder.

1. Eerst, download en installeer [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Navigeer naar de map `%programfiles%\Microsoft Azure Active Directory Connect`.
3. Importeer de naadloze eenmalige aanmelding PowerShell-module met de volgende opdracht: `Import-Module .\AzureADSSO.psd1`.
4. Voer PowerShell uit als beheerder. In PowerShell, roept u `New-AzureADSSOAuthenticationContext`. Met deze opdracht geeft u een pop-upvenster van uw tenant hoofdbeheerder referenties in te voeren.
5. Bel `Get-AzureADSSOStatus | ConvertFrom-Json`. Met deze opdracht biedt u de lijst met AD-forests (zoek op de lijst met 'Domeinen') op die deze functie is ingeschakeld.

### <a name="step-3-manually-delete-the-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>Stap 3. Verwijder handmatig de `AZUREADSSOACCT` computeraccount in elk AD-forest die u ziet die worden vermeld.

## <a name="next-steps"></a>Volgende stappen

- [**Snel aan de slag** ](how-to-connect-sso-quick-start.md) : aan de slag en Azure AD naadloze eenmalige aanmelding wordt uitgevoerd.
- [**Technische details** ](how-to-connect-sso-how-it-works.md) -te begrijpen hoe deze functie werkt.
- [**Problemen oplossen** ](tshoot-connect-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen in te dienen.

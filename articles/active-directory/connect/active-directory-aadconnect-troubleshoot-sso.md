---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding oplossen | Microsoft Docs'
description: Dit onderwerp wordt beschreven hoe u problemen met Azure Active Directory naadloze eenmalige aanmelding (Azure AD naadloze SSO).
services: active-directory
keywords: Wat is Azure AD Connect, installeer Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: b383a21500c753d8d2fe6747756541a3ff94ef02
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Problemen met Azure Active Directory naadloze eenmalige aanmelding

Dit artikel helpt u bij het oplossen van problemen informatie over veelvoorkomende problemen met betrekking tot Azure AD naadloze eenmalige aanmelding.

## <a name="known-issues"></a>Bekende problemen

- In sommige gevallen kan naadloze eenmalige aanmelding inschakelen maximaal 30 minuten duren.
- Edge-browser-ondersteuning is niet beschikbaar.
- Office-clients starten, met name in scenario's met gedeelde computer gebruikt, ertoe leiden dat extra aanmelden prompts voor gebruikers. Gebruikers moeten Voer regelmatig hun gebruikersnamen, maar geen wachtwoorden.
- Als naadloze eenmalige aanmelding is gelukt, krijgt de gebruiker niet de mogelijkheid om te kiezen 'Aangemeld blijven'. Vanwege dit gedrag werken SharePoint en OneDrive toewijzing scenario's niet.
- Naadloze eenmalige aanmelding werkt niet in de privémodus Browse op Firefox.
- Naadloze eenmalige aanmelding werkt niet in Internet Explorer als verbeterde beveiliging modus is ingeschakeld.
- Naadloze eenmalige aanmelding werkt niet op mobiele browsers op iOS en Android.
- Als u meer dan 30 AD-forests synchroniseert, kunt u naadloze eenmalige aanmelding met Azure AD Connect niet inschakelen. Als een tijdelijke oplossing kunt u [handmatig inschakelen](#manual-reset-of-azure-ad-seamless-sso) de functie op uw tenant.
- Azure AD-URL's (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) toe te voegen aan de zone 'Vertrouwde sites' in plaats van de zone 'Lokaal intranet' **voorkomen dat gebruikers aanmelden**.

## <a name="check-status-of-the-feature"></a>Controleer de status van de functie

Zorg ervoor dat de functie naadloze eenmalige aanmelding nog steeds is **ingeschakeld** op uw tenant. U kunt de status controleren door te gaan naar de **Azure AD Connect** blade in de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/).

![Azure Active Directory-beheercentrum - blade van Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Aanmelding mislukt redenen op het Azure Active Directory-beheercentrum (Premium-licentie vereist)

Als uw tenant een Azure AD Premium-licentie die is gekoppeld heeft, kunt u ook zoeken op de [aanmeldingsactiviteiten rapport](../active-directory-reporting-activity-sign-ins.md) op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/).

![Azure Active Directory-beheercentrum - aanmeldingen rapport](./media/active-directory-aadconnect-sso/sso9.png)

Navigeer naar **Azure Active Directory** -> **aanmeldingen** op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/) en klik op een specifieke gebruiker aanmelden activiteit. Zoek naar de **SIGN-IN-FOUTCODE** veld. De waarde van dat veld worden toegewezen aan een reden voor fout en de oplossing met behulp van de volgende tabel:

|Aanmelden foutcode|Aanmelding mislukt reden|Oplossing
| --- | --- | ---
| 81001 | Kerberos-ticket van de gebruiker is te groot. | Reduceer groepslidmaatschappen van gebruiker en probeer het opnieuw.
| 81002 | Kan Kerberos-ticket van de gebruiker niet valideren. | Zie [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81003 | Kan Kerberos-ticket van de gebruiker niet valideren. | Zie [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81004 | Poging tot Kerberos-verificatie is mislukt. | Zie [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81008 | Kan Kerberos-ticket van de gebruiker niet valideren. | Zie [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81009 | 'Kan niet de Kerberos-ticket van de gebruiker te valideren. | Zie [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81010 | Naadloze eenmalige aanmelding is mislukt omdat het Kerberos-ticket van de gebruiker is verlopen of ongeldig is. | Gebruiker moet zich aanmelden via een apparaat lid van een domein binnen uw bedrijfsnetwerk.
| 81011 | Kan gebruikersobject niet vinden op basis van de informatie in het Kerberos-ticket van de gebruiker. | Azure AD Connect gebruiken om gebruikersinformatie in Azure AD te synchroniseren.
| 81012 | De gebruiker die zich probeert aan te melden bij Azure AD, komt niet overeen met de gebruiker die is aangemeld bij het apparaat. | Meld u vanaf een ander apparaat.
| 81013 | Kan gebruikersobject niet vinden op basis van de informatie in het Kerberos-ticket van de gebruiker. |Azure AD Connect gebruiken om gebruikersinformatie in Azure AD te synchroniseren. 

## <a name="troubleshooting-checklist"></a>Controlelijst voor probleemoplossing

Gebruik de volgende controlelijst naadloze eenmalige aanmelding problemen kunt oplossen:

- Controleer of de functie naadloze eenmalige aanmelding is ingeschakeld in Azure AD Connect. Als u de functie (bijvoorbeeld als gevolg van een geblokkeerde-poort) kan niet inschakelt, zorg ervoor dat u alle de [vereisten](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites) aanwezig.
- Controleer of beide deze Azure AD-URL's (https://autologon.microsoftazuread-sso.com en https://aadg.windows.net.nsatc.net) deel uit van de Intranet-beveiligingszone-instellingen van de gebruiker zijn.
- Zorg ervoor dat het bedrijfsapparaat is toegevoegd aan de AD-domein.
- Zorg ervoor dat de gebruiker is aangemeld bij het apparaat met een AD-domeinaccount.
- Zorg ervoor dat het gebruikersaccount is van een AD-forest waarbij naadloze eenmalige aanmelding is ingesteld.
- Zorg ervoor dat het apparaat is aangesloten op het bedrijfsnetwerk bevinden.
- Zorg ervoor dat de tijd van het apparaat is gesynchroniseerd met de Active Directory en de domeincontrollers tijd en binnen vijf minuten van elkaar is.
- Lijst met bestaande Kerberos-tickets op het apparaat via de **Klist uit** opdracht vanaf een opdrachtprompt. Controleer als tickets uitgegeven voor de `AZUREADSSOACCT` computeraccount aanwezig zijn. Gebruikers Kerberos-tickets zijn doorgaans geldig 12 uur. Mogelijk hebt u verschillende instellingen in uw Active Directory.
- Bestaande Kerberos-tickets opschonen van het apparaat via de **Klist uit opschonen** opdracht en probeer het opnieuw.
- Om te bepalen of er problemen met de JavaScript zijn, Controleer de logboeken van de console van de browser (onder 'hulpprogramma's voor ontwikkelaars').
- Controleer de [registreert domeincontroller](#domain-controller-logs) ook.

### <a name="domain-controller-logs"></a>Registreert domeincontroller

Als de controle van geslaagde pogingen is ingeschakeld op uw domeincontroller en vervolgens telkens wanneer een gebruiker zich aanmeldt met naadloze eenmalige aanmelding wordt een vermelding beveiliging vastgelegd in het gebeurtenislogboek. U vindt deze de volgende query beveiligingsgebeurtenissen (zoek naar gebeurtenis **4769** die zijn gekoppeld aan het computeraccount **AzureADSSOAcc$**):

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Handmatig opnieuw instellen van de functie

Als het oplossen van problemen hebt gehad, kunt u de functie handmatig herstellen op uw tenant. Volg deze stappen op de lokale server waarop u Azure AD Connect uitvoert:

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>Stap 1: De naadloze eenmalige aanmelding PowerShell-module importeren

1. Eerst downloaden en installeren van de [Microsoft Online Services-aanmeldhulp](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Download en installeer vervolgens de [64-bits Azure Active Directory-module voor Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Navigeer naar de `%programfiles%\Microsoft Azure Active Directory Connect` map.
4. Importeer de naadloze eenmalige aanmelding PowerShell-module met de volgende opdracht: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>Stap 2: Haal de lijst met AD-forests waarop naadloze eenmalige aanmelding is ingeschakeld

1. PowerShell als Administrator uitvoeren. In PowerShell, roept `New-AzureADSSOAuthenticationContext`. Voer desgevraagd uw globale tenantbeheerderreferenties.
2. Roep `Get-AzureADSSOStatus`. Deze opdracht kunt u de lijst met AD-forests (zoek op de lijst 'Domeinen') op die deze functie is ingeschakeld.

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>Stap 3: Naadloze eenmalige aanmelding voor elke AD-forest dat deze is deze instellen op uitschakelen

1. Roep `$creds = Get-Credential`. Wanneer u wordt gevraagd, typt u de domeinbeheerder-referenties voor het beoogde AD-forest.
2. Roep `Disable-AzureADSSOForest -OnPremCredentials $creds`. Deze opdracht verwijdert u de `AZUREADSSOACCT` computeraccount van de lokale domeincontroller voor dit specifieke AD-forest.
3. Herhaal de voorgaande stappen voor elk AD-forest dat u de functie hebt ingesteld op.

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>Stap 4: Naadloze eenmalige aanmelding inschakelen voor elk AD-forest

1. Roep `Enable-AzureADSSOForest`. Wanneer u wordt gevraagd, typt u de domeinbeheerder-referenties voor het beoogde AD-forest.
2. Herhaal de voorgaande stappen voor elk AD-forest die u instellen van de functie wilt op.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>Stap 5. De functie inschakelen voor uw tenant

Roep `Enable-AzureADSSO` en typt u 'true' op de `Enable: ` vragen om de functie in uw tenant inschakelen.

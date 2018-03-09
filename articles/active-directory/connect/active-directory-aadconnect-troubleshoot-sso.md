---
title: 'Azure Active Directory Connect: Naadloze eenmalige aanmelding oplossen | Microsoft Docs'
description: Dit onderwerp wordt beschreven hoe u problemen met Azure Active Directory naadloze eenmalige aanmelding
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
ms.date: 03/07/2018
ms.author: billmath
ms.openlocfilehash: 6e81ea9f98733b1b7e0c9bf7466ac844a37b6046
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Problemen met Azure Active Directory naadloze eenmalige aanmelding

Dit artikel helpt u bij het oplossen van problemen informatie over veelvoorkomende problemen met betrekking tot Azure Active Directory (Azure AD) naadloze eenmalige aanmelding (SSO naadloze).

## <a name="known-issues"></a>Bekende problemen

- In sommige gevallen kan naadloze eenmalige aanmelding inschakelen maximaal 30 minuten duren.
- Als u uitschakelt en opnieuw naadloze eenmalige aanmelding voor uw tenant inschakelen, krijgen gebruikers niet de ervaring voor eenmalige aanmelding tot hun in de cache Kerberos-ticket, worden doorgaans geldig voor 10 uur zijn verlopen.
- Edge-browser-ondersteuning is niet beschikbaar.
- Als naadloze eenmalige aanmelding is gelukt, de gebruiker heeft geen selecteren **aangemeld blijven**. Vanwege dit probleem werken SharePoint en OneDrive toewijzing scenario's niet.
- Office-clients onder versie 16.0.8730.xxxx ondersteunen geen niet-interactief aanmelden met naadloze eenmalige aanmelding. Op deze clients, moeten gebruikers hun gebruikersnamen, maar geen wachtwoorden, aanmelden invoeren.
- Naadloze eenmalige aanmelding werkt niet in de privémodus Browse op Firefox.
- Naadloze eenmalige aanmelding werkt niet in Internet Explorer als uitgebreide beveiligde modus is ingeschakeld.
- Naadloze eenmalige aanmelding werkt niet op mobiele browsers op iOS en Android.
- Als u 30 of meer Active Directory-forests synchroniseert, kunt u naadloze eenmalige aanmelding met Azure AD Connect niet inschakelen. Als een tijdelijke oplossing kunt u [handmatig inschakelen](#manual-reset-of-azure-ad-seamless-sso) de functie op uw tenant.
- De URL van het Azure AD-service (https://autologon.microsoftazuread-sso.com) toe te voegen aan de zone Vertrouwde websites in plaats van de lokale intranetzone *voorkomen dat gebruikers aanmelden*.
- Het uitschakelen van het gebruik van de **RC4_HMAC_MD5** versleutelingstype voor Kerberos in uw Active Directory-instellingen naadloze eenmalige aanmelding wordt verbroken. In de Editor voor Groepsbeleidsbeheer hulpprogramma ervoor te zorgen dat de beleidswaarde voor **RC4_HMAC_MD5** onder **Computerconfiguratie -> Windows-instellingen -> Beveiligingsinstellingen -> lokaal beleid -> beveiligingsopties - > ' Netwerkbeveiliging: versleutelingstypen voor Kerberos toegestaan configureren '** is "ingeschakeld".

## <a name="check-status-of-feature"></a>Controleer de status van de functie

Zorg ervoor dat de functie naadloze eenmalige aanmelding nog steeds is **ingeschakeld** op uw tenant. U kunt de status controleren door te gaan naar de **Azure AD Connect** deelvenster in de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/).

![Azure Active Directory-beheercentrum: deelvenster Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

Klik in voor een overzicht van alle AD-forests die zijn ingeschakeld voor naadloze eenmalige aanmelding.

![Azure Active Directory-beheercentrum: deelvenster naadloze eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso13.png)

## <a name="sign-in-failure-reasons-in-the-azure-active-directory-admin-center-needs-a-premium-license"></a>Aanmelding mislukt redenen in het Azure Active Directory-beheercentrum (een Premium-licentie vereist)

Als uw tenant een Azure AD Premium-licentie die is gekoppeld heeft, kunt u ook zoeken op de [aanmeldingsactiviteiten rapport](../active-directory-reporting-activity-sign-ins.md) in de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/).

![Azure Active Directory-beheercentrum: aan de aanmeldingen rapport](./media/active-directory-aadconnect-sso/sso9.png)

Blader naar **Azure Active Directory** > **aanmeldingen** in de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/), en selecteer vervolgens de aanmeldingsactiviteiten van een specifieke gebruiker. Zoek naar de **SIGN-IN-FOUTCODE** veld. De waarde van dat veld worden toegewezen aan een reden voor fout en de oplossing met behulp van de volgende tabel:

|Foutcode voor aanmelding|Aanmelding mislukt reden|Oplossing
| --- | --- | ---
| 81001 | Kerberos-ticket van de gebruiker is te groot. | Reduceer groepslidmaatschappen van de gebruiker en probeer het opnieuw.
| 81002 | Kan geen Kerberos-ticket van de gebruiker te valideren. | Zie de [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81003 | Kan geen Kerberos-ticket van de gebruiker te valideren. | Zie de [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81004 | Poging tot Kerberos-verificatie is mislukt. | Zie de [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81008 | Kan geen Kerberos-ticket van de gebruiker te valideren. | Zie de [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81009 | Kan geen Kerberos-ticket van de gebruiker te valideren. | Zie de [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81010 | Naadloze eenmalige aanmelding is mislukt omdat het Kerberos-ticket van de gebruiker is verlopen of ongeldig is. | De gebruiker moet zich aanmelden via een apparaat lid van een domein binnen uw bedrijfsnetwerk.
| 81011 | Kan de gebruikersobject op basis van de informatie in de Kerberos-ticket van de gebruiker te vinden. | Azure AD Connect gebruiken om informatie van de gebruiker in Azure AD te synchroniseren.
| 81012 | De gebruiker probeert aan te melden bij Azure AD wijkt af van de gebruiker die is aangemeld bij het apparaat. | De gebruiker moet zich aanmelden via een ander apparaat.
| 81013 | Kan de gebruikersobject op basis van de informatie in de Kerberos-ticket van de gebruiker te vinden. |Azure AD Connect gebruiken om informatie van de gebruiker in Azure AD te synchroniseren. 

## <a name="troubleshooting-checklist"></a>Controlelijst voor probleemoplossing

Gebruik de volgende controlelijst naadloze eenmalige aanmelding problemen oplossen:

- Zorg ervoor dat de functie naadloze eenmalige aanmelding is ingeschakeld in Azure AD Connect. Als u de functie (bijvoorbeeld als gevolg van een geblokkeerde-poort) kan niet inschakelt, zorg ervoor dat u alle de [vereisten](active-directory-aadconnect-sso-quick-start.md#step-1-check-the-prerequisites) aanwezig.
- Als u beide hebt ingeschakeld [Azure AD Join](../active-directory-azureadjoin-overview.md) en naadloze eenmalige aanmelding op uw tenant, zorg ervoor dat het probleem niet met Azure AD Join. Eenmalige aanmelding van Azure AD Join heeft voorrang op naadloze eenmalige aanmelding als het apparaat geregistreerd bij Azure AD zowel domein is. Met eenmalige aanmelding van Azure AD Join ziet de gebruiker een tegel aanmelden met de tekst 'Verbonden voor Windows'.
- Zorg ervoor dat de URL van de Azure AD (https://autologon.microsoftazuread-sso.com) deel uit van de Intranet-beveiligingszone-instellingen van de gebruiker maakt.
- Zorg ervoor dat het bedrijfsapparaat is toegevoegd aan het Active Directory-domein.
- Zorg ervoor dat de gebruiker is aangemeld bij het apparaat via een Active Directory-domeinaccount.
- Zorg ervoor dat het gebruikersaccount is van een Active Directory-forest waarbij naadloze eenmalige aanmelding is ingesteld.
- Zorg ervoor dat het apparaat is verbonden met het bedrijfsnetwerk.
- Zorg ervoor dat de tijd van het apparaat is gesynchroniseerd met de tijd in Active Directory en de domeincontrollers en dat ze zijn binnen vijf minuten van elkaar.
- Lijst van de bestaande Kerberos-tickets op het apparaat met behulp van de `klist` opdracht vanaf een opdrachtprompt. Zorg ervoor dat de tickets uitgegeven voor de `AZUREADSSOACCT` computeraccount aanwezig zijn. Gebruikers Kerberos-tickets zijn doorgaans geldig voor 10 uur. Mogelijk hebt u verschillende instellingen in Active Directory.
- Als u uitgeschakeld en opnieuw ingeschakeld naadloze eenmalige aanmelding op uw tenant, krijgen gebruikers niet de ervaring voor eenmalige aanmelding tot hun in de cache Kerberos-ticket is verlopen.
- Bestaande Kerberos-tickets opschonen van het apparaat met behulp van de `klist purge` opdracht en probeer het opnieuw.
- Bekijk de logboeken van de console van de browser om te bepalen of er problemen met JavaScript zijn, (onder **hulpprogramma's voor ontwikkelaars**).
- Controleer de [domain controller logboeken](#domain-controller-logs).

### <a name="domain-controller-logs"></a>Domain controller Logboeken

Als u controle van geslaagde pogingen op uw domeincontroller en vervolgens telkens wanneer een gebruiker zich aanmeldt via naadloze eenmalige aanmelding inschakelt, wordt een vermelding voor de beveiliging wordt vastgelegd in het gebeurtenislogboek. U vindt deze beveiligingsgebeurtenissen met behulp van de volgende query. (Zoek naar gebeurtenis **4769** die zijn gekoppeld aan het computeraccount **AzureADSSOAcc$**.)

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Handmatig opnieuw instellen van de functie

Als het oplossen van problemen hebt gehad, kunt u de functie handmatig herstellen op uw tenant. Volg deze stappen op de lokale server waarop u Azure AD Connect wordt uitgevoerd.

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>Stap 1: De naadloze eenmalige aanmelding PowerShell-module importeren

1. Download en installeer de [Microsoft Online Services-aanmeldhulp](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Download en installeer de [64-bits Azure Active Directory-module voor Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Blader naar de `%programfiles%\Microsoft Azure Active Directory Connect` map.
4. De naadloze eenmalige aanmelding PowerShell-module importeren met behulp van deze opdracht: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-active-directory-forests-on-which-seamless-sso-has-been-enabled"></a>Stap 2: Haal de lijst met Active Directory-forests waarop naadloze eenmalige aanmelding is ingeschakeld

1. Voer de PowerShell als beheerder. In PowerShell, roept `New-AzureADSSOAuthenticationContext`. Voer desgevraagd uw globale tenantbeheerderreferenties.
2. Roep `Get-AzureADSSOStatus`. Met deze opdracht biedt u de lijst met Active Directory-forests (zoek op de lijst 'Domeinen') op die deze functie is ingeschakeld.

### <a name="step-3-disable-seamless-sso-for-each-active-directory-forest-where-youve-set-up-the-feature"></a>Stap 3: Naadloze eenmalige aanmelding voor elk Active Directory-forest waar u de functie hebt ingesteld uitschakelen

1. Roep `$creds = Get-Credential`. Wanneer u wordt gevraagd, typt u referenties voor de domeinbeheerder voor het beoogde Active Directory-forest.
2. Roep `Disable-AzureADSSOForest -OnPremCredentials $creds`. Deze opdracht verwijdert u de `AZUREADSSOACCT` computeraccount van de lokale domeincontroller voor dit specifieke Active Directory-forest.
3. Herhaal de voorgaande stappen voor elk Active Directory-forest waar u de functie hebt ingesteld.

### <a name="step-4-enable-seamless-sso-for-each-active-directory-forest"></a>Stap 4: Naadloze eenmalige aanmelding inschakelen voor elk Active Directory-forest

1. Roep `Enable-AzureADSSOForest`. Wanneer u wordt gevraagd, typt u referenties voor de domeinbeheerder voor het beoogde Active Directory-forest.
2. Herhaal de voorgaande stap voor elk Active Directory-forest waar u de functie instellen.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>Stap 5. De functie inschakelen voor uw tenant

Als u de functie op uw tenant, roepen `Enable-AzureADSSO` en voer **true** op de `Enable:` prompt.

---
title: 'Azure Active Directory Connect: Naadloze eenmalige aanmelding oplossen | Microsoft Docs'
description: In dit onderwerp wordt beschreven hoe u Azure Active Directory naadloze eenmalige aanmelding oplossen
services: active-directory
author: billmath
ms.reviewer: swkrish
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 09/24/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: d2e5f852678298a7916cf745ece6cd5711ee57ee
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198279"
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory naadloze eenmalige aanmelding oplossen

In dit artikel helpt u bij het oplossen van problemen informatie over veelvoorkomende problemen met betrekking tot Azure Active Directory (Azure AD) naadloze eenmalige aanmelding (naadloze eenmalige aanmelding).

## <a name="known-issues"></a>Bekende problemen

- In sommige gevallen, kan naadloze eenmalige aanmelding inschakelen maximaal 30 minuten duren.
- Als u uitschakelen en weer naadloze eenmalige aanmelding voor uw tenant inschakelen, krijgen gebruikers niet de ervaring voor eenmalige aanmelding tot hun in de cache Kerberos-tickets, worden doorgaans geldig voor 10 uur, zijn verlopen.
- Ondersteuning van Microsoft Edge-browser is niet beschikbaar.
- Als u naadloze eenmalige aanmelding is geslaagd, de gebruiker heeft geen selecteren **aangemeld blijven**. Vanwege dit gedrag [scenario's voor SharePoint en OneDrive toewijzing](https://support.microsoft.com/help/2616712/how-to-configure-and-to-troubleshoot-mapped-network-drives-that-connec) werken niet.
- Office 365 Win32-clients (Outlook, Word, Excel en andere) met versies 16.0.8730.xxxx en hoger worden ondersteund met behulp van een niet-interactieve stroom. Andere versies worden niet ondersteund. op deze versies voert gebruikers de gebruikersnamen, maar niet met wachtwoorden, om aan te melden. Voor OneDrive, hebt u activeren, de [OneDrive op de achtergrond config functie](https://techcommunity.microsoft.com/t5/Microsoft-OneDrive-Blog/Previews-for-Silent-Sync-Account-Configuration-and-Bandwidth/ba-p/120894) voor een ervaring voor op de achtergrond.
- Naadloze eenmalige aanmelding werkt niet in de privémodus bladeren in Firefox.
- Naadloze eenmalige aanmelding werkt niet in Internet Explorer als uitgebreide beveiligde modus is ingeschakeld.
- Naadloze eenmalige aanmelding werkt niet op mobiele browsers op iOS en Android.
- Als een gebruiker deel van te veel groepen in Active Directory uitmaakt, Kerberos-ticket van de gebruiker waarschijnlijk zal zijn te groot om te verwerken en dit zorgt voor naadloze eenmalige aanmelding mislukken. Azure AD-HTTPS-aanvragen kunnen headers met een maximale grootte van 50 KB; hebben Kerberos-tickets moeten kleiner zijn dan deze limiet voor andere Azure AD-artefacten (meestal 2-5 KB), zoals cookies. Onze aanbeveling is het verminderen van groepslidmaatschappen van gebruiker en probeer het opnieuw.
- Als u 30 of meer Active Directory-forests synchroniseren bent, kunt u naadloze eenmalige aanmelding via Azure AD Connect niet inschakelen. Als tijdelijke oplossing, kunt u [handmatig in te schakelen](#manual-reset-of-the-feature) de functie op uw tenant.
- Toevoegen van de URL van de Azure AD-service (https://autologon.microsoftazuread-sso.com) aan de zone Vertrouwde websites in plaats van de lokale intranetzone *wordt voorkomen dat gebruikers zich*.
- Naadloze eenmalige aanmelding gebruikt de **RC4_HMAC_MD5** versleutelingstype voor Kerberos. Uitschakelen van het gebruik van de **RC4_HMAC_MD5** versleutelingstype in uw Active Directory-instellingen, naadloze eenmalige aanmelding worden verbroken. In de Editor voor Groepsbeleidsbeheer hulpprogramma ervoor te zorgen dat de beleidswaarde voor **RC4_HMAC_MD5** onder **Computerconfiguratie > Windows-instellingen -> Beveiligingsinstellingen -> lokaal beleid Beveiligingsopties - -> > ' Netwerkbeveiliging: Voor Kerberos toegestane versleutelingstypen configureren'** is **ingeschakeld**. Bovendien naadloze eenmalige aanmelding kan geen andere versleutelingstypen gebruiken, dus zorg ervoor dat ze zijn **uitgeschakeld**.

## <a name="check-status-of-feature"></a>Controleer de status van de functie

Zorg ervoor dat de functie voor naadloze eenmalige aanmelding nog steeds is **ingeschakeld** op uw tenant. U kunt de status controleren door te gaan naar de **Azure AD Connect** deelvenster in de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/).

![Azure Active Directory-beheercentrum: Deelvenster in de Azure AD Connect](./media/tshoot-connect-sso/sso10.png)

Klik op om te zien van alle AD-forests die zijn ingeschakeld voor naadloze eenmalige aanmelding.

![Azure Active Directory-beheercentrum: Deelvenster met naadloze eenmalige aanmelding](./media/tshoot-connect-sso/sso13.png)

## <a name="sign-in-failure-reasons-in-the-azure-active-directory-admin-center-needs-a-premium-license"></a>Oorzaken voor het aanmelden mislukken in het Azure Active Directory-beheercentrum (een Premium-licentie vereist)

Als uw tenant een Azure AD Premium-licentie die is gekoppeld heeft, kunt u ook zoeken op de [rapport van aanmeldingsactiviteiten](../reports-monitoring/concept-sign-ins.md) in de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/).

![Azure Active Directory-beheercentrum: Aanmeldingenrapport](./media/tshoot-connect-sso/sso9.png)

Blader naar **Azure Active Directory** > **aanmeldingen** in de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/), en selecteer vervolgens de aanmeldingsactiviteiten voor een specifieke gebruiker. Zoek de **FOUTCODE voor aanmelding door** veld. De waarde van dat veld worden toegewezen aan een reden van fout en een oplossing met behulp van de volgende tabel:

|Foutcode voor aanmelding|Reden van fout-aanmelding|Oplossing
| --- | --- | ---
| 81001 | Kerberos-ticket van de gebruiker is te groot. | Reduceer het aantal groepslidmaatschappen van de gebruiker en probeer het opnieuw.
| 81002 | Kan niet valideren van het Kerberos-ticket van de gebruiker. | Zie de [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81003 | Kan niet valideren van het Kerberos-ticket van de gebruiker. | Zie de [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81004 | Poging tot Kerberos-verificatie is mislukt. | Zie de [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81008 | Kan niet valideren van het Kerberos-ticket van de gebruiker. | Zie de [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81009 | Kan niet valideren van het Kerberos-ticket van de gebruiker. | Zie de [controlelijst voor probleemoplossing](#troubleshooting-checklist).
| 81010 | Naadloze eenmalige aanmelding is mislukt omdat het Kerberos-ticket van de gebruiker is verlopen of ongeldig is. | De gebruiker moet zich aanmelden vanaf een apparaat lid is van een domein binnen uw bedrijfsnetwerk.
| 81011 | Kan niet vinden van het gebruikersobject op basis van de informatie in de Kerberos-ticket van de gebruiker. | Azure AD Connect gebruiken om gegevens van de gebruiker in Azure AD te synchroniseren.
| 81012 | De gebruiker zich aanmelden bij Azure AD verschilt van de gebruiker die is aangemeld bij het apparaat. | De gebruiker moet zich aanmelden vanaf een ander apparaat.
| 81013 | Kan niet vinden van het gebruikersobject op basis van de informatie in de Kerberos-ticket van de gebruiker. |Azure AD Connect gebruiken om gegevens van de gebruiker in Azure AD te synchroniseren. 

## <a name="troubleshooting-checklist"></a>Controlelijst voor probleemoplossing

Gebruik de volgende controlelijst om problemen met naadloze eenmalige aanmelding:

- Zorg ervoor dat de functie voor naadloze eenmalige aanmelding is ingeschakeld in Azure AD Connect. Als u de functie (bijvoorbeeld, als gevolg van een geblokkeerde-poort) kan niet inschakelt, zorgt u ervoor dat u beschikt over alle de [vereisten](how-to-connect-sso-quick-start.md#step-1-check-the-prerequisites) op locatie.
- Als u beide hebt ingeschakeld [Azure AD Join](../active-directory-azureadjoin-overview.md) en naadloze eenmalige aanmelding in uw tenant, zorg ervoor dat het probleem niet met Azure AD Join. Eenmalige aanmelding van Azure AD Join heeft voorrang op naadloze eenmalige aanmelding als het apparaat geregistreerd bij Azure AD en domein is. Met eenmalige aanmelding via de Azure AD Join ziet de gebruiker een tegel voor aanmelding met de tekst 'Verbonden aan Windows'.
- Zorg ervoor dat de Azure AD-URL (https://autologon.microsoftazuread-sso.com) maakt deel uit van de intranetinstellingen zone van de gebruiker.
- Zorg ervoor dat het bedrijfsapparaat is gekoppeld aan het Active Directory-domein. Het apparaat _niet_ moet [Azure AD join](../active-directory-azureadjoin-overview.md) voor naadloze eenmalige aanmelding om te werken.
- Zorg ervoor dat de gebruiker is aangemeld bij het apparaat wordt via Active Directory-domein-account.
- Zorg ervoor dat het gebruikersaccount is van een Active Directory-forest waarbij naadloze eenmalige aanmelding is ingesteld.
- Zorg ervoor dat het apparaat is verbonden met het bedrijfsnetwerk bevinden.
- Zorg ervoor dat de tijd van het apparaat is gesynchroniseerd met de tijd in Active Directory en de domeincontrollers en dat ze zich binnen vijf minuten van elkaar.
- Zorg ervoor dat de `AZUREADSSOACCT` computeraccount in elk AD-forest dat u wilt dat naadloze eenmalige aanmelding ingeschakeld is aanwezig en ingeschakeld. Als het computeraccount is verwijderd of ontbreekt, kunt u [PowerShell-cmdlets](#manual-reset-of-the-feature) om opnieuw te maken.
- Lijst van de bestaande Kerberos-tickets op het apparaat met behulp van de `klist` opdracht uit vanaf een opdrachtprompt. Zorg ervoor dat de tickets uitgegeven voor de `AZUREADSSOACCT` computeraccount aanwezig zijn. Gebruikers Kerberos-tickets zijn doorgaans geldig voor 10 uur. Mogelijk hebt u verschillende instellingen in Active Directory.
- Als u uitgeschakeld en opnieuw ingeschakeld naadloze eenmalige aanmelding in uw tenant, krijgen gebruikers niet de ervaring voor eenmalige aanmelding tot hun Kerberos-tickets in de cache zijn verlopen.
- Bestaande Kerberos-tickets van het apparaat verwijderen met behulp van de `klist purge` opdracht en probeer het opnieuw.
- Bekijk de logboeken van de console van de browser om te bepalen of er problemen met betrekking tot de JavaScript zijn, (onder **hulpprogramma's voor ontwikkelaars**).
- Controleer de [domain controller logboeken](#domain-controller-logs).

### <a name="domain-controller-logs"></a>Domain controller Logboeken

Als u de controle van geslaagde pogingen op uw domeincontroller en vervolgens telkens wanneer een gebruiker zich aanmeldt via naadloze eenmalige aanmelding inschakelt, wordt een vermelding security vastgelegd in het gebeurtenislogboek wordt geschreven. U vindt deze beveiligingsgebeurtenissen met behulp van de volgende query uit. (Zoek naar gebeurtenis **4769** die zijn gekoppeld aan het computeraccount **AzureADSSOAcc$**.)

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Handmatig opnieuw instellen van de functie

Als het oplossen van hebt gehad, kunt u de functie handmatig herstellen op uw tenant. Volg deze stappen op de on-premises server waarop u Azure AD Connect wordt uitgevoerd.

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>Stap 1: De naadloze eenmalige aanmelding PowerShell-module importeren

1. Eerst, download en installeer [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Blader naar de map `%programfiles%\Microsoft Azure Active Directory Connect`.
3. De naadloze eenmalige aanmelding PowerShell-module importeren met behulp van deze opdracht: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-active-directory-forests-on-which-seamless-sso-has-been-enabled"></a>Stap 2: De lijst met Active Directory-forests waarop naadloze eenmalige aanmelding is ingeschakeld

1. Voer PowerShell uit als beheerder. In PowerShell, roept u `New-AzureADSSOAuthenticationContext`. Desgevraagd geeft u de hoofdbeheerdersreferenties van uw tenant.
2. Bel `Get-AzureADSSOStatus`. Met deze opdracht geeft u de lijst met Active Directory-forests (zoek op de lijst met 'Domeinen') op die deze functie is ingeschakeld.

### <a name="step-3-disable-seamless-sso-for-each-active-directory-forest-where-youve-set-up-the-feature"></a>Stap 3: Naadloze eenmalige aanmelding uitschakelen voor elk Active Directory-forest waar u de functie hebt ingesteld

1. Bel `$creds = Get-Credential`. Wanneer u wordt gevraagd, typt u de referenties voor de domeinbeheerder voor het beoogde Active Directory-forest.

    >[!NOTE]
    >We gebruiken de domeinbeheerder username, opgegeven in de User Principal namen (UPN) (johndoe@contoso.com) indeling of domein gekwalificeerde sam-account,-naam (contoso\janjansen of contoso.com\johndoe), om te vinden van de beoogde AD-forest. Als u de domeinnaam van de gekwalificeerde sam-account gebruikt, gebruiken we het domeingedeelte van de gebruikersnaam voor [vinden van de domeincontroller van de beheerder van het domein met behulp van DNS](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx). Als u de UPN in plaats daarvan gebruiken we [vertaald in een domein gekwalificeerde sam-accountnaam](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa) voordat het zoeken naar de desbetreffende domeincontroller.

2. Bel `Disable-AzureADSSOForest -OnPremCredentials $creds`. Deze opdracht verwijdert u de `AZUREADSSOACCT` computeraccount van de lokale domeincontroller voor dit specifieke Active Directory-forest.
3. Herhaal de voorgaande stappen voor elk Active Directory-forest waar u de functie hebt ingesteld.

### <a name="step-4-enable-seamless-sso-for-each-active-directory-forest"></a>Stap 4: Naadloze eenmalige aanmelding inschakelen voor elk Active Directory-forest

1. Bel `Enable-AzureADSSOForest`. Wanneer u wordt gevraagd, typt u de referenties voor de domeinbeheerder voor het beoogde Active Directory-forest.

   >[!NOTE]
   >We gebruiken de domeinbeheerder username, opgegeven in de User Principal namen (UPN) (johndoe@contoso.com) indeling of domein gekwalificeerde sam-account,-naam (contoso\janjansen of contoso.com\johndoe), om te vinden van de beoogde AD-forest. Als u de domeinnaam van de gekwalificeerde sam-account gebruikt, gebruiken we het domeingedeelte van de gebruikersnaam voor [vinden van de domeincontroller van de beheerder van het domein met behulp van DNS](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx). Als u de UPN in plaats daarvan gebruiken we [vertaald in een domein gekwalificeerde sam-accountnaam](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa) voordat het zoeken naar de desbetreffende domeincontroller.

2. Herhaal de vorige stap voor elk Active Directory-forest waar u het instellen van de functie.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>Stap 5. De functie op uw tenant inschakelen

Als u wilt inschakelen op de functie op uw tenant, aanroepen `Enable-AzureADSSO -Enable $true`.

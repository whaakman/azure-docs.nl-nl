---
title: 'Azure Active Directory B2C: Gebruik reporting API samples en definities | Microsoft Docs'
description: Handleiding en voorbeelden op rapporten op Azure AD B2C-tenant ophalen van gebruikers, verificaties en multi-factor Authentication-oproepen
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mtillman
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: 6014301a026d60775634138cbdfe56bfa625508f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-the-reporting-api"></a>Toegang tot gebruiksrapporten in Azure AD B2C via de rapportage-API

Azure Active Directory B2C (Azure AD B2C) biedt verificatie op basis van de gebruiker zich aanmelden en Azure multi-factor Authentication. Verificatie is bestemd voor eindgebruikers van uw toepassing familie over id-providers. Als u het aantal gebruikers dat is geregistreerd in de tenant, de providers die ze gebruikt om u te registreren en het aantal authenticaties door type weet, kunt u beantwoorden van vragen als:
* Hoeveel gebruikers vanuit elk type id-provider (bijvoorbeeld een account van Microsoft of LinkedIn) hebt geregistreerd in de afgelopen tien dagen?
* Het aantal authenticaties met multi-factor Authentication zijn met succes voltooid in de afgelopen maand?
* Hoeveel aanmelding in gebaseerd verificaties zijn deze maand voltooid? Per dag? Per toepassing?
* Hoe kan ik schatting maken van de verwachte maandelijkse kosten van de activiteit van mijn Azure AD B2C-tenant?

Dit artikel is gericht op rapporten die zijn gekoppeld aan facturering activiteit, die is gebaseerd op het aantal gebruikers, factureerbare aanmelding in gebaseerd verificaties en multi-factor Authentication-oproepen.


## <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u de stappen in [vereisten voor toegang tot de Azure AD rapportage-API's](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/). Een toepassing maken en toegang verlenen verkrijgen van een geheim voor deze rechten aan uw Azure AD B2C-tenant-rapporten. *Script Bash* en *pythonscript* voorbeelden vindt u ook hier. 

## <a name="powershell-script"></a>PowerShell-script
Dit script wordt het maken van rapporten over gebruik van vier gedemonstreerd met behulp van de `TimeStamp` parameter en de `ApplicationId` filter.

```powershell
# This script will require the Web Application and permissions setup in Azure Active Directory

# Constants
$ClientID      = "your-client-application-id-here"  
$ClientSecret  = "your-client-application-secret-here"
$loginURL      = "https://login.microsoftonline.com"
$tenantdomain  = "your-b2c-tenant-domain.onmicrosoft.com"  
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body          = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth         = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
if ($oauth.access_token -ne $null) {
    $headerParams  = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    Write-host Data from the tenantUserCount report
    Write-host ====================================================
     # Returns a JSON document for the report
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the tenantUserCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?%24filter=TimeStamp+gt+2016-10-15&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCountSummary report
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=TimeStamp+gt+2016-09-20+and+TimeStamp+lt+2016-10-03&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with ApplicationId filter
    Write-host ====================================================
    # Returns a JSON document for the " " report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCountSummary
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCount?%24filter=TimeStamp+gt+2016-09-10+and+TimeStamp+lt+2016-10-04&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with ApplicationId filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
     Write-host $myReport.Content

} else {
    Write-Host "ERROR: No Access Token"
    }
```


## <a name="usage-report-definitions"></a>Gebruik rapportdefinities
* **tenantUserCount**: het aantal gebruikers in de tenant door type id-provider per dag in de afgelopen 30 dagen. (U kunt desgewenst een `TimeStamp` -filter biedt aantallen van de gebruiker van een opgegeven datum in de huidige datum). Het rapport bevat:
  * **TotalUserCount**: het aantal van alle gebruikersobjecten.
  * **OtherUserCount**: het aantal Azure Active Directory-gebruikers (geen Azure AD B2C-gebruikers).
  * **LocalUserCount**: het aantal Azure AD B2C-gebruikersaccounts gemaakt met de referenties die lokaal op de Azure AD B2C-tenant.

* **AlternateIdUserCount**: het aantal gebruikers van Azure AD B2C wordt geregistreerd bij externe id-providers (bijvoorbeeld, Facebook, een Microsoft-account of een andere Azure Active Directory-tenant, ook wel een `OrgId`).

* **b2cAuthenticationCountSummary**: samenvatting van de dagelijkse aantal factureerbare verificaties in de afgelopen 30 dagen door de dag en het type verificatiestroom.

* **b2cAuthenticationCount**: het aantal authenticaties binnen een periode. De standaardwaarde is de afgelopen 30 dagen.  (Optioneel: het begin en einde `TimeStamp` parameters definiëren een bepaalde periode.) De uitvoer bevat `StartTimeStamp` (vroegste datum van activiteiten voor deze tenant) en `EndTimeStamp` (laatste update).

* **b2cMfaRequestCountSummary**: samenvatting van de dagelijkse aantal multi-factor Authentication-oproepen per dag en het type (SMS of stem).


## <a name="limitations"></a>Beperkingen
Aantal gebruikersgegevens wordt elke 24-48 uur vernieuwd. Authenticaties worden meerdere keren per dag bijgewerkt. Wanneer u de `ApplicationId` filter, een antwoord leeg rapport kan zijn veroorzaakt door een van volgende voorwaarden:
  * De toepassings-ID bestaat niet in de tenant. Zorg ervoor dat deze juist is.
  * De toepassings-ID bestaat, maar er zijn geen gegevens gevonden in de rapportageperiode. Herzie de datum/tijd-parameters.


## <a name="next-steps"></a>Volgende stappen
### <a name="monthly-bill-estimates-for-azure-ad"></a>Maandelijkse factuur schattingen voor Azure AD
In combinatie met [de meest recente Azure AD B2C prijzen beschikbaar](https://azure.microsoft.com/pricing/details/active-directory-b2c/), u kunt schatten dagelijkse, wekelijkse en maandelijkse Azure-verbruik.  Een schatting is vooral nuttig wanneer u van plan bent om wijzigingen in gedrag die voor de totale kosten gevolgen mogelijk van de tenant. U kunt bekijken werkelijke kosten in uw [Azure-abonnement gekoppeld](active-directory-b2c-how-to-enable-billing.md).

### <a name="options-for-other-output-formats"></a>Opties voor andere uitvoerindelingen
De volgende code ziet u voorbeelden van uitvoer naar JSON, een lijst met waarden van naam en XML verzenden:
```powershell
# to output to JSON use following line in the PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# to output the content to a name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# to output the content in XML use the following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```

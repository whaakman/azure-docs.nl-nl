---
title: Gebruik reporting API voorbeelden en definities in Azure Active Directory B2C | Microsoft Docs
description: Handleiding en voorbeelden over het gebruik van rapporten in Azure AD B2C-tenant gebruikers, verificaties en multi-factor Authentication-oproepen.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 08/04/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 5ccd9ea36fcf20c56684ac5892a7fa32e0345633
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165996"
---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-the-reporting-api"></a>Toegang tot rapporten voor gebruik in Azure AD B2C via de rapportage-API

Azure Active Directory B2C (Azure AD B2C) biedt verificatie op basis van de gebruiker zich aanmelden en Azure multi-factor Authentication. Verificatie is bedoeld voor eindgebruikers van uw toepassing-familie over id-providers. Als u het aantal gebruikers die zijn geregistreerd in de tenant, de providers die ze gebruikt om u te registreren en het aantal verificaties per type weet, kunt u vragen beantwoorden zoals:
* Hoeveel gebruikers van elk type id-provider (bijvoorbeeld, een Microsoft- of LinkedIn-account) in de afgelopen 10 dagen hebben geregistreerd?
* Het aantal authenticaties met multi-factor Authentication in de afgelopen maand zijn voltooid?
* Het aantal verificaties sign-in-gebaseerd zijn deze maand voltooid? Per dag? Per toepassing?
* Hoe schat ik de verwachte maandelijkse kosten van de activiteit van mijn Azure AD B2C-tenant?

In dit artikel richt zich op rapporten die zijn gekoppeld aan facturering activiteit, die is gebaseerd op het aantal gebruikers, factureerbare sign-in-gebaseerd verificaties en multi-factor Authentication-oproepen.


## <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u de stappen in [vereisten voor toegang tot de rapportage-API's Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/). Maken van een toepassing, een geheim voor het verkrijgen en toegang verlenen rechten voor rapporten van uw Azure AD B2C-tenant. *Bash-script-* en *Python-script* voorbeelden vindt u ook hier. 

## <a name="powershell-script"></a>PowerShell-script
Dit script toont het maken van rapporten over gebruik van vier met behulp van de `TimeStamp` parameter en de `ApplicationId` filter.

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
* **tenantUserCount**: Het aantal gebruikers in de tenant door het type id-provider, per dag in de afgelopen 30 dagen. (U kunt desgewenst een `TimeStamp` -filter biedt een aantal van de gebruiker vanaf een opgegeven datum in de huidige datum). Het rapport bevat:
  * **TotalUserCount**: Het aantal alle gebruikersobjecten.
  * **OtherUserCount**: Het aantal Active Directory-gebruikers (niet Azure AD B2C-gebruikers).
  * **LocalUserCount**: Het aantal Azure AD B2C gebruikersaccounts die zijn gemaakt met de referenties van lokaal op de Azure AD B2C-tenant.

* **AlternateIdUserCount**: Het aantal Azure AD B2C-gebruikers die zijn geregistreerd bij externe id-providers (bijvoorbeeld Facebook, een Microsoft-account of een andere Azure Active Directory-tenant, ook wel een `OrgId`).

* **b2cAuthenticationCountSummary**: Samenvatting van het dagelijkse aantal factureerbare verificaties in de afgelopen 30 dagen, per dag en het type verificatie-stroom.

* **b2cAuthenticationCount**: Het aantal verificaties binnen een bepaalde periode. De standaardwaarde is de afgelopen 30 dagen.  (Optioneel: Het begin en einde `TimeStamp` parameters definiëren in een bepaalde periode.) De uitvoer bevat `StartTimeStamp` (vroegste datum van de activiteit voor deze tenant) en `EndTimeStamp` (meest recente update).

* **b2cMfaRequestCountSummary**: Samenvatting van het dagelijkse aantal multi-factor Authentication-oproepen per dag en type (SMS of spraak).


## <a name="limitations"></a>Beperkingen
Gegevens over het aantal gebruikers wordt elke 24 uur per dag op 48 uur vernieuwd. Verificaties zijn meerdere keren per dag bijgewerkt. Wanneer u de `ApplicationId` filter, een leeg rapport-antwoord kan worden veroorzaakt door een van de volgende voorwaarden:
  * De toepassings-ID bestaat niet in de tenant. Zorg ervoor dat deze juist is.
  * De toepassings-ID bestaat, maar er zijn geen gegevens gevonden in de rapportageperiode. Herzie de parameters van de datum/tijd.


## <a name="next-steps"></a>Volgende stappen
### <a name="monthly-bill-estimates-for-azure-ad"></a>Maandelijkse factuur maakt een schatting van voor Azure AD
In combinatie met [de meest recente Azure AD B2C prijzen beschikbaar](https://azure.microsoft.com/pricing/details/active-directory-b2c/), u kunt een schatting maken dagelijkse, wekelijkse en maandelijkse Azure-verbruik.  Een schatting is vooral nuttig wanneer u van plan bent om wijzigingen in de tenant-gedrag dat mogelijk invloed op de totale kosten. U kunt bekijken werkelijke kosten in uw [gekoppelde Azure-abonnement](active-directory-b2c-how-to-enable-billing.md).

### <a name="options-for-other-output-formats"></a>Opties voor andere uitvoerindeling op te geven
De volgende code ziet u voorbeelden van het verzenden van uitvoer in JSON, een lijst met de naam van waarde en XML:
```powershell
# to output to JSON use following line in the PowerShell sample
$myReport.Content | Out-File -FilePath name-your-file.json -Force

# to output the content to a name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# to output the content in XML use the following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```

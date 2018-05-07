---
title: 'Azure Active Directory B2C: De auditlogboeken voorbeelden en definities'
description: Handleiding en voorbeelden op toegang tot de Azure AD B2C-auditlogboeken
services: active-directory-b2c
author: sromeroz
manager: sasubram
ms.author: sezambra
ms.service: active-directory-b2c
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.openlocfilehash: 8cc48853b0677230c4e19df4f0ecd93ce88b119d
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Toegang tot Azure AD B2C-auditlogboeken

Azure Active Directory B2C (Azure AD B2C) verzendt controlelogboeken activiteit dat informatie bevat over B2C-resources, uitgegeven tokens en toegang als beheerder. Dit artikel bevat een kort overzicht van de informatie die beschikbaar is via controlelogboeken en instructies over de toegang tot deze gegevens voor uw Azure AD B2C-tenant.

> [!IMPORTANT]
> Controlelogboeken blijven alleen behouden voor zeven dagen. Plannen voor het downloaden en opslaan van uw logboeken met een van de methoden hieronder wordt weergegeven als u een langere bewaartermijn nodig. 

##<a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Overzicht van activiteiten die beschikbaar zijn in de categorie B2C van controlelogboeken
De **B2C** categorie van controlelogboeken bevat de volgende soorten activiteiten:
|Type activiteit |Beschrijving  |
|---------|---------|
|Autorisatie |Activiteiten met betrekking tot de autorisatie van een gebruiker toegang krijgen tot B2C bronnen (bijvoorbeeld een beheerder toegang tot een lijst met beleidsregels B2C)         |
|Directory |Activiteiten die zijn gerelateerd aan directorykenmerken die zijn opgehaald als een beheerder zich aanmeldt met de Azure Portal |
|Toepassing | CRUD-bewerkingen op B2C-toepassingen |
|Sleutel |CRUD-bewerkingen voor sleutels die zijn opgeslagen in een sleutelcontainer met B2C |
|Resource |CRUD-bewerkingen op B2C-resources (bijvoorbeeld, beleid en id-providers)
|Verificatie |Validatie van de gebruikersreferenties en uitgifte van tokens|

> [!NOTE]
> Voor een gebruiker object CRUD-activiteiten, raadpleegt u de **Core Directory** categorie.

##<a name="example-activity"></a>Voorbeeld van de activiteit
Het volgende voorbeeld ziet u de gegevens die zijn vastgelegd, wanneer een gebruiker zich met een externe id-provider aanmeldt: ![controlelogboeken - voorbeeld](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

##<a name="accessing-audit-logs-through-the-azure-portal"></a>Controlelogboeken openen via de Azure-Portal
1. Ga naar de [Azure Portal](https://portal.azure.com). Zorg ervoor dat u in uw B2C-directory.
2. Klik op **Azure Active Directory** in de werkbalk met Favorieten aan de linkerkant 
    
    ![Controlelogboeken - AAD-knop](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-aad.png)

1. Onder **activiteit**, klikt u op **controlelogboeken**

    ![Controlelogboeken - sectie Logs](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-section.png)

2. In de **categorie** dropbox, selecteer **B2C**
3. Klik op **toepassen**

    ![Controlelogboeken - categorie](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-category.png)

U ziet een lijst van activiteiten die zijn geregistreerd in de afgelopen zeven dagen. 
- Gebruik de **activiteit brontype** dropdown filteren op de activiteitstypen die hierboven worden beschreven
- Gebruik de **datumbereik** vervolgkeuzelijst voor het filteren van het datumbereik van de activiteiten weergegeven
- Als u op een specifieke rij in de lijst klikt, ziet een contextuele vak aan de rechterkant u extra kenmerken die zijn gekoppeld aan de activiteit
- Klik op **downloaden** voor het downloaden van de activiteiten als een csv-bestand

##<a name="accessing-audit-logs-through-the-azure-ad-reporting-api"></a>Controlelogboeken benaderen via de Azure AD rapportage-API
Auditlogboeken worden gepubliceerd naar de dezelfde pijplijn als andere activiteiten voor Azure Active Directory, zodat ze toegankelijk zijn via de [Azure Active Directory-rapportage API](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-api-audit-reference). 

###<a name="prerequisites"></a>Vereisten
Om te verifiëren met Azure AD rapportage-API moet u eerst een toepassing registreren. Zorg ervoor dat u de stappen in [vereisten voor toegang tot de Azure AD rapportage-API's](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).

###<a name="accesing-the-api"></a>Toegang tot de API
Voor het downloaden van de Azure AD B2C-controlelogboeken via de API, moet u voor het filteren van de logboeken naar de **B2C** categorie. Als u wilt filteren op categorie, gebruiken de querytekenreeksparameter bij het aanroepen van de Azure AD reporting API-eindpunt, zoals hieronder wordt weergegeven:

`https://graph.windows.net/your-b2c-tentant.onmicrosoft.com/activities/audit?api-version=beta&$filter=category eq 'B2C'`

###<a name="powershell-script"></a>PowerShell-script
Het volgende script geeft een voorbeeld van een query uitvoeren op de Azure AD rapportage-API en de resultaten opslaan als een JSON-bestand met behulp van PowerShell:

```powershell
# This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

# Constants
$ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
$ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
$loginURL       = "https://login.microsoftonline.com"     
$tenantdomain   = "your-b2c-tenant.onmicrosoft.com"       # AAD B2C Tenant; for example, contoso.onmicrosoft.com
$resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
$7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
Write-Output "Searching for events starting $7daysago"

# Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

# Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
if ($oauth.access_token -ne $null) {   
    $i=0
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    $url = 'https://graph.windows.net/' + $tenantdomain + '/activities/audit?api-version=beta&$filter=category eq ''B2C''and activityDate gt ' + $7daysago 

    # loop through each query page (1 through n)
    Do{
        # display each event on the console window
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output ($event | ConvertTo-Json)
        }

        # save the query page to an output file
        Write-Output "Save the output to a file audit$i.json"
        $myReport.Content | Out-File -FilePath audit$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)
} else {
    Write-Host "ERROR: No Access Token"
}
```


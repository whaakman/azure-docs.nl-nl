---
title: Auditlogboeken beschikbaar zijn voorbeelden en definities in Azure Active Directory B2C | Microsoft Docs
description: Handleiding en voorbeelden over de toegang tot de auditlogboeken van Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 56bd0dec5a829b055148668c4cad17055b2ed0e5
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54843669"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Toegang tot Azure AD B2C-auditlogboeken

Azure Active Directory B2C (Azure AD B2C) verzendt auditlogboeken dat activiteiteninformatie bevat over B2C-resources, uitgegeven tokens en toegang als beheerder. In dit artikel geeft een kort overzicht van de informatie is beschikbaar via de logboeken voor controle en instructies over hoe u toegang tot deze gegevens voor uw Azure AD B2C-tenant.

> [!IMPORTANT]
> Auditlogboeken worden alleen gedurende zeven dagen bewaard. Plannen om te downloaden en opslaan van uw logboeken met behulp van een van de methoden die hieronder wordt weergegeven als u een langere bewaartermijn nodig.

## <a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Overzicht van activiteiten in de B2C-categorie van de auditlogboeken beschikbaar
De **B2C** categorie in auditlogboeken bevat de volgende soorten activiteiten:
|Type activiteit |Description  |
|---------|---------|
|Autorisatie |Activiteiten met betrekking tot de autorisatie van een gebruiker voor toegang tot B2C resources (bijvoorbeeld een beheerder toegang tot een lijst met B2C-beleid)         |
|Directory |Activiteiten met betrekking tot directory-attributen opgehaald wanneer een beheerder zich aanmeldt met behulp van de Azure-Portal |
|Toepassing | CRUD-bewerkingen op B2C-toepassingen |
|Sleutel |CRUD-bewerkingen voor sleutels die zijn opgeslagen in de B2C-sleutelcontainer |
|Resource |CRUD-bewerkingen op B2C-resources (bijvoorbeeld beleidsregels en id-providers)
|Verificatie |Validatie van referenties van gebruiker en token-uitgifte|

> [!NOTE]
> Voor gebruikersactiviteiten object CRUD, raadpleegt u de **hoofddirectory** categorie.

## <a name="example-activity"></a>Voorbeeld van de activiteit
Het volgende voorbeeld ziet u de gegevens die zijn vastgelegd wanneer een gebruiker zich met een externe id-provider aanmeldt: ![Auditlogboeken - voorbeeld](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

## <a name="accessing-audit-logs-through-the-azure-portal"></a>Auditlogboeken openen via de Azure Portal
1. Ga naar de [Azure Portal](https://portal.azure.com). Zorg ervoor dat u zich in uw B2C-directory.
2. Klik op **Azure Active Directory** in de werkbalk met Favorieten aan de linkerkant
    
    ![Auditlogboeken - AAD-knop](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-aad.png)

1. Onder **activiteit**, klikt u op **auditlogboeken**

    ![Auditlogboeken - sectie van de logboeken](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-section.png)

2. In de **categorie** dropbox, selecteer **B2C**
3. Klik op **toepassen**

    ![Auditlogboeken - categorie](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-category.png)

Hier ziet u een lijst van activiteiten die zijn geregistreerd in de afgelopen zeven dagen.
- Gebruik de **resourcetype van activiteit** vervolgkeuzelijst om te filteren op de activiteitstypen die hierboven worden beschreven
- Gebruik de **datumbereik** vervolgkeuzelijst voor het filteren van het datumbereik van de activiteiten die worden weergegeven
- Als u op een specifieke rij in de lijst klikt, ziet een contextuele vak aan de rechterkant u aanvullende kenmerken die zijn gekoppeld aan de activiteit
- Klik op **downloaden** voor het downloaden van de activiteiten als een csv-bestand

## <a name="accessing-audit-logs-through-the-azure-ad-reporting-api"></a>Auditlogboeken openen via de Azure AD rapportage-API
Auditlogboeken worden gepubliceerd naar de dezelfde pijplijn als andere activiteiten voor Azure Active Directory, zodat ze kunnen worden geopend via de [Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference).

### <a name="prerequisites"></a>Vereisten
Als u wilt verifiëren met de Azure AD rapportage-API moet u eerst het registreren van een toepassing. Zorg ervoor dat u de stappen in [vereisten voor toegang tot de rapportage-API's Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).

### <a name="accessing-the-api"></a>Toegang tot de API
Voor het downloaden van de auditlogboeken van Azure AD B2C via de API, moet u de logboeken om te filteren de **B2C** categorie. Als u wilt filteren op categorie, gebruikt u de queryreeks-parameter bij het aanroepen van de Azure AD reporting API-eindpunt, zoals hieronder wordt weergegeven:

`https://graph.windows.net/your-b2c-tentant.onmicrosoft.com/activities/audit?api-version=beta&$filter=category eq 'B2C'`

### <a name="powershell-script"></a>PowerShell-script
Het volgende script geeft een voorbeeld van een query uitvoeren op de rapportage-API van Azure AD en de resultaten opslaan als een JSON-bestand met behulp van PowerShell:

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
    Do {
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

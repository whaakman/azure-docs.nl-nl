---
title: Auditlogboeken beschikbaar zijn voorbeelden en definities in Azure Active Directory B2C | Microsoft Docs
description: Handleiding en voorbeelden over de toegang tot de auditlogboeken van Azure AD B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 08/04/2017
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 216f5413ce3dae1f2d040643a30a4d7db4a879b8
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835403"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Toegang tot Azure AD B2C-auditlogboeken

Azure Active Directory B2C (Azure AD B2C) verzendt auditlogboeken dat activiteiteninformatie bevat over B2C-resources, uitgegeven tokens en toegang als beheerder. In dit artikel geeft een kort overzicht van de informatie is beschikbaar via de logboeken voor controle en instructies over hoe u toegang tot deze gegevens voor uw Azure AD B2C-tenant.

> [!IMPORTANT]
> Auditlogboeken worden alleen gedurende zeven dagen bewaard. Plannen om te downloaden en opslaan van uw logboeken met behulp van een van de methoden die hieronder wordt weergegeven als u een langere bewaartermijn nodig.

> [!NOTE]
> Er kan geen gebruikersaanmeldingen voor afzonderlijke Azure AD B2C-toepassingen onder de **gebruikers** sectie van de **Azure Active Directory** of **Azure AD B2C** blades. De aanmeldingen er gebruikersactiviteit worden weergegeven, maar kan niet worden gecorreleerd terug naar de B2C-toepassing die de gebruiker aangemeld bij. Hiervoor moet u de auditlogboeken gebruiken als verder uiteengezet in dit artikel.

## <a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Overzicht van activiteiten in de B2C-categorie van de auditlogboeken beschikbaar
De **B2C** categorie in auditlogboeken bevat de volgende soorten activiteiten:

|Type activiteit |Description  |
|---------|---------|
|Authorization |Activiteiten met betrekking tot de autorisatie van een gebruiker voor toegang tot B2C resources (bijvoorbeeld een beheerder toegang tot een lijst met B2C-beleid)         |
|Directory |Activiteiten met betrekking tot directory-attributen opgehaald wanneer een beheerder zich aanmeldt met behulp van de Azure portal |
|Toepassing | CRUD-bewerkingen op B2C-toepassingen |
|Sleutel |CRUD-bewerkingen voor sleutels die zijn opgeslagen in de B2C-sleutelcontainer |
|Resource |CRUD-bewerkingen op B2C-resources (bijvoorbeeld beleidsregels en id-providers)
|Authentication |Validatie van referenties van gebruiker en token-uitgifte|

> [!NOTE]
> Voor gebruikersactiviteiten object CRUD, raadpleegt u de **hoofddirectory** categorie.

## <a name="example-activity"></a>Voorbeeld van de activiteit
Het volgende voorbeeld ziet u de gegevens die zijn vastgelegd wanneer een gebruiker zich met een externe id-provider aanmeldt: ![Voorbeeld van Details van het auditlogboek activiteit-pagina in Azure portal](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

Het deelvenster met details van activiteit bevat de volgende relevante informatie:

|Section|Veld|Description|
|-------|-----|-----------|
| Activiteit | Name | Welke activiteit heeft plaatsgevonden. Bijvoorbeeld: "een id_token voor de toepassing uitgeven' (die eindigt de werkelijke gebruikers-aanmelding). |
| Gestart door (Actor) | ObjectId | De **Object-ID** van de B2C-toepassing die de gebruiker zich aanmeldt (deze id wordt niet weergegeven in de Azure-portal, maar het is bijvoorbeeld toegankelijk via de Graph API). |
| Gestart door (Actor) | Spn | De **toepassings-ID** van de B2C-toepassing die de gebruiker zich aanmeldt. |
| Doelen | ObjectId | De **Object-ID** van de gebruiker die zich aanmeldt. |
| Aanvullende details | TenantId | De **Tenant-ID** van de Azure AD B2C-tenant. |
| Aanvullende details | `PolicyId` | De **beleids-ID** van de gebruikersstroom (beleid) wordt gebruikt voor het ondertekenen van de gebruiker. |
| Aanvullende details | ApplicationId | De **toepassings-ID** van de B2C-toepassing die de gebruiker zich aanmeldt. |

## <a name="accessing-audit-logs-through-the-azure-portal"></a>Toegang tot de logboeken voor controle via Azure portal
1. Ga naar de [Azure Portal](https://portal.azure.com). Zorg ervoor dat u zich in uw B2C-directory.
2. Klik op **Azure Active Directory** in de werkbalk met Favorieten aan de linkerkant

    ![Azure Active Directory-knop in het linkermenu portal gemarkeerd](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-aad.png)

1. Onder **activiteit**, klikt u op **auditlogboeken**

    ![Audit Logs knop gemarkeerd in het gedeelte activiteit van menu](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-section.png)

2. In de **categorie** dropbox, selecteer **B2C**
3. Klik op **toepassen**

    ![Categorie en knop toepassen in het auditlogboek filter gemarkeerd](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-category.png)

Hier ziet u een lijst van activiteiten die zijn geregistreerd in de afgelopen zeven dagen.
- Gebruik de **resourcetype van activiteit** vervolgkeuzelijst om te filteren op de activiteitstypen die hierboven worden beschreven
- Gebruik de **datumbereik** vervolgkeuzelijst voor het filteren van het datumbereik van de activiteiten die worden weergegeven
- Als u op een specifieke rij in de lijst klikt, ziet een contextuele vak aan de rechterkant u aanvullende kenmerken die zijn gekoppeld aan de activiteit
- Klik op **downloaden** voor het downloaden van de activiteiten als een csv-bestand

> [!NOTE]
> U kunt ook de auditlogboeken bekijken door te navigeren naar **Azure AD B2C** in plaats van **Azure Active Directory** in de werkbalk met Favorieten aan de linkerkant. Onder **activiteiten**, klikt u op **auditlogboeken**, waar u de dezelfde logboeken met vergelijkbare filtermogelijkheden vinden.

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

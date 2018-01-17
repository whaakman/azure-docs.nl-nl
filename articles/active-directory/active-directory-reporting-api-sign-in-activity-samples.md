---
title: Azure Active Directory-aanmeldingsactiviteiten rapport API voorbeelden | Microsoft Docs
description: Hoe u aan de slag met de Azure Active Directory rapportage-API
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 158f121880dbc1fc1495c8599367502ffd93ce20
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory aanmeldingsactiviteiten rapport API-voorbeelden
In dit onderwerp maakt deel uit van een verzameling van onderwerpen over de Azure Active Directory rapportage-API.  
Rapportage van Azure AD biedt u een API waarmee u toegang tot gegevens van aanmeldingsactiviteiten met code of gerelateerde hulpprogramma's.  
Het bereik van dit onderwerp is om te voorzien van voorbeeldcode voor de **aanmelden activiteit API**.

Zie:

* [Controlelogboeken](active-directory-reporting-azure-portal.md#activity-reports) voor meer conceptuele informatie
* [Aan de slag met Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) voor meer informatie over de rapportage-API.


## <a name="prerequisites"></a>Vereisten
Voordat u de voorbeelden in dit onderwerp kunt, u moet voltooien de [vereisten voor toegang tot de Azure AD rapportage-API](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>PowerShell-script
    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.microsoftonline.com/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"

    $i=0

    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {

        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Uitvoeren van script
Zodra u klaar bent met het script bewerken, uitvoeren en controleren of de verwachte gegevens van de Audit rapport Logboeken wordt geretourneerd.

Het script retourneert de uitvoer van het rapport aanmelden in JSON-indeling. Maakt ook een `SigninActivities.json` bestand met dezelfde uitvoer. U kunt experimenteren door het script als u gegevens van andere rapporten en uitcommentarieer de output-indelingen u niet hoeft wilt wijzigen.

## <a name="next-steps"></a>Volgende stappen
* Wilt u de voorbeelden in dit onderwerp aanpassen? Bekijk de [Azure Active Directory aanmeldingsactiviteiten API-referentiemateriaal](active-directory-reporting-api-sign-in-activity-reference.md). 
* Als u wilt dat een volledig overzicht van het gebruik van de Azure Active Directory rapportage-API. Raadpleeg [aan de slag met de Azure Active Directory-rapportage API](active-directory-reporting-api-getting-started.md).
* Als u meer informatie over Azure Active Directory-rapportage wilt, raadpleegt u de [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  


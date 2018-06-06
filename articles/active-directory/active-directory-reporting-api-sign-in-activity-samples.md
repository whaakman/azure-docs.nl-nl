---
title: Azure Active Directory-aanmeldingsactiviteiten rapport API voorbeelden | Microsoft Docs
description: Hoe u aan de slag met de Azure Active Directory rapportage-API
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/31/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: 466755d7d1cc7fbf4006826ac849b74ba306bae9
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34698574"
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory aanmeldingsactiviteiten rapport API-voorbeelden
In dit artikel maakt deel uit van een verzameling van artikelen over de Azure Active Directory rapportage-API.  
Rapportage van Azure AD biedt u een API waarmee u toegang tot gegevens van aanmeldingsactiviteiten met code of gerelateerde hulpprogramma's.  
Het bereik van dit artikel is om te voorzien van voorbeeldcode voor de **aanmelden activiteit API**.

Zie:

* [Controlelogboeken](active-directory-reporting-azure-portal.md#activity-reports) voor meer conceptuele informatie
* [Aan de slag met Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) voor meer informatie over de rapportage-API.


## <a name="prerequisites"></a>Vereisten
Voordat u de voorbeelden in dit artikel kunt, u moet voltooien de [vereisten voor toegang tot de Azure AD rapportage-API](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>PowerShell-script

```powershell

# This script will require the Web Application and permissions setup in Azure Active Directory
$clientID       = "<appid>"             # ApplicationId
$clientSecret   = "<key>"         # Should be a ~44 character string insert your info here
$loginURL       = "https://login.windows.net/"
$tenantdomain   = "<domain>"            # For example, contoso.onmicrosoft.com
$msgraphEndpoint = "https://graph.microsoft.com"
$countOfSignInDocsToBeSavedInAFile = 2000
    
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$msgraphEndpoint;client_id=$clientID;client_secret=$clientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
    
if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    
    $url = "$msgraphEndpoint/beta/auditLogs/signIns"
    Write-Output "Fetching data using Uri: $url"
    $i=0
    $docCount=0
    Do{
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        $jsonReport = ($myReport.Content | ConvertFrom-Json).value
        $fetchedRecordCount = $jsonReport.Count
        $docCount = $docCount + $fetchedRecordCount
        $totalFetchedRecordCount = $totalFetchedRecordCount + $fetchedRecordCount
        Write-Output "Fetched $fetchedRecordCount records and saved into SignIns$i.json"
        if($docCount -le $countOfSignInDocsToBeSavedInAFile)
        {
            $myReport.Content | Out-File -FilePath SignIns$i.json -append  -Force       
        }
        else
        {           
            $docCount=0
            $i = $i+1
        }
            
        #Get url from next link
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'         
    }while($url -ne $null)
    Write-Output "Total Fetched record count is : $totalFetchedRecordCount"
                
} else {
    Write-Host "ERROR: No Access Token"
}

```




## <a name="executing-the-script"></a>Uitvoeren van script
Zodra u klaar bent met het script bewerken, uitvoeren en controleren of de verwachte gegevens van de aanmeldingspagina rapport Logboeken wordt geretourneerd.

Het script retourneert de uitvoer van het rapport aanmelden in JSON-indeling. Maakt ook een `SignIns.json` bestand met dezelfde uitvoer. U kunt experimenteren door het script als u gegevens van andere rapporten en uitcommentarieer de output-indelingen u niet hoeft wilt wijzigen.

## <a name="next-steps"></a>Volgende stappen
* Wilt u de voorbeelden in dit artikel aanpassen? Bekijk de [Azure Active Directory aanmeldingsactiviteiten API-referentiemateriaal](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin). 
* Als u wilt dat een volledig overzicht van het gebruik van de Azure Active Directory rapportage-API. Raadpleeg [aan de slag met de Azure Active Directory-rapportage API](active-directory-reporting-api-getting-started.md).
* Als u meer informatie over Azure Active Directory-rapportage wilt, raadpleegt u de [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  


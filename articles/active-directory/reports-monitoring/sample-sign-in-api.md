---
title: Voorbeelden van de rapportage-API voor aanmeldingsactiviteiten in Azure Active Directory | Microsoft Docs
description: Aan de slag gaan met de rapportage-API van Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 05/31/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: b02c0ac482c13868606674a097a84976780b7ef6
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "41917676"
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Voorbeelden van de rapportage-API voor aanmeldingsactiviteiten in Azure Active Directory
Dit artikel maakt onderdeel uit van een verzameling artikelen over de rapportage-API van Azure Active Directory.  
Azure AD-rapportage biedt een API waarmee u toegang krijgt tot gegevens over aanmeldingsactiviteiten met behulp van code of gerelateerde hulpprogramma’s.  
De bedoeling van dit artikel is om u voorbeeldcode te bieden voor de **API voor aanmeldingsactiviteiten**.

Zie:

* [Auditlogboeken](overview-reports.md#activity-reports) voor meer conceptuele informatie
* [Aan de slag met de rapportage-API van Azure Active Directory](concept-reporting-api.md) voor meer informatie over de rapportage-API.


## <a name="prerequisites"></a>Vereisten
Voordat u de voorbeelden in dit artikel kunt gebruiken, moet u voldoen aan de [vereisten voor toegang tot de rapportage-API van Azure AD](howto-configure-prerequisites-for-reporting-api.md).  

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




## <a name="executing-the-script"></a>Het script uitvoeren
Zodra u klaar bent met het bewerken van het script, voert u het uit en controleert u of de verwachte gegevens uit het rapport voor de aanmeldingslogboeken worden geretourneerd.

Met het script worden uitvoergegevens uit het aanmeldingsrapport geretourneerd in de JSON-indeling. Er wordt ook een `SignIns.json`-bestand gemaakt met dezelfde uitvoergegevens. U kunt experimenteren door het script te wijzigen zodat gegevens uit andere rapporten worden geretourneerd, en onnodige uitvoergegevens vervolgens uitcommentariëren.

## <a name="next-steps"></a>Volgende stappen
* Wilt u de voorbeelden in dit artikel aanpassen? Raadpleeg het [referentiemateriaal voor de API voor aanmeldingsactiviteiten in Azure Active Directory](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin). 
* Zie [Aan de slag met de rapportage-API van Azure Active Directory](concept-reporting-api.md) voor een volledig overzicht van het gebruik van de rapportage-API van Azure Active Directory.
* Zie de [handleiding voor Azure Active Directory-rapportage](overview-reports.md) voor meer informatie over Azure Active Directory-rapportage.  


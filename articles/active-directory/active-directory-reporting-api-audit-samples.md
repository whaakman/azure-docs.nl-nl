---
title: Voorbeelden van de controle-API voor Azure Active Directory-rapportage | Microsoft Docs
description: Aan de slag gaan met de rapportage-API van Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: de8b8ec3-49b3-4aa8-93fb-e38f52c99743
ms.service: active-directory
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/30/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 240095b64f4b7b37a71f84ce8d7a3521e0ab7c03
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36223882"
---
# <a name="azure-active-directory-reporting-audit-api-samples"></a>Voorbeelden van de controle-API voor Azure Active Directory-rapportage
Dit artikel maakt onderdeel uit van een verzameling artikelen over de rapportage-API van Azure Active Directory.  
Azure AD-rapportage biedt een API waarmee u toegang krijgt tot controlegegevens met behulp van code of gerelateerde hulpprogramma’s.
De bedoeling van dit artikel is om u voorbeeldcode te bieden voor de **controle-API**.

Zie:

* [Auditlogboeken](active-directory-reporting-azure-portal.md#activity-reports) voor meer conceptuele informatie
* [Aan de slag met de rapportage-API van Azure Active Directory](active-directory-reporting-api-getting-started.md) voor meer informatie over de rapportage-API.

Neem contact op met [Help voor AAD-rapportage](mailto:aadreportinghelp@microsoft.com) als u vragen, problemen of feedback hebt.


## <a name="prerequisites"></a>Vereisten
Voordat u de voorbeelden in dit artikel kunt gebruiken, moet u voldoen aan de [vereisten voor toegang tot de rapportage-API van Azure AD](active-directory-reporting-api-prerequisites.md).  

## <a name="known-issue"></a>Bekend probleem
App-verificatie werkt niet als de tenant zich in de regio EU bevindt. Gebruik als tijdelijke oplossing Gebruikersverificatie voor toegang tot de controle-API, totdat het probleem is opgelost. 

## <a name="powershell-script"></a>PowerShell-script


```powershell

# This script will require the Web Application and permissions setup in Azure Active Directory
$clientID       = "<appid>"             # ApplicationId
$clientSecret   = "<key>"         # Should be a ~44 character string insert your info here
$loginURL       = "https://login.windows.net/"
$tenantdomain   = "<domain>"            # For example, contoso.onmicrosoft.com
$msgraphEndpoint = "https://graph.microsoft.com"
$countOfAuditDocsToBeSavedInAFile = 2000
    
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$msgraphEndpoint;client_id=$clientID;client_secret=$clientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
    
if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    
    $url = "$msgraphEndpoint/beta/auditLogs/directoryAudits"
    Write-Output "Fetching data using Uri: $url"
    $i=0
    $docCount=0
    Do{
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        $jsonReport = ($myReport.Content | ConvertFrom-Json).value
        $fetchedRecordCount = $jsonReport.Count
        $docCount = $docCount + $fetchedRecordCount
        $totalFetchedRecordCount = $totalFetchedRecordCount + $fetchedRecordCount
        Write-Output "Fetched $fetchedRecordCount records and saved into Audits$i.json"
        if($docCount -le $countOfAuditDocsToBeSavedInAFile)
        {
            $myReport.Content | Out-File -FilePath Audits$i.json -append  -Force        
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

### <a name="executing-the-powershell-script"></a>Het PowerShell-script uitvoeren
Zodra u klaar bent met het bewerken van het script, voert u het uit en controleert u of de verwachte gegevens uit het rapport voor de auditlogboeken worden geretourneerd.

Met het script worden uitvoergegevens uit het controlerapport geretourneerd in de JSON-indeling. Er wordt ook een `Audits.json`-bestand gemaakt met dezelfde uitvoergegevens. U kunt experimenteren door het script te wijzigen zodat gegevens uit andere rapporten worden geretourneerd, en onnodige uitvoergegevens vervolgens uitcommentariëren.





## <a name="next-steps"></a>Volgende stappen
* Wilt u de voorbeelden in dit artikel aanpassen? Raadpleeg het [referentiemateriaal voor de controle-API voor Azure Active Directory](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit). 
* Zie [Aan de slag met de rapportage-API van Azure Active Directory](active-directory-reporting-api-getting-started.md) voor een volledig overzicht van het gebruik van de rapportage-API van Azure Active Directory.
* Zie de [handleiding voor Azure Active Directory-rapportage](active-directory-reporting-guide.md) voor meer informatie over Azure Active Directory-rapportage.  


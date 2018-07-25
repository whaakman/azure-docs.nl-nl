---
title: Interpreteren van het schema van de inloggen in Azure Active Directory in Azure Monitor (preview) | Microsoft Docs
description: Beschrijf de Azure AD-aanmelding in een logboek-schema voor gebruik in Azure Monitor (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 95153a4661f030824c9b85c10c5b4b1731ff8a91
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239882"
---
# <a name="interpret-the-azure-active-directory-sign-in-logs-schema-in-azure-monitor-preview"></a>Het schema van de logboeken voor aanmelding bij Azure Active Directory in Azure Monitor (preview) interpreteren

Dit artikel beschrijft het schema van de inloggen in Azure AD in Azure Monitor. De meeste van de informatie met betrekking tot aanmeldingen verstrekt onder de *eigenschappen* kenmerk van het object records.

```json
{ 
    "records": [ 
    { 
        "time": "2018-05-16T16:09:58.4634578Z", 
        "resourceId": null, 
        "operationName": "Sign-in activity", 
        "operationVersion": "1.0", 
        "category": "SignIn", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "50140", 
        "resultSignature": "None", 
        "resultDescription": "Other", 
        "durationMs": 0, 
        "callerIpAddress": "167.220.0.158", 
        "correlationId": "13e19598-e040-487f-bd32-d38a2cd75d9a", 
        "identity": "arvind harinder", 
        "Level": 4, 
        "location": "US", 
        "properties": { 
            "id": "0782c515-08b6-4029-a65c-29d9a3d20800", 
            "createdDateTime": "2018-05-16T16:09:58.4634578+00:00", 
            "userDisplayName": "Arvind Harinder", 
            "userPrincipalName": "ah@wingtiptoysonline.onmicrosoft.com", 
            "userId": "5b9f356d-9592-42fd-9ec4-d70963909534", 
            "appId": "c44b4083-3bb0-49c1-b47d-974e53cbdf3c", 
            "appDisplayName": "Azure Portal", 
            "ipAddress": "167.220.0.158", 
            "status": { 
                "errorCode": 50140, 
                "failureReason": "Other" 
            }, 
            "clientAppUsed": "Browser", 
            "deviceDetail": { 
                "operatingSystem": "Windows 10", 
                "browser": "Chrome 66.0.3359" 
            }, 
            "location": { 
                "city": "Sammamish", 
                "state": "Washington", 
                "countryOrRegion": "US", 
                "geoCoordinates": { 
                    "latitude": 47.66630935668945, 
                    "longitude": -122.09821319580078 
                } 
            }, 
            "correlationId": "13e19598-e040-487f-bd32-d38a2cd75d9a", 
            "conditionalAccessStatus": 2, 
            "conditionalAccessPolicies": [ 
            { 
                "id": "de7e60eb-ed89-4d73-8205-2227def6b7c9", 
                "displayName": "[billg] SharePoint limited access policy", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "7412a2d8-cbb1-4f1c-96cf-8410b4b8b37b", 
                "displayName": "[BillG] AIP MFA Policy", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "727ed8ea-059d-4d8f-aba5-c1dc500e8b06", 
                "displayName": "[billg] mfa for mail", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "6701123a-b4c6-48af-8565-565c8bf7cabc", 
                "displayName": "Medium signin risk block", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "fbafa2da-cf7f-4ec3-83cf-281188e53f76", 
                "displayName": "Require MFA for admins [Ignite talk] ", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "15339054-709d-4e06-a9ec-342bf043ea56", 
                "displayName": "Enhanced proofing for Azure portal [Ignite talk]", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "2ff9436f-bc72-4ce6-b17e-e7e51153146e", 
                "displayName": "[calebb] AIP policy", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "46ab586b-9447-4847-a889-e60705d96e56", 
                "displayName": "Test policy, OR", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "ceb6e17e-a5d0-4b3a-a150-6c2be2d5b0e9", 
                "displayName": "mm policy with Duo", 
                "enforcedGrantControls": [ 
                    "Require Duo Mfa" 
                ], 
            "enforcedSessionControls": [], 
            "result": 2 
            }, 
            ], 
            "isRisky": false 
            } 
        } 
    } 
```
| Veldnaam | Beschrijving |
|------------|-------------|
| Time | Datum en tijd in UTC |
| ResourceId | Deze waarde is niet-toegewezen en u kunt dit veld veilig negeren.  |
| OperationName | Voor aanmeldingen, is deze waarde altijd *aanmeldingsactiviteiten* |
| operationVersion | De REST-API-versie door de client aangevraagde |
| Categorie | Voor aanmeldingen, is dit altijd *aanmelding* | 
| TenantId | Tenant-Guid die is gekoppeld aan de logboeken |
| resultType | Kan het resultaat van de bewerking aanmelding zijn *succes* of *fout* | 
| resultSignature | Bevat de foutcode, indien van toepassing, voor de bewerking aanmelden |
| ResultDescription | Geeft de beschrijving van de fout voor de bewerking aanmelden |
| durationMs |  Deze waarde is niet-toegewezen en u kunt dit veld veilig negeren.|
| callerIpAddress | IP-adres van de client die de aanvraag heeft ingediend | 
| CorrelationId | Optionele Guid die is doorgegeven door de client. Deze waarde kan helpen bij elkaar te vergelijken client-side-bewerkingen met server-side-bewerkingen en is handig bij het traceren van logboeken met betrekking alle services tot. |
| Identiteit | De identiteit van het token dat is opgegeven bij het maken van de aanvraag. Mag een gebruikersaccount, systeem-account of service-principal. |
| Niveau | Geeft het type van het bericht. Voor controle, is het altijd *ter informatie* |
| Locatie | Bevat de locatie van de aanmeldingsactiviteiten |
| Eigenschappen | Geeft een lijst van alle eigenschappen die zijn gekoppeld aan aanmeldingen. Lees voor meer informatie de [MS Graph API Reference](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/signin). Dit schema maakt gebruik van de kenmerknamen zoals in de resource-in voor de leesbaarheid.

## <a name="next-steps"></a>Volgende stappen

* [Audit logs schema in Azure monitor interpreteren](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Meer informatie over hoe Azure diagnostische logboeken](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
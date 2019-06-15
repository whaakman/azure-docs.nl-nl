---
title: Azure Active Directory-aanmelding log-schema in Azure Monitor | Microsoft Docs
description: Beschrijf de Azure AD-aanmelding in een logboek-schema voor gebruik in Azure Monitor
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: a8ac6c56dca100ea9836158f46881c4eb12213e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60285202"
---
# <a name="interpret-the-azure-ad-sign-in-logs-schema-in-azure-monitor"></a>Het schema van de logboeken voor aanmelding bij Azure AD in Azure Monitor interpreteren

Dit artikel beschrijft het schema van de inloggen in Azure Active Directory (Azure AD) in Azure Monitor. De meeste informatie met betrekking tot aanmeldingen verstrekt onder de *eigenschappen* kenmerk van de `records` object.


```json
{ 
    "time": "2019-03-12T16:02:15.5522137Z", 
    "resourceId": "/tenants/<TENANT ID>/providers/Microsoft.aadiam",
    "operationName": "Sign-in activity", 
    "operationVersion": "1.0", 
    "category": "SignInLogs", 
    "tenantId": "<TENANT ID>", 
    "resultType": "50140", 
    "resultSignature": "None", 
    "resultDescription": "This error occurred due to 'Keep me signed in' interrupt when the user was signing-in.", 
    "durationMs": 0, 
    "callerIpAddress": "<CALLER IP ADDRESS>", 
    "correlationId": "a75a10bd-c126-486b-9742-c03110d36262", 
    "identity": "Timothy Perkins", 
    "Level": 4, 
    "location": "US", 
    "properties": 
        {
            "id":"0231f922-93fa-4005-bb11-b344eca03c01",
            "createdDateTime":"2019-03-12T16:02:15.5522137+00:00",
            "userDisplayName":"Timothy Perkins",
            "userPrincipalName":"<USER PRINCIPAL NAME>",
            "userId":"<USER ID>",
            "appId":"<APPLICATION ID>",
            "appDisplayName":"Azure Portal",
            "ipAddress":"<IP ADDRESS>",
            "status":
            {
                "errorCode":50140,
                "failureReason":"This error occurred due to 'Keep me signed in' interrupt when the user was signing-in."
            },
            "clientAppUsed":"Browser",
            "deviceDetail":
            {
                "operatingSystem":"Windows 10",
                "browser":"Chrome 72.0.3626"
            },
            "location":
                {
                    "city":"Bellevue",
                    "state":"Washington",
                    "countryOrRegion":"US",
                    "geoCoordinates":
                    {
                        "latitude":45,
                        "longitude":122
                    }
                },
            "correlationId":"a75a10bd-c126-486b-9742-c03110d36262",
            "conditionalAccessStatus":"notApplied",
            "appliedConditionalAccessPolicies":
            [
                {
                    "id":"ae11ffaa-9879-44e0-972c-7538fd5c4d1a",
                    "displayName":"Hr app access policy",
                    "enforcedGrantControls":
                    [
                        "Mfa"
                    ],
                    "enforcedSessionControls":
                    [
                    ],
                    "result":"notApplied"
                },
                {
                    "id":"b915a70b-2eee-47b6-85b6-ff4f4a66256d",
                    "displayName":"MFA for all but global support access",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
                {
                    "id":"830f27fa-67a8-461f-8791-635b7225caf1",
                    "displayName":"Header Based Application Control",
                    "enforcedGrantControls":["Mfa"],
                    "enforcedSessionControls":[],
                    "result":"notApplied"
                },
                {
                    "id":"8ed8d7f7-0a2e-437b-b512-9e47bed562e6",
                    "displayName":"MFA for everyones",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
                {
                    "id":"52924e0f-798b-4afd-8c42-49055c7d6395",
                    "displayName":"Device compliant",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
             ],
            "isInteractive":true,
            "tokenIssuerType":"AzureAD",
            "authenticationProcessingDetails":[],
            "networkLocationDetails":[],
            "processingTimeInMilliseconds":0,
            "riskDetail":"hidden",
            "riskLevelAggregated":"hidden",
            "riskLevelDuringSignIn":"hidden",
            "riskState":"none",
            "riskEventTypes":[],
            "resourceDisplayName":"windows azure service management api",
            "resourceId":"797f4846-ba00-4fd7-ba43-dac1f8f63013",
            "authenticationMethodsUsed":[]
        }
}
```


## <a name="field-descriptions"></a>Beschrijving van veld

| Veldnaam | Description |
|------------|-------------|
| Time | De datum en tijd in UTC. |
| ResourceId | Deze waarde is niet-toegewezen en u kunt dit veld veilig negeren.  |
| OperationName | Voor aanmeldingen, is deze waarde altijd *aanmeldingsactiviteiten*. |
| OperationVersion | De REST-API-versie die aangevraagd door de client. |
| Category | Voor aanmeldingen, is deze waarde altijd *SignIn*. | 
| TenantId | De tenant GUID die is gekoppeld aan de logboeken. |
| ResultType | Kan het resultaat van de bewerking aanmelding zijn *succes* of *fout*. | 
| resultSignature | Bevat de foutcode, indien aanwezig, voor het opnieuw aanmelden. |
| ResultDescription | Geeft de foutbeschrijving voor de bewerking aanmelden. |
| DurationMs |  Deze waarde is niet-toegewezen en u kunt dit veld veilig negeren.|
| CallerIpAddress | Het IP-adres van de client die de aanvraag heeft ingediend. | 
| CorrelationId | De optionele GUID die doorgegeven door de client. Deze waarde kan helpen bij elkaar te vergelijken client-side-bewerkingen met server-side-bewerkingen en dit is handig wanneer u logboeken met betrekking services tot bijhoudt. |
| Identiteit | De identiteit van het token dat is opgegeven wanneer u de aanvraag heeft ingediend. Een gebruikersaccount, systeem-account of service-principal kan het zijn. |
| Niveau | Geeft het type van het bericht. Voor controle, is het altijd *ter informatie*. |
| Locatie | Bevat de locatie van de activiteit aanmelden. |
| Properties | Geeft een lijst van alle eigenschappen die gekoppeld aan aanmeldingen zijn. Zie voor meer informatie, [Microsoft Graph API Reference](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin). Dit schema maakt gebruik van de namen van dezelfde als de resource-in voor de leesbaarheid.

## <a name="next-steps"></a>Volgende stappen

* [Interpret audit logs schema in Azure Monitor](reference-azure-monitor-audit-log-schema.md) (Auditlogboekenschema interpreteren in Azure Monitor)
* [Lees meer over de diagnostische logboeken in Azure](../../azure-monitor/platform/diagnostic-logs-overview.md)
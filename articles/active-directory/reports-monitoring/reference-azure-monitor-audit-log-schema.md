---
title: Interpreteren van de Azure Active Directory-schema audit log in Azure Monitor (preview) | Microsoft Docs
description: Beschrijf de Azure AD audit log-schema voor gebruik in Azure Monitor (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 12/14/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 0f6a5a345d5b0759e7dcb396b60d7466ae99cb16
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179596"
---
# <a name="interpret-the-azure-ad-audit-logs-schema-in-azure-monitor-preview"></a>Interpreteren van de Azure AD audit logs schema in Azure Monitor (preview)

Dit artikel beschrijft de Azure Active Directory (Azure AD) audit log schema in Azure Monitor. Elke afzonderlijke logboekvermelding is opgeslagen als tekst en opgemaakt als een JSON-blob, zoals wordt weergegeven in de volgende twee voorbeelden: 

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-17T00:14:31.2585575Z", 
        "operationName": "Change password (self-service)",
        "operationVersion": "1.0",
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "resultDescription": "None", 
        "durationMs": "-1", 
        "correlationId": "60d5e89a-b890-413f-9e25-a047734afe9f", 
        "identity": "sreens@wingtiptoysonline.com", 
        "Level": "Informational", 
        "location": "WUS", 
        "properties": { 
            "identityType": "UPN", 
            "operationType": "Update", 
            "additionalDetails": "None", 
            "additionalTargets": "", 
            "targetUpdatedProperties": "", 
            "targetResourceType": "UPN__TenantContextID__PUID__ObjectID__ObjectClass", 
            "targetResourceName": "sreens@wingtiptoysonline.com__bf85dc9d-cb43-44a4-80c4-469e8c58249e__1003BFFD9FEB17DB__7a408bdd-7d97-4574-8511-dd747b56465d__User", 
            "auditEventCategory": "UserManagement" 
        } 
    } 
    ] 
} 
```

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-18T19:47:43.0368859Z", 
        "operationName": "Update service principal.", 
        "operationVersion": "1.0", 
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "durationMs": "-1", 
        "callerIpAddress": "<null>", 
        "correlationId": "14916c7a-5a7d-44e8-9b06-74b49efb08ee", 
        "identity": "NA", 
        "Level": "Informational", 
        "properties": { 
            "identityType": "NA", 
            "operationType": "Update", 
            "additionalDetails": {}, 
            "additionalTargets": "", 
            "targetUpdatedProperties": [ 
            { 
                "Name": "Included Updated Properties", 
                "OldValue": null, 
                "NewValue": "" 
            }, 
            { 
                "Name": "TargetId.ServicePrincipalNames", 
                "OldValue": null, 
                "NewValue": "http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29" 
            } 
            ], 
        "targetResourceType": "Other__ObjectID__ObjectClass__Name__AppId__SPN", 
        "targetResourceName": "ServicePrincipal_ea70a262-4da3-440a-b396-9734ddfd9df2__ea70a262-4da3-440a-b396-9734ddfd9df2__ServicePrincipal__Salesforce__cd3ed3de-93ee-400b-8b19-b61ef44a0f29__http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29", 
        "auditEventCategory": "ApplicationManagement" 
      } 
    } 
    ] 
} 
```

```json
{
    "records": [
    {
        "time": "2018-12-10T00:03:46.6161822Z",
        "resourceId": "/tenants/7918d4b5-0442-4a97-be2d-36f9f9962ece/providers/Microsoft.aadiam",
        "operationName": "Update policy",
        "operationVersion": "1.0",
        "category": "AuditLogs",
        "tenantId": "7918d4b5-0442-4a97-be2d-36f9f9962ece",
        "resultSignature": "None",
        "durationMs": 0,
        "callerIpAddress": "<null>",
        "correlationId": "192298c1-0994-4dd6-b05a-a6c5984c31cb",
        "identity": "MS-PIM",
        "level": "Informational",
        "properties": {
            "id": "Directory_VNXV4_28148892",
            "category": "Policy",
            "correlationId": "192298c1-0994-4dd6-b05a-a6c5984c31cb",
            "result": 0,
            "resultReason": "",
            "activityDisplayName": "Update policy",
            "activityDateTime": "2018-12-10T00:03:46.6161822+00:00",
            "loggedByService": "Core Directory",
            "operationType": "Update",
            "initiatedBy": {},
            "targetResources": [
            {
                "id": "5e7a8ae7-165d-44a4-a4f4-6141f8c8ef40",
                "displayName": "Default Policy",
                "type": "Policy",
                "modifiedProperties": []
            }
            ],
            "additionalDetails": []
        }
    }
    ]
}

```

## <a name="field-and-property-descriptions"></a>Veld en de eigenschap beschrijvingen

| Veldnaam | Description |
|------------|-------------|
| time       | De datum en tijd (UTC). |
| operationName | De naam van de bewerking. |
| operationVersion | De REST-API-versie die aangevraagd door de client. |
| category | Op dit moment *Audit* is de enige ondersteunde waarde. |
| tenantId | De tenant GUID die is gekoppeld aan de logboeken. |
| resultType | Het resultaat van de bewerking. Kan het resultaat zijn *succes* of *fout*. |
| resultSignature |  Dit veld is niet-toegewezen en u kunt deze gewoon negeren. | 
| resultDescription | Een extra beschrijving van het resultaat, indien beschikbaar. | 
| durationMs |  Dit veld is niet-toegewezen en u kunt deze gewoon negeren. |
| callerIpAddress | Het IP-adres van de client die de aanvraag heeft ingediend. | 
| correlationId | Een optionele GUID die wordt doorgegeven door de client. Kunt vergelijken client-side-bewerkingen met serverzijde bewerkingen en dit is handig wanneer u logboeken met betrekking services tot bijhoudt. |
| identity | De identiteit van het token dat is opgegeven wanneer u de aanvraag heeft ingediend. De identiteit kan worden een gebruikersaccount, systeem-account of service-principal. |
| niveau | Het berichttype. Voor de logboeken voor controle, is het niveau van de altijd *ter informatie*. |
| location | De locatie van het datacenter. |
| properties | Geeft een lijst van de ondersteunde eigenschappen die betrekking op een auditlogboek hebben. Zie de volgende tabel voor meer informatie. | 

<br>

| Naam van eigenschap | Description |
|---------------|-------------|
| AuditEventCategory | Het type controlegebeurtenis. Kan het zijn *Gebruikersbeheer*, *Toepassingsbeheer*, of een ander type.|
| ID-Type | Het type kan worden *toepassing* of *gebruiker*. |
| Type bewerking | Het type kan worden *toevoegen*, *Update*, *verwijderen*. of *andere*. |
| Doelbrontype | Hiermee geeft u het doeltype van de resource die de bewerking is uitgevoerd op. Het type kan worden *toepassing*, *gebruiker*, *rol*, *beleid* | 
| Naam van de doel-Resource | De naam van de doelresource. De toepassingsnaam van een, de naam van een rol, een user principal name of een service principal name kan zijn. |
| additionalTargets | Hier worden eventuele aanvullende eigenschappen voor specifieke bewerkingen. Bijvoorbeeld, voor een updatebewerking de oude en de nieuwe waarden worden vermeld in *targetUpdatedProperties*. | 

## <a name="next-steps"></a>Volgende stappen

* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Aanmeldingslogboekenschema interpreteren in Azure Monitor)
* [Diagnostische logboeken van Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Veelgestelde vragen en bekende problemen](concept-activity-logs-azure-monitor.md#frequently-asked-questions)

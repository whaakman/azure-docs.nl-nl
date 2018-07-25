---
title: Interpreteren van de Azure Active Directory-schema audit log in Azure Monitor (preview) | Microsoft Docs
description: Beschrijf de Azure AD audit log-schema voor gebruik in Azure Monitor (preview)
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
ms.openlocfilehash: e1ae8e2a4dc9ef9c21300ebfc4df8c0f1c5819f2
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239887"
---
# <a name="interpret-the-azure-active-directory-audit-logs-schema-in-azure-monitor-preview"></a>Interpreteren van de Azure Active Directory-schema audit Logboeken in Azure Monitor (preview)

Dit artikel beschrijft de Azure AD audit log-schema in Azure Monitor. Elke afzonderlijke logboekvermelding wordt opgeslagen als tekst, opgemaakt als een JSON-blob, zoals wordt weergegeven in de onderstaande twee voorbeelden. 

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

| Veldnaam | Beschrijving |
|------------|-------------|
| tijd       | Datum en tijd (UTC) |
| operationName | Naam van de bewerking |
| operationVersion | REST-API-versie door de client aangevraagde |
| category | Op dit moment *Audit* is de enige ondersteunde waarde |
| tenant-id | Tenant-Guid die is gekoppeld aan de logboeken |
| resultType | Resultaat van de bewerking, kunt u *succes* of *fout* |
| resultSignature |  Dit is niet-toegewezen en u kunt dit veld veilig negeren. | 
| resultDescription | Extra beschrijving van het resultaat, indien beschikbaar | 
| durationMs |  Dit is niet-toegewezen en u kunt dit veld veilig negeren. |
| callerIpAddress | IP-adres van de client die de aanvraag heeft ingediend | 
| correlationId | Optionele Guid die is doorgegeven door de client. Dit kan helpen bij elkaar te vergelijken client-side-bewerkingen met server-side-bewerkingen en is handig bij het traceren van logboeken met betrekking alle services tot. |
| identity | De identiteit van het token dat is opgegeven bij het maken van de aanvraag. Mag een gebruikersaccount, de systeem-account of de service principal. |
| niveau | Het type van het bericht. Voor de logboeken voor controle, is dit altijd *ter informatie* |
| location | Locatie van het datacenter |
| properties | Een lijst met ondersteunde eigenschappen met betrekking tot een logboek. Zie voor meer informatie de onderstaande tabel. | 


| Naam van eigenschap | Beschrijving |
|---------------|-------------|
| AuditEventCategory | Het type controlegebeurtenis. Kan *Gebruikersbeheer*, *Toepassingsbeheer* enzovoort.|
| ID-Type | *Toepassing* of *gebruiker* |
| Type bewerking | Kan *toevoegen*, *Update*, *verwijderen* of *andere* |
| Doelbrontype | Hiermee geeft u het doeltype van de resource die de bewerking is uitgevoerd op. Kan *toepassing*, *gebruiker*, *rol*, *beleid* | 
| Naam van de doel-Resource | De naam van de doelresource. Bijvoorbeeld, kan dit de toepassingsnaam van een, de naam van een rol, een user principal name of een service principal name |
| additionalTargets | Hier worden eventuele aanvullende eigenschappen voor specifieke bewerkingen. Bijvoorbeeld, voor een updatebewerking de oude en de nieuwe waarden worden vermeld in *targetUpdatedProperties* | 

## <a name="next-steps"></a>Volgende stappen

* [Schema-in Logboeken in Azure monitor interpreteren](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Meer informatie over hoe Azure diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Veelgestelde vragen en bekende problemen](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)
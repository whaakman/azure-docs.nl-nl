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
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 3df0087a21547585883f791d4d385025f92580c7
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49395034"
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

## <a name="field-and-property-descriptions"></a>Veld en de eigenschap beschrijvingen

| Veldnaam | Beschrijving |
|------------|-------------|
| tijd       | De datum en tijd (UTC). |
| operationName | De naam van de bewerking. |
| operationVersion | De REST-API-versie die aangevraagd door de client. |
| category | Op dit moment *Audit* is de enige ondersteunde waarde. |
| tenant-id | De tenant GUID die is gekoppeld aan de logboeken. |
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

| Naam van eigenschap | Beschrijving |
|---------------|-------------|
| AuditEventCategory | Het type controlegebeurtenis. Kan het zijn *Gebruikersbeheer*, *Toepassingsbeheer*, of een ander type.|
| ID-Type | Het type kan worden *toepassing* of *gebruiker*. |
| Type bewerking | Het type kan worden *toevoegen*, *Update*, *verwijderen*. of *andere*. |
| Doelbrontype | Hiermee geeft u het doeltype van de resource die de bewerking is uitgevoerd op. Het type kan worden *toepassing*, *gebruiker*, *rol*, *beleid* | 
| Naam van de doel-Resource | De naam van de doelresource. De toepassingsnaam van een, de naam van een rol, een user principal name of een service principal name kan zijn. |
| additionalTargets | Hier worden eventuele aanvullende eigenschappen voor specifieke bewerkingen. Bijvoorbeeld, voor een updatebewerking de oude en de nieuwe waarden worden vermeld in *targetUpdatedProperties*. | 

## <a name="next-steps"></a>Volgende stappen

* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Aanmeldingslogboekenschema interpreteren in Azure Monitor)
* [Lees meer over Azure diagnostics-Logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Veelgestelde vragen en bekende problemen](concept-activity-logs-azure-monitor.md#frequently-asked-questions)

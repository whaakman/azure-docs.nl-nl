---
title: Schema voor Azure activiteit logboek met gebeurtenis | Microsoft Docs
description: Het schema van de gebeurtenis voor gegevens die in het activiteitenlogboek begrijpen
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2018
ms.author: dukek
ms.openlocfilehash: 4264bfd733f586dcdabdee8f29494bfffd9a7a76
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="azure-activity-log-event-schema"></a>Azure Activity Log gebeurtenis schema
De **Azure Activity Log** is een logboek die biedt inzicht in een abonnement op gebeurtenissen die hebben plaatsgevonden in Azure. Dit artikel wordt het schema van de gebeurtenis per categorie van gegevens.

## <a name="administrative"></a>Administratief
Deze rubriek bevat de record van alle maken, update, delete en actie bewerkingen uitgevoerd via Resource Manager. Voorbeelden van welke typen gebeurtenissen u in deze categorie ziet zijn 'virtuele machine maken' en 'netwerkbeveiligingsgroep verwijderen' elke actie op die door een gebruiker of toepassing die met Resource Manager is gemodelleerd als een bewerking op een bepaald resourcetype. Als het bewerkingstype schrijven, verwijderen of actie is, worden de records van de begin- en het slagen of mislukken van die bewerking vastgelegd in de beheercategorie. De beheercategorie omvat ook eventuele wijzigingen aan rollen gebaseerd toegangsbeheer in een abonnement.

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis
```json
{
    "authorization": {
        "action": "Microsoft.Network/networkSecurityGroups/write",
        "scope": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG"
    },
    "caller": "rob@contoso.com",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.core.windows.net/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "_claim_names": "{\"groups\":\"src1\"}",
        "_claim_sources": "{\"src1\":{\"endpoint\":\"https://graph.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/users/f409edeb-4d29-44b5-9763-ee9348ad91bb/getMemberObjects\"}}",
        "http://schemas.microsoft.com/claims/authnclassreference": "1",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
        "appid": "355249ed-15d9-460d-8481-84026b065942",
        "appidacr": "2",
        "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "10845a4d-ffa4-4b61-a3b4-e57b9b31cdb5",
        "e_exp": "262800",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Robertson",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Rob",
        "ipaddr": "111.111.1.111",
        "name": "Rob Robertson",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "onprem_sid": "S-1-5-21-4837261184-168309720-1886587427-18514304",
        "puid": "18247BBD84827C6D",
        "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "rob@contoso.com",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "rob@contoso.com",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2018-01-29T20:42:31.3810679Z",
    "id": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG/events/d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d/ticks/636528553513810679",
    "level": "Informational",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Network/networkSecurityGroups/write",
        "localizedValue": "Microsoft.Network/networkSecurityGroups/write"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Network",
        "localizedValue": "Microsoft.Network"
    },
    "resourceType": {
        "value": "Microsoft.Network/networkSecurityGroups",
        "localizedValue": "Microsoft.Network/networkSecurityGroups"
    },
    "resourceId": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-01-29T20:42:50.0724829Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "statusCode": "Created",
        "serviceRequestId": "a4c11dbd-697e-47c5-9663-12362307157d",
        "responseBody": "",
        "requestbody": ""
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
| Elementnaam | Beschrijving |
| --- | --- |
| Autorisatie |De BLOB van RBAC-eigenschappen van de gebeurtenis. Omvat gewoonlijk het eigenschappen "action", 'rol' en 'bereik'. |
| oproepende functie |E-mailadres van de gebruiker die is uitgevoerd. de bewerking, UPN-claim of SPN claim op basis van beschikbaarheid. |
| kanalen |Een van de volgende waarden: 'Admin', 'Operation' |
| Claims |De JWT-token gebruikt door Active Directory voor het verifiëren van de gebruiker of toepassing naar deze bewerking niet uitvoeren in het resourcemanager. |
| correlationId |Meestal een GUID in de indeling van de tekenreeks. Gebeurtenissen die een correlationId share deel uitmaken van dezelfde uber actie. |
| description |De beschrijving van de statische tekst van een gebeurtenis. |
| eventDataId |De unieke id van een gebeurtenis. |
| httpRequest |BLOB met een beschrijving van de Http-aanvraag. Omvat gewoonlijk het 'clientRequestId', "clientIpAddress" en "method" (HTTP-methode. For example, plaatsen). |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en 'Verbose' |
| resourceGroupName |De naam van de resourcegroep voor de betrokken resource. |
| resourceProviderName |Naam van de resourceprovider voor de betrokken resource |
| resourceId |Bron-id van de betrokken resource. |
| operationId |Een GUID die wordt gedeeld door de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |De naam van de bewerking. |
| properties |Een set `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: In voortgang, geslaagd, mislukt, actief, opgelost gestart. |
| subStatus |Meestal de HTTP-statuscode van de bijbehorende REST te bellen, maar kan ook andere tekenreeksen met een beschrijving van een substatus, zoals deze algemene waarden bevatten: OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), geen inhoud (http-Status Code: 204), onjuiste aanvraag (HTTP-statuscode: 400), niet is gevonden (HTTP-statuscode: 404), Conflict (HTTP-statuscode: 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar (HTTP-statuscode: 503), time-out van Gateway (HTTP-statuscode: 504). |
| eventTimestamp |Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstempel wanneer de gebeurtenis beschikbaar voor het uitvoeren van query's zijn geworden. |
| subscriptionId |Azure-abonnement-id. |

## <a name="service-health"></a>Servicestatus
Deze rubriek bevat de record van een service health incidenten die hebben plaatsgevonden in Azure. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "SQL Azure in VS-Oost ondervindt uitvaltijd." Gebeurtenissen van de health service in vijf soorten komen: actie vereist, ondersteunde herstel, Incident, onderhoud, gegevens of beveiliging, en wordt alleen weergegeven als er een resource in het abonnement dat zou worden beïnvloed door de gebeurtenis.

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/<subscription ID>/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/<subscription ID>",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "<subscription ID>",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```
Raadpleeg de [service health meldingen](./monitoring-service-notifications.md) voor documentatie over de waarden in de eigenschappen van het artikel.

## <a name="alert"></a>Waarschuwing
Deze rubriek bevat de record van alle activeringen van waarschuwingen van Azure. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "CPU-percentage op myVM is meer dan 80 voor de afgelopen vijf minuten." Een verscheidenheid aan Azure systemen hebben een waarschuwingsmethoden concept--u kunt een regel bepaalde hardwaresleutel definiëren en een melding ontvangen wanneer voorwaarden overeenkomen met die regel. Elke keer dat een ondersteunde Azure Waarschuwingstype 'wordt geactiveerd,' of de voorwaarden wordt voldaan voor het genereren van een melding, een record van de activering is ook naar deze categorie van het activiteitenlogboek gepusht.

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in the last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "<subscription ID>"
}
```

### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
| Elementnaam | Beschrijving |
| --- | --- |
| oproepende functie | Altijd Microsoft.Insights/alertRules |
| kanalen | Altijd 'Admin, bewerking' |
| Claims | JSON-blob met het type SPN (service principal name) of de bron van de waarschuwings-engine. |
| correlationId | Een GUID in de indeling van de tekenreeks. |
| description |De beschrijving van de statische tekst van de waarschuwing gebeurtenis. |
| eventDataId |De unieke id van de waarschuwing gebeurtenis. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en 'Verbose' |
| resourceGroupName |De naam van de resourcegroep voor de betrokken resource als deze een metrische waarschuwing. Dit is de naam van de resourcegroep die de waarschuwing zelf bevat voor de overige Waarschuwingstypen. |
| resourceProviderName |De naam van de resourceprovider voor de betrokken resource als deze een metrische waarschuwing. Dit is de naam van de resourceprovider voor de waarschuwing zelf voor andere typen waarschuwingen. |
| resourceId | De naam van de resource-ID voor de betrokken resource als deze een metrische waarschuwing. Dit is de bron-ID van de waarschuwing resource zelf voor andere typen waarschuwingen. |
| operationId |Een GUID die wordt gedeeld door de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |De naam van de bewerking. |
| properties |Een set `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: In voortgang, geslaagd, mislukt, actief, opgelost gestart. |
| subStatus | Meestal null voor waarschuwingen. |
| eventTimestamp |Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstempel wanneer de gebeurtenis beschikbaar voor het uitvoeren van query's zijn geworden. |
| subscriptionId |Azure-abonnement-id. |

### <a name="properties-field-per-alert-type"></a>Van eigenschappenveld per type waarschuwing
Het eigenschappenveld bevat verschillende waarden, afhankelijk van de bron van de waarschuwing gebeurtenis. Twee gebeurtenisproviders voor algemene waarschuwing zijn activiteitenlogboek van waarschuwingen en metrische waarschuwingen.

#### <a name="properties-for-activity-log-alerts"></a>Eigenschappen van activiteitenlogboek van waarschuwingen
| Elementnaam | Beschrijving |
| --- | --- |
| properties.subscriptionId | De abonnements-ID van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| properties.eventDataId | De gebeurtenis-ID van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| properties.resourceGroup | De resourcegroep van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| properties.resourceId | De resource-ID van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| properties.eventTimestamp | De gebeurtenis tijdstempel van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| properties.operationName | De naam van de bewerking van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| Properties.status | De status van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd.|

#### <a name="properties-for-metric-alerts"></a>Eigenschappen van metrische waarschuwingen
| Elementnaam | Beschrijving |
| --- | --- |
| Eigenschappen. RuleUri | Bron-ID van de metrische waarschuwingsregel zelf. |
| properties.RuleName | De naam van de metrische waarschuwingsregel. |
| Eigenschappen. RuleDescription | De beschrijving van de metrische waarschuwingsregel (zoals gedefinieerd in de waarschuwingsregel). |
| Eigenschappen. Drempelwaarde | De drempelwaarde die wordt gebruikt in de evaluatie van de metrische waarschuwingsregel. |
| Eigenschappen. WindowSizeInMinutes | De grootte die in de evaluatie van de metrische waarschuwingsregel wordt gebruikt. |
| Eigenschappen. Aggregatie | Het samenvoegingstype in de metrische waarschuwingsregel gedefinieerd. |
| properties.Operator | De voorwaardelijke operator die wordt gebruikt in de evaluatie van de metrische waarschuwingsregel. |
| properties.MetricName | De metrische naam van de metrische gegevens die in de evaluatie van de metrische waarschuwingsregel wordt gebruikt. |
| Eigenschappen. MetricUnit | De metrische eenheid voor de metriek gebruikt in de evaluatie van de metrische waarschuwingsregel. |

## <a name="autoscale"></a>Automatisch schalen
Deze rubriek bevat de record van alle gebeurtenissen met betrekking tot de werking van de engine voor het automatisch schalen op basis van de instellingen voor automatisch schalen die u hebt gedefinieerd in uw abonnement. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "Automatisch schalen opschaling van de actie is mislukt." Met automatisch schalen, kunt u automatisch geschaald uitbreiden of schalen op basis van tijd van de dag en/of laden (metrische) gegevens met behulp van een instelling voor automatisch schalen van het aantal exemplaren in een ondersteunde brontype. Wanneer de voorwaarden worden voldaan op schaal omhoog of omlaag is gestart en is geslaagd of mislukt gebeurtenissen worden vastgelegd in deze categorie.

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "The autoscale engine attempting to scale resource '/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "The autoscale engine attempting to scale resource '/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
    "ResourceName": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "<subscription ID>"
}

```

### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
| Elementnaam | Beschrijving |
| --- | --- |
| oproepende functie | Altijd Microsoft.Insights/autoscaleSettings |
| kanalen | Altijd 'Admin, bewerking' |
| Claims | JSON-blob met het type SPN (service principal name) of de bron van de engine voor automatisch schalen. |
| correlationId | Een GUID in de indeling van de tekenreeks. |
| description |De beschrijving van de statische tekst van de gebeurtenis met automatisch schalen. |
| eventDataId |De unieke id van de gebeurtenis met automatisch schalen. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en 'Verbose' |
| resourceGroupName |Naam van de resourcegroep voor de instelling voor automatisch schalen. |
| resourceProviderName |Naam van de resourceprovider voor de instelling voor automatisch schalen. |
| resourceId |Bron-id van de instelling voor automatisch schalen. |
| operationId |Een GUID die wordt gedeeld door de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |De naam van de bewerking. |
| properties |Een set `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. |
| Eigenschappen. Beschrijving | Gedetailleerde beschrijving van wat de engine voor het automatisch schalen die werd uitgevoerd. |
| properties.ResourceName | Bron-ID van de betrokken resource (de resource waarop de schaalactie werd uitgevoerd) |
| Eigenschappen. OldInstancesCount | Het aantal exemplaren voordat de actie voor automatisch schalen van kracht. |
| Eigenschappen. NewInstancesCount | Het aantal exemplaren nadat de actie voor automatisch schalen van kracht. |
| properties.LastScaleActionTime | De tijdstempel van wanneer de actie voor automatisch schalen is opgetreden. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: In voortgang, geslaagd, mislukt, actief, opgelost gestart. |
| subStatus | Meestal null voor automatisch schalen. |
| eventTimestamp |Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstempel wanneer de gebeurtenis beschikbaar voor het uitvoeren van query's zijn geworden. |
| subscriptionId |Azure-abonnement-id. |

## <a name="security"></a>Beveiliging
Deze rubriek bevat de record geen waarschuwingen gegenereerd door Azure Security Center. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is 'dubbele extensie verdachte file wordt uitgevoerd'

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis
```json
{
    "channels": "Operation",
    "correlationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "description": "Suspicious double extension file executed. Machine logs indicate an execution of a process with a suspicious double extension.\r\nThis extension may trick users into thinking files are safe to be opened and might indicate the presence of malware on the system.",
    "eventDataId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "eventName": {
        "value": "Suspicious double extension file executed",
        "localizedValue": "Suspicious double extension file executed"
    },
    "category": {
        "value": "Security",
        "localizedValue": "Security"
    },
    "eventTimestamp": "2017-10-18T06:02:18.6179339Z",
    "id": "/subscriptions/<subscription ID>/providers/Microsoft.Security/locations/centralus/alerts/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/events/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/ticks/636439033386179339",
    "level": "Informational",
    "operationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "operationName": {
        "value": "Microsoft.Security/locations/alerts/activate/action",
        "localizedValue": "Microsoft.Security/locations/alerts/activate/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Security",
        "localizedValue": "Microsoft.Security"
    },
    "resourceType": {
        "value": "Microsoft.Security/locations/alerts",
        "localizedValue": "Microsoft.Security/locations/alerts"
    },
    "resourceId": "/subscriptions/<subscription ID>/providers/Microsoft.Security/locations/centralus/alerts/2518939942613820660_a48f8653-3fc6-4166-9f19-914f030a13d3",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": null
    },
    "submissionTimestamp": "2017-10-18T06:02:52.2176969Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "accountLogonId": "0x2r4",
        "commandLine": "c:\\mydirectory\\doubleetension.pdf.exe",
        "domainName": "hpc",
        "parentProcess": "unknown",
        "parentProcess id": "0",
        "processId": "6988",
        "processName": "c:\\mydirectory\\doubleetension.pdf.exe",
        "userName": "myUser",
        "UserSID": "S-3-2-12",
        "ActionTaken": "Detected",
        "Severity": "High"
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
| Elementnaam | Beschrijving |
| --- | --- |
| kanalen | Altijd 'bewerking' |
| correlationId | Een GUID in de indeling van de tekenreeks. |
| description |De beschrijving van de statische tekst van de beveiligingsgebeurtenis. |
| eventDataId |De unieke id van de beveiligingsgebeurtenis. |
| eventName |Beschrijvende naam van de beveiligingsgebeurtenis. |
| id |Unieke resource-id van de beveiligingsgebeurtenis. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' of 'Uitgebreid' |
| resourceGroupName |De naam van de resourcegroep voor de resource. |
| resourceProviderName |Naam van de resourceprovider voor Azure Security Center. Altijd 'Microsoft.Security'. |
| resourceType |Het type resource dat de beveiligingsgebeurtenis, zoals 'Microsoft.Security/locations/alerts' gegenereerd |
| resourceId |Bron-id van de beveiligingswaarschuwing. |
| operationId |Een GUID die wordt gedeeld door de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |De naam van de bewerking. |
| properties |Een set `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. Deze eigenschappen varieert afhankelijk van het type beveiligingswaarschuwing. Zie [deze pagina](../security-center/security-center-alerts-type.md) voor een beschrijving van de typen waarschuwingen die afkomstig van Security Center zijn. |
| Eigenschappen. Ernst |De ernst op. Mogelijke waarden zijn 'Hoog', 'Gemiddeld' of 'Laag'. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: In voortgang, geslaagd, mislukt, actief, opgelost gestart. |
| subStatus | Meestal null voor beveiligingsgebeurtenissen. |
| eventTimestamp |Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstempel wanneer de gebeurtenis beschikbaar voor het uitvoeren van query's zijn geworden. |
| subscriptionId |Azure-abonnement-id. |

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het activiteitenlogboek (voorheen controlelogboeken)](monitoring-overview-activity-logs.md)
* [Stream de Azure Activity Log naar Event Hubs](monitoring-stream-activity-logs-event-hubs.md)

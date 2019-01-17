---
title: Gebeurtenisschema in het Azure-activiteitenlogboek
description: Inzicht in het gebeurtenissenschema voor voor gegevens die in het activiteitenlogboek
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 1/16/2019
ms.author: dukek
ms.component: logs
ms.openlocfilehash: d5e57442a163c8a93adc39517285bd88affab2fe
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353053"
---
# <a name="azure-activity-log-event-schema"></a>Gebeurtenisschema in het Azure-activiteitenlogboek
De **Azure Activity Log** is een logboek dat u inzicht biedt in een abonnement op gebeurtenissen die hebben plaatsgevonden in Azure. Dit artikel beschrijft de gebeurtenisschema per categorie van gegevens. Het schema van de gegevens verschilt afhankelijk van als u bij het lezen van gegevens in de portal, PowerShell of CLI, of rechtstreeks via de REST-API ten opzichte van [streaminggegevens opslag of Event Hubs met behulp van een Logboekprofiel](./../../azure-monitor/platform/activity-logs-overview.md#export-the-activity-log-with-a-log-profile). De voorbeelden hieronder ziet u het schema als beschikbaar via de portal, PowerShell, CLI en REST-API. Een toewijzing van deze eigenschappen aan de [Azure diagnostische logboeken schema](./tutorial-dashboards.md) wordt geleverd aan het einde van het artikel.

## <a name="administrative"></a>Administratief
Deze categorie bevat de record van alle maken, bijwerken, verwijderen en actie bewerkingen uitgevoerd via Resource Manager. Voorbeelden van de typen gebeurtenissen u in deze categorie ziet zijn 'virtuele machine maken' en 'netwerkbeveiligingsgroep verwijderen' elke actie op die door een gebruiker of een toepassing met behulp van Resource Manager is gemodelleerd als een bewerking op een bepaald resourcetype. Als het bewerkingstype schrijven, verwijderen of actie is, worden de records van de begin- en het slagen of mislukken van deze bewerking worden opgenomen in de beheercategorie. De beheercategorie omvat ook eventuele wijzigingen in de op rollen gebaseerd toegangsbeheer in een abonnement.

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
| De naam van element | Description |
| --- | --- |
| Autorisatie |De BLOB van RBAC-eigenschappen van de gebeurtenis. Omvat gewoonlijk de "action", 'rol' en 'bereik'-Eigenschappen. |
| oproepende functie |E-mailadres van de gebruiker die de bewerking, UPN-claim of op basis van beschikbaarheid SPN-claim heeft uitgevoerd. |
| kanalen |Een van de volgende waarden: 'Admin', "Bewerking" |
| claims |De JWT-token die wordt gebruikt door Active Directory voor het verifiëren van de gebruiker of toepassing voor deze bewerking in Resource Manager. |
| correlationId |Meestal een GUID in de indeling van de verbindingsreeks. Gebeurtenissen die delen van een correlationId behoren tot dezelfde uber actie. |
| description |De beschrijving van de statische tekst van een gebeurtenis. |
| eventDataId |De unieke id van een gebeurtenis. |
| httpRequest |De BLOB met een beschrijving van de Http-aanvraag. Omvat gewoonlijk het "clientRequestId", "clientIpAddress" en "method" (HTTP-methode. For example, plaatsen). |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing' en "Informatieve" |
| resourceGroupName |De naam van de resourcegroep voor de betrokken resource. |
| resourceProviderName |Naam van de resourceprovider voor de betrokken resource |
| resourceId |Resource-ID van de betrokken resource. |
| operationId |Een GUID die wordt gedeeld tussen de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |Naam van de bewerking. |
| properties |Instellen van `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: Gestart, In uitvoering, voltooid, is mislukt, actieve, opgelost. |
| subStatus |Meestal de HTTP-statuscode van de bijbehorende REST aanroepen, maar kan ook andere tekenreeksen met een beschrijving van een substatus, zoals deze algemene waarden bevatten: OK (HTTP-statuscode: 200), die zijn gemaakt (HTTP-statuscode: 201), geaccepteerd (HTTP-statuscode: 202), geen inhoud (HTTP-statuscode: 204), onjuiste aanvraag (HTTP-statuscode: 400), niet gevonden (HTTP-statuscode: 404), conflict (HTTP-statuscode: 409), een interne serverfout (HTTP-statuscode: 500), Service is niet beschikbaar (HTTP-statuscode: 503), time-out van gateway (HTTP-statuscode: 504). |
| eventTimestamp |Tijdstip waarop de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag die de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstip waarop de gebeurtenis is beschikbaar voor het uitvoeren van query's geworden. |
| subscriptionId |Azure-abonnement-ID. |

## <a name="service-health"></a>Servicestatus
Deze categorie bevat de record van de service health incidenten die hebben plaatsgevonden in Azure. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "SQL Azure in VS-Oost ondervindt uitvaltijd." Er zijn vijf soorten service health-gebeurtenissen: Actie vereist, telefonische herstel, Incident, onderhoud, informatie of beveiliging, en wordt alleen weergegeven als u een resource in het abonnement dat wordt beïnvloed door de gebeurtenis.

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
Raadpleeg de [health servicemeldingen](./../../azure-monitor/platform/service-notifications.md) artikel voor documentatie over de waarden in de eigenschappen.

## <a name="resource-health"></a>Status van resources
Deze categorie bevat de record van een resource health-gebeurtenissen die hebben plaatsgevonden naar uw Azure-resources. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "Virtuele Machine health-status gewijzigd in niet beschikbaar." Resource health-gebeurtenissen kunnen gelden voor een van de vier health-statussen: Beschikbaar, niet beschikbaar is, verminderde en onbekend. Resource health-gebeurtenissen kunnen ook worden gecategoriseerd als Platform die gebruiker geïnitieerd of.

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis

```json
{
    "channels": "Admin, Operation",
    "correlationId": "28f1bfae-56d3-7urb-bff4-194d261248e9",
    "description": "",
    "eventDataId": "a80024e1-883d-37ur-8b01-7591a1befccb",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "ResourceHealth",
        "localizedValue": "Resource Health"
    },
    "eventTimestamp": "2018-09-04T15:33:43.65Z",
    "id": "/subscriptions/<subscription Id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>/events/a80024e1-883d-42a5-8b01-7591a1befccb/ticks/636716720236500000",
    "level": "Critical",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Resourcehealth/healthevent/Activated/action",
        "localizedValue": "Health Event Activated"
    },
    "resourceGroupName": "<resource group>",
    "resourceProviderName": {
        "value": "Microsoft.Resourcehealth/healthevent/action",
        "localizedValue": "Microsoft.Resourcehealth/healthevent/action"
    },
    "resourceType": {
        "value": "Microsoft.Compute/virtualMachines",
        "localizedValue": "Microsoft.Compute/virtualMachines"
    },
    "resourceId": "/subscriptions/<subscription Id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-09-04T15:36:24.2240867Z",
    "subscriptionId": "<subscription Id>",
    "properties": {
        "stage": "Active",
        "title": "Virtual Machine health status changed to unavailable",
        "details": "Virtual machine has experienced an unexpected event",
        "healthStatus": "Unavailable",
        "healthEventType": "Downtime",
        "healthEventCause": "PlatformInitiated",
        "healthEventCategory": "Unplanned"
    },
    "relatedEvents": []
}
```

### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
| De naam van element | Description |
| --- | --- |
| kanalen | Altijd 'Admin, bewerking' |
| correlationId | Een GUID in de indeling van de verbindingsreeks. |
| description |De beschrijving van de statische tekst van de waarschuwing gebeurtenis. |
| eventDataId |De unieke id van de waarschuwing gebeurtenis. |
| category | Always "ResourceHealth" |
| eventTimestamp |Tijdstip waarop de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag die de gebeurtenis overeenkomt. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', "Error", 'Waarschuwing', "Informatief" en "Uitgebreide" |
| operationId |Een GUID die wordt gedeeld tussen de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |Naam van de bewerking. |
| resourceGroupName |De naam van de resourcegroep waarin de resource. |
| resourceProviderName |Altijd 'Microsoft.Resourcehealth/healthevent/action'. |
| ResourceType | Het type resource dat is beïnvloed door een gebeurtenis met Resource Health. |
| resourceId | Naam van de resource-ID voor de betrokken resource. |
| status |De tekenreeks met een beschrijving van de status van de health-gebeurtenis. Waarden zijn: Actieve, is opgelost, wordt uitgevoerd, bijgewerkt. |
| subStatus | Meestal null voor waarschuwingen. |
| submissionTimestamp |Tijdstip waarop de gebeurtenis is beschikbaar voor het uitvoeren van query's geworden. |
| subscriptionId |Azure-abonnement-id. |
| properties |Instellen van `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis.|
| properties.title | Een beschrijvende tekenreeks die de status van de resource beschrijft. |
| properties.details | Een beschrijvende tekenreeks die meer details over de gebeurtenis beschrijft. |
| properties.currentHealthStatus | De huidige status van de resource. Een van de volgende waarden: 'Beschikbaar', 'Niet beschikbaar', 'Verminderde' en 'Onbekend'. |
| properties.previousHealthStatus | De vorige status van de resource. Een van de volgende waarden: 'Beschikbaar', 'Niet beschikbaar', 'Verminderde' en 'Onbekend'. |
| properties.type | Een beschrijving van het type resource health gebeurtenis. |
| properties.cause | Een beschrijving van de oorzaak van de resource health-gebeurtenis. "UserInitiated" en 'PlatformInitiated'. |


## <a name="alert"></a>Waarschuwing
Deze categorie bevat de record van alle activeringen van de Azure-waarschuwingen. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "CPU-percentage op myVM is meer dan 80 voor de afgelopen vijf minuten." Een reeks systemen die Azure hebben een waarschuwingen concept--u kunt een regel voor een definiëren en een melding ontvangen wanneer er voorwaarden overeenkomen met die regel. Telkens wanneer een ondersteunde Azure Waarschuwingstype 'wordt geactiveerd,' of de voorwaarden wordt voldaan voor het genereren van een melding, een record van de activering wordt ook gepusht naar deze categorie van het activiteitenlogboek.

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
| De naam van element | Description |
| --- | --- |
| oproepende functie | Always Microsoft.Insights/alertRules |
| kanalen | Altijd 'Admin, bewerking' |
| claims | JSON-blob met het type SPN (service principal name) of de resource van de waarschuwings-engine. |
| correlationId | Een GUID in de indeling van de verbindingsreeks. |
| description |De beschrijving van de statische tekst van de waarschuwing gebeurtenis. |
| eventDataId |De unieke id van de waarschuwing gebeurtenis. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing' en "Informatieve" |
| resourceGroupName |De naam van de resourcegroep voor de betrokken resource als deze een waarschuwing voor metrische gegevens. Dit is de naam van de resourcegroep waarin de waarschuwing zelf voor andere typen waarschuwingen. |
| resourceProviderName |Naam van de resourceprovider voor de betrokken resource als deze een waarschuwing voor metrische gegevens. Dit is de naam van de resourceprovider voor de waarschuwing zelf voor andere typen waarschuwingen. |
| resourceId | Naam van de resource-ID voor de betrokken resource als deze een waarschuwing voor metrische gegevens. Dit is de resource-ID van de waarschuwing resource zelf voor andere typen waarschuwingen. |
| operationId |Een GUID die wordt gedeeld tussen de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |Naam van de bewerking. |
| properties |Instellen van `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: Gestart, In uitvoering, voltooid, is mislukt, actieve, opgelost. |
| subStatus | Meestal null voor waarschuwingen. |
| eventTimestamp |Tijdstip waarop de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag die de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstip waarop de gebeurtenis is beschikbaar voor het uitvoeren van query's geworden. |
| subscriptionId |Azure-abonnement-ID. |

### <a name="properties-field-per-alert-type"></a>Eigenschappenveld per type waarschuwing
Het eigenschappenveld bevat verschillende waarden, afhankelijk van de bron van de waarschuwing gebeurtenis. Twee algemene waarschuwing gebeurtenisproviders zijn waarschuwingen voor activiteitenlogboek en waarschuwingen voor metrische.

#### <a name="properties-for-activity-log-alerts"></a>Eigenschappen van waarschuwingen voor activiteitenlogboek
| De naam van element | Description |
| --- | --- |
| properties.subscriptionId | De abonnements-ID van de activiteit gebeurtenislogboek waardoor deze activiteit waarschuwingsregel wordt geactiveerd. |
| properties.eventDataId | De gebeurtenis-ID van de activiteit gebeurtenislogboek waardoor deze activiteit waarschuwingsregel wordt geactiveerd. |
| properties.resourceGroup | De resourcegroep van de activiteit gebeurtenislogboek waardoor deze activiteit waarschuwingsregel wordt geactiveerd. |
| properties.resourceId | De resource-ID van de activiteit gebeurtenislogboek waardoor deze activiteit waarschuwingsregel wordt geactiveerd. |
| properties.eventTimestamp | De gebeurtenis-timestamp van de activiteit gebeurtenislogboek waardoor deze activiteit waarschuwingsregel wordt geactiveerd. |
| properties.operationName | De naam van de bewerking van de activiteit gebeurtenislogboek waardoor deze activiteit waarschuwingsregel wordt geactiveerd. |
| Properties.status | De status van de activiteit gebeurtenislogboek waardoor deze activiteit waarschuwingsregel wordt geactiveerd.|

#### <a name="properties-for-metric-alerts"></a>Eigenschappen van metrische waarschuwingen
| De naam van element | Description |
| --- | --- |
| properties.RuleUri | Resource-ID van de waarschuwingsregel voor metrische gegevens zelf. |
| properties.RuleName | De naam van de waarschuwingsregel voor metrische gegevens. |
| de eigenschappen. RuleDescription | De beschrijving van de waarschuwingsregel voor metrische gegevens gebruik (zoals gedefinieerd in de waarschuwingsregel). |
| de eigenschappen. Drempelwaarde | De drempelwaarde die wordt gebruikt in de evaluatie van de waarschuwingsregel voor metrische gegevens. |
| properties.WindowSizeInMinutes | De grootte van het venster gebruikt in de evaluatie van de waarschuwingsregel voor metrische gegevens. |
| properties.Aggregation | Het aggregatietype is gedefinieerd in de waarschuwingsregel voor metrische gegevens. |
| properties.Operator | De conditionele operator die wordt gebruikt in de evaluatie van de waarschuwingsregel voor metrische gegevens. |
| properties.MetricName | De naam van de metrische gegevens van de metrische gegevens die worden gebruikt in de evaluatie van de waarschuwingsregel voor metrische gegevens. |
| properties.MetricUnit | De metrische eenheid voor de metrische gegevens die worden gebruikt in de evaluatie van de waarschuwingsregel voor metrische gegevens. |

## <a name="autoscale"></a>Automatisch schalen
Deze categorie bevat de record van alle gebeurtenissen die betrekking hebben op de werking van de engine voor automatisch schalen op basis van alle instellingen voor automatisch schalen die u hebt gedefinieerd in uw abonnement. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "Schalen via automatisch schalen van de actie is mislukt". Automatisch schalen gebruik, u kunt automatisch omhoog of omlaag schalen op basis van tijd van de dag en/of load (metrische) gegevens met behulp van een instelling voor automatisch schalen van het aantal exemplaren in een ondersteunde resourcetype. Wanneer de voorwaarden worden voldaan op schaal omhoog of omlaag, het begin en is geslaagd of mislukt gebeurtenissen geregistreerd in deze categorie.

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
| De naam van element | Description |
| --- | --- |
| oproepende functie | Always Microsoft.Insights/autoscaleSettings |
| kanalen | Altijd 'Admin, bewerking' |
| claims | JSON-blob met het type SPN (service principal name) of de resource van de engine voor automatisch schalen. |
| correlationId | Een GUID in de indeling van de verbindingsreeks. |
| description |De beschrijving van de statische tekst van de gebeurtenis voor automatisch schalen. |
| eventDataId |De unieke id van de gebeurtenis voor automatisch schalen. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing' en "Informatieve" |
| resourceGroupName |De naam van de resourcegroep voor de instelling voor automatisch schalen. |
| resourceProviderName |Naam van de resourceprovider voor de instelling voor automatisch schalen. |
| resourceId |Resource-ID van de instelling voor automatisch schalen. |
| operationId |Een GUID die wordt gedeeld tussen de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |Naam van de bewerking. |
| properties |Instellen van `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. |
| de eigenschappen. Beschrijving | Gedetailleerde beschrijving van wat de engine voor automatisch schalen die werd uitgevoerd. |
| properties.ResourceName | Resource-ID van de betrokken resource (de resource waarop de schaalactie werd uitgevoerd) |
| properties.OldInstancesCount | Het aantal exemplaren voordat de actie voor automatisch schalen van kracht. |
| properties.NewInstancesCount | Het aantal exemplaren na de actie voor automatisch schalen van kracht. |
| properties.LastScaleActionTime | De timestamp van wanneer de actie voor automatisch schalen is opgetreden. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: Gestart, In uitvoering, voltooid, is mislukt, actieve, opgelost. |
| subStatus | Meestal null voor automatisch schalen. |
| eventTimestamp |Tijdstip waarop de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag die de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstip waarop de gebeurtenis is beschikbaar voor het uitvoeren van query's geworden. |
| subscriptionId |Azure-abonnement-ID. |

## <a name="security"></a>Beveiliging
Deze categorie bevat de record geen waarschuwingen gegenereerd door Azure Security Center. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "verdacht bestand met dubbele extensie uitgevoerd."

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
| De naam van element | Description |
| --- | --- |
| kanalen | Altijd "bewerking' |
| correlationId | Een GUID in de indeling van de verbindingsreeks. |
| description |De beschrijving van de statische tekst van de beveiligingsgebeurtenis. |
| eventDataId |De unieke id van de beveiligingsgebeurtenis. |
| eventName |Beschrijvende naam van de beveiligingsgebeurtenis. |
| id |De unieke resource-id van de beveiligingsgebeurtenis. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', "Error", 'Waarschuwing' of 'Informatie' |
| resourceGroupName |De naam van de resourcegroep voor de resource. |
| resourceProviderName |Naam van de resourceprovider voor Azure Security Center. Altijd 'Microsoft.Security'. |
| ResourceType |Het type resource dat de beveiligingsgebeurtenis, zoals 'Microsoft.Security/locations/alerts' gegenereerd |
| resourceId |Resource-ID van de beveiligingswaarschuwing. |
| operationId |Een GUID die wordt gedeeld tussen de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |Naam van de bewerking. |
| properties |Instellen van `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. Deze eigenschappen varieert, afhankelijk van het type van de beveiligingswaarschuwing. Zie [deze pagina](../../security-center/security-center-alerts-type.md) voor een beschrijving van de typen waarschuwingen die afkomstig van Security Center zijn. |
| de eigenschappen. Ernst |De ernst op. Mogelijke waarden zijn 'Hoog', 'Gemiddeld' of 'Laag'. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: Gestart, In uitvoering, voltooid, is mislukt, actieve, opgelost. |
| subStatus | Meestal null voor beveiligingsgebeurtenissen. |
| eventTimestamp |Tijdstip waarop de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag die de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstip waarop de gebeurtenis is beschikbaar voor het uitvoeren van query's geworden. |
| subscriptionId |Azure-abonnement-ID. |

## <a name="recommendation"></a>Aanbeveling
Deze categorie bevat de record van een nieuwe aanbevelingen die worden gegenereerd voor uw services. Een voorbeeld van een aanbeveling hiervan is "beschikbaarheidssets gebruiken voor verbeterde fouttolerantie." Er zijn 4 typen aanbevelingsgebeurtenissen die kunnen worden gegenereerd: Hoge beschikbaarheid, prestaties, beveiliging en kosten optimaliseren. 

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis
```json
{
    "channels": "Operation",
    "correlationId": "92481dfd-c5bf-4752-b0d6-0ecddaa64776",
    "description": "The action was successful.",
    "eventDataId": "06cb0e44-111b-47c7-a4f2-aa3ee320c9c5",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Recommendation",
        "localizedValue": "Recommendation"
    },
    "eventTimestamp": "2018-06-07T21:30:42.976919Z",
    "id": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MYVM/events/06cb0e44-111b-47c7-a4f2-aa3ee320c9c5/ticks/636640038429769190",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Advisor/generateRecommendations/action",
        "localizedValue": "Microsoft.Advisor/generateRecommendations/action"
    },
    "resourceGroupName": "MYRESOURCEGROUP",
    "resourceProviderName": {
        "value": "MICROSOFT.COMPUTE",
        "localizedValue": "MICROSOFT.COMPUTE"
    },
    "resourceType": {
        "value": "MICROSOFT.COMPUTE/virtualmachines",
        "localizedValue": "MICROSOFT.COMPUTE/virtualmachines"
    },
    "resourceId": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MYVM",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-06-07T21:30:42.976919Z",
    "subscriptionId": "<Subscription ID>",
    "properties": {
        "recommendationSchemaVersion": "1.0",
        "recommendationCategory": "Security",
        "recommendationImpact": "High",
        "recommendationRisk": "None"
    },
    "relatedEvents": []
}

```
### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
| De naam van element | Description |
| --- | --- |
| kanalen | Altijd "bewerking' |
| correlationId | Een GUID in de indeling van de verbindingsreeks. |
| description |Beschrijving van de gebeurtenis aanbeveling statische tekst |
| eventDataId | De unieke id van de gebeurtenis aanbeveling. |
| category | Altijd "Recommendation" |
| id |De unieke resource-id van de gebeurtenis aanbeveling. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', "Error", 'Waarschuwing' of 'Informatie' |
| operationName |Naam van de bewerking.  Altijd "Microsoft.Advisor/generateRecommendations/action"|
| resourceGroupName |De naam van de resourcegroep voor de resource. |
| resourceProviderName |Naam van de resourceprovider voor de resource die deze aanbeveling is van toepassing op, zoals "MICROSOFT.COMPUTE" |
| ResourceType |Naam van het resourcetype voor de resource die deze aanbeveling is van toepassing op, zoals "MICROSOFT.COMPUTE/virtualmachines" |
| resourceId |Resource-ID van de resource die de aanbeveling is van toepassing op |
| status | Altijd 'Active' |
| submissionTimestamp |Tijdstip waarop de gebeurtenis is beschikbaar voor het uitvoeren van query's geworden. |
| subscriptionId |Azure-abonnement-ID. |
| properties |Instellen van `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de aanbeveling.|
| properties.recommendationSchemaVersion| Schemaversie van de aanbeveling-eigenschappen gepubliceerd in de activiteit logboekvermelding |
| properties.recommendationCategory | De categorie van de aanbeveling. Mogelijke waarden zijn 'Hoge beschikbaarheid', 'Prestaties', "Beveiliging" en "Kosten" |
| properties.recommendationImpact| De invloed van de aanbeveling. Mogelijke waarden zijn 'Hoog', 'Gemiddeld', 'Laag' |
| properties.recommendationRisk| Risico's van de aanbeveling. Mogelijke waarden zijn "Error", 'Waarschuwing', 'None' |

## <a name="policy"></a>Beleid

Deze categorie bevat records voor alle bewerkingen met effect actie uitgevoerd door [Azure Policy](../../governance/policy/overview.md). Voorbeelden van de typen gebeurtenissen u in deze categorie ziet zijn _Audit_ en _weigeren_. Elke actie op die door het beleid is gemodelleerd als een bewerking op een resource.

### <a name="sample-policy-event"></a>Voorbeeld van beleid voor de gebeurtenis

```json
{
    "authorization": {
        "action": "Microsoft.Resources/checkPolicyCompliance/read",
        "scope": "/subscriptions/<subscriptionID>"
    },
    "caller": "33a68b9d-63ce-484c-a97e-94aef4c89648",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.azure.com/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "appid": "1d78a85d-813d-46f0-b496-dd72f50a3ec0",
        "appidacr": "2",
        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "description": "",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Policy",
        "localizedValue": "Policy"
    },
    "eventTimestamp": "2019-01-15T13:19:56.1227642Z",
    "id": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/contososqlpolicy/events/13bbf75f-36d5-4e66-b693-725267ff21ce/ticks/636831551961227642",
    "level": "Warning",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Authorization/policies/audit/action",
        "localizedValue": "Microsoft.Authorization/policies/audit/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Sql",
        "localizedValue": "Microsoft SQL"
    },
    "resourceType": {
        "value": "Microsoft.Resources/checkPolicyCompliance",
        "localizedValue": "Microsoft.Resources/checkPolicyCompliance"
    },
    "resourceId": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/contososqlpolicy",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-01-15T13:20:17.1077672Z",
    "subscriptionId": "<subscriptionID>",
    "properties": {
        "isComplianceCheck": "True",
        "resourceLocation": "westus2",
        "ancestors": "72f988bf-86f1-41af-91ab-2d7cd011db47",
        "policies": "[{\"policyDefinitionId\":\"/subscriptions/<subscriptionID>/providers/Microsoft.
            Authorization/policyDefinitions/5775cdd5-d3d3-47bf-bc55-bb8b61746506/\",\"policyDefiniti
            onName\":\"5775cdd5-d3d3-47bf-bc55-bb8b61746506\",\"policyDefinitionEffect\":\"Deny\",\"
            policyAssignmentId\":\"/subscriptions/<subscriptionID>/providers/Microsoft.Authorization
            /policyAssignments/991a69402a6c484cb0f9b673/\",\"policyAssignmentName\":\"991a69402a6c48
            4cb0f9b673\",\"policyAssignmentScope\":\"/subscriptions/<subscriptionID>\",\"policyAssig
            nmentParameters\":{}}]"
    },
    "relatedEvents": []
}
```

### <a name="policy-event-property-descriptions"></a>Beschrijvingen van beleid voor gebeurtenissen-eigenschap

| De naam van element | Description |
| --- | --- |
| Autorisatie | Matrix van RBAC-eigenschappen van de gebeurtenis. Dit is de actie en het bereik van de aanvraag waarmee evaluatie is geactiveerd voor nieuwe resources. Voor bestaande resources is de actie 'Microsoft.Resources/checkPolicyCompliance/read'. |
| oproepende functie | Voor nieuwe resources, de identiteit die een implementatie wordt gestart. Voor bestaande bronnen, de GUID van de Microsoft Azure Policy Insights RP. |
| kanalen | Voor Beleidsgebeurtenissen gebruiken alleen het kanaal 'Bewerking'. |
| claims | De JWT-token die wordt gebruikt door Active Directory voor het verifiëren van de gebruiker of toepassing voor deze bewerking in Resource Manager. |
| correlationId | Meestal een GUID in de indeling van de verbindingsreeks. Gebeurtenissen die delen van een correlationId behoren tot dezelfde uber actie. |
| description | Dit veld is leeg voor Beleidsgebeurtenissen. |
| eventDataId | De unieke id van een gebeurtenis. |
| eventName | "BeginRequest" of 'EndRequest'. 'BeginRequest' wordt gebruikt voor vertraagde auditIfNotExists en deployIfNotExists evaluaties en wanneer de sjabloonimplementatie van een in een deployIfNotExists-effect wordt gestart. Alle andere bewerkingen retourneren 'EndRequest'. |
| category | Verklaart de activiteit gebeurtenislogboek als behorend tot '-beleid'. |
| eventTimestamp | Tijdstip waarop de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag die de gebeurtenis overeenkomt. |
| id | De unieke id van de gebeurtenis op de specifieke resource. |
| niveau | Niveau van de gebeurtenis. Audit 'Waarschuwing' gebruikt en weigeren maakt gebruik van "Error". Een fout auditIfNotExists of deployIfNotExists kunt genereren "Waarschuwing" of "Error", afhankelijk van ernst. Alle Beleidsgebeurtenissen van andere 'Ter informatie' gebruiken. |
| operationId | Een GUID die wordt gedeeld tussen de gebeurtenissen die met één bewerking overeenkomen. |
| operationName | Naam van de bewerking en rechtstreeks correleert met het effect van het beleid. |
| resourceGroupName | De naam van de resourcegroep voor de geëvalueerde resource. |
| resourceProviderName | De naam van de resourceprovider voor de geëvalueerde resource. |
| ResourceType | Voor nieuwe resources is het type dat wordt geëvalueerd. Retourneert 'Microsoft.Resources/checkPolicyCompliance' voor bestaande resources. |
| resourceId | Resource-ID van de geëvalueerde resource. |
| status | De tekenreeks met een beschrijving van de status van het resultaat van evaluatie van beleid. De meeste beleid evaluaties 'Geslaagd' retourneren, maar wel een weigeractie geeft als resultaat 'Mislukt'. Fouten in auditIfNotExists of deployIfNotExists ook retourneren 'Mislukt'. |
| subStatus | Dit veld is leeg voor Beleidsgebeurtenissen. |
| submissionTimestamp | Tijdstip waarop de gebeurtenis is beschikbaar voor het uitvoeren van query's geworden. |
| subscriptionId | Azure-abonnement-ID. |
| properties.isComplianceCheck | Retourneert 'False' wanneer een nieuwe resource wordt geïmplementeerd of een bestaande resource Resource Manager-eigenschappen zijn bijgewerkt. Alle andere [evaluatie triggers](../../governance/policy/how-to/get-compliance-data.md#evaluation-triggers) leiden tot 'True'. |
| properties.resourceLocation | De Azure-regio van de resource die wordt geëvalueerd. |
| properties.ancestors | Een door komma's gescheiden lijst met beheergroepen bovenliggende gerangschikt op de direct bovenliggende naar verst grootvader. |
| Properties.Policies | Bevat informatie over de beleidsdefinitie, toewijzing, kracht en parameters die deze evaluatie van het beleid een resultaat van is. |
| relatedEvents | Dit veld is leeg voor Beleidsgebeurtenissen. |

## <a name="mapping-to-diagnostic-logs-schema"></a>Toewijzing van schema van de logboeken met diagnostische gegevens

Bij het streamen van de Azure-activiteitenlogboek naar een opslagaccount of Event Hubs-naamruimte, de gegevens volgt de [Azure diagnostische logboeken schema](./tutorial-dashboards.md). Dit is de toewijzing van eigenschappen van het schema boven aan het schema van logboeken met diagnostische gegevens:

| Diagnostische logboeken schema-eigenschap | Activiteit Log REST API-schema-eigenschap | Opmerkingen |
| --- | --- | --- |
| time | eventTimestamp |  |
| resourceId | resourceId | abonnements-id, resourceType, resourceGroupName zijn afgeleid van de resourceId. |
| operationName | operationName.value |  |
| category | Onderdeel van de naam van bewerking | Groepen van het bewerkingstype - "Schrijven" / "verwijderen" / "Action" |
| resultType | status.Value | |
| resultSignature | substatus.Value | |
| resultDescription | description |  |
| durationMs | N/A | Altijd 0 |
| callerIpAddress | httpRequest.clientIpAddress |  |
| correlationId | correlationId |  |
| identity | claims en autorisatie-eigenschappen |  |
| Niveau | Niveau |  |
| location | N/A | Locatie van waar de gebeurtenis is verwerkt. *Dit is niet de locatie van de resource, maar in plaats van waar de gebeurtenis is verwerkt. Deze eigenschap wordt verwijderd in een toekomstige update.* |
| Properties | properties.eventProperties |  |
| properties.eventCategory | category | Als properties.eventCategory niet aanwezig is, is categorie "Administrative" |
| properties.eventName | eventName |  |
| properties.operationId | operationId |  |
| properties.eventProperties | properties |  |


## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het activiteitenlogboek (voorheen controlelogboeken)](../../azure-monitor/platform/activity-logs-overview.md)
* [Stream het Azure-activiteitenlogboek naar Eventhubs](../../azure-monitor/platform/activity-logs-stream-event-hubs.md)

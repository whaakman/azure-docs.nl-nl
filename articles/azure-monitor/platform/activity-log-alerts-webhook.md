---
title: Inzicht in de webhook-schema gebruikt in waarschuwingen voor activiteitenlogboeken
description: Meer informatie over het schema van de JSON die wordt gepost naar een webhook-URL wanneer een waarschuwing voor activiteitenlogboek wordt geactiveerd.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: johnkem
ms.subservice: alerts
ms.openlocfilehash: c91c1badaa4b1bc055859d700857cfd4d062babd
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491503"
---
# <a name="webhooks-for-azure-activity-log-alerts"></a>Webhooks voor waarschuwingen voor activiteitenlogboeken van Azure
U kunt webhook-eindpunten voor het ontvangen van waarschuwingsmeldingen voor activiteit log configureren als onderdeel van de definitie van een actiegroep. Met webhooks, kunt u deze meldingen doorsturen naar andere systemen voor na verwerking of aangepaste acties. Dit artikel leest hoe de nettolading voor de HTTP POST naar een webhook eruitziet.

Zie voor meer informatie over waarschuwingen voor activiteitenlogboeken, hoe u [waarschuwingen voor Azure-activiteitenlogboek maken](activity-log-alerts.md).

Zie voor informatie over actiegroepen, hoe u [Actiegroepen maken](../../azure-monitor/platform/action-groups.md).

> [!NOTE]
> U kunt ook de [algemene waarschuwing schema](https://aka.ms/commonAlertSchemaDocs), waarmee u het voordeel dat een enkele, uitbreidbare en geïntegreerde alert payload voor alle van de waarschuwing van services in Azure Monitor voor de webhook-integraties. [Meer informatie over de algemene waarschuwing schemadefinities.](https://aka.ms/commonAlertSchemaDefinitions)


## <a name="authenticate-the-webhook"></a>Verifiëren van de webhook
De webhook kunt u eventueel autorisatie op basis van tokens gebruiken voor verificatie. De URI wordt opgeslagen met een token-ID, bijvoorbeeld webhook `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.

## <a name="payload-schema"></a>De nettolading van schema
De JSON-nettolading die is opgenomen in de POST-bewerking afhankelijk van de nettolading van data.context.activityLog.eventSource veld.

### <a name="common"></a>Algemeen

```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```

### <a name="administrative"></a>Administratief

```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}
```

### <a name="security"></a>Beveiliging

```json
{
    "schemaId":"Microsoft.Insights/activityLogs",
    "data":{"status":"Activated",
        "context":{
            "activityLog":{
                "channels":"Operation",
                "correlationId":"2518408115673929999",
                "description":"Failed SSH brute force attack. Failed brute force attacks were detected from the following attackers: [\"IP Address: 01.02.03.04\"].  Attackers were trying to access the host with the following user names: [\"root\"].",
                "eventSource":"Security",
                "eventTimestamp":"2017-06-25T19:00:32.607+00:00",
                "eventDataId":"Sec-07f2-4d74-aaf0-03d2f53d5a33",
                "level":"Informational",
                "operationName":"Microsoft.Security/locations/alerts/activate/action",
                "operationId":"Sec-07f2-4d74-aaf0-03d2f53d5a33",
                "properties":{
                    "attackers":"[\"IP Address: 01.02.03.04\"]",
                    "numberOfFailedAuthenticationAttemptsToHost":"456",
                    "accountsUsedOnFailedSignInToHostAttempts":"[\"root\"]",
                    "wasSSHSessionInitiated":"No","endTimeUTC":"06/25/2017 19:59:39",
                    "actionTaken":"Detected",
                    "resourceType":"Virtual Machine",
                    "severity":"Medium",
                    "compromisedEntity":"LinuxVM1",
                    "remediationSteps":"[In case this is an Azure virtual machine, add the source IP to NSG block list for 24 hours (see https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/)]",
                    "attackedResourceType":"Virtual Machine"
                },
                "resourceId":"/subscriptions/12345-5645-123a-9867-123b45a6789/resourceGroups/contoso/providers/Microsoft.Security/locations/centralus/alerts/Sec-07f2-4d74-aaf0-03d2f53d5a33",
                "resourceGroupName":"contoso",
                "resourceProviderName":"Microsoft.Security",
                "status":"Active",
                "subscriptionId":"12345-5645-123a-9867-123b45a6789",
                "submissionTimestamp":"2017-06-25T20:23:04.9743772+00:00",
                "resourceType":"MICROSOFT.SECURITY/LOCATIONS/ALERTS"
            }
        },
        "properties":{}
    }
}
```

### <a name="recommendation"></a>Aanbeveling

```json
{
    "schemaId":"Microsoft.Insights/activityLogs",
    "data":{
        "status":"Activated",
        "context":{
            "activityLog":{
                "channels":"Operation",
                "claims":"{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress\":\"Microsoft.Advisor\"}",
                "caller":"Microsoft.Advisor",
                "correlationId":"123b4c54-11bb-3d65-89f1-0678da7891bd",
                "description":"A new recommendation is available.",
                "eventSource":"Recommendation",
                "eventTimestamp":"2017-06-29T13:52:33.2742943+00:00",
                "httpRequest":"{\"clientIpAddress\":\"0.0.0.0\"}",
                "eventDataId":"1bf234ef-e45f-4567-8bba-fb9b0ee1dbcb",
                "level":"Informational",
                "operationName":"Microsoft.Advisor/recommendations/available/action",
                "properties":{
                    "recommendationSchemaVersion":"1.0",
                    "recommendationCategory":"HighAvailability",
                    "recommendationImpact":"Medium",
                    "recommendationName":"Enable Soft Delete to protect your blob data",
                    "recommendationResourceLink":"https://portal.azure.com/#blade/Microsoft_Azure_Expert/RecommendationListBlade/recommendationTypeId/12dbf883-5e4b-4f56-7da8-123b45c4b6e6",
                    "recommendationType":"12dbf883-5e4b-4f56-7da8-123b45c4b6e6"
                },
                "resourceId":"/subscriptions/12345-5645-123a-9867-123b45a6789/resourceGroups/contoso/providers/microsoft.storage/storageaccounts/contosoStore",
                "resourceGroupName":"CONTOSO",
                "resourceProviderName":"MICROSOFT.STORAGE",
                "status":"Active",
                "subStatus":"",
                "subscriptionId":"12345-5645-123a-9867-123b45a6789",
                "submissionTimestamp":"2017-06-29T13:52:33.2742943+00:00",
                "resourceType":"MICROSOFT.STORAGE/STORAGEACCOUNTS"
            }
        },
        "properties":{}
    }
}
```

### <a name="servicehealth"></a>ServiceHealth

```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
            "channels": "Admin",
            "correlationId": "bbac944f-ddc0-4b4c-aa85-cc7dc5d5c1a6",
            "description": "Active: Virtual Machines - Australia East",
            "eventSource": "ServiceHealth",
            "eventTimestamp": "2017-10-18T23:49:25.3736084+00:00",
            "eventDataId": "6fa98c0f-334a-b066-1934-1a4b3d929856",
            "level": "Informational",
            "operationName": "Microsoft.ServiceHealth/incident/action",
            "operationId": "bbac944f-ddc0-4b4c-aa85-cc7dc5d5c1a6",
            "properties": {
                "title": "Virtual Machines - Australia East",
                "service": "Virtual Machines",
                "region": "Australia East",
                "communication": "Starting at 02:48 UTC on 18 Oct 2017 you have been identified as a customer using Virtual Machines in Australia East who may receive errors starting Dv2 Promo and DSv2 Promo Virtual Machines which are in a stopped &quot;deallocated&quot; or suspended state. Customers can still provision Dv1 and Dv2 series Virtual Machines or try deploying Virtual Machines in other regions, as a possible workaround. Engineers have identified a possible fix for the underlying cause, and are exploring implementation options. The next update will be provided as events warrant.",
                "incidentType": "Incident",
                "trackingId": "0NIH-U2O",
                "impactStartTime": "2017-10-18T02:48:00.0000000Z",
                "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"}],\"ServiceName\":\"Virtual Machines\"}]",
                "defaultLanguageTitle": "Virtual Machines - Australia East",
                "defaultLanguageContent": "Starting at 02:48 UTC on 18 Oct 2017 you have been identified as a customer using Virtual Machines in Australia East who may receive errors starting Dv2 Promo and DSv2 Promo Virtual Machines which are in a stopped &quot;deallocated&quot; or suspended state. Customers can still provision Dv1 and Dv2 series Virtual Machines or try deploying Virtual Machines in other regions, as a possible workaround. Engineers have identified a possible fix for the underlying cause, and are exploring implementation options. The next update will be provided as events warrant.",
                "stage": "Active",
                "communicationId": "636439673646212912",
                "version": "0.1.1"
            },
            "status": "Active",
            "subscriptionId": "45529734-0ed9-4895-a0df-44b59a5a07f9",
            "submissionTimestamp": "2017-10-18T23:49:28.7864349+00:00"
        }
    },
    "properties": {}
    }
}
```

Zie voor specifieke schema-informatie over service health melding activiteitenlogboekalarmen, [health servicemeldingen](../../azure-monitor/platform/service-notifications.md). Bovendien leert hoe u [service health webhook-meldingen configureren met uw bestaande oplossingen voor probleem](../../service-health/service-health-alert-webhook-guide.md).

### <a name="resourcehealth"></a>ResourceHealth

```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Admin, Operation",
                "correlationId": "a1be61fd-37ur-ba05-b827-cb874708babf",
                "eventSource": "ResourceHealth",
                "eventTimestamp": "2018-09-04T23:09:03.343+00:00",
                "eventDataId": "2b37e2d0-7bda-4de7-ur8c6-1447d02265b2",
                "level": "Informational",
                "operationName": "Microsoft.Resourcehealth/healthevent/Activated/action",
                "operationId": "2b37e2d0-7bda-489f-81c6-1447d02265b2",
                "properties": {
                    "title": "Virtual Machine health status changed to unavailable",
                    "details": "Virtual machine has experienced an unexpected event",
                    "currentHealthStatus": "Unavailable",
                    "previousHealthStatus": "Available",
                    "type": "Downtime",
                    "cause": "PlatformInitiated"
                },
                "resourceId": "/subscriptions/<subscription Id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>",
                "resourceGroupName": "<resource group>",
                "resourceProviderName": "Microsoft.Resourcehealth/healthevent/action",
                "status": "Active",
                "subscriptionId": "<subscription Id>",
                "submissionTimestamp": "2018-09-04T23:11:06.1607287+00:00",
                "resourceType": "Microsoft.Compute/virtualMachines"
            }
        }
    }
}
```

| De naam van element | Description |
| --- | --- |
| status |Gebruikt voor het metrische waarschuwingen. Altijd ingesteld op 'actief' voor waarschuwingen voor activiteitenlogboeken. |
| context |De context van de gebeurtenis. |
| resourceProviderName |De resourceprovider van de betrokken resource. |
| conditionType |Altijd 'gebeurtenis'. |
| name |De naam van de waarschuwingsregel. |
| id |Resource-ID van de waarschuwing. |
| description |Beschrijving van de waarschuwing instellen wanneer de waarschuwing is gemaakt. |
| subscriptionId |Azure-abonnement-ID. |
| timestamp |Tijd waarop de gebeurtenis is gegenereerd door de Azure-service die het verzoek heeft verwerkt. |
| resourceId |Resource-ID van de betrokken resource. |
| resourceGroupName |De naam van de resourcegroep voor de betrokken resource. |
| properties |Instellen van `<Key, Value>` paren (dat wil zeggen, `Dictionary<String, String>`) die vindt u informatie over de gebeurtenis. |
| gebeurtenis |Element met metagegevens van de gebeurtenis. |
| authorization |De Role-Based Access Control-eigenschappen van de gebeurtenis. Deze eigenschappen zijn meestal de actie, de rol en het bereik. |
| category |Categorie van de gebeurtenis. Ondersteunde waarden omvatten Beheersjablonen, waarschuwing, beveiliging, ServiceHealth en aanbevelingen. |
| Beller |E-mailadres van de gebruiker die de bewerking, UPN-claim of SPN claim op basis van beschikbaarheid uitgevoerd. Kan niet null zijn voor bepaalde systeemaanroepen. |
| correlationId |Meestal een GUID in de indeling van tekenreeks. Gebeurtenissen met correlationId deel uitmaken van dezelfde grotere actie en meestal een correlationId delen. |
| eventDescription |De beschrijving van de statische tekst van de gebeurtenis. |
| eventDataId |De unieke id voor de gebeurtenis. |
| eventSource |Naam van de Azure-service of de infrastructuur die de gebeurtenis is gegenereerd. |
| httpRequest |De aanvraag bevat meestal de clientRequestId, clientIpAddress en HTTP-methode (bijvoorbeeld plaatsen). |
| level |Een van de volgende waarden: Kritiek, fout, waarschuwing en ter informatie. |
| operationId |Meestal een GUID gedeeld tussen de gebeurtenissen die overeenkomt met één bewerking. |
| operationName |Naam van de bewerking. |
| properties |De eigenschappen van de gebeurtenis. |
| status |tekenreeks. Status van de bewerking. Algemene waarden zijn gestart, wordt uitgevoerd, is voltooid, is mislukt, actieve en opgelost. |
| subStatus |Omvat gewoonlijk het HTTP-statuscode van de bijbehorende REST-aanroep. Er kan ook andere tekenreeksen die een substatus beschrijven bevatten. Algemene substatus waarden zijn OK (HTTP-statuscode: 200), die zijn gemaakt (HTTP-statuscode: 201), geaccepteerd (HTTP-statuscode: 202), geen inhoud (HTTP-statuscode: 204), onjuiste aanvraag (HTTP-statuscode: 400), niet gevonden (HTTP-statuscode: 404), conflict (HTTP-statuscode: 409), een interne serverfout (HTTP-statuscode: 500), Service is niet beschikbaar (HTTP-statuscode: 503), en de time-out van Gateway (HTTP-statuscode: 504). |

Zie voor meer informatie voor een specifieke schema op ander activiteitenlogboekalarmen, [overzicht van de Azure-activiteitenlogboek](../../azure-monitor/platform/activity-logs-overview.md).

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het activiteitenlogboek](../../azure-monitor/platform/activity-logs-overview.md).
* [Azure automation-scripts (Runbooks) uit te voeren op Azure-waarschuwingen](https://go.microsoft.com/fwlink/?LinkId=627081).
* [Een logische app gebruiken voor het verzenden van een SMS-bericht via Twilio vanuit een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar het kan worden gewijzigd om te werken met een waarschuwing voor activiteitenlogboek.
* [Een logische app gebruiken op een Slack-bericht verzenden vanuit een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar het kan worden gewijzigd om te werken met een waarschuwing voor activiteitenlogboek.
* [Gebruik van een logische app een bericht verzenden naar een Azure-wachtrij vanuit een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar het kan worden gewijzigd om te werken met een waarschuwing voor activiteitenlogboek.


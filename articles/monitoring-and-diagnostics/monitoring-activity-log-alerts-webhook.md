---
title: Inzicht in het schema webhook in activiteit logboek waarschuwingen | Microsoft Docs
description: Meer informatie over het schema van de JSON die wordt gepost naar een webhook-URL wanneer een activiteit logboek waarschuwing wordt geactiveerd.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: 75c71bcd16573d4f4dd3377c623aa9b414aa3906
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a>Webhooks voor Azure activiteit logboek waarschuwingen
Als onderdeel van de definitie van een actiegroep, kunt u de webhook-eindpunten voor het ontvangen van meldingen van waarschuwingen activiteit logboek configureren. Met webhooks, kunt u deze meldingen met andere systemen voor na verwerking of aangepaste acties te routeren. Dit artikel laat zien hoe de nettolading voor de HTTP POST naar een webhook eruit ziet.

Zie voor meer informatie over activiteit logboek waarschuwingen hoe [Azure activiteit logboek waarschuwingen maken](monitoring-activity-log-alerts.md).

Zie voor informatie over actiegroepen, hoe [Actiegroepen maken](monitoring-action-groups.md).

## <a name="authenticate-the-webhook"></a>De webhook verifiëren
De webhook kan eventueel token gebaseerde verificatie gebruiken voor verificatie. De URI wordt opgeslagen met een token ID, bijvoorbeeld webhook `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.

## <a name="payload-schema"></a>De nettolading van schema
De JSON-nettolading opgenomen in de POST-bewerking verschilt op basis van de nettolading data.context.activityLog.eventSource veld.

###<a name="common"></a>Algemene
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
###<a name="administrative"></a>Beheerdersrechten
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
###<a name="servicehealth"></a>ServiceHealth
```json
{
    "schemaId": "unknown",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "properties": {
                    "title": "...",
                    "service": "...",
                    "region": "...",
                    "communication": "...",
                    "incidentType": "Incident",
                    "trackingId": "...",
                    "groupId": "...",
                    "impactStartTime": "3/29/2017 3:43:21 PM",
                    "impactMitigationTime": "3/29/2017 3:43:21 PM",
                    "eventCreationTime": "3/29/2017 3:43:21 PM",
                    "impactedServices": "[{...}]",
                    "defaultLanguageTitle": "...",
                    "defaultLanguageContent": "...",
                    "stage": "Active",
                    "communicationId": "...",
                    "version": "0.1"
                }
            }
        },
        "properties": {}
    }
}
```

Zie voor specifieke schema-informatie over de service health melding activiteit logboek waarschuwingen, [Service health meldingen](monitoring-service-notifications.md).

Zie voor meer informatie voor een specifiek schema bij alle waarschuwingen van andere activiteit logboek, [overzicht van het Azure activiteitenlogboek](monitoring-overview-activity-logs.md).

| Elementnaam | Beschrijving |
| --- | --- |
| status |Gebruikt voor metrische waarschuwingen. Altijd ingesteld op 'actief' voor activiteit logboek waarschuwingen. |
| Context |De context van de gebeurtenis. |
| resourceProviderName |De resourceprovider van de betrokken resource. |
| conditionType |Altijd 'gebeurtenis'. |
| naam |Naam van de waarschuwingsregel. |
| id |Bron-ID van de waarschuwing. |
| Beschrijving |Beschrijving van de waarschuwing ingesteld wanneer de waarschuwing wordt gemaakt. |
| subscriptionId |Azure-abonnement-ID. |
| tijdstempel |Tijd waarop de gebeurtenis is gegenereerd door de Azure-service die de aanvraag verwerkt. |
| resourceId |Bron-ID van de betrokken resource. |
| resourceGroupName |De naam van de resourcegroep voor de betrokken resource. |
| properties |Een set `<Key, Value>` paren (dat wil zeggen, `Dictionary<String, String>`) die bevat details over de gebeurtenis. |
| Gebeurtenis |Element met metagegevens over de gebeurtenis. |
| Autorisatie |De eigenschappen van de gebeurtenis die toegangsbeheer op basis van rollen. Deze eigenschappen zijn meestal de actie, de rol en het bereik. |
| category |De categorie van de gebeurtenis. Ondersteunde waarden zijn Administrative, waarschuwing, beveiliging, ServiceHealth en aanbeveling. |
| aanroeper |E-mailadres van de gebruiker die de bewerking, UPN-claim of SPN claim op basis van beschikbaarheid uitgevoerd. Kan niet null zijn voor bepaalde systeemaanroepen zijn. |
| correlationId |Meestal een GUID in de indeling van tekenreeks. Gebeurtenissen met correlationId deel uitmaken van dezelfde groter actie en meestal delen een correlationId. |
| eventDescription |De beschrijving van de statische tekst van de gebeurtenis. |
| eventDataId |De unieke id voor de gebeurtenis. |
| EventSource |Naam van de Azure-service of de infrastructuur die de gebeurtenis heeft gegenereerd. |
| httpRequest |De aanvraag bevat meestal de clientRequestId, clientIpAddress en HTTP-methode (bijvoorbeeld plaatsen). |
| niveau |Een van de volgende waarden: kritiek, fout, waarschuwing, informatief van aard en uitgebreid. |
| operationId |Meestal een GUID gedeeld door de gebeurtenissen die overeenkomt met één bewerking. |
| operationName |De naam van de bewerking. |
| properties |Eigenschappen van de gebeurtenis. |
| status |De tekenreeks. De status van de bewerking. Algemene waarden zijn gestart, wordt uitgevoerd, geslaagd, mislukt, actief en opgelost. |
| subStatus |Omvat gewoonlijk het HTTP-statuscode van de bijbehorende REST-aanroep. Het kan ook andere tekenreeksen die een substatus beschrijven omvatten. Algemene substatus waarden zijn OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), geen inhoud (HTTP-statuscode: 204), ongeldige aanvraag (HTTP-statuscode: 400), niet vinden (HTTP-statuscode: 404), Conflict (HTTP-statuscode: 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar (HTTP-statuscode: 503), en de time-out van Gateway (HTTP-statuscode : 504). |

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het activiteitenlogboek](monitoring-overview-activity-logs.md).
* [Azure automatiseringsscripts (Runbooks) uitvoeren op Azure waarschuwingen](http://go.microsoft.com/fwlink/?LinkId=627081).
* [Een logische app gebruiken voor het verzenden van een SMS-bericht via Twilio vanuit een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar het kan worden gewijzigd om te werken met een activiteit logboek-waarschuwing.
* [Een logische app gebruiken voor het verzenden van een toegestane bericht door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar het kan worden gewijzigd om te werken met een activiteit logboek-waarschuwing.
* [Gebruik van een logische app een bericht verzenden naar een Azure-wachtrij door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar het kan worden gewijzigd om te werken met een activiteit logboek-waarschuwing.

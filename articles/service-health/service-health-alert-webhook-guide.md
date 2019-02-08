---
title: Meldingen van de status van bestaande probleem management systemen met behulp van een webhook configureren | Microsoft Docs
description: Ophalen persoonlijke meldingen over service health-gebeurtenissen naar uw bestaande systeem voor probleembeheer.
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.workload: Supportability
ms.date: 3/27/2018
ms.openlocfilehash: 898f2eca0b6cc115f56bcae195c58c6eef190694
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884865"
---
# <a name="configure-health-notifications-for-existing-problem-management-systems-using-a-webhook"></a>Meldingen van de status van bestaande probleem management systemen met behulp van een webhook configureren

In dit artikel leest u hoe het configureren van uw waarschuwingen van de health service om gegevens te verzenden via Webhooks aan uw bestaande notification-systeem.

Vandaag, kunt u waarschuwingen van de health service configureren zodat wanneer een Azure-Service-Incident van invloed is op u, u een melding via SMS-bericht of e-mail.
Maar mogelijk hebt u al bestaande externe meldingssysteem op locatie die u wilt gebruiken.
Dit document ziet u de belangrijkste onderdelen van de nettolading van de webhook en hoe u aangepaste waarschuwingen om te worden geïnformeerd bij problemen met de service kunt maken.

Als u gebruiken van een vooraf geconfigureerde integratie wilt, raadpleegt u het:
* [Waarschuwingen configureren met ServiceNow](service-health-alert-webhook-servicenow.md)
* [Waarschuwingen configureren met PagerDuty](service-health-alert-webhook-pagerduty.md)
* [Waarschuwingen configureren met OpsGenie](service-health-alert-webhook-opsgenie.md)

## <a name="configuring-a-custom-notification-using-the-service-health-webhook-payload"></a>Een aangepaste melding met behulp van de nettolading van de service health webhook configureren
Als u wilt instellen van uw eigen aangepaste webhook-integratie, moet u de JSON-nettolading die is verzonden tijdens servicestatusmeldingen parseren.

Zoek [hier om een voorbeeld te zien](../azure-monitor/platform/activity-log-alerts-webhook.md) van wat de `ServiceHealth` webhookpayload ziet eruit als.

U kunt detecteren dit een statuswaarschuwing voor de service is door te kijken `context.eventSource == "ServiceHealth"`. Van daaruit worden de eigenschappen die het meest relevant zijn voor het opnemen:
 * `data.context.activityLog.status`
 * `data.context.activityLog.level`
 * `data.context.activityLog.subscriptionId`
 * `data.context.activityLog.properties.title`
 * `data.context.activityLog.properties.impactStartTime`
 * `data.context.activityLog.properties.communication`
 * `data.context.activityLog.properties.impactedServices`
 * `data.context.activityLog.properties.trackingId`

## <a name="creating-a-direct-link-to-the-service-health-dashboard-for-an-incident"></a>Het maken van een directe koppeling naar het dashboard status van de Service voor een incident
U kunt een directe koppeling naar uw Service Health-dashboard op bureaublad of mobiel maken door het genereren van een speciale URL. Gebruik de `trackingId`, evenals de eerste en laatste vier cijfers van uw `subscriptionId`, om te vormen:
```
https://app.azure.com/h/<trackingId>/<first and last three digits of subscriptionId>
```

Bijvoorbeeld, als uw `subscriptionId` is `bba14129-e895-429b-8809-278e836ecdb3` en uw `trackingId` is `0DET-URB`, dan is de URL van de Health-Service:

```
https://app.azure.com/h/0DET-URB/bbadb3
```

## <a name="using-the-level-to-detect-the-severity-of-the-issue"></a>Met behulp van het niveau voor het detecteren van de ernst van het probleem
Van de laagste prioriteit aan de hoogste prioriteit, de `level` eigenschap in de nettolading kan dit een van de `Informational`, `Warning`, `Error`, en `Critical`.

## <a name="parsing-the-impacted-services-to-understand-the-full-scope-of-the-incident"></a>Het parseren van de betrokken services voor meer informatie over het volledige bereik van het incident
Waarschuwingen van de health service kunnen u informeren over problemen in meerdere regio's en services. Als u de volledige details, moet u de waarde van parseren `impactedServices`.
De inhoud binnen een [JSON escape](http://json.org/) string, wanneer unescaped, bevat een ander JSON-object dat regelmatig kan worden geparseerd.

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

Als volgt uit:

```json
[
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia East"
         },
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"Alerts & Metrics"
   },
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"App Service"
   }
]
```

Dit laat zien dat er problemen met 'Waarschuwingen en metrische gegevens' in Australië-Oost en Zuidoost, evenals de problemen met het "App Service" in Australië-Zuidoost zijn.


## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen van de integratie van webhooks via een HTTP POST-aanvraag
1. Maak de nettolading van de service health die u wilt verzenden. U vindt een voorbeeld van de service health webhookpayload op [Webhooks voor Azure-activiteit waarschuwingen voor activiteitenlogboeken](../azure-monitor/platform/activity-log-alerts-webhook.md).

2. Maak een HTTP POST-aanvraag als volgt:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
3. U ontvangt een `2XX - Successful` antwoord.

4. Ga naar [PagerDuty](https://www.pagerduty.com/) om te bevestigen dat uw integratie met succes is ingesteld.

## <a name="next-steps"></a>Volgende stappen
- Controleer de [activiteit log waarschuwing webhook-schema](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Meer informatie over [health servicemeldingen](../azure-monitor/platform/service-notifications.md).
- Meer informatie over [actiegroepen](../azure-monitor/platform/action-groups.md).
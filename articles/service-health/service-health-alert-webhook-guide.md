---
title: Health meldingen configureren voor bestaande probleem beheersystemen met behulp van een webhook | Microsoft Docs
description: Aangepaste meldingen over gebeurtenissen van de health service aan uw bestaande beheersysteem van het probleem worden opgehaald.
author: shawntabrizi
manager: scotthit
editor: ''
services: service-health
documentationcenter: service-health
ms.assetid: ''
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: shtabriz
ms.openlocfilehash: 0e233fe537ea37da97ebe5d4e8221d24f656fd10
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="configure-health-notifications-for-existing-problem-management-systems-using-a-webhook"></a>Meldingen voor bestaande probleem management systems met behulp van een webhook health configureren

In dit artikel laat zien hoe uw waarschuwingen van de health service voor het verzenden van gegevens via Webhooks naar uw bestaande waarschuwingssysteem configureren.

Vandaag de dag, kunt u waarschuwingen van de health service configureren zodat wanneer een Incident Azure-Service is van invloed op u, u een melding krijgen via SMS of e-mail.
Echter, u hebt mogelijk al bestaande externe meldingen system op locatie die u wilt gebruiken.
Dit document beschrijft de belangrijkste onderdelen van de nettolading van de webhook en hoe u aangepaste waarschuwingen om u te waarschuwen wanneer u invloed hebben op problemen met de service kunt maken.

Als u gebruiken van een vooraf geconfigureerde integratie wilt, raadpleegt u het:
* [Waarschuwingen configureren met ServiceNow](service-health-alert-webhook-servicenow.md)
* [Waarschuwingen met PagerDuty configureren](service-health-alert-webhook-pagerduty.md)
* [Waarschuwingen met OpsGenie configureren](service-health-alert-webhook-opsgenie.md)

## <a name="configuring-a-custom-notification-using-the-service-health-webhook-payload"></a>Een aangepaste melding met behulp van de nettolading van de service health webhook configureren
Als u uw eigen aangepaste webhook-integratie instellen wilt, moet u de JSON-nettolading die worden verzonden tijdens servicestatusmeldingen parseren.

Zoek [hier een voorbeeld](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md) wat de `ServiceHealth` webhook nettolading eruit ziet.

U kunt detecteren dit een melding van de health service is door te kijken `context.eventSource == "ServiceHealth"`. Van daaruit zijn de eigenschappen die het meest relevant zijn voor het opnemen:
 * `data.context.activityLog.status`
 * `data.context.activityLog.level`
 * `data.context.activityLog.subscriptionId`
 * `data.context.activityLog.properties.title`
 * `data.context.activityLog.properties.impactStartTime`
 * `data.context.activityLog.properties.communication`
 * `data.context.activityLog.properties.impactedServices`
 * `data.context.activityLog.properties.trackingId`

## <a name="creating-a-direct-link-to-the-service-health-dashboard-for-an-incident"></a>Een directe koppeling naar het dashboard voor servicestatus voor een incident maken
U kunt een directe koppeling maken aan uw dashboard status van de Service op het bureaublad of mobiele een gespecialiseerde URL te genereren. Gebruik de `trackingId`, evenals de eerste en laatste vier cijfers van uw `subscriptionId`, om te vormen:
```
https://app.azure.com/h/<trackingId>/<first and last three digits of subscriptionId>
```

Bijvoorbeeld, als uw `subscriptionId` is `bba14129-e895-429b-8809-278e836ecdb3` en uw `trackingId` is `0DET-URB`, dan is de URL van de Health-Service:

```
https://app.azure.com/h/0DET-URB/bbadb3
```

## <a name="using-the-level-to-detect-the-severity-of-the-issue"></a>Met behulp van het niveau voor het detecteren van de ernst van het probleem
Van de laagste prioriteit aan de hoogste prioriteit, de `level` eigenschap in de nettolading zijn `Informational`, `Warning`, `Error`, en `Critical`.

## <a name="parsing-the-impacted-services-to-understand-the-full-scope-of-the-incident"></a>Bij het parseren van de betrokken services om te begrijpen van het volledige bereik van het incident
Waarschuwingen van de health service kunnen u informeren over problemen in meerdere regio's en services. Als u de volledige details, moet u de waarde van parseren `impactedServices`.
De inhoud in een [JSON escape-teken](http://json.org/) tekenreeks, wanneer unescaped, bevat een ander JSON-object dat regelmatig kan worden geparseerd.

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

Dit betekent dat er problemen met 'Waarschuwingen & metrische gegevens' in Australië-Oost en Zuidoost, evenals problemen met de 'App Service' in Australië-Zuidoost zijn.


## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen van uw integratie webhook via een HTTP POST-aanvraag
1. Maak de nettolading van de service health die u wilt verzenden. U vindt een voorbeeld van de service health webhook nettolading op [Webhooks voor Azure activiteit waarschuwingen melden](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Hiermee maakt u een HTTP POST-aanvraag als volgt:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
3. U ontvangt een `2XX - Successful` antwoord.

4. Ga naar [PagerDuty](https://www.pagerduty.com/) om te bevestigen dat uw integratie met succes is ingesteld.

## <a name="next-steps"></a>Volgende stappen
- Controleer de [activiteit logboek waarschuwing webhook schema](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Meer informatie over [service health meldingen](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Meer informatie over [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md).
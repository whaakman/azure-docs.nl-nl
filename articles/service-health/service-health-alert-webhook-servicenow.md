---
title: Waarschuwingen van Azure service health configureren met ServiceNow | Microsoft Docs
description: Aangepaste meldingen over gebeurtenissen aan uw ServiceNow-exemplaar van de health service ophalen.
author: shawntabrizi
services: service-health
documentationcenter: service-health
ms.assetid: 
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: shtabriz
ms.openlocfilehash: 625718ab82443c897d1b15c2eac51dea3d0dfeb4
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="configure-service-health-alerts-with-servicenow"></a>Waarschuwingen van de health service configureren met ServiceNow

In dit artikel leest u hoe waarschuwingen van Azure service health integreren met ServiceNow met behulp van een webhook. Na het instellen van de webhook-integratie met uw ServiceNow-exemplaar, krijgt u waarschuwingen via uw bestaande infrastructuur van de melding wanneer u invloed hebben op problemen met de Azure-service. Telkens wanneer een waarschuwing de status van de Azure-Service wordt gestart, wordt een webhook met ServiceNow van een script vastgelegd REST API aangeroepen.

## <a name="creating-a-scripted-rest-api-in-servicenow"></a>Maken van een script REST-API in ServiceNow
1.  Zorg dat u hebt zich heeft aangemeld en bent aangemeld bij uw [ServiceNow](https://www.servicenow.com/) account.

2.  Navigeer naar de **System webservices** sectie in ServiceNow en selecteer **REST-API's in een script vastgelegd**.

    ![De sectie 'Een script vastgelegd Web Service' in ServiceNow](./media/webhook-alerts/servicenow-sws-section.png)

3.  Selecteer **nieuw** voor het maken van een nieuwe script REST-service.
 
    ![De knop 'nieuwe script REST-API in ServiceNow](./media/webhook-alerts/servicenow-new-button.png)

4.  Toevoegen een **naam** op uw REST-API en de **API ID** naar `azureservicehealth`.

5.  Selecteer **indienen**.

    ![De 'REST API-instellingen' in ServiceNow](./media/webhook-alerts/servicenow-restapi-settings.png)

6.  Selecteer de REST-API die u hebt gemaakt, en klikt u onder de **Resources** tabblad Selecteer **nieuw**.

    ![Het 'Resource-tabblad' in ServiceNow](./media/webhook-alerts/servicenow-resources-tab.png)

7.  **Naam** uw nieuwe resource `event` en wijzig de **HTTP-methode** naar `POST`.

8.  In de **Script** sectie, voeg de volgende JavaScript-code:

    >[!NOTE]
    >Werk de `<secret>`,`<group>`, en `<email>` waarde in het onderstaande script.
    >* `<secret>`moet een willekeurige tekenreeks, zoals een GUID
    >* `<group>`moet de ServiceNow-groep die u wilt het incident aan toewijzen
    >* `<email>`moet de specifieke persoon die u wilt toewijzen van het incident (optioneel)
    >

    ```javascript
    (function process( /*RESTAPIRequest*/ request, /*RESTAPIResponse*/ response) {
        var apiKey = request.queryParams['apiKey'];
        var secret = '<secret>';
        if (apiKey == secret) {
            var event = request.body.data;
            var responseBody = {};
            if (event.data.context.activityLog.operationName == 'Microsoft.ServiceHealth/incident/action') {
                var inc = new GlideRecord('incident');
                var incidentExists = false;
                inc.addQuery('number', event.data.context.activityLog.properties.trackingId);
                inc.query();
                if (inc.hasNext()) {
                    incidentExists = true;
                    inc.next();
                } else {
                    inc.initialize();
                }
                var short_description = "Azure Service Health";
                if (event.data.context.activityLog.properties.incidentType == "Incident") {
                    short_description += " - Service Issue - ";
                } else if (event.data.context.activityLog.properties.incidentType == "Maintenance") {
                    short_description += " - Planned Maintenance - ";
                } else if (event.data.context.activityLog.properties.incidentType == "Information" || event.data.context.activityLog.properties.incidentType == "ActionRequired") {
                    short_description += " - Health Advisory - ";
                }
                short_description += event.data.context.activityLog.properties.title;
                inc.short_description = short_description;
                inc.description = event.data.context.activityLog.properties.communication;
                inc.work_notes = "Impacted subscription: " + event.data.context.activityLog.subscriptionId;
                if (incidentExists) {
                    if (event.data.context.activityLog.properties.stage == 'Active') {
                        inc.state = 2;
                    } else if (event.data.context.activityLog.properties.stage == 'Resolved') {
                        inc.state = 6;
                    } else if (event.data.context.activityLog.properties.stage == 'Closed') {
                        inc.state = 7;
                    }
                    inc.update();
                    responseBody.message = "Incident updated.";
                } else {
                    inc.number = event.data.context.activityLog.properties.trackingId;
                    inc.state = 1;
                    inc.impact = 2;
                    inc.urgency = 2;
                    inc.priority = 2;
                    inc.assigned_to = '<email>';
                    inc.assignment_group.setDisplayValue('<group>');
                    var subscriptionId = event.data.context.activityLog.subscriptionId;
                    var comments = "Azure portal Link: https://app.azure.com/h";
                    comments += "/" + event.data.context.activityLog.properties.trackingId;
                    comments += "/" + subscriptionId.substring(0, 3) + subscriptionId.slice(-3);
                    var impactedServices = JSON.parse(event.data.context.activityLog.properties.impactedServices);
                    var impactedServicesFormatted = "";
                    for (var i = 0; i < impactedServices.length; i++) {
                        impactedServicesFormatted += impactedServices[i].ServiceName + ": ";
                        for (var j = 0; j < impactedServices[i].ImpactedRegions.length; j++) {
                            if (j != 0) {
                                impactedServicesFormatted += ", ";
                            }
                            impactedServicesFormatted += impactedServices[i].ImpactedRegions[j].RegionName;
                        }

                        impactedServicesFormatted += "\n";

                    }
                    comments += "\n\nImpacted Services:\n" + impactedServicesFormatted;
                    inc.comments = comments;
                    inc.insert();
                    responseBody.message = "Incident created.";
                }
            } else {
                responseBody.message = "Hello from the other side!";
            }
            response.setBody(responseBody);
        } else {
            var unauthorized = new sn_ws_err.ServiceError();
            unauthorized.setStatus(401);
            unauthorized.setMessage('Invalid apiKey');
            response.setError(unauthorized);
        }
    })(request, response);
    ```

9.  Schakel op het tabblad Beveiliging **is verificatie vereist** en selecteer **indienen**. De `<secret>` u set beveiligt deze API in plaats daarvan.

    ![Het selectievakje 'Verificatie is vereist' in ServiceNow](./media/webhook-alerts/servicenow-resource-settings.png)

10.  Op de sectie REST-API's in een script vastgelegd, zult u de **Base API pad** voor uw nieuwe REST-API:

     ![De 'baseren API pad' ServiceNow](./media/webhook-alerts/servicenow-base-api-path.png)

11.  De URL van uw volledige integratie ziet eruit als:
        
         https://<yourInstanceName>.service-now.com/<baseApiPath>?apiKey=<secret>


## <a name="create-an-alert-using-servicenow-in-the-azure-portal"></a>Maken van een waarschuwing met ServiceNow in de Azure-portal
### <a name="for-a-new-action-group"></a>Voor een nieuwe actiegroep:
1. Volg de stappen 1 tot en met 8 in [in dit artikel](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md) een waarschuwing wilt maken met een nieuwe actiegroep.

2. Definiëren in de lijst met **acties**:

    a. **Actietype:** *Webhook*

    b. **Details:** de ServiceNow **URL-integratie** u eerder hebt opgeslagen.

    c. **Naam:** Webhook van naam-, alias- of -id.

3. Selecteer **opslaan** wanneer u klaar bent voor het maken van de waarschuwing.

### <a name="for-an-existing-action-group"></a>Voor een bestaande actiegroep:
1. In de [Azure-portal](https://portal.azure.com/), selecteer **Monitor**.

2. In de **instellingen** sectie **actiegroepen**.

3. Zoek en selecteer de actiegroep die u wilt bewerken.

4. Toevoegen aan de lijst met **acties**:

    a. **Actietype:** *Webhook*

    b. **Details:** de ServiceNow **URL-integratie** u eerder hebt opgeslagen.

    c. **Naam:** Webhook van naam-, alias- of -id.

5. Selecteer **opslaan** wanneer u klaar bent voor het bijwerken van de groep in te grijpen.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen van uw integratie webhook via een HTTP POST-aanvraag
1. Maak de nettolading van de status van de Service die u wilt verzenden. U vindt een voorbeeld van de servicestatus webhook nettolading op [Webhooks voor Azure activiteit waarschuwingen melden](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Hiermee maakt u een HTTP POST-aanvraag als volgt:

    ```
    POST        https://<yourInstanceName>.service-now.com/<baseApiPath>?apiKey=<secret>

    HEADERS     Content-Type: application/json

    BODY        <Service Health payload>
    ```
3. U ontvangt een `200 OK` antwoord met het bericht "Incident gemaakt."

4. Ga naar [ServiceNow](https://www.servicenow.com/) om te bevestigen dat uw integratie met succes is ingesteld.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [webhook meldingen configureren voor bestaande beheersystemen voor probleem](service-health-alert-webhook-guide.md).
- Controleer de [activiteit logboek waarschuwing webhook schema](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Meer informatie over [service health meldingen](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Meer informatie over [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md).
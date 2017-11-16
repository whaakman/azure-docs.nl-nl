---
title: Waarschuwingen van Azure service health configureren met PagerDuty | Microsoft Docs
description: Aangepaste meldingen over gebeurtenissen van de health service aan uw exemplaar PagerDuty ophalen.
author: shawntabrizi
manager: scotthit
editor: 
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
ms.openlocfilehash: 9edcb727b9f0af348cacd5533523c4f2e8214703
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="configure-service-health-alerts-with-pagerduty"></a>Waarschuwingen van de health service configureren met PagerDuty

In dit artikel leest u hoe u Azure servicestatusmeldingen via PagerDuty instelt met behulp van een webhook. Met behulp van [PagerDuty](https://www.pagerduty.com/)van type voor integratie met aangepaste Microsoft Azure, kunt u moeiteloos servicestatus waarschuwingen toevoegen aan uw nieuwe of bestaande PagerDuty-services.

## <a name="creating-a-service-health-integration-url-in-pagerduty"></a>Maken van een service-URL voor de integratie van health in PagerDuty
1.  Zorg dat u hebt zich heeft aangemeld en bent aangemeld bij uw [PagerDuty](https://www.pagerduty.com/) account.

2.  Navigeer naar de **Services** sectie in PagerDuty.

    ![De sectie 'Services' in PagerDuty](./media/webhook-alerts/pagerduty-services-section.png)

3.  Selecteer **nieuwe Service toevoegen** of open een bestaande service die u hebt ingesteld.

4.  In de **instellingen voor Adreslijstintegratie**, selecteert u het volgende:

    a. **Integratietype**: Microsoft Azure

    b. **De naam van de integratie**: \<naam\>

    ![De 'integratie-instellingen' in PagerDuty](./media/webhook-alerts/pagerduty-integration-settings.png)

5.  Vul alle vereiste velden en selecteer **toevoegen**.

6.  Deze nieuwe integratie en de kopie te openen en opslaan de **URL-integratie**.

    ![De 'integratie-URL' in PagerDuty](./media/webhook-alerts/pagerduty-integration-url.png)

## <a name="create-an-alert-using-pagerduty-in-the-azure-portal"></a>Maken van een waarschuwing met PagerDuty in de Azure-portal
### <a name="for-a-new-action-group"></a>Voor een nieuwe actiegroep:
1. Volg de stappen 1 tot en met 8 in [een waarschuwing op een melding van de health service voor een nieuwe actiegroep maken met behulp van de Azure-portal](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md).

2. Definiëren in de lijst met **acties**:

    a. **Actietype:** *Webhook*

    b. **Details:** de PagerDuty **URL-integratie** u eerder hebt opgeslagen.

    c. **Naam:** Webhook van naam-, alias- of -id.

3. Selecteer **opslaan** wanneer u klaar bent voor het maken van de waarschuwing.

### <a name="for-an-existing-action-group"></a>Voor een bestaande actiegroep:
1. In de [Azure-portal](https://portal.azure.com/), selecteer **Monitor**.

2. In de **instellingen** sectie **actiegroepen**.

3. Zoek en selecteer de actiegroep die u wilt bewerken.

4. Toevoegen aan de lijst met **acties**:

    a. **Actietype:** *Webhook*

    b. **Details:** de PagerDuty **URL-integratie** u eerder hebt opgeslagen.

    c. **Naam:** Webhook van naam-, alias- of -id.

5. Selecteer **opslaan** wanneer u klaar bent voor het bijwerken van de groep in te grijpen.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen van uw integratie webhook via een HTTP POST-aanvraag
1. Maak de nettolading van de status van de Service die u wilt verzenden. U vindt een voorbeeld van de servicestatus webhook nettolading op [Webhooks voor Azure activiteit waarschuwingen melden](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Hiermee maakt u een HTTP POST-aanvraag als volgt:

    ```
    POST        https://events.pagerduty.com/integration/<IntegrationKey>/enqueue

    HEADERS     Content-Type: application/json

    BODY        <Service Health payload>
    ```
3. U ontvangt een `202 Accepted` met een bericht met uw 'gebeurtenis-ID."

4. Ga naar [PagerDuty](https://www.pagerduty.com/) om te bevestigen dat uw integratie met succes is ingesteld.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [webhook meldingen configureren voor bestaande beheersystemen voor probleem](service-health-alert-webhook-guide.md).
- Controleer de [activiteit logboek waarschuwing webhook schema](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Meer informatie over [service health meldingen](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Meer informatie over [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md).
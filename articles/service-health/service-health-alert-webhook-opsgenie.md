---
title: Waarschuwingen van Azure service health configureren met OpsGenie | Microsoft Docs
description: Aangepaste meldingen over gebeurtenissen van de health service aan uw exemplaar OpsGenie ophalen.
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
ms.openlocfilehash: 6b8017f62dd895219f1d2cdac40f0efdf2db6c93
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30179341"
---
# <a name="configure-service-health-alerts-with-opsgenie"></a>Waarschuwingen van de health service configureren met OpsGenie

In dit artikel leest u hoe Azure-service health waarschuwingen instellen met OpsGenie met behulp van een webhook. Met behulp van [OpsGenie](https://www.opsgenie.com/)van Azure Service Health-integratie kunt u waarschuwingen van Azure Service Health naar OpsGenie doorsturen. OpsGenie kunt bepalen de juiste mensen op de hoogte wilt op basis van schema's op oproep, e, tekstberichten (SMS), telefoongesprekken, iOS en Android-pushmeldingen met en escaleren waarschuwingen totdat de waarschuwing is bevestigd of gesloten.

## <a name="creating-a-service-health-integration-url-in-opsgenie"></a>Maken van een service-URL voor de integratie van health in OpsGenie
1.  Zorg dat u hebt zich heeft aangemeld en bent aangemeld bij uw [OpsGenie](https://www.opsgenie.com/) account.

2.  Navigeer naar de **integraties** sectie in OpsGenie.

    ![De sectie 'Integraties' in OpsGenie](./media/webhook-alerts/opsgenie-integrations-section.png)

3.  Selecteer de **Azure-servicestatus** integratie knop.

    ![De 'Azure Service Health-knop' in OpsGenie](./media/webhook-alerts/opsgenie-azureservicehealth-button.png)

4.  **Naam** de waarschuwing en geef de **toegewezen aan het Team** veld.

5.  Vul de andere velden zoals **ontvangers**, **ingeschakeld**, en **onderdrukken meldingen**.

6.  Kopieer en sla de **URL-integratie**, die al moet bevatten uw `apiKey` toegevoegd aan het einde.

    ![De 'integratie-URL' in OpsGenie](./media/webhook-alerts/opsgenie-integration-url.png)

7.  Selecteer **integratie opslaan**

## <a name="create-an-alert-using-opsgenie-in-the-azure-portal"></a>Maken van een waarschuwing met OpsGenie in de Azure-portal
### <a name="for-a-new-action-group"></a>Voor een nieuwe actiegroep:
1. Volg de stappen 1 tot en met 8 in [een waarschuwing op een melding van de health service voor een nieuwe actiegroep maken met behulp van de Azure-portal](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md).

2. Definiëren in de lijst met **acties**:

    a. **Actietype:** *Webhook*

    b. **Details:** de OpsGenie **URL-integratie** u eerder hebt opgeslagen.

    c. **Naam:** Webhook van naam-, alias- of -id.

3. Selecteer **opslaan** wanneer u klaar bent voor het maken van de waarschuwing.

### <a name="for-an-existing-action-group"></a>Voor een bestaande actiegroep:
1. In de [Azure-portal](https://portal.azure.com/), selecteer **Monitor**.

2. In de **instellingen** sectie **actiegroepen**.

3. Zoek en selecteer de actiegroep die u wilt bewerken.

4. Toevoegen aan de lijst met **acties**:

    a. **Actietype:** *Webhook*

    b. **Details:** de OpsGenie **URL-integratie** u eerder hebt opgeslagen.

    c. **Naam:** Webhook van naam-, alias- of -id.

5. Selecteer **opslaan** wanneer u klaar bent voor het bijwerken van de groep in te grijpen.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen van uw integratie webhook via een HTTP POST-aanvraag
1. Maak de nettolading van de service health die u wilt verzenden. U vindt een voorbeeld van de service health webhook nettolading op [Webhooks voor Azure activiteit waarschuwingen melden](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Hiermee maakt u een HTTP POST-aanvraag als volgt:

    ```
    POST        https://api.opsgenie.com/v1/json/azureservicehealth?apiKey=<APIKEY>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
3. U ontvangt een `200 OK` antwoord met het bericht van de status 'mislukt'.

4. Ga naar [OpsGenie](https://www.opsgenie.com/) om te bevestigen dat uw integratie met succes is ingesteld.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [webhook meldingen configureren voor bestaande beheersystemen voor probleem](service-health-alert-webhook-guide.md).
- Controleer de [activiteit logboek waarschuwing webhook schema](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Meer informatie over [service health meldingen](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Meer informatie over [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md).
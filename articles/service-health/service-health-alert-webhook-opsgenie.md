---
title: Azure service health waarschuwingen configureren met OpsGenie | Microsoft Docs
description: Ophalen persoonlijke meldingen over service health-gebeurtenissen naar uw OpsGenie-exemplaar.
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
ms.openlocfilehash: 6e9d44c4b64e41b04c40d0c511a638e539752da4
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435023"
---
# <a name="configure-service-health-alerts-with-opsgenie"></a>Service health waarschuwingen configureren met OpsGenie

Dit artikel ziet u hoe u Azure service health waarschuwingen met OpsGenie instelt met behulp van een webhook. Met behulp van [OpsGenie](https://www.opsgenie.com/)van Azure Service Health-integratie, kunt u Azure Service Health waarschuwingen doorsturen naar OpsGenie. OpsGenie kunt bepalen de juiste mensen op de hoogte stellen op basis van schema's op oproep, met behulp van e-mailbericht, tekstberichten (SMS), telefonische oproepen, iOS en Android pushmeldingen te verzenden, en de waarschuwingen totdat de waarschuwing is bevestigd of gesloten.

## <a name="creating-a-service-health-integration-url-in-opsgenie"></a>Het maken van een service-URL voor de integratie van health in OpsGenie
1.  Zorg ervoor dat u hebt zich heeft aangemeld en bent aangemeld bij uw [OpsGenie](https://www.opsgenie.com/) account.

1.  Navigeer naar de **integraties** sectie in OpsGenie.

    ![De sectie "Integraties" in OpsGenie](./media/webhook-alerts/opsgenie-integrations-section.png)

1.  Selecteer de **Azure Service Health** integratie-knop.

    ![De 'Azure Service Health-knop' in OpsGenie](./media/webhook-alerts/opsgenie-azureservicehealth-button.png)

1.  **Naam** de waarschuwing en geef de **toegewezen aan het Team** veld.

1.  Vul de andere velden, zoals **ontvangers**, **ingeschakeld**, en **onderdrukken meldingen**.

1.  Kopieer en bewaar de **URL voor de integratie**, die al moet bevatten uw `apiKey` toegevoegd aan het einde.

    ![De 'integratie-URL' in OpsGenie](./media/webhook-alerts/opsgenie-integration-url.png)

1.  Selecteer **integratie opslaan**

## <a name="create-an-alert-using-opsgenie-in-the-azure-portal"></a>Een waarschuwing met OpsGenie in Azure portal maken
### <a name="for-a-new-action-group"></a>Voor een nieuwe actiegroep:
1. Volg de stappen 1 tot en met 8 in [een waarschuwing op een melding van de health service voor een nieuwe actiegroep maken met behulp van de Azure-portal](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md).

1. In de lijst van definiëren **acties**:

    a. **Actietype:** *Webhook*

    b. **Details:** de OpsGenie **URL voor de integratie** u eerder hebt opgeslagen.

    c. **Naam:** van de Webhook-naam, alias of id.

1. Selecteer **opslaan** wanneer u klaar bent om de waarschuwing te genereren.

### <a name="for-an-existing-action-group"></a>Voor een bestaande actiegroep:
1. In de [Azure-portal](https://portal.azure.com/), selecteer **Monitor**.

1. In de **instellingen** sectie, selecteer **actiegroepen**.

1. Zoek en selecteer de actiegroep die u wilt bewerken.

1. Toevoegen aan de lijst met **acties**:

    a. **Actietype:** *Webhook*

    b. **Details:** de OpsGenie **URL voor de integratie** u eerder hebt opgeslagen.

    c. **Naam:** van de Webhook-naam, alias of id.

1. Selecteer **opslaan** wanneer u klaar bent voor het bijwerken van de actiegroep.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen van de integratie van webhooks via een HTTP POST-aanvraag
1. Maak de nettolading van de service health die u wilt verzenden. U vindt een voorbeeld van de service health webhookpayload op [Webhooks voor Azure-activiteit waarschuwingen voor activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

1. Maak een HTTP POST-aanvraag als volgt:

    ```
    POST        https://api.opsgenie.com/v1/json/azureservicehealth?apiKey=<APIKEY>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. U ontvangt een `200 OK` -antwoord met het bericht van de status 'voltooid'.

1. Ga naar [OpsGenie](https://www.opsgenie.com/) om te bevestigen dat uw integratie met succes is ingesteld.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [configureren van de webhook-meldingen voor bestaande beheersystemen voor probleem](service-health-alert-webhook-guide.md).
- Controleer de [activiteit log waarschuwing webhook-schema](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Meer informatie over [health servicemeldingen](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Meer informatie over [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md).
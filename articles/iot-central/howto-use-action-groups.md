---
title: Meerdere acties uitvoeren vanuit een regel voor Azure IoT Central | Microsoft Docs
description: Meerdere acties uitvoeren vanuit een enkele IoT Central-regel en herbruikbare groepen van acties die u vanuit meerdere regels uitvoeren kunt maken.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 03/19/2019
ms.topic: conceptual
ms.service: iot-central
manager: philmea
ms.openlocfilehash: 857d747fa691d1ec2b386d5931a7edea08b7e609
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522940"
---
# <a name="group-multiple-actions-to-run-from-one-or-more-rules"></a>Groep meerdere acties uit te voeren vanaf een of meer regels

*In dit artikel is van toepassing op builders en beheerders.*

In Azure IoT Central maakt u regels voor het uitvoeren van acties wanneer een voorwaarde wordt voldaan. Regels zijn gebaseerd op de telemetrie van apparaten of gebeurtenissen. U kunt bijvoorbeeld een operator melden wanneer de temperatuur van een apparaat een drempelwaarde overschrijdt. In dit artikel wordt beschreven hoe u [Azure Monitor](../azure-monitor/overview.md) *actiegroepen* meerdere acties toevoegen aan een regel voor IoT Central. U kunt een actiegroep koppelen aan meerdere regels. Een [actiegroep](../azure-monitor/platform/action-groups.md) is een verzameling van voorkeuren voor meldingen gedefinieerd door de eigenaar van een Azure-abonnement.

## <a name="prerequisites"></a>Vereisten

- Een betalen per gebruik-toepassing
- Een Azure-account en abonnement op Azure Monitor actie-groepen maken en beheren

## <a name="create-action-groups"></a>Actiegroepen maken

U kunt [maken en beheren van actiegroepen in Azure portal](../azure-monitor/platform/action-groups.md) of met een [Azure Resource Manager-sjabloon](../azure-monitor/platform/action-groups-create-resource-manager-template.md).

Een actiegroep die u kunt het volgende doen:

- Verzenden van meldingen, zoals een e-mailbericht, een SMS-bericht, of een normaal telefoongesprek.
- Voer een actie, zoals het aanroepen van een webhook.

De volgende schermafbeelding ziet u een actiegroep die e-mail en SMS-berichten worden verzonden en een webhook aanroept:

![Actiegroep](media/howto-use-action-groups/actiongroup.png)

Als u een actiegroep die u in een regel voor IoT Central, moet de actiegroep zich in hetzelfde Azure-abonnement als de IoT Central-toepassing.

## <a name="use-an-action-group"></a>Een actiegroep gebruiken

Voor het gebruik van een actiegroep die u in uw IoT Central-toepassing, moet u eerst een telemetrie- of gebeurtenis-regel maken. Wanneer u een actie aan de regel toevoegen, selecteer **Azure Monitor-actiegroepen**:

![Actie kiezen](media/howto-use-action-groups/chooseaction.png)

Kies een actiegroep die u in uw Azure-abonnement:

![Kies de actiegroep](media/howto-use-action-groups/chooseactiongroup.png)

Selecteer **Opslaan**. De actiegroep wordt nu weergegeven in de lijst met acties uit te voeren wanneer de regel wordt geactiveerd:

![Actiegroep opgeslagen](media/howto-use-action-groups/savedactiongroup.png)

De volgende tabel geeft een overzicht van de informatie verzonden naar de actietypen ondersteunde:

| Actietype | Uitvoerindeling |
| ----------- | -------------- |
| Email       | Standard IoT Central e-mailsjabloon |
| Sms         | Azure IoT Central-waarschuwing: ${applicationName} - "${ruleName}", geactiveerd op "${deviceName}' op ${triggerDate}, ${triggerTime} |
| Spraak       | Azure I.O.T Central-waarschuwing: regel "${ruleName}", geactiveerd op apparaat '${deviceName}' op ${triggerDate}, ${triggerTime} in de toepassing ${applicationName} |
| Webhook     | {'schemaId': "AzureIoTCentralRuleWebhook", "gegevens": {[reguliere webhookpayload](#payload)}} |

De volgende tekst is een voorbeeld van de SMS-bericht van een actiegroep:

`iotcentral: Azure IoT Central alert: Sample Contoso 22xu4spxjve - "Low pressure alert" triggered on "Refrigerator 2" at March 20, 2019 10:12 UTC`

<a id="payload"></a> De volgende JSON ziet u een voorbeeld-webhookpayload actie:

```json
{
  "schemaId":"AzureIoTCentralRuleWebhook",
  "data":{
    "id":"97ae27c4-17c5-4e13-9248-65c7a2c57a1b",
    "timestamp":"2019-03-20T10:53:17.059Z",
    "rule":{
      "id":"031b660e-528d-47bb-b33d-f1158d7e31bf",
      "name":"Low pressure alert",
      "enabled":true,
      "deviceTemplate":{
        "id":"c318d580-39fc-4aca-b995-843719821049",
        "version":"1.0.0"
      }
    },
    "device":{
      "id":"2383d8ba-c98c-403a-b4d5-8963859643bb",
      "name":"Refrigerator 2",
      "simulated":true,
      "deviceId":"2383d8ba-c98c-403a-b4d5-8963859643bb",
      "deviceTemplate":{
        "id":"c318d580-39fc-4aca-b995-843719821049",
        "version":"1.0.0"
      },
      "measurements":{
        "telemetry":{
           "pressure":343.269190673549
        }
      }
    },
    "application":{
      "id":"8e70742b-0d5c-4a1d-84f1-4dfd42e61c7b",
      "name":"Sample Contoso",
      "subdomain":"sample-contoso"
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over het gebruik van actiegroepen met regels, de voorgestelde volgende stap is te leren hoe u [beheren van uw apparaten](howto-manage-devices.md).

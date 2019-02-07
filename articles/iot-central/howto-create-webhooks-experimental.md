---
title: Webhooks maken van regels in Azure IoT Central | Microsoft Docs
description: Webhooks maken in Azure IoT Central op andere toepassingen automatisch een melding wanneer de regels worden geactiveerd.
author: viv-liu
ms.author: viviali
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 62599419d5634bea7cd25c93fbada8b472b95c4d
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55773344"
---
# <a name="create-webhook-actions-on-rules-in-azure-iot-central"></a>Webhookacties op regels in Azure IoT Central maken

*In dit onderwerp is van toepassing op builders en beheerders.*

Webhooks kunt u uw IoT Central-app verbinden met andere toepassingen en services voor externe bewaking en meldingen. Webhooks geven door andere toepassingen en services dat u verbinding maken wanneer een regel wordt geactiveerd in uw IoT Central-app automatisch aan. Uw IoT Central-app wordt een POST-aanvraag naar de andere toepassing HTTP-eindpunt verzenden wanneer een regel wordt geactiveerd. De nettolading bevat Apparaatdetails en de details van de regel-trigger. 

## <a name="how-to-set-up-the-webhook"></a>Over het instellen van de webhook
In dit voorbeeld maakt u verbinding met RequestBin om te worden geïnformeerd wanneer regels worden geactiveerd met behulp van webhooks. 

1. Open [RequestBin](http://requestbin.net/). 
1. Maak een nieuwe RequestBin en kopieer de **Bin-URL**. 
1. Maak een [telemetrie regel](howto-create-telemetry-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) of een [gebeurtenis regel](howto-create-event-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json). De regel niet opslaan en voeg een nieuwe actie toe.
    ![Webhook maken van het scherm](media/howto-create-webhooks-experimental/webhookcreate.png)
1. Kies de webhookactie en een weergavenaam invoeren en plakt u de Bin-URL als de Callback-URL. 
1. De regel niet opslaan.

Nu wanneer de regel wordt geactiveerd, u een nieuwe aanvraag in RequestBin worden weergegeven ziet.

## <a name="payload"></a>Nettolading
Wanneer een regel wordt geactiveerd, wordt een HTTP POST-aanvraag die naar de callback-URL met een json-payload met de metingen, apparaat, regel en toepassingsgegevens. Voor een regel telemetrie, kan de nettolading van de er uitzien als het volgende:

```json
{
    "id": "ID",
    "timestamp": "date-time",
    "device" : {
        "id":"ID",
        "name":  "Refrigerator1",
        "simulated" : true,
        "deviceId": "deviceID",
        "deviceTemplate":{
            "id": "ID",
            "version":"1.0.0"
        },
        "properties":{
            "device":{
                "firmwareversion":"1.0"
            },
            "cloud":{
                "location":"One Microsoft Way"
            }
        },
        "measurements":{
            "telemetry":{
                "temperature":20,
                "pressure":10
            }
        }

    },
    "rule": {
        "id": "ID",
        "name": "High temperature alert",
        "enabled": true,
        "deviceTemplate": {
            "id":"GUID",
            "version":"1.0.0"
        }
    },
    "application": {
        "id": "ID",
        "name": "Contoso app",
        "subdomain":"contoso-app"
    }
}
```

## <a name="known-limitations"></a>Bekende beperkingen
Er is momenteel geen programmatische manier van het abonnement/abonnement opzegt uit deze webhooks via een API.

Als u ideeën voor het verbeteren van deze functie hebt, post u uw suggesties voor onze [Uservoice-forum](https://feedback.azure.com/forums/911455-azure-iot-central).

## <a name="next-steps"></a>Volgende stappen
U hebt geleerd hoe u kunt instellen en gebruiken van webhooks, de voorgestelde volgende stap is om te verkennen [het bouwen van werkstromen in Microsoft Flow](howto-add-microsoft-flow-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

---
title: Azure Media Services-gebeurtenissen te routeren naar een aangepaste website-eindpunt | Microsoft Docs
description: Gebruik Azure gebeurtenis raster abonneren op Media Services taak statuswijzigingsgebeurtenis.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 2780438e715b6f6cb04d820c02d09f14e14b480f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="route-azure-media-services-events-to-a-custom-web-endpoint-using-cli"></a>Azure Media Services-gebeurtenissen te routeren naar een aangepaste website-eindpunt met CLI

Azure Event Grid is een gebeurtenisservice voor de cloud. In dit artikel kunt u de Azure CLI gebruiken voor een abonnement op Azure Media Services taak statuswijzigingsgebeurtenissen en gebeurtenis om het resultaat weer te geven. 

Meestal stuurt u gebeurtenissen naar een eindpunt dat reageert op de gebeurtenis, zoals een webhook of Azure-functie. Deze zelfstudie laat zien hoe maken en een webhook instellen.

Als u de stappen in dit artikel hebt voltooid, ziet u dat de gegevens van gebeurtenissen naar een eindpunt zijn verzonden.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij de [Azure-portal](http://portal.azure.com) en start **CloudShell** CLI-opdrachten uitvoeren, zoals wordt weergegeven in de volgende stappen.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

Zorg ervoor dat de waarden die u voor Opslagnaam van de naam van de Media Services-account, en resourcenaam gebruikt onthouden.

## <a name="enable-event-grid-resource-provider"></a>Gebeurtenis raster resourceprovider ingeschakeld

Eerste wat die u moet doen is Zorg ervoor dat u de gebeurtenis raster resourceprovider is ingeschakeld op uw abonnement hebt. 

In de **Azure** portal het volgende doen:

1. Ga naar de abonnementen.
2. Selecteer uw abonnement.
3. Selecteer onder instellingen Resourceproviders.
4. Zoek naar 'EventGrid'.
5. Zorg ervoor dat gebeurtenis raster is geregistreerd. Als dit niet het geval is, drukt u op de **registreren** knop.  

## <a name="create-a-generic-azure-function-webhook"></a>Een algemene webhook van de Azure-functie maken 

### <a name="create-a-message-endpoint"></a>Het eindpunt van een bericht maken

Voordat u abonneren op artikel van het raster gebeurtenis, maakt u een eindpunt dat de berichten worden verzameld zodat u ze kunt weergeven.

Maak een functie die wordt geactiveerd door een algemene webhook, zoals beschreven in de [generic webhook](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function) artikel. In deze zelfstudie de **C#** code wordt gebruikt.

Nadat de webhook is gemaakt, kopieert u de URL door te klikken op de *ophalen van de functie URL* koppeling aan de bovenkant van de **Azure** portal venster. U hoeft niet het laatste deel van de URL (*& clientID = standaard*).

![Maken van een webhook](./media/job-state-events-cli-how-to/generic_webhook_files.png)

### <a name="validate-the-webhook"></a>Valideren van de webhook

Wanneer u uw eigen webhook-eindpunt met raster gebeurtenis registreert, stuurt u een POST-aanvraag met een eenvoudige validatiecode om te bewijzen eindpunt eigendom. Uw app moet reageren door echo terug code voor de validatie. Gebeurtenis raster leveren niet gebeurtenissen aan webHook-eindpunten die nog niet gevalideerd. Zie voor meer informatie [gebeurtenis raster beveiligings- en verificatie](https://docs.microsoft.com/azure/event-grid/security-authentication). Deze sectie definieert twee onderdelen die moeten worden gedefinieerd voor de validatie slaagt.

#### <a name="update-the-source-code"></a>Werk de broncode

Nadat u uw webhook gemaakt de **run.csx** bestand wordt weergegeven in de browser. De standaardcode vervangen door de volgende code. 

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"Webhook was triggered!");

    string jsonContent = await req.Content.ReadAsStringAsync();
    string eventGridValidation = 
        req.Headers.FirstOrDefault( x => x.Key == "Aeg-Event-Type" ).Value?.FirstOrDefault();

    dynamic eventData = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"event: {eventData}");

    if (eventGridValidation != String.Empty)
    {
        if (eventData[0].data.validationCode !=String.Empty && eventData[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent")
        {
            return req.CreateResponse(HttpStatusCode.OK, new 
            {
                validationResponse = eventData[0].data.validationCode
            });
        }
    }
    
    log.Info(jsonContent);

    return req.CreateResponse(HttpStatusCode.OK);
}
```

#### <a name="update-test-request-body"></a>De aanvraagtekst test bijwerken

Aan de rechterkant van de **Azure** portal venster ziet u twee tabbladen: **bestanden bekijken** en **Test**. Selecteer het tabblad **Testen**. In de **aanvraagtekst**, plak de volgende json. U kunt deze plakken, zoals is, niet nodig om alle waarden te wijzigen.

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2017-08-06T22:09:30.740323Z"
}
]
```

Druk op **opslaan en uitvoeren** aan de bovenkant van het venster.

![Aanvraagtekst](./media/job-state-events-cli-how-to/generic_webhook_test.png)

## <a name="register-for-the-event-grid-subscription"></a>Registreren voor het abonnement gebeurtenis raster 

U zich abonneert op een artikel gebeurtenis raster zien welke gebeurtenissen u wilt traceren. Het volgende voorbeeld is lid van het Media Services-account hebt gemaakt de URL van de Azure-functie webhook die u hebt gemaakt als het eindpunt voor gebeurtenismelding doorgegeven. 

Vervang `<event_subscription_name>` met een unieke naam voor uw gebeurtenisabonnement. Gebruik voor `<resource_group_name>` en `<ams_account_name>` de waarden die u eerder hebt gemaakt.  Voor de `<endpoint_URL>` plak uw eindpunt-URL. Verwijder *& clientID = standaard* van de URL. Door een eindpunt op te geven wanneer u zich abonneert, wordt via Event Grid de routering van gebeurtenissen naar dit eindpunt verwerkt. 

```cli
amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $amsResourceId \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

De waarde van de Media Services-account resource id ziet er ongeveer als volgt uit:

/Subscriptions/81212121-2f4f-4b5d-a3dc-ba0015515f7b/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amstestaccount

## <a name="test-the-events"></a>De gebeurtenissen te testen

Voer een codering. Bijvoorbeeld, zoals beschreven in de [Stream videobestanden](stream-files-dotnet-quickstart.md) Quick Start.

U hebt de gebeurtenis geactiveerd, en de gebeurtenis is via Event Grid verzonden naar het eindpunt dat u hebt geconfigureerd toen u zich abonneerde. Blader naar de webhook die u eerder hebt gemaakt. Klik op **Monitor** en **vernieuwen**. U ziet de status van de taak verandert gebeurtenissen: 'In de wachtrij', 'Gepland', 'Verwerking', 'Voltooid', 'Fout', 'Geannuleerd', 'Annuleren'.  Zie voor meer informatie [Media Services-schema's voor gebeurtenis](media-services-event-schemas.md).

Bijvoorbeeld:

```json
[{
  "topic": "/subscriptions/<subscription id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job id>",
  "eventType": "Microsoft.Media.JobStateChange",
  "eventTime": "2018-04-20T21:17:26.2534881",
  "id": "<id>",
  "data": {
    "previousState": "Scheduled",
    "state": "Processing"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

![Test-gebeurtenissen](./media/job-state-events-cli-how-to/test_events.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u verder wilt werken met dit opslagaccount en dit gebeurtenisabonnement, moet u de resources die u hebt gemaakt in dit artikel niet opschonen. Als u niet verder wilt werken, gebruikt u de volgende opdracht om de resources te verwijderen die u in dit artikel hebt gemaakt.

Vervang `<resource_group_name>` door de resourcegroep die u eerder hebt gemaakt.

```azurecli-interactive
az group delete --name <resource_group_name>
```

## <a name="next-steps"></a>Volgende stappen

[Reageren op gebeurtenissen](reacting-to-media-services-events.md)

---
title: Azure Media Services-gebeurtenissen routeren naar een aangepaste web-eindpunt | Microsoft Docs
description: Gebruik Azure Event Grid abonneren op de statuswijzigingsgebeurtenis van Media Services-taak.
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
ms.openlocfilehash: e9df0cd24ef890765b78c25a073d671889be10a7
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724062"
---
# <a name="route-azure-media-services-events-to-a-custom-web-endpoint-using-cli"></a>Azure Media Services-gebeurtenissen routeren naar een aangepaste web-eindpunt met behulp van CLI

Azure Event Grid is een gebeurtenisservice voor de cloud. In dit artikel kunt u de Azure CLI gebruiken om u te abonneren op Azure Media Services-taak statuswijzigingsgebeurtenissen en activeren van de gebeurtenis om het resultaat weer te geven. 

Meestal stuurt u gebeurtenissen naar een eindpunt dat reageert op de gebeurtenis, zoals een webhook of Azure-functie. In deze zelfstudie laat zien hoe maken en instellen van een webhook.

Als u de stappen in dit artikel hebt voltooid, ziet u dat de gegevens van gebeurtenissen naar een eindpunt zijn verzonden.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij [Azure Portal](http://portal.azure.com) en start **CloudShell** om CLI-opdrachten uitvoeren, zoals wordt beschreven in de volgende stappen.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u wilt installeren of upgraden, Zie [Azure CLI installeren](/cli/azure/install-azure-cli). 

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

Zorg ervoor dat u vergeet niet de waarden die u voor Opslagnaam van de Media Services-accountnaam, en de resourcenaam van de gebruikt.

## <a name="enable-event-grid-resource-provider"></a>Event Grid-resourceprovicer inschakelen

Eerste wat die u moet doen is Zorg ervoor dat u Event Grid-resourceprovider is ingeschakeld op uw abonnement. 

In de **Azure** portal het volgende doen:

1. Ga naar de abonnementen.
2. Selecteer uw abonnement.
3. Selecteer onder instellingen Resourceproviders.
4. Zoek naar 'EventGrid'.
5. Zorg ervoor dat Event Grid is geregistreerd. Als dit niet het geval is, drukt u op de **registreren** knop.  

## <a name="create-a-generic-azure-function-webhook"></a>Een algemene webhook van de Azure-functie maken 

### <a name="create-a-message-endpoint"></a>Het eindpunt van een bericht maken

Voordat u zich abonneert op de Event Grid-artikel, maakt u een eindpunt dat de berichten worden verzameld, zodat u ze kunt bekijken.

Maak een functie geactiveerd door een algemene webhook, zoals beschreven in de [generieke webhook](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function) artikel. In deze zelfstudie de **C#** code wordt gebruikt.

Nadat de webhook is gemaakt, kopieert u de URL door te klikken op de *functie-URL ophalen* koppelen aan de bovenkant van de **Azure** portalvenster. U hoeft niet het laatste deel van de URL (*& clientID = standaard*).

![Een webhook maken](./media/job-state-events-cli-how-to/generic_webhook_files.png)

### <a name="validate-the-webhook"></a>Valideren van de webhook

Wanneer u uw eigen webhook-eindpunt met Event Grid registreren, stuurt u een POST-aanvraag met een eenvoudige validatie-code voor het eindpunt eigendom bewijzen. Uw app nodig heeft om te reageren door deze echo terug code voor de validatie. Event Grid leveren niet van gebeurtenissen naar webHook-eindpunten die nog niet gevalideerd is. Zie voor meer informatie, [Event Grid-beveiliging en verificatie](https://docs.microsoft.com/azure/event-grid/security-authentication). In dit gedeelte definieert twee onderdelen die moeten worden gedefinieerd voor de validatie om door te geven.

#### <a name="update-the-source-code"></a>De broncode bijwerken

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

#### <a name="update-test-request-body"></a>Test-aanvraagtekst bijwerken

Aan de rechterkant van de **Azure** portal venster ziet u twee tabbladen: **bestanden bekijken** en **Test**. Selecteer het tabblad **Testen**. In de **aanvraagtekst**, plak de volgende json. U kunt deze plakken is, hoeft niet te wijzigen van alle waarden.

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

## <a name="register-for-the-event-grid-subscription"></a>Meld u aan voor de Event Grid-abonnement 

U zich abonneert op een artikel Event Grid zien welke gebeurtenissen u wilt bijhouden. Het volgende voorbeeld is geabonneerd op de Media Services-account u gemaakt en hoe de URL van de Azure-functie webhook die u hebt gemaakt als het eindpunt voor de gebeurtenismelding. 

Vervang `<event_subscription_name>` met een unieke naam voor het gebeurtenisabonnement. Gebruik voor `<resource_group_name>` en `<ams_account_name>` de waarden die u eerder hebt gemaakt.  Voor de `<endpoint_URL>` uw eindpunt-URL plakken. Verwijder *& clientID = standaard* van de URL. Door een eindpunt op te geven wanneer u zich abonneert, wordt via Event Grid de routering van gebeurtenissen naar dit eindpunt verwerkt. 

```cli
amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $amsResourceId \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

De waarde van de Media Services-account resource id ziet er ongeveer als volgt uit:

/Subscriptions/81212121-2f4f-4b5d-a3dc-ba0015515f7b/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amstestaccount

## <a name="test-the-events"></a>Testen van de gebeurtenissen

Een coderingstaak wordt uitgevoerd. Bijvoorbeeld, zoals is beschreven in de [Stream videobestanden](stream-files-dotnet-quickstart.md) Quick Start.

U hebt de gebeurtenis geactiveerd, en de gebeurtenis is via Event Grid verzonden naar het eindpunt dat u hebt geconfigureerd toen u zich abonneerde. Blader naar de webhook die u eerder hebt gemaakt. Klik op **Monitor** en **vernieuwen**. U ziet de taakstatus verandert gebeurtenissen: 'In de wachtrij', 'Gepland', "Verwerking", "Voltooid", "Error", "Geannuleerd", "Annuleren".  Zie voor meer informatie, [Media Services-gebeurtenisschema](media-services-event-schemas.md).

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

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)

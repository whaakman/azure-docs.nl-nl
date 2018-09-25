---
title: Azure Media Services-gebeurtenissen routeren naar een aangepaste web-eindpunt | Microsoft Docs
description: Gebruik Azure Event Grid abonneren op de statuswijzigingsgebeurtenis van Media Services-taak.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/20/2018
ms.author: juliako
ms.openlocfilehash: e7268a066acf41c454de0c66aa21603199d85a60
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034838"
---
# <a name="route-azure-media-services-events-to-a-custom-web-endpoint-using-cli"></a>Azure Media Services-gebeurtenissen routeren naar een aangepaste web-eindpunt met behulp van CLI

Azure Event Grid is een gebeurtenisservice voor de cloud. In dit artikel kunt u de Azure CLI gebruiken om u te abonneren op Azure Media Services-taak statuswijzigingsgebeurtenissen en activeren van de gebeurtenis om het resultaat weer te geven. 

Meestal stuurt u gebeurtenissen naar een eindpunt dat reageert op de gebeurtenis, zoals een webhook of Azure-functie. In deze zelfstudie laat zien hoe maken en instellen van een webhook.

Als u de stappen in dit artikel hebt voltooid, ziet u dat de gegevens van gebeurtenissen naar een eindpunt zijn verzonden.

## <a name="prerequisites"></a>Vereisten

- Een actief Azure-abonnement hebben.
- [Een Media Services-account maken](create-account-cli-how-to.md).

    Zorg ervoor dat u vergeet niet de waarden die u voor de naam van de resourcegroep en de naam van de Media Services-account gebruikt.

- Installeer de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). In dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. U kunt ook de [Azure Cloud Shell](https://shell.azure.com/bash).

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

Vervang `<event_subscription_name>` met een unieke naam voor het gebeurtenisabonnement. Voor `<resource_group_name>` en `<ams_account_name>`, gebruikt u de waarden die u hebt gebruikt bij het maken van het Media Services-account. Voor de `<endpoint_URL>` uw eindpunt-URL plakken. Verwijder *& clientID = standaard* van de URL. Door een eindpunt op te geven wanneer u zich abonneert, wordt via Event Grid de routering van gebeurtenissen naar dit eindpunt verwerkt. 

```cli
amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $amsResourceId \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

De waarde van de Media Services-account resource id ziet er ongeveer als volgt uit:

```
/subscriptions/81212121-2f4f-4b5d-a3dc-ba0015515f7b/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amstestaccount
```

## <a name="test-the-events"></a>Testen van de gebeurtenissen

Een coderingstaak wordt uitgevoerd. Bijvoorbeeld, zoals is beschreven in de [Stream videobestanden](stream-files-dotnet-quickstart.md) Quick Start.

U hebt de gebeurtenis geactiveerd, en de gebeurtenis is via Event Grid verzonden naar het eindpunt dat u hebt geconfigureerd toen u zich abonneerde. Blader naar de webhook die u eerder hebt gemaakt. Klik op **Monitor** en **vernieuwen**. U ziet de taakstatus verandert gebeurtenissen: 'In de wachtrij', 'Gepland', "Verwerking", "Voltooid", "Error", "Geannuleerd", "Annuleren".  Zie voor meer informatie, [Media Services-gebeurtenisschema](media-services-event-schemas.md).

Het volgende voorbeeld ziet u het schema van de gebeurtenis JobStateChange:

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

## <a name="next-steps"></a>Volgende stappen

[Reageren op gebeurtenissen](reacting-to-media-services-events.md)

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)

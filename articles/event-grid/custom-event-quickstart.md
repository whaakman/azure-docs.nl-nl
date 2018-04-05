---
title: Aangepaste gebeurtenissen voor Azure Event Grid met CLI | Microsoft Docs
description: Gebruik Azure Event Grid en Azure CLI om een onderwerp te publiceren en u te abonneren op deze gebeurtenis.
services: event-grid
keywords: ''
author: tfitzmac
ms.author: tomfitz
ms.date: 03/20/2018
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: be98fa88727a867e0fca0ca3587db276f8a2d7f2
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="create-and-route-custom-events-with-azure-cli-and-event-grid"></a>Aangepaste gebeurtenissen maken en routeren met behulp van Azure CLI en Event Grid

Azure Event Grid is een gebeurtenisservice voor de cloud. In dit artikel gebruikt u de Azure CLI om een aangepast onderwerp te maken, u op het onderwerp te abonneren, en de gebeurtenis te activeren om het resultaat weer te geven. Meestal stuurt u gebeurtenissen naar een eindpunt dat reageert op de gebeurtenis, zoals een webhook of Azure-functie. Ter vereenvoudiging van dit artikel stuurt u hier de gebeurtenissen echter naar een URL via welke de berichten alleen maar worden verzameld. U maakt deze URL met behulp van een hulpprogramma van derden, [Hookbin](https://hookbin.com/).

>[!NOTE]
>**Hookbin** is niet bedoeld voor gebruik met hoge doorvoer. Dit hulpprogramma wordt alleen gebruikt ter demonstratie. Als u meer dan een gebeurtenis tegelijk pusht, ziet u mogelijk niet alle gebeurtenissen in het hulpprogramma.

Wanneer u klaar bent, ziet u dat de gebeurtenisgegevens naar een eindpunt zijn verzonden.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel de nieuwste versie van Azure CLI (2.0.24 of hoger) uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

Als u Cloud Shell niet gebruikt, moet u zich eerst aanmelden met `az login`.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Event Grid-onderwerpen zijn Azure-resources en moeten in een Azure-resourcegroep worden geplaatst. De resourcegroep is een logische verzameling waarin Azure-resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create). 

In het volgende voorbeeld wordt een resourcegroep met de naam *gridResourceGroup* gemaakt op de locatie *westus2*.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Een aangepast onderwerp maken

Een Event Grid-onderwerp biedt een door de gebruiker gedefinieerd eindpunt waarop u de gebeurtenissen kunt posten. In het volgende voorbeeld wordt het aangepaste onderwerp in uw resourcegroep gemaakt. Vervang `<topic_name>` door een unieke naam voor het onderwerp. De onderwerpnaam moet uniek zijn omdat deze wordt vertegenwoordigd door een DNS-vermelding.

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a>Het eindpunt van een bericht maken

Voordat u zich abonneert op het onderwerp, gaan we het eindpunt voor het gebeurtenisbericht maken. In plaats van code te schrijven om op de gebeurtenis te reageren, maken we een eindpunt waarop de berichten worden verzameld, zodat u ze kunt weergeven. Hookbin is een hulpprogramma van derden waarmee u een eindpunt kunt maken en aanvragen kunt weergeven die naar dit eindpunt worden verzonden. Ga naar [Hookbin](https://hookbin.com/) en klik op **Create New Endpoint**.  Kopieer de bin-URL. U hebt deze nodig wanneer u zich abonneert op het aangepaste onderwerp.

## <a name="subscribe-to-a-topic"></a>Abonneren op een onderwerp

U abonneert u op een onderwerp om Event Grid te laten weten welke gebeurtenissen u wilt traceren. In het volgende voorbeeld ziet u hoe u zich abonneert op het onderwerp dat u hebt gemaakt, en hoe de URL van Hookbin wordt doorgegeven als het eindpunt voor de gebeurtenismelding. Vervang `<event_subscription_name>` door een unieke naam voor het abonnement, en `<endpoint_URL>` door de waarde uit de voorgaande sectie. Door een eindpunt op te geven wanneer u zich abonneert, wordt via Event Grid de routering van gebeurtenissen naar dit eindpunt verwerkt. Gebruik voor `<topic_name>` de waarde die u eerder hebt gemaakt. 

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

## <a name="send-an-event-to-your-topic"></a>Een gebeurtenis verzenden naar het onderwerp

We activeren een gebeurtenis om te zien hoe het bericht via Event Grid naar het eindpunt wordt gedistribueerd. Eerst gaan we de URL en de sleutel voor het aangepaste onderwerp ophalen. Gebruik opnieuw de onderwerpnaam voor `<topic_name>`.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Ter vereenvoudiging van dit artikel gebruikt u voorbeeldgebeurtenisgegevens om naar het onderwerp te verzenden. Meestal worden de gebeurtenisgegevens verzonden via een toepassing of Azure-service. In de volgende gegevens worden de gebeurtenisgegevens opgehaald:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

Als u de volledige gebeurtenis wilt weergeven, gebruikt u `echo "$body"`. Het element `data` van de JSON is de nettolading van de gebeurtenis. Elke juist opgemaakte JSON kan in dit veld worden ingevoerd. U kunt het onderwerpveld ook gebruiken voor geavanceerd routeren en filteren.

CURL is een hulpprogramma waarmee HTTP-aanvragen worden verzonden. In dit artikel gebruiken we CURL om de gebeurtenis naar het onderwerp te verzenden. 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

U hebt de gebeurtenis geactiveerd en Event Grid heeft het bericht verzonden naar het eindpunt dat u hebt geconfigureerd toen u zich abonneerde. Blader naar de eindpunt-URL die u eerder hebt gemaakt. U kunt ook op Vernieuwen klikken in het geopende browservenster. U ziet de gebeurtenis die u zojuist hebt verzonden. 

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a>Resources opschonen
Als u verder wilt werken met deze gebeurtenis, schoon dan de resources die u in dit artikel hebt gemaakt, niet op. Gebruik anders de volgende opdracht om de resources te verwijderen die u in dit artikel hebt gemaakt.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

U weet nu hoe u onderwerpen maakt en hoe u zich abonneert op een gebeurtenis. Kijk waar Event Grid u nog meer bij kan helpen:

- [Over Event Grid](overview.md)
- [Blob Storage-gebeurtenissen naar een aangepast eindpunt op het web routeren](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitor virtual machine changes with Azure Event Grid and Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) (Wijzigingen in virtuele machines bewaken met Azure Event Grid en Logic Apps)
- [Big data streamen naar een datawarehouse](event-grid-event-hubs-integration.md)
